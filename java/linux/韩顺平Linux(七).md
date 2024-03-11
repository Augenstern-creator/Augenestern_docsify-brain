# 1、大数据定制-Shell编程

1. Linux 运维工程师在进行服务器集群管理时，需要编写 Shell 程序来进行服务器管理。
2. 对于 JavaEE 和 Python 程序员来说，工作的需要，你的老大会要求你编写一些 Shell 脚本进行程序或者是服务器的维护，比如编写一个定时备份数据库的脚本
3. 对于大数据程序员来说，需要编写 Shell 程序来管理集群。

## 1.1、什么是Shell

Shell 是一个命令行解释器，它为用户提供了一个向 Linux 内核发送请求以便运行程序的界面系统级程序，用户可以用 Shell 来启动、挂起、停止甚至是编写一些程序。



## 1.2、Shell脚本执行方式

脚本格式要求:

1. 脚本以 `#!/bin/bash` 开头
1. 脚本需要有可执行权限

例子:创建一个Shell脚本，输出 Hello World！

```bash
#! /bin/bash
echo "Hello World!"
```

![](韩顺平Linux(七).assets/1.png)

- 进入文件所在目录: `sh 文件名`

```bash
sh hello.sh
```

![](韩顺平Linux(七).assets/2.png)







# 2、Shell变量

1. Linux Shell 中的变量分为: 系统变量、用户自定义变量
2. 系统变量：
   - `$HOME` 、`$PWD`、`$SHELL`、`$USER`
   - 例如: `echo $HOME`

![](韩顺平Linux(七).assets/4.png)



3. 显示当前shell中的所有变量 : `set`













## 2.1、shell变量的定义

语法:

- 定义变量: `变量名=值`

- 撤销变量: `unset 变量`
- 声明静态变量: `readonly 变量`  ,不能 unset

```bash
#! /bin/bash
# 1. 定义变量
A=100
# 输出变量需要加上 $
echo A=$A
echo "A=$A"

# 2.撤销变量A
unset A
echo "A=$A"

# 声明静态变量 B=2
readonly B=2
echo "B=$B"
```

![](韩顺平Linux(七).assets/3.png)

定义变量的规则:

1. 变量名称可有字母、数字和下划线组成,但是不能以数字开头。
2. 等号两侧不能有空格
3. 变量名称一般习惯大写,这是一个规范

将命令的返回值赋给变量:

```bash
# 将指令返回的结果赋给变量
# 将 data 指令返回的结果赋给变量
A=$(data)
# 等价于
A=`data`
```





### 2.1.1、设置环境变量

语法:

1. `export 变量名=变量值` : 将shell变量输出为环境变量/全局变量
2. `source 配置文件` : 让修改后的配置信息立即生效
3. `echo $变量名` : 查询环境变量的值



例子: 

1. 在 `/etc/profile` 文件中定义 `TOMCAT_HOME` 环境变量
2. 查看环境变量 `TOMCAT_HOME` 的值
3. 在另外一个shell程序中使用 `TOMCAT_HOME` 

```bash
# 1. 增加一个自定义环境变量
vim /ect/profile
```

![](韩顺平Linux(七).assets/5.png)

```bash
# 2. 刷新配置文件
source /etc/profile

# 3. 输出自定义环境变量
echo $TOMCAT_HOME
```

![](韩顺平Linux(七).assets/6.png)

我们在其他 shell 脚本中使用这个自定义环境变量:

```bash
echo  "tomcat_home=$TOMCAT_HOME"
```

![](韩顺平Linux(七).assets/7.png)









### 2.1.2、多行注释

```bash
# shell 脚本的多行注释
:<<! 

多行注释

!
```







## 2.2、位置参数变量

当我们执行一个 shell 脚本时,如果希望获取到命令行的参数信息,就可以使用到位置参数变量。

例如:

```bash
# 这个就是一个执行 shell 的命令行,可以在 myshell 脚本中获取到参数信息
sh hello.sh 100 200
```





### 2.2.1、语法

- `$n` : n 为数字, $0 代表命令本身, `$1-$9` 代表第一到第九个参数, 十以上的参数需要用大括号包含,比如`${10}`
- `$*` : 这个变量代表命令行中所有的参数, `$*` 把所有的参数看成一个整体
- `$@` : 这个变量也代表命令行中所有的参数,不过`$@` 把每个参数区分对待
- `$#` : 这个变量代表命令行中所有参数的个数

例子:编写一个 shell 脚本 `position.sh` ,在脚本中获取到命令行的各个参数信息

```bash
#! /bin/bash
echo "命令本身=$0 第一个参数=$1 第二个参数=$2"
echo "所有的参数整体=$*"
echo "所有的参数区分:$@"
echo "参数的个数=$#"
```



![](韩顺平Linux(七).assets/8.png)







## 2.3、预定义变量

预定义变量: 就是 shell 设计者事先已经定义好的变量,可以直接在 shell 脚本中使用(用的很少)

语法:

- `$$` : 当前进程的进程号 PID
- `$!` : 后台运行的最后一个进程的进程号 PID

- `$?` ：最后一次执行的命令的返回状态。如果这个变量的值为0，证明上一个命令正确执行；如果这个变量的值为非0，则证明上一个命令执行不正确

示例:`pre.sh`

```bash
#!/bin/bash
echo "当前执行的进程id=$$"
# 以后台的方式运行一个脚本,并获取他的进程号(一个脚本调用另一个脚本)
/www/kuang/shell/position.sh &
echo "最后一个后台方式运行的进程id=$!"
echo "执行的结果是$?"
```

![](韩顺平Linux(七).assets/9.png)











## 2.4、运算符

- `$((运算式))` 或 `$[运算式]`

示例：

```bash
#! /bin/bash
# 1.计算(2+3)×4的值
 res=$[(2+3)*4]
 echo "res=$res"
 
# 2.请求出命令行的两个参数的和 20 50
sum=$[$1+$2]
echo "sum=$sum"
```

![](韩顺平Linux(七).assets/10.png)





### 2.4.1、条件判断

语法：

- `[ condition ]` ： 注意 condition 前后要有空格
- 非空返回 true，可使用 `$?` 验证，0为true，>1 为false



常用判断条件：

1. `=` 字符串比较
2. 两个整数的比较
3. `-lt` 小于
4. `-le` 小于等于
5. `-eq` 等于
6. `-gt` 大于
7. `-ge` 大于等于
8. `-ne` 不等于

按照文件权限进行判断：

- `-r` 有读的权限
- `-w` 有写的权限
- `-x` 有执行的权限

按照文件类型进行判断：

- `-f` 文件存在并且是一个常规的文件
- `-e` 文件存在
- `-d` 文件存在并且是一个目录

语法：

```bash
if [ 条件判断式 ]
then
	代码
elif [ 条件判断式 ]
then
	代码
fi	
```

> [ 条件判断式 ]  中括号和条件判断式之间必须有空格



```bash
#! /bin/bash
# 1.判断 ok 是否 等于 ok
if [ "ok" = "ok" ]
then 
	echo "equal"
fi

# 2.判断23是否大于等于22
if [ 23 -ge 22 ]
then
	echo "大于"
fi


# 3. /www/shell/hello.sh 目录中的文件是否存在
if [ -f /www/shell/hello.sh ]
then
	echo "存在"
fi

# 4.中括号里面有内容,默认为真
if [ qinxiaolin ]
then
	echo "hello,qinxiaolin"
fi
```

![](韩顺平Linux(七).assets/11.png)







### 2.4.2、case语句

语法：

```bash
case $变量名 in 
"值1")
echo "如果变量的值等于值1,则执行1"
;;
"值2")
echo "如果变量的值等于值2,则执行2"
;;
*)
echo "如果变量的值都不是以上的值,则执行此程序"
;;
esac
```

示例：当命令行参数是1时,输出周一，是二时，输出周二

```bash
#! /bin/bash
# 当命令行参数是1时,输出周一
case $1 in
"1")
echo "周一"
;;
"2")
echo "周二"
;;
*)
echo "other"
;;
esac
```

![](韩顺平Linux(七).assets/12.png)





### 2.4.3、for循环

语法：

```bash
# 语法一
for 变量 in 值1 值2 值3
do
代码
done

# 语法二
for((初始值;循环控制条件;变量变化))
do
代码
done
```

示例：

```bash
#! /bin/bash
# 1.打印命令行输入的参数【这里可以看出 $* 和 $@ 的区别
# 注意： $* 是把输入的参数,当作一个整体，所以只会输出一句
for i in "$*"
do
	echo "num is $i"
done

# 2. 使用 $@ 来获取输入的参数，注意，这时是分别对待，所以有几个参数，就输出几个
echo "=========="
for j in "$@"
do
	echo "num is $j"
done
```

![](韩顺平Linux(七).assets/13.png)

示例:从1加到100的值输出显示

```bash
#! /bin/bash
# 定义一个变量sum
sum=0
for(( i=1; i<=$1; i++))
do
	sum=$[$sum+$i]
done
echo "总和sum=$sum"
```

![](韩顺平Linux(七).assets/14.png)





### 2.4.4、while循环

语法：

```bash
# while 和 [ 有空格,条件表达式和 [ 也有空格
while [ 条件表达式 ]
do
代码
done
```

示例：

```bash
#! /bin/bash
# 1.从命令行输入一个数n,统计从1 + .. + n 的值是多少
sum=0
i=0
while [ $i -le $1 ]
do
	sum=$[$sum+$i]
	# i 自增
	i=$[$i+1]
done
echo "执行结果=$sum"
```

![](韩顺平Linux(七).assets/15.png)











## 2.5、read读取控制台输入

语法：`read(选项)(参数)`

选项：

- `-p` ： 指定读取值时的提示符
- `-t`：  指定读取值时等待的时间，如果没有在指定的时间内输入，就不再等待了

参数：

- 变量：指定读取值的变量名

示例：

```bash
#! /bin/bash
#1. 读取控制台输入一个 num1 值
read -p "请输入一个数num1=" num1
echo "你输入的num1=$num1"

#2.读取控制台输入一个num2值,请在10s内输入
read -t 10 -p "请输入一个数num2=" num2
echo "你输入的num2=$num2"
```

![](韩顺平Linux(七).assets/16.png)





# 3、函数

## 3.1、系统函数

basename语法：

- `basename[pathname][suffix]` ：返回完整路径最后 `/`的部分，常用于获取文件名
- `basename[string][suffix]` ：basename 命令会删掉所有的前缀包括最后一个 `/` 字符，然后将字符串显示出来

选项：

- `suffix` 为后缀，如果 suffix 被指定了，basename 会将 pathname 或 string 中的 suffix 去掉

```bash
#! /bin/bash
#1. 返回 /www/shell/test.txt 的 "hello.txt" 部分
basename /www/kuang/shell/hello.txt

#2.返回 /www/shell/test.txt 的 "hello" 部分
basename /www/kuang/shell/hello.txt .txt
```

![](韩顺平Linux(七).assets/17.png)



dirname 语法：返回完整路径最后 `/` 的前面的部分，常用于返回路径部分

- `dirname 文件绝对路径`：从给定的包含绝对路径的文件名中去除文件名，然后返回剩下的路径

示例：

```bash
# 请返回 /www/kuang/shell/hello.txt 的/www/kuang/shell/
dirname  /www/kuang/shell/hello.txt
```

![](韩顺平Linux(七).assets/18.png)



## 3.2、自定义函数

语法：

```bash
# 1.定义
[ function ]funname[()]
{
	Action;
	[return int;]
}

# 2.调用
funname [值]
```

示例：

```bash
#! /bin/bash
#1. 计算输入两个参数的和(动态的获取),getSum
# 定义函数
function getSum(){
	sum=$[$n1+$n2]
	echo "和是=$sum"
}

# 输入两个值
read -p "请输入一个数n1=" n1
read -p "请输入一个数n2=" n2
# 调用自定义函数
getSum $n1 $n2
```

![](韩顺平Linux(七).assets/19.png)

# 4、备份案例

1. 每天凌晨 2.30 备份数据库 sql 到 /www/db
2. 备份开始和备份结束能够给出相应的提示信息
3. 备份后的文件要求以备份时间为文件名，并打包成`.tar.gz`的形式，比如 ：`2023-11-24_xxxx.tar.gz`
4. 在备份的同时，检查是否有10天前备份的数据库文件，如果有就将其删除



















































































































