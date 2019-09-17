# gitbook
## 创建gitbook账号
1. 

## 安装gitbook
1. 需要安装nodejs : 使用nvm进行管理
2. 通过 gitbook : npm install gitbook -g 
3. 安装gitbook cli : npm install -g gitbook-cli
4. 卸载gitbook : npm uninstall -g gitbook
## gitbook 命令
1. gitbook init  : 初始化和更新目录,会创建summary中新加入的目录,同时会生产readme
2. gitbook serve : 将本地gitbook项目进行本地启动
3. gitbook build : 打包本地项目
4. gitbook install : 安装book.json文件中的插件和package.json中的依赖
5. gitbook pdf ./ ./mybook.pdf :  Generate a PDF file
6. gitbook epub ./ ./mybook.epub : Generate an ePub file
7. gitbook mobi ./ ./mybook.mobi  :Generate a Mobi file