# :id=notebook
# Css
## less
### VSCODE EASY WXLESS 插件编译问题之calc
![calc](./images/calc.jpg ':size=30%')

VSCODE 使用 __EASY WXLESS__ 插件 遇到一个问题
这个插件是让.less的文件能够自动转成.wxss文件
确实方便了许多，但还是有点问题

当我在less里写 __width:calc(100%-10rpx)__ 的时候,他就会自动给我编译成 __width:calc(90%)__ 。很明显，这是个问题，后来去作者的github查看，发现有人遇到类似的问题

解决方案就是在样式上加上
```less
<style lang="less">
~''~
</style>
```
如下
```less
<style lang="less">
    .class{
        width:~'calc(100%-10rpx)'~
    }
</style>
```
据说这样就会让浏览器去编译，而不是软件编译
最后生成的wxss文件里面就是下面这样的
```Css
<style>
    width:calc(100%-10rpx)
</style>
```
这样就达到了我们想要的效果

# Javascript
## typescript
## es6
### 数组去重
![calc](./images/es6_array_duplicate.png ':size=30%')
#### 1.普通数组去重
```Javascript
<script>
    let arr = [1, 2, 3, 2, 1];
    let temp = new Set(arr);
    console.log([...temp]); //输出[1, 2, 3]
</script>
```
#### 2.对象数组去重 某个值
##### 数组方法
```Javascript
<script>
    arr = [
        { name: 1, value: 2 },
        { name: 2, value: 3 },
        { name: 1, value: 2 },
        { name: 4, value: 3 },
    ];
    temp = [];
    let newArr = arr.filter(
    (item) => !temp.includes(item.value) && temp.push(item.value)
    );
    console.log(newArr); //输出{name: 1, value: 2}{name: 2, value: 3}
</script>
```
##### set方法
```Javascript
<script>
    temp = new Set();
    newArr = arr.filter(
    (item) => !temp.has(item.value) && temp.add(item.value)
    );
    console.log(newArr); //输出{name: 1, value: 2}{name: 2, value: 3}
</script>
```
##### map方法
```Javascript
<script>
    temp = new Map();
    newArr = arr.filter(
    (item,key) => !temp.has(item.value + '') && temp.set(item.value + '',true)
    );
    console.log(newArr); //输出{name: 1, value: 2}{name: 2, value: 3}
</script>
```
?>三个方法都可以封装为  fn(arr,key)  key 即是item.value的value

#### 3.对象数组去重 整个对象
```Javascript
<script>
    temp = new Map();
    newArr = arr.filter((item,key)=> 
        !temp.has(JSON.stringify(item)) && temp.set(JSON.stringify(item),true)
    );
    console.log(newArr); //输出{name: 1, value: 2}{name: 2, value: 3}{name: 4, value: 3}
</script>
```
