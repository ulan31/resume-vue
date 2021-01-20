<template>
  <button
    class="btn"
    :class="type"
    :disabled="isDisabled"
    @click="$emit('action')"
  >
    <slot></slot>
  </button>
</template>

<script>
export default {
  name: 'AppBt',
  props: {
    type: {
      type: String,
      validator (value) {
        return ['', 'primary', 'danger'].includes(value)
      }
    },
    isDisabled: {
      type: Boolean,
      required: false,
      default: false
    }
  },
  emits: ['action']
}
</script>

<style scoped>

</style>
