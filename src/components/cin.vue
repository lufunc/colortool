<template>
  <input
    class="cin"
    :style="{
      width: w + 'px',
      'line-height': h + 'px',
      'border-radius': h * 0.2 + 'px',
      padding: '0 ' + h * 0.2 + 'px',
      'font-size': h * 0.8 + 'px',
    }"
    type="text"
    v-model="modelValue"
  />
</template>

<script>
import { watch } from 'vue'
export default {
  props: {
    w: { default: 80 },
    h: { default: 20 },
    modelValue: { default: '0' }
  },
  setup(props, { emit }) {
    watch(() => props.modelValue, (v) => {
      emit('update:modelValue', v)
    })
  }
}
</script>

<style lang="less">
.cin {
  vertical-align: middle;
  border: 2px solid #c4c7ce;
  color: #333;
  text-align: center;
  font-weight: 500;
  &:focus {
    border-color: #4e6ef2;
  }
}
</style>