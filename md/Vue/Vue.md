# Vue

# 一些技巧

## vue强制刷新子组件

[转](https://www.cnblogs.com/betty-niu/p/11199082.html)  
父组件中利用v-if 强制刷新子组件

```html
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

## 如何使用keepAlive组件

vux 管理一个cachedViews 路由名称的缓存数组就可以了

```html
<keep-alive :include="cachedViews">
   <router-view />
</keep-alive>
<script>
  computed: {
    cachedViews() {
      let white_list: any = []
      let that = this as any
      that.$router.options.routes.forEach((element: any) => {
        if (element.meta && element.meta.keepAlive) {
          white_list.push(element.name)
        }
      })
      let cached: any = []
      white_list.forEach((element: any) => {
        if (this.$store.state.cachedViews.includes(element)) {
          cached.push(element)
        }
      })
      return cached
    }
  }
</script>
```

千万别用下面这种，误入歧途  
这种方式会引起异常情况和beforeRouteLeave方法配合动态改变keepAlive，第一次执行正常，第二次及之后组件会一直是keepAlive=false

```html
<keep-alive>
  <router-view v-if="$route.meta.keepAlive"></router-view>
</keep-alive>
<router-view v-if="!$route.meta.keepAlive"></router-view>
```

# 组件

## vue2/vue3 svg组件

<p align="left" style="color:#777777;">发布日期：2021-03-08</p>

1. 安装
   
   ```
   yarn add svg-sprite-loader -D
   ```

2. 配置vue.config.js
   
   ```javascript
   module.exports = {
    chainWebpack: config => {
      // svg 配置 开始
      const svgRule = config.module.rule('svg') // 找到svg-loader
      svgRule.uses.clear() // 清除已有的loader, 如果不这样做会添加在此loader之后
      svgRule.exclude.add(/node_modules/) // 正则匹配排除node_modules目录
      svgRule // 添加svg新的loader处理
        .test(/\.svg$/)
        .use('svg-sprite-loader')
        .loader('svg-sprite-loader')
        .options({
          symbolId: 'icon-[name]'
        })
        .end()
      // svg配置结束
    },
   }
   ```

3. 组件
   
   - SvgIonc.vue
     
     ```typescript
     <template>
      <svg :class="'svg-icon ' + props.className" aria-hidden="true">
        <use :xlink:href="`#icon-${props.name}`" />
      </svg>
     </template>
     <script lang="ts">
     import { defineComponent } from 'vue'
     
     export default defineComponent({
      name: 'SvgIcon',
      props: {
        className: {
          type: String,
          default: '',
        },
        name: {
          type: String,
          required: true,
          default: '',
        },
      },
      setup(props) {
        return { props }
      },
     })
     </script>
     ```
   
   - 编写svg插件Index.ts
     
    ```typescript
    import SvgIcon from './SvgIcon.vue'
   
    const componentPlugin: any = {
      install: function (vue: any, options: any) {
        if (
    
          options &&
          options.imports &&
          Array.isArray(options.imports) &&
          options.imports.length > 0
    
        ) {
    
          // 按需引入图标
          const { imports } = options
          imports.forEach((name: any) => {
            require(`@/assets/svg/${name}.svg`)
          })
    
        } else {
    
          // 全量引入图标
          const ctx = require.context('@/assets/svg', false, /\.svg$/)
          ctx.keys().forEach(path => {
            const temp = path.match(/\.\/([A-Za-z0-9\-_]+)\.svg$/)
            if (!temp) return
            const name = temp[1]
            require(`@/assets/svg/${name}.svg`)
          })
    
        }
        vue.component(SvgIcon.name, SvgIcon)
      }
    }
    export default componentPlugin
    ```

4. 使用插件
   main.ts使用
   
   ```typescript
   import SvgPlugin from '@/components/SvgIcon'
   app.use(SvgPlugin, {
    imports: []
   })
   ```

5. 使用
   src/assets/下创建svg文件夹  
   
   ```typescript
   <svg-icon name="mini" className="icon"/>
   ```

# Vue3

## vue3基础

<p align="left" style="color:#777777;">发布日期：2021-03-07 更新日期：2021-03-08</p>

## Composition API

- setup()函数
  
  - 在beforeCreate之前调用，所有变量、方法都在setup函数中定义，最后return出去给模板使用
  - 该函数有2个参数：  
    props 属性  
    context 上下文对象  
    其中context具有属性（attrs，slots，emit，parent，root），其对应于vue2中的this.$attrs，this.$slots，this.$emit，this.$parent，this.$root。

- ref 和 reactive
  
  - ref和reactvie的数据都是响应式的
  - 基本类型值（String 、Nmuber 、Boolean 等）或单值对象（类似像 {count: 3} 这样只有一个属性值的对象）使用ref
  - 引用类型值（Object 、Array）复杂数据类型使用reactive

- toRef和ref  
  toRef 是将某个对象中的某个值转化为响应式数据，其接收两个参数，第一个参数为 obj 对象；第二个参数为对象中的属性名
  
  - ref 是对原数据的拷贝，响应式数据对象值改变后会同步更新视图，不会影响到原始值
  
  - toRef 是对原数据的引用，响应式数据对象值改变后不会改变视图，会影响到原始值。
  
  - 用法
    
    ```javascript
    const obj = {count: 3}
    const state1 = ref(obj.count) //对象.属性
    const state2 = toRef(obj, 'count') //对象，属性
    ```

- toRefs
  将传入的对象里所有的属性的值都转化为响应式数据对象(ref) 解构返回  
  ..toRefs(data) 就是把data对象解构成逐个变量  然后每个变量都是响应式了  
  
  ```javascript
  <template>
    {{str}}
  </template>
  <script>
  import {toRefs,reactive} from 'vue'
  export default {
      setup() {
        const state = reactive({
          str: '响应式数据',
        });
        return {...toRefs(state)}
      }
  }
  </script>
  ```

!> reactive如果不用toRefs转就不能响应式，会导致数据更改画面不更新

## vue3-vite-electron

<p align="left" style="color:#777777;">发布日期：2021-03-04</p>

[官方推荐github模板地址](https://github.com/vitejs/awesome-vite#templates)  
[vite推荐社区模板](https://cn.vitejs.dev/guide/#%E7%A4%BE%E5%8C%BA%E6%A8%A1%E6%9D%BF)

!> nodejs版本 > 16.3.2

[快速开始](https://www.electronjs.org/zh/docs/latest/tutorial/quick-start)

[参考搭建](https://dev.to/brojenuel/vite-vue-3-electron-5h4o)

注意运行electron需要等待vite运行完毕，不然白屏

**创建vite app**

```
yarn create @vitejs/app  project-name --template vue-ts
```

**安装依赖**

```
cd project-name
yarn
```

**启动**

```
yarn dev
```

**安装electron**

```
yarn add --dev electron
```

**依次安装依赖**

```
yarn add -D concurrently cross-env electron electron-builder wait-on
```

concurrently：阻塞运行多个命令，`-k`参数用来清除其它已经存在或者挂掉的进程

cross-env：跨环境设置环境变量包

wait-on：等待资源，此处用来等待url可访问

**src文件夹下依次建立文件background.ts 和 preload.js**

background.ts

```ts
//主进程
const { app, BrowserWindow } = require("electron");
const path = require("path");
const loadUrl =
  process.env.NODE_ENV == "development"
    ? "http://localhost:3000"
    : `file://${path.join(__dirname, "../dist/index.html")}`;

function createWindow() {
  const win = new BrowserWindow({
    width: 800,
    height: 600,
    webPreferences: {
      preload: path.join(__dirname, "preload.js"),
    },
  });

  win.loadURL(loadUrl);
}

app.whenReady().then(() => {
  createWindow();

  app.on("activate", () => {
    if (BrowserWindow.getAllWindows().length === 0) {
      createWindow();
    }
  });
});

app.on("window-all-closed", () => {
  if (process.platform !== "darwin") {
    app.quit();
  }
});
```

preload.js

```javascript
window.addEventListener('DOMContentLoaded', () => {
    const replaceText = (selector, text) => {
      const element = document.getElementById(selector)
      if (element) element.innerText = text
    }

    for (const type of ['chrome', 'node', 'electron']) {
      replaceText(`${type}-version`, process.versions[type])
    }
  })
```

**配置文件vite.config.ts和package.json**

```ts
import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'

// https://vitejs.dev/config/
export default defineConfig({
  base: './',//新增
  plugins: [vue()],
})
```

```json
{
  "name": "vue3-electron17-template",
  "private": true,
  "version": "0.0.0",
  "main": "src/background.ts",
  "scripts": {
    "dev": "vite",
    "build": "vue-tsc --noEmit && vite build",
    "preview": "vite preview",
    "electron": "wait-on tcp:3000 && cross-env NODE_ENV=development electron .",
    "electron:serve": "concurrently -k \"yarn dev\"  \"yarn electron\"",
    "electron:builder": "electron-builder",
    "electron:build": "yarn build && yarn electron:builder"
  },
  "dependencies": {
    "vue": "^3.2.25"
  },
  "devDependencies": {
    "@vitejs/plugin-vue": "^2.2.0",
    "concurrently": "^7.0.0",
    "cross-env": "^7.0.3",
    "electron": "^17.2.0",
    "electron-builder": "^22.14.13",
    "typescript": "^4.5.4",
    "vite": "^2.8.0",
    "vue-tsc": "^0.29.8",
    "wait-on": "^6.0.1"
  },
  "build": {
    "appId": "com.org.vue3-electron17-template",
    "productName": "vue3-electron17-template",
    "copyright": "Copyright © sxc",
    "directories": {
      "buildResources": "./assets_electron",
      "output": "./dist_electron"
    },
    "win": {
      "target": [
        "nsis"
      ],
      "icon": "./public/favicon.ico"
    },
    "nsis": {
      "oneClick": false,
      "allowToChangeInstallationDirectory": true,
      "perMachine": true,
      "allowElevation": true,
      "installerIcon": "./public/favicon.ico",
      "uninstallerIcon": "./public/favicon.ico",
      "installerHeaderIcon": "./public/favicon.ico",
      "deleteAppDataOnUninstall": true,
      "createDesktopShortcut": true,
      "createStartMenuShortcut": false,
      "shortcutName": "vue3-electron17-template"
    },
    "publish": [
      {
        "provider": "generic",
        "url": "http://xxx.xxx.com/download/"
      }
    ]
  }
}
```

至此搭建完成

运行yarn electron:serve 或者 yarn electron:build查看效果

其他问题参考下面的vue3-vue-cli-electron中的bug解决方案即可解决

模板尚未完成

## vue3-vue-cli-electron

<p align="left" style="color:#777777;">发布日期：2021-03-04 更新日期：2021-03-22</p>

有成熟工具 [我的模板](https://github.com/shenxingchao/vue3-electron13-template)

- 创建
  
  ```shell
  vue create project-name
  ```
  
  ```shell
  1.手动选择需要添加的包Manually select features
    Choose Vue version # 选择Vue版本
    TypeScript # 使用TypeScript
    Router # 路由
    Vuex # 状态管理
    CSS Pre-processors # 使用css预处理Sass
  2.选择Vue版本（Choose a version of Vue.js）
    3.x(Preview)
  3.使用class风格组件（Use class-style component syntax）
    No
  4.使用（Jsx Use Babel alongside TypeScript）
    No
  5.使用history路由（Use history mode for router）
    No
  6.选择css预处理（Pick a CSS pre-processor）
    Sass/SCSS (with dart-sass)
  7.配置文件放置位置（Where do you prefer placing config）
    In dedicated config files
  ```

- 添加electron builder
  
  ```shell
  cd project-name
  vue add electron-builder
  ```
  
  ```shell
  选择Electron版本 Choose Electron Version
    ^13.0.0 √
  ```

- 启动
  
  ```
  yarn electron:serve
  ```
  
  !> 这时候运行会出问题 运行超时 vue devtool  vue electron Failed to fetch extension, trying 4 more times 解决方法：注释VUEJS_DEVTOOLS相关内容

- 打包
  
  ```
  yarn electron:build
  ```

- 打包配置 vue.config.js
  
  ```javascript
  module.exports = {
    pluginOptions: {
      electronBuilder: {
        nodeIntegration: true, //配置防止浏览器报错'__dirname' is not defind
        builderOptions: {
          appId: 'com.sxc.code-auto-tool', //appId
          productName: 'code-auto-tool', //安装目录名称
          copyright: 'Copyright © sxc', //版权
          directories: {
            buildResources: 'build', //打包时的资源文件夹
            output: './dist' //打包文件输出路径
          },
          win: {
            //windows平台配置
            target: [
              //打包类型
              'nsis' //打包为nsis安装文件
            ],
            icon: './public/favicon.ico' //app图标
          },
          nsis: {
            //nsis配置参数
            oneClick: false, //单击安装
            allowToChangeInstallationDirectory: true, //允许用户选择安装位置
            perMachine: true, //显示是否为所有用户安装程序
            allowElevation: true, //运行提升为管理员权限
            installerIcon: './public/favicon.ico', //安装图标
            uninstallerIcon: './public/favicon.ico', //卸载图标
            installerHeaderIcon: './public/favicon.ico', //安装头部图标
            deleteAppDataOnUninstall: false, //卸载时是否清除数据
            createDesktopShortcut: true, //创建桌面图标
            createStartMenuShortcut: false, //创建开始菜单图标
            shortcutName: '代码自动生成工具' //快捷图标名称
          },
          publish: [
            //更新参数 http服务器方式 其他方式见#https://www.electron.build/configuration/publish
            {
              provider: 'generic',
              url: 'http://demo.o8o8o8.com/download/' //存放软件版本的地址 自动更新用
            }
          ]
        }
      }
    }
  }
  ```

[electron打包配置](https://www.electron.build/configuration/configuration)

- 添加自动更新
  
  ```
  yarn add electron-updater
  ```

- 自动更新部分代码 background.ts
  
  ```javascript
  import {
    dialog,
    MessageBoxReturnValue,
  } from 'electron'
  
  if (!isDevelopment) {
    autoUpdater.checkForUpdates()
    autoUpdater.on('update-downloaded', () => {
      dialog
        .showMessageBox({
          type: 'info',
          title: '提示',
          message: '有新版本已经下载完毕，是否立即安装？',
          buttons: ['ok', 'cancel']
        })
        .then((res: MessageBoxReturnValue) => {
          if (res.response == 0) {
            //下载完成后执行 quitAndInstall
            autoUpdater.quitAndInstall() //关闭软件并安装新版本
          } else {
            //关闭应用程序时安装
          }
        })
    })
  }
  ```

- [解决第一次显示画面闪烁问题](https://www.electronjs.org/docs/api/browser-window#%E4%BD%BF%E7%94%A8ready-to-show%E4%BA%8B%E4%BB%B6)
  electron13+ ready to show 方法无效 去掉这个方法直接在loadUrl后面显示就行了

- 使用预加载 preload.js 解决nodeapi使用问题,官方推荐
  主进程main.ts
  
  ```typescript
  const path = require('path')
  
  webPreferences: {
    nodeIntegration: true,
    preload: path.join(__dirname, 'preload.js'),
    contextIsolation: false
  }
  ```
  
  preload.js 需要放在dist_electron才生效！！！
  
  ```javascript
  const { ipcRenderer } = require('electron')
  window.ipcRenderer = ipcRenderer
  ```
  
  使用
  
  ```typescript
  (window as any).ipcRenderer.send
  ```

- electron version 12+,vue项目使用ts,不使用preload 解决nodeapi使用问题 nodeIntegration设置为true
  
  ```typescript
  webPreferences: {
    nodeIntegration: true,
    contextIsolation: false
  }
  ```

- [The remote module is deprecated解决方法](https://github.com/electron/remote) 

- electron version 12+,vue项目使用js,报fs.existsSync is not a function,官方不推荐的解决方法
  background.js
  
  ```javascript
  webPreferences: {
    nodeIntegration: true,
    nodeIntegrationInWorker: true,
    contextIsolation: false
  }
  ```
  
  vue.config.js
  
  ```javascript
  module.exports = {
    pluginOptions: {
      electronBuilder: {
        nodeIntegration: true
      }
    }
    //...
  }
  ```

!> 解决build失败,could not find:messages.nsh
node_module/app-builder-lib/out/targets/nsis/NsisTarget.js 文件里设一行编码

```javascript
async executeMakensis(defines, commands, script) {
const args = this.options.warningsAsErrors === false ? [] : ["-WX"];
//此处新增
args.push("-INPUTCHARSET", "UTF8");
//结束
for (const name of Object.keys(defines)) {
  const value = defines[name];

  if (value == null) {
    args.push(`-D${name}`);
  } else {
    args.push(`-D${name}=${value}`);
  }
}
```

- [asar解压工具](https://www.npmjs.com/package/asar)

# vite

## 创建

```shell
yarn create vite
?Project name: app
?Select a framework:vue
?Select a variant:vue (js)
yarn
yarn dev
```