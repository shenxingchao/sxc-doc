# Css
## style
### div水平垂直居中的常用方法
<p align="left" style="color:#777777;">发布日期：2021-02-01</p>

假如有如下代码
```html run {height: '200px', row: true, open: true} 
<template>
<div class="wrap">
    <div class="wrap-box">
        333
    </div>
</div>
</template>
<style>
    .wrap{
        width:50px;
        height:50px;
        background:red;
    }
    .wrap-box{
        width:30px;
        height:30px;
        background:blue;
    }
</style>
```
1. 最简单的采用flex
```html run {height: '200px', row: true, open: true} 
<template>
<div class="wrap">
    <div class="wrap-box">
        333
    </div>
</div>
</template>
<style>
    .wrap{
        width:50px;
        height:50px;
        background:red;
        display:flex;/*这句*/
    }
    .wrap-box{
        width:30px;
        height:30px;
        background:blue;
        margin:auto;/*这句*/
    }
</style>
```
2. 采用绝对定位 需要确定wrap-box的高度
```html run {height: '200px', row: true, open: true} 
<template>
<div class="wrap">
    <div class="wrap-box">
        333
    </div>
</div>
</template>
<style>
    .wrap{
        width:50px;
        height:50px;
        background:red;
        position:relative;/*这句*/
    }
    .wrap-box{
        width:30px;
        height:30px;
        background:blue;
        position:absolute;/*这句*/
        top:0;/*这句*/
        right:0;/*这句*/
        bottom:0;/*这句*/
        left:0;/*这句*/
        margin:auto;/*这句*/
    }
</style>
```
3. 采用flex布局里的另外2个属性
```html run {height: '200px', row: true, open: true} 
<template>
<div class="wrap">
    <div class="wrap-box">
        333
    </div>
</div>
</template>
<style>   
    .wrap{
        width:50px;
        height:50px;
        background:red;
        display:flex;/*这句*/
        justify-content:center;/*这句*/
        align-items:center;/*这句*/
    }
    .wrap-box{
        width:30px;
        height:30px;
        background:blue;
    }
</style>
```
4. 采用translate
```html run {height: '200px', row: true, open: true} 
<template>
<div class="wrap">
    <div class="wrap-box">
        333
    </div>
</div>
</template>
<style>   
    .wrap{
    width:50px;
    height:50px;
    background:red;
    position:relative;/*这句*/
    }
    .wrap-box{
        width:30px;
        height:30px;
        background:blue;
        position:absolute;/*这句*/
        top:50%;/*这句*/
        left:50%;/*这句*/
        transform:translate(-50%,-50%);/*这句*/
    }
</style>
```
    
### 父元素高度不确定的情况下-子元素高度相等-右边元素高度始终与左边相等
<p align="left" style="color:#777777;">发布日期：2021-01-26</p>

###### 1.纯CSS实现
```html
<style>
    .wrap{
        width: 100%;
        height: auto;
        background-color: red;
        display: flex;
        position: relative;
        color: #ffffff;
    }
    .left{
        width: 50%;
        background-color: green;
    }
    .right{
        position: absolute;
        top: 0;
        right: 0;
        width: 50%;
        height: 100%;
        background-color: blue;
    }
</style>
<div class="wrap">
    <div class="left">
        <p>123</p>
        <p>123</p>
        <p>123</p>
        <p>123</p>
        <p>123</p>
    </div>
    <div class="right"></div>
</div>
```
###### 2.flex 实现
```html
<style>
    .wrap{
        width: 100%;
        height: auto;
        background-color: red;
        display: flex;
        align-items: stretch; 
        color: #ffffff;
    }
    .item{
        flex:1;
        
    }
    .left{
        background-color: green;
    }
    .right{
        background-color: blue;
    }
</style>
<div class="wrap">
    <div class="item left">
        <p>123</p>
        <p>123</p>
        <p>123</p>
        <p>123</p>
        <p>123</p>
    </div>
    <div class="item right">
    </div>
</div>
```
__效果如下图__   
![calc](../images/css_left_right_height.png ':size=30%')  

## less
### VSCODE EASY WXLESS 插件编译问题之calc
<p align="left" style="color:#777777;">发布日期：2019-04-09</p>

VSCODE 使用 __EASY WXLESS__ 插件 遇到一个问题
这个插件是让.less的文件能够自动转成.wxss文件
确实方便了许多，但还是有点问题

当我在less里写 __width:calc(100%-10rpx)__ 的时候,他就会自动给我编译成 __width:calc(90%)__ 。很明显，这是个问题，后来去作者的github查看，发现有人遇到类似的问题

解决方案就是在样式上加上
```
~''~
```
如下
```less
<style lang="less">
    .class{
        width:~'calc(100%-10rpx)'~;
    }
</style>
```
据说这样就会让浏览器去编译，而不是软件编译
最后生成的wxss文件里面就是下面这样的
```Css
<style>
    .class{
        width:calc(100%-10rpx);
    }
</style>
```
这样就达到了我们想要的效果


