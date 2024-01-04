# 1、Linux搭建JavaEE环境

Linux的安装，安装步骤比较繁琐(操作系统本身也是一个软件)，所以我是双十一购买了轻量云服务器进行学习。而且这样也更加接近真实线上的工作。



## 1.1、安装JDK

1. `mkdir /opt/jdk`

![](韩顺平Linux(五).assets/1.png)



2. 下载jdk
   1. 在上传之前，我们要去 oracle 官网下载jdk，我这里演示下载 jdk8
   2. 打开oracle网站下载地址：https://www.oracle.com/cn/java/technologies/javase/javase8-archive-downloads.html
   3. 我们下载rpm包和压缩包，因为我这里记录 rpm 包安装和 压缩包安装，所以我将两者都下载

![](韩顺平Linux(五).assets/2.png)



当然oracle官网下载还要登录，有时候网络影响也会造成下载失败。这里我偶然间发现了一个网站，可以镜像下载jdk，且版本较全。

编程宝库：[JDK下载](http://www.codebaoku.com/jdk/jdk-index.html)

![](韩顺平Linux(五).assets/3.png)



![](韩顺平Linux(五).assets/4.png)



### 1.1.1、压缩包安装

1. 将 jdk 通过 xftp 上传到 `/opt/jdk` 下。我这里先演示压缩包的安装

![](韩顺平Linux(五).assets/5.png)

2. `cd /opt/jdk`

3. 解压 `tar -zxvf jdk-8u202-linux-x64.tar.gz`

![](韩顺平Linux(五).assets/6.png)



![](韩顺平Linux(五).assets/7.png)



4. `mkdir /usr/local/java`

5. `mv /opt/jdk/jdk1.8.0_202 /usr/local/java`

![](韩顺平Linux(五).assets/8.png)





6. 配置环境变量的配置文件 `vim /etc/profile`，将下方代码粘贴进去,之后`:wq`保存

```
export JAVA_HOME=/usr/local/java/jdk1.8.0_202
export PATH=$JAVA_HOME/bin:$PATH
```

![](韩顺平Linux(五).assets/9.png)

7. `source /etc/profile ` 让新的环境变量生效

![](韩顺平Linux(五).assets/10.png)



8. `echo $PATH` 查看环境变量

![](韩顺平Linux(五).assets/11.png)



9. `javac -version` 查看

![](韩顺平Linux(五).assets/12.png)



### 1.1.2、rpm安装

rpm安装和压缩包基本一样，我们将下载的 rpm 包通过 xftp 上传到Linux服务器上，进行安装。

```bash
rpm -ivk rpm包
```

只需要运行上述命令即可，这里需要注意的是：==rpm方式不需要配置环境变量，rpm方式不需要配置环境变量，rpm方式不需要配置环境变量==

直接就可以 `java -version` 查看即可

### 1.1.3、jdk卸载

```bash
# rpm -qa|grep jdk  # 检测jdk版本信息
# rpm -e --nodeps jdkxxx
```

卸载也很简单，首先检测jdk版本信息，之后会过滤出已安装jdk包的信息，之后执行第二个条强制删除命令即可



## 1.2、tomcat的安装

1. 进入tomcat官网下载tomcat，我这里下载 tomcat9

![](韩顺平Linux(五).assets/13.png)

2. 上传安装文件，并解压到`/opt/tomcat`

   ```
   mkdir /opt/tomcat
   
   cd /opt/tomcat
   
   tar -zvxf tomcat
   ```

![](韩顺平Linux(五).assets/14.png)







3. 进入解压目录 `/bin`，启动 tomcat ` ./startup.sh`

```bash
# 启动 ./startup.sh
# 停止 ./shotdown.sh
```

![](韩顺平Linux(五).assets/15.png)



4. 开放端口8080

```bash
firewall-cmd --permanent --add-port=8080/tcp

firewall-cmd --reload
```

当然因为我是购买的腾讯云轻量应用服务器，我需要在控制台添加 8080 端口开放

![](韩顺平Linux(五).assets/16.png)





5. 在windows、linux 下访问 http://linuxip:8080

![](韩顺平Linux(五).assets/17.png)













## 1.3、mysql5.7的安装

在安装前需要确定现在这个系统有没有 MySQL，如果有那么必须卸载。尤其是新的 CentOS 7 系统，它自带mariaDB数据库，所以需要卸载掉。

1. 查找已安装的MySQL软件包

```bash
rpm -qa|grep mysql
```

2. CentOS7下还需要查找是否存在mariadb包

```bash
rpm -qa|grep mariadb
```

3. 如果输入上述两个命令后都输出存在有包，则需要执行删除命令

```bash
rpm -e --nodeps mysqlxxx

rpm -e --nodeps mariadbxx
```

例如我的服务器有 mariadb 数据库，它会与mysql相冲突

![](韩顺平Linux(五).assets/19.png)

4. 新建文件夹/opt/mysql 

```bash
mkdir /opt/mysql
cd /opt/mysql
```

5. 下载 MySQL 安装包，进入如下网站下载：https://downloads.mysql.com/archives/community/

![](韩顺平Linux(五).assets/18.png)

我们既可以下载下来使用xftp上传到服务器，也可以在 Download 按钮右键复制链接地址，然后使用如下命令命令行下载

```bash
wget 下载链接
```

![](韩顺平Linux(五).assets/20.png)



两种方法都可以使用，速度都不是很快，我这里参考此篇文章获取别人分享的rpm文件：https://blog.csdn.net/qq_43280818/article/details/115625306





6. 因为下载的是 rpm包，所以就舍去了解压的步骤

    这 4 个安装包通过 Xtfp 复制到 /opt/mysql下：

![](韩顺平Linux(五).assets/21.png)

使用 rpm 命令按顺序依次安装 4 个包：

```bash
rpm -ivh mysql-community-common-5.7.16-1.el7.x86_64.rpm 
rpm -ivh mysql-community-libs-5.7.16-1.el7.x86_64.rpm
rpm -ivh mysql-community-client-5.7.16-1.el7.x86_64.rpm 
rpm -ivh mysql-community-server-5.7.16-1.el7.x86_64.rpm
```

![](韩顺平Linux(五).assets/22.png)



7. 使用如下命令查看

```bash
mysqladmin --version
```

![](韩顺平Linux(五).assets/23.png)



8. 运行 `systemctl start mysqld.service` ，启动mysql

9. 然后开始设置 root 用户密码

   MySQL自动给root用户设置随机密码，运行`grep "password" /var/log/mysqld.log` 可看到当前密码

![](韩顺平Linux(五).assets/24.png)



10. 运行 `mysql -u root -p` ,用 root 用户登录，提示输入密码可用上述的临时密码，可以成功登录进入mysql命令行

![](韩顺平Linux(五).assets/25.png)



11. 设置 root密码，对于个人开发环境，如果要设置比较简单的密码(生产环境服务器要设置复杂密码)，可以运行如下提示代码设置策略

```bash
set global validate_password_policy=0;
```

- validate_password_policy 默认值是1
- mysql 密码复杂度要求分三种
  - 0 or LOW： 只要求长度(默认8位)
  - 1 or MEDIUM： 中等，要求长度、大小写、特殊字符
  - 2 or STRONG： 要求长度、大小写、特殊字符、字典文件



接下来设置密码：

```bash
set password for 'root'@'localhost'=password('qxl666nb');
```

最后运行`flush privileges;` 使密码生效

![](韩顺平Linux(五).assets/26.png)

这样就修改好密码了，我们可以使用 `quit` 退出mysql，然后重新登录

```bash
mysql -u root -p
```

![](韩顺平Linux(五).assets/27.png)



这样我们的mysql就安装完成。！！

## 1.4、在服务器运行SpringBoot项目

我们在 Windows 下开发的SpringBoot项目最终可以通过Maven的Package打包成一个 `jar` 包,我们使用 xftp 将此 jar 包上传至服务器，然后执行 `java -jar jar包名称`。这样我们的项目就发布在服务器上了，访问也很简单。`Linuxip:端口号` 即可访问。

- ==需要注意的是在访问之前我们要确保服务器的端口号是打开的状态==

```bash
# 查看防火墙规则
firewall-cmd --list-all # 查看全部信息
firewall-cmd --list-ports # 只看端口信息

CREATE DATABASE bigcreation DEFAULT CHARACTER SET utf8;
quit
mysql -u root -p'qxl666nb' --default-character-set=utf8 bigcreation < /usr/local/bigcreation.sql
```









## 1.5、在服务器运行SSM项目

1. 首先将SSM项目的数据库文件导入

![](韩顺平Linux(五).assets/28.png)



2. 在服务器登录Mysql

```bash
mysql -u root -p
```

![](韩顺平Linux(五).assets/29.png)

3. 创建SSM项目连接的数据库名

```bash
create database 数据库名 default character set utf8;
show databases;
```

例如我的就是：

```bash
create database bigcreation default character set utf8;
show databases;
```

![](韩顺平Linux(五).assets/30.png)



4. 将sql文件导入

```sql
use 数据库名;
source 我们将sql文件放入服务器的路径;
```

例如我的就是：

```sql
use bigcreation;
source /usr/local/BCreation/bigcreation.sql;
```

![](韩顺平Linux(五).assets/31.png)



5. Maven的package可以将SSM项目打包成war包，在target目录下，我们将war包传入服务器tomcat中的webapps下

![](韩顺平Linux(五).assets/32.png)



6. 启动tomcat

```bash
cd tomcat的bin目录;
./startup.sh;
```

例如我的就是

```bash
cd /opt/tomcat/apache-tomcat-9.0.56/bin;
./startup.sh;
```

![](韩顺平Linux(五).assets/33.png)

7. 访问路径为：`Linuxip:端口号/项目名称`，例如我的就是

```bash
49.232.28.14:8080/BC
```















