
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

## 组件
### vue2/vue3 svg组件
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

## Vue3
### vue3基础
<p align="left" style="color:#777777;">发布日期：2021-03-07 更新日期：2021-03-08</p>

#### Composition API
- setup()函数
  - 在beforeCreate之前调用，所有变量、方法都在setup函数中定义，最后return出去给模板使用
  - 该函数有2个参数：
    props
    context
    其中context是一个上下文对象，具有属性（attrs，slots，emit，parent，root），其对应于vue2中的this.$attrs，this.$slots，this.$emit，this.$parent，this.$root。

- ref 和 reactive
  - ref和reactvie的数据都是响应式的，但是如果需要对整个对象进行重新赋值, 那么用ref，如果只是改变属性，用reactive
  - 基本类型值（String 、Nmuber 、Boolean 等）或单值对象（类似像 {count: 3} 这样只有一个属性值的对象）使用 ref
  - 引用类型值（Object 、Array）使用 reactive
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
  将传入的对象里所有的属性的值都转化为响应式数据对象(ref)
  解构返回
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

### vue3-vite-electron
<p align="left" style="color:#777777;">发布日期：2021-03-04</p>

待成熟再用，现在没啥好的工具

[官方推荐github模板地址](https://github.com/vitejs/awesome-vite#templates)  
[vite推荐社区模板](https://cn.vitejs.dev/guide/#%E7%A4%BE%E5%8C%BA%E6%A8%A1%E6%9D%BF)

!> nodejs版本 > 12.0

- 创建
  ```
  yarn create @vitejs/app  project-name --template vue-ts
  ```
- 安装依赖
  ```
  cd project-name
  yarn
  ```
- 启动
  ```
  yarn dev
  ```

### vue3-vue-cli-electron 
<p align="left" style="color:#777777;">发布日期：2021-03-04 更新日期：2021-03-22</p>

有成熟工具

- 创建
  ```
  vue create project-name
  ```
- 添加electron builder
  ```
  cd project-name
  vue add electron-builder
  ```
- 启动
  ```
  yarn electron:dev
  ```
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
- 解决运行超时 vue devtool  vue electron Failed to fetch extension, trying 4 more times
  注释VUEJS_DEVTOOLS相关内容

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
- The remote module is deprecated https://github.com/electron/remote
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