# Day11linux

centos查看IP：ifconfig

## 目录树

/bin：存放着最常用的命令和程序，如 ls、cp 等。

/boot：包含启动 Linux 时使用的核心文件，如内核镜像和引导加载器。

/dev：包含设备文件，Linux 中访问设备的方式与访问文件相同。

/etc：存放系统配置文件，这些文件对系统启动和运行至关重要。

/home：用户的主目录，通常以用户的账号命名。

/lib：包含系统最基本的动态链接共享库，类似于 Windows 中的 DLL 文件。

/media：用于自动挂载外部设备，如 U 盘和光驱。

/mnt：用于临时挂载文件系统，如挂载光驱或其他分区。

/opt：用于存放可选的应用软件包和额外安装的软件。

/proc：虚拟文件系统，提供系统信息和内核运行状态的接口。

/root：系统管理员（超级用户）的主目录。

/sbin：存放系统管理员使用的系统管理程序。

/tmp：用于存放临时文件。

/usr：存放用户的应用程序和文件，类似于 Windows 下的 Program Files 目录。

/var：包含经常变化的文件，如日志文件。

## 命令构成

命令：表示命令的名称，如 ls、cd、cp等

选项：定义命令的执行特性，通常前带 - 号或--号(长选项/短选项)

参数：表示命令的作用对象

**常用命令**

|     |     |
| --- | --- |
| **命令** | **说明** |
| shutdown -h now | 立该进行关机 |
| shutdown -h 1 | "hello,1分钟后会关机了" |
| shutdown -r now | 现在重新启动计算机 |
| halt f | 关机命令 |
| reboot | 现在重新启动计算机 |
| sync | 把内存的数据同步到磁盘. |
| init 0 | 立刻进行关机 |
| init 6 | 现在重新启动计算机 |

## 文件目录类命令

### cd 切换到文件目录

pwd 移动到当前位置

|     |     |
| --- | --- |
| 命令  | 作用  |
| cd 或cd | 回到家位置（不是home） |
| cd ../ | 回到当前目录的上一级目录 |
| 绝对目录 |     |
| cd /目录/... | 以/开头，移动到绝对目录 |
| 相对目录 |     |
| cd 目录/目录... | 移动到当前位置的目录下 |

### ls 查看文件内容

|     |     |
| --- | --- |
| 命令  | 作用  |
| ls -a | 显示全部包括隐藏文件(隐藏文件以.开头) |
| ls -l （文件名） | 等于 ll 显示长格式属性 |
| ls -r | 反向排序 |
| ls -S | 按照占磁盘大小从大到小排序(S为大写) |
| ls -t | 以时间排序（由新到 |
| ls 目录/目录... | 可使用绝对位置和相对位置查看相应位置下的文件 |
| ls /目录/目录 ... |

注：选项可以组合使用，如ls -al

### **mkdir 创建目录（文件夹）**

|     |     |
| --- | --- |
| mkdir 文件1 文件2 ... | 创建当前位置下多个同级目录 |
| mkdir -p 文件1/文件2 ... | 递归创建多级目录 |
| mkdir -p /文件1/文件2... |

### touch 创建文件

|     |     |
| --- | --- |
| touch 文件名.格式 文件名.格式... | 创建多个同级文件 |
| touch {1...n}.格式 | 创建表名部分内容递增文件 |
| touch {a...n}.格式 |

### 通配符

配合合文件名及目录使用，可用于创建或删除文件

|     |     |
| --- | --- |
| f?.txt | 仅匹配一个字符 |
| \*.txt | \*匹配任意字符 |
| {1..n} | 循环1到n |
| {a..n} | 循环a到n |

注：{}{}代表二层循环，类似for{ for{} }

### rm 删除文件或目录

|     |     |
| --- | --- |
| rm -f... | 强制删除，不会询问，其他均会询问 |
| rm -r... | 递归删除目录，联通目录内的文件一同删除 |
| rm t.txt | 删除文本文件t |
| rm -r a | 删除a目录（文件夹）及其里面的文件及子目录 |
| rm -rf \* | 强制删除文件夹下面的子目录和文件 |
| rm -rf q\* | 强制删除以q开头的文件夹及下面的子目录和文件 |

注：rm可配合{}删除特定名称的文件

rm -rf /tmp/\*

删除/temp下的所有文件和目录，保留tmp

### mv 移动文件或目录/重命名

|     |     |
| --- | --- |
| mv a b | 重命名 |
| mv a.txt b.txt |
| mv a (目录) | 将文件a移动到目录下 |
| mv (目录)a.txt (目录)b.txt | 移动到目录并重命名 |

### cp 拷贝文件/目录

|     |     |
| --- | --- |
| cp -r | 递归拷贝目录（复制目录下所有子文件） |
| cp (目录)a.txt (目录)/a.txt | 拷贝文件a到新目录 |
| cp (目录)a.txt (目录)/b.txt | 拷贝时更改文件名称为b.txt |
| cp -r /root/a /root/b |     |

### ln 软链接

软连接是Linux系统上的另一个文件或目录

格式：ln -s (目标目录) (软连接所在目录)软连接名称

ln -s /usr/local/python3/bin/python3.9 /usr/bin/python3

**指向路径**：/usr/local/python3/bin/python3.9

这是真正的可执行程序，软连接只是指向它

**软连接目录位置**：/usr/bin/

**软连接名称**：python3

**删除软链接**

rm (-f) (软连接所在目录)软连接名称

## 编辑文件

vim 目录/文件名 进入一般模式

### 一般模式操作

1.  dd 删除光标所在的行,且保存到剪贴板
2.  3dd 删除光标所在的三行,且保存到剪贴板
3.  yy复制光标所在的行
4.  4yy复制光标所在的连续4行
5.  p（小写） 将已复制的内容在光标的下一行粘贴
6.  P（大写）将已复制的内容在光标的上一行粘贴
7.  x,X：在一行字中，x 为向后删除一个字符（相当于\[Del\]键），X 为向前删除一个字符，前也可加数字（相当于\[Backspace\]）也可5x等
8.  G光标快速定位到最后一行
9.  gg 光标快速定位到第一行
10. u 撤销上一步操作

快速删除/复制

:开始行数,结束行数y

### 命令行模式操作

1.  q 不保存退出 后面加！为强制退出
2.  wq 保存后退出 后面加！为强制保存后退出
3.  ！强制执行（强制退出，强制保存）
4.  :set nu 显示行号
5.  :set nonu 取消行号
6.  :5 光标快读定位到第5行 （输入数字定位到对应的行号）
7.  :nohl 去除高亮显示

**查找操作：**

（以下直接输入/）

/(查找字符串) 按n向下搜索，按N向上搜索

**替换字符串**

\[n1\],\[n2\]s/\[需要替换的字符串\]/\[替换的字符串\](/g)

注：/g表示全局替换，没有则只替换每一行的第一个

\[n1\]：开始检索行号

\[n2\]：结束检索行号，可用 $ 表示无下限

1,$s#nologin#88888#g

把整个文件的nologin替换成888888

1,10s/nologin/88888/g

把1到10行的nologin替换成888888

# Day12

## 查看文件内容

### find + 格式 从指定目录查找文件

格式： find （目录） 选项 字符串

选项（按照什么方式查找）

- \-name &lt;查询方式&gt; 按照指定的文件名查找模式查找文件

find /root -name "\*.txt"

\# 查找所有.txt格式的文件

- \-mtime n 查找n天以前被修改或创建的所有文件。

find /root -mtime +3 查找3天之前

find /root -mtime -3 查找3天之内

- \-exec&lt;执行指令&gt;：假设find指令的回传值为True，就执行该指令；

find /root/xxx -name "\*.pdf" -exec rm -f {} \\;

代表找到的文件

注意：find不能用|传递执行命令，只能用-exec

注：文件名前可跟目录，且可并列文件

- \-size &lt;文件大小&gt; 按照指定的文件大小查找文件

查找大于2mb的文件

find /root -size +2m

find /root -name "\*.pdf" -a -size +2M

查找大于2mb的pdf文件

\-a表示and，同理可用-o表示or，也可省略

### head tail 根据查看行

head 文件名 （不加 -n 数字 默认开头十行）

tail 文件名 （不加 -n 数字 默认结尾十行）

head -n +3 文件名 #只显示前三行

tail -n -3 文件名 #只显示后三行

**注**：文件名前可跟目录，且可并列文件

### cat + 选项 查看文件所有内容

（concatenate）命令用于连接文件并打印到标准输出设备上

\-n 显示行号包括空行

\-b 跳过空白行编号

\-s 将所有的连续的多个空行替换为一个空行（压缩成一个空行）

cat -bs age.txt 跳过空白行编号，且压缩空白行

**注：**文件名前可跟目录，且可并列文件

### more 查看大内容

more 分屏查看文件（敲空格查看下一页）

### grep + 选项 过滤查找文件内容

以行为单位进行查找，显示结果为满足的行

**格式**：grep \[选项\] “目标字符串” 文件 \[文件...\]

\-c 统计满足的行数

\-v 反转不包含

\-i 忽略大小写

返回值：查找的字符串或统计的行数（-c）

grep "p" 1.txt 单文件搜索包含p的行

grep "P" 1.txt b.txt 多文件搜索

grep -v "p" 1.txt 单文件搜索不包含p的行

grep -c "p" 1.txt #统计出现多少行

grep "n$" 1.txt #现实以n**结尾**的行

grep "^n" 1.txt #现实以n**开头**的行

grep -i "a" 1.txt #单文件搜索包含a/A的行

### wc 统计文件

wc -c 文件 查看文件的字节数

wc -l 文件 查看文件的行数

注：wc -l统计的是换行符

### du 查看空间占用

查看文件占用大小

du -h 带单位

du -s 只统计每个参数所占用空间总的大小

du -s /home

只统计home目录的大小

du -sh （目录）

### 管道符 | 连接命令

ls -l | grep "^-" 查看当前目录下的所有目录

history | grep -c "ls" 统计使用ls命令的数量

## 编辑文件内容

### \> 和 >> 指令

\> 输出重定向(覆盖写), >> 追加（追加写）

grep "oracle" name.txt > s0415.txt 向文件覆盖写查找到的内容

若没有文件则新建文件

## 解压/压缩

### zip unzip

**压缩/解压格式：**

zip 文件名.zip 需要压缩的内容

unzip 文件名.zip

**选项**：

\-r：递归压缩，即压缩目录

\-d&lt;目录&gt; ：指定解压后文件的存放目录

**带目录压缩/解压：**

zip -r 目标文件名 路径 绝对路径压缩会带前面的路径文件夹

unzip 文件名.zip -d 目标目录

注：选项名和文件位置可以互换，zip压缩不能用绝对路径

### tar 解压/压缩

命令自带解压和压缩选项

**压缩**： tar -zcvf 目标文件名.tar.gz 需压缩文件夹

注：压缩最阿红用相对路径，用绝对路径会多压一层目录

**解压**： tar -zxvf 需解压文件名.tar.gz 目标文件夹

**选项**：

\-z 调用 gzip 程序进行压缩或解压

\-c 创建（Create）.tar 格式的包文件

\-x 解开.tar 格式的包文件

\-C </解压时指定释放的目标文件夹 指定目录

tar -zxvf a.tar.gz -C /root/ceshi/

注：-C要放置在目录前

\-v 输出详细信息（Verbose）

\-f 表示使用归档文件（一般都要带上表示使用tar,放在最后）

### Yum包管理

Yum是一个Shell前端软件包管理器。基于RPM包管理，能够从指定的服务器自动下载

RPM包并且安装，可以自动处理依赖性关系，并且一次安装所有依赖的软件包。

**使用**：

查询yum服务器是否有需要安装的软件 yum list | grep xxx

查询指定的yum包信息 yum info xxx

安装指定的yum包 yum install xxx

卸载指定的yum包 yum remove xxx

查看已安装的软件包 yum list installed

yum install ntpdate # 安装网络对时

## 帮助

man 命令——查看操作手册

命令 --help——查看帮助，有选项

## 用户权限

**用户切换**

su - 用户名

**用户登出**

回到上次登陆的用户，也可能登出

### 用户及用户组

类似于角色，系统可以对有共性的多个用户进行统一的管理。

groupadd 组名——新增用户组

useradd 用户名——添加用户

useradd -g 组名 用户名——添加用户时加上组

\-G 新账户的附加组列表

useradd -g oinstall -G dba oracle

创建用户oracle时加入oinstall主组和dba附组

passwd 用户名——指定/修改密码

id 用户名——查询用户信息

su - 用户名——切换用户

whoami——查看当前用户

usermod -g 用户组 用户名——修改用户的组

userdel 用户名——(exit退出后再删除) 删除用户

\-r 删除主目录和邮件池

groupdel 组名——删除组

杀死进程：

kill -9 进程编号

## 文件权限

### chmod 修改权限

通过 chmod 指令，可修改文件或目录的权限

\-R表示递归里面的所有文件及目录

**简单权限变更**

\+ 、-、= 变更权限

u:所有者 g:所有组 o:其他人 a:所有人(u、g、o的总和)

chmod u=rwx,g=rx,o=x 文件/目录名

chmod o+w 文件/目录名 给其他人可写权限

chmod a-x 文件/目录名 给所有人执行权限

可用数字表示为: r=4,w=2,x=1

例如 rwx=4+2+1=7

chmod 744 文件/目录名 自己有所有权限，组员有只读，其他人有只读权限

查目录内文件个数

1表示文件

2表示目录，且内部无内容

3表示目录，内有3-2个目录，同理，4表示有4-2个目录

### chown 修改文件所有者

结构：

chown 所有者:组 文件——修改文件权限

chown test02 /root/test.txt

chown -R 所有者:用户 目录——-R表示递归里面的所有文件及目录

用户/组可省略

# Day13

## 网络配置类

clear 清屏 ifconfig 列出网卡信息 ping ip地址 看网络是不是连通

### top 查看系统整体资源

PID：进程的标识符。 USER：运行进程的用户名。

PR（优先级）：进程的优先级。 NI（Nice值）：进程的优先级调整值。

VIRT（虚拟内存）：进程使用的虚拟内存大小。

RES（常驻内存）：进程实际使用的物理内存大小。

SHR（共享内存）：进程共享的内存大小。

%CPU：进程占用 CPU 的使用率。

%MEM：进程占用内存的使用率。

TIME+：进程的累计 CPU 时间。

### ps 显示系统执行的进程

ps -aux 查看所有用户所有进程

ps -ef 查看子父进程之间的关系

### pstree 查看进程树

pstree 1660 # 树状的形式显示进程的pid

### kill

最常用的信号是：

\-1 (HUP)：重新加载进程。

\-9 (KILL)：强制杀死一个进程。

\-15 (TERM)：正常停止一个进程。

kill -9 16989 杀死进程

### systemctl 服务管理

systemctl \[ start | stop | restart | status\] 服务名

service 服务名 \[ start | stop | restart | status\]

服务名：mysql network firewalld等

systemctl是新版本写法，service是老版本写法

防火墙操作：status/start/stop/restart/disable/enable 多两个

查看防火墙： systemctl status firewalld

停止防火墙： systemctl disable firewalld 重启后生效

注：单机版关闭火墙

### systemd 自定义服务

自定义服务三大要素：

\[Unit\]：控制单元，定义服务的描述信息、依赖关系、启动顺序等元信息

\[Service\]：服务的定义，服务的核心运行配置，最关键的部分

\[Install\]：定义服务安装相关的配置，主要用于设置开机自启时关联的系统目标

常见错误---虚拟机重启网卡失败或者网卡丢失

出现这种报错一般是和 NetworkManager 服务冲突导致的，直接关闭 NetworkManger

服务。

1.关闭NetworkManager ：service NetworkManager stop

2.禁止开机启动 ：systemctl disable NetworkManager

3.重启网络： service network restart

4.查看网络状态：systemctl status network

或者登录虚拟机，点击电源按钮选择有线连接即可

## plsql developer更改桥接文件

### mysql停启

## linux中的Oracle服务启动

1.  \[root@localhost ~\]# su - oracle #转到Oracle账户
2.  \[oracle@localhost ~\]$ sqlplus / as sysdba
3.  SQL> startup; #启动数据库
4.  SQL> quit;
5.  \[oracle@centos7 ~\]$ lsnrctl start #启动监听

**查看进程/服务**

ps -ef | grep "oracle"

**查看端口**

netstat -nltp

# Day14

## 扩展类

### echo 输出字符串

echo 输出双引号

echo '"\[内容\]"'

echo 输出单引号

echo "'\[内容\]'"

**echo选项**

\-n 不换行显示

\-e 出现转义字符进行解释处理

转义符：

/n——换行 /t——tab

**写入文本内容**

echo "test" > t.txt

**ech输出变量内容**

echo "$\[变量名\]"

### date 日期相关

**date 显示当前日期 (用于日期转字符串)**

date (显示当前时间)

date +"%Y" (显示当前年份)

date +"%Y-%m-%d %H:%M:%S" (显示当前是哪一天)

注：Y若使用小写则只会显示年份后两位

**date -d 日期解析（用于字符串转日期——显示指定的日期）**

date -d "2009-12-12"

date -d "2009-12-12 + 1 day"

date -d "+1 day"

date -d "+1 month"

date -d "+1 year"

date -d "2009-12-12 + 1 day" +"%Y/%m/%d %H:%M:%S" > time.txt

date -d "20200212 14:20:30"

**date 设置系统日期**

date -s "2023-08-08 12:34:56"

**linux网络对时**

1.安装netdate

yum install ntpdate

2.执行命令，同步时间。

ntpdate us.pool.ntp.org

### cal 查看日历

cal \[日\] \[月\] \[年\]

cal 显示当前日历

cal 2023 显示2023年日历

cal 01 2023 显示2023年1月日历

cal 15 01 2023 显示2023年1月15日日历

wget下载命令

wegat \[网址\]

### seq输出序列

seq \[选项\] 尾数

seq \[选项\] 首数 尾数

seq \[选项\] 首数 增量 尾数

注：增量和尾数可以为负数，但必须写完整

输出横向序列

seq -s \[","分隔符\] 首数 增量 尾数

### crontab 定时执行计划

方式一：

修改配置文件：/etc/crontab （要指明执行用户）

vim /etc/crontab

分 时 日 月 周 用户名 执行的命令

5 \* \* \* \* root date > /root/time.txt

每时的05分将时间覆盖写入/root/time.txt

方式二：通过crontab命令（不需要指明执行用户，默认就是当前用户）

crontab -e 注：编辑用户的cron配置文件；

crontab -l 注：查看用户的计划任务；

crontab -r 注：删除用户的计划任务；

crontab -e

5 \* \* \* \* date > /root/time.txt

注：命令的字符串有特殊字符时需要用\\转译

\*/2 \* \* \* \* root date +"\\%Y-\\%m-\\%d \\%H:\\%M\\:S

每2分钟写入当前时间

特殊符号

|     |     |
| --- | --- |
| **符号** | **含义** |
| \*  | 任何时间。比如第一个 \* 就代表一小时中每分钟都执行一次的意思。 |
| ,   | 不连续的时间。比如 0 8,12,16 \* \* \* ，在每天的8点0分，12点0分，16点0分都执行一次命令 |
| \-  | 连续的时间范围。比如 0 5 \* \* 1-6 ，在周一到六凌晨5点0分执行命令 |
| \*/n | 每隔多久执行一次。比如 \*/10 \* \* \* \* ，每隔10分钟就执行一遍命令 |

## shell脚本

Shell是一个命令行解释器，它为用户提供了一个向Linux内核发送请求以便运行程序的界

面系统级程序

### shell脚本执行

bash test.sh

sh test.sh

./test.sh #相对路径执行脚本

/root/shell/test.sh #绝对路径执行

chmod a+x ./test.sh #使脚本具有执行权限

# Day15

## 脚本操作

### 快速操作

ctrl + ?/ 键快速注释

### 查看运行过程日志（用于调试）：

bash -xe \[脚本\]

## 变量

注：变量等号两边不能有空格，否则赋不上值

### 自定义变量

定义变量：变量名=值

**定义命令的变量**

变量名=\`命令\` 需要用反引号\`\`引起来

变量名=$(命令) 需要用 $() 引起来

n=\`history | grep -c "ll"\`

echo "ll使用的次数：$n"

注：有些执行方式不能返回正确值，对于上例，仅有相对路径和绝对路径能返回正常值，但sh 和 bash 均不能返回正常值

### 系统变量

$HOME :当前登录用户的 "家目录" 路径

$USER：当前用户名

$RANDOM 可以随机生成 0~32767之间的整数数字

echo "$HOME"

注：注意区分大小写

### $ 特殊变量

$n n为number，$0代表该脚本名称，$1-$9代表第一到第九个参数

注：若使用相对路径或绝对路径，则会打印对应的路径

\[root@centos7 shell\]# ./t0422.sh 114 514 1918

echo "$3"

echo "$2"

echo "$1"

输出： 1918

514

114

$# 获取所有输入参数的个数，常用于循环；

$@ 代表命令行中所有的参数，$@会把每个参数区分对待；

$? 返回最后一次命令执行的状态，返回0代表正确执行，返回非0代表执行不正确。

### read 读取终端输入

\-p：指定读取值时的提示符；

\-t：指定读取值时限制的时间（秒）。

read -p "\[提示词\]" \[变量名\]

read -p "\[提示词\]" -t \[限制时间/s\] \[变量名\]

read -p "姓名：" -t 3 n7 # 限制输入时间为3s

echo "名字是：$n7"

注：可一次输入多个参数，用空格隔开

自定义函数输入参数大于定义参数时

func

## 运算

### 比较运算符

|     |     |     |     |     |     |     |     |     |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 数<br><br>值<br><br>比<br><br>较 | 符号  | 作用  | 字<br><br>符<br><br>串<br><br>比<br><br>较 | 符号  | 作用  | 类<br><br>型<br><br>比<br><br>较 | 符号  | 说明  |
| \-eq | 等于  | \== | 等于  | \-f | 存在且是文件 |
| \-ne | 不等于 | \-d | 存在且是目录 |
| \-lt | 小于  | \-r | 读 (read) |
| \-le | 小于等于 | !=  | 不等于 | \-w | 写(write) |
| \-gt | 大于  | \-x | 执行 (execute) |
| \-ge | 大于等于 |     |     |

### 算数运算符

**运算式：**

$((运算式)) 或 $\[运算式\]

**运算符：**

\+ , - , \*, /, %

### 逻辑运算符

## 选择结构

read -p "输入性别：" sex  
read -p "输入跑步时间：" n  
if \[ $sex == "男" -a $n -le 8 \]  
then  
echo "合格啦"  
elif \[ $sex == "女" -a $n -le 9 \]  
then  
echo "合格"  
else  
echo "回去重修"  
fi

### if 结构

if \[ 条件判断 \]

必有

必有

then

程序

elif \[ 条件判断 \]

then

程序

else

程序

fi #结尾

### case结构

case $变量名 in

"value 1"）

程序1

;;

"value 2"）

程序2

;;

\[…省略其他分支…\]

\*）

其他程序

;;

esac

## 循环结构

### for循环

**格式1:：传统循环**

for ((i=0; i < 数; i++))

do

&nbsp;   程序体

done

**格式2：用序列做循环**

for i in \`seq \[\]\`

do

&nbsp;   程序体

done

**格式3：用输入值$n循环**

for i in $#

do

&nbsp;   程序体

done

for i in $@

do

&nbsp;   程序体

done

**格式4：用字符串/文件循环**

name="111 222 333"

for i in $name

do

&nbsp;   程序体

done

list=\`ls /root\`

for i in $list

do

&nbsp;   程序体

done

### while 循环

while \[ ture \]

do

程序体

done

### rev倒转

ls -l | grep ".txt$" | rev

倒转查询到的文件信息

# Day16

### 打断程序

break 跳出循环——只会跳出离自己最近的循环

continue——打断本次循环，始下一次循环

exit——退出脚本

return——退出函数

## 自定义函数

注：shell脚本不会编译，函数必须写在调用前

### 基本结构

函数名(){ 函数体 }

### 函数调用

函数名 \[参数\] \[参数\]

### 函数返回值

可用echo返回值

注：参数可以为特殊变量$1-$9，即从命令行直接输入参数

若传入的参数未知，可用$@

### 本地参数

local 参数

尽在函数内使用，不会污染函数外变量

### 递归

函数调用自身的方法，使用需谨慎，注意结束条件

使用例：阶乘、遍历单个目录下的所有目录

递归例1：用递归计算阶乘

factorial(){

if \[ $1 -eq 1 -o $1 -eq 0 \]

then

echo 1

else

local temp=$(factorial $(($1-1)))

echo $(( $1\*$temp ))

fi    

}

递归例2：用递归拼接

joint(){

if \[ $1 -eq 1 -o $1 -eq 0 \]

then

echo 1

else

local temp=$(joint $(($1-1)))

&nbsp;   echo "$1×$temp"

fi

}

输出：1×2×3×...×n

# Systemd

全称system daemon

systemd 是 Linux 系统中一套主流的系统和服务管理器，主要负责系统启动过程的初始

化、后台服务的生命周期管理（启动、停止、重启等）

## 自定义服务

定义服务，将服systemd 管理的 .service 单元文件放入特定目录，可由systemctl命令调用

**服务存放文件：**

|     |
| --- |
| /etc/systemd/system/ ——优先级最高 |
| /usr/lib/systemd/system |
| /lib/systemd/system |

## Service文件

用于定义系统服务的配置信息（如启动命令、依赖关系、运行权限等）

service文件构成：

\[Unit\] \[Service\] \[Install\]

### \[Unit\]

控制单元，定义服务的描述信息、依赖关系、启动顺序等元信息

|     |     |
| --- | --- |
| **Unit选项** | **说明** |
| Description | 对当前服务的简单描述 |
| After | 指定在哪些服务之后进行启动 |

### \[Service\]

服务的定义，服务的核心运行配置，最关键的部分

|     |     |
| --- | --- |
| **Service选项** | **说明** |
| Type | 指定启动类型(见下表) |
| User | 指定运行服务的用户 |
| ExecStart | 指定服务启动时执行的命令(必选) |
| Restart | 指定服务崩溃/退出后的重启策略(见下表) |
| RestartSec | 指定服务在重启时等待的时间，单位为秒 |

|     |     |
| --- | --- |
| **Type选项** | **说明** |
| simple | 指定ExecStart字段的进程为主进程 |
| forking | 指定以fork()子进程执行ExecStart字段的进程 |

|     |     |
| --- | --- |
| **Restart选项** | **说明** |
| always | 无论何种原因退出都重启（包括正常退出、被手动停止后） |
| no  | 任何情况都不重启，一次性任务 |
| on-failure | 非0状态码退出、被异常信号终止、超时等 |

### \[Install\]

定义服务安装相关的配置，主要用于设置开机自启时关联的系统目标

|     |     |
| --- | --- |
| **Type选项** | **说明** |
| WantedBy | 被哪些units所依赖，弱依赖 |
| RequiredBy | 被哪些units所依赖，强依赖 |

注：\[Install\] 一般填为WantedBy=multi-user.target ，表示服务在多用户模式（默认运行级别）下开机自启

### service文件示例

\[Unit\]

\# 服务描述

Description=my test Service

\# 表示该服务在network.target启动后再启动

After=network.target

\[Service\]

\# 服务类型（simple 表示执行 ExecStart 后立即启动）

Type=simple

\# 运行服务的用户（根据需求修改，如普通用户）

User=root

\# 服务启动命令（脚本/程序路径）

ExecStart=/usr/local/bin/\[服务名\].sh

\# 服务退出后自动重启（推荐on-failure）

Restart=always

\# 重启间隔（秒）

RestartSec=5

\[Install\]

\# 服务安装目标（多用户模式下启动）

WantedBy=multi-user.target

### 服务命令

|     |     |
| --- | --- |
| systemctl daemon-reload | 重新加载配置文件 |
| systemctl start \[服务名\] | 启动服务 |
| systemctl enable \[服务名\] | 开机自启动服务 |
| systemctl status \[服务名\] | 查看当前状态 |
| systemctl stop \[服务名\] | 停止服务 |
| journalctl -u \[服务名\] -f | 查看日志 |

注：建好服务后一般要 重新加载配置文件 再启动服务

# Day17

## Shell工具

### sort 文件内容排序

将文件进行排序，并将排序结果标准输出

|     |     |
| --- | --- |
| **选项** | **说明** |
| \-n | 依照数值的大小排序 |
| \-r | 以相反的顺序来排序 |
| \-t | 设置排序时所用的分隔字符 |
| \-k | 指定需要排序的列 |

sort -t ":" -nrk 3 /root/shell/sort.txt

以 : 为分隔符，第三列降序来排序

## 数据清洗

### grep

更适合单纯的查找或匹配文本

### sed 打印、修改内容

更适合编辑匹配到的文本

处理时，把当前处理的行存储在缓冲区中处理，处理完成后把缓冲区的内容送往屏幕。接着处理下一行，直到文件末尾

结构：sed \[选项\] '\[选项\]/\[查找内容\]/\[替换内容\]/\[选项\]' 文件地址

|     |     |
| --- | --- |
| **选项** | **作用** |
| p 打印 | 将选择的数据打出，**通常 p 会与参数 sed -n 一起运行** |
| i 插入 | i 后面可接字串，会在新的一行出现(目前的上一行)； |
| a 新增 | a 的后可接字串，会在新的一行出现(目前的下一行) |
| s 取代 | 可以直接取代目标字串，可以搭配正则表达式使用(^ $等) |
| d 删除 | 删除包含元素的整行内容 |

注：将''换成""可调用自定义参数

\# 显示文件的第2行的内容(用P打印，需配合-n)

sed -n '2,4 p' /root/shell/sort.txt

\# 以文件bb开头的上一行添加一个he11 ^ $

sed '/^bb/i hello' /root/shell/sort.txt

\# 将文件中的bb全部替换为BB g表示全局替换

sed 's/bb/BB/g' /root/shell/sort.tx

注：sed s///g中需要转义的字符：\[\] \\ /

#将文件中含有b的行全部删除 参数 d

sed '/b/d' /root/shell/sort.txt

\# 管道符 | 连续处理，使用\\换行 \\后不能有空格

sed 's/bb/BB/g' /root/shell/sort.txt \\

| sed '/2$/a你好' \\

| sed 's/0/0000/g' >> /root/shell/sort.tx

\# 用 >> 续写到文件

### awk 打印、修改结构

更适合格式化文本，对文本进行较复杂格式处理

**基本结构**

awk \[options\] 'BEGIN{ commands } { commands } END{ commands }' file

\-v 指定 FS 和 OFS 字段分隔符和输出字段分隔符

内置参数：

|     |     |
| --- | --- |
| NF  | 分割完字段的字段数量 |
| $NF | 分割后和列的数量匹配的那一列(最后一列) |
| NR  | 显示每一行的行号 |
| $1  | 代表文本行中的第 1 个数据字段 |
| $2  | 代表文本行中的第 2 个数据字段 |

输出指定列：{print $1,$2}

$0 输出一整行；分隔符相同的情况输出一整行：{print}

1.以:为分隔符，打印第2列和第1列

awk -v FS=":" '{print $2,$1}' /root/shell/sort.txt

2.以:为分隔符，打印第2列和第1列，列之间用,分割

awk -v FS=":" -v OFS="," '{print $2,$1}' /root/shell/sort.txt

3.添加列保存为csv，下载，使用excel查看

awk -v FS=":" -v OFS="," 'BEGIN{print "one,two,three"}{print $2,$1,$3}'

/root/shell/sort.txt > /root/shell/sort.csv

\# 打印第2至4行

awk 'NR==2,NR==4 {print NR"$0}' /root/shell/sort.txt

\# 注：打印2和4行用：NR==2 || NR==4

\# 5.用 awk 打印2的行数，以及2的倍数行数

awk '{if(NR%2==0){print NR" ",$0}}' /root/wangka.txt

awk 'NR%2==0{print NR" ",$0}' /root/wangka.txt

**awk带函数使用**

\# 用 awk 打印2的行数，以及2的倍数行数

awk '{if(NR%2==0){print NR" ",$0}}' /root/wangka.txt

**指定特定的分隔符：**

区别于外部定义的分隔符，指定特定位置的分隔符用""包含

\-v OFS="," '{print $18,$8,$2,$4,$6,$10":"$11,$13,$15":"$16,$20,$22}'

普通位置用 , 作为分隔符，指定位置指定用 : 作为分隔符

### awk使用多个分隔符

**结构一：**awk -F "\[:,\]" ≈ awk -F '分隔符|分隔符'

**结构二：**awk 'BEGIN{FS="\[,-:.\]"} {print $1,$2,$3}'

注：\[\]内放多个分隔符，无区分标志，放多少个符号就有多少种分隔符

**多个分隔符+函数**

awk -F "\[多个分隔符\]" '{for(i=1;i<=NF;i+=2)printf "%s ",$i;print ""}'

\# 多个分隔符分列，再取其中的单数列 "%S "指定列之间的分隔符，这里为空格

printf表示输出不换行, print ""用来换行

**使用多个分隔符分隔后，指定分隔符，打印所有行**

- {$1=$1}1

awk -F '\_|-' -v OFS="," '{$1=$1}1' 文件名

- {$0=$0; print}

awk -F '\_|-' -v OFS="," '{$0=$0; print}' 文件名

- {$1=$1; print $0}

awk -F '\_|-' -v OFS="," '{$1=$1; print $0}' 文件名

- for循环

awk -F '\_|-' -v OFS="," '{for(i=1;i<=NF;i++) printf "%s%s", $i, (i==NF ? ORS : OFS)}' 文件名

\# 外部指定分隔符

awk -F '\_|-' '{for(i=1;i<=NF;i++) printf "%s分隔符",$i;print ""}'

\# 内部指定分隔符

### 简单数据清洗

1\. 掐头去尾

去除头尾不重复的部分，剩下重复的部分

2.找换行

找到应该换行的部分，替换为\\n

3.去除标题，对齐列

4.awk BEGING加一行标题输出到.csv文件

## 连接数据库

1.dbever

2.命令行 mysql -uroot -p

show databases; 查看数据库

3.shell脚本内用mysql命令连接

## mysql命令构成

是MySQL数据库服务器的客户端工具，它工作在命令行终端中，完成对远程MySQL数据库服务器的操作。

### shell脚本操作

|     |     |
| --- | --- |
| \-h | MySQL服务器的ip地址或主机名； |
| \-u | 连接MySQL服务器的用户名； |
| \-e | 执行mysql内部命令； |
| \-p | 连接MySQL服务器的密码。 |
| \-P | 连接MySQL服务器的端口 |

mysql -h127.0.0.1 -P3306 -uroot -proot123456 test -e "select \* from student"

注：除 -e 外几个命令可以调换位置

命令只能写入 -e ""内，且每句命令都要有前面的部分

可用变量替代命令

### 从csv导入数据

步骤：

1.  创建表格
2.  将数据文件.csv导入到/usr/local/mysql/data目录下
3.  使用**导入数据命令**将其导入

建立mysql数据库时指定的数据文件

**导入数据命令：**

csvin="LOAD DATA INFILE '/usr/local/mysql/data/ip.csv' INTO TABLE ip

CHARACTER SET utf8 # 字符串设置为utf8字体避免乱码

FIELDS TERMINATED BY ',' #列的分隔符

LINES TERMINATED BY '\\n' #行的分隔符

IGNORE 1 LINES " #省略开头几行

mysql -h$host -P$port -u$user -p$passwd $dbname -e "$csvin"

### mysqldump 从数据库导出数据

1.导出数据库里面的表的表数据和表结构

mysqldump -u\[用户名\] -h\[ip\] -p\[密码\] -P\[端口号\] 数据库名 表1 表2 表3 > 文件名.sql

注：若不写表名则导出整个数据库

不写 > 文件名 则打印在控制台内

文件不写路径，文件会生成到根目录/下，一定要写路径

2.只导出表结构不导表数据——添加"-d"命令参数

mysqldump -u\[用户名\] -h\[ip\] -p\[密码\] -P\[端口号\] -d 数据库名 表名 > 文件名.sql

3.只导出表数据不导表结构——添加"-t"命令参数

mysqldump -u\[用户名\] -h\[ip\] -p\[密码\] -P\[端口号\] -t 数据库名 表名 > 文件名.sql

4.导出完整数据库，适合整库备份、迁移、多库备份——添加"--databases“命令参数

导出命令:

mysqldump -u\[用户名\] -h\[ip\] -p\[密码\] -P\[端口号\] 数据库1 --databases 数据库1 > 文件名.sql

5.同时导入多个数据库:

mysql -u\[用户名\] -h\[ip\] -p\[密码\] -P\[端口号\] < 文件名.sql

注：该方法的前提是SQL文件必须是 --databases 备份出来的，否则只能指定数据库名导入