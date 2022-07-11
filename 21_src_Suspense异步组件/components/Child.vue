<template>
  <div class="child">
    <h3>我是Child组件</h3>
    {{sum}}
  </div>
</template>

<script>
import { ref } from '@vue/reactivity'
export default {
  // eslint-disable-next-line vue/multi-word-component-names
  name: 'Child',
  async setup() {
    let sum = ref(0)
    let p = new Promise((resolve, reject) => {
      setTimeout(() => {
        resolve({ sum })
      }, 3000)
    })
    return await p
  },
}
</script>

<style>
.child {
  background-color: skyblue;
  padding: 10px;
}
</style>