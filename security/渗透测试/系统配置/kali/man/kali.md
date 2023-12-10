# [kali](https://www.kali.org/)

## 准备

kali-linux-installer-amd64.iso

> https://www.kali.org/get-kali/#kali-platforms

**服务器**

vultr

> https://www.vultr.com/

**虚拟机**

vmware

> https://www.vmware.com/

directory

```
D:\documents\virtual machines\#-kali-#
├─Kali-Mirror
│      kali-linux-installer-amd64.iso
│
└─Kali-VM
```

## 
## 安装

**服务器**

在 `Server Image` 选择 `Upload ISO` 

勾选 `My ISOs `

点击 `Upload ISO` 

在 `Upload ISO from remote machine` 中添加 `kali-linux-installer-amd64.iso` 的 `url` 

> https://cdimage.kali.org/kali/kali-linux-installer-amd64.iso 

点击 `Upload` 

等待 `ISOs` 中镜像的 `Status` 变为 `Available` 

在 `My ISOs` 中选中 上传的镜像，配置硬件后点击 `Deploy Now` 

等待安装完成后点击 `Running` 后进入 `View Console` 进行部署

**虚拟机**

您希望使用什么类型的配置

> 自定义（高级）

硬件兼容性

> Workstation 17.5.x

安装客户机操作系统

> 稍后安装操作系统

客户机操作系统

> Linux

版本

> Debian 12.x 64 位

虚拟机名称

> `#-kali-#` 

位置

> D:\documents\virtual machines\\#-kali-#\kali-vm

处理器数量

> 1

每个处理器的内核数量

> 4

此虚拟机的内存

> 8 GB

网络连接

> 使用桥接网络

选择 I/O 控制器类型

> LSI Logic

虚拟磁盘类型

> SCSI

选择磁盘

> 创建新虚拟磁盘

最大磁盘大小

> 1024 GB
>
> 将虚拟磁盘存储为单个文件

磁盘文件

> D:\documents\virtual machines\\#-kali-#\kali-vm\kali.vmdk

完成

> 编辑虚拟机设置

处理器

> 虚拟化 Intel VT-X/EPT 或 AMD-V/RVI(V)

使用 ISO 映像文件

> D:\documents\virtual machines\\#-kali-#\kali-mirror\kali-linux-installer-amd64.iso

USB 兼容性

> USB 3.1

显示器

> 关闭加速 3D 图形

快照

> 拍摄新快照
>
> 确定

拍摄快照，命名为 `安装` ，点击 `开启此虚拟机` 后进行部署

## 部署

Kali Linux installer menu

> Install

Select a language

> 中文（简体）

请选择您的位置

> 中国

配置键盘

> 美式英语

主机名

> `#-kali-#` 

域名

> 

请输入新用户的全名

> sec

您的账户的用户名

> sec

请为新用户选择一个密码

> 

请再次输入密码以验证其正确性

> 

分区方法

> 使用整个磁盘并配置 LVM

请选择要分区的磁盘

> /dev/

分区方案

> 将所有文件放在同一个分区中

将修改写入磁盘并配置 LVM

> 是

可用于分区向导的卷组的数量

> 默认

将改动写入磁盘

> 是

软件选择

> 图形化终端 默认安装
>
> 命令行终端 取消 Desktop Xfce 安装

将 GRUB 启动引导器安装至您的主驱动器

> 是

安装启动引导器的设备

> /dev/

请选择 <继续> 以重新启动

> 服务器安装要在 Settings 中的 Custom ISO 点击 Remove ISO 移除镜像
>
> 继续

关机，快照命名为 `部署`

## 初始化

**切换为 `root` 用户**

设置 `root` 密码

```shell
┌──(sec㉿#-kali-#)-[~]
└─$ sudo passwd root
[sudo] sec 的密码：
新的密码：
重新输入新的密码：
已成功更新密码
```

切换为 `root` 用户

```shell
┌──(sec㉿#-kali-#)-[~]
└─$ su - root
Password:
┌──(root㉿#-kali-#)-[~]
└─#
```

**更改用户界面为中文**

> 命令行终端不需要

```shell
┌──(root㉿#-kali-#)-[~]
└─$ sudo dpkg-reconfigure locales
```

```
  [*] zh_CN.UTF-8 UTF-8   
  zh_CN.UTF-8 
```

> 重启，使用 `root` 登录
>
> 勾选不要再次询问我
>
> 保留旧的名称

**开启 `ssh` 服务**

> 需要关闭防火墙

允许远程登录 `root`

```shell
┌──(root㉿#-kali-#)-[~]
└─# vim /etc/ssh/sshd_config
```

```
33	#PermitRootLogin prohibit-password
33	PermitRootLogin yes
```

设置 `ssh` 开机自启

```shell
┌──(root㉿#-kali-#)-[~]
└─# sudo systemctl enable ssh --now && sudo systemctl status ssh
```

禁止 `ssh` 开机自启

```shell
┌──(root㉿#-kali-#)-[~]
└─# sudo systemctl disable ssh --now && sudo systemctl status ssh
```

开启 `ssh` 

```shell
┌──(root㉿#-kali-#)-[~]
└─# sudo systemctl start ssh && sudo systemctl status ssh
```

关闭 `ssh` 

```shell
┌──(root㉿#-kali-#)-[~]
└─# sudo systemctl stop ssh && sudo systemctl status ssh
```

**配置密钥连接**

使用 `xshell` 生成密钥，将公钥添加到目标服务器

```shell
┌──(root㉿#-kali-#)-[~]
└─# vim ~/.ssh/authorized_keys
```

禁止密码验证连接

```shell
┌──(root㉿#-kali-#)-[~]
└─# vim /etc/ssh/sshd_config
```

```
57	#PasswordAuthentication yes
57	PasswordAuthentication no
```

> 重启后生效

**配置 `apt` 源**

用 `vim` 打开文件

```shell
┌──(root㉿#-kali-#)-[~]
└─# vim /etc/apt/sources.list
```

```
# deb http://http.kali.org/kali kali-rolling main contrib non-free non-free-firmware

deb https://mirrors.ustc.edu.cn/kali kali-rolling main non-free contrib
deb-src https://mirrors.ustc.edu.cn/kali kali-rolling main non-free contrib

deb http://mirrors.ustc.edu.cn/debian/ buster main contrib non-free
deb-src http://mirrors.ustc.edu.cn/debian/ buster main contrib non-free
```

获取更新

```shell
┌──(root㉿#-kali-#)-[~]
└─# sudo apt update
```

更新系统

```shell
┌──(root㉿#-kali-#)-[~]
└─# sudo apt full-upgrade -y
```

清理缓存

```shell
┌──(root㉿#-kali-#)-[~]
└─# sudo apt clean
```

> 重启

**配置 `pip`**

更新 `pip`

```shell
┌──(root㉿#-kali-#)-[~]
└─# pip install -i https://mirrors.ustc.edu.cn/pypi/web/simple pip -U
```

更新配置文件

```shell
┌──(root㉿#-kali-#)-[~]
└─# mkdir ~/.pip && vim ~/.pip/pip.conf
```

```
[global]
index-url = https://pypi.mirrors.ustc.edu.cn/simple
[install]
trusted-host = https://pypi.mirrors.ustc.edu.cn
```

查看 `pip` 源

```shell
┌──(root㉿#-kali-#)-[~]
└─# pip3 config list
```

```
global.index-url='https://pypi.mirrors.ustc.edu.cn/simple'
install.trusted-host='https://pypi.mirrors.ustc.edu.cn'
```

**配置 `git` 代理**

配置代理

```shell
┌──(root㉿#-kali-#)-[~]
└─# git config --global url."https://mirror.ghproxy.com/https://github.com/".insteadOf https://github.com/ && git config --global --get-regexp "url\..*\.insteadOf"
```

**配置 `DNS`**

修改配置文件

```shell
┌──(root㉿#-kali-#)-[~]
└─# vim /etc/resolv.conf
```

```
# Generated by NetworkManager
nameserver 8.8.8.8
nameserver 1.1.1.1
```

更新缓存并测试网络

```shell
┌──(root㉿#-kali-#)-[~]
└─# sudo service networking restart && ping g.cn -c 3
```

**配置 `mysql`**

运行服务

```shell
┌──(root㉿#-kali-#)-[~]
└─# sudo systemctl start mysql
```

修改密码

```shell
┌──(root㉿#-kali-#)-[~]
└─# mysqladmin -u root password
New password: 
Confirm new password:
```

**配置 `proxychains4`**

> 服务器安装则不需要

修改配置文件

```shell
┌──(root㉿#-kali-#)-[~]
└─# vim /etc/proxychains4.conf
```

```
# socks4	127.0.0.1 9050
socks5		192.168.1.7 10808
```

测试代理

```shell
┌──(root㉿#-kali-#)-[~]
└─# proxychains4 ping -c3 google.com
```

**配置防火墙**

安装 `ufw` 

```shell
┌──(root㉿#-kali-#)-[~]
└─# sudo apt install -y ufw
```

开启防火墙

```shell
┌──(root㉿#-kali-#)-[~]
└─# sudo ufw enable
Firewall is active and enabled on system startup
```

关闭防火墙

```shell
┌──(root㉿#-kali-#)-[~]
└─# sudo ufw disable
Firewall stopped and disabled on system startup
```

**创建目录**

```shell
┌──(root㉿#-kali-#)-[~]
└─# mkdir /root/tools
```

**图形化终端系统设置**

窗口管理器

> 样式
>
> > 主题：Kali-Light

外观

> 样式：Kali-Light
>
> 图标：Flat-Remix-Blue -Light

Qt5 设置

> 外观
>
> > 颜色方案：Kali-Light
>
> 图标主题：Flat-Remix-Blue -Light

终端参数配置

> 字体大小：12
>
> 终端颜色：Kali-Light

桌面设置

> 桌面壁纸：kali-layers-16x9.png

鼠标和触摸板

> 主题
>
> > Adwaita
> >
> > 大小：36

Firefox

> Language 简体中文
>

电源管理器

> 系统
>
> > 系统睡眠模式：挂起
> >
> > 在闲置时：从不
> 
> 显示
> 
> > 转入黑屏状态时间：从不
> >
> > 转入休眠状态时间：从不
> > 
> > 转入关闭状态时间：从不
> 
> 安全性
> 
> > 自动锁定会话：从不
>>
> > 在屏保之后延迟锁定：1 秒
>>
> > 勾选当系统休眠时锁定屏幕

电源按钮

> 关闭保存会话用于将来登录

关机，快照命名为 `初始化` 

卧底模式

```shell
┌──(root㉿#-kali-#)-[~]
└─# kali-undercover
```

> 鼠标和触摸板
>
> Windows 10
>
> 大小：46

## tools

获取更新

```shell
┌──(root㉿#-kali-#)-[~]
└─# sudo apt update
```
