<template>
  <h2>一个人的信息</h2>
  <h2>姓名:{{name}}</h2>
  <h2>年龄:{{age}}</h2>
  <h2>性别:{{sex}}</h2>
  <h2>a的值:{{a}}</h2>
  <button @click="sayHello">说话(Vue3所配置的——sayHello)</button>
  <br />
  <br />
  <button @click="sayWelcome">说话(Vue2所配置的——sayWelcome)</button>
  <br />
  <br />
  <button @click="test1">测试一下在Vue2的配置中去读取Vue3中的数据、方法</button>
  <button @click="test2">测试一下在Vue3中setup配置去读取Vue2中的数据、方法</button>
</template>

<script>
import { h } from 'vue'
export default {
  name: 'App',
  data() {
    return {
      sex: '男',
      // a: 100,
    }
  },
  methods: {
    sayWelcome() {
      console.log('尚硅谷欢迎你来学习')
    },

    test1() {
      console.log(this.sex)
      console.log(this.sayWelcome)
      console.log(this.name)
      console.log(this.age)
      console.log(this.sayHello)
    },
  },
  // 此处只是测试一下setup，暂时不考虑响应式的问题
  setup() {
    // 数据
    let name = '张三'
    let age = 18
    let a = 200

    // 方法
    function sayHello() {
      alert(`我叫${name}，我${age}，你好呀`)
    }

    function test2() {
      console.log(name)
      console.log(age)
      console.log(a)
      console.log(this.sex)
      console.log(this.sayWelcome)
    }

    // 返回一个对象(常用)
    return {
      name,
      age,
      sayHello,
      a,
      test2
    }

    // 返回一个函数(渲染函数)
    // return () =>  h('h1', '尚硅谷')
  },
}
</script>