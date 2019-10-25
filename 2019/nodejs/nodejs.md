# nodejs
### javaScript
1. 单线程执行，根本不能进行同步IO操作,所以基本都是使用异步;
2. ECMAScript是一种语言标准，而JavaScript是网景公司对ECMAScript标准的一种实现。
### Node.js
1. 基于JavaScript语言和V8引擎的开源Web服务器项目,JavaScript运行时引擎;
2. 命令
    2.1 node -v 查看node版本
    2.2 node 进入node环境
    2.3 double ctrl + c
    2.4 运行js脚本
        2.4.1 node name.js 

### npm
1. 一个网站，就是前面提到用于搜索 JS 模块的网站：https://www.npmjs.com/
2. 一个仓库，保存着人们分享的 JS 模块的大数据库
3. 命令行里的客户端，开发者使用它来管理、安装、发布模块
4. 命令
    4.1 npm -v
    4.2 npm install npm@latest -g : 更新npm; npm@latest 就是 <packageName>@<version> 的格式，我们在下载其他模块时也是这个格式。-g 代表全局安装。
    4.3 npm install -g cnpm --registry=https://registry.npm.taobao.org  -->  使用cnpm 淘宝进行进行包下载
    4.4 包管理命令
        ```
        npm install 
        npm install <package_name> --save-dev
        npm install -g

        npm update [-g] <package>
        npm uninstall --save lodash
     
        npm config get cache

        ```


### nvm 
1. node 和npm的版本管理工具
2. 安装包下载[nvm-windows](https://github.com/coreybutler/nvm-windows/releases)
3. 使用命令
    3.1 nvm -v
    3.2 nvm install node-version : 安装node
    3.3 npm install : 本地（当前项目路径）安装 或者 全局安装
        3.3.1 npm install : 默认安装所有的
        3.3.2 npm install --production :安装生产的所有依赖
        3.3.3 npm install <package_name>[@版本号] [--save/save-dev] : 安装指定包,默认安装最新;如果当前项目有 package.json 文件，下载包时会下载这个文件中指定的版本
         ```
        npm install sax@latest
        npm install sax@0.1.1
        npm install sax@">=0.1.0 <0.2.0"
        --save 表示将这个包名及对应的版本添加到 package.json的 dependencies
        --save-dev 表示将这个包名及对应的版本添加到 package.json的 devDependencies
         ```

### yarn 新的包管理工具
1. 安装yarn
2. 使用yarn cli
    2.1 
    ```
    yarn config get registry
    yarn config set registry 'https://registry.npm.taobao.org'
    ```

### babel 
1. 安装babel搭建es6 环境
2. 使用babel
    1. babel-node .\index.js
### ECMAScript 
1. JavaScript 开发标准,语法