
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

### vue3-vuecli-electron 
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