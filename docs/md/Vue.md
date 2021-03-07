
# Vue.js
## 一些技巧
### vue强制刷新子组件
[转](https://www.cnblogs.com/betty-niu/p/11199082.html)  
父组件中利用v-if 强制刷新子组件
```Html
<template>
    <Son v-if="sonRefresh"></Son>
</template>
<script>
import Son from './Son'
export default {
    components:{
        Son
    },
    data(){
    　　return {
    　　　　sonRefresh: true
    　　}
    },
    methods:{
        aFn:function(){
            this.sonRefresh= false;
            this.$nextTick(() => {
                this.sonRefresh= true;
            });
        }
    }
}
</script>
```

## Vue3
### vue3基础
#### Composition API
- setup()函数
  - 在beforeCreate之前调用，所有变量、方法都在setup函数中定义，最后return出去给模板使用
  - 该函数有2个参数：
    props
    context
    其中context是一个上下文对象，具有属性（attrs，slots，emit，parent，root），其对应于vue2中的this.$attrs，this.$slots，this.$emit，this.$parent，this.$root。

- ref 和 reactive
  - ref和reactvie的数据都是响应式的，但是如果需要对整个对象进行重新赋值, 那么用ref，如果只是改变属性，用reactive
  - 基本类型值（String 、Nmuber 、Boolean 等）或单值对象（类似像 {count: 3} 这样只有一个属性值的对象）使用 ref
  - 引用类型值（Object 、Array）使用 reactive
- toRef和ref
  toRef 是将某个对象中的某个值转化为响应式数据，其接收两个参数，第一个参数为 obj 对象；第二个参数为对象中的属性名
  - ref 是对原数据的拷贝，响应式数据对象值改变后会同步更新视图，不会影响到原始值
  - toRef 是对原数据的引用，响应式数据对象值改变后不会改变视图，会影响到原始值。
  - 用法
    ```javascript
    const obj = {count: 3}
    const state1 = ref(obj.count) //对象.属性
    const state2 = toRef(obj, 'count') //对象，属性
    ```
- toRefs
  将传入的对象里所有的属性的值都转化为响应式数据对象(ref)
  解构返回
  ```javascript
  <template>
    {{str}}
  </template>
  <script>
  import {toRefs,reactive} from 'vue'
  export default {
      setup() {
        const state = reactive({
          str: '响应式数据',
        });
        return {...toRefs(state)}
      }
  }
  </script>
  ```

### vue3-vite-electron
待成熟再用，现在没啥好的工具

[官方推荐github模板地址](https://github.com/vitejs/awesome-vite#templates)  
[vite推荐社区模板](https://cn.vitejs.dev/guide/#%E7%A4%BE%E5%8C%BA%E6%A8%A1%E6%9D%BF)

!> nodejs版本 > 12.0

- 创建
  ```
  yarn create @vitejs/app  project-name --template vue-ts
  ```
- 安装依赖
  ```
  cd project-name
  yarn
  ```
- 启动
  ```
  yarn dev
  ```

### vue3-vue-cli-electron 
有成熟工具

- 创建
  ```
  vue create project-name
  ```
- 添加electron builder
  ```
  cd project-name
  vue add electron-builder
  ```
- 启动
  ```
  yarn electron:dev
  ```
- 打包
  ```
  yarn electron:build
  ```

[electron打包配置](https://www.electron.build/configuration/configuration)

- 添加自动更新
  ```
  yarn add electron-updater
  ```
