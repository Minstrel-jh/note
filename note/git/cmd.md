[git]: /note/git/README.md

# Git命令

[返回][git]

## 基本命令

```text
git init                            初始化本地仓库
git remote add origin <gitpath>     关联到远程
git clone <gitpath>                 克隆到本地
git add <filename>                  添加到Index
git rm <filename>                   删除文件，并把删除add到Stage中
git commit                          提交
    -a                              添加到Index再提交
    -m "msg"                        提交信息
git push                            推送到远程仓库
git status [<filename>]             查看状态
git diff [<filename>]               查看WorkSpace和Stage的文件差异
git diff HEAD~n [<filename>]        查看WorkSpace和Repo(之前的n个版本)的文件差异
```

## 撤销操作

```text
git checkout -- <filename>          将文件回退到最后一次提交的状态(撤销WordSpace的更新)
    --                              后面的是文件名而不是分支名
git reset HEAD <filename>           从Index中删除，撤销add
git reset --hard HEAD~n             撤销commit到之前的第n次commit
```

## 分支相关命令

```text
git branch                          列出本地分支
    -r                              列出远程分支
    -a                              列出本地远程所有分支
git branch <branchname>             创建分支
    -d                              删除分支
    -D                              强制删除分支(没有合并)
git checkout <branchname>           切换分支
    -b                              创建并切换分支
git checkout –b <本地分支> <远程分支>   创建远程分支的本地分支，并切换到该分支
git push origin <本地分支>:<远程分支>   将本地分支推送到远程分支？
git push origin :<远程分支>         清空(删除)远程分支
git push -u origin <分支名>         推送到远程同名分支
    -u                              将远程仓库新分支设为当前分支上游
git merge <分支名>                  合并分支，只保留单条分支记录
    --no-ff                         保留合并分支的历史
```

## 历史相关命令

```text
git log                             查看commit日志
    --graph                         以分支结构查看
git reflog                          查看HEAD变化日志(包括撤销的commit)
git reset                           重置到HEAD
git reset HEAD~1                    重置到HEAD到之前的1次commit
    --mixed                         (默认)重置HEAD，保留WORKING
    --hard                          重置HEAD，WORKING
    --soft                          重置HEAD，保留WORKING在stage中
git push -f                         本地版本落后于远程版本，强制推送
```

## 其他技巧

```text
git update-index --assume-unchanged path/to/exclude         将本地的修改设置为不被提交
git update-index --no-assume-unchanged path/to/exclude      恢复
git ls-files -v | findstr "^h "                             查看忽略列表(windows系统)
```
