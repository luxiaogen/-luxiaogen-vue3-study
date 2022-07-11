<template>
  <div class="app">
    <h3>我是App组件(祖),{{name}}--{{price}}</h3>
    <Child :car="car" />
  </div>
</template>

<script>
import { reactive, toRefs } from '@vue/reactivity'
import Child from './components/Child.vue'
import { provide } from '@vue/runtime-core'
export default {
  name: 'App',
  components: { Child },
  setup() {
    let car = reactive({name:'奔驰',price:'40w'})
    provide('car', car)
    return {...toRefs(car), car}
  }
}
</script>

<style>
  .app {
    background-color: gray;
    padding: 10px
  }
</style>