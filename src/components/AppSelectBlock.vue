<template>
  <div class="form-control">
    <label for="type">{{ title }}</label>
    <select id="type" :value="modelValue" @change="onChangeSelect">
      <option
        v-for="(item, index) in typeBlocks"
        :key="index"
        :value="item.value"
      >
        {{ item.title }}
      </option>
    </select>
  </div>
</template>

<script>
export default {
  name: 'AppSelectBlock',
  props: {
    modelValue: String
  },
  emits: ['update:modelValue'],
  data () {
    return {
      title: 'Тип блока'
    }
  },
  inject: ['typeBlocks'],
  methods: {
    onChangeSelect (event) {
      this.$emit('update:modelValue', event.target.value)
    }
  }
}
</script>

<style scoped>

</style>
