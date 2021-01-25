# :id=notebook

![https://api.xhboke.com/news/](https://api.xhboke.com/news/)

* * *

# 常用网址
| 主题                                                                                                                                |    标签    |   更改日期    |
| :---------------------------------------------------------------------------------------------------------------------------------- | :--------: | :-----------: |
| [free api test 苍穹精华 API](https://api.xhboke.com/doc/)                                                                  |  Markdown  | 2021年1月25日 |
| [docsify一个神奇的文档网站生成器](https://docsify.js.org/#/zh-cn/)                                                                  |  Markdown  | 2021年1月25日 |
| [mockjs在线格式编辑](http://mockjs.com/0.1/editor.html#help)                                                                        |  Mock.js   | 2021年1月25日 |
| [SortableJS功能强大的JavaScript 拖拽库](http://www.sortablejs.com)                                                                  |  js 插件   | 2021年1月25日 |
| [Vuex Vue.js状态管理](https://vuex.vuejs.org/zh/guide/)                                                                             |    vue     | 2021年1月25日 |
| [Element，一套为开发者、设计师和产品经理准备的基于 Vue 2.0 的桌面端组件库](https://element.eleme.cn/#/zh-CN/component/installation) | vue ui框架 | 2021年1月25日 |
| [uni-app一个使用 Vue.js 开发所有前端应用的框架](https://uniapp.dcloud.net.cn/quickstart-cli)                                        |  vue app   | 2021年1月25日 |
| [vue-gaode在Vue中完美的使用高德地图](http://vue-gaode.rxshc.com/)                                                                   |  vue 地图  | 2021年1月25日 |

* * *

# Css
## less
### VSCODE EASY WXLESS 插件编译问题之calc
<p align="right" style="color:#777777;">发布日期：2019-04-09</p>

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
    .class{
        width:calc(100%-10rpx)
    }
</style>
```
这样就达到了我们想要的效果

* * *

# Javascript
## 方法
### 数组按某个值排序
<p align="right" style="color:#777777;">发布日期：2021-01-22</p>

?>按order值的大小，对数组List进行升序排序
```Javascript
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
<p align="right" style="color:#777777;">发布日期：2021-01-22</p>

?>就是以对象的某个属性作为索引值key 变成一个关联数组，然后再用Object.keys 循环关联数组赋给新数组，以去掉索引key
```Javascript
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
## typescript

* * *

## es6
### 数组去重
<p align="right" style="color:#777777;">发布日期：2020-11-13</p>

![calc](./images/es6_array_duplicate.png ':size=30%')
##### 普通数组去重
```Javascript
<script>
    let arr = [1, 2, 3, 2, 1];
    let temp = new Set(arr);
    console.log([...temp]); //输出[1, 2, 3]
</script>
```

##### 对象数组去重 某个值
__1. 数组方法__
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
__2. set方法__
```Javascript
<script>
    temp = new Set();
    newArr = arr.filter(
    (item) => !temp.has(item.value) && temp.add(item.value)
    );
    console.log(newArr); //输出{name: 1, value: 2}{name: 2, value: 3}
</script>
```
__3. map方法__
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


##### 对象数组去重 整个对象
```Javascript
<script>
    temp = new Map();
    newArr = arr.filter((item,key)=> 
        !temp.has(JSON.stringify(item)) && temp.set(JSON.stringify(item),true)
    );
    console.log(newArr); //输出{name: 1, value: 2}{name: 2, value: 3}{name: 4, value: 3}
</script>
```

* * *

# Linux
## 常用命令
### 创建多级目录
mkdir -p
## vim命令

* * *

# 版本管理工具
## svn服务器搭建
### centos7.2 svn服务器搭建
<p align="right" style="color:#777777;">发布日期：2020-02-25</p>

![calc](./images/svn.png ':size=30%')

##### 1.putty登陆服务器

##### 2.安装svn服务端
    yum -y install subversion

##### 3.查看安装位置
    rpm -ql subversion

##### 4.查看版本
    svnserve --version

##### 5.*创建svn版本库目录
    mkdir -p /var/svn/sxc

##### 6.*创建版本库
    svnadmin create /var/svn/sxc

##### 7.*进入版本库
    cd /var/svn/sxc

##### 8.查看列表
    ls -l
    
##### 9.进入
    cd /var/svn/sxc/conf  
有三个文件 
1. authz权限管理 
2. passwd账号密码管理 
3. svnserve.conf配置文件
   
##### 10.添加账号密码
    vi passwd  
1. 末尾加上用户名=密码（i编辑）
2. 退出并保存esc+:wq
   
##### 11.修改权限
    vi authz 
- 末尾加上
- [\]
- 用户 = rw
- 表示用户有读写权限，修改完并保存
  
##### 12.修改配置文件
    vim /var/svn/sxc/conf/svnserve.conf
- 取消以下注释  并修改realm为你的svn版本库目录
- anon-access = read  #匿名用户可读
- auth-access = write #授权用户可写
- password-db = passwd #SVN账号文件
- authz-db = authz  #SVN权限文件
- realm = /var/svn/sxc  #SVN版本库目录
- 这里会出现一个配置的问题，配置项需要顶格写
  
##### 12.启动svn版本库
    svnserve -d -r /var/svn/
- 若无法启动，说明svn服务已经被占用，
- 执行 killall svnserve 关掉当前正在执行的SVN进程后即可。
  
##### 13.查看svn是否启动
    ps aux | grep svn

##### 14.防火墙开启3690端口
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

##### 15.无法连接主机“X.X.X.X”: 由于连接方在一段时间后没有正确答复或连接的主机没有反应，连接尝试失败。
- 需要配置安全组规则
- 入站规则 协议 0.0.0.0/0 所有来源可访问 tcp:3690  策略:允许
  
##### 16.拓展，多个库使用一个配置文件
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

##### 17.*启动svn
    killall svnserve
    svnserve -d -r /var/svn --config-file=/var/svn/conf/svnserve.conf （多个库访问一套配置文件 需要制定   配置文件的位置 所以使用这个代码启动服务）
    ps aux | grep svn

##### 18.*checkout
    svn://ip/sxc
##### 19.下次建版本库只要执行带*号的就可以了

##### 20.服务器web目录检出空目录（空版本库）
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
<p align="right" style="color:#777777;">发布日期：2020-07-24</p>

![calc](./images/git.jpg ':size=30%')

?> [SSH公钥配置](https://www.jianshu.com/p/464a3373d15c)

###### 1.下载git客户端 官网下载即可
###### 2.打开命令行 配置全局用户名邮箱
	$ git config --global user.name "Your Name"
	$ git config --global user.email "email@example.com"
###### 3.初始化git仓库
	$ git init
###### 4.告诉git添加单个文件到仓库(可以反复添加，一次提交即可，就是添加到暂存区)
	$ git add test.html
###### 5.单个文件提交到仓库（就是提交到当前分支）
	$ git commit -m "我添加了第一个文件test.html"
###### 6.查看仓库状态，修改了或者新增了什么文件
	$ git status
###### 7.查看修改了哪些内容
	$ git diff
###### 8.提交修改的文件是重复4，5这两步
	$ git add test.html
	$ git commit -m "我修改了test.html"
###### 9.查看提交日志
	$ git log
###### 10.查看简略提交日志
	$ git log --pretty=oneline
###### 11.版本回退到上一版本
	$ git reset --hard HEAD^
###### 12.版本恢复到回退前的版本
	$ git reset --hard 1234a (就是commit id，前几位就可以)
###### 13.查看命令行历史，可用于忘记commit id，去回退版本
	$ git reflog
###### 14.查看工作区和版本库最新版本的区别
	$ git diff HEAD -- test.html(查看指定文件)
###### 15.撤销修改 恢复到最近一次git commit或git add的状态 丢弃工作区的修改
	$ git checkout -- test.html
###### 16.暂存区修改回退到工作区
	$ git reset HEAD  test.html
###### 17.删除文件
	$ git rm test.html
	$ git commit -m "delete test.html"
###### 18.如果没有提交删除文件，只提交到暂存区（就是只执行了git rm操作），那么回退，提交了就只能回退版本
	$ git reset HEAD test.html
	$ git checkout --test.html
###### 19.本地仓库关联远程仓库
	$ git remote add origin git@github.com:xxx/demo.git
###### 20.把当前master分支推送到远程仓库
	$ git push -u origin master
###### 21.SSH key获取方法
	$ ssh-keygen -t rsa -C  "email@example.com"
	$ cat ~/.ssh/id_rsa.pub
###### 22.克隆远程仓库到本地,直接文件夹下来
	$ git clone git@github.com:xxx/demo.git
###### 23.创建dev分支
	$ git branch dev
###### 24.切换到dev分支
	$ git checkout dev 或 $ git switch dev
###### 25.创建dev分支并切换到该分支
	$ git checkout -b dev 或 $ git switch -c dev
###### 26.查看当前分支
	$ git branch
###### 27.合并dev分支到master分支上，这种合并没有历史提交记录
	$ git merge dev （合并指定分支到当前分支）
###### 28.删除dev分支
	$ git branch -d dev
###### 29.合并分支分支历史记录分支信息,合并会创建一个新的commit
	$ git merge --no-ff -m "merge with no-ff" dev
###### 30.存储当前工作区（还未提交）
	$ git stash
###### 31.查看存储的工作区列表
	$ git stash list
###### 32.恢复存储的工作区
	$ git stash apply
###### 33.删除存储的工作区
	$ git stash drop
###### 34.恢复并删除存储的工作区
	$ git stash pop
###### 35.恢复指定存储的工作区
	$ git stash pop stash@{0}
###### 36.修复某个分支的BUG 并把该BUG提交复制到当前分支,假如当前分支是dev，bug修复在master分支
	用法
	$ git cherry-pick 100aaa
	流程
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
	总结：修复bug时，先保存当前工作区，创建新的bug分支进行修复，然后合并，最后删除，然后在恢复工作区
###### 37.强制删除没有合并过的分支
	$ git branch -D feature
###### 38.新功能使用feature分支
###### 39.创建远程分支到本地
	$ git checkout -b dev origin/dev
###### 40.指定分支拉取最新远程仓库代码
	$ git checkout dev
	$ git pull
###### 41.git pull拉取失败需要先指定本地dev分支与远程分支之间的连接
	$ git branch --set-upstream-to=origin/dev dev  (远程分支名称，本地分支名称)
###### 42.查看分支的合并情况
	$ git log --graph --pretty=oneline --abbrev-commit
###### 43.把分叉的提交历史 整理成一条直线
	$ git rebase
###### 44.当前分支加标签
	$ git tag v1.0 aaa100（默认是打在最新的commit上，要打以前的加commit id）
###### 45.查看标签
	$ git tag
###### 46.查看标签信息
	$ git show v1.0
###### 47.删除标签
	$ git tag -d v1.0
###### 48.标签提交到远程仓库
	$ git push origin v1.0
###### 49.所有标签提交到远程仓库
	$ git push origin --tags
###### 50.删除远程标签，先删除本地的
	$ git tag -d v1.0
	$ git push origin :refs/tags/v1.0
###### 51.拉去远程主分支
	step3
	step19
	$ git pull origin master
###### 52.提交时不改变文件的crlf
	$ git config --globa core.autocrlf false
###### 53.提交分支到远程
    $ git push --set-upstream origin dev
###### 54.撤销更改
    $ git checkout .
###### 55.解决冲突
    解决后add ,commit 后再合并分支
###### 56.提交后修改提交的注释文字-m
    $ git commit --amend
    $ git push -f origin master