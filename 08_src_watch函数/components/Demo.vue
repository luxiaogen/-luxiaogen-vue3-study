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
    let person = reactive({
      name: '张三',
      age: 18,
      job: {
        j1: {
          salary: 3,
        },
      },
    })

    // 情况一：监视 ref 所定义的一个响应式数据
    /* watch(sum,(newValue,oldValue) => {
      console.log('sum变了', newValue, oldValue)
    },{immediate:true}) */

    // 情况二：监视 ref 所定义的多个响应式数据
    /* watch([sum,msg], (newValue,oldValue) => {
      // 这里的形参是一个数组
      console.log('sum或msg改变了', newValue,oldValue)
    }, {immediate: true})
    */

    /* 
      情况三：监视reactive所定义的一个响应式数据
        1. 注意：此处无法正确的获取oldValue
        2. 注意：强制开启了深度监视(deep配置无效)

    */
    /* watch(person,(newValue,oldValue) => {
      console.log('person变化了', newValue, oldValue)
    },{deep:false}) // 此处的 deep 配置无效 */

    // 情况四：监视 reactive 所定义的一个响应式数据中的某个属性
    /* watch(()=>person.age, (newValue, oldValue) => {
      console.log('person的age变化了', newValue, oldValue)
    }) */

    // 情况五：监视 reactive 所定义的一个响应式数据中的某些属性
    /* watch([()=>person.name,()=>person.age], (newValue,oldValue) => {
      console.log('person中的name或age变化了', newValue, oldValue)
    }) */

    // 特殊情况
    watch(()=>person.job, (newValue, oldValue) => {
      console.log('person的job变化了', newValue, oldValue)
    }, {deep:true}) // 此处由于监视的是 reactive 所定义的对象中的某个属性(值为对象)，所以 deep 配置有效
    return {
      sum,
      msg,
      person,
    }
  },
}
</script>