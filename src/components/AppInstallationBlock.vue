<template>
  <form class="card card-w30" @submit.prevent="submitForm">
    <app-select-block
      v-model="selectValue"
    >
    </app-select-block>
    <app-text-area-block
      v-model:text="textAreaValue"
    >
    </app-text-area-block>
    <app-btn
      :type="'primary'"
      :is-disabled="isDisabled"
    >
      Добавить
    </app-btn>
  </form>
</template>

<script>
import AppSelectBlock from './AppSelectBlock'
import AppTextAreaBlock from './AppTextAreaBlock'
import AppBtn from './AppBtn'

export default {
  name: 'AppInstallationBlock',
  components: {
    AppSelectBlock,
    AppTextAreaBlock,
    AppBtn
  },
  data () {
    return {
      selectValue: '0',
      textAreaValue: ''
    }
  },
  computed: {
    isDisabled () {
      return this.textAreaValue.length <= 3
    }
  },
  methods: {
    submitForm () {
      console.log('here')
    }
  }
}
</script>

<style scoped>

</style>
