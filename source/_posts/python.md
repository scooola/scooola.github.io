---
title: python学习笔记
date: 2019-10-21 03:11:46
tags: python学习笔记
---

# Python学习

Python是一种跨平台的计算机程序设计语言。是一种面向对象的动态类型语言，最初被设计用于编写自动化脚本(shell)，随着版本的不断更新和语言新功能的添加，越来越多被用于独立的、大型项目的开发。 

本文主要记录了在学习python过程中所遇到的一些小知识点。

<!-- more -->
<!-- toc -->

## 1.`__str__` 和`__repr__`区别

```python
# __str__ 和 __repr__ 区别
```

1. `__str__`用于为最终用户创建输出，而 `__repr__` 主要用于调试和开发。 `__repr__` 的目标是明确无误，`__str__`是可读的
2. `__repr__` [用于推断对象的"官方"字符串表示形式](https://link.zhihu.com/?target=https%3A//docs.python.org/3/reference/datamodel.html%23object.__repr__)（包含有关对象的所有信息的表示）, `__str__` [用于推断对象的“非正式”字符串表示形式](https://link.zhihu.com/?target=https%3A//docs.python.org/3/reference/datamodel.html%23object.__str__)（对打印对象有用的表示形式）

再通过一个例子说明问题:

```python
import datetime
today = datetime.datetime.now()

print str(today)
print repr(today)
```

输出结果如下:

```python
>>> print(str(today))
2018-08-22 16:52:37.403320
>>> print(repr(today))
datetime.datetime(2018, 8, 22, 16, 52, 37, 403320)
```

**当仅定义 `__repr__` 的时候， `__repr__` == `__str__`, 但是仅定义 `__str__` 的情况下，两者不相等。当两者均定义的时候，会各自使用自己的方法。**

[上述详解](https://zhuanlan.zhihu.com/p/42730827)

## 2.`__init__`和`__new__`关系

python中有二个特殊的方法`__new__` 和 `__init__` 方法。`__init__` 方法为初始化方法, `__new__`方法才是真正的构造函数。

```python
class Person(object):
    def __new__(cls, *args, **kwargs):
        print("in __new__")
        instance = object.__new__(cls, *args, **kwargs)
        return instance

    def __init__(self, name, age):
        print("in __init__")
        self._name = name
        self._age = age

p = Person("Sheng", 22)
```

输出结果：

```python
in __new__
in __init__
```

上面的代码中实例化了一个Person对象，可以看到`__new__`和`__init__`都被调用了。`__new__`方法用于创建对象并返回对象，当返回对象时会自动调用`__init__`方法进行初始化。`__new__`方法是静态方法，而`__init__`是实例方法。

## 3.python字典赋值新姿势

平时字典的赋值是:test_dict = {"Name":"Tom"}

下面的方法赋值时在后面添加了for语句，可提高字典的赋值效率，且代码更加简洁。

```python
#!/usr/bin/python
# coding:utf8

person_tom = {"name":"Tom","age":18}
person_jack = {"name":"Jack","age":20}
person_list = (person_tom,person_jack)
new_dict = {indicators["name"]: indicators["age"]
                          for indicators in person_list}
print(new_dict)
```

输出结果：

```python
{'Jack': 20, 'Tom': 18}
```

其次还有利用dict()快捷创建字典

```python
>>>dict()                        # 创建空字典
{}
>>> dict(a='a', b='b', t='t')     # 传入关键字
{'a': 'a', 'b': 'b', 't': 't'}
>>> dict(zip(['one', 'two', 'three'], [1, 2, 3]))   # 映射函数方式来构造字典
{'three': 3, 'two': 2, 'one': 1} 
>>> dict([('one', 1), ('two', 2), ('three', 3)])    # 可迭代对象方式来构造字典
{'three': 3, 'two': 2, 'one': 1}
```

## 4.格式化字符时多使用format函数

```python
# bad
name = "tony"
age = 100
str = "myname : " + name + " my age : " + str(age)
str1 = "myname : %s my age : %d" % (name, age)
# good
str2 = "myname : {} my age {}".format(name, age)
```

示例1：

```python
>>>"{} {}".format("hello", "world")    # 不设置指定位置，按默认顺序
'hello world'
 
>>> "{0} {1}".format("hello", "world")  # 设置指定位置
'hello world'
 
>>> "{1} {0} {1}".format("hello", "world")  # 设置指定位置
'world hello world'
```

示例2：

```python
#!/usr/bin/python
# -*- coding: UTF-8 -*-
 
print("网站名：{name}, 地址 {url}".format(name="百度", url="www.baidu.com"))
 
# 通过字典设置参数
site = {"name": "百度", "url": "www.baidu.com"}
print("网站名：{name}, 地址 {url}".format(**site))
 
# 通过列表索引设置参数
my_list = ['百度', 'www.baidu.com']
print("网站名：{0[0]}, 地址 {0[1]}".format(my_list))  # "0" 是必须的
```

## 5.使用enumerate代替for循环中的index变量访问

```python
# bad
my_container = ['lily', 'lucy', 'tom']
index = 0
for element in my_container:
    print '{} {}'.format(index, element)
    index += 1

# good
for index, element in enumerate(my_container):
    print '%d %s' % (index, element)
```

## 6.避免使用可变(mutable)变量作为函数参数的默认初始化值

```python
# bad
def function(l = []):
 l.append(1)
 return l

 print function()
 print function()
 print function()

 # print
 [1]
 [1, 1]
 [1, 1, 1]

 # good 使用None作为可变对象占位符
 def function(l=None):
     if l is None:
         l = []
     l.append(1)
     return l
```

## 7.is 和 == 的区别

**is比较的是两个对象的id值是否相等，也就是比较两个对象是否为同一个实例对象，是否指向同一个内存地址。**

**== 比较的是两个对象的内容是否相等，默认会调用对象的`__eq__()`方法。**

### 哪些情况下is和==结果是完全相同的

```python
>>> a = 256
>>> b = 256
>>> a is b
True
>>> a == b
True
>>>
>>> a = 1000
>>> b = 10**3
>>> a == b
True
>>> a is b
False
```

**为什么256时相同， 而1000时不同？**

因为出于对性能的考虑，Python内部做了很多的优化工作，对于整数对象，Python把一些频繁使用的整数对象缓存起来，保存到一个叫small_ints的链表中，在Python的整个生命周期内，任何需要引用这些整数对象的地方，都不再重新创建新的对象，而是直接引用缓存中的对象。Python把这些可能频繁使用的整数对象规定在范围[-5, 256]之间的小对象放在small_ints中，但凡是需要用些小整数时，就从这里面取，不再去临时创建新的对象。

```python
>>> a1 = 'Hi'
>>> b1 = 'Hi'
>>> a1 is b1
True
>>> a1 == b1
True
>>> a2 = 'I am using long string for test'
>>> b2 = 'I am using long string for test'
>>> a2 is b2
False
>>> a2 == b2
True
```

**为什么a1与b1相同，而a2与b2不同？**

这是python中的**string interning（字符串驻留）**机制所决定的：对于较小的字符串，为了提高系统性能会保留其值的一个副本，当创建新的字符串的时候直接指向该副本即可。

### 结论

当变量是数字、字符串、元组，列表，字典时，is和==都不相同， 不能互换使用！当比较值时，要使用==，比较是否是同一个内存地址时应该使用is。当然，开发中比较值的情况比较多。

## 8.了解bytes、str与unicode的区别

在`Python3`中，有两种类型的字符代表序列：`bytes(字节)`和 `str（字符串）`。字节的实例包含8个原生的比特值，而字符串的实例则是用Unicode字符来堆砌的。

在`Python2`中，有两种类型的字符代表序列：`str(字符串)` 和 `unicode(Unicode字符)`。与`Python3`相反，字符串实例代表着原生的8比特值序列，而`unicode`则由`Unicode`字符堆砌而成。

要想将`Unicode`字符转换成二进制数据，就必须使用`encode`方法，反过来，要想把二进制数据转换成`Unicode`字符，就必须使用`decode`方法。

**两大陷阱**

在`Python`中处理原生的8比特值 序列以及`Unicode`字符的时候，有两大陷阱。

一个是在`Python2`中，当一个`str`数据仅仅包含7比特的`ASCII`码字符的时候，`unicode`和`str`实例看起来是一致的。

- 可以使用‘+’运算符和合并str和unicode。
- 可以使用等价或者不等价运算符来比较str和unicode实例。
- 可以使用unicode来代换 像‘%s’这种字符串中的格式化占位符。

以上行为意味着，如果你的代码中仅仅处理原生的7比特序列，那么便可以不必在意是`str`类型的数据还是`unicode`类型的数据了。在`Python3`中，`bytes`和`str`实例是不可能等价的，即使是空的字符串也不可能等价。所以你必须谨慎地对正在处理的代码进行字符类别的区分处理。

另一个是在`Python3`中，涉及到文件处理的操作（使用内置的`open`函数）会默认的以`UTF-8`进行编码。而在`Python2`中默认采用二进制形式来编码。这也是导致意外事故发生的根源，特别是对于那些更习惯于使用`Python2`的程序员而言。

比方说，你想将几个随机的二进制数据写入到一个文件中。在`Python2`中，下面的这段代码可以正常的工作，但是在`Python3`中却会报错并退出。详细信息如下。

```python
def open('/tmp/random.bin','w') as f:
    # os.urandom() 随即产生n个字节的字符串，可以作为随机加密key使用
    f.write(os.urandom(10))

>>>
TypeError: must be str,not bytes
```

导致这个异常发生的原因是在`Python3`中对于`open`函数又新增了一个名为`encoding`的参数。此参数默认为`UTF-8`。这使得其对于文件的读写操作预期的源为包含了`Unicode`字符串的`str`实例，而不是包含了二进制数据的字节文件。

为了使得上面的函数正常的工作，我们必须指明被操作的数据是以‘wb’模式打开，而不是简单的‘w’模式。这里，作者介绍了一个在`Python2`和`Python3` 中都通用的方法，详细如下。

```python
with open('/tmp/random.bin','wb) as f:
    f.write(os.urandom(10))
```

好了，写文件的问题算是解决了，但是不要忘了还有读文件的问题哦。同样的我们也只需要改变一下读文件的模式即可。即‘r’换成‘rb’。

## 9.python 星号

在Python中，调用函数时，利用`*`语句可以将参数列表解包，如下所示，`foo`有两个参数，我们可以传入一个参数列表`num_list`，然后利用`*`运算符，解包参数列表：

```python
In [1]: def foo(bar,lee):
   ...:     print bar, lee
   ...:

In [2]: num_list = [1, 2]

In [3]: foo(*num_list)
1 2
```

**序列解包**

在**Python3.0**之后，Python提供了一种非常方便的办法解压可迭代对象赋值给多个变量。

另外一种情况，假设你现在有一些用户的记录列表，每条记录包含一个名字、邮件，接着就是不确定数量的电话号码。 你可以像下面这样分解这些记录：

```python
>>> record = ('Dave', 'dave@example.com', '773-555-1212', '847-555-1212')
>>> name, email, *phone_numbers = record
>>> name
'Dave'
>>> email
'dave@example.com'
>>> phone_numbers
['773-555-1212', '847-555-1212']
```

值得注意的是上面解压出的 `phone_numbers` 变量永远都是列表类型，不管解压的电话号码数量是多少（包括 0 个）。 所以，任何使用到 `phone_numbers` 变量的代码就不需要做多余的类型检查去确认它是否是列表类型了。

星号表达式也能用在列表的开始部分。比如，你有一个公司前 8 个月销售数据的序列， 但是你想看下最近一个月数据和前面 7 个月的平均值的对比。你可以这样做：

```python
*trailing_qtrs, current_qtr = sales_record
trailing_avg = sum(trailing_qtrs) / len(trailing_qtrs)
return avg_comparison(trailing_avg, current_qtr)
```

下面是在 Python 解释器中执行的结果：

```python
>>> *trailing, current = [10, 8, 7, 1, 9, 5, 10, 3]
>>> trailing
[10, 8, 7, 1, 9, 5, 10]
>>> current
3
```

## Keyword-Only Arguments

在Python3中，也加入了一个新的语法（参见 [PEP 3102](https://www.python.org/dev/peps/pep-3102/)）

```python
def func(arg1, arg2, arg3, *, kwarg1, kwarg2):
    pass
```

这个函数只接受3个位置参数，`*`之后的所有内容只能作为关键字参数传递(只能由关键字提供的参数，并且永远不会被位置参数自动填充)。

[更加详细相关内容请戳](https://sikasjc.github.io/2018/10/12/star/)

## 10.描述器

Python 有三个特殊方法，`__get__`、`__set__`、`__delete__`，用于覆盖属性的一些默认行为，如果一个类定义了其中一个方法，那么它的实例就是描述器

描述器是一种代理机制，对属性的操作由这个描述器来代理

访问: `__get__(self, instance, cls)` # instance 代表实例本身，cls 表示类本身，使用类直接访问时，instance 为 None

赋值: `__set__(self, instance, value)` # instance 为实例，value 为值

删除: `__delete__(self, instance)` # instance 为实例

[更加详细相关内容请戳](https://anyisalin.github.io/2017/03/08/python-descriptor/)

## 11.Unicode码转中文输出

```python
unicode_str = "\u4e2d\u56fd"
print unicode_str.decode("unicode-escape")

>>>
中国
```

## 12.将python对象保存到.bin文件

[详细点击](http://skyhigh233.com/blog/2016/11/18/effective-python-4/)

**Effective Python 第44条**

```python
# -*-coding:utf8-*-
# 该代码为自己写的测试

# 将GameState实例化对象通过save()保存到game_data.bin
# 通过read()将game_data.bin内保存的实例化对象还原

# copyreg.pickle(GameState, pickle_game_state)的目的：
# 实例化对象保存到game_data.bin后，GameState加入新字段
# GameState加了新字段后，读取会读的到新字段，且新字段值为默认值

import pickle
import copyreg


class GameState(object):
    def __init__(self, level=0, lives=3, exp=0):
        self.level = level
        self.lives = lives
        self.exp = exp


path = "./game_data.bin"


def save():
    state = GameState()
    with open(path, 'wb') as f:
        pickle.dump(state, f)


def read():
    with open(path, 'rb') as f:
        state_after = pickle.load(f)
    print state_after.__dict__


def pickle_game_state(game_state):
    kwargs = game_state.__dict__
    return unpickle_game_state, (kwargs,)


def unpickle_game_state(kwargs):
    return GameState(**kwargs)


if __name__ == "__main__":
    # copyreg模块注册pickle_game_state
    # 目的是：GameState加了新字段后，读取会读的到新字段，且新字段值为默认值
    copyreg.pickle(GameState, pickle_game_state)
    # save()
    read()
```

## 13.python2和python3同时安装导致python3的pip报错解决

原来只安装了python2.7。后来因为要用到python3，为了让两者共存，降python3的运行文件改成了python3.exe. 问题就此而来，这时候运行python3 的pip会遇到如下错误

```cmd
Fatal error in launcher: Unable to create process using 
```

但是运行pip2是好的。如果这时候讲python2.7的运行文件改成Python2.exe，你就会发现pip2 也抛出了通用的错误。由此可以断定这个错误是因为改动了python的执行文件的名字造成的。

网上看了很多方案都说直接运行 python3 -m pip install --upgrade pip 来升级pip版本就好了，这个在大多数情况下都是有用的。因为重装以后会根据更改后的python的执行文件来创建关联。 但是如果你的pip已经是最新版本的话就行不通了，因为已经是最新的版本根本就不让你升级。那么就用下面的命令来强制重装 pip

```shell
python3  -m pip install --upgrade --force-reinstall pip
```

至此就解决了pip的错误。

## 14.virtualenv 分别创建Python2和Python3的虚拟空间

virtualenv是python开发中经常要用到的一个工具，如果在开发多个程序时候，A程序需要用python2.7，B程序需要python3时候，就可以利用virtualenv来创建一套独立的环境

接下来，在自己想要创建的目录中创建一个独立的python环境，

如果要创建**python2**的环境的话，命令为

```cmd
virtualenv -p D:\Python\python.exe env2.7
```

创建**python3**的环境命令如下：

```python
virtualenv -p D:\python37\python3.exe env3.7
```

以上路径需要替换为自己的python2和python3的安装路径。

如果要创建一个不带已经安装到系统的中第三方包的环境，可以加上参数 --no-site-packages，这样就可以得到一个不带任何第三方包的干净的python运行环境，命令为:

```cmd
virtualenv --no-site-packages myenv
```

**使用**：

**进入虚拟环境**：

在你的虚拟环境目录下的Scripts内`your_env_dir\Scripts`，输入**activate**即可在虚拟环境内开发。

```cmd
D:\pyProject\test\test_virtualenv\env3.7\Scripts>activate

(env3.7) D:\pyProject\test\test_virtualenv\env3.7\Scripts>
```

**退出虚拟环境**：

输入**deactivate**即可退出虚拟环境。

```cmd
(env3.7) D:\pyProject\test\test_virtualenv\env3.7\Scripts>deactivate
D:\pyProject\test\test_virtualenv\env3.7\Scripts>
```

**pycharm使用虚拟环境**：

```
File --> Settings --> Project:your project name --> Project Interpreter --> 点击设置图标 --> Add --> System Interpreter --> ...图标 --> 选择虚拟环境目录\Scripts --> 选择python.exe
```

这样pycharm运行就会使用虚拟环境的python解释器以及虚拟环境内的第三方包。

**注意**：

使用虚拟环境时，有个地方需要注意，那就是别移动环境目录，因为所有的路径（包括python3命令所指向的路径），都以硬编码的形式写在了安装目录之中，如果移动了，那么环境自然就会失效。为此我们可以将旧环境的依赖关系导出，然后创建新的环境。

导出环境：

```cmd
pip freeze > requirements.txt
```

恢复环境：

```cmd
# 在requirements.txt所在目录下运行
pip install -r requirements.txt
```
## 15.windows pip安装第三方包报错

报错如下：
```cmd

```
输入如下命令解决：
pip install --user package_name

