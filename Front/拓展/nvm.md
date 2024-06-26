## 安装

settings.txt

```shell
	root: C:\dev\nvm       # nvm.exe所在位置的父路径，也是环境变量NVM_HOME的值
    path: C:\dev\nodejs    # nodejs快捷方式的存放位置，也是环境变量NVM_SYMLINK的值
    arch: 64               # 如果是32位系统，则这里的64改为32
    proxy:                 # 没有使用代理，
	node_mirror: https://npmmirror.com/mirrors/node/
	npm_mirror: https://npmmirror.com/mirrors/npm/
```

### 配置环境变量

```sh
1、NVM_HOME:C:\dev\nvm
2、NVM_SYMLINK:C:\dev\nodejs
3、PATH:%NVM_HOME%;%NVM_SYMLINK%;
```



```powershell
nvm list [available] # 查询可用的包
```

### 命令提示

nvm arch ：显示node是运行在32位还是64位。

nvm install  [arch] ：安装node， version是特定版本也可以是最新稳定版本latest。可选参数arch指定安装32位还是64位版本，默认是系统位数。可以添加--insecure绕过远程服务器的SSL。

nvm list [available] ：显示已安装的列表。可选参数available，显示可安装的所有版本。list可简化为ls。

nvm on ：开启node.js版本管理。

nvm off ：关闭node.js版本管理。

nvm proxy [url] ：设置下载代理。不加可选参数url，显示当前代理。将url设置为none则移除代理。

nvm node_mirror [url] ：设置node镜像。默认是[nodejs.org/dist/](https://link.juejin.cn?target=https%3A%2F%2Fnodejs.org%2Fdist%2F) 如果不写url，则使用默认url。设置后可至安装目录settings.txt文件查看，也可直接在该文件操作。

nvm npm_mirror [url] ：设置npm镜像。[github.com/npm/cli/arc…](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2Fnpm%2Fcli%2Farchive%2F) 。如果不写url，则使用默认url。设置后可至安装目录settings.txt文件查看，也可直接在该文件操作。

nvm uninstall  ：卸载指定版本node。

nvm use [version] [arch] ：使用制定版本node。可指定32/64位。

nvm root [path] ：设置存储不同版本node的目录。如果未设置，默认使用当前目录。

nvm version ：显示nvm版本。version可简化为v。



