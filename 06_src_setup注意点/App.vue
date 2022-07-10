<template>
  <Demo @hello="showHelloMsg" msg="你好呀" school="尚硅谷">
    <template v-slot:qwe>
      <span>尚硅谷</span>
    </template>
    <template v-slot:abc>
      <span>尚硅谷</span>
    </template>
  </Demo>
</template>

<script>
import { reactive } from 'vue'
import Demo from './components/Demo.vue'
export default {
  name: 'App',
  components: { Demo },
  setup() {
    function showHelloMsg(value) {
      alert(`你好啊，你触发了hello事件，我收到的参数是${value}`)
    }

    return {
      showHelloMsg,
    }
  },
}
</script>