<template>
  <form class="card card-w30" @submit.prevent="submitForm">
    <app-select-block
      v-model="selectValue"
    >
    </app-select-block>
    <app-text-area-block
      v-model:text.trim="textAreaValue"
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
  emits: {
    submitForm (value) {
      if (value) {
        return true
      }
      return false
    }
  },
  data () {
    return {
      selectValue: 'title',
      textAreaValue: ''
    }
  },
  computed: {
    isDisabled () {
      return this.textAreaValue.length <= 3
    },
    isSubmitForm () {
      return this.selectValue && this.textAreaValue
    }
  },
  methods: {
    submitForm () {
      if (this.isSubmitForm) {
        const data = {
          type: this.selectValue,
          value: this.textAreaValue
        }
        this.$emit('submitForm', data)
        this.selectValue = 'title'
        this.textAreaValue = ''
      }
    }
  }
}
</script>

<style scoped>

</style>
