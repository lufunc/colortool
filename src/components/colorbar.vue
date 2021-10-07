<template>
  <div style="font-size: 0;">
    <div
      class="colorbar"
      ref="mySlider"
      @mousedown="sliderHandle"
      :style="{
        width: w + 'px',
        height: h + 'px',
        margin: '0 ' + (h / 2 + 2) + 'px',
        'background-image': `linear-gradient(to right, ${cc0}, ${cc}, ${cc1})`,
      }"
    >
      <div
        class="colorbar_bar"
        :style="{
          width: h + 'px',
          height: h + 'px',
          'background-color': cout,
          left: (parseInt(num) + 100) / 2 + '%',
        }"
      ></div>
    </div>
    <div class="colorbar_input">
      <cin v-model="num" :style="{ 'vertical-align': 'unset' }" :h="h" w="48" />
      <span :style="{ 'font-size': h * 0.8 + 'px' }">%</span>
    </div>
  </div>
</template>

<script>
import { ref, watch, reactive, toRefs } from 'vue'
import cin from './cin.vue'
export default {
  components: { cin },
  props: {
    w: { default: 400 },
    h: { default: 20 },
    c: { default: '#777' },
    c0: { default: '#000' },
    c1: { default: '#fff' }
  },
  setup(props, { emit }) {
    const data = reactive({
      mySlider: null,
      num: 0,
      cc: '#777',
      cc0: '#000',
      cc1: '#f00',
      cout: '',
      rgb: {
        cc: { r: 0, g: 0, b: 0 },
        cc0: { r: 0, g: 0, b: 0 },
        cc1: { r: 0, g: 0, b: 0 }
      }
    })
    const toRGB = (p) => {
      let r = 0, g = 0, b = 0
      let hex = data[p] // cc #777
      if (hex.length === 4) {
        r = parseInt(hex[1] + hex[1], 16)
        g = parseInt(hex[2] + hex[2], 16)
        b = parseInt(hex[3] + hex[3], 16)
      } else {
        r = parseInt(hex[1] + hex[2], 16)
        g = parseInt(hex[3] + hex[4], 16)
        b = parseInt(hex[5] + hex[6], 16)
      }
      data.rgb[p] = { r, g, b }
    }
    const computeRGB = () => {
      const getc = (c1, c2, n) => {
        let t = (c2 - c1) * n + c1
        t = Math.round(t)
        let y = t.toString(16)
        if (y.length === 1) y = '0' + y
        return y
      }
      let res = '#'
      if (data.num > 0) {
        res += getc(data.rgb.cc.r, data.rgb.cc1.r, data.num / 100)
        res += getc(data.rgb.cc.g, data.rgb.cc1.g, data.num / 100)
        res += getc(data.rgb.cc.b, data.rgb.cc1.b, data.num / 100)
      } else {
        res += getc(data.rgb.cc.r, data.rgb.cc0.r, -data.num / 100)
        res += getc(data.rgb.cc.g, data.rgb.cc0.g, -data.num / 100)
        res += getc(data.rgb.cc.b, data.rgb.cc0.b, -data.num / 100)
      }
      data.cout = res
      emit('change', res)
    }
    const tohex = (p, c) => {
      let t = c.match(/^#?([a-fA-F0-9]{6}|[a-fA-F0-9]{3})$/)
      if (t) data[p] = '#' + t[1]
      toRGB(p)
      computeRGB()
    }
    tohex('cc', props.c)
    tohex('cc0', props.c0)
    tohex('cc1', props.c1)
    data.cout = data.cc
    const sliderHandle = (e) => {
      const temp = data.mySlider
      const tl = temp.getBoundingClientRect().left
      const tw = temp.offsetWidth
      const numRange = (n) => {
        let res = n
        if (n < 0) res = 0
        else if (n > tw) res = tw
        data.num = Math.round(res * 200 / tw - 100)
      }
      numRange(e.pageX - tl)
      document.onmousemove = (e) => {
        numRange(e.pageX - tl)
      }
      document.onmouseup = () => {
        document.onmousemove = null
        document.onmouseup = null
      }
    }
    watch(() => data.num, (v) => {
      if(v==='-') return
      computeRGB()
    })
    watch(() => props.c, (v) => {
      tohex('cc', v)
    })
    watch(() => props.c0, (v) => {
      tohex('cc0', v)
    })
    watch(() => props.c1, (v) => {
      tohex('cc1', v)
    })
    return { ...toRefs(data), sliderHandle }
  }
}
</script>

<style lang="less">
.colorbar {
  display: inline-block;
  vertical-align: middle;
  position: relative;
  margin-right: 12px;
  &_bar {
    position: absolute;
    top: -2px;
    border: 2px solid #1e1e1e;
    transform: translateX(-50%);
  }
  &_input {
    display: inline-block;
    vertical-align: middle;
    & > span {
      font-weight: 500;
    }
  }
}
</style>