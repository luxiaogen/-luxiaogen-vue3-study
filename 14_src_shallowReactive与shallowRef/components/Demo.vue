<template>
  <h4>当前的x.y的值是:{{x.y}}</h4>
  <button @click="x.y++">点我x.y++</button>
  <button @click="x={y:888}">点我替换x</button>
  <hr />
  <h4>{{person}}</h4>
  <h2>姓名:{{name}}</h2>
  <h2>年龄:{{age}}</h2>
  <h2>薪资:{{job.j1.salary}}k</h2>
  <button @click="name+='~'">修改姓名</button>
  <button @click="age++">增长年龄</button>
  <button @click="job.j1.salary++">涨薪</button>
</template>

<script>
import { reactive, toRef, toRefs, shallowReactive, ref, shallowRef } from 'vue'
export default {
  // eslint-disable-next-line vue/multi-word-component-names
  name: 'Demo',
  setup() {
    // 数据
    // let person = shallowReactive({ // 只考虑第一层数据的响应式
    let person = reactive({
      name: '张三',
      age: 18,
      job: {
        j1: {
          salary: 3,
        },
      },
    })

    // 如果是ref并且它的值是对象类型 它底层也会调用 reactive 函数
    let x = shallowRef({
      y: 0
    })

    console.log('@@@',x)

    return {
      person,
      x,
      ...toRefs(person),
    }
  },
}
</script>