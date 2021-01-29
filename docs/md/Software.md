# 软件
## vscode
### 快捷键
<p align="left" style="color:#777777;">发布日期：2021-01-26</p>

![calc](./images/vscode_keyboard_shortcuts.png ':size=100%')  
[英文原版pdf](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)
### 格式化
#### eslint+prettier+vetur 格式化vue项目
<p align="left" style="color:#777777;">发布日期：2021-01-28</p>

- 准备工作
    - 安装vscode扩展vetur,安装prettier
    - 安装node模块 
    ```npm
     npm -i eslint prettier  eslint-plugin-prettier eslint-config-prettier eslint-plugin-vue--save-dev
    ```

- vscode工作区配置文件  \\.vscode\settings.json
```json
{
  //需要安装vetur
  "vetur.format.defaultFormatter.html": "js-beautify-html", //html部分用这个插件
  "vetur.format.defaultFormatter.js": "prettier", //js部分用prettier
  "vetur.format.defaultFormatterOptions": {
    "js-beautify-html": {
      "indent_size": 2, //首行4个字符缩进
      "wrap_line_length": 120, //一行最多字符
      "wrap_attributes": "aligned-multiple", //属性强制换行 不对齐设置auto 强制对齐并换行force-aligned
      "end_with_newline": false //末尾是否添加新行
    },
    "prettier": {
      "semi": false, //不加分号
      "singleQuote": true //用单引号
    }
  },
  "[vue]": {
    "editor.defaultFormatter": "octref.vetur" //vue 文件默认格式化
  },
  "eslint.validate": [
    //eslint检查的语言
    "javascript",
    "html",
    "vue"
  ],
  "editor.formatOnSave": true, //保存时自动格式化
  "editor.codeActionsOnSave": {
    //保存时eslint自动修复
    "source.fixAll.eslint": true
  },
  //需要安装prettier
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "[json]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[javascript]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[scss]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "prettier.parser": "flow", //prettier格式化不能用时加
  "prettier.singleQuote": true, //强制添加单引号
  "prettier.semi": false, //末尾没有分号
  "prettier.arrowParens": "avoid", //  (x) => {} 箭头函数参数只有一个时是否要有小括号。avoid：省略括号
  "prettier.bracketSpacing": true, // 在对象，数组括号与文字之间加空格 "{ foo: bar }"
  "prettier.jsxBracketSameLine": false, // 在jsx中把'>' 单独放一行
  "prettier.trailingComma": "none",
  "prettier.disableLanguages": ["vue"] // 不格式化vue文件
}
```
- eslint规则配置文件 \.eslintrc.js
```javascript
module.exports = {
  root: true,
  env: {
    node: true
  },
  extends: ['plugin:vue/recommended', 'eslint:recommended', 'prettier'], //prettier放最后解决冲突
  parserOptions: {
    parser: 'babel-eslint'
  },
  rules: {
    'vue/max-attributes-per-line': [
      'error', //不符合报错
      {
        singleline: 100, //单行最多属性数量
        multiline: {
          max: 100, //多行最多属性数量
          allowFirstLine: true //是否允许属性在第一行 标签行
        }
      }
    ],
    'vue/html-closing-bracket-newline': [
      //结束标签换行 > 配置为不换行
      'error',
      {
        singleline: 'never',
        multiline: 'never'
      }
    ],
    'vue/html-indent': [
      'error',
      2,
      {
        attribute: 1,
        baseIndent: 1,
        closeBracket: 0,
        alignAttributesVertically: true,
        ignores: []
      }
    ],
    'vue/mustache-interpolation-spacing': 0, //{{}}之间有没有空格
    'vue/html-self-closing': [
      //html标签闭合规则
      'error',
      {
        html: {
          void: 'always',
          normal: 'any',
          component: 'any'
        },
        svg: 'always',
        math: 'always'
      }
    ],
    'vue/singleline-html-element-content-newline': 'off',
    'vue/multiline-html-element-content-newline': 'off',
    'vue/name-property-casing': ['error', 'PascalCase'],
    'vue/no-v-html': 'off', //以上几个与vetur冲突
    'no-console': process.env.NODE_ENV === 'production' ? 'error' : 'off',
    'no-debugger': process.env.NODE_ENV === 'production' ? 'error' : 'off',
    quotes: ['error', 'single'], //强制使用单引号
    semi: ['error', 'never'], //强制不使用分号结尾
    'no-unused-vars': 0 //变量未定义不提示
  }
}
```
- vetur 新版本需要的配置文件jsconfig.json 如果是ts 则需要tsconfig.json
```json
{
  "compilerOptions": {
    "baseUrl": "./",
    "paths": {
        "@/*": ["src/*"]
    }
  },
  "exclude": ["node_modules", "dist"]
}
```
__效果如下图__   
![calc](./images/eslint_prettier_vetur.png ':size=50%')  


