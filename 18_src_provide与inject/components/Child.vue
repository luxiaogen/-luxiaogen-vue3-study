<template>
  <div class="child">
    <h3>我是Child组件(子) {{car.name}}--{{car.price}}</h3>
    <Son />
  </div>
</template>

<script>
import { toRef, toRefs } from '@vue/reactivity'
import Son from './Son.vue'
export default {
  // eslint-disable-next-line vue/multi-word-component-names
  name: 'Child',
  props: ['car'],
  components: { Son },
  setup(props) {

  },
}
</script>

<style>
.child {
  background-color: skyblue;
  padding: 10px;
}
</style>