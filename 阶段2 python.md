# day19

## 执行.py脚本

/bin/python3 \[脚本\]

## 基本函数

|     |     |
| --- | --- |
| print() | 打印变量 |
| type() | 查看变量类型 |
| input() | 从键盘输入内容 |

注：判断数据类型type(变量)==int/list...

### print() 打印

可用print(type())

多字符串拼接：

print("..."+"..."+...)

print(f"..{\[变量\]}..{\[变量\]}..")

sno=1

age=18

sname="小明"

high=1.786

print(f"{sname}学号为{sno:05d}，年龄为{age}岁，身高为{high:.2f}米")

d代表整数，:05d表示保留5位整数，用0填充，:.2f代表位置2代表保留两位小数，f代表小数点

注：变量保留位数是可用 变量=f"{值:03d}"

**不换行打印**

print(变量1,变量2，...,end="\[换行符\]")

注：换行符可不写

### input() 输入字符串

输入一个字符串

变量=input()

## 数据类型

|     |     |     |     |     |     |     |     |     |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 数<br><br>字<br><br>型 | bool |     | 非<br><br>数<br><br>字<br><br>型 | str |     | 日<br><br>期<br><br>型 | time |     |
|     | list |     |     |
| int |     | tuple |     |     |
|     | set |     | datetime |     |
| float |     | dict |     |     |

注: 字符类型要用引号引起，一般情况用双引号，因为单引号被系统占用

自定义变量在双引号冲突的情况下使用单引号

str='"114ss"'——输出"114ss"

注：通过赋值决定变量数据类型

一行赋值多个变量

n1,n2,n3=值1,值2,值3

### 数据类型转换

int()、float()、str()

## 字符串

+拼接 \*复制 """保留格式

注""内有""的话，打印时不会输出里面的""，但用""" """括起来的可以正常输出里面的""

### 索引切片

用字符串变量\[n\]切片，从0开始，左闭右开

|     |     |
| --- | --- |
| 字符串\[n\] | 取第n+1个字符 |
| 字符串\[n:n2\] | 取第n+1个到第n2个 |
| 字符串\[n:\] | 取第n+1个到最后一个 |
| 字符串\[n:n2:n3\] | 取第n+1个到第n2个，步长为n3 |
| 字符串\[n::n3\] | 取第n+1个到最后一个，步长为n3 |

### len() 获取长度

len(str)——返回数值

### .find() .rfind()查找位数

查找特定字符串在目标字符串中第一次出现的位置，找到返回索引，没找到返回-1

.rfind()为倒叙查找

**结构：**

str.find("c")——返回数值

str.find("目标字符串",\[起始位置\])

str.find("目标字符串",\[起始位置\],\[终止位置\])

### .isdigit() 判断是否为数字

判断字符串是否为数字，返回bool

**结构：**

str.isdigit()

可用于if判断

### .count() 统计字符串出现的次数

统计目标字符串内某字符/字符串出现的次数，返回数值

**结构：**

str.count()

### .replace() 替换符号

**结构：**

字符串.replace("被替换的字符","用于替换的字符")

## 运算符

|     |     |     |     |     |     |     |     |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 数<br><br>值<br><br>运<br><br>算 | +   | 赋<br><br>值<br><br>运<br><br>算 | \=  | 比<br><br>较<br><br>运<br><br>算 | \== | 逻<br><br>辑<br><br>运<br><br>算 | and |
| \-  | +=  | !=  |
| \*  | \-= | \>  | or  |
| /   | \*= | <   |
| % 取余 | /=  | \>= | not |
| \*\* 求幂 | \*\*= | <=  |
| // 整除 | //= |     |     |

注：当 + 两边变量为字符类型时拼接

注：当 \* 一侧为数值，另一侧为字符串时会复制字符换

求幂：n1\*\*3

## 选择结构

### if else

结构：

if \[判断，没有括号\]:

\[语句\]

elif \[判断\]:

\[语句\]

else:

\[语句\]

注：一定要有:

用缩进或四个空格区分语句块

## 循环结构

### for 循环

**遍历字符**

for i in \[字符串\]:

\[语句\]

**遍历数字**

for i in range(1,10,2)

\[语句\]

range(n)

0~n-1

range(1,n)

1~n-1

range(1,n1,步长)

range()左闭右开

### while 循环

while true:

\[语句\]

## list\[\] 列表

列表可以完成大多数集合类的数据结构实现

列表中元素的类型可以不相同，它支持数字，字符串甚至可以包含列表（所谓嵌套）

——交错数组

**列表定义：**

列表用 \[\] 定义，元素之间用逗号分隔

list1=\[22,33,22, "hello",33,88,\[22, "hello","test"\]\]

print(list1\[-1\]\[1\]\[1\])

输出e

### 常用列表操作

|     |     |     |
| --- | --- | --- |
| **分**<br><br>**类** | **关键字/函数/方法** | **说明** |
| 增<br><br>加 | 列表.append(值) | 向列表末尾中追加单个元素，值可为列表，但结果为双层列表 |
| 列表.extend(\[值1,值2\]) | 在列表后面追加多个元素 |
| 列表.insert(index,值) | 将某个元素插放到列表中指定的位置 |
| 删<br><br>除 | 列表.remove(值) | 从列表中删除第一次出现在列表中的值 |
| 列表.pop(index) | 删除索引对应的元素，如果不加索引，默认删除最后元素，同时返回删除元素的引用关系 |
| del 列表\[1:2\] | 按照切片指定索引删除列表元素 |
| 列表.clear() | 清空列表 |
| 修<br><br>改 | 列表\[索引\] = 值 | 修改指定索引的数据，数据不存在会报错 |

注：用append(\[值1,值2\])追加的是"\[值1,值2\]",.insert同理

|     |     |     |
| --- | --- | --- |
| **分类** | **关键字/函数/方法** | **说明** |     |
| 查询  | 列表\[索引\] | 根据索引取值，索引不存在会报错 |     |
| 列表.count(值) | 返回列表中包含某个值的个数 |     |
| 列表.sort() | 将列表中的元素进行排序，(reverse=True)代表降序 |     |
| 列表.reverse() | 列表的反转，用来改变原列表的先后顺序 |     |
| len(列表) | 列表长度（元素个数） |     |
| max(列表) | 返回列表元素最大值 | 元素必须全部为数值 |
| min(列表) | 返回列表元素最小值 |
| sum(列表) | 返回列表元素的总和 |
|     | round(值,位数) | 保留小数 |     |

### 字符串与列表转换 .split 分割字符串为列表

字符串.split("分隔符")

### .join 拼接列表为字符串

变量="分隔符".join(列表)

# Day20

## tuple元组

只读的列表——不能做数据的增删改，修改只能新建新的元组

优点：安全性，效率高，系统占用元组类型

**结构：**

变量=(值1,值2,...)

**支持：**

索引下标——tuple\[i\]

循环

## set集合

类似列表，去掉重复数据，保留第一个，无序——不支持索引下标

**结构：**

变量={值1,值2,...}

定义空集合：

集合名=set()——{}被字典占用

**支持：**

|     |     |     |
| --- | --- | --- |
| **分类** | **方法** | **作用** |
| 增加  | 集合.add(值) | 添加到随机位置 |
| 删除  | 集合.pop() | 随机删除值 |
| 集合.remove(值) | 删除特定值 |
| 集合.clear(值) | 清空集合 |

## dict字典

字典是一些值和其特殊的索引(键)的集合，可用于通过键查找值

特点：

无序——不能用索引

键(key)必须唯——建冲突时只显示最后一个

创建空字典使用 { }

**结构：**

字典名={键1:值1,键2:值2,...}

**字典的调用：**

字典名\[建\]

注：建为字符串时，调用要加上""

### 字典的函数

|     |     |     |
| --- | --- | --- |
| **分类** | **函数** | **说明** |
| 增加  | 字典\[键\]=值 | 键不存在，会添加键值对 |
| 修改  | 字典\[键\]=值 | 键存在，会修改键值对 |
| 删除  | 字典.pop(键) | 删除键和对应的值<br><br>没有索引，效果相同 |
| del字典\[键\] |
| 字典.clear() | 清空字典 |
| 查询  | 字典.keys() | 获取所有键 |
| 字典.values() | 获取所有值 |
| 字典.items() | 获取所有键值对 |

### 字典用于循环

for i in 集合.keys():

语句

注：in后直接写集合名，默认遍历键，可以不用写.keys()

for i in 集合.values():

语句

## 推导式

经过选择，循环等过程的一串数据，选择，循环可以调换位置

用于快速、简洁地创建列表、字典、集合等数据的一行代码语法

**推导式格式：**

\[算数式 for i in 输入源 if 条件\]

表达式 for 变量 in 输入源 if 条件

表达式 for 变量 in 输入源 if 条件 for 变量 in 输入源 if 条件

|     |     |     |     |
| --- | --- | --- | --- |
| 输<br><br>入<br><br>源 | range | 输<br><br>出<br><br>源 | list |
| list | tuple |
| tuple | set |
| set | dic |
| dic |     |

**例子**

- 推导式输出字典

listA=\[1,2,3,4,5,6\]

dic1={i:str(i\*\*2) for i in listA}

print(dic1)

\# 输出：{1: '1', 2: '4', 3: '9', 4: '16', 5: '25', 6: '36'}

t1=((1,100),(2,30),(3,80),(4,234))

list2=\[{"name":elm\[0\],"value":elm\[1\]} for elm in t1\]

print(list2)

\# 输出：

\[{'name': 1, 'value': 100}, {'name': 2, 'value': 30}, {'name': 3, 'value': 80}, {'name': 4, 'value': 234}\]

- 由字典的元素新建列表

students=\[{...},{...}...\]

score=\[item for elm in students for item in \[elm\['name'\],elm\['score'\]\]\]

print(score)

\# 输出'name'的值,'score'的值

- 推导式使用列表函数

listE=\[\]

\[listE.append(stu\[elm\]) for stu in students for elm in stu if stu\['score'\]<60\]

print(listE)

\# 可用推导式进行列表.append

- 推导式if else结构（此时if ifel 要位于for前）

str1="hello","worldd","test"

result=\[True if len(elm)>5 else False for elm in str1\]

print(result)

\# 输出：\[False, True, False\]

- 不用if else实现输出true/false（判断是本身就是bool）

str1="hello","worldd","test"

result=\[len(s) > 5 for s in str\]

print(result)

\# 输出：\[False, True, False\]

# Day21

## 自定义函数

### 基本结构

def 函数名(形参):

函数体

return 参数（可没有参数）

注：()后要有:，函数体要有缩进

### 规范命名法

骆驼命名法：sayHelloTest一般用于自定义函数

帕斯卡命名法：SayHelloTest一般用于自定义类

下划线命名法（系统自带）:say_hello_test

### 返回值

只能激活一个return

要返回多个值：return 值1,值2,...

一个return返回多个值时返回的是tuple

### 输入默认值

def 函数名(形参1=n1,形参2=n2...)

若调用函数时没有传入参数，则会使用默认参数

由于传入参数的顺序都是从第一位给的，无法确认传入几位参数，传给第几位形参，所以默认值要么都给，要么给第2个及之后的参数，不能只给第一个，否则会报错

同理，不定长参数不能放在前面

### 输入键值对

函数名(参数1=值1,参数2=值2)

键值对换为= 键不用加""

### 不定长参数

加了星号 \* 的参数会以元组(tuple)的形式导入，存放所有未命名的变量参数

加了两个星号 \*\* 的参数会以字典(dict)的形式导入

def getNumDict(n1,\*\*n):

&nbsp;   print(n1)

&nbsp;   print(n)

getNumDict(8,age=40,name="周杰伦")

\# 输出

8

{'age': 40, 'name': '周杰伦'}

### 值类型、引用类型数据

<div class="joplin-table-wrapper"><table><tbody><tr><td rowspan="5"><h3>值类型</h3></td><td><h3>int</h3></td><td rowspan="5"><h3>存于栈</h3></td><td rowspan="5"><h3>引用类型</h3></td><td><h3>list</h3></td><td rowspan="5"><h3>存于堆</h3></td></tr><tr><td><h3>float</h3></td><td><h3>set</h3></td></tr><tr><td><h3>bool</h3></td><td><h3>dict</h3></td></tr><tr><td><h3>str</h3></td><td><h3>class</h3></td></tr><tr><td><h3>tuple</h3></td><td><h3></h3></td></tr></tbody></table></div>

## 规范python程序写法

### 全局变量

可在整个.py文件内调用的

### 函数

存放自定义函数的地方

### 入口

与终端交互的地方，类似于Main()函数，而且程序一运行就执行

定义方法：

if \__name_\_=="\__main_\_":

程序体

注：要有冒号，程序体要有缩进

# Day22

## File(文件) 读写

### 文本文件写

with open("文件绝对路径",mode='模式',encoding='UTF-8') as 文件别名:

文件别名.功能

文件别名.close()

注：别名可看成一对象

|     |     |
| --- | --- |
| 模式  | 功能  |
| w   | 覆盖写入 |
| a   | 追加写入 |
| r   | 读取文件 |

|     |     |     |
| --- | --- | --- |
| 功能  | 作用  | 返回值 |
| .read() | 读取所有内容 | 字符串 |
| .readline() | 一行一行读取内容 | 列表  |
| .write() | 写入内容 | null |

## Python库

标准库、扩展库、自定义库

在 python 用 import 或者 from ... import 来导入相应的库

### 标准库

安装时自带，需要用时直接导入，如csv、time、datetime、os、json

### 扩展库

不自带需要另外下载导入才能使用，如pymysql、flask、pandas、requests

### 自定义库

如

class()所有库里最小的单位

## try except 异常试错

出错时跳转，可有异常的预定义

结构：

try:

函数体

except 预定义错误:

函数体

except:

|     |     |
| --- | --- |
| 异常  | 说明  |
| ValueError | 值类型异常 |
| ZeroDivisionError | 除数为0异常 |
| TypeError | 类型错 |
| IndexError | 下标越界 |
| FileNotFoundError | 文件找不到 |
| KeyError | 字典键不存在 |
| Exception | 异常（以上的父类） |

### raise 主动抛出异常

常配合选择结构使用

raise 异常类型()

注：异常类型可自定义，但必须为Exception的子类型

## 面向对象

### 类：一类功能的集合

类及对象包含属性和方法

属性：静态特征 全局变量 成员

方法：动态特征 函数 功能

注：类的方法必须要传入一个参数作为占位参数

**定义类内函数：**

def sayHello(self):

print(f"我的名字叫{self.name},我的年龄是{self.age}")

魔法方法：不需要调用就可以自动执行。

作用：初始化对象的成员(给对象添加属性)

——构造函数 注：构造函数∈魔法方法

**定义构造函数：**

def \__init_\_(占位参数，形参1，形参2...)

占位参数.属性1=形参1

占位参数.属性2=形参2

...

注：占位参数是用于区分局部参数与类内属性的参数

类内的方法必须有占位参数，可以无形参

**构造函数的使用：实例化时可初始化属性**

对象名=类名(值1,值2,...)

### 类的继承(指定父类)

def 子类(父类)：

属性

方法

注：子类的对象继承父类的属性和方法，可直接调用父类方法

**方法重写：**

子类的方法与父类重名，子类对象调用该函数会调用重写后的方法

**多态：**

需求：

一个出发点，多个返回结果

有继承，子类的方法名与父类函数名相同

### 对象：实例化的类，承载类的属性和方法

python对象的定义：

对象名=类(名)

# Day23

## time&datetime库

### time库

|     |     |     |
| --- | --- | --- |
| 方法  | 作用  | 返回值 |
| time.localtime() | 获取当前时间 | 结构化时间(元组) |
| time.strftime() | 日期转字符串 | 字符串 |
| time.strptime() | 字符串转日期 | 日期  |
| time.sleep(sec) | 休眠时间，以秒为单位 | null |
| time.perf_counter() | 获取系统启动的时间 | 浮点秒数 |

time.localtime()返回

(tm_year=2026, tm_mon=5, tm_mday=7, tm_hour=9, tm_min=34, tm_sec=10, tm_wday=3, tm_yday=127, tm_isdst=0)

**使用方法：**

time.strftime("%Y-%m-%d %H:%M:%S",日期)

time.strptime(字符串,"%Y-%m-%d %H:%M:%S")

注：使用strptime时需保证日期格式与字符串格式一致

注："%Y-%m-%d %H:%M:%S"为日期格式，其中的 - 和 : 可任意替换

### datetime库

|     |     |     |
| --- | --- | --- |
| 方法  | 作用  | 返回值 |
| datetime.datetime.now() | 获取当前时间 | 日期  |
| datetime.datetime.strftime() | 时间转字符串 | 字符串 |
| datetime.datetime.strptime() | 字符串转日期 | 日期  |

**使用方法：**

datetime.datetime.strftime(日期,"%Y-%m-%d %H:%M:%S")

注：strftime的"%Y-%m-%d %H:%M:%S"为日期格式，其中的 - 和 : 可任意替换

datetime.datetime.strptime("20230211","%Y%m%d")

注：strptime的"%Y%m%d"日期格式必须与字符串的格式匹配

MySQL与datetime结合比较紧密

mysql的date仅包含年月日，time仅包时分秒，datetime包含年月日，时分秒

## pymysql库

可通过python对数据库进行增删改

调用pymysql库函数：pymysql.函数

安装：pip3 install pymysql

本地ip：

192.168.32.139

局域网内网 IP（私有地址）

电脑在路由器局域网里的真实 IP，同 WiFi / 同路由器下的手机、其他电脑都能访问

127.0.0.1

本地回环地址（Loopback），只能自己本机访问自己，只在本机内部通信

python函数最好分为三类：结构类、编辑类、查看类

### 函数结构

def structure():

**第一步 线连接数据库（铺路）：**

conn=pymysql.connect(

host="127.0.0.1",

user="root",

password="root123456",

database="test")

**第二步 创建对象指向连接（造车）：**

vehicle=conn.cursor()

**第三步 执行语句：**

vehicle.execute("语句")

额外语句

|     |     |     |     |
| --- | --- | --- | --- |
|     | 额外语句 | 作用  | 返回类型 |
| 编辑类 | conn.commit() | 提交事务 | null |
| 查看类 | .fetchall() | 获取所有查询结果 | 嵌套元组 |
| .fetchone() | 只取一行 | 单行元组 |
| .fetchmany(n) | 取 n 行 | 嵌套元组(n行数据) |

注：查询类里datetime.datetime.strftime()时间结构'%Y-%m-%'只能用单引号

注：.execute()返回查找到的行数，为int类型

**第四步 关闭数据库：**

vehicle.close()

conn.close()

注：第一步和最后一步可拆到两个方法内

# Day24

## HTML 超文本标记语言

Hyper Text Markup Language

**部分关键字：**

1.&lt;html&gt;&lt;/html&gt;:根标签

2.&lt;head&gt;&lt;/head&gt;:头标签

3.&lt;title&gt;&lt;/title&gt;:头标题标签，在&lt;head&gt;标签里设置。

4.&lt;meta charset="utf-8"&gt;:常用于指定页面编码，在&lt;head&gt;标签内.

5.&lt;body&gt;&lt;/body&gt;:页面的内容基本上写在此标签内。

6.标题标签：&lt;h1&gt;&lt;/h1&gt; h1 ... h6

7.段落标签：&lt;p&gt;&lt;/p&gt;

8.超级链接标签：&lt;a href="https://www.baidu.com" target="\_blank"&gt;百度&lt;/a&gt;

**注：**href后可写路由名

9.表格标签：&lt;table&gt;&lt;tr&gt;&lt;td&gt;学号&lt;/td&gt;&lt;td&gt;姓名&lt;/td&gt;&lt;/tr&gt;&lt;/table&gt;

10.表单标签：&lt;form action="" method="post"&gt;表单元素控件&lt;/form&gt;

11.表单元素控件：&lt;input &gt;

文本显示：&lt;input type="text" name="tname" value="动漫" readonly&gt;

类型：number(step 0.1) date password

提示功能：&lt;input type="text" placeholder="请输入电影名称"&gt;

**注：** &lt;form action="路由名" method="post"&gt;配合

&lt;p&gt;&lt;input type="text" name="dataname" value="default" readonly&gt;&lt;/p&gt;

可由网页输入，向特定路由传输驶入的数据，value=后设置默认值，reanonly为只读(可选)

后端： @app.route("/addSubmit",methods=\["POST"\]) **注：**后端也要改methods

def addSubmit():

tid=request.form.get("tid")

tname=request.form.get("tname")

tcontent=request.form.get("tcontent")

**注：**网页内打印传给王爷的值可用{{info}}

必须有return("网页.html",info=值)

12.下拉框标签：&lt;select&gt;&lt;/select&gt;

&lt;select name="tid"&gt;

&lt;option value="1"&gt;喜剧&lt;/option&gt;

&lt;option value="2" selected&gt;动画&lt;/option&gt;

&lt;/select&gt;

13.图片标签：&lt;img src="static/p11.png" width="300" &gt;

style类型

|     |     |     |     |
| --- | --- | --- | --- |
| 关键字 | 名称  | 作用  | 单位/默认值 |
| width | 宽度  | 设置盒子的宽度 | px 英文pixel 像素 |
| height | 高度  | 设置盒子的高度 |     |
| margin | 外边距 | 设置盒子和其他盒子<br><br>之间的距离 |     |
| border | 边框  | 设置盒子边框 | 默认透明 |
| padding | 填充  | 内边距 |     |

## flask库

安装：pip3 install -i https://pypi.tuna.tsinghua.edu.cn/simple Flask

默认端口port=5000

主程序（包括类）和templates、static文件放到一个文件夹内

.html文件需传到templates下

图片在需要在项目文件夹下创建文件夹static上传图片

# Day25

接口(API)检测

postmane

http://127.0.0.1/路径

## os库

os（operating system）是Python程序与操作系统进行交互的接口

os库导入：import os

1、os.listdir()返回对应目录下的所有文件及文件夹

2、os.mkdir()创建目录（只支持一层创建）即新建一个路径

3、os.open()创建文件相当于全局函数open()（IO流） os.open("t.txt",os.O_CREAT)

4、os.remove（文件名或路径）删除文件

5、os.rmdir()删除目录

**6、os.system()**执行终端命令os.system("touch a.txt")

终端命令内直接写Linux控制台命令行

# pandas 库

Pandas 是 Python 语言的一个扩展程序库，用于数据分析。

可处理文件：CSV、JSON、Excel (不能处理.txt)

**安装：**

python终端输入安装： pip3 install -i https://pypi.tuna.tsinghua.edu.cn/simple pandas

json格式：字典包列表

data = {"Site":\["Google", "Runoob", "Wiki"\], "Age":\[10, 12, 13\],"sss":\[22,33,44\]}

## 格式转换

### 字典转dataframe

字典嵌套列表转panda（表格形式）

df = pd.DataFrame(data)

打印列

print(df\["列名"\])

## 文件处理

### panda的csv文件

**文件导入**

df = panda.read_csv("文件位置")

**数据处理：**筛选语句可不断往后添加

df=df\[\["id","title","rate"\]\]\[df.rate>7.5\]

**文件导出**

df.to_csv("导出位置",mode="a", header=False, index=False, sep="|" )

a追加写 不加标题 不加行索引 指定分隔符

Pandas的JSON文件

json.loads()

将字符串转化为字典

### panda的json文件

**将字符串转为字典**

json.loads(数据)

**将字典转为字符串**

json.dump(data,jf)

注：以上函数都有其他参数

### Pandas的excel文件

**安装openpyxl：**

pip3 install openpyxl

**使用方法：**

df = pd.read_excel("student.xlsx",sheet_name="Sheet1",header=1)

表格名 忽略前几行

**选项**

|     |     |
| --- | --- |
| sheet_name | 指定了读取excel里面的哪一个sheet |
| usecols | 指定了读取哪些列 |
| nrows | 指定了总共读取多少行 |
| header | 指定了列名在第几行，并且只读取这一行往下的数据 |
| index_col | 指定了index在第几列 |
| engine="openpyxl" | 指定了使用什么引擎来读取excel文件 |

注：其余同csv

## 数据筛选

### df.\[\]一般筛选

**筛选列**

df\[df.Age>11\]\["Age"\]

筛选df中的Age列中大于11的值

注：一个\[\]代表行，相当where，保留索引

df\[(df.Age>11) & (df.sss>35)\]

**获得具体数值**

df\["列名"\]\[行索引\]=99

修改整列\[输入的数量必须与列的行数相等\]

df\["列名"\]=\[值1,值2,...\]

中括号嵌套代表筛选列\[\[\]\]不保留索引

**数据类型转换.astype()**

df=df\[df.rate.astype(float)>7.5\]

常用于筛选大小

### 使用聚合函数

df=df\["票房"\].sum()

### .loc 筛选

df.loc\[行, 列\]

**带条件筛选**

df.loc\[ 行条件 , 列条件 \]

筛选全部行：行条件为:筛选全部行

df=df.loc\[:,(df>=passLine).all()\]

筛选列值全部大于passLine的全部列

df.loc\[:,(df > X).any()\]

筛选至少有一个值大于X的列

### 条件筛选

**获取首行**

first_row = df.head(1)

first_row = df.iloc\[0\]

**获取最后一行**

last_row = df.tail(1)

first_row = df.iloc\[-1\]

**条件筛选**

df_northChina=df\[df\["地区"\]=="华北"\]

可筛选出所有符合条件的行，不用循环

可配合

df_northChina.to_csv("/root/Day41/northChina.csv", sep="|", index=False, encoding="utf-8-sig")

实现数据快速导出

### .groupby分组、聚合

.groupby("列1")\["列2"\].聚合函数()

以列1分组，对列2进行聚合

注：有返回值需要接收

df4=df.groupby("制片地区")\["票房"\].sum()

df4.sort_values("票房",ascending=False).head(3)

print(df4)

统计出票房总数最高的三个国家

### .sort_values 排序

.sort_values(by="列名",ascending=False)

注：有返回值需要接收，by=可不写，ascending=False表示降序，默认升序（不写）

### merge

### 列重命名/重排序

df=df.rename(columns={"自定义":"国标代码"})  # 列名重命名

df=df\[\["分类名称","国标代码"\]\]  # 列排序

## 数据提取

### 循环每一行

for index,row in df.iterrows():

内容

可循环每一行，其中index为行索引，row为列值，类型为pandas.core.series.Series

row为多列的值，用typeName=row\["分类名称"\] GBcode=row\["国标代码"\]可以使用具体列值

### 提取特定行的数据

df_northChina=df\[df\["地区"\]=="华北"\]  # 分类数据

## 数据处理

### .insert 插入数据：

.insert(列索引."列名",值)

### 更改某行小数位数

df=pandas.read_excel("D:/document/python/Day41/超市数据.xlsx",sheet_name="工作表1")

df\['销售额'\] = df\['销售额'\].map(lambda x: f"{x:.2f}")

### 空值去除

**删除行.dropna()**

df1=df1.dropna(subset=\["码值", "码值含义"\], how="all")

## 爬虫

**安装requests 库：**

pip3 install -i https://pypi.tuna.tsinghua.edu.cn/simple requests

pip3 install urllib3==1.26.15

注：预览处格式为json时才可用以下方法

### 第一步：查看响应码

url="https://movie.douban.com/j/chart/top_list"

params={"type":"24","interval_id":"100:90","action":"","start":"0","limit":"20"}

headers={"user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/148.0.0.0 Safari/537.36 Edg/148.0.0.0"}

\# cookie可选

cookie='bid=4tFRLddnZaI; ll="118304"; ap_v=0,6.0'

result=requests.get(url=url,params=params,headers=headers)

print(result)

注：请求固定格式为：

result=requests.get(url=,params=,headers=)

url=,params=,headers=必须写

**响应码**：访问一个网页时，浏览者的浏览器会向网页所在服务器发出请求。当浏览器接收并显示网页前，此网页所在的服务器会返回一个包含 HTTP 状态码的信息头（server header）用以响应浏览器的请求。

**响应码分类**

1xx（信息性状态码）：表示接收的请求正在处理。

2xx（成功状态码）：表示请求正常处理完毕。

3xx（重定向状态码）：需要后续操作才能完成这一请求。

4xx（客户端错误状态码）：表示请求包含语法错误或无法完成。

5xx（服务器错误状态码）：服务器在处理请求的过程中发生了错误。

### 第二步：转换数据

info=result.json()

print(info)

\# 提取数据

info=result.json()

\# print(info)

\# 转为dataframe数据

df=pandas.DataFrame(info)

df=df\[\["id","title","release_date","score"\]\]

df\["tid"\]=6 # 记得改tid

### 第三步：去重

df=pandas.read_csv("/root/python/douban.csv")

df=df.drop_duplicates(subset=\["id","title","release_date","score"\])

df.to_csv("/root/python/douban_1.csv",index=False)

### 第四步，到入到数据库

.py程序

os.system("cp /root/python/douban_1.csv /usr/local/mysql/data/douban_1.csv")

os.system("/root/python/db.sh")

shell脚本

host="192.168.17.129"

port="3306"

user="root"

passwd="root123456"

db="test"

dbdir="/usr/local/mysql/data"

\# 导入表格

csvin="LOAD DATA INFILE '/usr/local/mysql/data/douban_1.csv' INTO TABLE Movie

CHARACTER SET utf8

FIELDS TERMINATED BY ','

LINES TERMINATED BY '\\n'

IGNORE 1 LINES "

mysql -h$host -p$passwd -P$port -u$user $db -e "$csvin"

\# 4.查询 Movie表验证结果

mysql -h$host -p$passwd -P$port -u$user $db -e "select \* from Movie"

# Day27 考试

# Day28

## 部署项目

### 部署到Nginx

1.命令行输入code /usr/local/nginx/conf/nginx.conf

2\. 动态网站可以使用代理转地址

将上方红框替换为以下内容

location / {

root html;

proxy_pass http://127.0.0.1:5000; #请求转向

index index.html index.htm;

}

3\. 重启Nginx服务：/usr/local/nginx/sbin/nginx -s reload

4.（可选）更改柱状图、饼状图的ajax地址

自主开发学生信息管理平台，支持学生信息录入、成绩管理、班级查询、数据统计功能。使用 Python+Flask+MySQL 开发，HTML 构建前端页面，Flask 实现接口与逻辑处理，MySQL 存储学生、成绩、班级数据，通过 SQL 完成数据操作。使用 Pandas 做成绩分析，ECharts 展示分数分布，项目通过 Nginx 部署运行。