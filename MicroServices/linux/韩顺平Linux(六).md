# 1、VMware虚拟机安装宝塔

1. 进入宝塔官网：[宝塔面板 - 简单好用的Linux/Windows服务器运维管理面板 (bt.cn)](https://www.bt.cn/new/index.html)
2. 注册、登录、进入会员后台

![](韩顺平Linux(六).assets/1.png)





2. 点击安装宝塔

![](韩顺平Linux(六).assets/2.png)



3. 首先挂载磁盘

![](韩顺平Linux(六).assets/3.png)

4. 使用官方一键挂载脚本

```bash
yum install wget -y && wget -O auto_disk.sh http://download.bt.cn/tools/auto_disk.sh && bash auto_disk.sh
```

![](韩顺平Linux(六).assets/4.png)



5. 因为我只有一块磁盘，所以无法挂载，建议不论是不是一块磁盘，都执行上述命令

![](韩顺平Linux(六).assets/5.png)



6. 安装宝塔

```bash
yum install -y wget && wget -O install.sh http://download.bt.cn/install/install_6.0.sh && sh install.sh ed8484bec
```

![](韩顺平Linux(六).assets/6.png)



![](韩顺平Linux(六).assets/7.png)



> 这里注意使用 root 用户进行安装，大概 5min 左右

7. 安装完毕以后则会出现一个外网面板地址和内网面板地址的界面！自己保存复制下来

![](韩顺平Linux(六).assets/8.png)



8. 安装网络端口扫描软件：`yum install nmap`

![](韩顺平Linux(六).assets/9.png)

9. 查看端口号：`nmap 127.0.0.1`  ，有22端口就好

![](韩顺平Linux(六).assets/10.png)



10. 在Windows的浏览器登入内网地址，输入账号密码。一键安装环境

> 进入后会提示打开面板SSL、更新宝塔面板，点点点就OK了

![](韩顺平Linux(六).assets/11.png)



![](韩顺平Linux(六).assets/12.png)





11. `nmap 127.0.0.1` 查看端口命令，可以看到端口已经开启

![](韩顺平Linux(六).assets/13.png)



12. 在软件安装商店可以选择需要的软件安装，
    - Tomcat : 注意安装了Tomcat会自动安装 jdk1.8
    - 

![](韩顺平Linux(六).assets/14.png)



13. 在XShell中查看安装的软件版本号

```bash
# 查看nginx版本
nginx -V

# 查看mysql版本
mysql --version

# 查看java版本
java -version

# 查看php版本
php -v
```

> 注：宝塔安装的软件默认在 `根目录/www/service` 里面

> ==注：宝塔修改安全入口、端口后一定也要在防火墙进行开放端口，否则就进不去内网地址==
>
> ==注：宝塔修改安全入口、端口后一定也要在防火墙进行开放端口，否则就进不去内网地址==
>
> ==注：宝塔修改安全入口、端口后一定也要在防火墙进行开放端口，否则就进不去内网地址==



## 1.1、宝塔面板地址忘掉

1. 在XShell 中输入 `bt` ，输入`14`

![](韩顺平Linux(六).assets/15.png)











## 1.2、宝塔内网地址进不去

1. 我在宝塔设置面板端口和安全入口后内网地址进不去

![](韩顺平Linux(六).assets/16.png)



> 解决办法如下：

1. 查询对外开放的防火墙端口集合

```bash
firewall-cmd --zone=public --list-ports
```

2. 查询指定端口是否已经打开

```bash
firewall-cmd --query-port=8888/tcp
# 提示 yes，表示开启；no表示未开启。
```

3. 添加需要开放的端口

```bash
firewall-cmd --add-port=8888/tcp --permanent

# 刷新
firewall-cmd --reload
```

扩展：

- 开关防火墙

```bash
# 查看防火墙状态
systemctl status firewalld

# 开启防火墙
systemctl start firewalld

# 关闭防火墙
systemctl stop firewalld
```







































































