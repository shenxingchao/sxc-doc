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
    .class{
        width:calc(100%-10rpx)
    }
</style>
```
这样就达到了我们想要的效果

# Javascript
## 方法
### 数组按某个值排序
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
## es6
### 数组去重
![calc](./images/es6_array_duplicate.png ':size=30%')
- ##### 普通数组去重
```Javascript
<script>
    let arr = [1, 2, 3, 2, 1];
    let temp = new Set(arr);
    console.log([...temp]); //输出[1, 2, 3]
</script>
```

- ##### 对象数组去重 某个值
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


- ##### 对象数组去重 整个对象
```Javascript
<script>
    temp = new Map();
    newArr = arr.filter((item,key)=> 
        !temp.has(JSON.stringify(item)) && temp.set(JSON.stringify(item),true)
    );
    console.log(newArr); //输出{name: 1, value: 2}{name: 2, value: 3}{name: 4, value: 3}
</script>
```

# Linux
## 常用命令
### 创建多级目录
mkdir -p
## vim命令
## svn服务器搭建
### centos7.2 svn服务器搭建
![calc](./images/svn.png ':size=30%')

##### 1. putty登陆服务器

##### 2. 安装svn服务端
    yum -y install subversion

##### 3. 查看安装位置
    rpm -ql subversion

##### 4. 查看版本
    svnserve --version

##### 5. *创建svn版本库目录
    mkdir -p /var/svn/sxc

##### 6. *创建版本库
    svnadmin create /var/svn/sxc

##### 7. *进入版本库
    cd /var/svn/sxc

##### 8. 查看列表
    ls -l
    
##### 9. 进入
    cd /var/svn/sxc/conf  
有三个文件 
1. authz权限管理 
2. passwd账号密码管理 
3. svnserve.conf配置文件
   
##### 10. 添加账号密码
    vi passwd  
1. 末尾加上用户名=密码（i编辑）
2. 退出并保存esc+:wq
   
##### 11. 修改权限
    vi authz 
- 末尾加上
- [\]
- 用户 = rw
- 表示用户有读写权限，修改完并保存
  
##### 12. 修改配置文件
    vim /var/svn/sxc/conf/svnserve.conf
- 取消以下注释  并修改realm为你的svn版本库目录
- anon-access = read  #匿名用户可读
- auth-access = write #授权用户可写
- password-db = passwd #SVN账号文件
- authz-db = authz  #SVN权限文件
- realm = /var/svn/sxc  #SVN版本库目录
- 这里会出现一个配置的问题，配置项需要顶格写
  
##### 12. 启动svn版本库
    svnserve -d -r /var/svn/
- 若无法启动，说明svn服务已经被占用，
- 执行 killall svnserve 关掉当前正在执行的SVN进程后即可。
  
##### 13. 查看svn是否启动
    ps aux | grep svn

##### 14. 防火墙开启3690端口
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

##### 15. 无法连接主机“X.X.X.X”: 由于连接方在一段时间后没有正确答复或连接的主机没有反应，连接尝试失败。
- 需要配置安全组规则
- 入站规则 协议 0.0.0.0/0 所有来源可访问 tcp:3690  策略:允许
  
##### 16. 拓展，多个库使用一个配置文件
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

##### 17. *启动svn
    killall svnserve
    svnserve -d -r /var/svn --config-file=/var/svn/conf/svnserve.conf （多个库访问一套配置文件 需要制定   配置文件的位置 所以使用这个代码启动服务）
    ps aux | grep svn

##### 18. *checkout
    svn://ip/sxc
##### 19. 下次建版本库只要执行带*号的就可以了

##### 20. 服务器web目录检出空目录（空版本库）
```
cd /usr/local/nginx/html/sxc
svn checkout svn://x.x.x.x/sxc(目录或文件的全路径)　./(当前web目录路径])
svn add *
svn add . --no-ignore --force (全部包括隐藏文件)
svn commit -m "sxc"
svn update
```
