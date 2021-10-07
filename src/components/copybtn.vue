<template>
  <button
    @click="copycc"
    class="copyBtn"
    :style="{
      width: w + 'px',
      height: h + 'px',
      'border-radius': h * 0.2 + 'px',
    }"
  >
    <span :style="{ color: textColor }">{{ title }}</span> {{ msg }}
  </button>
</template>

<script>
import { ref } from 'vue'
export default {
  props: {
    w: { default: 160 },
    h: { default: 60 },
    msg: { default: '#000' }
  },
  setup(props) {
    const title = ref('复制 ')
    const textColor = ref('#fff')
    let timeout = null
    const copycc = () => {
      var input = document.createElement('input');
      input.setAttribute('readonly', 'readonly');
      input.setAttribute('value', props.msg);
      document.body.appendChild(input);
      input.select();
      input.setSelectionRange(0, 9999);
      document.execCommand('Copy');
      if (document.execCommand('Copy')) {
        title.value = '成功 '
        let cc = props.msg
        if (cc[0] !== '#') cc = '#' + cc
        textColor.value = cc
        clearTimeout(timeout)
        timeout = setTimeout(() => {
          title.value = '复制 '
          textColor.value = '#fff'
        }, 500);
      }
      document.body.removeChild(input);
    }
    return { title, copycc,textColor }
  }
}
</script>

<style lang="less">
.copyBtn {
  display: inline-block;
  margin: 8px 16px 0 0;
  background-color: #4e6ef2;
  font-size: 17px;
  font-weight: 500;
  color: #fff;
  &:hover,
  &:active {
    background-color: #4662d9;
  }
}
</style>