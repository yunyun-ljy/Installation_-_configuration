# Day37

## hadoop构成

### HDFS 存储数据

NameNode——文件目录结构

DataNode(dn)——文件块数据，块数据的校验

SecondaryNameNode(2nn)——定时备份NameNode元数据进行

### 分布式计算MapReduce

### Yarn 资源调度

ResourceManager (资源管理器):YARN集群中的中心调度器和资源管理器。负责整个集群的资源分配和调度 监控集群中的计算资源任务的运行状态

NodeManager (节点管理器):每个计算节点上运行的代理程序负责管理和监控节点上的资源和任务。接收来自RM的任务调度请求;启动、停止和监控任务的执行;发送节点的状态和可用资源报告

ApplicationMaster(应用程序管理器)：每个应用程序在YARN中都有一个对应的AM.AppMaster负责协调和管理应用程序的执行。它与RM交互申请资源并监任务的执行。它还负责任务的划分和调度、容错和恢复、进度跟踪等。

# Day38

## hive & hadoop

### Hadoop

是大数据底层平台，包含三大核心：

HDFS：大数据分布式存储（存海量数据）

YARN：资源调度（分配算力）

MapReduce：分布式计算（处理海量数据）

缺点： 只能用复杂的 Java 代码写计算逻辑，普通人不会。

### Hive

它是基于 Hadoop 的数据仓库工具，作用只有一个： 把复杂的大数据计算，变成简单的 SQL 语句！

你写一条 Hive SQL → 它自动翻译成 MapReduce 程序 → 交给 Hadoop 运行。

它们的关系（最重要）

Hive 依赖 Hadoop 才能运行

Hive 不存数据，数据存在 HDFS（Hadoop）里

Hive 不做计算，计算交给 MapReduce / Tez / Spark（Hadoop）

Hive 只是一个 “翻译官 + 入口”

Hive默认在HDFS的工作目录默认

/user/hive/warehouse

hive登录命令

beeline -u jdbc:hive2://localhost:10000 -n root -p 123456

hive的有些约束要在后面加入NOT ENFORCED

检查hive是否启动：用登录命令

beeline -u jdbc:hive2://localhost:10000 -n root -p 12345

查看nohup.out有四条hive session后可尝试用bdever连接

### 查看数据文件信息

192.168.200.100:9870

### 查看操作历史

192.168.200.100:19888

### Hive默认在HDFS的工作目录

/user/hive/warehouse</value

## 虚拟机扩容

#1.查看当前磁盘占用情况

df -lh

#2. 查看新增物理磁盘盘符

ls /dev/sd\*

#3.查看新增物理情况磁盘

fdisk -l

sda(使用中)

sdb

centos组

#4.创建物理卷

pvcreate /dev/sdb

注：若已有sdb则新建sdc，以此类推

#5.查看根目录所在卷组名称

vgdisplay/vgs # 卷积组——>获取VG name

pvdisplay/pvs # 物理卷

lvdisplay/lvs # 逻辑卷

sda(使用中)

sdb

centos组

#6.使用新增物理卷扩容根目录卷组

vgextend centos /dev/sdb

#7.扩容根目录所在逻辑卷大小(按照G扩容)

lvextend -L +500G /dev/mapper/centos-root

或

lvextend -l +100%FREE /dev/mapper/centos-root

sda(使用中)

sdb

centos组

#8.重新读取根目录逻辑卷信息

xfs_growfs /dev/mapper/centos-root

#9.查看扩容后 磁盘占用

df -lh

#10.查看磁盘挂载

lsblk

## hive创建索引

### 1\. 创建索引（延迟重建）

CREATE INDEX index_saleno

ON TABLE ods_u_sale_pay(saleno)

AS 'COMPACT'

WITH DEFERRED REBUILD;

COMPACT（紧凑型索引，最常用）

存储：索引列值 + HDFS 文件偏移量、块位置

适用：高基数字段（id、手机号、订单号等唯一 / 不重复字段）

BITMAP（位图索引）

完整类：org.apache.hadoop.hive.ql.index.bitmap.BitmapIndexHandler

适用：低基数字段（性别、状态、分类，取值只有少数几种）

### 2\. 重建索引，写入索引数据

**全表重建索引，会全表扫面**

ALTER INDEX index_saleno ON ods_u_sale_pay REBUILD;

**按分区重建索引**

ALTER INDEX index_saleno ON ods_u_sale_pay PARTITION(year='2018') REBUILD;

### 3\. 开启索引优化参数

SET hive.optimize.index.filter=true; SET hive.optimize.index.filter.compact.minsize=0; SET hive.input.format=org.apache.hadoop.hive.ql.io.HiveInputFormat;

# Day39

## hdfs命令

### Hadoop平台-进程启停命令

#1.日志服务启停命令

mapred --daemon start historyserver

#2.HDFS文件系统服务启停命令

hdfs --daemon start namenode/datanode/secondarynamenode

#3.Yarn服务启停命令

yarn --daemon start/stop resourcemanager/nodemanager

#4.查看各个节点hdfs文件系统状态

hdfs dfsadmin -report

#5查看各个节点yarn运行状态

yarn node -list -all

http://192.168.200.101:8088/cluster/nodes

hdfs namenode -format

#hdfs 安全模式 关闭

hdfs dfsadmin -safemode leave

#hdfs 安全模式 强制关闭

hdfs dfsadmin -safemode forceExit

### 命令模板

hadoop dfs +命令

|     |     |
| --- | --- |
| \-appendToFile &lt;localsrc&gt; ... &lt;dst&gt; | 将本地目录追加到HDFS下的某个文件中。 |
| \-cat \[-ignoreCrc\] &lt;src&gt; ... | 查看某个文件的内容。 |
| \-checksum &lt;src&gt; ... | 计算并确认文件的校验和。 |
| \-chgrp \[-R\] GROUP PATH... | 修改文件或目录的所属用户组。 |
| \-chmod \[-R\] &lt;MODE\[,MODE\]... \| OCTALMODE&gt; PATH... | 修改文件或目录的权限。 |
| \-chown \[-R\] \[OWNER\]\[:\[GROUP\]\] PATH... | 修改文件或目录的所属用户。 |
| \-copyFromLocal \[-f\] \[-p\] \[-l\] \[-d\] \[-t &lt;thread count&gt;\] &lt;localsrc&gt; ... &lt;dst&gt; | 从本地复制文件到HDFS。 |
| \-copyToLocal \[-f\] \[-p\] \[-ignoreCrc\] \[-crc\] &lt;src&gt; ... &lt;localdst&gt; | 从HDFS复制文件到本地。 |
| \-count \[-q\] \[-h\] \[-v\] \[-t \[&lt;storage type&gt;\]\] \[-u\] \[-x\] \[-e\] &lt;path&gt; ... | 统计文件或目录的数量。 |
| \-cp \[-f\] \[-p \| -p\[topax\]\] \[-d\] &lt;src&gt; ... &lt;dst&gt; | 在HDFS文件系统中拷贝文件。 |
| \-createSnapshot &lt;snapshotDir&gt; \[&lt;snapshotName&gt;\] | 创建指定目录的快照。 |
| \-deleteSnapshot &lt;snapshotDir&gt; &lt;snapshotName&gt; | 删除指定目录下的快照。 |
| \-df \[-h\] \[&lt;path&gt; ...\] | 查看文件系统剩余空间。 |
| \-du \[-s\] \[-h\] \[-v\] \[-x\] &lt;path&gt; ... | 计算文件或目录的磁盘使用情况。 |
| \-expunge | 清空HDFS垃圾箱。 |
| \-find &lt;path&gt; ... &lt;expression&gt; ... | 在指定路径下查找符合条件的文件。 |
| \-get \[-f\] \[-p\] \[-ignoreCrc\] \[-crc\] &lt;src&gt; ... &lt;localdst&gt; | 从HDFS**下载**文件到本地。 |
| \-getfacl \[-R\] &lt;path&gt; | 获取文件或目录的ACL（访问控制列表）信息。 |
| \-getfattr \[-R\] {-n name \| -d} \[-e en\] &lt;path&gt; | 获取文件或目录的扩展属性信息。 |
| \-getmerge \[-nl\] \[-skip-empty-file\] &lt;src&gt; &lt;localdst&gt; | 将多个文件合并为一个文件并下载到本地。 |
| \-head &lt;file&gt; | 查看文件的开头部分内容。 |
| \-help \[cmd ...\] | 获取HDFS命令的帮助。 |
| \-ls \[-C\] \[-d\] \[-h\] \[-q\] \[-R\] \[-t\] \[-S\] \[-r\] \[-u\] \[-e\] \[&lt;path&gt; ...\] | 查看指定路径的文件或目录列表。 |
| \-mkdir \[-p\] &lt;path&gt; ... | 创建新的文件夹。 |
| \-moveFromLocal &lt;localsrc&gt; ... &lt;dst&gt; | 从本地移动文件到HDFS。 |
| \-moveToLocal &lt;src&gt; &lt;localdst&gt; | 将文件从HDFS移动到本地。 |
| \-mv &lt;src&gt; ... &lt;dst&gt; | 在HDFS文件系统内移动文件。 |
| \-put \[-f\] \[-p\] \[-l\] \[-d\] &lt;localsrc&gt; ... &lt;dst&gt; | **上传文件到HDFS。** |
| 例：hadoop dfs -put /root/Day39/student2_final.csv /Day39<br><br>将/root内的文件复制到HDFS内部的文件夹/Day39下 |     |
| \-renameSnapshot &lt;snapshotDir&gt; &lt;oldName&gt; &lt;newName&gt; | 重命名指定目录下的快照。 |
| \-rm \[-f\] \[-r\|-R\] \[-skipTrash\] \[-safely\] &lt;src&gt; ... | 删除文件或目录（只能删除空文件夹）。 |
| \-rmdir \[--ignore-fail-on-non-empty\] &lt;dir&gt; ... | 删除空文件夹，可以使用\`--ignore-fail-on-non-empty\`选项删除非空文件夹。 |
| \-setfacl \[-R\] \[{-b\|-k} {-m\|-x &lt;acl_spec&gt;} &lt;path&gt;\]\|\[--set &lt;acl_spec&gt; &lt;path&gt;\] | 设置文件或目录的ACL（访问控制列表）。 |
| \-setfattr {-n name \[-v value\] \| -x name} &lt;path&gt; | 设置文件或目录的扩展属性。 |
| \-setrep \[-R\] \[-w\] &lt;rep&gt; &lt;path&gt; ... | 设置文件的副本数量。 |
| \-stat \[format\] &lt;path&gt; ... | 显示文件或目录的状态信息。 |
| \-tail \[-f\] \[-s &lt;sleep interval&gt;\] &lt;file&gt; | 查看文件的末尾部分内容。 |
| \-test -\[defsz\] &lt;path&gt; | 测试文件的存在性、目录的空或非空等属性。 |
| \-text \[-ignoreCrc\] &lt;src&gt; ... | 以文本形式查看文件的内容。 |
| \-touch \[-a\] \[-m\] \[-t TIMESTAMP \] \[-c\] &lt;path&gt; ... | 创建一个空文件或者更新已有文件的时间戳。 |
| \-touchz &lt;path&gt; ... | 创建一个空文件。 |
| \-truncate \[-w\] &lt;length&gt; &lt;path&gt; ... | 清空文件内容或者将文件截断到指定的长度。 |
| \-usage \[cmd ...\] | 显示HDFS命令的用法信息。 |

## hive 语句

### 内置命令

\-- 查看系统自带的函数

show functions;

\-- 显示自带的函数的用法

desc function upper;

\-- 详细显示自带的函数的用法

desc function extended upper;

### 元数据查看语句

\--查看数据库

show database db_hive

\--过滤查看数据库

show databases like 'db_hive\*';

\--查看表详情

desc database db_hive

desc database extended db_hive;

\--查看表

show tables;

\--查看表列详情

desc dept;

\--查看表所有详细信息

desc extended emp;

show formatted emp;

\--查看分区信息

show partitions emp;

### hive 建库语句

\-- 创建数据库

CREATE DATABASE \[IF NOT EXISTS\] database_name

\[COMMENT database_comment\]

\[LOCATION hdfs_path\]

### hive建表语句

|     |     |
| --- | --- |
| CREATE \[EXTERNAL\] TABLE \[IF NOT EXISTS\] table_name | 内部表 |
| \[(col_name data_type \[COMMENT col_comment\], ...)\] | 数据类型 |
| \[COMMENT table_comment\] |     |
| \[PARTITIONED BY (col_name data_type \[COMMENT col_comment\], ...)\] | 分区表 |
| \[CLUSTERED BY (col_name, col_name, ...) |     |
| \[SORTED BY (col_name \[ASC\|DESC\], ...)\] INTO num_buckets BUCKETS\] | 分桶表 |
| ROW FORMAT DELIMITED | **数据格式** |
| FIELDS TERMINATED BY '' | **列分隔** |
| COLLECTION ITEMS TERMINATED BY ',' | 复合数据item分隔 |
| MAP KEYS TERMINATED BY ':' | 复合数据key分隔 |
| LINES TERMINATED BY ',' | **行分隔** |
| \[STORED AS file_format\] | 压缩格式 |
| \[LOCATION hdfs_path\] | 表数据文件存储路径 |
| \[TBLPROPERTIES (property_name=property_value, ...)\] | 内外部表转换 |
| TBLPROPERTIES ("skip.header.line.count"="1") | **跳过第一行** |
| 补充：修改表格为跳过第一行：<br><br>ALTER TABLE student SET TBLPROPERTIES ("skip.header.line.count"="1"); |     |
| \[AS select_statement\] |     |
| \-ls \[-C\] \[-d\] \[-h\] \[-q\] \[-R\] \[-t\] \[-S\] \[-r\] \[-u\] \[-e\] \[&lt;path&gt; ...\] | 查看指定路径的文件或目录列表。 |
| \-mkdir \[-p\] &lt;path&gt; ... | 创建新的文件夹。 |
| \-moveFromLocal &lt;localsrc&gt; ... &lt;dst&gt; | 从本地移动文件到HDFS。 |
| \-moveToLocal &lt;src&gt; &lt;localdst&gt; | 将文件从HDFS移动到本地。 |
| \-mv &lt;src&gt; ... &lt;dst&gt; | 在HDFS文件系统内移动文件。 |
| **\-put** \[-f\] \[-p\] \[-l\] \[-d\] &lt;localsrc&gt; ... &lt;dst&gt; | 上传文件到HDFS。 |
| \-renameSnapshot &lt;snapshotDir&gt; &lt;oldName&gt; &lt;newName&gt; | 重命名指定目录下的快照。 |
| \-rm \[-f\] \[-r\|-R\] \[-skipTrash\] \[-safely\] &lt;src&gt; ... | 删除文件或目录（只能删除空文件夹）。 |
| \-rmdir \[--ignore-fail-on-non-empty\] &lt;dir&gt; ... | 删除空文件夹，可以使用\`--ignore-fail-on-non-empty\`选项删除非空文件夹。 |
| \-setfacl \[-R\] \[{-b\|-k} {-m\|-x &lt;acl_spec&gt;} &lt;path&gt;\]\|\[--set &lt;acl_spec&gt; &lt;path&gt;\] | 设置文件或目录的ACL（访问控制列表）。 |
| \-setfattr {-n name \[-v value\] \| -x name} &lt;path&gt; | 设置文件或目录的扩展属性。 |
| \-setrep \[-R\] \[-w\] &lt;rep&gt; &lt;path&gt; ... | 设置文件的副本数量。 |
| \-stat \[format\] &lt;path&gt; ... | 显示文件或目录的状态信息。 |
| \-tail \[-f\] \[-s &lt;sleep interval&gt;\] &lt;file&gt; | 查看文件的末尾部分内容。 |
| \-test -\[defsz\] &lt;path&gt; | 测试文件的存在性、目录的空或非空等属性。 |
| \-text \[-ignoreCrc\] &lt;src&gt; ... | 以文本形式查看文件的内容。 |
| \-touch \[-a\] \[-m\] \[-t TIMESTAMP \] \[-c\] &lt;path&gt; ... | 创建一个空文件或者更新已有文件的时间戳。 |
| \-touchz &lt;path&gt; ... | 创建一个空文件。 |
| \-truncate \[-w\] &lt;length&gt; &lt;path&gt; ... | 清空文件内容或者将文件截断到指定的长度。 |
| \-usage \[cmd ...\] | 显示HDFS命令的用法信息。 |

### hive 数据操作语句

**\--load从特定路径导入数据**

load data \[local\] inpath '数据的path' \[overwrite\] into table student \[partition (partcol1=val1,…)\];

例：

从默认路径（/user/hive/warehouse/数据文件所在子目录）导入（不用写路径）

LOAD DATA INPATH INTO TABLE student;

**从HDFS指定路径**

LOAD DATA INPATH '/Day39/student.csv' INTO TABLE student;

**从本机系统指定路径**

LOAD DATA LOCAL INPATH '/Day39/student.csv' INTO TABLE student;

**\--上传hdfs**

dfs -put /opt/module/hive/datas/student.txt /user/atguigu/hive;

**\--插入数据**

insert into table student_par values(1,'wangwu'),(2,'zhaoliu');

**\--覆盖插入并且使用结果集进行插入**

insert overwrite table student_par select id, name from student ;

insert overwrite local directory '数据的path'

ROW FORMAT DELIMITED FIELDS TERMINATED BY '\\t' 查询语句;

行分割

**\--hadoop 导出**

dfs -get /user/hive/warehouse/student/student.txt

/opt/module/datas/export/student3.txt;

**\--hive shell导出**

bin/hive -e 'select \* from default.student;' >

/opt/module/hive/datas/export/student4.txt;

**\--export导出**

export table default.student to

'/user/hive/warehouse/export/student';

**例：**

SELECT \[ALL | DISTINCT\] select_expr, select_expr, ...

&nbsp; FROM table_reference

&nbsp; \[WHERE where_condition\]

&nbsp; \[GROUP BY col_list\]

&nbsp; \[ORDER BY col_list\]

&nbsp; \[CLUSTER BY col_list | \[DISTRIBUTE BY col_list\] \[SORT BY col_list\]

&nbsp; \]

&nbsp;\[LIMIT number\]

# Day40

## hive复合数据

### 数组array

储存形式：\[值,值,...,值\]

调用：列名\[索引\]

建表语句：列名 array&lt;数据类型&gt;

导入数据格式：值1;值2;值3;...

注：此处用 ; 作为分隔符，下同

构造复合数据-array

select array(值,值) from student

### 集合struct

储存形式：\[{键:值,键:值,...,键:值}\]

调用：列名.键

建表语句：列名 struct&lt;键1:值的数据类型,键2:值的数据类型...&gt;

导入数据格式：值1;值2;值3;...

select named_struct(key,value,key,value)

from student

### 字典map

储存形式：{键1:值1,键2:值2,...,键:值}

调用：列名\[键\]

建表语句：列名 map&lt;键的数据类型,值的数据类型&gt;

导入数据格式：键1:值1;键2:值2;...

构造复合数据

select map(key,value,key,value)

from student

注：符合数据建表时末尾经常配合以下命令使用

|     |     |
| --- | --- |
| \[COLLECTION ITEMS TERMINATED BY ';'\] | 复合数据item分隔 |
| \[MAP KEYS TERMINATED BY ':'\] | 复合数据key分隔 |

## Git命令

1\. git init：创建本地仓库

2\. 仓库区和工作区

①.git文件夹为仓库区：类似于一个数据存储着每一次提交的变化

②.git所在目录称为工作区,我们在这里创建项目和其他文件

3\. git add&lt;文件名&gt;：把文件添加到暂存区，暂存区存储将要被提交的文件变化

4\. git commit -m “创建a.txt文件”：提交暂存区存储变化并生成一个新的版本

5\. git status：查看命令状态

6\. git log：查看日志

## 克隆git

新建git，复制地址

到文件夹 git clone git链接

# Day41

## 分区表操作

分区就是把大表，按某个字段（比如日期、地区、品牌）拆成很多小文件夹。

核心作用：

1.  大幅加快查询速度 不分区：查一天数据要扫描全表 → 慢 分区：只查对应日期文件夹 → 极快
2.  减少数据扫描量——只读取需要的分区，不读无用数据
3.  方便数据管理——按天分区，可单独删除 / 覆盖某天数据
4.  优化统计、报表类 SQL

### 创建分区表

create table dept_partition(

deptno int, dname string, loc string

)

partitioned by (day\[分区字段\] string) # 额外语句

row format delimited

fields terminated by ','

lines terminated by ''

注：分区字段名不能与表的列名相同

## 静态分区表配置

### 分区表数据导入

load data local inpath '/opt/module/hive/datas/dept_20200401.log' into table dept_partition partition(day='20200401');

### select分区表插入数据

insert into table log_list_6 partition(dat='20221231')

select \* from log_list_tmp

插入动态分区

insert into table log_list_6 partition(dat)

select \* from log_list_tmp

### 查看分区

show partitions tab_name;

### 添加分区

alter table dept_partition add partition(day='20200404')

### 添加多分区

alter table dept_partition add partition(day='20200405') partition(day='20200406');

### 删除分区

alter table dept_partition drop partition (day='20200406');

### 查看分区表信息

show partitions dept_partition;

### 查看分区表结构

desc formatted dept_partition;

### 修改分区表

ALTER TABLE table_name PARTITION (dt='2008-08-08') SET LOCATION "new location";

ALTER TABLE table_name PARTITION (dt='2008-08-08') RENAME TO PARTITION (dt='20080808');

## 动态分区表配置

\--开启动态分区(默认开启)

set hive.exec.dynamic.partition=true

\--指定非严格模式 nonstrict模式表示允许所有的分区字段都可以使用动态分区

set hive.exec.dynamic.partition.mode=nonstrict

\--在所有执行MR的节点上，最大一共可以创建多少个动态分区。默认1000

set hive.exec.max.dynamic.partitions=1000

\--在每个执行MR的节点上，最大可以创建多少个动态分区(分区字段有多少种设多少个)

set hive.exec.max.dynamic.partitions.pernode=100

\--整个MR Job中，最大可以创建多少个HDFS文件。默认100000

set hive.exec.max.created.files=100000

\--当有空分区生成时，是否抛出异常

set hive.error.on.empty.partition=false

\--打开正则查询模式

set hive.support.quoted.identifiers=none

### select插入动态分区表数据

首先建表，表的列一般与原表相同

首先打开正则模式，使用正则表达式排除掉原表的分区列：

insert into table supermarket_year partition(exyear)

select \`(dest_area)?+.+\`, SUBSTR(exch_date,1,4) from supermarket_p

若原表无分区则可直接用select \*

# Day42

## 安装pyhive库

pip3 install -i https://mirrors.aliyun.com/pypi/simple/ thrift

pip3 install -i https://mirrors.aliyun.com/pypi/simple/ thrift-sasl

pip3.6 install -i https://mirrors.aliyun.com/pypi/simple/ PyHive

pip3.6 install -i https://mirrors.aliyun.com/pypi/simple/ PyM

注：pip3.6可换为其他版本

# Day43

## hive排序关键字

cluster by——控制分割也可以控制排序

order by——用于结果集，确保数据按照一个或多个列排序的全局排序

sort by——指定排序所使用的Reduce任务数量，但请注意，SORT BY不能保证全局排序

distribute by——用于控制MapReduce的排序过程，DISTRIBUTE BY用于控制Reduce任务的输入是如何被分割的

\--使用order by 排序

select \* from student2 order by id

\--使用sort by 排序

select \* from student2 sort by class_name desc

\--使用distribute by 分组

set mapreduce.job.reduces=15;

select \* from student2 distribute by class_name sort by id desc

insert overwrite local directory '/root/student2/'

row format delimited fields terminated by '\\t'

select \* from student2_b

distribute by sex

sort by chinese desc

\--使用cluster by 分组并排序

select \* from student2 cluster by class_name

## sqoop

Sqoop 是一个专门用来在「关系型数据库」和「Hadoop 大数据平台」之间做数据搬运的工具，全称是 SQL-to-Hadoop。传统数据库存不下海量数据，需要把历史数据搬到 Hadoop

导入：把 MySQL、Oracle、SQL Server、PostgreSQL 等关系型数据库的数据 → 搬到 HDFS、Hive、HBase

导出：把 HDFS、Hive 里的大数据 → 写回 MySQL、Oracle 等业务数据库

典型使用场景

每天把 MySQL 的订单数据导入 Hive 做离线分析

把 Oracle 的用户数据同步到 HDFS 做机器学习训练

把 Hive 计算好的报表导出回 MySQL 给后台系统展示

全量 / 增量同步数据，支持定时调度（配合 crontab、Azkaban）

|     |     |     |
| --- | --- | --- |
| 指令  | 方法  | 说明  |
| import | ImportTool | 将数据导入到集群 |
| export | ExportTool | 将集群数据导出 |
| codegen | CodeGenTool | 获取数据库中某张表数据生成Java并打包Jar |
| create-hive-table | CreateHiveTableTool | 创建Hive表 |
| eval | EvalSqlTool | 查看SQL执行结果 |
| import-all-tables | ImportAllTablesTool | 导入某个数据库下所有表到HDFS中 |
| job | JobTool | 用来生成一个sqoop的任务，生成后，该任务并不执行，除非使用命令执行该任务 |
| list-databases | ListDatabasesTool | 列出所有数据库名 |
| list-tables | ListTablesTool | 列出某个数据库下所有表 |
| merge | MergeTool | 将HDFS中不同目录下面的数据合在一起，并存放在指定的目录中 |
| metastore | MetastoreTool | 记录sqoop job的元数据信息 |
| help | HelpTool | 打印sqoop帮助信息 |
| version | VersionTool | 打印sqoop版本信息 |

### sqoop创建hive表

$ bin/sqoop create-hive-table \\

\--connect jdbc:mysql://hadoop102:3306/company \\

\--username test \\

\--password test \\

\--table test \\

\--hive-table test

### sqoop全量导入导出表

导入MySQL到hadoop

#!/bin/bash

sqoop import \\

\--connect "jdbc:mysql://hadoop100:3306/test?useUnicode=true&characterEncoding=utf-8" \\

\--username test \\

\--password test \\

\--table student2 \\

\--hive-import \\

\--delete-target-dir \\

\--hive-database db_hive \\

\--fields-terminated-by "\\t" \\

\--target-dir "/user/hive/warehouse/db_hive/student2_sqoop" \\

\--hive-table student2_sqoop \\

\-m 1

从hadoop导出到MySQL

#!/bin/bash

sqoop export\\

\-connect "jdbc:mysgl://hadoop10e:3306/test?useunicode=true&characterEncoding=utf-8" \\

\--username test \\

\--password test \\

\-m1 \\

\--table student \\

\--input-fields-terminated-by \\

\--export-dir '/user/hive/warehouse/db_hive.db/student2'

#!/bin/bash

sqoop_log(){

sqoop export\\

\--connect "jdbc:mysql://hadoop1oo:3306/test?useUnicode=true&characterEncoding=utf-8" \\

\--username test \\

\--password test \\

\-m 1 \\

\--table log_ \\

\--input-fields-terminated-by "t" \\

\-export-dir "/user/hive/warehouse/db_hive.db/log/$val/"

}

part='beeline-ujdbc:hive2://hadoop10o:1000o/db_hive \\

\-n root -p root --outputformat=csv2--showHeader=false \\

\-e 'show partitions log;'

for val in $part

do

echo $val

### sqoop增量导入表

bin/sqoop import \\

\--connect jdbc:mysql://hadoop102:3306/test \\

\--username root \\

\--password root \\

\--table emp \\

\--check-column deptno \\

\--incremental lastmodified \\

\--last-value "10" \\

\--m 1 \\

\--append

### sqoop分区表导入表

#!/bin/bash

sqoop_import(){

sqoop import \\

\--connect "jdbc:mysql://hadoop100:3306/test?useUnicode=true&characterEncoding=utf-8" \\

\--username test \\

\--password test \\

\--query "select \* from maket_p where cast(year(ord_date) as decimal)='$part' and \\$CONDITIONS" \\

\--hive-import \\

\--create-hive-table \\

\--hive-overwrite \\

\--fields-terminated-by "\\t" \\

\--hive-database db_hive \\

\--hive-table market_sqoop \\

\--target-dir "/user/hive/warehouse/db_hive/market_sqoop/type_p=$part/" \\

\--hive-partition-key type_p \\

#分区值，删除该字段

\--hive-partition-value "$part" \\

\-m 1

}

for part in \`mysql -uroot -proot --database=test -N -e \\

"select distinct cast(year(ord_date) as decimal) from maket_p order by cast(year(ord_date) as decimal) "\`

do

echo "$part 年数据 导入..."

sqoop_import

done

beeline -u jdbc:hive2://hadoop100:10000/db_hive \\

\-n root -p root --outputformat=csv2 --showHeader=false \\

\-e 'msck repair table market_sqoop;'

### sqoop分区表导出表

#!/bin/bash

sqoop_maket(){

sqoop export \\

\--connect "jdbc:mysql://hadoop100:3306/test?useUnicode=true&characterEncoding=utf-8" \\

\--username test \\

\--password test \\

\-m 1 \\

\--table maket_p \\

\--input-fields-terminated-by '\\t' \\

\--export-dir "/user/hive/warehouse/db_hive.db/maket_p/$val/"

}

part=\`beeline -u jdbc:hive2://hadoop100:10000/db_hive \\

\-n root -p root --outputformat=csv2 --showHeader=false \\

\-e 'show partitions maket_p;'\`

for val in $part

do

echo $val

sqoop_maket

done

Day44

**docker**

Docker 是容器化工具，把软件 + 依赖打包成独立容器，一处打包、到处运行，解决 “本地能跑、服务器报错” 环境不一致问题

**三大核心组件**

**镜像 (image)**

只读模板，相当于软件安装包（如 mysql:8.0、nginx），用来生成容器，存放在 Docker 仓库。

**容器 (container)**

镜像运行后的实例，一个镜像可启动 N 个独立容器，程序实际跑在容器里，删除容器不影响原镜像。

**Docker Registry（仓库）**

存放镜像的云端仓库，官方：Docker Hub，类似应用商店，拉取 / 推送镜像。

**Docker 优势**

环境统一：开发、测试、生产环境完全一致，告别环境 bug

部署极速：打包后一键部署，不用逐个装依赖、配置环境

资源节省：同硬件下，容器数量远多于虚拟机

弹性扩容：需要扩容服务时，一键多启几个容器即可

**使用场景**

后端项目打包部署（Java/Python/PHP 服务）

快速安装中间件：MySQL、Redis、RabbitMQ、Elasticsearch

多版本软件共存：一台机器同时跑 mysql5.7+mysql8.0 互不冲突

微服务集群、CI/CD 自动化发布

**镜像**

**镜像获取**

**1.从镜像仓库获取**

docker image pull tomcat:9.0.44-jdk8

注：用软件名:版本，不写版本为最新版本

**2.从tar压缩包获取**

docker image load -i /root/oracle.tar

**3.从import 导出包获取**

docker image import mysql_image.tar mysql:5.7

注：该命令用于导入 系统文件 /rootfs 做成新镜像

**4.从dockerFile 文件构建**

docker image build -t centos7:7 /root/Dockerfile

**5.从container容器转化**

docker commit mysqlmaster mysql_master:5.7

将容器转为镜像

**镜像操作**

**1.保存迁移镜像**

从docker导出镜像，压缩成.tar文件导出到指定位置

docker image save -o /root/tomcat.tar tomcat:9.0.44-jdk8

**2.删除镜像**

docker image rm mysql:5.7

注：也可用docker images查到的id删除

正在使用的镜像不能直接删除，要先删除容器

**3.删除无用镜像**

docker image prune

**4.上传镜像到仓库**

docker image push tomcat:9.0.44-jdk8

**镜像查看**

**1.查看镜像列表**

docker images

**2.显示一个或多个映像的详细信息**

docker image inspect mysql:5.7

**3.到仓库检索镜像**

docker search mysql

**4.上传镜像到仓库**

docker image push tomcat:9.0.44-jdk8

需要绑定仓库

**5.查看镜像历史**

docker image history mysql:5.7

**容器**

**生成容器**

**1.创建容器**

docker create centos

**2.创建并启动容器**

docker run -d tomcat:9.0.44-jdk8

run=create+start

**3.生成容器常用参数**

docker run --name mysql_new -d -p 3306:3306 \\

\--net mysql-test \\

\-v /usr/mysql/conf:/etc/my.cnf.d \\

\-v /usr/mysql/data:/var/lib/mysql \\

\-e MYSQL_ROOT_PASSWORD=root \\

\--restart always \\

\--privileged=true \\

\--network-alias mysql \\

mysql:5.7

\--name：指定名称/别名

\-d：后台执行

\-p 3306:3306：外部访问端口:容器内端口

\-v /usr/mysql/conf:/etc/my.cnf.d ：外部访问路径:内部路径

\-e MYSQL_ROOT_PASSWORD=root ：环境配置

\--restart always ：重启策略

\--privileged=true ：允许扩展

\--network-alias mysql ：网络别名

mysql:5.7 镜像名称:版本

**容器操作**

**1.进入容器命令**

docker exec -it mysql_new bash

docker attach mysql_new

**2.删除容器**

docker container rm mysql_new

注：可用id或者名称删除

**3.启停容器**

docker container start|stop|restart mysql_new

docker container pause|unpause mysql_new

**4.容器文件传输(拷出)**

docker container cp mysql_new:/root/test /root/test

本机目录容器内目录

**5.导出容器**

docker container export -o /root/mysql.tar mysql_new

**容器查看**

**1.查看所有容器**

docker container ls -a

**查看正在运行的容器**

docker container ls

**2.查看容器详情**

docker container inspect mysql_new

**3.查看容器执行日志**

docker container logs mysql_new

如果容器内的程序不在运行，就用该命令查看情况

**4.查看容器进程**

docker container ps mysql_new

docker container top mysql_new

**6.查看容器资源占用**

docker container stats

**5.查看docker 网络\\卷**

docker network ls

docker volume ls

Day45

**网络**

**docker端口**

**1.随机暴露容器所有端口(危险)**

docker run -P -it ubuntu /bin/bash

**2.将容器指定端口随机映射到宿主机一个端口上**

docker run -P 80 -it ubuntu /bin/bash

**3.将容器指定端口指定映射到宿主机的一个端口上**

docker run -p 8000:80 -it ubuntu /bin/bash

**4.将容器ip和端口，随机映射到宿主机上**

docker run -P 192.168.0.100::80 -it ubuntu /bin/bash

**5.将容器ip和端口，指定映射到宿主机上**

docker run -p 192.168.0.100:8000:80 -it ubuntu /bin/bash

注：只有目标ip才能使用该容器

**6.指定协议**

docker run -d -p 8080:80/tcp nginx

**docker网络**

**1.使用host网络**

docker container run --network=host nginx

**2.使用macvlan 网络**

创建:

docker network create -d macvlan --subnet=192.168.10.0/24 \\

\--gateway=192.168.100.1 -o parent=ens33 mac1

使用:

docker run --ip =192.168.100.2 --network mac1 nginx

**3.使用自定义桥接网络**

创建:

docker network create --driver bridge \\

\--subnet 192.168.10.0/24 --gateway 192.168.10.1 mybridge

使用:

docker run --network=mybridge -d nginx

**docker compose 指令**

**1.创建和启动服务**

cd到有yaml文件的目录

docker compose up -d

绝对路径启动

docker compose -f \[文件路径/yaml文件名\] up -d

**2.删除和停止服务**

相对路径：cd到有yaml文件的目录

docker compose down

绝对路径

docker compose -f \[文件路径/yaml文件名\] down

**3.服务启动停止重启**

docker compose start/stop/restart

**4.指定启动数量**

docker compose up --scale master=2 --scale slave=2

**5.查看日志**

docker compose logs \[ -f 容器名称 --tail =50 \]

**yaml文件**

YAML是一种标记性语言,类似于json数据描述语言,可读性高

**编写规范**

1.  YAML数据结构通过缩进来表示,连续项目通过减号表示,键值对用冒号分隔,数组使用总括号\[\]括起来,bash用花括号{}括起来
2.  不支持制表符TAB缩进,只能使用空格缩进
3.  字符后缩进一个空格(如冒号,逗号,横杠后须加空格)
4.  使用#号表示注释
5.  如果包含特殊字符用单引号\` ' ' 标记为普通字符,用双引号表示特殊字符本身的意思,布尔值必须使用双引号""括起来
6.  YAML 区分大小写

**文件构成：**

|     |     |
| --- | --- |
| version | 指定此yml文件基于的compose的版本 |
| services | 指定创建容器的服务选项 |
| 服务名 | 例如nginx等 |
| network | 网络服务创建 |
| volume | 数据卷服务创建 |
| hostname | 容器主机名 |
| build | 指定构建镜像上下文路径 |
| context | 上下文路径 |
| dockerfile | 指定构建镜像的 Dockerfile 文件名 |
| ports | 暴露容器端口，与-p相同，但端口不能低于60；例如- 1234:80 |
| networks | 加入顶级networks下配置的网络 |
| deploy | 指定部署和运行服务相关配置，只能在Swarm模式使用 |
| volumes | 挂载宿主机路径或命令卷 |
| image | 指定容器运行的镜像 |
| command | 执行命令，覆盖默认命令 |
| container_name | 指定容器名称，由于容器名称是唯一的，如果指定自定义名称，则无法scale（扩展） |
| environment | 添加环境变量 |
| restart | 重启策略，重启策略是no，always，no-failure，unless-stoped |
| no  | 默认策略：在容器退出时不重启容器。 |
| on-failure | 在容器非正常退出时（退出状态非0），才会重启容器。可加(:3) 规定重启次数 |
| always | 在容器退出时总是重启容器。 |
| unless-stopped | 在容器退出时总是重启容器 |
| networks | 配置网络，指定网卡设备等 |

# Day46

登录redis

redis-cli

auth 123456

info replication

# Day47

## 正向代理、反向代理：

正向代理：替客户端访问外网（比如翻墙、内网上网），服务端不知道真实客户端。 反向代理：替服务端接收请求，客户端只访问 Nginx，不知道后端真实服务，是服务端网关。

通俗理解：

正向代理：帮用户上网

反向代理：帮服务器接请求

核心作用：

隐藏后端真实服务地址、端口，提升安全

负载均衡，分发流量到多台后端服务

统一入口、静态资源缓存、SSL 证书统一配置

跨域请求转发、动静分离

## 核心配置语法

Nginx 反向代理主要靠 proxy_pass 指令，配置写在 location 块内

负载均衡

### 1\. 轮询：

特点：请求按顺序轮流分发，1→2→3→1→2→3…

适用：后端服务器配置完全相同、无状态服务。

nginx

upstream backend {

server 192.168.1.10:8080;

server 192.168.1.11:8080;

server 192.168.1.12:8080;

}

### 2\. 加权轮询

特点：给性能好的服务器设置更高权重，分到更多流量。

适用：服务器配置不一样（高配机多扛流量）

upstream backend {

server 192.168.1.10:8080 weight=5; # 高配，权重5

server 192.168.1.11:8080 weight=2; # 中配，权重2

server 192.168.1.12:8080 weight=1; # 低配，权重1

}

### 3\. IP 哈希（ip_hash）

特点：根据客户端 IP 计算哈希值，固定分配到某台后端。

作用：解决会话保持问题（登录状态不丢失）。

适用：需要登录态、session 会话的系统。

upstream backend {

ip_hash; # 开启 ip_hash

server 192.168.1.10:8080;

server 192.168.1.11:8080;

}

### 4\. 最少连接（least_conn）

特点：把请求发给当前连接数最少的服务器。

适用：请求处理时间长短不一，容易造成部分服务器积压。

upstream backend {

least_conn; # 开启最少连接

server 192.168.1.10:8080;

server 192.168.1.11:8080;

}

第三方 2 种（需安装模块）

### 5\. 公平调度（fair）

特点：根据后端响应时间分配，响应快的优先。

适用：接口响应速度差异大的场景。

upstream backend {

fair;

server 192.168.1.10:8080;

server 192.168.1.11:8080;

}

### 6\. URL 哈希（url_hash）

特点：根据请求 URL 计算哈希，同一个 URL 永远访问同一台机器。

适用：文件服务器、缓存服务器（提高缓存命中率）。

upstream backend {

hash $request_uri;

server 192.168.1.10:8080;

server 192.168.1.11:8080;

}

### 高可用常用参数

不管用哪种均衡策略，下面这几个参数生产必配：

upstream backend {

server 192.168.1.10:8080 max_fails=3 fail_timeout=30s;

server 192.168.1.11:8080 backup; # 备用机

}

max_fails=3：失败 3 次就标记宕机

fail_timeout=30s：30s 内不再访问它

backup：热备，其他全挂才启用

# Day48

## Kubernetes

Kubernetes（简称k8s）是一个开源的容器编排平台，用于自动化部署、扩展和管理容器化应用。

它的核心功能包括自动化容器部署、负载均衡、自我修复、存储编排以及跨集群资源管理。

通过Kubernetes，企业能够高效管理大规模的容器化应用，确保应用的高可用性和弹性扩展

## Kubernetes架构

K8S节点有2个角色分别是：Master和Node

Master为控制节点，负责整个集群的管理控制

- Master节点由：APIserver、ETCD 、controller Manager、schedule等组件构成

Node的作用是承接工作负载

- Node节点有由：kubelet、rumtime、kube-proxy组成

### k8s架构-Master

### k8s架构-Node

### Kubernetes资源-分类

### K8s资源介绍-层级关系

## 常用命令

### kubernetes-集群信息查看

|     |     |
| --- | --- |
| kubectl get pods | 查看所有 Pod |
| kubectl get deployments | 查看所有 Deployment |
| kubectl get services | 查看所有 Service |
| kubectl get pvc | 查看所有 PVC |
| kubectl get nodes | 查看所有 Node |
| kubectl get namespaces | 查看所有命名空间 |
| kubectl get configmap &lt;name&gt; -o yaml | 查看 ConfigMap 内容 |
| kubectl logs &lt;pod-name&gt; | 查看 Pod 日志 |

### kubernetes-资源创建命令

1\. 创建 Pod

kubectl create deployment my-pod --image=nginx

2\. 创建 Deployment

kubectl create deployment my-deployment --image=nginx --replicas=3

3\. 创建 Service

kubectl create service clusterip my-service --tcp=80:8080

4\. 创建 ConfigMap

kubectl create configmap my-config --from-literal=key1=value1 --from-literal=key2=value2

5\. 创建 Secret

kubectl create secret generic my-secret --from-literal=username=admin --from-literal=password=secret

6\. 创建 Namespace

kubectl create namespace my-namespace

### kubernetes-Pod 信息查看

|     |     |
| --- | --- |
| 1\. 列出特定命名空间中的pod | kubectl get pods -n &lt;namespace&gt; |
| 2\. 查看一个 Pod 详情 | kubectl describe pod &lt;pod-name&gt; -n &lt;namespace&gt; |
| 3\. 查看 Pod 日志 | kubectl logs &lt;pod-name&gt; -n &lt;namespace&gt; |
| 4\. 尾部 Pod 日志 | kubectl logs -f &lt;pod-name&gt; -n &lt;namespace&gt; |
| 5\. 在 pod 中执行命令 | kubectl exec -it &lt;pod-name&gt; -n &lt;namespace&gt; -- &lt;command&gt; |

### kubernetes-deploymen信息

|     |     |
| --- | --- |
| 1\. 列出命名空间中的所有Deployment | kubectl get deployments -n &lt;namespace&gt; |
| 2\. 查看一个Deployment详情 | kubectl describe deployment &lt;deployment-name&gt; -n &lt;namespace&gt; |
| 3\. 查看滚动发布状态 | kubectl rollout status deployment/&lt;deployment-name&gt; -n &lt;namespace&gt; |
| 4\. 查看滚动发布历史记录 | kubectl rollout history deployment/&lt;deployment-name&gt; -n &lt;namespace&gt; |

### kubernetes-label指令

\# 给名为foo的Pod添加label unhealthy=true

$ kubectl label pods foo unhealthy=true

\# 给名为foo的Pod修改label 为 'status' value 'unhealthy'，且覆盖现有的value

$ kubectl label --overwrite pods foo status=unhealthy

\# 给 namespace 中的所有 pod 添加 label

$ kubectl label pods --all status=unhealthy

\# 仅当resource-version=1时才更新 名为foo的Pod上的label

$ kubectl label pods foo status=unhealthy --resource-version=1

\# 删除名为“bar”的label 。（使用“ - ”减号相连）

$ kubectl label pods foo bar-

kubernetes-基础指令

create，delete，get，run，expose，set，explain，edit

\# 创建Deployment和Service资源

$ kubectl create -f demo-deployment.yaml

$ kubectl create -f demo-service.yaml

\# 根据yaml文件删除对应的资源，但是yaml文件并不会被删除，这样更加高效

$ kubectl delete -f demo-deployment.yaml

$ kubectl delete -f demo-service.yaml

\# 也可以通过具体的资源名称来进行删除，使用这个删除资源，同时删除deployment和service资源

$ kubectl delete 具体的资源名称

https://kubernetes.io/zh-cn/docs/reference/kubectl/quick-reference/

\# 创建Deployment和Service资源

$ kubectl create -f demo-deployment.yaml

$ kubectl create -f demo-service.yaml

\# 根据yaml文件删除对应的资源，但是yaml文件并不会被删除，这样更加高效

$ kubectl delete -f demo-deployment.yaml

$ kubectl delete -f demo-service.yaml

\# 也可以通过具体的资源名称来进行删除，使用这个删除资源，同时删除deployment和service资源

$ kubectl delete 具体的资源名称