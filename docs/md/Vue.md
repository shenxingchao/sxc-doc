
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
### vue3-vite-electron

!> nodejs版本 > 12.0

- 创建
  ```
  yarn create @vitejs/app project-name --template vue
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
- 安装electron相关依赖
  ```
  yarn add electron electron-builder electron-updater -dev
  //核心 构建工具 更新工具
  ```