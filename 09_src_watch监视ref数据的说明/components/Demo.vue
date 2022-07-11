<template>
  <h2>当前求和为:{{sum}}</h2>
  <button @click="sum++">点我+1</button>
  <hr />
  <h2>当前消息为:{{msg}}</h2>
  <button @click="msg+='!'">修改信息</button>
  <hr />
  <h2>姓名:{{person.name}}</h2>
  <h2>年龄:{{person.age}}</h2>
  <h2>薪资:{{person.job.j1.salary}}k</h2>
  <button @click="person.name+='~'">修改姓名</button>
  <button @click="person.age++">增长年龄</button>
  <button @click="person.job.j1.salary++">涨薪</button>
</template>

<script>
import { reactive, ref, watch } from 'vue'
export default {
  // eslint-disable-next-line vue/multi-word-component-names
  name: 'Demo',
  setup() {
    // 数据
    let sum = ref(0)
    let msg = ref('你好呀')
    let person = ref({
      name: '张三',
      age: 18,
      job: {
        j1: {
          salary: 3,
        },
      },
    })

    console.log(person)

    watch(sum, (newValue,oldValue) => {
      console.log('sum属性发生了改变', newValue, oldValue)
    })

    watch(person, (newValue, oldValue) => {
      console.log('person属性发生了改变', newValue, oldValue)
    },{deep:true})

    return {
      sum,
      msg,
      person,
    }
  },
}
</script>