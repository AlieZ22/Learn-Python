# Python 教程

关于大作页：

1. 不超过四人组队 

## 简介

#### python发行版

1. CPython
2. Anaconda
3. Python(x,y)
4. Active Python

#### Jupyter

自带的iPython，显示历史命令（%hist）

快捷键：Ctrl+L 清屏; Ctrl+a 光标移至开头；Ctrl+r 反向搜索等， 上下键可导航

#### 课堂目标

1. 流程控制，面向对象
2. 数据获取，储存
3. 数据可视化
4. web开发

## 基础

数值运算： '/' 表示除法  ；‘//’ 表示除后取整数部分

单下划线 _ 表示一个（无关紧要的）临时变量

**注释**

1. 单行注释：#
2. 多行注释：三个引号

开发工具：

- IDLE-自带的集成开发工具：用户体验不太好
- IPython-交互式解释器
- PyCharm-开发神器
- VSCode-高级编辑器

#### 字符串

可是使用单引号，也可以双引号，没有区别。

 \ 用于转义，用r省去转义（raw）如‘a/n’  与 r'a/n'

''' ... ''' 或 “”“ ... ”“” 支持换行，在复杂语句中表现较好

其他技巧：

1. 加号 拼接字符串
2. 乘号 重复字符串
3. 字符串常量直接拼接（em. 'hello ''world'）
4. 字符串可下标（0 起始 ；-1 最后）-语法糖
5. ：用于切片，左闭右开（左边 开始，右边-左边 表示长度），右边超出长度不会报错，因为仅仅是切片，但取下标时不行
6. 插基本类型的值 ‘The result is %d’ % 3.5  或 '{0}ffff{1}ffff{2}ddad'.format(1,2,3)

注意：

1. python字符串是不可变的，不能直接改变
2. len()获取字符串的长度

#### 列表

python中的列表支持多种数据类型，与数组类似，可下标可切片。与字符串不同的是 列表是可变的。

使用 [ ]来表示一个列表

type()可用来判断数据类型	type(array)  -->  list

 列表的追加：array.append()

#### 流程控制

if 用于分支，可有任意多个elif， True/False表示真假。 

None/False/0/""/()/[]/{}  这些会判定为假；

while for 用于循环; range 可以生成遍历序列，左开右闭

**逻辑的关联使用 and or not来表示**

#### 函数

def 开头，表示定义一个函数，（函数里面可以添加文档字符串docstring），函数要有返回值。

注意：**圆括号()后一定要加冒号：**

```python
def foo(bar, buz):
    "This is a docstring"  # 可选
    return bar+buz
```

![1574122563753](https://github.com/AlieZ22/Learn-Python/blob/master/res/1574122563753.png)

实例：使用turtle类进行绘图

```python
def square2(length):
    for i in range(4):
        turtle.forward(length)
        turtle.left(90)
        turtle.speed(2)
    square2(length+20)

if __name__ == "__main__":
    square2(100)
```

#### 分形(Fractal)

又称碎型，具有自相似的性质，便于使用递归来实现。

```python
import turtle

def tree(length,threshold):
    "使用递归构造分形结构"
    if length <= threshold:
        pass
    else:
        # 树干
        turtle.speed(5)
        turtle.forward(length)
        turtle.left(45)
        tree(length/2, threshold)
        turtle.right(90)
        tree(length/2, threshold)
        turtle.left(45)
        turtle.backward(length)
        

if __name__ == "__main__":
    tree(200, 5)
```

运行结果如下：

![1574124623922](https://github.com/AlieZ22/Learn-Python/blob/master/res/1574124623922.png)

或者还可以绘制其他很多图像：

![1574126175874](https://github.com/AlieZ22/Learn-Python/blob/master/res/1574126175874.png)

![1574127208180](https://github.com/AlieZ22/Learn-Python/blob/master/res/1574127208180.png)



## 面向对象

面向对象三要素：继承，封装，多态

（插）C++中struct与class有何区别？ 

```
struct 默认的访问权限是public，而class则需要自己声明访问权限
```

类的docstring 可以用 ''' some instruction '''' 

#### __ init __函数

类似于初始化函数，实例成员可以动态添加/删除（不同于Java,C++），可使用hasattr判断是否包含某成员

```python
def __init__(self):
	self.data = []    # 说明有一个成员data是列表类型的
    
class Complex:
    # 若变量放到这个位置，那么相当于类的静态成员
    def __init__(self, realpart, imagpart):
        # 变量放在__init__函数中声明，则不代表静态成员
        self.r = realpart
        self.i = imagpart      
class A:
    def __init__(self, a, b):
        self.a = a
        self.b = b
# 下面动态添加和删除变量
a = A(1,2)
a.c = [1,2,3]  # 动态添加
del a.c  # 动态删除
hasattr(a,"c")   # 查看a对象是否还有c这个成员
>>False
```

#### 成员可见性

私有的实例属性用两个下划线开头

在成员变量前加一个下划线，仍是普通的成员，只是表示不希望别人修改。在对象.之后不会自动补全，只有在._之后才会显示。

#### 继承，多态

```python
class Base:
    def foo(self):
        print("foo in Base")
class Sub(Base):              # 实现继承
    def foo(self):
        print("foo in Sub")   # 同名函数实现多态

# isinstance(对象名，类名)可以判断该对象是不是这个类型
```

#### 函数式特性

1. Closure

   ```python
   # 闭包
   def outer():
       count = 0
       def inner():
           nonlocal count
           count+=1
           return count
       return inner   # 函数可以返回函数
   
   >> x = outer()
   >> type(x())
   function
   >> x()
   1
   >> x()
   2
   ```

   

2. Partial Function

   ```python
   # 偏函数
   from functools import partial
   def multiply(x,y):
       return x*y
   
   double = partial(multiply,2)  # 指定multiply的第一个参数，使double变成一个单参函数
   triple = partial(multiply,3)  # 同上
   
   >> double(5)
   10
   >> double(5)
   15
   ```

   

3. List Comprehension

   ```python
   # 列表推导
   [i*2 for i in range(10) if i%2==0]
   >>[0, 4, 8, 12, 16]
   
   a = [1,2,3,4,5]
   b = [2,3,4,5,6]
   [(x,y) for x in a for y in b]
   >> 产生a,b的笛卡儿积
   ```

   

4. Map-Filter-Reduce

   ```python
   from functools import reduce
   def even(i):
       return i%2==0
   a = [i for i in range(10)]
   filter(even,a)
   print(list(filter(even,a)))
   
   a = [i for i in range(10)]
   b = list(map(lambda x: x**2, a))  # map返回一个迭代器
   print(b)
   print(list(filter(lambda x: x>50,b)))
   
   # 计算filter/容器内从左到右两两运算 最终返回单值---使用reduce
   a = [1,4,2,-1,5]
   def _min(x,y):
       if x<y:
           return x
       else:
           return y
   result = reduce(_min,a,-10)  # 后面的可选参数为initial
   print(result)
   ```

   

函数中可以存入变量。。。

lambda表达式，可以理解为匿名函数

```python
add = lambda x,y : x+y
add(1,2)
>>3
# 注意：python的lambda表达式不支持换行
```

#### 元组

元组与列表相似，都是有序的。**元组是不可变的**，用逗号分隔，可以嵌套。

用()构造空元组。**构造单元素列表(a,)** , 如果元组元素是可变的，则可以修改（坑）

使用(a,)构造单元素元组，若直接用(a)则会被理解成一个数据（如(1)被理解成整数）

```python
t = 123, 543, "hello"   # 这样赋值后其类型是tuple
>>>type(t)
tuple
# 因为元组是不可变的，所以不能改变其中的元素值
# 若元组元素可变则可修改的坑
x = (1,2,3[4,5])
# 若直接x[-1] += [6] 则会报出错误，但实际上可以加进去（这是编译器的一个未修复的问题）,但使用x[-1].append(...)就没有问题

```

元组与列表的区别：

1. 元组直接表达的是实际地址空间的内容
2. 列表则是一些引用，指向实际地址空间的内容

所以元组中元素不可变，而列表中元素可变的原理是：引用指向其他地址空间的内容了

#### 字典

字典类使用键值对进行组织，不能通过下标值随机访问

任何不可变对象均可作为键（元组可以，但不能包含可变元素），元组可哈希（如{(1,2):5}）

空字典使用{}来构造；非空字典在{}内用逗号分割的键：值对表示

list(d.keys())可以返回字典的所有键；如果需要有序，使用sorted(d.keys())

直接使用list(d)会返回键的列表

```python
# 遍历字典的方法
diction = {"zs":90,"ls":80}
for key in diction.keys():
    print("{}:{}".format(key,diction[key]))
# 若要判断一个键是否在字典内
"jack" in diction
>>>False
# 取出对应键的值，可使用get()，这个方法还能标取默认值，若没有默认值则返回None
diction.get("jack",0)

```

字典推导：

```python
{x:x**2 for x in range(10) if x%2==0}

```

#### 集合

无序，不重复的数据结构。支持成员测试，添加，删除等操作。

初始化使用{}，但其中不是键值对。注意，{}表示空字典，空集合用set()表示，{1}表示集合

可以进行集合的并(|)，交(&)，差(-)等运算

```python
type({})
>>> dict
type(set())
>>>set
type({1})
>>>set

```

集合推导：

```python
{x for x in range(10)}

```



## 网络爬虫

#### 盗亦有道

robots.txt（统一小写）协议 规范了爬虫可爬取的内容

1. robots协议并不是一个规范，只是约定俗成的，并不能保证网站的隐私
2. 支持通配符，用字符串来表示

#### License

开源软件许可证 https://choosealicense.com/

最宽松的：MIT

#### Requests库

```python
# 启动python内置服务器
'''
    1,cmd进入到要当成服务器根目录的文件夹
    2,键入python -m http.server <指定端口号>
'''
import requests

response = requests.get("http://localhost:8000/index.html")
code = response.status_code
print(code)
text = response.text
print(text)

```

#### 正规式

```python
import re
# 使用re,捕获所有超链接
result = re.findall('href="(.*?)"',text)
print(result)
>>>['apple-touch-icon.png', 'css/bootstrap.min.css', 'css/bootstrap-theme.min.css', 'css/fontAwesome.css', 'css/light-box.css', 'css/templatemo-style.css', 'https://fonts.googleapis.com/css?family=Kanit:100,200,300,400,500,600,700,800,900', 'index.html', '#', 'img/intro1.jpg', 'img/intro2.jpg', 'img/intro3.jpg', 'img/intro4.jpg', 'img/p3.jpg', 'img/p1.jpg', 'img/p4.jpg', 'img/intro5.jpg', 'index.html', 'about.html', 'connect.html']

```

匹配原则：

| 模式  | 描述                                         |
| ----- | -------------------------------------------- |
| \w    | 匹配字母、数字以及下划线                     |
| \s    | 匹配任意空白字符，等价于[\t\n\r\f]           |
| \d    | 匹配任意数字，等价于[0-9]                    |
| ^     | 匹配一行字符串的开头                         |
| $     | 匹配一行字符串的结尾                         |
| .     | 匹配任意字符，**除了换行符**                 |
| *     | 匹配0或多个表达式                            |
| +     | 匹配1或多个表达式                            |
| ?     | 匹配0或1个正则表达式片段，非贪婪方式         |
| {n,m} | 匹配n至m次前面定义的正则表达式片段，贪婪方式 |
| a\|b  | 匹配a或b                                     |
| ( )   | 匹配括号内的表达式，也表示一个组             |

常用的方法：

1. **re.match**

   在match()方法中，第一个参数传入正则表达式，第二个参数传入要匹配的字符串

   ```python
   import re
   content = "Hello 123 321"
   result = re.match("^Hello\s\d{3}.*",content)
   print(result)
   print(result.group())
   # 输出SRE_Match对象表示匹配成功
   >>><_sre.SRE_Match object; span=(0, 13), match='Hello 123 321'>
   >>>Hello 123 321
   
   ```

   **匹配目标**：

   通过match()可以得到匹配的字符串，但是要从字符串中提取一部分内容应该怎么办？此时可以将要匹配部分的正则表达式用()括起来表示一个分组，然后用group(1)获取匹配结果。假如后面还有（）的部分，则用group(2), group(3)等取出结果。

   ```python
   result = re.match("^Hello\s\(d{3}).*",content)
   print(result.group(1))
   >>>123
   
   ```

   **通用匹配**：

   若出现空白字符就写\s，出现数字就写\d，那么工作量会很大。可以使用通用匹配

   .（点）表示任意字符，*（星）表示匹配前面的字符无限次。

   所以用.*结合起来就可以匹配任意字符串了。

   **贪婪与非贪婪**:

   - 在贪婪匹配下，.*会匹配尽可能多的字符
   - 在非贪婪匹配下，会匹配最少的字符，做法是在.*后面加？(即.*?)

   ```python
   import re
   content = "Hello 123327 Demo"
   result = re.match("^Hel.*(\d+).*Demo$",content)    # 贪婪匹配
   print(result.group(1))
   result2 = re.match("^Hel.*?(\d+).*Demo$",content)   # 非贪婪匹配
   print(result2.group(1))
   >>>7   
   >>>123327
   
   ```

   **修饰符**：

   正则表达式可以包含一些可选的标志修饰符来控制匹配的模式。例如

   ```python
   import re
   content = """Hello 123327 Demo
   is a Regex Demo
   """
   result = re.match("^Hel.*?(\d+).*?Demo$",content)
   print(result)
   >>>None
   # 这里输出None表示匹配失败了，原因是.不能匹配换行符
   # 解决方法可以在match函数中给出修饰符re.S
   # match函数原型：re.match(pattern, string, flags=0)
   result = re.match("^Hel.*?(\d+).*?Demo$",content,re.S)
   print(result)
   print(result.group(1))
   >>><_sre.SRE_Match object; span=(0, 33), match='Hello 123327 Demo\nis a Regex Demo'>
   >>>123327
   
   ```

2. **re.findall**

   

3. re.search

4. re.sub

5. re.compile

match和search的区别？

#### BeautifulSoup

以先序顺序遍历文档树

BeautifulSoup将HTML文档转换成树状结构，每个节点都是Python对象，所有对象可分为四种：

1. Tag：tag可能有多个属性，操作方式类似于字典  可通过a.attrs返回它的一个字典
2. NavigableString：表示字面上的文本值，同时这个字符串是可导航的（定位）
3. BeautifulSoup：包含了文档的全部内容，存在一个虚拟的根节点[document]
4. Comment：注释信息

BeautifulSoup提供了很多搜索方法：find() 和 find_all()

介绍性：

《Hacker & Painter》

Hacker News 流行的技术性网站

#### 文件读写

```python
# 使用handle进行文件读写
handle = open('xxx.txt','r')
# 再使用handle进行操作

```

#### 数据格式

##### CSV格式

逗号分割。  python内置CSV格式的支持

```python
import csv
# 1,写csv数据
students = [
    ['name','gender','age'],
    ['zhangsan','male',14],
    ['lisi','femal',15]
]
# 输出的结果中间有换行，因为win下换行符是\r\n，而Linux下是\n
# 解决方式是添加newline参数
fd = open('student.csv','w',newline="")
# fd = open('student.csv','w')
writer = csv.writer(fd)   
writer.writerows(students)
fd.close

# 2,读csv文件
handle = open('student.csv','r')
reader = csv.reader(handle)
rows = list(reader)

print(rows)

```

##### JSON格式

设计周期仅为十天。信噪比 较xml格式更高。python内置对json的支持。

将json格式转化成字典

#### 数据持久化

sqlite3 是python内置的数据库，不需要安装其他东西。可以看成一个库或者一个可执行程序

有两种模式：

1. 正常模式：   便于进行持久化，数据存放在硬盘中
2. 内存模式：   special name :memory:     便于计算

Sqlite3默认支持的类型：NULL, INTEGER, REAL, TEXT, BLOB

```python
# 写入
import sqlite3

conn = sqlite3.connect("example.db")    # 会创建这个文件(注意，当前目录是要有可写权限的)
# 或者内存模式
# conn_memory = sqlite3.connect(":memory:")

# conn还可以读入SQL脚本 - 多条语句之间是要有分号的

# 获得连接后可执行语句，官方建议使用游标进行操作
c = conn.cursor()
# create table
c.execute('''CREATE TABLE students(
    id int primary key not null unique, name text, gender text, age int
)''')
# insert a row
c.execute('''INSERT INTO students VALUES(
    111,'zhangsan','male',21
)''')
# save - 保证写入进文件
conn.commit()
# 关闭连接
conn.close()

```

```python
# 查询
import sqlite3

conn = sqlite3.connect("example.db")

#conn.execute('''INSERT INTO students VALUES(
#    112,'lisi','female',20
#)''')
#conn.commit()

cursor = conn.execute("select * from students")
#result = list(cursor)
for row in cursor:
    print(row)

#print(result)

# 多行数据插入
data = [
    ("121","aaaa","male",23),
    ("122","bbbb","female",24)
]
conn.executemany("insert into student values (?,?,?,?)",data)
conn.commit()

```

防止SQL注入的要点：不要使用直接的字符串拼接

```python
# Never do this
name = "zhangsan"
c.execute("select name from students where name = '%s'" % name)

# Do this instead
t = ('zhangsan',)     # 使用一个元组来作为占位符
c.execute("select name from students where name = ?",t)
print(c.fetchone())   # 游标操作： 返回下一个

```

指定按行遍历： conn.row_factory = sqlite3.Row

这样，conn.fetchone()就会返回一个Row对象，而不是一个Cursor



## 文本编辑工具

vscode, vim, nano, pico , emacs等

![1575940494295](https://github.com/AlieZ22/Learn-Python/blob/master/res/1575940494295.png)

**vim上手门槛比较高，但可以成为自己有意识培养的优势**，因为别人不愿学而自己能够花心思去学懂

vi by Bill Joy

vim by Bram Moolenaar   (Git的默认版本)

neovim by Thiago de Arruda

**高效编辑**

1. 基于模式
2. 高效移动
3. 命令与文本对象的组合
4. （待补充



## 版本控制工具

git，是一个自嘲的名字，意思大概是“混账”

详见Learn-Git仓库

## 单元测试

python内置单元测试的库：unittest

```python
import unittest
from triangle import triangle

# 这个判断三角形的函数是错误的，两边之和大于第三边没有完全判断
def triangle(a, b, c):
    if a + b < c:
        return "错误"
    if a == b == c:
        return "等边"
    elif a == b or b == c or c == a:
        return "等腰"
    else:
        return "一般"

class TestTriangle(unittest.TestCase):
    def test_1(self):
        # 格式：assertEqual(函数调用，预期结果)
        self.assertEqual(triangle(-1, -1, -1), "错误")
    def test_2(self):
        self.assertEqual(triangle(3, 3, 3), "等边")
    def test_3(self):
        self.assertEqual(triangle(3, 4, 5), "一般")
    def test_4(self):
        self.assertEqual(triangle(3, 4, 1), "错误")

if __name__ == "__main__":
    unittest.main()

```

unittest.main()运行过程：在main作用域中找unittest的子类的以test开头的方法，并运行这些方法。

运行结果：

![1576543145091](https://github.com/AlieZ22/Learn-Python/blob/master/res/1576543145091.png)

有多种运行的方法：

1. 命令行中python3 <filename>
2. 命令行中python3 -m unittest discover

思考：在类中找到方法是如何实现的？    -- hasattr



coverage文档    覆盖测试



## Mongodb

启动mongodb ,   mongod.exe --dbpath <dir>

插入为Bson对象即可，不需要考虑关系型数据库的范式

Bson是Json数据的二进制表示形式

详见官方doc：https://docs.mongodb.com/manual/tutorial/getting-started/

使用anaconda安装pymongo： conda install pymongo

mongo-python  API: https://api.mongodb.com/python/current/#overview



### Create a Database

如果数据库不存在，那么在你往那个数据库中插入数据时，MongoDB就会自动创建那个数据库

```
use myNewDB
db.myNewCollection1.insertOne({x:1})
```

### Collections

MongoDB不用严格的表来存储，而是用collection(集合)来存储数据。这个集合类似于关系数据库中的表。

#### create a collection

如果collection不存在，那么同样，在第一次向collection中插入数据时将会创建它

```
db.myNewCollection2.insertOne({y:2})
db.myNewCollection3.insertOne({z:3})
```

#### explicit create

MongoDB提供以下方法来显式地创建一个collection，可以指定最大的容量或者文件验证规则（documentation validation rules），但是如果你不需要特殊地配置，你不需要显式地去创建。

```
db.createCollection()
```





## Web开发

使用轻量级的工具：flask

an easy example

```python
from flask import Flask
app = Flask(__name__)

@app.route('/')      # 修饰器
def hello_world():
    return 'Hello, world !'
```

![1576739153228](https://github.com/AlieZ22/Learn-Python/blob/master/res/1576543145091.png/1576739153228.png)