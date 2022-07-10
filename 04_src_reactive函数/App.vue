<template>
  <h2>一个人的信息</h2>
  <h2>姓名:{{person.name}}</h2>
  <h2>年龄:{{person.age}}</h2>
  <h3>工作种类:{{person.job.type}}</h3>
  <h3>工作薪水:{{person.job.salary}}</h3>
  <h3>测试数据c:{{person.job.a.b.c}}</h3>
  <h3>爱好:{{person.hobby}}</h3>
  <button @click="changeInfo">修改人的信息</button>
</template>

<script>
import { reactive } from 'vue'
export default {
  name: 'App',
  // 此处只是测试一下setup，暂时不考虑响应式的问题
  setup() {
    // 数据
    let person = reactive({
      name: '张三',
      age: 18,
      job: {
        type: '后端工程师',
        salary: '3k',
        a: {
          b: {
            c: 666,
          },
        },
      },
      hobby: ['抽烟', '喝酒', '烫头'],
    })

    // 方法
    function changeInfo() {
      person.name = '李四'
      person.age = 29
      person.job.type = '后端小子'
      person.job.salary = '2.5k'
      person.job.a.b.c = 999
      person.hobby[0] = '学习'
    }

    // 返回一个对象(常用)
    return {
      person,
      changeInfo,
    }
  },
}
</script>