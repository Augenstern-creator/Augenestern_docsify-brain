# 1、Linux高级篇-日志管理

1. 日志文件是重要的系统信息文件，其中记录了许多重要的系统事件，包括用户的登录信息、系统的启动信息、系统的安全信息、邮件相关信息、各种服务相关信息等。
2. 日志对于安全来说也很重要，它记录了系统每天发生的各种事情，通过日志来检查错误发生的原因，或者受到攻击时攻击者留下的痕迹。
3. 可以这样理解日志是用来记录**重大事件**的工具



## 1.1、系统日志

- `/var/log` 目录就是系统日志文件的保存位置

| 日志文件              | 说明                                                         |
| --------------------- | ------------------------------------------------------------ |
| **/var/log/boot.log** | 系统启动日志                                                 |
| **/var/log/cron**     | 记录与系统定时任务相关的日志                                 |
| /var/log/cups/        | 记录打印信息的日志                                           |
| /var/log/dmesg        | 记录了系统在开机时内核自检的信总。也可以直接使用dmesg命令直接查看内核自检信息 |
| /var/log/btmp         | 记录错误登录的日志。这个文件不能用Vi查看，需使用`lastb`命令查看 |
| **/var/log/lastlog**  | 记录系统中所有用户最后一次的登录时间的日志，需使用`lastlog`命令查看 |
| **/var/log/mailog**   | 记录邮件信息的日志                                           |
| **/var/log/message**  | 记录系统重要信息的日志，这个日志文件中会记录Linux系统的绝大多数重要信息。如果系统出了问题，首先要检查的就是这个日志文件 |
| **/var/log/secure**   | 记录验证和授权方面的信息，只要涉及账户和密码的程序都会记录。比如系统的登录、ssh的登录、su切换用户、sudo授权，甚至添加用户和修改用户密码都会记录在这个日志文件中 |
| /var/log/wtmp         | 永久记录所有用户的登录、注销信息，同时记录系统的启动、重启、关机事件。需使用`lastb`命令查看 |
| **/var/log/ulmp**     | 记录当前已经登录的用户的信息，这个文件会随着用户的登录和注销而不断变化，只记录当前登录用户的信息，这个文件不能用Vi查看，而要使用w、who、users等命令查看 |

- **/var/log/lastlog** ： 查看此日志

![](韩顺平Linux(九).assets/1.png)

- /var/log/secure：查看此日志

> 使用root用户通过 xshell 登陆，第一次使用错误的密码，第二次使用正确的密码登录成功看看在日志文件`var/log/secure`里有没有记录相关信息
>
> - 可以事先用`echo "" > secure` 将secure日志清空



## 1.2、日志管理服务rsyslogd

上面的那些日志就是Centos的日志管理服务rsyslogd去帮我们把相应日志记录进去的，在`/etc/rsylog.conf`配置文件里面配置了哪些服务的日志信息会被记录到哪个位置。



1. 首先查询Linux中的 rsyslogd 服务是否启动

```bash
ps aux | grep "rsyslog"
```

2. 查询 rsyslogd 服务的自启动状态(保证是自启动enable状态)

```bash
systemctl list-unit-files | grep rsyslog
```

![](韩顺平Linux(九).assets/2.png)

- `cat /etc/rsylog.conf` ： 如果没有此目录或者文件，提前使用`ls -a` 查看一下

```bash
#### MODULES ####
# 模块加载：模块加载部分用于加载必要的模块，例如本地系统日志、内核日志、UDP 和 TCP 日志接收等

$ModLoad imuxsock # 支持本地系统日志
$ModLoad imjournal # provides access to the systemd journal

# 支持UDP协议
#$ModLoad imudp  
#$UDPServerRun 514

# 支持TCP协议
#$ModLoad imtcp
#$InputTCPServerRun 514


#### GLOBAL DIRECTIVES ####
# 全局指令部分定义了日志格式和其他全局设置。

# Where to place auxiliary files
$WorkDirectory /var/lib/rsyslog

# # 定义日志格式
$ActionFileDefaultTemplate RSYSLOG_TraditionalFileFormat


# 包含其他配置文件
$IncludeConfig /etc/rsyslog.d/*.conf

# Turn off message reception via local log socket;
# local messages are retrieved through imjournal now.
$OmitLocalLogging on

# File to store the position in the journal
$IMJournalStateFile imjournal.state


#### RULES ####
# 规则部分定义了日志的记录方式和位置
*.info;mail.none;authpriv.none;cron.none                /var/log/messages

# The authpriv file has restricted access.
authpriv.*                                              /var/log/secure

# Log all the mail messages in one place.
mail.*                                                  -/var/log/maillog


# Log cron stuff
cron.*                                                  /var/log/cron

# Everybody gets emergency messages
*.emerg                                                 :omusrmsg:*

# Save news errors of level crit and higher in a special file.
uucp,news.crit                                          /var/log/spooler

# Save boot messages also to boot.log
local7.*                                                /var/log/boot.log
```

![](韩顺平Linux(九).assets/3.png)

配置日志文件的格式为`*.*`，其中第一个 * 代表日志类型，第二个 * 代表日志级别

| 日志类型             | 说明                                |
| -------------------- | ----------------------------------- |
| auth                 | pam产生的日志                       |
| authpriv             | ssh、ftp登录信息的验证信息          |
| corn                 | 时间任务相关                        |
| kern                 | 内核                                |
| lpr                  | 打印                                |
| mail                 | 邮件                                |
| mark(syslog)-rsyslog | 服务内部的信息，时间标识            |
| news                 | 新闻组                              |
| uucp                 | unix to nuix copy主机之间相关的通信 |
| local 1-7            | 自定义的日志设备                    |

| 日志级别 | 说明                         |
| -------- | ---------------------------- |
| debug    | 调试信息的，日志通信最多     |
| info     | 一般信息日志，最常用         |
| notice   | 最具有重要性的普通条件的信息 |
| warning  | 警告级别                     |
| err      | 错误级别                     |
| crit     | 严重级别                     |
| alert    | 需要立刻修改的信息           |
| emerg    | 内核崩溃等重要信息           |
| none     | 什么都不记录                 |

从上到下，级别从低到高，记录信息越来越少。

---

由日志服务rsyslogd记录的日志文件，日志文件的格式包含以下四列：

1. 事件产生的时间
2. 产生事件的服务器的主机名
3. 产生事件的服务名或程序名
4. 事件的具体信息

例如`/var/log/secure`

![](韩顺平Linux(九).assets/4.png)

> 在`/etc/rsyslog.conf`中添加一个一条配置：
>
> - 这样ssh、ftp登录信息的验证信息就会记录到`/var/log/demo.log`里面了
>
> ```bash
> # 增加的自定义日志 
> authpriv.*    	/var/log/demo.log
> ```



## 1.3、日志轮替

日志轮替就是把**旧的日志文件移动并改名**，同时建立新的空日志文件，当旧日志文件超出保存的范围之后，就会进行删除。

- Centos7使用 logrotate 进行日志轮替管理，要想改变日志轮替文件名字，通过`/etc/logrotate.conf`配置文件中 `dateext` 参数
- 如果配置文件中有 `dateext` 参数，那么日志会用日期来作为日志文件的后缀，例如 "secure-20201010"，这样日志文件名不会重叠，也就不需要日志文件的改名，只需要指定保存日志个数，删除多余的日志文件即可。
- 如果配置文件中没有 `dateext` 参数，日志文件就需要进行改名了，当第一次进行日志轮替时，当前的 `secure` 日志会自动改名为 `secure.1`，然后新建`secure`日志，用来保存新的日志。当第二次进行日志轮替时，`secure.1`会自动改名为`secure.2`，当前的`secure`日志会自动改名为`secure.1`，然后也会新建`secure`日志，用来保存新的日志，以此类推。

![](韩顺平Linux(九).assets/5.png)

- cat /etc/logrotate.conf

```bash
# see "man logrotate" for details
# 每周对日志文件进行一次轮替
weekly

# 共保存8份日志文件，当建立新的日志文件时，旧的将会被删除
rotate 8

# 在日志轮替后创建新的空日志文件
create

# 使用日期作为日志轮替文件的后缀
dateext

#日志文件是否压缩，如果取消注释，则日志会在转储的同时进行压缩
#compress

# 包含/etc/logrotate.d/目录中所有的子配置文件，也就是说会把这个目录中所有子配置文件读取进来
include /etc/logrotate.d


# 下面是单独设置，优先级更高
/var/log/wtmp {
    monthly   # 每月对日志文件进行一次轮替
    create 0664 root utmp # 建立的新日志文件，权限是0664，所有者是root，所属组是utmp组
        minsize 1M # 日志文件最小轮替大小是1MB，也就是日志一定要超过1MB才会轮替，否则就算时间达到1个月，也不进行日志转储
    rotate 8 # 仅保留8个日志备份，也就是只有 wtmp、wtmp1、wtmp2...日志保留
}

/var/log/btmp {
    missingok # 如果日志不存在，则忽略该日志的警告信息
    monthly
    create 0600 root utmp
    rotate 8
}
```

| 参数                    | 说明                                                         |
| ----------------------- | ------------------------------------------------------------ |
| daily                   | 日志的轮替周期是每天                                         |
| weekly                  | 日志的轮替周期是每周                                         |
| monthly                 | 日志的轮替周期是每月                                         |
| rotate 数字             | 保留的日志文件的个数，0指没有备份                            |
| compress                | 日志轮替时，旧的日志进行压缩                                 |
| create mode owner group | 建立新日志，同时指定新日志的权限与所有者和所属组             |
| mail address            | 当日志轮替时，输出内容通过邮件发送到指定的邮件地址           |
| missingok               | 如果日志不存在，则忽略该日志的警告信息                       |
| notifempty              | 如果日志为空文件，则不进行日志轮替                           |
| minsize 大小            | 日志轮替的最小值，也就是日志一定要达到这个最小值才会轮替，否则就算时间达到也不轮替 |
| size 大小               | 日志只有大于指定大小才进行日志轮替，而不是按时间轮替         |
| dateext                 | 使用日期作为日志轮替文件的后缀                               |
| sharedscripts           | 在此关键字之后的脚本只执行一次                               |
| prerotate/endscript     | 在日志轮替之前执行脚本命令                                   |
| postrotate/endscript    | 在日志轮替之后执行脚本命令                                   |

### 1.3.1、把自己的日志加入日志轮替

1. 确定你想要加入轮替的日志文件的路径。例如，假设你的日志文件位于`/var/log/myapp.log`
2. 在`/etc/logrotate.d/`目录下创建一个新的配置文件，例如`myapp`

3. 在`myapp`中添加以下配置内容

```bash
/var/log/myapp.log {
    daily                  # 日志轮替周期（daily, weekly, monthly, 或自定义）
    rotate 7               # 保留最近7个轮替的日志文件
    missingok              # 如果日志文件不存在，不报错继续下一个
    notifempty             # 如果日志文件为空，不进行轮替
    compress               # 压缩轮替后的日志文件
    delaycompress          # 延迟压缩到下一个轮替周期（与compress一起使用时有用）
    create 0640 root utmp  # 以指定的权限和所有者创建新的日志文件
    postrotate
        # 在日志轮替后执行的命令，例如重启服务或发送信号
        /usr/bin/systemctl reload myapp.service > /dev/null 2>/dev/null || true
    endscript
}
```

4. 可以手动运行`logrotate`来测试配置文件是否有效

```bash
sudo logrotate -d /etc/logrotate.d/myapp
```

`-d`选项表示调试模式，不会实际执行轮替操作，但会输出它将要执行的操作。确保没有错误输出。







## 1.4、备份与恢复

Linux的备份和恢复很简单，有两种方式：

1. 把需要的文件(或者分区)用tar打包就行，下次需要的时候，再解压覆盖即可。
2. 使用dump和restore命令

如果Linux上没有dump和restore指令，需要先安装：

```bash
yum -y install dump
yum -y install restore
```



### 1.4.1、使用dump完成备份

dump支持分卷和增量备份(所谓增量备份是指上次备份后，修改/增加过的文件，也称差异备份)。

dump 命令使用“备份级别”来实现增量备份，它支持 0～9 共 10 个备份级别。其中，0 级别指的就是完全备份，1～9 级别都是增量备份级别。

- 举个例子，当我们备份一份数据时，第一次备份应该使用 0 级别，会把所有数据完全备份一次；第二次备份就可以使用 1 级别了，它会和 0 级别进行比较，把 0 级别备份之后变化的数据进行备份；第三次备份使用 2 级别，2 级别会和 1 级别进行比较，把 1 级别备份之后变化的数据进行备份，以此类推。
- **需要注意的是，只有在备份整个分区或整块硬盘时，才能支持 1～9 的增量备份级别；如果只是备份某个文件或不是分区的目录，则只能使用 0 级别进行完全备份。**

```bash
dump [选项] 备份之后的文件名 原文件或目录
```

选项：

- -level：就是我们说的 0～9 共 10 个备份级别；
- -f 文件名：指定备份之后的文件名；
- -u：备份成功之后，把备份时间、备份级别以及实施备份的文件系统等信息，都记录在 /etc/dumpdates 文件中；
- -v：显示备份过程中更多的输出信息；
- -j：调用 bzlib 库压缩备份文件，其实就是把备份文件压缩为 .bz2 格式，默认压缩等级是 2；
- -W：显示允许被 dump 的分区的备份等级及备份时间；

---

案例1：将 /boot 分区所有内容备份到 /opt/boot.bak0.bz2 文件中，备份层级为0

```bash
dump -0uj -f /opt/book.bak0.bz2 /boot
```

案例2：在 /boot 目录下增加新文件，备份层级为1(只备份上次使用层次0备份后发生过改变的数据)，注意比较看看这次生成的备份文件 boot1.bak 有多大

```bash
dump -1uj  0f /opt/boot.bak1.bz2 /boot
```

> 通过 dump 命令配置 crontab 可以实现无人值守备份

- 查看备份时间文件：`cat /etc/dumpdates`

我们在备份分区时，是可以支持增量备份的，如果备份文件或者目录，不再支持增量备份，即只能使用0级别备份。

案例3：使用dump备份 /etc 整改目录

```bash
dump -0j -f /opt/etc.bak.bz2 /etc/
```

如果是重要的备份文件，建议将文件上传到其他服务器保存。



### 1.4.2、使用restore完成恢复

restore命令用来恢复已备份的文件，可以从 dump 生成的备份文件中恢复原文件。

```bash
restore [模式选项][选项]
```

模式选项：不能混用，一次命令中只能使用一种

- -C：使用对比模式，将备份的文件与已存在的文件相互对比
- -i：使用交互模式，在进行还原操作时，restore 指令将依序询问用户
- -r：进行还原模式
- -t：查看模式，看备份文件有哪些文件

选项： -f<备份设备> ：从指定的文件中读取备份数据，进行还原操作

1. restore 命令比较模式，比较备份文件和原文件的区别

```bash
restore -C -f boot.bak1.bz2
```

2. restore 命令查看模式，看备份文件有哪些数据/文件

```bash
restore -t -f boot.bak0.bz2
```



3. restore 命令还原模式：如果有增量备份，需要把增量备份文件也进行恢复，有几个增量备份文件，就要恢复几个，按顺序来恢复即可。

```bash
# 恢复到第一次完全备份状态
restore -r -f /opt/boot/bak0.bz2 
# 恢复到第二次增量备份状态
restore -r -f /opt/boot/bak1.bz2 
```



4. restore命令恢复备份的文件，或者整改目录的文件

```bash
restore -r -f 备份好的文件
```















































