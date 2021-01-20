<template>
  <appAlert :alert-data="alert" @close-alert="alert = null"/>
  <div class="container column">
    <app-installation-block></app-installation-block>
    <div class="card card-w70">
      <app-heading></app-heading>
      <app-avatar></app-avatar>
      <app-subheading></app-subheading>
      <app-text></app-text>
      <h3>Добавьте первый блок, чтобы увидеть результат</h3>
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
import AppBtn from './components/AppBtn'
import AppCommentList from './components/AppCommentList'
import AppLoader from './components/AppLoader'
import AppAlert from './components/AppAlert'
import AppInstallationBlock from './components/AppInstallationBlock'
import AppHeading from './components/AppHeading'
import AppAvatar from './components/AppAvatar'
import AppSubheading from './components/AppSubheading'
import AppText from './components/AppText'

export default {
  components: {
    AppBtn,
    AppCommentList,
    AppLoader,
    AppAlert,
    AppInstallationBlock,
    AppHeading,
    AppAvatar,
    AppSubheading,
    AppText
  },
  data () {
    return {
      loading: false,
      commentList: [],
      alert: null
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
