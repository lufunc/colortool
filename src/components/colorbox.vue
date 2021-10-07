<template>
  <div
    class="color_box"
    :style="{ width: w + 'px', height: w + 'px', 'background-color': cc }"
  ></div>
</template>

<script>
import { computed, ref, watch } from 'vue'
export default {
  props: {
    w: { default: 70 },
    c: { default: '#777' }
  },
  setup(props) {
    const cc = ref('#000')
    const initcc = () => {
      let t = props.c.match(/^#?([a-fA-F0-9]{6}|[a-fA-F0-9]{3})$/)
      if (t) cc.value = '#'+t[1]
      else cc.value = '#000'
    }
    initcc()
    watch(() => props.c, (v) => {
      let t = v.match(/^#?([a-fA-F0-9]{6}|[a-fA-F0-9]{3})$/)
      if (t) cc.value = '#'+t[1]
    })
    return { cc }
  }
}
</script>

<style lang="less">
.color_box {
  display: inline-block;
  vertical-align: middle;
  border: 2px solid #1e1e1e;
}
</style>