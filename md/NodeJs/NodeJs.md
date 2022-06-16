# NodeJs
# node
## node安装与使用
- 安装
  https://nodejs.org/zh-cn/download/直接下载
- 更新
  ```
  npm config ls
  ```
  找到安装路径node bin location
  新的安装路径覆盖就的就可以了

#  node_modules
## 使用patch-package修改Node.js依赖包内容
- 安装patch-package
    ```
    yarn add --dev patch-package postinstall-postinstall
    ```
- 创建补丁
    ```
    yarn patch-package package-name  # 使用yarn
    ```
- 部署
    ```json
    "scripts": {
        "postinstall": "patch-package"
    }
    ```

# npm
## npm基本操作
- 设置镜像
  ```
    npm config set registry https://registry.npm.taobao.org
    npm config set disturl https://npm.taobao.org/dist
    npm config set electron_mirror https://npm.taobao.org/mirrors/electron/
    npm config set sass_binary_site https://npm.taobao.org/mirrors/node-sass/
    npm config set phantomjs_cdnurl https://npm.taobao.org/mirrors/phantomjs/
    npm查看当前地址源：npm get registry
  ```

# yarn
## yarn基本操作
- 安装
  ```
  npm install -g yarn
  yarn --version
  ```
- 更新包
  ```
  yarn upgrade-interactive --latest
  // 需要手动选择升级的依赖包，按空格键选择，a 键切换所有，i 键反选选择
  ```
- 移除包
  ```
  yarn remove <packageName>
  ```
- 设置镜像
  ```
    yarn config set registry https://registry.npm.taobao.org -g
    yarn config set disturl https://npm.taobao.org/dist -g
    yarn config set electron_mirror https://npm.taobao.org/mirrors/electron/ -g
    yarn config set sass_binary_site https://npm.taobao.org/mirrors/node-sass/ -g
    yarn config set phantomjs_cdnurl https://npm.taobao.org/mirrors/phantomjs/ -g
    yarn config set chromedriver_cdnurl https://cdn.npm.taobao.org/dist/chromedriver -g
    yarn config set operadriver_cdnurl https://cdn.npm.taobao.org/dist/operadriver -g
    yarn config set fse_binary_host_mirror https://npm.taobao.org/mirrors/fsevents -gyarn
    查看当前地址源：yarn config get registry
  ```
- yarn 超时
  删除yarn.lock文件 重新执行yarn

# express
## 使用express快速搭建本地测试服务器
1. 创建应用文件夹
2. 初始化
    ```powershell
    npm init
    ```
3. 安装express
    ```powershell
    yarn add express
    ```
    或者
    ```
    npm install express --save
    ```
4. 创建入口文件index.js
    ```javascript
    const express = require('express')
    const app = express()
    const port = 3000

    app.get('/', (req, res) => {
      res.send('Hello World!')
    })

    app.listen(port, () => {
      console.log(`Example app listening at http://localhost:${port}`)
    })
    ```
5. 配置运行命令
    package.json
    ```json
    {
      "scripts": {
        "dev":"node index.js"
      }
    }
    ```
6. 运行
    ```powershell
    yarn dev
    ```
    或者
    ```
    npm run dev
    ```
7. 访问html
    安装ejs
    ```powershell
    yarn add ejs
    ```
    或者
    ```powershell
    npm instal ejs --save
    ```
    在项目根目录新建views 放入index.html  
    然后修改index.js  
    ```javascript
    const express = require('express')
    const app = express()
    const port = 3001

    //设置模板文件目录
    app.set('views', __dirname + '/views');
    //设置视图引擎
    app.set( 'view engine', 'html' );
    //设置视图后缀 ejs模板库
    app.engine( '.html', require( 'ejs' ).__express );

    //路由
    app.get('/', (req, res) => {
      res.render('index')
    })

    //监听
    app.listen(port, () => {
      console.log(`Example app listening at http://localhost:${port}`)
    })
    ```