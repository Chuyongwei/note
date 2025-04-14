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



### 安装输入法



```shell
sudo apt install fcitx
```

修改打字法

```shell
im-config
```

## pytorch

1. 英伟达驱动

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

