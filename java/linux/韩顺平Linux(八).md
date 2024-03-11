# 1、Linux之Python定制篇-Python开发平台Ubuntu

- Ubuntu(友帮拓、优般图、乌班图)是一个以桌面应用为主的开源GNU/Linux操作系统，Ubuntu是基于GNU/Linux,
  支持x86、amd64(即x64)和ppc架构，由全球化的专业开发团队(Canonical Ltd)打造的。
- 专业的Python开发者一般会选择Ubuntu这款Linux系统作为生产平台，
- Ubuntu和Centos都是基于GNU/Linux内核的，因此基本使用和Centos是几乎一样的，它们的各种指令可以通
  用，同学们在学习和使用Ubuntu的过程中，会发现各种操作指令在前面学习CentOS都使用过。只是界面和预安装的软
  件有所差别。
- Ubuntu下载地址：https://cn.ubuntu.com/download

![](韩顺平Linux(八).assets/1.png)









## 1.1、安装Ubuntu



![](韩顺平Linux(八).assets/2.png)



![](韩顺平Linux(八).assets/3.png)



![](韩顺平Linux(八).assets/4.png)





![](韩顺平Linux(八).assets/5.png)





![](韩顺平Linux(八).assets/6.png)





![](韩顺平Linux(八).assets/7.png)



![](韩顺平Linux(八).assets/8.png)



- 用户名：qxl，密码：`qxl666nb`

![](韩顺平Linux(八).assets/9.png)





![](韩顺平Linux(八).assets/10.png)



完成后重启，即可安装完成。



## 1.2、Ubuntu的root用户

安装`ubuntu`成功后，都是普通用户权限，并没有最高root权限，如果需要使用root权限的时候，通常都会在命令前面加上`sudo`。有的时候感觉很麻烦。

```bash
# 显示当前分区情况(权限不够)
fdisk -l

# 使用 root 权限 
sudo fdisk -l
```

![](韩顺平Linux(八).assets/11.png)

我们一般使用su命令来直接切换到root用户的`su root`，但是如果没有给root设置初始密码，就会抛出`su:Authentication failure`这样的问题。所以，我们只要给root用户设置一个初始密码就好了。

1. 输入命令设置 root密码：`qxl666nb`

```bash
# 输入命令设置 root密码
sudo passwd

# 切换到root
su root
```

![](韩顺平Linux(八).assets/12.png)

2. 输入`exit`命令，退出 root 并返回一般用户





## 1.3、Ubuntu下开发Python

安装好 Ubuntu 后，默认就已经安装好 Python的开发环境：`python3`

![](韩顺平Linux(八).assets/13.png)

```bash
# 编写 hello.py
vim hello.py
# 运行 hello.py
python3 hello.py
```

## 1.4、APT软件管理和远程登录

apt是Advanced Packaging Tool的简称，是一款安装包管理工具。在Ubuntu下，我们可以使用apt命令进行软件包的安装、删除、清理等，类似于Windows中的**软件管理工具**。

![](韩顺平Linux(八).assets/14.png)



Ubuntu系统上`/etc/apt/sources.list` 存了很多美国服务器地址，可以使用 `apt`命令完成很多软件的安装、更新和卸载，那它肯定要走网络，所以需要网关。我们加了镜像网站，`sources.list` 我们指向镜像网站，这样我们访问就很快了。





## 1.5、Ubuntu软件操作的相关命令

```bash
# 更新源(重要)
sudo apt-get update

# 安装软件包(重要)
sudo apt-get install package

# 删除软件包(重要)
sudo apt-get remove package

# 搜索软件包

# 获取软件包的相关信息,如大小、说明、版本等(重要)
sudo apt-cache show package

# 重新安装包
sudo apt-get install package --reinstall

# 修复安装
sudo apt-get -f install

# 删除包,包括配置文件等
sudo apt-get remove package --purge

# 安装相关的编译环境
sudo apt-get build-dep package

# 更新已安装的包
sudo apt-get upgrade

# 升级系统
sudo apt-get dist-upgrade 

# 了解使用该包依赖哪些包
sudo apt-cache depends package

# 查看该包被哪些包依赖
sudo aot-cache rdepends package

# 下载该包的源代码(重要)
sudo apt-get source package
```



1. 进入清华大学镜像源：https://mirrors.tuna.tsinghua.edu.cn/，找到`ubuntu` ，点击问号

![](韩顺平Linux(八).assets/15.png)

2. 输入如下命令：备份 Ubuntu 默认的源地址

```bash
sudo cp /etc/apt/sources.list /etc/apt/sources.list.backup
```

3. 切换root用户

```bash
su
```

4. 清空`sources.list`文件

```bash
# 把空输入到 sources.list 文件
echo '' > sources.list
```

5. 复制镜像网站的地址

![](韩顺平Linux(八).assets/16.png)





6. 拷贝到`sources.list`文件

```bash
vi sources.list

# 粘贴进去

# 保存文件 :wq
```

7. 更新源地址

```bash
sudo apt-get update
```



## 1.6、安装vim

通过 vim 来示例软件的安装、删除

```bash
# 安装vim
sudo apt-get install vim

# 删除vim
sudo apt-get remove vim

# 获取软件信息
sudo apt-cache show vim
```



# 2、远程登录Ubuntu

使用SSH服务，需要安装相应的服务器和客户端。

- 客户端和服务器的关系：如果，A机器想被B机器远程控制，那么，A机器需要安装SSH服务器，B机器需要安装SSH客户端。
- 和CentOS不一样，Ubuntu默认没有安装SSHD服务，使用 netstat 查看，会发现没有监听22端口，所以需要先安装SSH

```bash
netstat -anp | more

# 如果没有netstat命令可以安装
apt install net-tools
```

![](韩顺平Linux(八).assets/17.png)





## 2.1、安装SSH

```bash
# 安装指令:执行这条指令,在这台Linux就安装了SSH服务端和客户端
sudo apt-get install openssh-server

# 启动sshd服务,会监听22端口
service sshd restart
```

## 2.2、连接

我们使用Xshell来连接我们的Ubuntu:

![](韩顺平Linux(八).assets/18.png)





## 2.3、从一台Linux系统远程登录另外一台linux

在创建服务器集群时,会使用到该技术。基本语法：

```bash
ssh 用户名@IP

# 示例
ssh qxl@192.168.6.138
```



使用ssh 访问，如果访问出现错误，可以查看是否有 `~/.ssh/known_ssh` 尝试删除该文件解决，一般不会有问题。























