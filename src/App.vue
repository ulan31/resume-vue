<template>
  <appAlert :alert-data="alert" @close-alert="alert = null"/>
  <div class="container column">

    <form class="card card-w30">
      <div class="form-control">
        <label for="type">Тип блока</label>
        <select id="type">
          <option value="title">Заголовок</option>
          <option value="subtitle">Подзаголовок</option>
          <option value="avatar">Аватар</option>
          <option value="text">Текст</option>
        </select>
      </div>

      <div class="form-control">
        <label for="value">Значение</label>
        <textarea id="value" rows="3"></textarea>
      </div>

      <button class="btn primary">Добавить</button>
    </form>

    <div class="card card-w70">
      <h1>Резюме Nickname</h1>
      <div class="avatar">
        <img src="https://cdn.dribbble.com/users/5592443/screenshots/14279501/drbl_pop_r_m_rick_4x.png">
      </div>
      <h2>Опыт работы</h2>
      <p>
        главный герой американского мультсериала «Рик и Морти», гениальный учёный, изобретатель, атеист (хотя в некоторых сериях он даже молится Богу, однако, каждый раз после чудесного спасения ссылается на удачу и вновь отвергает его существование), алкоголик, социопат, дедушка Морти. На момент начала третьего сезона ему 70 лет[1]. Рик боится пиратов, а его главной слабостью является некий - "Санчезиум". Исходя из того, что существует неограниченное количество вселенных, существует неограниченное количество Риков, герой сериала предположительно принадлежит к измерению С-137. В серии комикcов Рик относится к измерению C-132, а в игре «Pocket Mortys» — к измерению C-123[2]. Прототипом Рика Санчеза является Эмметт Браун, герой кинотрилогии «Назад в будущее»[3].
      </p>
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

export default {
  components: {
    AppBtn,
    AppCommentList,
    AppLoader,
    AppAlert
  },
  data () {
    return {
      loading: false,
      commentList: [],
      alert: null
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
