# Linux实操篇

# 1、进程管理



## 1.1、基本介绍

1. 在Linux 中，每个执行的程序都称为一个进程，每一个进程都分配一个ID号(pid,进程号)
2. 每个进程都可能以两种方式存在。前台与后台，所谓前台进程就是用户目前的屏幕上可以进行操作的。后台进程则是实际在操作，但由于屏幕上无法看到的进程，通常使用后台方式执行。
2. 一般系统的服务都是以后台进程的方式存在，而且都会常驻在系统中。直到关机才结束。

## 1.2、显示系统执行的进程

基本命令：`ps`

- `ps -a` ：显示当前终端的所有进程信息
- `ps -u`：以用户的格式显示进程信息
- `ps -x` ： 显示后台进程运行的参数

一般这些选项都会组合使用

```
ps -aux
```

![](韩顺平Linux(四).assets/1.png)

### 1.2.1、ps详解

`ps -aux | grep xxx` 

| System V | 展示风格                                                     |
| -------- | ------------------------------------------------------------ |
| USER     | 用户名称                                                     |
| PID      | 进程号                                                       |
| %CPU     | 进程占用CPU的百分比                                          |
| %MEM     | 进程占用物理内存的百分比                                     |
| VSZ      | 进程占用的虚拟内存大小(单位：KB)                             |
| RSS      | 进程占用的物理内存大小(单位：KB)                             |
| TT       | 终端名称，缩写                                               |
| STAT     | 进程状态：其中S-睡眠，s-表示该进程是会话的先导进程，N-表示进程拥有比普通优先级更低的优先级，R-正在运行，D-短期等待，Z-僵死进程，T-被跟踪或者被停止等待 |
| STARTED  | 进程的启动时间                                               |
| TIME     | CPU时间，即进程使用CPU的总时间                               |
| COMMAND  | 启动进程所用的命令和参数(执行该进程的指令)，如果过长会被截断显示 |





### 1.2.2、应用实例

1. 以全格式显示当前所有的进程，查看进程的父进程。查看 sshd 的父进程信息

   `ps -ef` 是以全格式显示当前所有的进程，`-e` 显示所有进程，`-f` 全格式

```
ps -ef | grep sshd
```



![](韩顺平Linux(四).assets/2.png)



| UID   | 用户ID                                                       |
| ----- | ------------------------------------------------------------ |
| PID   | 进程ID                                                       |
| PPID  | 父进程ID                                                     |
| C     | CPU用于计算执行优先级的因子。数值越大，表明进程是CPU密集型运算，执行优先级会降低；数值越小，表明进程的I/O密集型运算，执行优先级会提高 |
| STIME | 进程启动的时间                                               |
| TTY   | 完整的终端名称                                               |
| TIME  | CPU时间                                                      |
| CMD   | 启动进程所用的命令和参数                                     |





## 1.3、终止进程kill和killall

若是某个进程执行一半需要停止时，或是已消耗了很大的系统资源时，此时可以考虑停止该进程。使用`kill` 命令来完成此项任务。

基本语法：

- `kill [选项] 进程号` (功能描述：通过进程号杀死/终止进程)
- `killall 进程名称` (功能描述：通过进程名杀死进程，也支持通配符，这在系统因负载过大而变得很慢时很有用)

常用选项： `-9` 表示强迫进程立即停止。



1. 踢掉某个非法登录用户
   - `kill` 进程号
   - 例如我们登录tom的用户，然后查看tom登录对应的进程号，然后踢出tom

![](韩顺平Linux(四).assets/3.png)

![](韩顺平Linux(四).assets/4.png)

```
kill 9396
```



2. 终止远程登录服务 sshd，在适当的时候再次重启 sshd 服务
   - `kill sshd对应的进程号`

![](韩顺平Linux(四).assets/5.png)



我们终止sshd之后，使用XShell就连接不上我们的Linux虚拟机了。重启sshd指令如下：

```
/bin/systemctl start sshd.service
```





3. 终止多个 gedit

```
killall gedit
```



4. 强制杀死一个终端
   - `kill -9 对应的进程号`







## 1.4、查看进程树pstree

基本语法： `pstree[选项]` 可以更加直观的来看进程信息

常用选项：

- `-p` ： 显示进程的PID
- `-u` ：显示进程的所属用户

![](韩顺平Linux(四).assets/6.png)

1. 请你以树状的形式显示进程的pid

```
pstree -p
```

![](韩顺平Linux(四).assets/7.png)

2. 请你以树状的形式显示进程的用户

```
pstree -u
```





## 1.5、服务管理

服务(service)本质就是进程，但是是在运行在后台的，通常都会监听某个端口，等待其他程序的请求。比如(mysql、sshd、防火墙等)，因此我们又称为守护进程，是Linux中非常重要的知识点。

![](韩顺平Linux(四).assets/8.png)



在Linux中，找到服务的前提是先找到其监听的端口。



### 1.5.1、service管理指令

语法： 

```
service 服务名 [start|stop|restart|reload|status]
```

在 CentOS7.0之后，很多服务不再使用 service，而是 systemctl。保留的 service 指令命令在 `/etc/init.d` 可以查看

![](韩顺平Linux(四).assets/9.png)



1. 使用 service 指令 查看、关闭、启动 network

```
service network status

service network stop

service network start
```

### 1.5.2、查看服务名

方式一：setup 查看全部的系统服务

```
setup
```

![](韩顺平Linux(四).assets/10.png)



![](韩顺平Linux(四).assets/11.png)





方式二：

```
ls -l /etc/init.d/
```



### 1.5.3、服务的运行级别

Linux 系统有7种运行级别(runlevel)： 常用的是 级别3和5

运行级别0：系统停机状态，系统默认运行级别不能为0，否则不能正常启动

运行级别1：单用户工作状态，root权限，用于系统维护，禁止远程登录

运行级别2：多用户状态(没有NFS)，不支持网络

运行级别3：完全的多用户状态(有NFS)，无界面，登陆后进入控制台命令行模式

运行级别4：系统未使用，保留

运行级别5：登陆后进入图形GUI模式

运行级别6：系统正常关闭并重启，默认运行级别不能设为6，否则不能正常启动

开机的流程说明：

![](韩顺平Linux(四).assets/12.png)

1. 查看我们当前的运行级别

```
systemctl get-default
```

- multi-user.target : 对应运行级别3
- graphical.target: 对应运行级别5

![](韩顺平Linux(四).assets/13.png)



2. 更改我们的运行级别

```bash
# 格式：systemctl set-default TARGET.target
# 更改为运行级别5
systemctl set-default graphical.target.target
```



### 1.5.4、chkconfig指令

通过 chkconfig 命令可以给服务的各个运行级别设置自启动/关闭

1. 查看服务：`chkconfig --list[|grep xxx]`
2. `chkconfig 服务名 --list`
3. `chkconfig --level 5 服务名 on/off`

![](韩顺平Linux(四).assets/14.png)



1. 把 network 在3运行级别关闭自启动

```
chkconfig --level 3 network off
```

使用细节：chkconfig 重新设置服务后自启动或者关闭，需要重启机器 reboot 生效。





### 1.5.5、systemctl管理指令

基本语法：

```bash
systemctl [start|stop|restart|status] 服务名

# systemctl 指令管理的服务在 /usr/lib/systemd/system 查看
```



### 1.5.6、systemctl设置服务的自启动状态

- `systemctl list-unit-files [|grep 服务名]` ：查看服务开机启动状态，grep可以进行过滤
- `systemctl enable 服务名` ： 设置服务开机启动
- `systemctl disable 服务名` ：关闭服务开机启动
- `systemctl is-enabled 服务名` ：查询某个服务是否是自启动的  

```
systemctl list-unit-files | grep firewalld.service
```

![](韩顺平Linux(四).assets/15.png)

1. 查看当前防火墙的状况

```
systemctl status firewalld
```

![](韩顺平Linux(四).assets/16.png)

2. 关闭防火墙和重启防火墙

```
systemctl stop firewalld
systemctl start firewalld
```





关闭或者启动防火墙后，立即生效。这种方式只是临时生效，当重启系统后，还是回归以前对服务的设置。如果希望设置某个服务自启动或关闭永久生效，要使用 `systemctl [enable|disable] 服务名`



### 1.5.7、打开或者关闭指定端口

在真正的生产环境，往往需要将防火墙打开，但是问题来了，如果我们把防火墙打开，那么外部请求数据包就不能跟服务器监听端口通讯。这时，需要打开指定的端口，比如80、22、8080等，这个要怎么做呢？

![](韩顺平Linux(四).assets/17.png)

1. 打开端口： `firewall-cmd --permanent --add-port=端口号/协议`
2. 关闭端口：`firewall-cmd --permanent --remove-port=端口号/协议`
3. 重新载入，才能生效：`firewall-cmd --reload`
4. 查询端口是否开放：`firewall-cmd --query-port=端口号/协议`

实例：

1. 开放111端口

```
firewall-cmd --permanent --add-port=111/tcp

foerwall-cmd --reload
```

2. 关闭111端口

```
firewall-cmd --permanent --remove-port=111/tcp

foerwall-cmd --reload
```



## 1.6、动态监控进程

top 和 ps 命令很相似。他们都用来显示正在执行的进程。top 与 ps 最大的不同之处在于 top 在执行一段时间可以更新正在运行的进程。

基本语法： `top [选项]`

选项说明：

| 选项    | 功能                                       |
| ------- | ------------------------------------------ |
| -d 秒数 | 指定top命令每隔几秒更新，默认是3秒         |
| -i      | 使top不显示任何闲置或者僵死进程            |
| -p      | 通过指定监控进程ID来仅仅监控某个进程的状态 |

我们在 XShell 中输入 `top` 指令，显示如下：

![](韩顺平Linux(四).assets/18.png)



![](韩顺平Linux(四).assets/19.png)





![](韩顺平Linux(四).assets/20.png)



![](韩顺平Linux(四).assets/21.png)







### 1.6.1、交互操作说明

| 操作 | 功能                          |
| ---- | ----------------------------- |
| P    | 以CPU使用率排序，默认就是此项 |
| M    | 以内存的使用率排序            |
| N    | 以PID排序                     |
| q    | 退出top                       |

我们输入`top`,之后再输入 `P`,就可以以CPU使用率进行排序,按`q`可以退出top



1. 监视特定用户，比如我们监控 root 用户
   - top ： 输入此命令，按回车键，查看执行的进程
   - u：然后输入 u ，再输入用户名 回车即可



2. 终止指定的进程，比如我们要结束 tom 登录
   - top ： 输入此命令，按回车键，查看执行的进程
   - k：然后输入 k 回车，再输入要结束的进程ID号





3. 指定系统状态更新的时间(每隔10s自动更新)，默认是3s

```
top -d 10
```







### 1.6.2、监控网络状态

#### 1、查看系统网络情况netstat

基本语法：`netstat [选项]`

选项说明：

- `-an` 按一定顺序排列输出
- `-p` 显示哪个进程在调用

![](韩顺平Linux(四).assets/22.png)

![](韩顺平Linux(四).assets/23.png)





1. 请查看服务名为 sshd 的服务的信息

```
netstat -anp | grep sshd
```



#### 2、检测主机连接命令ping

ping 是一种网络检测工具，它主要是用来检测远程主机是否正常，或者两部主机间的网线或者网卡故障。

如：ping 对方ip地址





# 2、RPM与YUM

## 2.1、rpm包的管理

rpm用于互联网下载包的打包及安装工具，它包含在某些Linux分发版中。它生成具有.RPM扩展名的文件。RPM是 RedHat Package Manager(RedHat软件包管理工具)的缩写，类似 windows 的 setup.exe,这一文件格式名虽然打上了 RedHat 的标志，但是理念是通用的。

### 2.1.1、rpm包查询

查询已安装的 rpm 列表：

```
rpm -qa 
```



1. 查看当前系统，是否安装了 firefox

```
rpm -qa | grep firefox
```





### 2.1.2、rpm包名基本格式

一个rpm包名： **firefox=60.2.2-1.el7.centos.x86_64**

名称：firefox

版本号：60.2.2-1

适用操作系统：el7.centos.x86_64

表示 centos7.x 的64位系统，如果是 i686、i386表示32位系统，noarch 表示通用



### 2.1.3、rpm包的其他查询指令

- `rpm -qa` ： 查询所安装的所有 rpm 软件包
- `rpm -qa | more` 
- `rpm -q 软件包名` : 查询软件包是否安装

```
rpm -q firefox
```

- `rpm -qi 软件包名` ：查询软件包信息

```
rpm -qi firefox
```

- `rpm -ql 软件包名` : 查询软件包中的文件

```
rpm -ql firefox
```

- `rpm -qf 文件全路径名` : 查询文件所属的软件包

```
rpm -qf /etc/passwd

rpm -qf /root/install.log
```



### 2.1.4、卸载rpm包

基本语法：`rpm -e RPM包的名称`

1. 删除 firefox 软件包

```
rpm -e firefox
```



如果其他软件包依赖于您要卸载的软件包，卸载时会产生错误信息。







### 2.1.5、安装rpm包

基本语法：`rpm -ivh RPM包全路径名称`

参数说明：

- `i=install ` 安装
- `v=verbose` 提示
- `h=hash` 进度条

1. 卸载 firefox 浏览器

```
rpm -e firefox
```

2. 安装 firefox 浏览器

```
rpm -ivh firefox
```



## 2.2、yum

Yum 是一个 Shell 前端软件包管理器。基于RPM包管理，能够从指定的服务器自动下载RPM包并且安装，可以自动处理依赖性关系，并且一次安装所有依赖的软件包。





### 2.2.1、yum的基本命令

查询yum服务器是否有需要安装的软件

```
yum list | grep xx软件列表
```

![](韩顺平Linux(四).assets/24.png)



### 2.2.2、安装指定的yum包

```
yum install xxx
```



1. 安装 firefox

```
rpm -e firefox

yum list | grep firefox
```



























