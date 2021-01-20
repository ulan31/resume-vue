<template>
  <div class="alert" v-if="alertData" :class="alertData.type">
    <h1>{{ alertData.title }}</h1>
    <p>{{ alertData.text }}</p>
    <app-btn
      type="danger"
      @action="$emit('closeAlert')"
    >
      Close
    </app-btn>
  </div>
</template>

<script>
import AppBtn from './AppBtn'

export default {
  name: 'AppAlert',
  props: {
    alertData: {
      type: null,
      required: true
    }
  },
  emits: ['closeAlert'],
  components: {
    AppBtn
  }
}
</script>

<style scoped>

</style>
