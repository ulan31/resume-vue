<template>
  <div class="line">
    <p>{{ $attrs.value }}</p>
    <app-btn
      :class="'danger'"
      @action="$emit('remove', $attrs.id)"
    >
      Delete
    </app-btn>
  </div>
</template>

<script>
import AppBtn from './AppBtn'

export default {
  name: 'AppText',
  components: {
    AppBtn
  },
  emits: {
    remove (id) {
      if (id) {
        return true
      }
      return false
    }
  }
}
</script>

<style scoped>
  .line {
    display: flex;
    justify-content: space-between;
    align-items: center;
  }
</style>
