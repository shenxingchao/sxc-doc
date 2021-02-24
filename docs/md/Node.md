# NodeJs
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