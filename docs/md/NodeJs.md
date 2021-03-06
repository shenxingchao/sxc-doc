# NodeJs
## node
### node安装与使用
- 安装
  https://nodejs.org/zh-cn/download/直接下载
- 更新
  ```
  npm config ls
  ```
  找到安装路径node bin location
  新的安装路径覆盖就的就可以了
##  node_modules
### 使用patch-package修改Node.js依赖包内容
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
## npm

### npm基本操作
- 设置镜像
  ```
    npm config set registry https://registry.npm.taobao.org
    npm config set disturl https://npm.taobao.org/dist
    npm config set electron_mirror https://npm.taobao.org/mirrors/electron/
    npm config set sass_binary_site https://npm.taobao.org/mirrors/node-sass/
    npm config set phantomjs_cdnurl https://npm.taobao.org/mirrors/phantomjs/
    npm查看当前地址源：npm get registry
  ```
## yarn

### yarn基本操作
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