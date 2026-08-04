# Day01

## 基本知识

plsqldeveloper

用户名：scott

密码：oracle—>pin

控制台命令查看IP地址：ipconfig

虚拟机检查网段通讯情况：

控制台命令：ping （主机ipv4网址）

字段 = 列

一条数据 = 一行记录

## sql代码构成

关键字+表名+列名

select \* from emp;

select \* form emp where ename='SCOTT';

字符串注意大小写

select后跟列/表达式/函数

form后跟行名

### select

select (列名) , (列名) --列与列之间用逗号隔开

列的名称不用管大小写，逗号后不跟列会报错

## where对行筛选

form (行名) where

过滤关键字，后跟条件判断式

### between and

于where(列明)后

between n1 and n2——检索值在n1到n2之间的数据

可以and (列名)='xxx'

### and or

and且运算

or或运算

and优先级高于or，会先运行and进行筛选，再列举or

可用()提高运算优先级

### in

(行列)in(条件)

### order by

select 列名 from 表名order by (列名)——以某列进行降序排序（从大到小）

order by (列名) desc——升序

可用数字指代需要排序的列

### group by having

group by 列名——根据一个列对内容进行分组

group by 列名 having 条件——group by中的筛选条件

### 别名

- 不改变真实列明/行名/表名名的方式，改变显示的列明/行名/表名

列明/行名/表名 (别名)

列明/行名/表名 as (别名)

表名别名.列/行 可直接调用某表的行/列 emp.ename

### distinct去重

对列去重，对多列进行去重时，保留最多的列

select distinct 列名 , 列名 from emp

### 连接符(管道符)

select ename||'的工资是：'||sal||'元' from emp

输出单列表格

## 字符串

单引号'xx'使用字符串

'''x'''输出'x'

'''x'输出'x

### 通配符

select (列名) form emp like 'x%'——列举列中包含x的行

select (列名) form emp like 'x_'——列举包含x，且后只有一个字符的行

### dual虚拟表格

- 用于测试

select 1+2 from dual;——输出表头为1+2，内容为3的表格

select \* from dual;——输出表头为dummy，内容为X的表格

### rownum伪列

- 创建一从1开始的递增数列

select ename,rownum from emp;——在enamel列后加一列rownum ，内容为1开始递增

- 伪列可用于判断

select ename,sal,deptno,rownum from emp where rownum<3;

where rownum>3则输出无内容，应为rownum只能从开始

### rowid数据身份证

- 创建一列，标识数据的物理地址（数据库中的地址）

# 函数

## 基本运算函数

AVG（列名），SUM（列名），MAX（列名），MIN(列名），COUNT（列名）

round(\_,n)——将位数保留到n位，四舍五入

# Day02

## 数据类型

### 字符型

|     |     |     |     |
| --- | --- | --- | --- |
| char | 固定长度（最大2000字节） | 区分中英文 | 英文占一个字节<br><br>中文占2个字节 |
| varchar2 | 可变长度（最大4000字节） | 区分中英文 |
| nchar | 固定长度（最大2000字节） | 不区分中英文 | 中英文都占一个字节 |
| nvarchar2 | 可变长度（最大4000字节） | 不区分中英文 |

- 用法：行名 数据类型(n) 内容——n字节位数
- 常用verchar2类型

### length() lengthb()

查看数据占用的字符长度

### 数值型

关键字number——（最大38字节）

用法：

- 数据名 number(n)——n位数值，只能储存整型，带小数会报错
- 数据名 number(n,c) n：总数位 c：小数位；整数位 = n - c
- 直接运算：select sal+500 from emp;

### 日期类型

data——结构：yyyymmdd hh:mi:se

特点：可做加减，不可做乘除

### lob数据类型

可以储存大型文件，如world，pdf

### 表格规范

1.  尽可能细化，区分每一列的内容
2.  一张表只做一件事
3.  插艾儒数据，一般不考虑关系比较远的数据（如部门地址，放在部门表而不是员工表）

## 数据储存逻辑

### Oracle 完整存储层级

数据库 (Database) → 实例 (Instance) → 表空间 (Tablespace) → 数据文件 (Datafile) → 【表 Table → 数据段 Data Segment】 → 区 Extent → 数据块 Data Block → 磁盘扇区

1 个数据库 → 可被 1 个实例挂载访问

1 个数据库 → 包含 N 个表空间

1 个表空间 → 包含 N 个数据文件

1 个数据文件 → 包含 N 个段（段不跨文件）

1 个段 → 包含 N 个区（区不跨文件）

1 个区 → 包含 N 个连续数据块

1 个数据块 → 多个磁盘扇区

### 表的数据存储：

从小到大

块(block)-->区(extent)-->段(segment)-->表空间(tablespace)

## 创建-赋权-编辑-删除

### 权限赋予

1.  登录管理员账号——账号：system
2.  赋予用户权限

grant dba to scott;

双击切换账号

### 创建表空间

create tablespace （表空间名） --不建议起中文名

datafile 'c:\\test\\tab.dbf' --储存地址\\表空间的数据文件

size 2G --默认大小

autoextend on next 100M --每次扩展大小

maxsize unlimited; --容量无限制

### 创建临时表空间

create temporary tablespace （表空间名） --不建议起中文名

tempfile 'c:\\test\\temptab.dbf' --储存地址，表空间数据文件

size 5m --默认大小

autoextend on next 5M --每次扩展大小

maxsize unlimited; --容量无限制创建

### 查看表空间

select \* from dba_data_files --查看表空间

select \* from dba_temp_files --查看所有临时表空间

### 删除表空间

drop tablespace DAY02 including contents;

\--后续还需手动删除数据文件

\--including contents选项用于删除表空间时包含其内容。如果不使用这个选项，表空间会 被删除，但数据文件仍然存在，磁盘空间不会被释放。使用这个选项可以确保表空间及 其内容被完全删除，从而释放磁盘空间

（但用后数据文件似乎还在）

### 创建用户

create user lb111 --用户名

identified by 114514 --用户密码

default tablespace daytab --用户管理的表空间

temporary tablespace TEMPDAYTAB --用户管理的临时表空间

\--如不指定默认的表空间是users表空间，临时表空间是temp

grant dba to lb111--记得给用户权限，否则无法登录

用户查看其他用户的表

select \* from scott.emp

### 用户权限

|     |     |
| --- | --- |
| grant resource,connect to ora; | 连接权限和资源权限 |
| grant create any table to ora; | 建表权限 |
| grant create any tablespace to ora; | 建表空间权限 |
| grant select any table to ora; | 只读权限 |
| grant create any view to bw; | 创建视图权限 |
| grant select any table to ora; | 给ora用户预编译表的权限 |
| select \* from role_sys_privs; | 查看角色权限信息 |
| grant dba to ora; | 管理员权限 |

## 表数据操作

### 创建表格

create teble 表名 (列名 数值类型,...)

### 插入数据

**分开插入**

insert into 表名 values(对应列的值,对应列的值,...) 或

insert into 表名 values(值,值,...),(值,值,...),(值,值,...),...

例：

**批量插入：**

INSERT ALL

INTO 表1(列) VALUES(值1)

INTO 表2(列) VALUES(值2) ...

SELECT 查询集 FROM 数据源;

注：最后一行为固定结构，必须写完整的select语句，可用 SELECT 1 FROM DUAL 替代

### 数据缓存

运行插入数据后，数据并不会直接插入表格中，而是被缓存，产生事务，点击“提交事务”后才会保存在表格中，选择撤回，数据则不会被添加到表格中，或通过指令提交或者回滚：

commit——提交

rollback——回滚

### 列级约束：六大约束

作用：规范输入表数据

|     |     |     |
| --- | --- | --- |
| not null | 非空约束 |     |
| unique | 唯一约束 |     |
| primary key | 主键约束，非空和唯一的组合 |     |
| default(值) | 默认约束 | 输入default启用默认值 |
| check(条件) | 检查约束 | 满足条件可输入 |
| references 父级表名(引用父级表列名) | 外键约束 | 引用父表格的列的内容 ~ :this<br><br>父表格的内容有插入的内容时才能输入 |

使用：列名 数据类型 约束类型

### 表级约束

需自定约束名，卸载所有列级约束的后面

### 复合主键约束

- 多个列共同的主键约束
- 格式：

create table 表名(列名 数据类型,...)

constraint 自定义约束名 约束类型;

例：

create table sc(

sno varchar2(10),

cno varchar2(10),

score number(5,2),

constraint pk_sc primary key (sno,cno));

输入数据时，当其中的sno和cno列与其他行都重复时才报错

- 表级外键约束，同上例

constraint fk_sc_cno foreign key (cno) references course(cno)

结构：constraint 自定义约束名 foreign key(需要约束的列名) references 父级表名(引用父级表的列名)

### 删除表

drop table 表名;

## 数据类型转换

字符串转转日期：to_date('20240815','yyyymmdd')--精确到日期

# Day03

## alter table 表修改

### alter table _ add _ 添加新列

alter table 表名 add 列名 类型(长度)\[约束&默认值\];

### alter table _ modify_ 修改列的定义

alter table 表名 modify 列名 类型 \[约束 默认值\];

### alter table _ modify _ default() 修改列默认值

修改数据类型，添加约束，有冲突则改为最新，无冲突则保留

alter table 表名 modify 列名 default()

### 删除一列

alter table 表名 drop column 列名;

### 修改列名

alter table 表名 rename column 旧列名 to 新列名;

### 修改表名

alter table 表名 rename to 新表名;

### 修改默认值

alter table 表名 modify 列名default '北京';

### 重命名列

alter table 表名 rename column 旧列名 to 新列名;

### 添加约束

- 最好建表初期加上约束
- 添加表级约束结构：

alter table 表名 add 表级约束语法; --给表添加表级约束

Constraint PK_empno primary key (empno)

- 添加列级约束结构

alter table 表名 modify 列名 约束名

### 删除约束

- alter table 表名 drop constraint 约束名; --删除表级约束
- alter table 表名 drop 约束名; --删除列级约束
- alter table 表名modify 列名null; --删除not null约束

### 删除表中的列

alter table 目标表名 drop column 列名;

## 更改只读状态

ALTER TABLE 表名 READ ONLY;

ALTER TABLE 表名 READ WRITE;（改回可编辑模式）

## 继承建表

结构、数据基于查询的原表，不会继承约束

create table 新表名

as

select 列内容 from 目标表名 \[where...\];

## insert into 插入

### insert into _ values()

- 插入一行数据

insert into 表名 values(对应列的值,对应列的值,...)

- 插入一行指定列的数据

insert into 表名(指定列名,指定列名) values(对应列的值,对应列的值,...)

注：无内容输入的列，可以为空时才能用此方法插入

### insert into _ select _ from _

使得新表继承目标表的数据

- 插入一行数据

insert into 新表 select 要插入的列内容 from 旧表

注：旧表可用dual表

- 指定列插入数据

insert into 新表(列,列,...) select 要插入的列内容 from 旧表

## update set 更新数据

### update _ set _ where _

- 更新具体单元格的数据

update 表名 set \[列名 =\_\],\[列名 =\_\] where...;

注：可同时更改多个格子的数据

### update _ set （子查询） where _

update 表名 set 列名 =（子查询） where...;

## delate 删除数据

### delete form _ where _

删除具体数据

delete from 表名 where...\[and...\];

注：删选空值时应为：where _ is null

### delete form _ where （子查询）

## 删除总结

|     |     |     |     |
| --- | --- | --- | --- |
| drop | 结构上的删除 | 不可以回滚 | 仅删除表数据 |
| delete | 数据上的删除 | 可以回滚 |
| truncate | 数据上的删除 | 不可以回滚 | 表数据和表结构一起删除，只剩下结构 |
| 执行效率：drop > truncate > delete |     |     |     |

## merge into 备份还原

- 现表与备份表进行对比，不相同的地方，用备份表还原现表，缺失的地方用备份表数据添加到现表
- 结构：

merge into 现表 using 备份表 --现表指向目标表（备份表）

on(现表.列 = 备份表.列 \[and 现表.列 = 备份表.列\]) --关联关系，可有多个

When matched then update set

\[现表.列 = 备份表.列,现表.列 = 备份表.列,...\] / delete --相同时执行的操作，最好不要修改主键约束的列

When not matched then insert values(备份表名.列名...每个列都要.出来) --匹配不上（找不到行）时执行的操作

例：

merge into stuCourseTab using stuCourseTab1

on(stuCourseTab.stu_no = stuCourseTab1.stu_no and stuCourseTab.cor_no = stuCourseTab1.cor_no) _\--关联关系_

when matched then update set _\--关联关系相同_

stuCourseTab.score = stuCourseTab1.score

when not matched then insert values ( _\--关联关系找不到_

stuCourseTab1.stu_no, stuCourseTab1.cor_no,stuCourseTab1.score);

## 运算符

### select后

算数操作符 + - \* /

连接符 ||

### where后

- 逻辑操作符 and（与） or（或） not（非）

优先级：not > and > or，优先级高的语句会先执行/筛选，可用()提高优先级

- 比较操作符：>, <, =, !=, all, any, between and, is null, is not null,

like

补充：all any in——与多个值进行比较，>, <, =, != 只能与一个值比较

补充：between and 只能从小往大写，值可以是数值、文本或者日期

|     |
| --- |
| \>all:表示大于最大值 |
| <all：表示小于最小值 |
| \>any：表示大于最小值 |
| <any：表示小于最大值 |
| \=any: 和in类似 |

select \* from emp where sal > all(100,200,1000)

一般搭配子查询使用

select \* from emp where sal > all(子查询)

select \* from emp where sal = any(100,200,1000)

\= select \* from emp where sal = in(100,200,1000)

### like模糊查询

select \* from emp where ename like '%m%'; --查询ename中包含m的

通配符：

- %说明包含任意长度任意字段

select \* from emp where ename like '%m_'; --查询ename倒数第二位为m的

- \_占一位字段

select \* from emp where ename like '%\\%%'

escape '\\'; --声明转义符

- 转义符：\\，\*，将特殊符号转义为普通字符，用escape'\\'声明转义符，一次只能转义一个字符

——查询名字里包含两个_的员工

select \* from emp where ename like'%\\\_%\\\_%' escape'\\';

## 快捷数据更改

可快速编辑数据，输入：

select \* from emp2 for update

后点击解锁进行编辑

编辑后点击发布改变

再锁定完成数据编辑

# Day04

## 排序

根据某列排列

select 列 from 表 order by 列 \[asc/desc\]

asc——升序后缀（默认升序，从小到大）， desc——降序后缀

- 可以与where组合

select \* from emp where deptno=10 order by sal desc;

- 可以先后排序

select \* from emp order by deptno asc,sal desc;

选对前面的列进行完全排序，再基于前面的排序尝试进行排序

多列排序时前面的列最好有重复值

- 可以使用别名

select sal as 薪资 from emp1 order by 薪资 desc;

- 可用数字指代列

select deptno,ename,sal from emp order by 1,3

数字指代的是select后的列的顺序

- 可以以聚合函数排列

select deptno,job,sum(sal) from emp

group by deptno,job

order by deptno,sum(sal);

注：降序排序时如果有空值，那么空值会作为最大值排在最前面

## 分组

### 分组查询group

分组的前提是目标列内要有重复值

- 对一列进行分组

select deptno,count(\*) from emp group by deptno ;

select deptno,max(sal),min(sal) from emp group by deptno ;

- 对多列进行分组

select deptno ,job,count(1) from emp group by deptno,job order by deptno;

|     |     |
| --- | --- |
| max() | 最大值 |
| min() | 最小值 |
| sum() | 求和  |
| avg() | 求平均值 |
| count() | 求个数 |

### 聚合函数

也叫组函数，对一组数据（一列或多列）进行处理，返回单个结果

group by 经常与聚合函数一起使用，可以实现对查询结果中每一组数据进行分类统计。

### having分组后过滤

having筛选分组的列，聚合函数

select A from B where C group by D having E order by F

SQL执行顺序from——where——group by——having——select——order by

where、having区别：

- where后面不可以加聚合函数的过滤条件，having可以
- 只有出现聚合函数作为过滤条件时用having，其余所有情况都用where
- where比having先执行

select deptno,job from emp where sal>1000 _\--where筛选sal>1000的数据_

group by deptno,job having deptno>10 order by deptno desc;

_\--以deptno和job分组，having过滤deptno>10的内容，再排序_

## 执行顺序

查询语句构成：最少2个关键词，最多6个，一个关键词在一条语句里只能出现一次

+on

## 基本函数

### 字符串操作函数

|     |     |     |
| --- | --- | --- |
| **序**<br><br>**号** | **函数名** | **含义** |
| 1   | length(str) | 返回一个字符串的长度 |
| 2   | concat(str1,str2) | 字符串连接函数 |
| 3   | chr() | 将一个ASCII码转换成字符 |
| 4   | ascii(字符) | 将一个字符转换成ASCII码值 |
| 5   | instr(str1,str2,start,n) | instr(源字符串,目标字符串,开始位置,匹配序号) |
| 6   | substr(str,start,len) | 表示从start位置开始截取字符串str，截取的长度为len，返回值是一个字符串 |
| 7   | initcap(str) | 将首字母大写其他字母小写(以空格来区分单词的) |
| 8   | lower/upper() | 大小写转换函数 |
| 9   | replace(str,s,d) | 字符串替换函数，将字符串str中的s字符替换成字符d |
| 10  | translate(char,from,to) | 返回将出现在from中的每个字符替换为to中的相应字符以后的字符串。 |

补充：translate

select translate('22重庆的人','1重庆的重庆','北京男士们') from dual;

对比相同项

查找替换位置

查找相同长度

字符

对应位置替换

select translate('22重庆的人','1重庆的重庆','北京男士们') from dual;

返回结果：22京男士人

注：instr “匹配序号”意思是“目标字符串”第几次出现，返回值为相对字符串的绝对位置，“开始位置”可以为复数，意思是从后往前找，但返回值仍从前往后算

instr 后两参数可不写全

注：substr最后一个参数可不写全

replace替换号码例

select replace(empno,substr(empno,1,2),'XX'),ename from emp;

可以用字符类型替换数字类型

### 数值操作函数

|     |     |     |
| --- | --- | --- |
| **序号** | **函数名** | **含义** |
| 11  | round(列名,\[位数\]) | 四舍五入函数，精度是正数小数点之后，负数时小数点之前 |
| 12  | mod(num1，num2) | 求余函数 |
| 13  | trunc(值/日期\[,位数\]) | 截取函数 |
| 14  | floor() | 向下取整 |
| 15  | ceil() | 向上取整 |
| 16  | power(n,m) | 返回n的m次幂 |
| 17  | sqrt(n) | 返回数字n的平方根 |
| 18  | to_date(str) | 将字符串转换成日期yyyy,MM,dd,hh24,mi,ss |
| 19  | to_number() | 将字符串转换成数字 |
| 20  | to_char() | 字符串转换函数 |

注：trunc截取sysdate时只保留 年/月/日

# Day05

### 日期操作函数

|     |     |     |
| --- | --- | --- |
| **序号** | **函数名** | **含义** |
| 21  | last_day(日期) | 取当前日期月的最后一天 |
| 22  | next_day(sysdate,n) | 取下一个(最近的)一周的第几天，1是星期日 |
| 23  | add_months(日期,月) | 给一个日期加上若干个月 |
| 24  | months_between(date1,date2) | 取两个日期相差的月数 |
|     | interval '\[日期格式\]' \[日期格式2\] | 增加一段时间 |

注：interval 用法：

可用云加减，如

interval '1 5' day to second

## 转换函数

### nvl()空值赋值

nvl(列名,值1)

若列内的值为NULL，返回值1；列内的值不为NULL，返回列内的值。

注意两者的类型要一致

### nvl2()空值转换

nvl2(列名,值1,值2) 空值转换函数

- 当第一个参数的值是空时，返回结果是第3个参数的值，当第一个参数不为空时，返回结果是第2个参数的值

**注**：nvl，nvl2的数据类型需一致

## 数据类型转换

### 隐式转换

自动转换

- 只有数字构成的字符串转为数字

### 显示转换

|     |     |     |
| --- | --- | --- |
| to_number | 字符转换为数字 |     |
| to_date() | 将字符类型按一定格式转化为日期类型。 |     |
| to_char() | 数字转化为字符 |     |
| 日期转化为字符 | 必须加单引号,并且区分大小写 |

- tu_date通用格式:

to_date('2019/10/20 23:30:02','yyyy/MM/dd hh24:mi:ss')

'yyyy/MM/dd hh24:mi:ss'部分可任意截取不同类型的时间（时，分，秒，年，日，月）

## case when 额外列

**用途**：

翻译函数、行转列、选择结构（本质上是一列）

**用法**：（类似于if else 和switch case的结合）

- 简单Case函数

select 列名 ,case 列名

when 值1 then 新列的值1

when 值2 then 新列的值2

...

else 新值n

end

frome 表名;

- 搜索Case函数

select 列名 ,case

when 列名 = 值1 then 新列的值1

when 列名 = 值2 then 新列的值2

...(then与值之间需要有空格)

\[else 新值n\]

end

frome 表名;

注：

- 简单case when只能做等值判断，搜索case when可以做逻辑判断
- then、else后的数据类型需一致
- Case函数只返回第一个符合条件的值，剩下的Case部分将会被自动忽略，包括你when后跟的列不相同的情况
- where,having后都可以 跟case
- 各个分支&lt;表达式&gt;返回的数据类型要统一
- 不能省略end

### 行转列

- case when

- pivot

select \* from

(select deptno,job, sal from emp)_\--数据源，后面做聚合需要用到的列_

pivot (max(sal) for job in ('SALESMAN'，'MANAGER','CLERK'))

- 行转列，再列转行

## decode 等值翻译

用法：

select 列名,列名,...

decode(列名,

值1,新值1

值2,新值2

...

值n) from 表名

decode与case when 区别：

- decode 只能用做相等判断，但是可以配合sign函数进行大于，小于，等于的判断
- case when可用于=,>=,&lt;,<=,<&gt;,is null,is not null 等的判断
- 在decode中，null和null是相等的，但在case when中，只能用is null来判断
- 对数据库、语言的支持不同

## over开窗函数

格式：（要配合函数使用，不能单独使用）

- 聚合函数(列)over(\[条件\])

sum()搭配开窗函数，会有累加效果

当2个以上累加值相同时，会默认把累加值加到最后一个上 —>

可以不加任何条件，可加order by

- 序列函数()over(order by 列名 \[desc\])

注：本质上是一列，类似rownum

例：

### 序列函数：

row_number()——无并列排序，不跳号

rank()——考虑并列，会跳号（值相同时会并列，号码相同）

dense_rank()——考虑并列，不会跳号

### partition by开窗内分组

聚合/序列函数(列)over(partition by 列名 order by)

开窗函数内的分组

# Day06

## 多行合并

将一列数据合并为一行展示

### wm_concat

把列值以","号分隔起来，显示成一行

可与group by使用

### listagg

格式：

1.  单独使用

select listagg(列名,'分隔符')within group(order by 列名)name from 表名;

1.  搭配分组使用：+ group by

select 分组列,listagg(列名,'分隔符')within group(order by 列名) from 表名 group by 分组列;

1.  作为分析函数

select 分组列,合并列,排序列,listagg(合并列,'分隔符')within group(order by 排序列)over(partition by 分组列) name from emp

注：over内不应有order by，外不应有group by

## 偏移函数

将数值偏移到同一行进行运算

格式：

select \[列名,\] lead/lag(params,m,n)over(order by 列名) from 表名

### lead()向上偏移

新列以原列params为目标向上偏移m位数，当取不到时默认为 n

取当前行下方（后面）第 N 行数据

LEAD(字段, 偏移行数, 缺省值) OVER(PARTITION BY 分组列 ORDER BY 排序列)

### lag()向下偏移

新列以原列params为目标向下偏移m位数，当取不到时默认为n

取当前行上方（前面）第 N 行数据

LAG(字段, 偏移行数, 缺省值) OVER(PARTITION BY 分组列 ORDER BY 排序列)

## 子查询

### 多条件子查询

筛选每个条件一一对应的数据，用括号包括数据

_\--4.查询员工工资和工作都和20号部门同时一样的员工信息_

select \* from emp1

where (job,sal) in (select job,sal from emp1 where deptno = 20)

## 连接查询

列拼

### 笛卡尔连接(交叉连接)

两表作笛卡尔积，最后得到n×m行，c1+c2列的表

交叉连接的左外/右外写法：主表位于(+)的另一侧

主表

select \* from emp, dept where emp.deptno(+)=dept.deptno

主表

_\--右外连接_

select \* from emp, dept where emp.deptno=dept.deptno(+)

_\--左外连接_

### inner join内连接

匹配不上的数据不显示

\--合并emp和dept,显示deptno为20的数据

select \* from emp inner join dept on emp.deptno=dept.deptno

and dept.deptno = 20s

select \* from emp inner join dept on emp.deptno=dept.deptno where dept.deptno = 20

### natural 自然连接

没有链接条件on，以两个表里相等的列作为链接条件，把这两列合成一列作为第一列

select \* from emp natural join dept

cross join

### 不等值连接

过滤条件的符号不是等号，常用于一表中数据根据另一表数据分等级

_\--查询员工的工资级别_

select ename,grade,sal from emp

inner join salgrade on emp.sal between losal and hisal;

emp:ename、sal

salgrade:losal、hisal

### left outer jion 左外连接

以左表作为主表，主表的数据全部出现，右边能匹配上则出现，匹配不上则以空值填充

关键字outer可省略

主表

select \* from emp left outer join dept on emp.deptno=dept.deptno

主表

select\* from emp1 right outer join dept on emp1.deptno=dept.deptno;

right outer jion 同上

### full outer jion 全外连接

左表和右表的数据全部都出来，互为主表，互相匹配不上的则以空值填充

select \* from emp full outer join dept on emp.deptno=dept.deptno

### 自连接

一个表自我连接，from后最好给表明名进行区分

_\--查询出每个员工的上级领导(查询内容：员工编号、员工姓名、领导编号、领导姓名)_

select yg.empno,yg.ename,ld.empno,ld.ename

from emp yg inner join emp ld on yg.mgr=ld.empno

例：查询不是领导的员工

- 方法一：先外连接组合 再筛去空值

select \* from emp zb right outer join emp ld on yg.mgr = ld.empno

**zb 总表**

**ld 领导表**

**ld 员工表**

_\--选取ld表，筛选条件为zb中为null的地方_

select ld.\* from emp zb right outer join emp ld on zb.mgr = ld.empno

where zb.empno is null

- 方法2：子查询筛掉员工编号等于经理编号的

select \* from emp where empno not in(select distinct nvl (mgr, 0) from emp)

select \* from emp where empno not in(select mgr from emp where mgr is not null)

# Day07

## 集合查询

- 行拼，需表的列相同，无需关联关系
- 别名按第一条select语句的别名
- 要求列的数量、每列的数据类型相同
- 没有优先级，写在前面的先执行，可用()改变优先级
- order by只能出现一次，且只能放最后，其余筛选语句(where,group by等)正常写
- order by排序时有别名则识别别名，原来的字段名不生效，建议用数字指代需排序的列，避免列名映射不到下一列而报错

**格式**：

select 列名 from 表名

集合运算关键字

select 列名 from 表名

select deptno ename from emp

union all

select deptno ,dname from dept

_\--集合查询+连接查询_

select \* from emp inner join

(select deptno from emp union select deptno from dept) t

on emp.deptno=t.deptno;

_\--6.查询出不是领导的员工_

select \* from emp where empno in

(select empno from emp minus select mgr from emp);

|     |     |     |     |
| --- | --- | --- | --- |
| 集合类型 | 关键字 | 特点  | 作用方式 |
| 并集  | union | 可排序，可去重 |     |
| union all | 不可排序，不会去重 |
| 交集  | intersect | 有一定的去重作用 |
| 减集  | minus | 有去重作用 | 写在前的减去写在后的 |

### 交、并、差

## 删除重复数据

_\--方法1：_

delete from dept1 where rowid not in

(select max(rowid) from dept1 group by deptno)

_\--方法2：_

delete from dept1 where rowid not in

(select rowid from (select dept1.\*,rowid,row_number()over(partition by deptno order by deptno) r

from dept1)where r=1)

## 索引

- 数据库中的对象-加快检索速度-基于列建立-由程序自动管理
- 在哪个列建立了索引，查询语句里出现这个列即可走到索引

**索引分类：**

### 按储存形式分类：

**B-Tree索引（二叉树索引）**

使用于列基数大的情况

_\--创建b-tree 索引_

create index ind_emp_ename on emp(ename)

**位图索引**

适用于基数较小的情况，有些数据库用不了

create bitmap index ind_emp_job on emp(job);

**反向键索引**

适用于从后往前找数据时

create index ind_emp_job on emp(job) reverse ;

**基于函数的索引**

将索引列原始数据经函数处理后存储+ROWID，适用于查询时经常搭配函数使用

create index ind_emp_lower_ename on emp(lower(ename));

### 按唯一性分类

索引列中的数据是否有重复值

**非唯一性**

create index ind_name on tb_name(col_name)；_\--column_

**唯一性**

create unique index ind_name on tb_name(col_name)；

**单列索引**

create index ind_name on tb_name(col_name);

**复合索引**

以索引里的第一个列作为主键列，当查询语句中出现该主键列，调用该联合索引

create index ind_emp_ename_job on emp(ename,job)

select ename,job from emp _\--调用_

select job,ename from emp _\--调用_

select job from emp _\--不调用_

select ename from emp _\--调用_

### 修改索引

只能改索引名称

alter index ind_emp_ename_job rename to ind;

### 删除索引

drop index ind;

**索引失效**

遇到不确定的情况： > < like not in

**优/缺点**

|     |     |
| --- | --- |
| 优点  | 缺点  |
| 1.大大加快数据的检索速度 | 1.索引需要占物理空间。 |
| 3.加速表和表之间的连接 | 2.当对表中的数据进行增加、删除和修改的时候，索引也要动态的维护，降低了数据的维护速度 |
| 2.创建唯一性索引，保证数据库表中每一行数据的唯一性 |
| 4.在使用分组和排序子句进行数据检索时，可以显著减少查询中分组和排序的时间 |

## 视图

提供预览，不真实存储数据，只访问数据

当视图结构与原表一致或部分一致时，可以通过视图更改原表部分数据

如果源表字段可以为空，那么也可以通过视图往源表里修改数据

**创建/封装视图**

create or replace view v_dept

as

select \* from dept _\--与原表数据结构一致_

with read only _\--加上只读权限_

create or replace view v_dept

as

select dptno,dname from dept _\--与原表数据结构部分一致_

**多个表合成的视图写只读权限也无法更改**

_\--连接表不能通过视图编辑_

create view v_emp

as

select ename,sal,dname from emp

inner Join dept on emp.deptno=dept.deptno;

## sequence 序列

序列是一个数据库项，用于生成一个整数序列，生成的序列用来填充数字型主键列。

### 创建序列

**普通序列**

create sequence 序列名

start with 开始数字

increment by 增量数

minvalue 最小数/nominvalue

maxvalue 最大数/nomaxvalue**循环序列**

create sequence 序列名

start with 开始数字

minvalue 最小数

maxvalue 最大数

increment by 增量数

cycle(循环后缀)

### 使用序列

**序列激活**：序列名.nextval

**用序列填充主键**

insert into 表名 (主键，列名，列名) values (序列名.nextval,值,值...)

insert into 表名 (主键，列名，列名) values (序列名.currval,值,值...)

**从dual表查看序列**

select seq_a.nextval from dual

select seq_a.currval from dual

### 修改序列

修改步数、增长值、修改为循环序列

_\--改为循环序列_

alter sequence seq_a cycle

# Day08

## 事务

定义：是数据库在执行一系列操作时，保证所有的操作都正确完成，要么都执行，要 么都不执行，保证数据的完整性

|     |
| --- |
| 事务的属性 |
| A:原子性（Atomicity）：事务是一个完整的操作。事务的各步操作是不可分的（原子的）；要么都执行，要么都不执行。 |
| C:一致性（Consistency）：一个查询的结果必须与数据库在查询开始时的状态保持一致（读不等待写，写不等待读）。 |
| I:隔离性（Isolation）：数据库中每一个用户的操作都是互不影响的，对于其他会话来说，未完成的（也就是未提交的）事务必须不可见。 |
| D:持久性（Durability）：事务一旦提交完成后，数据库就不可以丢失这个事务的结果，数据就永久的保存到数据库中 |

### 隐式事务

1.sqlplus 可以设置set autocommit on(它会自动地提单事务，不需要手动调用commit)

2.执行create、drop、grant、revoke等操作时，数据库会自动提交事务

### 杀死进程

两个账号同时修改一条数据会产生死锁，用管理员 查看被锁住的表，查看进程，杀掉进程

**1.查看被锁的表**

Select b.owner,b.object_name,a.session_id,a.locked_mode

From v$locked_object a,dba_objects b

Where b.object_id = a.object_id;

**2.查看哪个用户那个进程造成死锁**

SELECT s.sid, q.sql_text

FROM v$sqltext q, v$session s

WHERE q.address = s.sql_address AND s.sid = &sid _\-- 这个&sid 是第一步查询出来的_

ORDER BY piece;_\--查看导致锁死的SQL_

**3.查看session_id和serial_id**

Select

b.username,b.sid,b.serial#,logon_time

From v$locked_object a,v$session b

Where a.session_id = b.sid order by b.logon_time;

**\--4.杀掉进程**

SELECT 'alter system kill session ''' || sid || ',' || serial# || ''';' "Deadlock" FROM v$session

WHERE sid IN (SELECT sid FROM v$lock WHERE block = 1);

## sql优化

from后面的表执行顺序从右往左，从后往前

SQL执行顺序from——where——group by——having——select——order by

From后面的表执行顺序从右往左，从后往前

**具体优化方法：**

1.  如果结果集没有影响的关联，将小的表放在后面
2.  Where条件顺序，将过滤条数大的放在后面，过滤条数小的放在前面
3.  尽量减少对表的重复查询
4.  使用exists代替in：in后面用子查询，用exists代替in（看exists子查询中where条件，结果返回true或者fasle），如果in后面是具体的值，还是用in，用in的SQL语句总是多了一种转换过程
5.  distinct，查询效率低，要先排序，再去重
6.  索引正确使用，不能使用聚合函数，不能使用not
7.  大于等于效率要高于大于，用>=替代>，前提是整数相比较
8.  like效率低，使用instr代替instr(name,'n')>=1可以代替like'%c%'
9.  Where 是过滤行，having对分组的过滤
10. 要查看执行计划(F5, EXPLAIN )
11. 对 WHERE + ORDER BY 组合的优化, 在where进行筛选后，再进行 order BY
12. 尽量少排序 ORDER BY
13. 任何地方都不要使用select \* from表，用具体的字段列表代替“\*”，不要返回用不到 的任何字段
14. 尽量用 JOIN 替换子查询
15. 尽量少使用 OR ,索引失效
16. 尽量避免使用 UNION,使用 UNION ALL,然后再GROUP BY 去重
17. 尽早过滤数据, WHERE 过滤,使用 join时,先过滤再 JOIN
18. 尽量避免一条 UPDATE 更新多条记录, 用 MERGE INTO , 效率比 UPDATE 高
19. 尽量使用 TRUNCATE 替换 DELETE
20. 如果 时间列 只需要精度到天,则尽量使用date类型, 不使用 时间戳
21. 尽量保证在相同字段类型的比较
22. 使用group by 去重后计数
23. 尽量使用nvl()去空值

## exists

exists的右操作数是一个子查询，这个子查询是用来做存在性检查的，

exists() 子查询

## exists 存在性检查

### 简单使用方法：无关联关系

exists() 子查询有结果 返回 逻辑 1——ture

exists() 子查询没有结果 返回 逻辑 0——false

全都显示，或全都不显示

select \* from emp where exists(select deptno from dept where deptno>20);

### 有关联关系时时筛选

select \* from emp where exists

关联关系

(select deptno from dept where emp.deptno=dept.deptno and deptno>20);

### exists与in不同适用

外表大内表小/子查询小——适用in

外表小内表大/子查询大——适用exists

select \* from emp where exists(select deptno from dept where deptno>20);

外表

内表

select \* from emp where deptno in _\--外表 emp_

(select deptno from dept where emp. deptno = dept.deptno and deptno >20)

_\--内表dept_

not exists比not in好

## with as 子查询

作临时结果集，方便调用

with 表别名1 as (子查询), 别名2 as (子查询)...

# 补充

### GREATEST()

作用：筛选同一行中值最大的列

GREATEST(\[列\],\[列\],\[列\],...)

GREATEST(Chinese, Math, English) AS \`最高分数\`

### if()

结构：

if((条件),true语句,false语句)

例：

IF( (Chinese + Math + English) > (SELECT AVG(Chinese + Math + English) FROM student2), '是',

'否') AS \`是否超过总平均分\`

## 执行顺序全

标准逻辑顺序（几乎所有数据库都遵守）：

FROM → JOIN/ON → WHERE → GROUP BY → HAVING → SELECT → OVER (窗口) → DISTINCT → ORDER BY → LIMIT

1.  FROM：扫描表、多表 JOIN、ON 条件
2.  WHERE：行级过滤（分组前过滤）
3.  GROUP BY：分组、执行聚合函数（SUM/COUNT 等）
4.  HAVING：分组后过滤（只能用聚合结果）
5.  SELECT：投影列、计算普通表达式、给列起别名
6.  OVER () 开窗函数：在SELECT 之后、ORDER BY 之前执行LAG()/ROW_NUMBER()/RANK() 都在这里算分区 PARTITION BY、排序 ORDER BY 都在这一步生效
7.  DISTINCT：去重
8.  ORDER BY：全局排序
9.  LIMIT：取前 N 行

注：hive由执行顺序决定group by后不能使用别名，而有group by的情况下，order by对已分组的列一定要使用别名

总结：

· GROUP BY 永远写完整表达式 / 原始字段（不要依赖别名）

· ORDER BY 优先用别名（简洁、且规避 “原始字段不存在” 报错

## collection、concat补充

| 函数  | 作用  | 特点  |
| --- | --- | --- |
| collect_list (列) | 把多行的值收集成一个数组 | 不去重，保留所有值 |
| collect_set (列) | 把多行的值收集成一个数组 | 自动去重，只留唯一值 |
| concat_ws (分隔符，数组) | 把数组转成字符串，用分隔符连接 | 数组专用拼接函数 |

| 需求  | Hive 写法 |
| --- | --- |
| 多行合并成数组（不去重） | collect_list(col) |
| 多行合并成数组（去重） | collect_set(col) |
| 多行合并成字符串（不去重） | concat_ws(',', collect_list(col)) |
| 多行合并成字符串（去重） | concat_ws(',', collect_set(col)) |
| Map 转字符串 → 合并成字符串 | concat_ws(';', collect_set(map_concat(map_col, '=', ','))) |

# hive函数补充

## 字符串拼接

### concat (col1, col2, col3...) 基础拼接

任一 null，结果 null

用法：concat(字段1, 字段2, 字段3...)

### concat_ws (分隔符，col1, col2...) 带分隔符拼接

自动跳过 null，不会整体置空

用法：concat_ws('分隔符', 字段1, 字段2, 字段3...)

### || 管道符拼接（Hive2.1.0+ 支持

例：

select name || ':' || phone from user_info;

select col1 || col2 || col3 from table;

### collect_set + concat_ws 多行合并成一个字符串（特殊场景

例子：

\-- 按班级分组，拼接所有学生姓名，去重

select class, concat_ws('、', collect_set(name)) from student group by class;

\-- 不去重

collect_list select class, concat_ws('、', collect_list(name)) from student group by class;

## 格式转换

### from_unixtime(时间戳)：Unix 时间戳转时间

Unix 时间戳转时间

把秒级时间戳转为标准时间字符串

用法：

from_unixtime(ts, 'yyyy-MM-dd HH:mm:ss')

### to_timestamp() 字符串转时间

**用法：**

to_timestamp(string date, string format)

**示例：**

to_timestamp(dt_str, 'yyyy-MM-dd HH:mm:ss')

### date_format()时间转字符串  
用法：

date_format(date/timestamp/2024-01-01 12:00:00字符串,string 格式模板)

**示例：**

date_format(current_timestamp(), 'yyyy-MM-dd HH:mm:ss')

date_format('2019-05-12 06:34:08', 'yyyyMMdd')

## 数组操作

### 数组截取slice()

**用法：**

slice(数组, 起始下标, 截取长度)

**扩展：**

配合collect_list(),concat_ws()可以限定拼接的数量

concat_ws(',', slice(collect_list(sickness), 1, 5))