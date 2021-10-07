<template>
  <div class="wrap">
    <div class="colorIn">
      <colorbox :c="c_in" />
      <cin v-model="c_in" h="54" w="180" />
    </div>
    <colorbar @change="getcc" h="24" :c="c_in" :c0="c0" :c1="c1" />
    <div class="colorPicker">
      <div style="margin-right: 160px;">
        <colorbox w="24" :c="c0" />
        <cin v-model="c0" h="24" />
      </div>
      <div>
        <cin v-model="c1" h="24" />
        <colorbox w="24" :c="c1" />
      </div>
    </div>
    <div class="colorCopy">
      <div>
        <colorbox :c="cout" />
        <span class="cout">{{ cout }}</span>
      </div>
      <div style="vertical-align: top;">
        <cbtn :msg="cout1" />
        <cbtn :msg="cout2" />
      </div>
    </div>
  </div>
</template>

<script>
import { reactive, toRefs, computed } from 'vue'
import colorbox from './components/colorbox.vue'
import colorbar from './components/colorbar.vue'
import cin from './components/cin.vue'
import cbtn from './components/copybtn.vue'
export default {
  name: 'App',
  components: { colorbox, colorbar, cin, cbtn },
  setup() {
    const data = reactive({
      c_in: '#777',
      c0: '#000',
      c1: '#fff',
      cout: '#000',
      cout1: computed(() => {
        let res = data.cout
        return res.toUpperCase()
      }),
      cout2: computed(() => {
        let res = data.cout
        res = res.substring(1)
        return res.toUpperCase()
      })
    })
    const getcc = (e) => {
      if (typeof e !== 'string') return
      data.cout = e
    }
    return { ...toRefs(data), getcc }
  }
}
</script>

<style lang="less">
.wrap{
  width: 540px;
  margin: 60px auto;
}
.colorIn {
  margin-bottom: 32px;
  .cin {
    margin-left: 12px;
  }
}
.colorPicker {
  font-size: 0;
  margin: 12px 0 32px;
  & > div {
    display: inline-block;
  }
  .cin {
    margin: 0 6px;
  }
}
.colorCopy {
  font-size: 0;
}
.cout {
  display: inline-block;
  vertical-align: middle;
  margin: 0 12px;
  border: 2px solid #4e6ef2;
  color: #333;
  text-align: center;
  font-weight: 500;
  width: 180px;
  line-height: 54px;
  border-radius: 11px;
  padding: 0 11px;
  font-size: 43px;
}
</style>