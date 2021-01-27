# :id=notebook

![https://api.xhboke.com/news/](https://api.xhboke.com/news/)

* * *

# Site
| 主题                                                                                                                                |    标签    |   更新日期    |
| :---------------------------------------------------------------------------------------------------------------------------------- | :--------: | :-----------: |
| [MDN 前端开发学习网站](https://developer.mozilla.org/zh-CN/)                                                                        |    MDN     | 2021年1月26日 |
| [iconfont  阿里巴巴矢量图标库](https://www.iconfont.cn/)                                                                            |    MDN     | 2021年1月27日 |
| [free api test 苍穹精华 API](https://api.xhboke.com/doc/)                                                                           |  Markdown  | 2021年1月25日 |
| [json对比 用于接口json格式参数对比](https://www.sojson.com/jsondiff.html)                                                           |    json    | 2021年1月26日 |
| [docsify一个神奇的文档网站生成器](https://docsify.js.org/#/zh-cn/)                                                                  |  Markdown  | 2021年1月25日 |
| [mockjs在线格式编辑](http://mockjs.com/0.1/editor.html#help)                                                                        |  Mock.js   | 2021年1月25日 |
| [Lodash JavaScript 实用工具库](https://www.lodashjs.com/)                                                                           |  js 插件   | 2021年1月26日 |
| [SortableJS功能强大的JavaScript 拖拽库](http://www.sortablejs.com)                                                                  |  js 插件   | 2021年1月25日 |
| [Vuex Vue.js状态管理](https://vuex.vuejs.org/zh/guide/)                                                                             |    vue     | 2021年1月25日 |
| [Element，一套为开发者、设计师和产品经理准备的基于 Vue 2.0 的桌面端组件库](https://element.eleme.cn/#/zh-CN/component/installation) | vue ui框架 | 2021年1月25日 |
| [uni-app一个使用 Vue.js 开发所有前端应用的框架](https://uniapp.dcloud.net.cn/quickstart-cli)                                        |  vue app   | 2021年1月25日 |
| [vue-gaode在Vue中完美的使用高德地图](http://vue-gaode.rxshc.com/)                                                                   |  vue 地图  | 2021年1月25日 |
| [香蕉云编无苹果电脑上传ipa](https://www.yunedit.com/)                                                                               |    ios     | 2021年1月25日 |
| [微信开发文档-公众号-小程序](https://developers.weixin.qq.com/doc/)                                                                 |   wechat   | 2021年1月26日 |

* * *

# My Project
| 项目名称                                                                                                                                                                                          |             类型              |       更新日期        |
| :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | :---------------------------: | :-------------------: |
| [Helpdeco网上商城](https://www.helpdeco.com/mobile/#/)-[扫码查看](http://demo.o8o8o8.com/vue/shop/qrcode.png)                                                                                     |          vue 公众号           | 2018年7月 ~ 2018年9月 |
| [医药类H5基于vue+vantui+mockjs](http://demo.o8o8o8.com/fastmedicine/#/)                                                                                                                           |            vue h5             |       2020年1月       |
| [多商家微信小程序](https://shenxingchao.github.io/project/image/lyqq_qrcode.jpg)                                                                                                                  |      wechat mini program      |       2019年7月       |
| [工作台基于vue+electron+thinkphp5.0+elementui+workerman+redis](https://shenxingchao.github.io/project/video/workbench.mp4)                                                                        |         vue electron          |      2019年10月       |
| [spladmin基于thinkphp5.1+adminLTE框架开发一键生成CURD 集成 权限管理 菜单管理 全局设置](http://spladmin.o8o8o8.com/admin) 演示账号：test 111111                                                    |      thinkphp5.1 bootstrap       |       2019年9月       |
| [vue-admin-thinkphp5是基于vue-admin-template+thinkphp5的后台权限管理系统](https://github.com/shenxingchao/vue-admin-thinkphp5) -[演示视频](http://demo.o8o8o8.com/vue/vueAdminTemplate/index.html) | thinkphp5.0 vue-element-template |       2019年1月       |
| [vue-admin-elementui mockjs async-router elementui i18n custom-table custom-theme curd-demo](https://shenxingchao.github.io/vue-admin-elementui/) 演示账号 任意字符登录                           |        vue elementui         |     2021年1月3日      |
| [vue-admin-thinkphp6基于vue-admin-elementui 和 thinkphp6.0.* 的 前后端分离项目模板 ——登录 增删改查 权限管理 composer](http://demo.o8o8o8.com/vue-admin-thinkphp6/#/) 演示账号 admin admin                           |        vue thinkphp6.0.* vue-admin-elementui php8.0   |     2021年1月16日      |



* * *

# Css
## style
### 父元素高度不确定的情况下，子元素高度相等，右边元素高度始终与左边相等
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
![calc](./images/css_left_right_height.png ':size=30%')  

## less
### VSCODE EASY WXLESS 插件编译问题之calc
<p align="left" style="color:#777777;">发布日期：2019-04-09</p>

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
    .class{
        width:calc(100%-10rpx)
    }
</style>
```
这样就达到了我们想要的效果

* * *

# Javascript
## 方法
### 浮点数四舍五入保留小数位数
<p align="left" style="color:#777777;">发布日期：2021-01-26</p>

错误写法使用toFixed()，导致精度丢失
```javascript
<script>
    let num = 0.35;
    num = num.toFixed(1);//输出0.3 并没有进位
</script>
```
!>千万不要直接使用toFixed 保留小数
正确写法使用Math.round()四舍五入取整后，使用toFixed()保留
```javascript
<script>
    let num = 0.35;
    num = (Math.round(num*10)/10).toFixed(1);//输出0.4
</script>
```
!>精髓就是先四舍五入，再保留。保留几位小数就*多少
?>实际开发可以直接用lodash中的[ceil](https://www.lodashjs.com/docs/lodash.ceil)方法
### 数组按某个值排序
<p align="left" style="color:#777777;">发布日期：2021-01-22</p>

?>按order值的大小，对数组List进行升序排序
```javascript
<script>
    let List = [
        {
            id:1,
            order:1
        },
        {
            id:2,
            order:0
        }
    ];
    List.sort((x, y) => {
        return x.order - y.order
    });
</script>
```
!>sort会改变原数组内容 若需要保留原数组 使用深拷贝即可
### 数组对象按某个属性值分组
<p align="left" style="color:#777777;">发布日期：2021-01-22</p>

?>就是以对象的某个属性作为索引值key 变成一个关联数组，然后再用Object.keys 循环关联数组赋给新数组，以去掉索引key
```javascript
<script>
    let List =  [
        { id: '1001', name: '值1', value: '1' },
        { id: '1001', name: '值1', value: '2' },
        { id: '1002', name: '值2', value: '3' },
        { id: '1002', name: '值2', value: '4' },
        { id: '1002', name: '值2', value: '5' },
        { id: '1003', name: '值3', value: '6' },
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
</script>
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
<script>
    let arr = [1, 2, 3, 2, 1];
    let temp = new Set(arr);
    console.log([...temp]); //输出[1, 2, 3]
</script>
```

##### 对象数组去重 某个值
__1. 数组方法__
```javascript
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
__2. set方法__
```javascript
<script>
    temp = new Set();
    newArr = arr.filter(
    (item) => !temp.has(item.value) && temp.add(item.value)
    );
    console.log(newArr); //输出{name: 1, value: 2}{name: 2, value: 3}
</script>
```
__3. map方法__
```javascript
<script>
    temp = new Map();
    newArr = arr.filter(
    (item,key) => !temp.has(item.value + '') && temp.set(item.value + '',true)
    );
    console.log(newArr); //输出{name: 1, value: 2}{name: 2, value: 3}
</script>
```
?>三个方法都可以封装为  fn(arr,key)  key 即是item.value的value


##### 对象数组去重 整个对象
```javascript
<script>
    temp = new Map();
    newArr = arr.filter((item,key)=> 
        !temp.has(JSON.stringify(item)) && temp.set(JSON.stringify(item),true)
    );
    console.log(newArr); //输出{name: 1, value: 2}{name: 2, value: 3}{name: 4, value: 3}
</script>
```

### 求数组并集、差集、交集
<p align="left" style="color:#777777;">发布日期：2021-1-26</p>

```javascript
<script>
    let a = new Set([1,2,3,4,5]);
    let b = new Set([1,2,3,6]);
    let union = new Set([...a,...b]);//并集 输出1,2,3,4,5,6
    let difference1 = [...union].filter(x => (!a.has(x) || !b.has(x)));//差集 输出4,5,6
    let difference2 = [...b].filter(x=>!a.has(x));//返回a在b中没有的  输出6
    let difference3 = [...a].filter(x=>!b.has(x));//返回b在a中没有的  输出4,5
    let intersect = [...a].filter(x => b.has(x));//交集 返回a和b共有的  也可以反着来 输出1,2,3
</script>
```

* * *

# Vue.js
## 一些技巧
### vue 强制刷新子组件
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

* * *

# UI框架
## Element ui
## Vant ui

* * *

# Linux
## 常用命令
### 创建多级目录
mkdir -p
## vim命令

* * *

# Web服务器
## apcahe
## nginx
### Nginx开启gzip压缩
开启gzip压缩，可以大大减少请求文件的大小，加快网页的访问速度
配置如下：
```nginx
server {
	......

    # 开启gzip
    gzip on;
    # 启用gzip压缩的最小文件，小于设置值的文件将不会压缩
    gzip_min_length 1k;
    # gzip 压缩级别，1-10，数字越大压缩的越好，也越占用CPU时间，后面会有详细说明
    gzip_comp_level 2;
    # 进行压缩的文件类型。javascript有多种形式。其中的值可以在 mime.types 文件中找到。
    gzip_types text/plain application/javascript application/x-javascript text/css application/xml text/javascript application/x-httpd-php image/jpeg image/gif image/png;
    # 是否在http header中添加Vary: Accept-Encoding，建议开启
    gzip_vary on;
    # 禁用IE 6 gzip
    gzip_disable "MSIE [1-6]\.";
}
```
查看是否开启，
看浏览器->F12->network –>response headers 
Content-Encoding: gzip
那便是开启了，而且文件的SIZE和你上传的文件大小变小了很多

* * *

# 版本管理工具
## svn服务器搭建
### centos7.2 svn服务器搭建
<p align="left" style="color:#777777;">发布日期：2020-02-25</p>

###### 1.putty登陆服务器

###### 2.安装svn服务端
    yum -y install subversion

###### 3.查看安装位置
    rpm -ql subversion

###### 4.查看版本
    svnserve --version

###### 5.*创建svn版本库目录
    mkdir -p /var/svn/sxc

###### 6.*创建版本库
    svnadmin create /var/svn/sxc

###### 7.*进入版本库
    cd /var/svn/sxc

###### 8.查看列表
    ls -l
    
###### 9.进入
    cd /var/svn/sxc/conf  
有三个文件 
1. authz权限管理 
2. passwd账号密码管理 
3. svnserve.conf配置文件
   
###### 10.添加账号密码
    vi passwd  
1. 末尾加上用户名=密码（i编辑）
2. 退出并保存esc+:wq
   
###### 11.修改权限
    vi authz 
- 末尾加上
- [\]
- 用户 = rw
- 表示用户有读写权限，修改完并保存
  
###### 12.修改配置文件
    vim /var/svn/sxc/conf/svnserve.conf
- 取消以下注释  并修改realm为你的svn版本库目录
- anon-access = read  #匿名用户可读
- auth-access = write #授权用户可写
- password-db = passwd #SVN账号文件
- authz-db = authz  #SVN权限文件
- realm = /var/svn/sxc  #SVN版本库目录
- 这里会出现一个配置的问题，配置项需要顶格写
  
###### 12.启动svn版本库
    svnserve -d -r /var/svn/
- 若无法启动，说明svn服务已经被占用，
- 执行 killall svnserve 关掉当前正在执行的SVN进程后即可。
  
###### 13.查看svn是否启动
    ps aux | grep svn

###### 14.防火墙开启3690端口
- centos查询端口是不是开放的
```    
firewall-cmd --permanent --query-port=3690/tcp
```
- 添加对外开放端口
```
firewall-cmd --permanent --add-port=3690/tcp
```
- 重启防火墙
```
firewall-cmd --reload
```
- 查看端口监听情况
```
netstat -nlp
```
- 启动防火墙
```
systemctl start firewalld 
```

###### 15.无法连接主机“X.X.X.X”: 由于连接方在一段时间后没有正确答复或连接的主机没有反应，连接尝试失败。
- 需要配置安全组规则
- 入站规则 协议 0.0.0.0/0 所有来源可访问 tcp:3690  策略:允许
  
###### 16.拓展，多个库使用一个配置文件
-移动conf到svn目录
```
mv -f /var/svn/sxc/conf   /var/svn/conf
```
- 修改配置文件
```
anon-access = read  #匿名用户可读
auth-access = write #授权用户可写
password-db = /var/svn/conf/passwd #SVN账号文件
authz-db = /var/svn/conf/authz  #SVN权限文件
realm = /var/svn  #SVN版本库目录
```

###### 17.*启动svn
    killall svnserve
    svnserve -d -r /var/svn --config-file=/var/svn/conf/svnserve.conf （多个库访问一套配置文件 需要制定   配置文件的位置 所以使用这个代码启动服务）
    ps aux | grep svn

###### 18.*checkout
    svn://ip/sxc
###### 19.下次建版本库只要执行带*号的就可以了

###### 20.服务器web目录检出空目录（空版本库）
```
cd /usr/local/nginx/html/sxc
svn checkout svn://x.x.x.x/sxc(目录或文件的全路径)　./(当前web目录路径])
svn add *
svn add . --no-ignore --force (全部包括隐藏文件)
svn commit -m "sxc"
svn update
```
## git
### 常用操作和命令
<p align="left" style="color:#777777;">发布日期：2020-07-24</p>

?> [SSH公钥配置](https://www.jianshu.com/p/464a3373d15c)

###### 1.下载git客户端 官网下载即可
###### 2.打开命令行 配置全局用户名邮箱
```git
$ git config --global user.name "Your Name"
$ git config --global user.email "email@example.com"
```
###### 3.初始化git仓库
```git
$ git init
```
###### 4.告诉git添加单个文件到仓库(可以反复添加，一次提交即可，就是添加到暂存区)
```git
$ git add test.html
```
###### 5.单个文件提交到仓库（就是提交到当前分支）
```git
$ git commit -m "我添加了第一个文件test.html"
```
###### 6.查看仓库状态，修改了或者新增了什么文件
```git
$ git status
```
###### 7.查看修改了哪些内容
```git
$ git diff
```
###### 8.提交修改的文件是重复4，5这两步
```git
$ git add test.html
$ git commit -m "我修改了test.html"
```
###### 9.查看提交日志
```git
$ git log
```
###### 10.查看简略提交日志
```git
$ git log --pretty=oneline
```
###### 11.版本回退到上一版本
```git
$ git reset --hard HEAD^
```
###### 12.版本恢复到回退前的版本
```git
$ git reset --hard 1234a (就是commit id，前几位就可以)
```
###### 13.查看命令行历史，可用于忘记commit id，去回退版本
```git
$ git reflog
```
###### 14.查看工作区和版本库最新版本的区别
```git
$ git diff HEAD -- test.html(查看指定文件)
```
###### 15.撤销修改 恢复到最近一次git commit或git add的状态 丢弃工作区的修改
```git
$ git checkout -- test.html
```
###### 16.暂存区修改回退到工作区
```git
$ git reset HEAD  test.html
```
###### 17.删除文件
```git
$ git rm test.html
$ git commit -m "delete test.html"
```
###### 18.如果没有提交删除文件，只提交到暂存区（就是只执行了git rm操作），那么回退，提交了就只能回退版本
```git
$ git reset HEAD test.html
$ git checkout --test.html
```
###### 19.本地仓库关联远程仓库
```git
$ git remote add origin git@github.com:xxx/demo.git
```
###### 20.把当前master分支推送到远程仓库
```git
$ git push -u origin master
```
###### 21.SSH key获取方法
```git
$ ssh-keygen -t rsa -C  "email@example.com"
$ cat ~/.ssh/id_rsa.pub
```
###### 22.克隆远程仓库到本地,直接文件夹下来
```git
$ git clone git@github.com:xxx/demo.git
```
###### 23.创建dev分支
```git
$ git branch dev
```
###### 24.切换到dev分支
```git
$ git checkout dev 或 $ git switch dev
```
###### 25.创建dev分支并切换到该分支
```git
$ git checkout -b dev 或 $ git switch -c dev
```
###### 26.查看当前分支
```git
$ git branch
```
###### 27.合并dev分支到master分支上，这种合并没有历史提交记录
```git
$ git merge dev （合并指定分支到当前分支）
```
###### 28.删除dev分支
```git
$ git branch -d dev
```
###### 29.合并分支分支历史记录分支信息,合并会创建一个新的commit
```git
$ git merge --no-ff -m "merge with no-ff" dev
```
###### 30.存储当前工作区（还未提交）
```git
$ git stash
```
###### 31.查看存储的工作区列表
```git
$ git stash list
```
###### 32.恢复存储的工作区
```git
$ git stash apply
```
###### 33.删除存储的工作区
```git
$ git stash drop
```
###### 34.恢复并删除存储的工作区
```git
$ git stash pop
```
###### 35.恢复指定存储的工作区
```git
$ git stash pop stash@{0}
```
###### 36.修复某个分支的BUG 并把该BUG提交复制到当前分支,假如当前分支是dev，bug修复在master分支
用法
```git
$ git cherry-pick 100aaa
```
流程
```git
$ git stash //保存当前工作区
$ git switch master //切换到master分支
$ git switch -c issue-101 //创建问题101分支
$ git add .
$ git commit -m "fix bug 101"
$ git switch master
$ git merge --no-ff -m "meged bug fix 101" issue-101 //合并分支
$ git log --pretty=oneline //找到bug提交的commit id 假如是100aaa
$ git branch -d issue-101 //删除分支
$ git switch dev
$ git cherry-pick 100aaa //复制这个提交到当前分支
$ git stash pop //恢复存储的工作区并删除
```
总结：修复bug时，先保存当前工作区，创建新的bug分支进行修复，然后合并，最后删除，然后在恢复工作区
###### 37.强制删除没有合并过的分支
```git
$ git branch -D feature
```
###### 38.新功能使用feature分支
###### 39.创建远程分支到本地
```git
$ git checkout -b dev origin/dev
```
###### 40.指定分支拉取最新远程仓库代码
```git
$ git checkout dev
$ git pull
```
###### 41.git pull拉取失败需要先指定本地dev分支与远程分支之间的连接
```git
$ git branch --set-upstream-to=origin/dev dev  (远程分支名称，本地分支名称)
```
###### 42.查看分支的合并情况
```git
$ git log --graph --pretty=oneline --abbrev-commit
```
###### 43.把分叉的提交历史 整理成一条直线
```git
$ git rebase
```
###### 44.当前分支加标签
```git
$ git tag v1.0 aaa100（默认是打在最新的commit上，要打以前的加commit id）
```
###### 45.查看标签
```git
$ git tag
```
###### 46.查看标签信息
```git
$ git show v1.0
```
###### 47.删除标签
```git
$ git tag -d v1.0
```
###### 48.标签提交到远程仓库
```git
$ git push origin v1.0
```
###### 49.所有标签提交到远程仓库
```git
$ git push origin --tags
```
###### 50.删除远程标签，先删除本地的
```git
$ git tag -d v1.0
$ git push origin :refs/tags/v1.0
```
###### 51.拉去远程主分支
```git
step3
step19
$ git pull origin master
```
###### 52.提交时不改变文件的crlf
```git
$ git config --globa core.autocrlf false
```
###### 53.提交分支到远程
```git
$ git push --set-upstream origin dev
```
###### 54.撤销更改
```git
$ git checkout .
```
###### 55.解决冲突
    解决后add ,commit 后再合并分支
###### 56.提交后修改提交的注释文字-m
```git
$ git commit --amend
$ git push -f origin master
```

* * *

# Hybird App
## uniapp
### Native.js示例汇总
<p align="left" style="color:#777777;">发布日期：2021-01-26</p>

工作时用到了低功耗蓝牙开发BLE蓝牙  
[示例地址](https://ask.dcloud.net.cn/article/114)

* * *

# 微信
## 小程序
### 微信小程序之登录详解
<p align="left" style="color:#777777;">发布日期：2021-01-26</p>

直接上图，后面再写详细代码，及解释  
![calc](./images/wechat_mini_program.jpg ':size=70%')  
###### 1.入口文件pages/index/index.js onLoad或者OnShow检查session_key
```javascript
import auth from "../../utils/auth";
Page({
  data: {
  },
  //页面加载回调
  onLoad: function(options) {
    auth.checkSession();
  },
});
```
###### 2.下面上auth.js文件
```javascript
/**
 * 微信登录验证  需要登录的页面使用
 */
import http from "./request";
const auth = {
  /**
   * 检查微信登录session_key是否过期
   * 1.在需要获取敏感用户信息数据的时候，检查
   * 2.再重新登陆之前检查session_key
   */
  checkSession: function() {
    wx.checkSession({
      success() {
        //未过期，同时，检查token 是否存在，不存在也要重新执行登录流程,这里的token是和自己服务器换取信息重要凭证 这里其实还可以检查token的合法性 不合法token则重新登录 也可以在需要用token的时候返回错误代码，执行重新登录
        wx.getStorage({
          key: "token",
          fail() {
            // 不存在
            auth.checkAuth();
          }
        });
      },
      fail() {
        // session_key 已经失效，需要重新执行登录流程
        auth.checkAuth("expire");
      }
    });
  },
  /**
   * 检查是否授权 未授权则跳转到授权页
   * @param {*传session是否过期} type
   */
  checkAuth: function(type = "") {
    wx.getSetting({
      success(res) {
        if (res.authSetting["scope.userInfo"]) {
          //已授权,则开始登录
          auth.doLogin({ type: type, back: false });
        } else {
          //未授权
          wx.navigateTo({
            url: "/pages/auth_login/auth_login?type=" + type
          });
        }
      }
    });
  },
  /**
   * 执行登录操作
   * @param {Object} param0
   */
  doLogin: function({ type, back = true }) {
    wx.login({
      success(res) {
        if (res.code) {
          wx.getUserInfo({
            lang: "zh_CN",
            success(userInfo) {
              // 发起网络请求
              http.post({
                url: "后端处理url",
                data: {
                  code: res.code,
                  signature: userInfo.signature,
                  encryptedData: userInfo.encryptedData,
                  iv: userInfo.iv,
                  type: type
                },
                showLoading: false,
                success: function(res) {
                  if (res.data.code === 0) {
                    //设置token
                    wx.setStorage({
                      key: "token",
                      data: res.data.data
                    });
                    if (back) {
                      wx.navigateBack({
                        delta: 1
                      });
                    } else {
                      //刷新当前页
                      const pages = getCurrentPages();
                      const perpage = pages[pages.length - 1];
                      perpage.onShow();
                    }
                  } else {
                    wx.showToast({
                      title: "登录失败！",
                      icon: "none"
                    });
                  }
                }
              });
            }
          });
        } else {
          wx.showToast({
            title: "登录失败！",
            icon: "none"
          });
        }
      }
    });
  }
};

export default auth;
```
###### 3.下面是request.js代码
```javascript
import auth from "./auth";
let app = getApp();
let baseURL =
  app.globalData.production === "1"
    ? "正式接口url前缀"
    : "测试接口url前缀";

const request = {
  get: function({
    url,
    params = {},
    success,
    showLoading = true,
    isAuth = false
  }) {
    if (showLoading) wx.showLoading({ title: "加载中" });
    request.createHeader(isAuth, function(header) {
      wx.request({
        url: baseURL + url,
        method: "get",
        dataType: "json",
        data: params,
        header: header,
        success: function(res) {
          if (
            typeof res.header.Code !== "undefined" &&
            res.header.Code === "40001"
          ) {
            //token错误
            wx.removeStorage({
              key: "token",
              success(res) {
                auth.checkSession();
              }
            });
          }
          if (typeof res.header.Token !== "undefined") {
            wx.setStorage({
              key: "token",
              data: res.header.Token
            });
          }
          success(res);
        },
        complete: function() {
          wx.hideLoading();
        }
      });
    });
  },
  post: function({
    url,
    data = {},
    success,
    showLoading = true,
    isAuth = false
  }) {
    if (showLoading) wx.showLoading({ title: "加载中" });
    request.createHeader(isAuth, function(header) {
      wx.request({
        url: baseURL + url,
        method: "post",
        dataType: "json",
        data: data,
        header: header,
        success: function(res) {
          if (
            typeof res.header.Code !== "undefined" &&
            res.header.Code === "40001"
          ) {
            //token错误
            wx.removeStorage({
              key: "token",
              success(res) {
                auth.checkSession();
              }
            });
          }
          if (typeof res.header.Token !== "undefined") {
            wx.setStorage({
              key: "token",
              data: res.header.Token
            });
          }
          success(res);
        },
        complete: function() {
          wx.hideLoading();
        }
      });
    });
  },
  createHeader: function(isAuth, compete) {
    let header = {
      "content-type": "application/json;charset=UTF-8" // 默认值
    };
    if (isAuth) {
      wx.getStorage({
        key: "token",
        success(res) {
          //token存在 但是session_key过期 这里不影响因为用不到session_key
          header.Auth = res.data;
          compete(header);
        },
        fail() {
          // 不存在
          auth.checkSession();
        }
      });
    } else {
      compete(header);
    }
  }
};
export default request;
```
###### 4.下面是授权页面的js文件 auto_login.js
```javascript
import auth from "../../utils/auth";
Page({
  data: {
    canIUse: wx.canIUse("button.open-type.getUserInfo"),
    type: ""
  },
  onLoad: function(options) {
    this.setData({
      type: options.type
    });
  },
  bindGetUserInfo: function(e) {
    if (e.detail.errMsg !== "getUserInfo:ok") {
      //取消授权等原因
      wx.navigateBack({
        delta: 1
      });
    }
    let _this = this;
    auth.doLogin({
      type: _this.data.type
    });
  }
});
```
###### 5.最为期待的后端代码
```php
<?php
namespace app\index\controller;
use app\index\common\Base;
use think\Controller;
use think\Db;
use think\facade\Request;
use think\Exception;

class WeChat extends Base {

    private $appId = "你的appid";
    private $secret = "你的秘钥去你的小程序后台获取";

    public function doLogin(){
        $code = 0;
        $msg = 'success';
        $data = null;
        try{
            if(Request::isPost()){
                $param = Request::param();
                if(!isset($param['code'])||!isset($param['signature'])||
                    !isset($param['encryptedData'])||!isset($param['iv'])||!isset($param['type'])){
                    $code = 10002;
                    throw new Exception('缺少参数');
                }

                //从微信接口获取openid session_key
                $url = "https://api.weixin.qq.com/sns/jscode2session?"
                    ."appid={$this->appId}&secret={$this->secret}&js_code={$param['code']}&grant_type=authorization_code";
                $response = json_decode(http_get($url));
                if(isset($response->errcode)&&$response->errcode!=0){
                    $code = 10003;
                    throw new Exception('登录失败');
                }else{
                    //获取用户信息，此处用到解密方法
                    $pc=new \weChatLogin\WXBizDataCrypt($this->appId,$response->session_key);
                    $errCode = $pc->decryptData($param['encryptedData'], $param['iv'], $userInfo );
                    if($errCode!==0){
                        $code = 10004;
                        throw new Exception('解密用户数据失败');
                    }
                    //记录openID sessionKey 生成加密token返回
                    $user = Db::name('微信用户表')
                        ->where([
                            'openId'=>$response->openid
                        ])
                        ->find();
                    if($user){
                        if($param['type'] === 'expire'){
                            //session_key过期了 需要更新session_key
                            $update = [
                                'sessionKey'=>$response->session_key,
                            ];
                            Db::name('微信用户表')
                                ->where([
                                    'openId'=>$response->openid,
                                ])
                                ->update($update);
                        }
                        $token= $user['token'];
                    }else{
                        $userInfo = json_decode($userInfo);
                        $insert = [
                            'nickName'=>base64_encode($userInfo->nickName),
                            'city'=>$userInfo->city,
                            'province'=>$userInfo->province,
                            'country'=>$userInfo->country,
                            'avatarUrl'=>$userInfo->avatarUrl,
                            'openId'=>$response->openid,
                            'sessionKey'=>$response->session_key,
                        ];
                        $userId = Db::name('微信用户表')
                            ->insertGetId($insert);
                        $token = $this->refreshUserToken($userId);
                    }
                    $data = $token;
                }

            }else{
                $code = 10001;
                throw new Exception('请求失败');
            }

        }catch (Exception $e){
            $msg = $e->getMessage();
        }
        return json(['data'=>$data,'msg'=>$msg,'code'=>$code]);
    }

    public static function refreshUserToken($userId){
        if(!$userId)
            return false;
        //生成token
        $rand_num = rand(10,99999);//随机数
        $time = time();//时间戳
        $token =  md5($userId.$rand_num.$time);
        $expire_time = time() + 600;//过期时间为10分钟
        //更新当前用户token 和 有效期
        $update = [
            'token'=>$token,
            'expire_time'=>$expire_time,
        ];
        $update_res = Db::name('微信用户表')->where(['id'=>$userId])->update($update);
        if(!$update_res)
            return false;
        else
            return $token;
    }
}
```

* * *

# 软件
## vscode
### 快捷键
<p align="left" style="color:#777777;">发布日期：2021-01-26</p>

![calc](./images/vscode_keyboard_shortcuts.png ':size=100%')  
[英文原版pdf](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)