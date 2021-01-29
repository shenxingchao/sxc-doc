# Javascript
## 方法
### 浮点数四舍五入保留小数位数
<p align="left" style="color:#777777;">发布日期：2021-01-26</p>

错误写法使用toFixed()，导致精度丢失
```javascript
let num = 0.35;
num = num.toFixed(1); //输出0.3 并没有进位
```
!>千万不要直接使用toFixed 保留小数
正确写法使用Math.round()四舍五入取整后，使用toFixed()保留
```javascript
let num = 0.35;
num = (Math.round(num * 10) / 10).toFixed(1); //输出0.4
```
!>精髓就是先四舍五入，再保留。保留几位小数就*多少

?>实际开发可以直接用lodash中的[ceil](https://www.lodashjs.com/docs/lodash.ceil)方法
### 数组按某个值排序
<p align="left" style="color:#777777;">发布日期：2021-01-22</p>

?>按order值的大小，对数组List进行升序排序
```javascript
let List = [{
        id: 1,
        order: 1
    },
    {
        id: 2,
        order: 0
    }
];
List.sort((x, y) => {
    return x.order - y.order
});
```
!>sort会改变原数组内容 若需要保留原数组 使用深拷贝即可
### 数组对象按某个属性值分组
<p align="left" style="color:#777777;">发布日期：2021-01-22</p>

?>就是以对象的某个属性作为索引值key 变成一个关联数组，然后再用Object.keys 循环关联数组赋给新数组，以去掉索引key
```javascript
let List = [{
        id: '1001',
        name: '值1',
        value: '1'
    },
    {
        id: '1001',
        name: '值1',
        value: '2'
    },
    {
        id: '1002',
        name: '值2',
        value: '3'
    },
    {
        id: '1002',
        name: '值2',
        value: '4'
    },
    {
        id: '1002',
        name: '值2',
        value: '5'
    },
    {
        id: '1003',
        name: '值3',
        value: '6'
    },
];
let map = {}
for (let i = 0; i < List.length; i++) {
    let item = List[i]
    if (!map[item.id]) {
        map[item.id] = [item]
    } else {
        map[item.id].push(item)
    }
}
let res = []
Object.keys(map).forEach(key => {
    res.push(map[key])
})
console.log(res)
```
## jquery
### jquery插件编写模板
<p align="left" style="color:#777777;">发布日期：2019-04-02</p>

可以编写各种插件，如弹窗，表格，选项卡，等等。
```javascript
;(function($){
    $.fn.functionName1 = function(options){
        var defaults = {
            //默认值
        }
        var options = $.extend(defaults,options);
        this.each(function(){ 
            var _this = $(this);
			//功能编写
        });
        return this;
}

$.fn.functionName2 = function(options){
		...
}
})(jQuery);
```

## typescript

* * *

## es6
### 数组去重
<p align="left" style="color:#777777;">发布日期：2020-11-13</p>

##### 普通数组去重
```javascript
let arr = [1, 2, 3, 2, 1];
let temp = new Set(arr);
console.log([...temp]); //输出[1, 2, 3]
```

##### 对象数组去重 某个值
__1. 数组方法__
```javascript
arr = [{
        name: 1,
        value: 2
    },
    {
        name: 2,
        value: 3
    },
    {
        name: 1,
        value: 2
    },
    {
        name: 4,
        value: 3
    },
];
temp = [];
let newArr = arr.filter(
    (item) => !temp.includes(item.value) && temp.push(item.value)
);
console.log(newArr); //输出{name: 1, value: 2}{name: 2, value: 3}
```
__2. set方法__
```javascript
temp = new Set();
newArr = arr.filter(
    (item) => !temp.has(item.value) && temp.add(item.value)
);
console.log(newArr); //输出{name: 1, value: 2}{name: 2, value: 3}
```
__3. map方法__
```javascript
temp = new Map();
newArr = arr.filter(
    (item, key) => !temp.has(item.value + '') && temp.set(item.value + '', true)
);
console.log(newArr); //输出{name: 1, value: 2}{name: 2, value: 3}
```
?>三个方法都可以封装为  fn(arr,key)  key 即是item.value的value


##### 对象数组去重 整个对象
```javascript
temp = new Map();
newArr = arr.filter((item, key) =>
    !temp.has(JSON.stringify(item)) && temp.set(JSON.stringify(item), true)
);
console.log(newArr); //输出{name: 1, value: 2}{name: 2, value: 3}{name: 4, value: 3}
```

### 求数组并集、差集、交集
<p align="left" style="color:#777777;">发布日期：2021-01-26</p>

```javascript
let a = new Set([1, 2, 3, 4, 5]);
let b = new Set([1, 2, 3, 6]);
let union = new Set([...a, ...b]); //并集 输出1,2,3,4,5,6
let difference1 = [...union].filter(x => (!a.has(x) || !b.has(x))); //差集 输出4,5,6
let difference2 = [...b].filter(x => !a.has(x)); //返回a在b中没有的  输出6
let difference3 = [...a].filter(x => !b.has(x)); //返回b在a中没有的  输出4,5
let intersect = [...a].filter(x => b.has(x)); //交集 返回a和b共有的  也可以反着来 输出1,2,3
```

## 框架
### apidoc
<p align="left" style="color:#777777;">发布日期：2021-01-28</p>

[apidoc官方网站](https://apidocjs.com/#demo)

1. 已经安装了node.js 和 npm
2. 安装apidoc
```npm
npm install apidoc -g
```
3. 创建apidoc.json 在项目根目录
内容如下
```json
{
    "name": "接口文档",
    "version": "0.3.0",
    "description": "接口描述",
    "url": "http://www.baidu.com",
    "sampleUrl": "http://test.baidu.com"
}
```

?>url和sampleUrl分别为正式地址和测试地址

4. 按他的规则写注释如下
```
/**
 * @api {get} /user/:id Get方法获取用户信息（前面的是接口名称）
 * @apiVersion 0.2.2
 * @apiName getUserInfo（前面的是方法名）
 * @apiGroup 用户
 * 
 * @apiParam (参数) {Number} id 用户id
 * @apiParamExample {json} 请求示例
 * {
 *  "id": 1
 * }
 * @apiSuccess (返回字段) {String} firstname 姓
 * @apiSuccess (返回字段) {String} lastname  名字
 *
 * @apiSuccessExample 成功示例
 * HTTP/1.1 200 Success
 *    {
 *       "firstname": "张",
 *       "lastname": "三四"
 *     }
 * @apiErrorExample 失败示例1
 *     {
 *       "code": "1001"
 *     }
 * @apiErrorExample 失败示例2
 *     {
 *       "code": "1002"
 *     }
 * @apiError (错误代码) 1001 内容1
 * @apiError (错误代码) 1002 内容2
 */
/**
 * @api {post} /user Post方法获取用户信息（前面的是接口名称）
 * @apiVersion 0.2.2
 * @apiName getUserInfoByPost（前面的是方法名）
 * @apiGroup 用户
 * 
 * @apiParam (参数) {Number} id=2 用户id
 * @apiParam (参数) {Number} [age=4] 年龄(中括号表示可选)
 * @apiParamExample {json} 请求示例
 * {
 *  "id": 1，
 *  "age": 2
 * }
 * @apiSuccess (返回字段) {String} firstname 姓
 * @apiSuccess (返回字段) {String} lastname  名字
 *
 * @apiSuccessExample 成功示例
 * HTTP/1.1 200 Success
 *    {
 *       "firstname": "张",
 *       "lastname": "三四"
 *     }
 * @apiErrorExample 失败示例1
 *     {
 *       "code": "1001"
 *     }
 * @apiErrorExample 失败示例2
 *     {
 *       "code": "1002"
 *     }
 * @apiError (错误代码) 1001 内容1
 * @apiError (错误代码) 1002 内容2
 */
```
5. 生成api文档到指定目录
```
apidoc -i ./test（需要扫描的文件夹） -o ./doc(存放的文件夹) -f .php(需要生成接口的文件类型)
```
6. 版本控制
建立一个同类型的后缀文件 如_olddoc.php 存放之前接口的注释就可以

