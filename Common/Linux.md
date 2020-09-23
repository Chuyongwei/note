# 初级

`whoami`和`hostname`

gnome-terminal

`ctrl +alt+f2~f6`打开终端

## 查找历史记录

- `↑`和`↓`：查找上一条（下一条）记录
- `ctrl+R`: 动态搜索
- `history`打开历史记录 使用`!+数字执行`



## 快捷键

- `ctrl+l`清屏 同`clear`
- `ctrl+D`给终端加上EOF
- `CTRL+A`到命令头
- `Ctrl+E`到命令尾部
- `Ctrl+U`删除光标左侧
- `Ctrl+K`删除光标右侧
- `Ctrl+W`删除左侧的单词
- `Ctrl+Y`复制删除的



## 文件管理

了解各个目录的功能,有助于系统维护的标准化和保障系统更加健壮的运作.在Linux系统中,请注意,任意修改的配置都不需要重启整个系统,除非是内核级的升级,但升级内核也不是每次必须要做的事,把握一个原则,稳定剩余一切(内核bug,严重安全漏洞,应用需要最新的软件包方可运行等除外)

详细列表

| 目录      | 说明                                     | 备注                                                         |
| --------- | ---------------------------------------- | ------------------------------------------------------------ |
| bin       | 存放普通用户可执行的指令                 | 即使在单用户模式下也能够执行处理                             |
| boot      | 开机引导目录                             | 包括Linux内核文件与开机所需要的文件                          |
| dev       | 设备目录                                 | 所有的硬件设备及周边均放置在这个设备目录中                   |
| etc       | 各种配置文件目录                         | 大部分配置属性均存放在这里                                   |
| lib/lib64 | 开机时常用的动态链接库                   | bin及sbin指令也会调用对应的lib库                             |
| media     | 可移除设备挂载目录                       | 类似软盘 U盘 光盘等临时挂放目录                              |
| mnt       | 用户临时挂载其他的文件系统               | 额外的设备可挂载在这里,相对临时而言                          |
| opt       | 第三方软件安装目录                       | 现在习惯性的放置在/usr/local中                               |
| proc      | 虚拟文件系统                             | 通常是内存中的映射,特别注意在误删除数据文件后，比如DB，只要系统不重启,还是有很大几率能将数据找回来 |
| root      | 系统管理员主目录                         | 除root之外,其他用户均放置在/home目录下                       |
| run       | 系统运行是所需文件                       | 以前防止在/var/run中,后来拆分成独立的/run目录。重启后重新生成对应的目录数据 |
| sbin      | 只有root才能运行的管理指令               | 跟bin类似,但只属于root管理员                                 |
| snap      | ubunut全新软件包管理方式                 | snap软件包一般在/snap这个目录下                              |
| srv       | 服务启动后需要访问的数据目录             |                                                              |
| sys       | 跟proc一样虚拟文件系统                   | 记录核心系统硬件信息                                         |
| tmp       | 存放临时文件目录                         | 所有用户对该目录均可读写                                     |
| usr       | 应用程序放置目录                         |                                                              |
| var       | 存放系统执行过程经常改变的文件           |                                                              |
| vmlinuz   | 软连接到boot下的vmlinuz-4.4.0-87-generic |                                                              |



`pwd`查看当前文件

`which 命令`查看命令所在位置

`ls`查看文件目录 `-a` 查看所有 `-l`查看 详情`-t`按时间排序

`-lath`组合



`du` 查看文件大小

`-h`适合人显示

`-a`显示文件夹

`-s`只显示文件总大小



查看文件

`cat` 即catt 连接文件

`less`更少的文件，空格向下一屏，回车向下一行，d向下半屏 b向上1页 y向上一行 u半屏 q退出 

- =显示位置：行数 字符数 整个文件所含字符数
- h显示帮助文档
- \ 搜索  n下一个 N上一个 

`head`显示文件头几行 `-n 数字`显示几行

`tail`显示尾几行

-  `-f`实时监控文件更新 `-s 数字`更新时间 `CTRL+c`退出



创建

使用空格文件用括号`"new file"`

`touch`创建空白文件

`mkdir`创建目录 `-p 1\2\3`递归创建目录结构



`cp`拷贝文件

`文件 目录` 拷贝文件

`文件 目录/新文件`拷贝到新文件

`-r ` recursive（递归）拷贝目录

`cp *.txt`拷贝所有的txt文件



`mv`移动文件

`文件 目标目录`移动文件

`文件 新文件名`改名



`rm`删除 remove

==在终端是没有回收站==

`文件1 文件2`

`-i`向用户确认是否删除 `y`是`n`否

`-f`强制删除

`-r`递归删除 删除目录   `rmdir`删除空文件

==rm -rf==

```powershell
[chuyongwei@localhost ~]$ rm -rf /
rm: 在"/" 进行递归操作十分危险
rm: 使用 --no-preserve-root 选项跳过安全模式
```



文件的存储

三部分：文件名，权限 文件内容（inode `ls -i`）



创建硬链接

- 一旦创建后，会同时修改
- 只能创建文件链接，不能创建文件夹的应连接
- `ln file1 file2`文件file1指向file2

创建软连接

- `ln -s file1 file2`
- link连接,可以连接目录



## 权限管理

`sodu`管理员执行

`sodu su`一直成为管理员

`Ctrl+d`退出管理员



用户

`useradd 用户`创建用户

`passwd 用户`设置密码

`userdel 用户`删除用户 `-r`或`--remove`删除home的用户文件

群组

`groupadd` 创建群组

`usermod`修改用户账户   `-l`对用户重命名   `-g`修改所在群组

`usemod -g 群组 用户` 

如果有多个群组就需要使用逗号`-G`

若不想离开原先的群组就要加上`-a`（append）`-aG`

`groups 用户`查找该用户所在群组 

`groupdel`删除群组名



授权

`chown 用户 文件`change和owner的缩写 改变所有者

`chown 用户:群组 文件`

`-R`递归将文件夹中的子文件和子目录都更改

`chgrp 群组 文件`change和group的缩写 改变文件群组



权限

d：directory表示“目录”， 

l：link 表示“链接”，

r：read表示“读”

w：write表示“写”

x：在目录上表示这个文件可以被读

`-`表示没有相应权限

文件详情的

`d`      `rwx`   `rwx               `               `rwx`

属性 所有者 群组用户  其他用户

`chmod`

| 权限 | 数字 |
| ---- | ---- |
| r    | 4    |
| w    | 2    |
| x    | 1    |

- 最宽泛的权限`777`
- `数字 文件`
- `u `用户 `g`群组 `o`其他 ` a`所有 ` +`添加 `-`删除 ` =`授权
- 例：`chmod u+x  o-r file`授予用户file文件的x权限,其他用户除去r权限
- `-R`递归修改
- `chmod -R 700 dir`



## 文本编辑

nano

[官网](https://www.nano-editor.org/)

`esc `然后`ctrl+x`关闭帮助

`-m`:激活鼠标控制光标位置

`i`激活缩进

`-A`激活智能home(从文章首改为段首)

`nano -miA file`组合键

通过.nanorc配置Nano

创建.nanorc文件

每行都由set和unset开头

`set mouse`激活鼠标

`set autoindent`激活自动缩进

`set smarthome`激活home键

全局配置文件`/etc/bashrc`



`.bashrc`配置终端文件

`.profile`外观



## 下载

安装包

- Red Hat : .rpm
- Debain: .deb

[镜像站](https://www.centos.org/download/mirrors/) [阿里云](https://developer.aliyun.com/mirror/)

修改镜像

1. 备份到 /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup

   ```powershell
   mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup
   ```

2. 拉取代码

   ```
   wget -O /etc/yum.repos.d/CentOS-Base.repo https://mirrors.aliyun.com/repo/Centos-7.repo
   
   ```

3. 生成缓存

   ```
   yum makecache 
   ```

包管理工具

`yum`

更新

`yum update/upgrade` 

下载

`yum install 安装包`

删除

`yum remove 安装包`

安装

`rpm -i *.rpm`

`yum localinstall *.rpm`

卸载

`rpm -e 包名`

`yum remove 包名`



Read The F**king Manual

## 帮助man

`man` 帮助

`sudo yum install -y man-pages`

`sudo mandb`

`man 编号 方法`

- 键盘上下键
- pagup pagedown 上下页
- 空格
- Home和end键 开的头和结尾
- `/`实现搜索 回车确认  `N`（shift+n）找上一个 `n`下一个

`apropos`:查找命令



### 查找文件

`locate 文件`查找文件

`sudo updatedb`更新文件数据库

`find 何处 何物 做什么`

`find -name "newfile"`

`find -size +10m`按大小查找 大于10M的文件

`find -size -50k`小于50k的文件

`find -atime -7`近7天的文件

`-type d`查找目录类型

`-type l`查找文件

`printf "%p-%u\n"`

- `%p`文件名
- `%u`所有者
- `\n`换行

`-exec`后接执行语句

- `-exec chdom 600 {}\`
- `{}`文件名
- `/`语句结束

`-ok`同`-exec`但是可以逐个确认



# 高级

### `grep`筛选数据

- `grep text file`搜索file文件下的text文件
- `-i`不区分大小写
- `-n`显示行号
- `-v`invert 所有没有text行
- `-r`在所有子目录和子文件查找 同`rgrep`
- 例：`grep text -Ir file`
- 高级正则
- `-E` `egrep`

![正则](data:image/png;base64,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oc76iHtftsRdd62/R9t3my+OaYP8f/e73x30G0JZX0MY5/ef//mfg27z1dmPfvSjQfdJsSFtMhT2O9/5znpu46COF5MH6Qz1uSEMteH3v//95s4779yg8q8UfPzjH58lq0pYTyB0xs2VsD760Y82H/vYx5pPfvKTHT7xiU90+I//+I+ug37qU5+a9fv0pz/dDX5K4OCDD24OOeSQ5qCDDuqe3/a2t3XPl112WfPud7+7OeKII2bxrne9q/ne977XpUcpvOlNb+qwyy67NG94wxvmKJRVq1Y1W2yxRack4jYftN+GKmQQD8iP+CZVUscdd1yX50nlxU/+C1/4whz3E044oXnxi188x62EOhQudQnyye2b3/zmHNk+5O0pT3lKc8UVV8yWk7vw2uTqq69uPvCBD3Rt3g974IEHNm9+85s7mSuvvHI9cL/qqquaz3zmM7NxJ/7rrrtutm6ClP+OO+5YL60hvPe97+3kTzrppEH/oEwDuD35yU/uwqc9A3mIDCAmaZSklbrp5z/+5fPmgEpYUwIdb3MlLIPx2c9+dvPc5z63w5Oe9KTm3//935vnPOc5zVZbbdU89alP7Z632WabDsIgrNNOO6059thjO4WAqIQ588wzmxtvvLHzO/3002chjSjUr33ta937Rz7ykebkk0/unpMXCs/7hz/84e53yJro43Wve10n9+pXv7pTMHvttVdz6KGHDsrOh1e96lVdXDvuuON6Cm0cyKkHZDDk3wcyVr5vfetbHdQNhYmwXvKSlwyGkZ9bb721C+eZm3S1RdxK9POeer/pppu63z623HLLru4uvfTSOeHExV84Mv/2b/+2HrgnDukGwvvVP7bffvuufX784x93BPLCF75wPdkhIEJxf+Mb3+h+1cGQHOgDfUhHOM/aNfD++c9/vgsnXyZd5Mpyve9972sOO+yw2bIF3ufL90pEJawpgY63ORPWbbfdNqtMkBPl5nm33XZrTjzxxM5vKKwZ8jOe8YzmnHPO6YiD29AglkafsMix3rbbbruOJM4777zO/ZZbbunktt122+59SPmWIBPLw7s85XmhEC4zbb+j0r3wwgubnXfeeQ623nrrLtwb3/jGWbAi1W0/vAkC2RIXXHDBWMLaYYcdOguJ7Otf//rOOuWO9JHlTjvtNAfXXnvtnPDCsXaV6Ytf/GLz5S9/uXvmzorQ3qV8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)



sort排序算法

`nano name.txt`

`sort 文件`

`-o newfile file`将file排序放到newfile

`-r`反序

`-R`随机排序

`-n`数字排序



`wc 文件`

显示行数，单词数，字节数

`-l`行数

`-w`单词

`-c`字节数

`-m`字符数



`uniq 文件`：unique删除文件中重复的内容

`-c`统计重复的行数

`-d`只显示重复的行



`cut`剪切文件的一部分

`cut -c 2-4 name.txt`剪切文件中每行2到4字符

`cut -d , -f 3 notes.csv`以`，`为界限每行的第3组的数据

`>`cut的结果流到目标文件

- 若有该文件则替换，没有则新建，且不做确认
- `/dev/null`黑洞文件

`>>`cut的结果流到目标文件

- 若有该文件则追加，没有则新建，且不做确认
- `/dev/null`黑洞文件

### 流、管道

三种流

`stdout`标准输出流、`stderr`标准错误流、`stdin`stdin是标准输入 

std即standard（标准），in即input（输入），合起来就是标准输入。 一般就是指键盘输入到[缓冲区](https://baike.baidu.com/item/缓冲区)里的东西

`cat not_exist_file.scv > result.txt`标准输出流

`cat not_exist_file.csv 2>  error.txt`标准错误输出流

`cat not_exist_file.scv >> result.txt`标准输出流（追加）

`cat not_exist_file.csv 2> > error.txt`标准错误输出流

`cat not_exist_file.scv > result.txt 2> error.txt`

`2>&1`

- cat not_exist.csv > results.txt 2>&1
- cat not_exist.csv >> results.txt 2>&1



`<`输入流

- `cat < notes.csv`的运行结果和`cat notes.csv`一样
- 但是原理不一样
- 前者是将内容交给终端，后者是显示内容

`<<`重定向的输入为键盘输入

三种流的组合

`sort -n << END > number.txt  sorted.txt 2>&1`



管道

多个命令同时使用

`A|B` : A-->B  指令A输入指令B输出

 

### 多用户多任务管理

`w`

`uptime`运行时间

```
执行时间 up 运行时间, 运行用户数，load average: 1min 5min 15min
```

接下来  的  条目 显示 每位 用户 的: 登录名, tty 名, 远程主机, 登录时间,空闲时间, JCPU, PCPU, 以及 他们 当前进程 的 命令行.

   JCPU 时间 指 某个 tty 上 所有 进程 用掉的 时间, 不包括 过去的 后台任务,   但是 包括 正在 运行 的 后台任务.

`tload`运行进程

tty的 pts非全屏界面开的多个终端

idle多长时间没活跃

JCPU:该终端所有相关进程使用的CPU时间，每当进程结束时就停止计时，开始则重新计时

Pcpu:

表示执行当前程序所消耗的时间，当前的程序是what

`who`跟`w`的第二行差不多

- （:0）由谁进行的



`ps`进程

- pid 进程号
- tty 
- TIME 进程运行了多久
- CMD 进行程序名
- `-ef`
- uid用户名
- PPID程序的父进程
- `-u`当前用户的进程
- `-aux`
- `%CPU`占用cpu的百分比
- `%MEM`内存比
- `VSZ`
- `STAT`进程状态
- `ps -aux --sort -pcpu | less`:根据cpu的使用率降序排列
- `ps -aux --sort -pcpu | less`根据内存的降序排列
- `ps -aux --sort -pcpu，+pmen | less`
- `ps -axjf`和`pstree`树形显示进程

`top`

按使用比率进行动态显示进程

`h`帮助文档

`u`依照用户过滤显示

`f/F`过滤某些列

`k`结束进程号  通过输入进程号

`s`设置刷新时间 默认3s



安装`glances`

```powershell
yum install epel* -y
yum install python-pip python-devel -y
yum install glances -y
```

输入`glances`运行

`htop`软件

`sudo yum install -y htop`

输入`htop`运行



`ctrl +c`结束进程

`shift+ctrl+c`复制`shift+ctrl+V`粘贴

`kill PID1 PID2 ...`杀死进程

`kill -9  PID1`强制结束进程

`killall 程序名`结束该程序

`halt`关闭系统

`reboot`重启

`shutdown`



`&`和`nohup`：后台运行进程

`&`后台运行

`nohup`命令

- &终端关闭时进程（hupup信号）也会关闭
- nohup也会

`ctrl+z`转到后台，并暂停运行

`bg`使进程转到后台



`bg`使进程转到前台

`jobs`显示后台进程状态



![状态转换图](data:image/png;base64,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)



5种常见的进程状态

1. 不可中断D
2. 僵死（即停止但pid存在）Z
3. 停止 T
4. 运行 S



`date`时间

`date "+%H:%M:%S"`

`at`

`sudo yum install at`

```shell
[chuyongwei@localhost ~]$ at 22:10
at> touch file.txt
at> <EOT> #ctrl+d停止
job 1 at Sat Jun 27 22:10:00 2020

at 22:10 tomorrow #明天执行
at 22:10 12/10/19 #2019年12月10日执行 
```

`at now +10 minures`

- minutes:分钟
- hours：小时
- days：天
- week:星期
- mouths：月
- years：年



`atq`列出正等待执行的at任务

`atrm`删除正在等待执行的at任务

- rm是英语remove的首字母，remove表示“删除”
- `atrm at编号`

`sleep 数字`休息一会

- 默认以秒为单位

- m
- h
- d



`&&`和`||`符号

- `&&`：前面执行成功才执行后面的
- `||`：前面执行失败则执行后面的
- `;`：执行完前面的执行后面的



Crontab

安装

Centos：

```powershell
sudo yum install vixie-cron crontabs
```

ubuntu：

```powershell
sudo apt install cron #安装cron
service cron restart 或者 restart cron #重启cron
```

前期配置

```powershell
echo "export EDITOR=nano" >> ~/.bashrc #配置默认编译器为nano否则默认使用vi
source ~/.bashrc #立即应用
```

三个参数

- `-l`显示crontab文件
- `-e`修改crontab文件
- `-r`删除crontab文件

编辑crontab文件

- m h dom mon dow command
- 分 时 日 月 星期几 执行命令
- (0~59)(0~23)(1~31)(1~12)(0~6)
- 路径建议使用结对路径
- 成功就显示crontab: installing new crontab



### 解压和压缩

常用的压缩软件 gzip和bzip2 tar

基本概念：

- 打包：将多个文件变成一个总的文件(archive)存档，归档
- 压缩：将大文件用压缩算法压成更小的文件
- 用tar将多个文件归档成总的文件，然后用gzip或bzip2命令将archive压缩为更小的文件

`tar`

`tar -cvf xxx.tar fold`

`tar -cvf xxx.tar file1,file2`

- c:  create创建一个归档文件 
- v: 显示操作
- f: file 指定归档文件

`-tf`显示归档内容

`-rvf`追加文件当归档

`-xvf`解开归档

gzip和bzip2

`.tar.gz`用gzip命令压缩的文件后缀名

`.tar.nz2`用bzip2

```powershell
gzip sorting.tar
bzip2 sorting.tar
```

gunzip和bunzip2解压



归档，压缩

gzip

`tar -zcvf soring.tar.gz sorting/`压缩

`tar -zxvf sorting.tar.gz`解压

bzip2

`tar -jcvf soring.tar.gz sorting/`压缩

`tar -jxvf sorting.tar.gz`解压



显示压缩内容

`zcat sorting.tar.gz`

`bzcat sorting.tar.bz2`

处理windows的压缩文件

```shell
 sudo yum instal unzip #Red Hat安装
 unzip archive.zip #解压.zip文件
 unzip -l archive.zip #不解开。zip文件，只看其中内容
```

压缩成zip文件

```powershell
 sudo yum instal zip #Red Hat安装
 zip -r sorting.zip sorting/ #解压.zip文件
 zip -l archive.zip #不解开。zip文件，只看其中内容
```

`rar`和`unrar`

```shell
rar a xxx.rar /sort #压缩
unrar e xxx.rar #解压
unrar l xxx.rar #查看内容
```



### 编译安装

==下载源代码==--> ==解压压缩包== -->==配置== -->==编译==--> ==安装== 

Linux存在多个发行版有很多变数因此做安装很难

1. 找安装包 `rpm`

   alien:可以将deb安装包和rpm安装包相互转换

   ```shell
   sudo yum install alien #安装alien
   sudo alien -r xxx.deb #将deb转换为rpm
   ```

2. 安装rpm包

   ```powershell
   sudo rpm -i xxx.rpm
   ```

如果找不到rpm安装包就。。自己做

编译安装

将源代码转换成可执行文件

```
yum -y install ncurses-devel
make
sudo make install

```

例：

安装rar和unrar

`wget http：//。。`拉取

```shell
wget https://www.rarlab.com/rar/rarlinux-x64-5.7.0.tar.gz
tar -zxvf rar...
cd rar
sudo make
```

https://www.rarlab.com/rar/rarlinux-x64-5.9.1b1.tar.gz





## 服务器

查看ip

```powershell
ipconfig
ip addr
```

安装net-tool

以获取==ipconfig==

```
yum install net-tools
```

```
yum install nano
```



### 网络协议

各种协议

http协议：超文本传输协议

https协议

文件协议: ftp协议

邮件协议：POP3 IMAP



### 明文和密文

wireshark网络分析器监听网络

SSH保护协议

shell

非对称加密的两个密钥



制作ssh协议

安装[openssh](https://www.openssh.com/)

安装客户端

```
sudo yum install openssh-clients
```

安装服务端

```
sudo yum install openssh-server
```

安装完成后会自动开启sshd进程

手动开启

```shell
sudo systemctl start sshd
```

关闭

```shell
sudo systemctl stop sshd
```

重启

```shell
sudo systemctl restart sshd
```

查看

```shell
sudo systemctl status sshd
```

设为开机自启

```shell
sudo systemctl enable sshd
```



```powershell
systemctl list-unit-files | grep enabled
```

### 安装ssh工具

[putty](https://www.chiark.greenend.org.uk/~sgtatham/putty/)、[secureCRT](https://www.vandyke.com/products/securecrt/)、[xhsell](https://www.netsarang.com/zh/xshell-download/)、[termius](https://termius.com/)



linux/MAC

```shell
ssh root@192.168.229.130#用户名@ip
#确认信任
#输入密码

```

配置config文件

全局有两个

- SSH客户端：/etc/ssh/ssh_config
- /~/.ssh/config
- SSH服务端：/etc/ssh/sshd_config

查看帮助文件

- man ssh_config
- man sshd_config

配置完后

```powershell
sudo systemctl restart sshd#重启sshd
```

客户端

```powershell
ssh localhost#生成/~/.ssh目录
#创建config
chmod 600 config#一般将该文件建成该权限
```

config

```
Host imooc
    HostName 192.168.229.130
    Port 22
    User root
```

输入`ssh imooc`进入响应的服务器

<!-- 2020.06.29 -->

建立密钥对

```powershell
[chuyongwei@localhost ~]$ ssh-keygen  #建立密钥对
Generating public/private rsa key pair.  #创建成功
Enter file in which to save the key  (/home/chuyongwei/.ssh/id_rsa): #输入保存路径（默认/home/chuyongwei/.ssh/id_rsa）
Enter passphrase (empty for no passphrase):  #密钥（默认为空）
Enter same passphrase again:   #（确认密钥）
Your identification has been saved in /home/chuyongwei/.ssh/id_rsa.
Your public key has been saved in /home/chuyongwei/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:FjNvLSFirh6j5BROIeRGvMbLjd1NGS4W4czxBd4YdkA chuyongwei@localhost
The key's randomart image is:
+---[RSA 2048]----+
|.o   ooEoo       |
|+.  +.=.*        |
|ooo  +=+B..      |
|.= . = = * o     |
|o B o = S + .    |
| * + o o . .     |
|  + +            |
| + o o           |
|  o .            |
+----[SHA256]-----+
```

然后会生成连两个文件

- id_rsa.pub:公钥
- id_rsa：私钥

```powershell
ssh-copy-id 用户@地址
等于
ssh-copy-id -i ~/.ssh/id_rsa.pub 用户@地址
```

```powershell
[chuyongwei@localhost ~]$ ssh-copy-id root@192.168.229.130
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
root@192.168.229.130's password: 

Number of key(s) added: 1

Now try logging into the machine, with:   "ssh 'root@192.168.229.130'"
and check to make sure that only the key(s) you wanted were added.
```

ssh-copy-id把客户机的公钥追加到服务器的一个文件==~/.ssh/authorized_keys==



设置ssh后还想使用密码登录

```powershell
ssh -o PreferredAuthentications=password -o PubkeyAuthentication=no user@host
```



## 顶级编译器vim

[vim](https://www.vim.org/)和[emacs](https://www.gnu.org/software/emacs/)选一个

vim

```powershell
sudo yum install vim
```

`vimtutor`vim的教程

### 三种模式

1. 交互模式

2. 插入模式

   `a`下一字节插入`A`该行的末尾，`o`和`O`，`i`和`I`

3. 命令模式



在正常模式下修改命令的格式是：
               operator   [number]   motion
     其中：
       operator - 操作符，代表要做的事情，比如 d 代表删除
       [number] - 可以附加的数字，代表动作重复的次数
       motion   - 动作，代表在所操作的文本上的移动，例如 w 代表单词(word)，
                  $ 代表行末等等。

删除命令 

`d motion`d 删除操作符号 motion操作符的操作对象

- `w`从光标处删除一个单词，直到下一单词
- `$`从当前光标删除到行末
- `0`从当前位置删到行首
- `e`从当前位置删到单词尾
- 输入 `d2w` 以删除两个大写字母单词。
- 输入 dd 删除该行。
- 接着输入 2dd 删除两行。

`[number] x`删除单个字符



移动符号

    1. 输入 3e 使光标向前移动到第三个单词的末尾。
    2. 输入 2w 使光标向前移动两个单词。
    3. 输入 0 (数字零) 移动光标到行首。
    4. 输入`$`末尾

撤销

1. `u`撤销以前操作`U`恢复该行原始状态
2. `ctrl+r`撤销撤销操作



置入类指令

`p`将删除的内容下一行粘贴



更改类

`c [number] motion`

`cw`删除单词并进入插入模式

定位及文件状态

- 输入 CTRL-G 显示当前编辑文件中当前光标所在行位置以及文件状态信息。
- 输入`行号`然后`G` 则直接跳转到文件中的某一指定行。
- `G`跳到最后一行`gg`跳到第一行

搜索类

- 正向查找：`/`输入要找的内容`n`下一条`N`上一条
- 反向查找：`?`输入要找的内容
- `ctril+o`回到原来位置`ctrl+i`跳到新的位置

配对括号

- 输入在[、]、(、)、{、}上打`%`将会自动跳到另一边

替换

- 输入 :s/old/new/g 可以替换 old 为 new（不加/g只换一个）

- ```powershell
   输入   :#,#s/old/new/g   其中 #,# 代表的是替换操作的若干行中
                            首尾两行的行号。
   输入   :%s/old/new/g     则是替换整个文件中的每个匹配串。
   输入   :%s/old/new/gc    会找到整个文件中的每个匹配串，并且对每个匹配串
                            提示是否进行替换。
  ```



外部执行方法

输入`:!指令`

`:w file`将该文件写入file中

`v`选中文件中的某些内容

`:r file`将file文件内容写入光标处

`:r ! 指令`将结果输入内容



`r`替换单个字符

`R`覆盖当前内容输入

`y`复制`p`粘贴

  1. 输入大写的 O 可以在光标上方打开新的一行。

  2. 输入小写的 a 可以在光标所在位置之后插入文本。
     

输入大写的 A 可以在光标所在行的行末之后插入文本。
     
  3. e 命令可以使光标移动到单词末尾。

  4. 操作符 y 复制文本，p 粘贴先前复制的文本。

  5. 输入大写的 R 将进入替换模式，直至按 <ESC> 键回到正常模式。

  6. 输入 :set xxx 可以设置 xxx 选项。一些有用的选项如下：
     'ic' 'ignorecase'       查找时忽略字母大小写
     'is' 'incsearch'        查找短语时显示部分匹配
     'hls' 'hlsearch'        高亮显示所有的匹配短语
     选项名可以用完整版本，也可以用缩略版本。

  7. 在选项前加上 no 可以关闭选项：  :set noic



查看帮助文档

1. 按下`help`
2. 按下`F1`
3. `:help `

设之高亮

~/.vimrc

`:r $VIMRUNTIME/vimrc_example.vim`



`ctrl+d`查看可能的命令

`tab`补全内容



### 可视模式

`v`：可视模式

`shift+v`：行可视模式

`ctrl+v`：块可视模式 

- 选中后用`I`来编译最后按两次`ESC`就可以块编辑



[帮助文档](http://vimdoc.sourceforge.net/htmldoc/)

[插件](https://www.vim.org/scripts/index.php)

编辑

`: set nu`

配置文件`/etc/vimrc`

```shell
cp /etc/vimrc ~/.vimrc
```

`synatx`语法高亮

`number`显示行号

`showcmd`显示命令

`ignorecase`忽略大小写

`set mouse=a`激活鼠标





## git

```powershell
sudo install -y git
```



```powershell
git config --global color.ui auto #关闭颜色配置
git config --global color.ui false #关闭
```



## 网络安全

### wget

```powershell
sudo yum install wget
```

`wget url`拉取代码

`wget -c url`继续下载

### `scp`网间拷贝

`scp 源文件 目标文件`

user@ip:file_name

- user用户名
- ip地址
- 路径目录

`-P`设置端口号（默认22）



### ftp&sftp:传输文件

```
ftp -p url
```

用户名anonymous

密码随便

```powershell
get 文件 #获得文件
put 文件 #写入文件
```

`!`使用本机指令

`delete`删除文件



sftp

```powershell
sftp [-oProt 端口] user@id
```



`rsync`同步备份

```powershell
sudo yum install rsync
```



```
rsync -arv --delete /images/username@id:/
```

`-arv`

`-a`：保留文件所有信息

`-r`：递归调用

`v`：冗余模式显示信息

不会删除文件

`--delete`删除不存在文件



### ipv4和ipv6

主机名

`host url`获取IP地址



whois

```powershell
sudo yum install whois
```

`whois url`获取相关的网址信息



ifconfig

`lo`本地回环：用于在自己电脑上测试服务器

`wlano`无线网卡

`eth0`有线网卡

`virbr0`虚拟网络接口

enp0s3 en网卡 p（0，3）

新版用systemd替换了initd来引导系统



列出的网络接口

- 网卡名称，inet参数后面的ip地址
- ether参数后面的网卡物理地址（MAC）
- RX TX

ifconfig：配置网路接口

```powershell
ifconfig interfacr state #开关网卡
```

- interface 要替换的网络接口（eth0）
- state up和down打开和关闭

`yum info ifconfig`查看相关信息



`netstat -i`网络接口的统计信息

`RX`接收 `Tx`发送的包

`MTU`最大传输单元

`RX-ok`接收成功的

`RX-ERR`接收失败的

`RX-DRP`丢弃的

`RX-OVR`过速丢弃的

`netstat -uta`：列出所有开启的链接

`u`UDP

`t`TCP

`a`所有状态

sate

ESTABLISHED 连接已建立

TIME——WAIT：连接正在等待

CLOSE_WAIT:远程服务器终止

CLOSEING 远程服务器关闭

LISTEN监听



`netstat -i`列出listen的信息

`-s`统计信息

port端口



### 防火墙

- 打开防火墙

  ```powershell
  systemctl stop firewalld
  ```

- 查看状态

  ```
  systemctl status firewalld
  ```


### firewall-cmd

Linux上新用的防火墙软件，跟iptables差不多的工具

firewall-cmd 是 firewalld的字符界面管理工具，firewalld是centos7的一大特性，最大的好处有两个：支持动态更新，不用重启服务；第二个就是加入了防火墙的“zone”概念。

firewalld跟iptables比起来至少有两大好处：

1. firewalld可以动态修改单条规则，而不需要像iptables那样，在修改了规则后必须得全部刷新才可以生效。
2. firewalld在使用上要比iptables人性化很多，即使不明白“五张表五条链”而且对TCP/IP协议也不理解也可以实现大部分功能。

firewalld自身并不具备防火墙的功能，而是和iptables一样需要通过内核的netfilter来实现，也就是说firewalld和 iptables一样，他们的作用都是用于维护规则，而真正使用规则干活的是内核的netfilter，只不过firewalld和iptables的结 构以及使用方法不一样罢了。

**命令格式**

```shell
firewall-cmd [选项 ... ]
```

#### 选项

通用选项

```shell
-h, --help    # 显示帮助信息；
-V, --version # 显示版本信息. （这个选项不能与其他选项组合）；
-q, --quiet   # 不打印状态消息；
```

状态选项

```shell
--state                # 显示firewalld的状态；
--reload               # 不中断服务的重新加载；
--complete-reload      # 中断所有连接的重新加载；
--runtime-to-permanent # 将当前防火墙的规则永久保存；
--check-config         # 检查配置正确性；
```

日志选项

```shell
--get-log-denied         # 获取记录被拒绝的日志；
--set-log-denied=<value> # 设置记录被拒绝的日志，只能为 'all','unicast','broadcast','multicast','off' 其中的一个；
```

#### 实例

```shell
# 安装firewalld
yum install firewalld firewall-config

systemctl start  firewalld # 启动
systemctl stop firewalld  # 停止
systemctl enable firewalld # 启用自动启动
systemctl disable firewalld # 禁用自动启动
systemctl status firewalld # 或者 firewall-cmd --state 查看状态

# 关闭服务的方法
# 你也可以关闭目前还不熟悉的FirewallD防火墙，而使用iptables，命令如下：

systemctl stop firewalld
systemctl disable firewalld
yum install iptables-services
systemctl start iptables
systemctl enable iptables
```

配置firewalld

```shell
firewall-cmd --version  # 查看版本
firewall-cmd --help     # 查看帮助

# 查看设置：
firewall-cmd --state  # 显示状态
firewall-cmd --get-active-zones  # 查看区域信息
firewall-cmd --get-zone-of-interface=eth0  # 查看指定接口所属区域
firewall-cmd --panic-on  # 拒绝所有包
firewall-cmd --panic-off  # 取消拒绝状态
firewall-cmd --query-panic  # 查看是否拒绝

firewall-cmd --reload # 更新防火墙规则
firewall-cmd --complete-reload
# 两者的区别就是第一个无需断开连接，就是firewalld特性之一动态添加规则，第二个需要断开连接，类似重启服务


# 将接口添加到区域，默认接口都在public
firewall-cmd --zone=public --add-interface=eth0
# 永久生效再加上 --permanent 然后reload防火墙

# 设置默认接口区域，立即生效无需重启
firewall-cmd --set-default-zone=public

# 查看所有打开的端口：
firewall-cmd --zone=dmz --list-ports

# 加入一个端口到区域：
firewall-cmd --zone=dmz --add-port=8080/tcp
# 若要永久生效方法同上

# 打开一个服务，类似于将端口可视化，服务需要在配置文件中添加，/etc/firewalld 目录下有services文件夹，这个不详细说了，详情参考文档
firewall-cmd --zone=work --add-service=smtp

# 移除服务
firewall-cmd --zone=work --remove-service=smtp

# 显示支持的区域列表
firewall-cmd --get-zones

# 设置为家庭区域
firewall-cmd --set-default-zone=home

# 查看当前区域
firewall-cmd --get-active-zones

# 设置当前区域的接口
firewall-cmd --get-zone-of-interface=enp03s

# 显示所有公共区域（public）
firewall-cmd --zone=public --list-all

# 临时修改网络接口（enp0s3）为内部区域（internal）
firewall-cmd --zone=internal --change-interface=enp03s

# 永久修改网络接口enp03s为内部区域（internal）
firewall-cmd --permanent --zone=internal --change-interface=enp03s
```

服务管理

```shell
# 显示服务列表  
Amanda, FTP, Samba和TFTP等最重要的服务已经被FirewallD提供相应的服务，可以使用如下命令查看：

firewall-cmd --get-services

# 允许SSH服务通过
firewall-cmd --new-service=ssh

# 禁止SSH服务通过
firewall-cmd --delete-service=ssh

# 打开TCP的8080端口
firewall-cmd --enable ports=8080/tcp

# 临时允许Samba服务通过600秒
firewall-cmd --enable service=samba --timeout=600

# 显示当前服务
firewall-cmd --list-services

# 添加HTTP服务到内部区域（internal）
firewall-cmd --permanent --zone=internal --add-service=http
firewall-cmd --reload     # 在不改变状态的条件下重新加载防火墙
```

端口管理

```shell
# 打开443/TCP端口
firewall-cmd --add-port=443/tcp

# 永久打开3690/TCP端口
firewall-cmd --permanent --add-port=3690/tcp

# 永久打开端口好像需要reload一下，临时打开好像不用，如果用了reload临时打开的端口就失效了
# 其它服务也可能是这样的，这个没有测试
firewall-cmd --reload

# 查看防火墙，添加的端口也可以看到
firewall-cmd --list-all
```

直接模式

```shell
# FirewallD包括一种直接模式，使用它可以完成一些工作，例如打开TCP协议的9999端口

firewall-cmd --direct -add-rule ipv4 filter INPUT 0 -p tcp --dport 9000 -j ACCEPT
firewall-cmd --reload
```

**自定义服务管理**

选项

```shell
（末尾带有 [P only] 的话表示该选项除了与（--permanent）之外，不能与其他选项一同使用！）
--new-service=<服务名> 新建一个自定义服务 [P only]
--new-service-from-file=<文件名> [--name=<服务名>]
                      从文件中读取配置用以新建一个自定义服务 [P only]
--delete-service=<服务名>
                      删除一个已存在的服务 [P only]
--load-service-defaults=<服务名>
                      Load icmptype default settings [P only]
--info-service=<服务名>
                      显示该服务的相关信息
--path-service=<服务名>
                      显示该服务的文件的相关路径 [P only]
--service=<服务名> --set-description=<描述>
                      给该服务设置描述信息 [P only]
--service=<服务名> --get-description
                      显示该服务的描述信息 [P only]
--service=<服务名> --set-short=<描述>
                      给该服务设置一个简短的描述 [P only]
--service=<服务名> --get-short
                      显示该服务的简短描述 [P only]

--service=<服务名> --add-port=<端口号>[-<端口号>]/<protocol>
                      给该服务添加一个新的端口(端口段) [P only]

--service=<服务名> --remove-port=<端口号>[-<端口号>]/<protocol>
                      从该服务上移除一个端口(端口段) [P only]

--service=<服务名> --query-port=<端口号>[-<端口号>]/<protocol>
                      查询该服务是否添加了某个端口(端口段) [P only]

--service=<服务名> --get-ports
                      显示该服务添加的所有端口 [P only]

--service=<服务名> --add-protocol=<protocol>
                      为该服务添加一个协议 [P only]

--service=<服务名> --remove-protocol=<protocol>
                      从该服务上移除一个协议 [P only]

--service=<服务名> --query-protocol=<protocol>
                      查询该服务是否添加了某个协议 [P only]

--service=<服务名> --get-protocols
                      显示该服务添加的所有协议 [P only]

--service=<服务名> --add-source-port=<端口号>[-<端口号>]/<protocol>
                      添加新的源端口(端口段)到该服务 [P only]

--service=<服务名> --remove-source-port=<端口号>[-<端口号>]/<protocol>
                      从该服务中删除源端口(端口段) [P only]

--service=<服务名> --query-source-port=<端口号>[-<端口号>]/<protocol>
                      查询该服务是否添加了某个源端口(端口段) [P only]

--service=<服务名> --get-source-ports
                      显示该服务所有源端口 [P only]

--service=<服务名> --add-module=<module>
                      为该服务添加一个模块 [P only]
--service=<服务名> --remove-module=<module>
                      为该服务移除一个模块 [P only]
--service=<服务名> --query-module=<module>
                      查询该服务是否添加了某个模块 [P only]
--service=<服务名> --get-modules
                      显示该服务添加的所有模块 [P only]
--service=<服务名> --set-destination=<ipv>:<address>[/<mask>]
                      Set destination for ipv to address in service [P only]
--service=<服务名> --remove-destination=<ipv>
                      Disable destination for ipv i service [P only]
--service=<服务名> --query-destination=<ipv>:<address>[/<mask>]
                      Return whether destination ipv is set for service [P only]
--service=<服务名> --get-destinations
                      List destinations in service [P only]
```

**控制端口 / 服务**

可以通过两种方式控制端口的开放，一种是指定端口号另一种是指定服务名。虽然开放 http 服务就是开放了 80 端口，但是还是不能通过端口号来关闭，也就是说通过指定服务名开放的就要通过指定服务名关闭；通过指定端口号开放的就要通过指定端口号关闭。还有一个要注意的就是指定端口的时候一定要指定是什么协议，tcp 还是 udp。知道这个之后以后就不用每次先关防火墙了，可以让防火墙真正的生效。

```shell
firewall-cmd --add-service=mysql        # 开放mysql端口
firewall-cmd --remove-service=http      # 阻止http端口
firewall-cmd --list-services            # 查看开放的服务
firewall-cmd --add-port=3306/tcp        # 开放通过tcp访问3306
firewall-cmd --remove-port=80tcp        # 阻止通过tcp访问3306
firewall-cmd --add-port=233/udp         # 开放通过udp访问233
firewall-cmd --list-ports               # 查看开放的端口
```

伪装 IP

```shell
firewall-cmd --query-masquerade # 检查是否允许伪装IP
firewall-cmd --add-masquerade   # 允许防火墙伪装IP
firewall-cmd --remove-masquerade# 禁止防火墙伪装IP
```

**端口转发**

端口转发可以将指定地址访问指定的端口时，将流量转发至指定地址的指定端口。转发的目的如果不指定 ip 的话就默认为本机，如果指定了 ip 却没指定端口，则默认使用来源端口。 如果配置好端口转发之后不能用，可以检查下面两个问题：

1. 比如我将 80 端口转发至 8080 端口，首先检查本地的 80 端口和目标的 8080 端口是否开放监听了
2. 其次检查是否允许伪装 IP，没允许的话要开启伪装 IP

```shell
firewall-cmd --add-forward-port=port=80:proto=tcp:toport=8080   # 将80端口的流量转发至8080
firewall-cmd --add-forward-port=port=80:proto=tcp:toaddr=192.168.0.1 # 将80端口的流量转发至192.168.0.1
firewall-cmd --add-forward-port=port=80:proto=tcp:toaddr=192.168.0.1:toport=8080 # 将80端口的流量转发至192.168.0.1的8080端口
```

1. 当我们想把某个端口隐藏起来的时候，就可以在防火墙上阻止那个端口访问，然后再开一个不规则的端口，之后配置防火墙的端口转发，将流量转发过去。
2. 端口转发还可以做流量分发，一个防火墙拖着好多台运行着不同服务的机器，然后用防火墙将不同端口的流量转发至不同机器。





## Shell

`sh` `csh` `ksh`

### 配置

切换shell

```shell
chsh
```

选择学习bash

- 默认
- 好用

制作sh文件

```sh
#!/bin/bash
#执行的路径👆
#注释
ls
```

给权限

```powershell
chmod +x test.sh
```

运行

```shell
./test.h
```

bug

```shell
bash -x test.sh
```

==必须在当前文件才能运行==

### 环境变量

`echo $PATH`

### 执行文件

`message='hello world'`

- message是变量
- 'hello world'是值
- 等于不能有空格

echo:显示内容

- 如果要插入换行符`\n`就要使用`-e`
- 显示变量需要变量前加上`$`

单引号把所有的特殊符号无效

双引号不无效`、$、?



### read读取

`-p`描述

`-n 4`写四个字

`-t 4`4秒

`-s`隐藏字符

```sh
read firstname Lastname
echo "Hello $firstname $Lastname"
read -p "Plase press your name: " -t 4 name
echo -e "\nHello $name"
```

### let

```sh
let "a = 5*3"
echo "$a" #15
```

### if语句

基本格式

```powershell
if []
then
  做这个
else
  做那个
fi
```

- `[]`两边要空一格如`[ test ]`

```powershell
if []
  事情1
elif []
then
  事情2
  ...
fi
```

条件

字符类

| 条件                 | 意义                 |
| -------------------- | -------------------- |
| $string1 = $string2  | 看两个字符是否相等   |
| $string1 != $string2 | 看两个字符是否不相等 |
| -z $String           | 判断是否为空，zore   |
| -n $String           | 判断是否不为空，not  |
| $num  -eq $num2      | 等于equal            |
| $num1 -ne $num2      | 不等于not equal      |
| $num1 -lt $num2      | 小于lower than       |
| $num1 -le $num2      | 小于等于lower equal  |
| $num1 -gt $num2      | 大于gather than      |
| $num1 -ge $num2      | 大于等于gather equal |



| 条件        | 意义                                                         |
| -------------------- | -------------------- |
| [ -a FILE ] | 如果 FILE 存在则为真。                                       |
| [ -d FILE ]            | 如果 FILE 存在且是一个目录则返回为真。directory              |
| [ -e FILE ]            | 如果 指定的文件或目录存在时返回为真。exist                   |
| [ -L FILE ] | 文件是否为Link |
| [ -f FILE ]            | 如果 FILE 存在且是一个普通文件则返回为真。file               |
| [ -r FILE ]            | 如果 FILE 存在且是可读的则返回为真。read                     |
| [ -w FILE ]            | 如果 FILE 存在且是可写的则返回为真。（一个目录为了它的内容被访问必然是可执行的）write |
| [ -x FILE ]            | 如果 FILE 存在且是可执行的则返回为真。executable             |
| [ $file1 -nt $file2 ] | 文件file1是否比file2更新。nt是new than |
| [ $file1 -nt $file2 ] | 文件file1是否比file2更旧。 |

### case

```powershell
case $1 in
    "dog")
      语句1
      ;;
    "cat")
      语句2 
      ;;
    *)
      默认语句
      ;;
esac
```

```shell
#!/bin/bash

case $1 in
    "dog" |"cat" |"pig")
        echo "It is a mammal"
        ;;
    "pigeon")
        echo "It is a bird"
        ;;
    *)
        echo  "I do not know what it is"
        ;;
esac
```

```

```

### 函数

```shell
function 函数名(){
	函数体
}

函数名(){

}
```



```shell
#!/bin/bash

print_something () {
        echo go $1
}

print_something dsf
```



```shell
#!/bin/bash

catsome(){
    cat $1
}

result=$(catsome $1)

echo "Its result is $result"
```

### 局部变量

```sh
local var1="local1" #局部变量
```

重载

```
command ls
```





## 开发集成

### java

搜索java版本

```shell
yum search java | grep openjdk
```

### tomcat搭建与部署

```shell
systemctl status tomcat
systemctl start tomcat
systemctl enable tomcat
rpm -ql tomcat #查看配置文件
/etc/tomcat/server.xml
/etc/tomcat/tomcat.conf #配置路径
yum install tomcat-webapps tomcat-admin-webapps
yum install tomcat-docs-webapp tomcat-javadoc
firewall-cmd --zone=public --add-port=8080/tcp --permanent
#打开8080的口
firewall-cmd --reload
```



设置账户

```
vim /etc/tomcat/tomcat-users.xml
systemctl restart tomcat
```



```
semanage fcontext -a -t tomcat_var_lib_t jenkins.rar
restorecon -Rv .
/var/lib/tomcat/webapps/jenkins

```



```shell
chown tomcat:tomcat /var/lib/jenkins/
[root@localhost tomcat]# chown tomcat:tomcat /var/lib/jenkins/
[root@localhost tomcat]# vim /etc/tomcat/context.xml 
```



```xml
<Context>

    <Environment name="JENKINS_HOME" value="/var/lib/jenkins" type="java.lang.String"/>
</Context>
```



```shell
vim /var/lib/jenkins/hudson.model.UpdateCenter.xml #改成http
```



#### 多个tomcat的部署

修改conf/server.xml的端口port

```xml
<Server port="80" shutdown="SHUTDOWN">
<Connector port="8080" protocol="HTTP/1.1"
               connectionTimeout="20000"
               redirectPort="8443" />
<Connector port="8009" protocol="AJP/1.3" redirectPort="8443" />
```



### 反向代理(Nginx)



> 正向代理：建立在客户机的
>
> 反向代理：建立在服务器,用于集群服务器，隐藏服务器的信息

[Nginx相关介绍](https://www.cnblogs.com/wcwnina/p/8728391.html)

淘宝的[nginx](https://www.nginx.com/)的[https://tengine.taobao.org/]

apache是堵塞型的

排名[https://www.similartech.com/categories/web-server]



安装

```shell
yum install epel-release
yum install nginx
```

使用

```powershell
systemctl status nginx
```

配置

nginx.conf

```powershell
upstream server_lb{ #服务器列表
	server localhost:8080; #weight = 10 权值 
	server localhost:8081;
	# ip_hash; 专一模式
}
server{
	location / {
		proxy pass 代理
	}
}
```

共享session

- 一个机子，只使用一个tomcat

- 共享：

  - 广播式(集群) (linux上会出错)

    ```xml
    <!-- tomcat的配置server。xml -->
    <Cluster className="org.apache.catalina.ha.tcp.SimpleTcpCluster"/>
    <!--在项目的web.xml中加入-->
    <distributable/>
    ```

  - redis

## 磁盘管理

### 存储

df:报告文件

df -h

进程

 w 

free:显示系统的使用情况

- m 按m算
- g 按g算

top 显示进程

### 版本

uname -a

cat /proc/cpuinfo

cat /proc/meminfo



<!-- 11.1-11.5 35：09 -->

### 磁盘

SSD:固态硬盘

HHD：机械硬盘







# 写在后面

## 屏幕适应

```powershell
yum install xorg-x11-drv-vmware
 
yum install open-vm-tools
 
yum install open-vm-tools-desktop
```

## 网络连接

```powershell
前两部已经有大量博客说明 但是往往不能真正解决问题
在安装好centos7后 不能连接外网的主要原因是启动的时候没有启动网卡 解决方法是：
** cd /etc/sysconfig/network-scripts/
**ls
**找到 ifcfg-ensxxxx文件
**使用 vi打开 vi ifcfg-ens33 将其中的ONBOOT=on  改为yes
** 按‘退出’ + ‘：’+wq 保存并退出
**重启动网络 service network restart
```



## 开机报错

### centos 虚拟机出问题 Oh no,something has gone wrong! 解决方法

然后ctrl+alt+F2 进入命令模式，然后输入root 账号和密码。

```
root
输入密码
yum update
...等待
输入 y
...等待
就可以了
```

[https://www.365jz.com/article/24845]





# 附录

## 我的Centos的配置

### centos7

基本上都在`/usr/local/`文件夹中

tomcat redis zookeeper

java:使用yum



