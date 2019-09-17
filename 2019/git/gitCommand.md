# gitCommand
1. tag
    1.1 git tag 
        1.1.1 -l: 查询匹配的tag   git tag -l *-*-RELEASE
        1.1.2 -d: 删除某个tag   git tag -d 2.5-20190905.1406-RELEASE
        1.1.3
    1.2 git push
        1.2.1 origin tag/feature : 推送tag/分支到远程
        1.2.1 origin -d tag/feature : 删除tag/分支到远程
    1.3 git checkout [tagname] : 切换到某个tag

2. add/commit
    2.1 git add . : 会把工作时的所有变化提交到暂存区，包括文件内容修改(modified)以及新文件(new)，但不包括被删除的文件
    2.2 git add -u : 监控已经被add的文件（即tracked file），他会将被修改的文件提交到暂存区。add -u 不会提交新文件(--update)
    2.3 git add -A/--all : 添加所有的
    2.4 git commit -m 

3. init/clone/push
    1. 
