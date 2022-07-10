# 一、创建Vue3.0工程
## 1.使用 vue-cli 创建

官方文档：https://cli.vuejs.org/zh/guide/creating-a-project.html#vue-create

```bash
## 查看@vue/cli版本，确保@vue/cli版本在4.5.0以上
vue --version
## 安装或者升级你的@vue/cli
npm install -g @vue/cli
## 创建
vue create vue_test
## 启动
cd vue_test
npm run serve
```

## 2.使用 vite 创建

官方文档：https://v3.cn.vuejs.org/guide/installation.html#vite

vite官网：https://vitejs.cn

- 什么是vite？—— 新一代前端构建工具。
- 优势如下：
  - 开发环境中，无需打包操作，可快速的冷启动。
  - 轻量快速的热重载（HMR）。
  - 真正的按需编译，不再等待整个应用编译完成。
- 传统构建 与 vite构建对比图


```bash
## 创建工程
npm init vite-app <project-name>
## 进入工程目录
cd <project-name>
## 安装依赖
npm install
## 运行
npm run dev
```

# 二、常用 Composition API
官方文档：https://v3.cn.vuejs.org/guide/composition-api-introduction.html、
## 1. 拉开序幕的 setup
1. 理解：Vu3.0中的一个新的配合项，值为一个函数
2. `setup`是所有的**Composition API(组合API)**，表演的舞台
3. 组件中所用到的：数据、方法等等，均要配置在`setup`中
4. `setup`函数的两种返回值：
   - 若返回一个对象，则对象中的属性、方法，在模板中均可以直接使用。(!!!!)
   - <span style="color:#aad">若返回的是渲染函数：则可以自定义渲染内容(了解)</span>
5. 注意点：
   1. 尽量不要与Vue2.x配置混用
     - Vue2.x配置(data、methods、computed..)中**可以访问到**setup中属性、方法
     - 但是setup中不能访问到Vue2.x的配置(data、methods、computed..)
     - 如果有重名，setup优先
   2. setup不能是`async`函数，因为返回值不再是`return`的对象，而是`promise`
      模板看不到`return`对象中的属性(后期也可返回一个Promise实例，但需要Suspense和异步组件的配合)
6. 示例代码：
```js
import { ref } from 'vue';
export default {
  name: 'App',
  // 此处只是测试一下setup，暂时不考虑响应式的问题
  setup() {
    // 数据
    let name = '张三'
    return {name}
  },
}
```
## 2. ref 函数
- 作用：定义一个响应式数据
- 语法：`const xxx = ref(initValue)`
  - 创建一个包含响应式数据的引用对象(`reference对象`，简称为ref对象)
  - JS中操作数据：`xxx.value`
  - 模板中读取数据：不需要.value，直接`{{xxx}}`
- 备注：
  - 接收的数据可以是：基本类型、也可以是对象类型
  - 基本类型的数据：响应依然是靠：`Object.defineProperty()`的`get`和`set`完成的
  - 对象类型的数据：内部 **求助** 了Vue3.0 中的一个新函数 ——— `reactive` 函数。
- 示例代码：
```js
setup() {
  let name = ref('张三')
  let job = ref({
    type: '后端工程师',
    salary: '3k'
  })
  function changeInfo() {
    name.value = '李四',
    job.value.type = '后端小子'
    job.value.salary = '2.5k'
  }
  return { name,job, changeInfo }
},
```


## 3. reactive 函数
- 作用：定义一个 **对象类型** 的响应式数据(基本类型不要用它，用`ref`函数)
- 语法：`const 代理对象 = reactive(源对象)`接收一个对象(或数组)，返回一个 **代理对象**(Proxy的实例对象,简称为proxy对象)
- `reactive`定义的响应式是"深层次的"
- 内部基于 ES6 的 Proxy 实现的，通过代理对象操作源对象内部数据进行操作。

## 4. Vue3.0中的响应式原理
### 1. Vue2.x的响应式
- 实现原理：
  - 对象类型：通过`Object.defineProperty()`对属性的读取、修改进行拦截(数据劫持)。
  - 数组类型：通过重写更新数组的一系列方法来实现拦截。（对数组的变更方法进行了包裹）
- 存在问题：
  - 新增属性、删除属性，界面不会更新
  - 直接通过下标修改属性，界面不会自动更新
### 2.Vue3.0的响应式
- 实现原理：
  - 通过`Proxy(代理)`：拦截对象中任意属性的变化，包括：属性值的读取、属性的添加、属性的删除等。
  - 通过`Reflect(反射)`: 对源对象的属性进行操作。
  - MDN文档
    - `Proxy`：https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Proxy
    - `Reflect`：https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Reflect
```js
new Proxy(data, {
	// 拦截读取属性值
    get (target, prop) {
    	return Reflect.get(target, prop)
    },
    // 拦截设置属性值或添加新属性
    set (target, prop, value) {
    	return Reflect.set(target, prop, value)
    },
    // 拦截删除属性
    deleteProperty (target, prop) {
    	return Reflect.deleteProperty(target, prop)
    }
})

proxy.name = 'tom'   
```

## 5. reactive 对比 ref
- 从定义数据角度对比：
  - ref用来定义：基本类型的数据
  - reactive用来定义：对象(或数组)类型数据
  - 备注：ref也可以用来定义对象(或数组)类型数据，它内部会自动通过`reactive`转为代理对象

