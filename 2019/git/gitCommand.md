# gitCommand
1. tag
    - git tag 
        - -l: 查询匹配的tag   git tag -l *-*-RELEASE
        - -d: 删除某个tag   git tag -d 2.5-20190905.1406-RELEASE
        - tagName : 本地打tagName
    - git push
        - origin tag/feature : 推送tag/分支到远程
        - origin -d tag/feature : 删除tag/分支到远程
    - git checkout [tagname] : 切换到某个tag
    - git push origin --tags : 将本地所有tag推送到git 仓库

2. add/commit
    - git add . : 会把工作时的所有变化提交到暂存区，包括文件内容修改(modified)以及新文件(new)，但不包括被删除的文件
    - git add -u : 监控已经被add的文件（即tracked file），他会将被修改的文件提交到暂存区。add -u 不会提交新文件(--update)
    - git add -A/--all : 添加所有的
    - git commit -m 

3. init/clone/push
    -


