<template>
  <input type="text" v-model="keyword" />
  <h3>{{keyword}}</h3>
</template>

<script>
import { customRef, ref } from '@vue/reactivity'

export default {
  name: 'App',
  setup() {
    // 自定义一个 ref——名为：myRef
    function myRef(value,delay) {
      let timer
      return customRef((track, trigger) => {
        return {
          get() {
            console.log(`有人从myRef这个容器中读取数据了，我把${value}给他了`)
            track() // 通知 Vue 追踪 value 的变化(提前和 get 商量一下，让他认为这个 value 是有用的)
            return value
          },
          set(newValue) {
            console.log(`有人从myRef这个容器中数据改为了${newValue}`)
            clearTimeout(timer)
            timer = setTimeout(() => {
              value = newValue
              trigger() // 通知 Vue 重新解析模板
            }, delay)
          },
        }
      })
    }

    // let keyword = ref('hello') // Vue 提供的 ref
    let keyword = myRef('hello', 500) // 使用 自定义的 ref

    return { keyword }
  },
}
</script>
