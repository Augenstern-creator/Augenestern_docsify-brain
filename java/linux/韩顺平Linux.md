# 1、VMware下载

1. 官网地址：https://www.vmware.com/cn.html

   **选择->资源->产品下载**

![](韩顺平Linux.assets/1.png)

2. 在所有下载页面选择 `产品列表`,然后鼠标下滑，找到 `VMware Workstation Pro`

![](韩顺平Linux.assets/2.png)



3. 点击`下载产品`

![](韩顺平Linux.assets/3.png)



4. 我是windows系统，所以选择Windows版本，`转至下载->立即下载`

![](韩顺平Linux.assets/4.png)



5. 下载之后跳转注册页面，这里需要注意的是我们先将页面切换为中文，将除了`验证码`的其他表单都填了，之后将页面切换为`English`，这样的验证码才会正常显示。

   如果有问题可参考此篇博客：[VMware官网注册账号之验证码问题](https://blog.csdn.net/tt15304911199/article/details/113552341)

![](韩顺平Linux.assets/5.png)

6. 注册激活登录之后，进行VMware 16.0 版本的下载

## 1.1、VMware安装

1. 右键下载的安装包，`以管理员身份运行`

2. 点击下一步

![](韩顺平Linux.assets/6.png)

3. 勾选接受许可协议，点击下一步

![](韩顺平Linux.assets/7.png)



4. 更换安装位置，然后下一步

![](韩顺平Linux.assets/8.png)



5. 下方两个想勾选也可以勾选，想不勾选也可以不勾选，点击下一步

![](韩顺平Linux.assets/9.png)



6. 然后一直下一步，点击安装即可

7. 安装成功后桌面会有快捷方式，右键以管理员方式运行，输入激活码(**激活码百度、淘宝等自己找找，这里我就不提供了哈**)

![](韩顺平Linux.assets/10.png)



## 1.2、centos下载

目前centos主流有7版本和8版本，我这里以7版本为例。

1. 我们去网易开源镜像站进行下载：https://mirrors.163.com/

![](韩顺平Linux.assets/11.png)

2. 找到 7.6.1810 版本，进去有一个 readme 文件

![](韩顺平Linux.assets/12.png)

3. 下载 readme 文件，使用记事本打开，进入下图网站：http://vault.centos.org/ 

![](韩顺平Linux.assets/13.png)

4. 进入网站后下载 7.6 版本即可，路径为 `/7.6.1810/isos/x86_64/`

![](韩顺平Linux.assets/14.png)



![](韩顺平Linux.assets/15.png)



![](韩顺平Linux.assets/16.png)



![](韩顺平Linux.assets/17.png)



5. 按照上述步骤再下载8.1版本



## 1.3、安装centos

1. 打开VMware，[文件] -> [新建虚拟机]

![](韩顺平Linux.assets/18.png)

2. 选择稍后安装操作系统

![](韩顺平Linux.assets/19.png)

3. 客户机操作系统选择 `Linux`，版本选择 `CentOS 7` 。因为我们下载的是版本 7.6

![](韩顺平Linux.assets/20.png)

4. 给自己的虚拟机起名，并且将其安装位置进行设置

![](韩顺平Linux.assets/21.png)

5. 这里可以按如下勾选即可

![](韩顺平Linux.assets/22.png)

6. 选择自定义硬件

![](韩顺平Linux.assets/23.png)

7. 根据建议内存选择即可(**这里我选择2GB**)

![](韩顺平Linux.assets/24.png)

8. 处理器给 2*2 即可

![](韩顺平Linux.assets/25.png)

9. 网络适配器选择**NAT模式**即可
   - 桥接模式，虚拟系统可以和外部系统通讯，但是容易造成IP冲突。
   - NAT模式，网络地址转换模式，虚拟系统可以和外部系统通讯，不造成IP冲突。
   - 主机模式：独立的系统

![](韩顺平Linux.assets/26.png)

10. 之后点击关闭，点击完成，左侧我的计算机就会出现我们的虚拟机名称。右键我们的虚拟机，点击设置

![](韩顺平Linux.assets/27.png)

11. 选择 使用ISO影像文件，将我们上述目录[1.2]下载的 centos7.6 .iso 放置进去

![](韩顺平Linux.assets/28.png)





## 1.4、开启虚拟机

1. 点击开启虚拟机

![](韩顺平Linux.assets/29.png)



2. 双击虚拟机屏幕，键盘上下选择，我们选择 Install Centos ，然后回车等待一会进入下面的页面

![](韩顺平Linux.assets/30.png)

3. 选择中文，点击继续，选择软件安装

![](韩顺平Linux.assets/31.png)

4. 在以后工作生活动我们可以默认选择 `最小安装`,但是在学习时尽量选择带有如下环境

![](韩顺平Linux.assets/32.png)

5. 选择安装位置

![](韩顺平Linux.assets/33.png)

6. 我们可以自动配置分区，也可以自己配置分区，这里我选择`我要配置分区`，然后点击完成

![](韩顺平Linux.assets/34.png)

> 自动配置分区点击方式：
>
> ![](韩顺平Linux.assets/99.png)

7. 手动分区，Linux系统一般分三个区，
   - `/boot` 分区：1G
   - `swap` 分区：一般以运行内存为准即可(上述我们分了2GB的运存)
   - `/` 根分区：剩余空间

![](韩顺平Linux.assets/35.png)

8. 更改`/boot`分区的文件系统为 ext4

![](韩顺平Linux.assets/36.png)



9. 给`swap`分区，上述我给的运存是2GB，所以这里给分2GB

![](韩顺平Linux.assets/37.png)







10. 更改`swap`分区设备类型为**标准分区**，文件系统为 `swap`

![](韩顺平Linux.assets/38.png)

11. 之后就是给`/`根分区，根分区分剩余的17GB空间即可

![](韩顺平Linux.assets/39.png)



12. 更改`/`分区的设备类型和文件系统

![](韩顺平Linux.assets/40.png)



13. 点击完成，点击接受更改

![](韩顺平Linux.assets/41.png)





14. 点击 `KDUMP`

![](韩顺平Linux.assets/42.png)



15. 我们不勾选`启用kdump`，这个在实际工作中可以勾选，学习过程就不用勾选了节省内存

![](韩顺平Linux.assets/43.png)



16. 点击网络与主机名

![](韩顺平Linux.assets/44.png)





17. 打开以太网，更改主机名，点击应用

![](韩顺平Linux.assets/45.png)

> 1. 修改主机名为自己喜欢的主机名，不要出现中文和特殊字符，建议用localhost
> 2. 点击应用
> 3. 将网络连接打开(使用截图工具记住IP地址、子网掩码、默认路由、NDS)
> 4. 点击配置，设置详细网络信息
>
> ![](韩顺平Linux.assets/100.png)
>
> 点击配置按钮后，我们需要把网卡地址改为静态IP，这样可以避免每次启动虚拟机IP都变化。
>
> ![](韩顺平Linux.assets/101.png)
>
> 最后点击完成即可







18. 点击 `SECURITY POUCY`

![](韩顺平Linux.assets/46.png)





19. 关闭如下按钮

![](韩顺平Linux.assets/47.png)



20. 点击开始安装

![](韩顺平Linux.assets/48.png)



21. 点击ROOT密码

![](韩顺平Linux.assets/49.png)





22. 设置ROOT密码，这里我就设置为简单的`root`，但是在实际工作中密码必须要复杂

![](韩顺平Linux.assets/50.png)





23. 我们点击创建用户，因为Linux系统通常不会以最高权限的用户使用，我们创建一个低级用户

![](韩顺平Linux.assets/51.png)



24. 我这里就创建tom用户

![](韩顺平Linux.assets/52.png)







25. 等待完成，点击重启

![](韩顺平Linux.assets/53.png)







26. 点击如下按钮

![](韩顺平Linux.assets/54.png)



27. 选择同意协议，点击完成

![](韩顺平Linux.assets/55.png)





28. 点击完成配置

![](韩顺平Linux.assets/56.png)





29. 这里默认是以tom为账户进行登录，我们也可以点击`未列出`登录其他账号

![](韩顺平Linux.assets/57.png)







30. 例如我这里登录root用户

![](韩顺平Linux.assets/58.png)





31. 选择汉语

![](韩顺平Linux.assets/59.png)



32. 选择汉语带拼音

![](韩顺平Linux.assets/60.png)





33. 关闭位置服务

![](韩顺平Linux.assets/61.png)

34. 跳过

![](韩顺平Linux.assets/62.png)

35. 这样我们就可以进入如下界面

![](韩顺平Linux.assets/63.png)



36. OK,至此我们的Linux系统就安装好了！









## 1.5、虚拟机克隆

如果你已经安装了一台Linux系统，你还想要更多的。难道我们还有必要再重新上述步骤重新安装吗？当然不需要，我们可以克隆就可以。

> 方式一：直接拷贝一份安装好的虚拟机文件

1. 找到我们的安装目录

![](韩顺平Linux.assets/70.png)

2. 找到目录，右键复制

![](韩顺平Linux.assets/71.png)

3. 发送给其他电脑，其他电脑使用VMware打开，这样我们这里的Linux系统就可以被其他电脑使用了，包括root、tom账户等。



> 方式二：使用VMware提供的方式克隆，注意，在克隆之前我们要先关闭Linux系统

1. 点击如下按钮关闭Linux系统

![](韩顺平Linux.assets/64.png)

2. 点击我们的虚拟机，右键

![](韩顺平Linux.assets/65.png)



3. 点击下一页

![](韩顺平Linux.assets/66.png)



4. 选择如下按钮

![](韩顺平Linux.assets/67.png)

5. 选择创建完整克隆



![](韩顺平Linux.assets/68.png)



6. 修改名称、位置，然后点击完成等待即可克隆成功

![](韩顺平Linux.assets/69.png)







## 1.6、虚拟机快照

如果我们在使用虚拟机系统的时候，想回到原先的一个状态，也就是说我们担心可能有些误操作造成系统异常，需要回到原先某个正常运行的状态，VMware提供了这样的功能，就叫做快照管理。

1. 我们右键虚拟机-快照-拍摄快照

![](韩顺平Linux.assets/72.png)

2. 如下设置名称、描述，点击拍摄快照

![](韩顺平Linux.assets/73.png)



3. 快照拍摄完成，我们右键桌面，选择新建文件夹。这里我新建了一个`QXL`的文件夹

![](韩顺平Linux.assets/74.png)

4. 我们再次拍摄快照，名称我取为 快照B

![](韩顺平Linux.assets/75.png)

5. 然后我又新建了一个文件夹`QXL2`



![](韩顺平Linux.assets/76.png)

6. 再次拍摄快照，第三次拍摄快照，名称我设为 快照C

![](韩顺平Linux.assets/77.png)

7. 我们选择快照管理器

![](韩顺平Linux.assets/78.png)

8. 我们转到快照A的状态，点击确定，虚拟机会重启，然后回到我们快照A的状态

![](韩顺平Linux.assets/79.png)

9. 如下图，快照A的状态没有`QXL`文件夹的创建

![](韩顺平Linux.assets/80.png)

10. 若我们想去其他快照，我们可以按照上述操作选择去其他快照。





## 1.7、虚拟机的迁移和删除

虚拟系统安装好了，它的本质就是文件(放在文件夹的)。因此虚拟系统的迁移很方便，我们可以把安装好的虚拟系统这个文件夹整体拷贝或剪切到另外位置使用。

删除也很简单，用VMware进行移除，再点击菜单->从磁盘删除即可，或者直接手动删除虚拟系统对应的文件夹即可。



![](韩顺平Linux.assets/81.png)



这里移除只是在VMware左侧菜单移除，之后还需要去对应安装的文件夹删除对应文件夹，这样才是从磁盘中彻底的删除。



## 1.8、安装vmtools

- vmtools 安装后，可以让我们在 windows 下更好的管理 vm 虚拟机
- 可以设置 windows 和 centos 的共享文件夹



1. 弹出Centos

![](韩顺平Linux.assets/82.png)

2. 重新安装VMware Tools

![](韩顺平Linux.assets/83.png)



3. 桌面会出现 VMware Tools，我们打开，复制如下文件

![](韩顺平Linux.assets/84.png)

4. 打开桌面的主文件夹，选择其他位置-计算机

![](韩顺平Linux.assets/85.png)



5. 找到 opt 文件夹，粘贴我们刚才的文件

![](韩顺平Linux.assets/86.png)

![](韩顺平Linux.assets/87.png)

6. 使用解压命令 tar，得到一个安装文件。

   首先在桌面右键打开终端，输入如下指令，然后回车

   ```
   cd/opt/  [进入到opt目录]
   ls       [打印出当前目录的列表]
   tar -zxvf  XX.tar.gz[解压XX.tar.gz文件]
   ```

   

![](韩顺平Linux.assets/88.png)

![](韩顺平Linux.assets/89.png)



7. 解压完成之后，再次使用`ls`指令打印目录列表，发现有如下蓝色文件夹

![](韩顺平Linux.assets/91.png)

8. 进入文件夹，`cd` 命令

![](韩顺平Linux.assets/90.png)

9. 安装，命令为 `./xx`

![](韩顺平Linux.assets/92.png)

10. 之后一直回车，等待片刻

![](韩顺平Linux.assets/93.png)



11. 我们在自己的主机Windows上的磁盘上建立共享文件夹。我这里在D盘建立 `myshare`文件夹，并且建立 hello.txt，在里面写上hello



![](韩顺平Linux.assets/94.png)



12. 在VMware里面我们的虚拟机右键，点击设置-选项-共享文件夹，勾选如下

![](韩顺平Linux.assets/95.png)

13. 点击添加，将我们刚才的共享文件夹`myshare` 添加，选择下一步->启用此共享

![](韩顺平Linux.assets/96.png)



14. 在Linux系统打开主文件夹-其他位置-计算机-mnt-hgfs

![](韩顺平Linux.assets/97.png)

15. 如果我们看不到共享文件夹，可以先将虚拟机关机，然后再设置共享，之后再重启虚拟机应该就可以了。(若还有问题可以百度搜寻，有很多伙伴都有此类问题)

![](韩顺平Linux.assets/98.png)

16. 这样我们在 `myshare` 文件夹对文件进行增删改操作，在我们的`Windows`主机也会进行增删改。



> Windows 和 Centos 在实际开发中，文件的上传下载是使用 `远程方式` 完成的，这种方法会在后面记录





## 1.9、配置静态IP

如果在创建虚拟机时，没有配置静态IP（目录1.3第17步），则需要在 VMware 中手动配置静态ip。因为Centos网络配置是使用 dhcp 方式分配ip地址，这种方式会在系统每次联网的时候分配一个ip给我们用，也就是说有可能系统下次启动的时候ip会变，这样非常不方便我们管理。

1. 打开VMware，点击左上角 编辑 - 虚拟网络编辑器 - NAT模式的那一行 - NAT设置

![](韩顺平Linux.assets/102.png)

2. 记录下来子网、子网掩码、网关

3. 进入XShell，输入命令

```bash
cd /etc/sysconfig/network-scripts/
```

然后再输入`ls`可以看到下面有许多文件，找到以`ifcfg-en`开头的，使用`vim 文件名称`打开它，按 `i` 来进行编辑

![](韩顺平Linux.assets/103.png)

配置如下代码，并按`ESC` - `:wq` 保存并退出

```bash
# 将dhcp改为static静态 
BOOTPROTO = static
# ip地址
IPADDR = 192.168.6.0
# 子网掩码
NETMASK = 255.255.255.0
# 网关
GATEWAY = 192.168.6.2
# DNS1,一搬设置为8.8.8.8
DNS1 = 8.8.8.8

# 是否开机启动
ONBOOT = yes
```



![](韩顺平Linux.assets/104.png)



4. 重启

```bash
service network restart
```

5. 重启后 XShell 会断开连接，因为 IP 变化了，所以需要重新连接，在主机处写上自己配置的静态IP

![](韩顺平Linux.assets/105.png)



并且宝塔的内网地址也会发生变化，具体可输入`bt 14` 来显示

6. 输入`ifconfig` 或者 `ip addr` ，静态 IP 配置成功

![](韩顺平Linux.assets/106.png)

> Tips:如果克隆虚拟机，安装好也要设置虚拟静态IP步骤呦













## 1.10、初始安装部分命令

更新yum

```bash
yum update -y
```

> Linux - yum 安装软件时被 PackageKit 锁定解决方案:https://cloud.tencent.com/developer/article/1817591

1. 安装wget

```bash
yum install -y wget
```

> 注：-y 表示安装过程中不询问是否继续，直接安装，不写-y则安装过程中提示是否继续，必须手动输入y/d/N中的一个字母，回车后继续进行操作。
>
> y：在线下载安装
>
> d：只下载不安装
>
> N：不安装

2. 安装gcc环境

```bash
yum -y install gcc-c++ 
```

3. 安装PCRE

```bash
yum install -y pcre pcre-devel
```

4. 安装zlib

```bash
yum install -y zlib zlib-devel
```

5. 安装openssl

```bash
yum install -y openssl openssl-devel
```

6. 安装 ifconfig

```bash
yum install -y net-tools
```

7. 安装vim

```bash
yum install -y vim
```

8. 安装tree

```bash
yum install -y tree
```

9. 安装网络端口扫描工具nmap

```bash
yum install -y nmap
```

`nmap 127.0.0.1` 查看本机开放的端口，注意不等同于防火墙端口

































## 1.11、踩坑：静态ip设置后会回复

问题描述：静态ip设置后重启会恢复为原来ip,修改配置文件也不好使,解决办法：

```bash
# 关闭NetworkManager
systemctl stop NetworkManager
# 禁用NetworkManager
systemctl disable NetworkManager
# 重启
reboot
```





























