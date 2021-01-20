<template>
  <div class="line">
    <h2>{{ $attrs.value }}</h2>
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
  name: 'AppSubtitle',
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
