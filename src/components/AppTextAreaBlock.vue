<template>
  <div class="form-control">
    <label for="value">{{ title }}</label>
    <textarea
      id="value"
      rows="3"
      :value="text"
      @input="onChangeText"
    ></textarea>
  </div>
</template>

<script>
export default {
  name: 'AppTextAreaBlock',
  props: ['text'],
  emits: ['update:text'],
  data () {
    return {
      title: 'Значение'
    }
  },
  methods: {
    onChangeText (event) {
      this.$emit('update:text', event.target.value)
    }
  }
}
</script>

<style scoped>

</style>
