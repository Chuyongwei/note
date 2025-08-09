# 正文

安装系统

[系统镜像](https://blog.csdn.net/weixin_45498383/article/details/131047312)

[【记录】重装Ubuntu系统，格式化根目录、保留home数据_ubuntu重装系统保留数据-CSDN博客](https://blog.csdn.net/qq_46248455/article/details/133818175)

简单来说就是

## 安装软件

### apt

```shell
sudo apt install 程序包 # 安装软件
sudo apt update  # 更新
sudo apt upgrade # 更新
sudo apt remove 程序包 # 删除软件
```

### deb

1. 安装某个.deb文件，简单的在.deb文件上Right单击鼠标，然后选择Kubuntu Package Menu->安装软件包。

2. 或者，你可以在终端下输入如下内容来安装.deb文件

   ```shell
   sudo dpkg -i 软件包名.deb
   ```

3. 卸载.deb文件，可以使用Adept，或输入：

   ```shell
   sudo apt-get remove 软件包名称
   ```

## 防火墙

### systemctl

```powershell
systemctl status xxx
systemctl restart xx
systemctl stop xxx
systemctl enable xxx
```

### ufw

```powershell
sudo ufw allow 22/tcp
sudo ufw status
sudo ufw delete
```

### windows访问

> 在控制面版->程序中添加试验功能`telnet`

```sh
telnet [ipaddr] [port]
```

**查看是否监听**

> 如果没有监听，你检测也检测不到\~_\~

win

```bash
netstat -aon|findstr "9050"
```

ubuntu

```bash
netstat -ap | grep 8080
```



## 显示模式

桌面环境

```shell
 ps -A | egrep -i "gnome|kde|mate|cinnamon|lx|xfce|jwm"
```

显示模式

```shell
cat /etc/X11/default-display-manager
```

安装vnc

[常用链接](https://help.aliyun.com/zh/simple-application-server/use-cases/use-vnc-to-build-guis-on-ubuntu-18-04-and-20-04#21e0b772d7fgc)

目前可能存在问题

- Ubuntu18.04版本太旧不再优化
- 连接显示器导致display：0占用问题
- 桌面环境问题

x11vnc法

```bash
x11vnc -display :0 -forever  -shared -rfbauth ~/.vnc/passwd
```

### 修改内存

临时修改

```bash
ulimit -n  # 查看当前限制（默认可能是1024）
ulimit -n 65536 # 临时提高限制（仅当前会话有效）
```

永久修改（需重启生效）：

```bash
echo "web603 soft nofile 65536" | sudo tee -a /etc/security/limits.conf
echo "web603 hard nofile 65536" | sudo tee -a /etc/security/limits.conf
```



## 安装输入法



```shell
sudo apt install fcitx
```

修改打字法

```shell
im-config
```

## pytorch

1. 英伟达驱动

   删除驱动

   ```sh
   sudo apt purge nvidia-*  # 清除旧驱动
   sudo reboot
   ```

   安装驱动

   > 一般从官网上下载run文件然后`sh xxx.run`安装驱动

   自动检测驱动

   ```sh
   ubuntu-drivers devices
   ```

   

2. cuda cudnn

   

3. anconda

4. 换源

5. pytorch

   

# 后话

关于旧版本的问题

- cuda 11+
- pytorch 1.6
- python 1.9





## conda

（https://blog.csdn.net/JineD/article/details/129507719）

## 安装VNC



```shell
sudo apt install x11vnc

# nohup x11vnc -display :0 -forever -shared -rfbauth ~/.vnc/passwd
x11vnc -display :0 -forever -shared -rfbauth ~/.vnc/passwd
```

### x11vnc

安装

```bash
sudo apt-get install x11vnc
```

创建密码

```bash
 sudo x11vnc -storepasswd

 Enter VNC password: *********

 Verify password: *********

 Write password to ~/.vnc/passwd? [y]/n y

 Password written to: ~/.vnc/passwd
```

### 手动测试

```bash
sudo x11vnc -auth guess -once -loop -noxdamage -repeat -rfbauth /home/USERNAME/.vnc/passwd -rfbport 5901 -shared
```

==修改路径`/home/USERNAME/.vnc/passwd`==

崩溃运行

```bash
x11vnc -display :0 -forever -shared -rfbauth ~/.vnc/passwd -noxfixes -noxrecord -noxdamage -nowf -nowcr
```

**参数说明**：

- `-noxfixes`：禁用光标形状更新（解决 `initialize_xfixes` 问题）。
- `-noxrecord`：禁用键盘事件记录（避免输入冲突）。
- `-noxdamage`：禁用屏幕损坏检测（降低资源占用）。
- `-nowf`：禁用 `Xinerama` 多显示器支持。
- `-nowcr`：禁用光标渲染。

**配置日志的时候**

```sh
 -logfile /var/log/x11vnc.log
```

#### 查看访问端口的主机数量

```sh
 # 查找 x11vnc 的监听端口（通常是 5900）
sudo netstat -tulpn | grep x11vnc

# 查看连接到该端口的客户端数量（替换 5900 为实际端口）
 sudo ss -tunp state established sport = :5900 | grep -v "State" | wc -l
```

其中`wc -l`是指行数

或者

```sh
sudo ss -tanp | grep "$(pgrep x11vnc)" | grep ESTAB | wc -l
```





### 设置开机自启

```bash
sudo vim /etc/systemd/system/x11vnc.service
```

```ini
[Unit]
Description=x11vnc (Remote access)
After=network-online.target

[Service]
Type=simple
ExecStart=/usr/bin/x11vnc -auth guess -display :0 -rfbauth /home/USERNAME/.vnc/passwd -rfbport 5901 -forever -loop -noxdamage -repeat -shared -capslock -nomodtweak
ExecStop=/bin/kill -TERM $MAINPID
ExecReload=/bin/kill -HUP $MAINPID
KillMode=control-group
Restart=on-failure

[Install]
WantedBy=graphical.target
```

**Note**

- ExecStart处的`/home/USERNAME/.vnc/passwd`修改为自己的路径

运行

```bash
$ sudo systemctl daemon-reload
$ sudo systemctl enable x11vnc
$ sudo systemctl start x11vnc
```

x11vnc -display :0 -auth ~/.Xauthority
