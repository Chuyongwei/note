起步

配置

```powershell
git config (--local) user.name "sss"
git config (--local) user.email 'dfsfs'
git config --list #查看配置内容
```

初始化

```powershell
git init
```

提交

```powershell
git add 文件
git add -u #已提交的文件添加
git status 
git -commit -m'介绍'
```

查看历史

```powershell
git log
git log -n2 --oneline#只看最近的记录
```

查看当前文件

```powershell
ls -al
```

修改名字

```
git mv 旧文件名 新文件名
```

分支

```powershell
 git branch -v #查看分支
 git checkout -b 分支名 f7f25d009228a20e5c7eb7594aa726cdc5910545 #创建分支
 git checkout 分支名#选择分支
git log --all
git log --all --graph 
```

查看

```powershell
 git hlep --web log #图形化帮助
gitk #
```

,git文件

```powershell
git cat-file -p #内容
git cat-file -t #类型
```

上传到远程仓库

```
 git remote add origin https://github.com/Chuyongwei/test.git
 git push -u origin tage2
 $ git pull --rebase origin master

```



# 获取所有分支

## 方法一：

[git如何clone所有的远程分支](https://blog.csdn.net/yuanchao99/article/details/39118439)

git clone只能clone远程库的master分支，无法clone所有分支，解决办法如下：

1. 找一个干净目录，假设是git_work
2. `cd git_work`
3. `git clone http://myrepo.xxx.com/project/.git` ,这样在git_work目录下得到一个project子目录
4. `cd project`
5. `git branch -a`，列出所有分支名称如下：
   remotes/origin/dev
   remotes/origin/release
6. `git checkout -b dev origin/dev`，作用是checkout远程的dev分支，在本地起名为dev分支，并切换到本地的dev分支
7. `git checkout -b release origin/release`，作用参见上一步解释
8. `git checkout dev`，切换回dev分支，并开始开发。



## 方法二：

[git 从远程仓库获取所有分支](https://www.cnblogs.com/lpt1229/p/5979688.html)

```
git clone xxx
git branch -r | grep -v '\->' | while read remote; do git branch --track "${remote#origin/}" "$remote"; done
git fetch --all
git pull --all
```



Octotree 

Sourcegraph

