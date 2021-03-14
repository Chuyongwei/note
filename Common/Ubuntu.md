## 安装

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

### 安装输入法



```shell
sudo apt install fcitx
```



修改打字法

```shell
im-config
```



安装