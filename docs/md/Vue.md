
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

