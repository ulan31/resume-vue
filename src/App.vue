<template>
  <appAlert :alert-data="alert" @close-alert="alert = null"/>
  <div class="container column">
    <app-installation-block
      @submitForm="createData"
    >
    </app-installation-block>
    <div class="card card-w70">
      <template v-if="isDataBlock">
        <component
          v-for="value in dataBlocks"
          :key="value.id"
          :is="'app-' + value.type"
          :="value"
          @remove="removeItem"
        ></component>
      </template>
      <h3 v-else-if="!loading">Добавьте первый блок, чтобы увидеть результат</h3>
    </div>
  </div>
  <div class="container">
    <p v-if="commentList.length === 0">
      <app-btn
        :type="'primary'"
        @action="loadingComment"
      >
        Загрузить комментарии
      </app-btn>
    </p>
    <app-loader v-if="loading"></app-loader>
    <app-comment-list
      v-else
      v-for="comment in commentList"
      :key="comment.id"
      :comment-item="comment"
    ></app-comment-list>
  </div>
</template>

<script>
import axios from 'axios'
import AppBtn from './components/AppBtn'
import AppCommentList from './components/AppCommentList'
import AppLoader from './components/AppLoader'
import AppAlert from './components/AppAlert'
import AppInstallationBlock from './components/AppInstallationBlock'
import AppTitle from './components/AppTitle'
import AppSubtitle from './components/AppSubtitle'
import AppAvatar from './components/AppAvatar'
import AppText from './components/AppText'

export default {
  components: {
    AppBtn,
    AppCommentList,
    AppLoader,
    AppAlert,
    AppInstallationBlock,
    AppTitle,
    AppSubtitle,
    AppAvatar,
    AppText
  },
  data () {
    return {
      loading: false,
      commentList: [],
      dataBlocks: [],
      alert: null
    }
  },
  computed: {
    isDataBlock () {
      if (this.dataBlocks.length !== 0) {
        return true
      }
      return false
    }
  },
  provide () {
    return {
      typeBlocks: [
        {
          title: 'Заголовок',
          value: 'title'
        },
        {
          title: 'Подзаголовок',
          value: 'subtitle'
        },
        {
          title: 'Аватар',
          value: 'avatar'
        },
        {
          title: 'Текст',
          value: 'text'
        }
      ]
    }
  },
  mounted () {
    this.getData()
  },
  methods: {
    async loadingComment () {
      this.loading = true
      try {
        const response = await fetch('https://jsonplaceholder.typicode.com/comments?_limit=42', {
          method: 'GET',
          headers: {
            'Content-Type': 'Application/json'
          }
        })
        if (!response) {
          throw new Error('not comments')
        }
        this.commentList = await response.json()
        this.loading = false
      } catch (e) {
        this.alert = {
          type: 'danger',
          title: 'Error',
          text: e.message
        }
        this.loading = false
      }
    },
    async createData (newData) {
      this.loading = true
      try {
        const { data } = await axios.post('https://rezume-vue-default-rtdb.europe-west1.firebasedatabase.app/rezume.json', newData)
        if (data) {
          this.dataBlocks.push({
            id: data,
            ...newData
          })
        }
        this.loading = false
      } catch (e) {
        this.alert = {
          type: 'danger',
          title: 'Error',
          text: e.message
        }
        this.loading = false
      }
    },
    async getData () {
      this.loading = true
      try {
        const { data } = await axios.get('https://rezume-vue-default-rtdb.europe-west1.firebasedatabase.app/rezume.json')
        if (data) {
          this.dataBlocks = Object.keys(data).map(keys => {
            return {
              id: keys,
              ...data[keys]
            }
          })
        }
        this.loading = false
      } catch (e) {
        this.alert = {
          type: 'danger',
          title: 'Error',
          text: e.message
        }
        this.loading = false
      }
    },
    async removeItem (id) {
      this.loading = true
      try {
        const name = this.dataBlocks.find(item => item.id === id).type
        await axios.delete(`https://rezume-vue-default-rtdb.europe-west1.firebasedatabase.app/rezume/${id}.json`)
        this.dataBlocks = this.dataBlocks.filter(item => item.id !== id)
        this.alert = {
          type: 'danger',
          title: 'Delete!',
          text: `Delete block "${name}"`
        }
        this.loading = false
      } catch (e) {
        this.alert = {
          type: 'danger',
          title: 'Something wrong',
          text: e.message
        }
        this.loading = false
      }
    }
  }
}
</script>

<style>
  .avatar {
    display: flex;
    justify-content: center;
  }

  .avatar img {
    width: 150px;
    height: auto;
    border-radius: 50%;
  }
</style>
