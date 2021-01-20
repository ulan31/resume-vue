<template>
  <div class="card" v-if="commentItem">
    <h2>{{ title }}</h2>
    <ul class="list">
      <li class="list-item">
        <div>
          <p><strong>{{ commentItem.email }}</strong></p>
          <small>{{ commentItem.body }}</small>
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: 'AppCommentList',
  props: {
    commentItem: {
      type: Object,
      required: true
    }
  },
  data () {
    return {
      title: 'Комментарии'
    }
  }
}
</script>

<style scoped>

</style>
