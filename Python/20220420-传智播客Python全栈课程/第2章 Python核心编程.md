[TOC]

# 第1节 Python核心编程

开始时间：20220421

结束时间：

## 01- Python高级一

### 1.1 import导入模块

import搜索模块的导入路径

```python
import sys
>>> sys.path
```

![image-20220420104944769](https://raw.githubusercontent.com/TheBeginnerMeng/notes_backup/main/imgs/image-20220420104944769.png?token=AHAKQ3VLQHSUX27NUHF3T5TCMS4J6)

如果想要手动添加路径，可以用`sys.path.append("xxx/path")`

### 1.2 重新导入模块

模块被导入后，使用`import xxx`不能重新导入模块，正确方式：

```python
import importlib
importlib.reload(xxx)  # xxx is the module name
```

### 1.3 循环导入

两个模块分别导入对方，导致死循环导入。

防止此情况：程序设计上分层，降低耦合

### 1.4 == 和 is

`is` 是比较两个引用是否指向了同一个对象

`==` 是比较两个对象是否相等

> 在赋值整型时，Python内部作了优化处理，在`[-5, 256]`之间的整数的内存地址是固定的，并不会因为赋值给变量就开辟一块新的内存空间。

### 1.5 深拷贝、浅拷贝

**浅拷贝**：拷贝引用

**深拷贝**：复制了内容，并重新开辟了一块内存空间

深拷贝用法：

```python
import copy
xx = copy.deepcopy(xx)
```

扩展：

```python
a = [11, 22, 33]
b = [44, 55, 66]
c = [a, b]

d = c  # 浅拷贝
e = copy.deepcopy(c)  # 深拷贝，此时e中的两个列表也是新生成的，并未浅拷贝a和b

f = copy.copy(c)  # 此时列表f是新生成的（id(f)和id(c)不同），但是f中的两个列表是浅拷贝的a和b
```

使用`copy.copy`方法时，会根据当前拷贝的数据类型有不同的处理方式，如果是可变数据类型（列表，字典），则进行深拷贝，如果是不可变数据类型，则进行浅拷贝（仅复制引用），当使用深拷贝方法`copy.deepcopy`时，如果是不可变数据类型，则也只能进行浅拷贝。

### 1.6 进制、位运算

#### 1.6.1 进制

进制：十进制（Decimal）、八进制（Octal）、十六进制（Hexadecimal）

#### 1.6.2 有符号数和无符号数

把二进制数中的最高位（最左边的那位）用作符号位

>     对于有符号数，最高位被计算机系统规定为符号位（0为正，1为负）
>
>     对于无符号数，最高位被计算机系统规定为数据位

于是，有符号数+2和-2的原码形式为：

```
+2 = 0000 0000 0000 0010
-2 = 1000 0000 0000 0010
真值      机器数
```

> 数字在计算机中，都是按照补码的形式存储的，因此`-1 + 1`需要按照各自的补码进行相加计算

#### 1.6.3 原码、反码、补码

计算补码的方法：

```
规则:
正数：原码 = 反码 = 补码
负数：反码 = 符号位不变，其他位取反
     补码 = 反码+1
     
+1的原码：0000 0000 0000 0001
-1的原码：1000 0000 0000 0001
-1的反码：1111 1111 1111 1110
-1的补码：1111 1111 1111 1111

计算-1 + 1的结果
1111 1111 1111 1111
0000 0000 0000 0001
---------------------------
0000 0000 0000 0000
```

从补码转回原码：

```
原码 = 补码的符号位不变 -->数据位取反--> 尾+1
-1的补码:1111 1111 1111 1111
    取反:1000 0000 0000 0000
-1的原码:1000 0000 0000 0001
```

拓展：

为何要使用原码？

> 反码和补码，既然原码才是被人脑直接识别并用于计算表示方式，为何还会有反码和补码呢？
>
> 首先，因为人脑可以知道第一位是符号位，在计算的时候我们会根据符号位，选择对应加减，但是对于计算机，加减乘数已经是最基础的运算，要设计的尽量简单。计算机辨别"符号位"显然会让计算机的基础电路设计变得十分复杂！
>
> 于是人们想出了将符号位也参与运算的方法。我们知道，根据运算法则减去一个正数等于加上一个负数，即：`1-1 = 1 + (-1) = 0` , 所以机器可以只有加法而没有减法，这样计算机运算的设计就更简单了。
>
> 最终人们开始探索将符号位参与运算，并且只保留加法的方法。

#### 1.6.4 进制间的转换

![image-20220421104409500](https://raw.githubusercontent.com/TheBeginnerMeng/notes_backup/main/imgs/image-20220421104409500.png?token=AHAKQ3W3YQCKERQE5I667DTCMS4MA)

#### 1.6.5 位运算

位运算的操作符：

- & 按位与
- | 按位或
- ^ 按位异或
- ~ 按位取反
- << 按位左移
- \>> 按位右移

位运算的优点：直接操作二进制，省内存，效率高

1） 按位左移 <<

- **左移1位相当于乘以2**
- 用途：快速计算一个数乘以2的n次方 (8<<3 等同于8*2^3)

2）按位右移 >>

- **右移1位相当于除以2**
- x 右移 n 位就相当于除以2的n次方 用途：快速计算一个数除以2的n次方 (8>>3 等同于8/2^3)

3）按位与 &

>  全1才1，否则0：只有对应的两个二进位均为1时，结果位才为1，否则为0

4）按位或 |

> 有1就1：只要对应的二个二进位有一个为1时，结果位就为1，否则为0

5）按位异或  ^

> 不同为1：当对应的二进位相异（不相同）时，结果为1，否则为0

6）取反 ~

~9 = -10

【为什么9取反变成了-10的说明】：

9的原码 ==> 0000 1001 因为正数的原码=反码=补码，所以在真正存储的时候就是0000 1001

接下来进行对9的补码进行取反操作

进行取反==> 1111 0110 这就是对9 进行了取反之后的补码

既然已经知道了补码，那么接下来只要转换为咱们人能识别的码型就可以，因此按照规则 ，把这个1111 0110 这个补码 转换为原码即可

符号位不变，其它位取反==> 1000 1001

然后+1 ，得到原码 ====>1000 1010 这就是 -10

扩展：

>  任何数和1进行&操作，得到这个数二进制形式的最低位

### 1.7 私有化

- xx：公有变量
- _x：单前置下划线，私有化属性或方法，`from somemodule import *`禁止导入，类对象和子类可以访问
- __xx：双前置下划线，避免与子类中的属性命名冲突，无法在外部直接访问（名字重整所以访问不到）
- \_\_xx\_\_：双前后下划线，用户名字空间的魔法对象或属性。例如：`__init__` ，不要自己发明这样的名字
- xx_：单后置下划线，用于避免与Python关键词的冲突

通过name mangling（名字重整，目的就是以防子类意外重写基类的方法或者属性）如：`_Class__object`）机制就可以访问private了。

总结

- 父类中属性名为`__`名字的，子类不继承，也不能访问
- 如果在子类中向`__`名字赋值，那么会在子类中定义一个与父类相同名字的属性（实质还是没有访问到父类的同名属性）
- `_`名的变量、函数、类在使用`from xxx import *` 时不会被导入

### 1.8 属性property

#### 1.8.1 私有属性添加getter和setter方法

```python
class Money(object):
    def __init(self):
        self.__money = 0
        
    def getMoney(self):
        return self.__money
    
    def setMoney(self, value):
        if isinstance(value, int):
            self.__money = value
        else:
            print("error: value is not a integer")
```

#### 1.8.2 使用property升级getter和setter方法

```python
class Money(object):
    def __init(self):
        self.__money = 0
        
    def getMoney(self):
        return self.__money
    
    def setMoney(self, value):
        if isinstance(value, int):
            self.__money = value
        else:
            print("error: value is not a integer")
    
    money = property(getMoney, setMoney)
```

运行结果：

```python
In [1]: from get_set import Money

In [2]: a = Money()

In [3]: a.money
Out[3]: 0

In [4]: a.money = 100

In [5]: a.money  # money当做实例属性使用
Out[5]: 100

In [6]: a.getMoney()
Out[6]: 100
```

property相当于把类的setter和getter方法进行了封装，开发者对属性设置数据的时候更方便。

#### 1.8.3 使用property取代getter和setter方法

`@property`成为属性函数，可以对属性赋值时做必要的检查，并保证代码的清晰短小，主要有2个作用：

- 将方法转换为只读
- 重新实现一个属性的设置和读取方法，可以做边界判定

```python
class Money(object):
    def __init__(self):
        self.__money = 0

    @property
    def money(self):
        return self.__money

    @money.setter
    def money(self, value):
        if isinstance(value, int):
            self.__money = value
        else:
            print("error:不是整型数字")
```

## 02-Python高级二

### 2.1 迭代器

迭代是访问集合元素的一种方式。迭代器是一个可以记住遍历位置的对象。迭代器对象从集合的第一个元素开始访问，直到所有的元素被访问完结束。迭代器只能往前不能后退。

#### 2.1.1 可迭代对象

可以直接作用于for循环遍历的数据类型。有以下几种：

1）集合类型：list、tuple、dict、set、str

2）generator，包括生成器和带yield的generator function

以上这些可以直接作用于for循环的对象统称为可迭代对象：Iterable。

#### 2.1.2 判断是否可以迭代

使用`isinstance()`判断是否是一个Iterable对象

```python
In [50]: from collections import Iterable

In [51]: isinstance([], Iterable)
Out[51]: True

In [52]: isinstance({}, Iterable)
Out[52]: True

In [53]: isinstance('abc', Iterable)
Out[53]: True

# 生成器对象也是可迭代的
In [54]: isinstance((x for x in range(10)), Iterable)
Out[54]: True

In [55]: isinstance(100, Iterable)
Out[55]: False
```

生成器对象不仅可以作用于for循环，还可以被`next()`方法不断调用并返回下一个值，直到抛出StopIteration错误，则表示无法抛出下一个值。

#### 2.1.3 迭代器

可以被`next()`方法调用，并不断返回下一个值的对象，叫做**迭代器（Iterator）**。

也可以使用`isinstance()`方法来判断一个对象是否是迭代器对象。

```python
In [56]: from collections import Iterator

# 生成器对象也是一个迭代器
In [57]: isinstance((x for x in range(10)), Iterator)
Out[57]: True

In [58]: isinstance([], Iterator)
Out[58]: False

In [59]: isinstance({}, Iterator)
Out[59]: False

In [60]: isinstance('abc', Iterator)
Out[60]: False

In [61]: isinstance(100, Iterator)
Out[61]: False
```

#### 2.1.4 iter()方法

生成器都是`Iterator`对象，但诸如list、tuple、set、dict等数据类型，虽然是`Iterable`的，但是都不是`Iterator`对象

然而，可以通过`iter()`方法来将诸如list、tuple、set、dict等`Iterable`对象转换为`Iterator`对象。

```python
In [62]: isinstance(iter([]), Iterator)
Out[62]: True

In [63]: isinstance(iter('abc'), Iterator)
Out[63]: True
```

#### 总结

- 凡是可以作用于for循环的对象都是`Iterable`对象
- 凡是可以作用于next()方法的对象都是`Iterator`对象
- 集合数据类型（list、tuple、set、dict）等都是`Iterable`但不是`Iterator`对象，不过可以通过`iter()`方法来转换为一个`Iterator`对象

### 2.2 闭包

#### 2.2.1 函数引用

```python
def test1():
    print("--- in test1 func----")

#调用函数
test1()

#引用函数
ret = test1

print(id(ret))
print(id(test1))

#通过引用调用函数
ret()

# 运行结果：
--- in test1 func----
140212571149040
140212571149040
--- in test1 func----
```

#### 2.2.2 什么是闭包

```python
#定义一个函数
def test(number):

    #在函数内部再定义一个函数，并且这个函数用到了外边函数的变量，那么将这个函数以及用到的一些变量称之为闭包
    def test_in(number_in):
        print("in test_in 函数, number_in is %d"%number_in)
        return number+number_in
    #其实这里返回的就是闭包的结果
    return test_in


#给test函数赋值，这个20就是给参数number
ret = test(20)

#注意这里的100其实给参数number_in
print(ret(100))

#注意这里的200其实给参数number_in
print(ret(200))

# 运行结果
in test_in 函数, number_in is 100
120

in test_in 函数, number_in is 200
220
```

#### 2.2.3 闭包再理解

内部函数对外部函数变量的引用（非全局变量），则称内部函数为 **闭包**。

使用`nonlocal`关键字才能访问（修改）外部函数的局部变量

```python
def counter(start=0):
    def incr():
        nonlocal start
        start += 1
        return start
    return incr

c1 = counter(5)
print(c1())  # 6
print(c2())  # 7

c2 = counter(50)
print(c2())  # 51
print(c2())  # 52

print(c1())  # 8
print(c1())  # 9

print(c2())  # 53
print(c2())  # 54
```

#### 2.2.4 闭包的实际应用

```python
def line_conf(a, b):
    def line(x):
        return a*x + b
    return line

line1 = line_conf(1, 1)
line2 = line_conf(4, 5)
print(line1(5))
print(line2(5))
```

在上面的代码案例中，函数`line`和变量`a,b`共同构成闭包，我们通过`line_conf`的参数`a,b`来对对这两个变量进行赋值。如此，我们就确定了函数的最终形式（y=x+1和y=4x+5），只需要通过传入不同的a和b，就可以得到不同的直线函数。由此，闭包具有提高代码可复用性的作用。

如果没有闭包，每次创建直线函数的时候都需要同时传入参数a,b,x，使用闭包提升了代码的可移植性。

关于闭包的思考：

- 闭包优化了变量的使用，原来需要类对象完成的工作，闭包也可以实现
- 由于内部函数引用的外部函数的局部变量，则外部函数的局部变量没有及时释放，会消耗内存

### 2.3 装饰器

#### 2.3.1 定义

写代码要遵循开放封闭原则

- 开放：多扩展开放
- 封闭：已经实现的功能代码

装饰器代码示例：

```python
def w1(func):
    def inner():
        # 验证1
        # 验证2
        # 验证3
        func()
    return inner

@w1
def f1():
    print('f1')
```

Python解释器在执行上述代码块时，执行顺序：

1. def w1(func) —> 将函数w1加载到内存
2. @w1

`@w1`内部执行的操作：执行w1函数，并将装饰器下面的函数作为参数传入w1，即`@w1`等价于`w1(f1)`，内部会执行

```python
def w1(f1):
    def inner():
        # 验证1
        # 验证2
        # 验证3
        原来的f1()
    return inner
```

然后将执行完的w1函数的返回值赋值给f1，即：

```python
新f1 = def inner(): 
            #验证 1
            #验证 2
            #验证 3
            原来f1()
```

如此一来，以后业务部门再想调用执行f1函数时，就会执行新f1函数，从而在内部先进行验证，再执行原来的f1函数，然后再将原来f1函数的返回值返回给业务调用者。

#### 2.3.2 被装饰的函数不带参数

```python
from time import ctime, sleep


def time_func(func):
    def wrapped_func():
        print(f"{func.__name__} called at {ctime()}")
        func()
    return wrapped_func

@time_func
def foo():
    print("I am foo")

    
foo()
sleep(2)
foo()
```

上面的代码理解装饰器执行行为可以理解为：

```python
foo = time_func(foo)
#  foo先作为参数赋值给func后，foo再接收来自time_func返回的wrapped_func
foo()
#  调用foo()，等价于调用了wrapped_func()
#  内部函数wrapped_func被引用，所以外部函数的func变量未被释放
#  func里保存的是原foo函数对象
```

#### 2.3.3 被装饰的函数带有参数

```python
from time import ctime, sleep


def time_func(func):
    def wrapped_func(a, b):
        print(f"{func.__name__} called at {ctime()}")
        print(a, b)
        func(a, b)
    return wrapped_func


@time_func
def foo(a, b):
    print(a + b)
    

foo(3, 5)
sleep(2)
foo(2, 4)
```

#### 2.3.4  被装饰的函数带有不定长参数

```python
from time import ctime, sleep


def time_func(func):
    def wrapped_func(*args, **kwargs):
        print(f"{func.__name__} called at {ctime()}")
        func(*args, **kwargs)
    return wrapped_func


@time_func
def foo(a, b, c):
    print(a + b + c)
    
    
foo(3, 5, 7)
sleep(2)
foo(2, 4, 9)
```

#### 2.3.5 带有return的装饰器

```python
from time import ctime, sleep


def time_func(func):
    def wrapped_func():
        print(f"{func.__name__} called at {ctime()}")
        ret = func()
        return ret
    return wrapped_func


@time_func
def foo():
    print("I am foo")
    
    
@time_func
def get_info():
    return "hello world"


foo()
sleep(2)
foo()

print(get_info())
```

#### 2.3.6 通用装饰器

```python
def func(function_name):
    def func_in(*args, **kwargs):
        ret = function_name(*args, **kwargs)
        return ret
    return func_in
```

#### 2.3.7 自带参数的装饰器

```python
from time import ctime, sleep


def timefunc_arg(pre="hello"):
    def time_func(func):
        def wrapped_func():
            print(f"{func.__name__} called at {ctime()} {pre}")
            return func()
        return wrapped_func
    return time_func


@timefunc_arg("world")
def foo():
    print("I am foo")
    
    
@timefunc_arg("python")
def too():
    print("I am too")

    
foo()
sleep(2)
foo()

too()
sleep(2)
too()
```

#### 2.3.8 类装饰器

装饰器函数其实是这样的一个接口约束，它必须接收一个callable对象作为参数，然后返回一个callable对象。在Python中callable对象一般都是函数，但也有例外。只要某个对象重写了`__call__()`方法，那么这个对象就是callable的。

```python
class Test():
    def __call__(self):
        print("call me!")

        
t = Test()
t()  # call me
```

实现类装饰器

```python
class Test(object):
    def __init__(self, func):
        print("---初始化---")
        print(f"func name is {func.__name__}")
        self.__func = func
    
    def __call__(self):
        print("---装饰器中的功能---")
        self.__func()
        
# 说明
# 1. 当用Test作为装饰器来对test()函数进行装饰的时候，首先会创建一个Test的实例对象
# 并且会把test这个函数引用当做参数传递到__init__方法中
# 即在__init__方法中的__func变量指向了test函数体

# 2. test引用则相当于指向了Test创建出来的实例对象

# 3. 当在使用test()进行调用的时候，相当于让这个对象()，因此会调用这个对象的__call__方法

# 4. 为了能在__call__方法中去调用原来的test函数体，所以在__init__方法中需要有一个实例属性，用来# 用来保存到原来的test函数体的引用，所以就有了self.__func = func，从而在__call__方法中可以
# 调用原来的test函数体

@Test
def test():
    print("---test---")


test()
```

### 2.4 作用域

#### 2.4.1 什么是命名空间

它表示着一个标识符（identifier）的可见范围。

#### 2.4.2 globals、locals

打印全局变量和局部变量`print(globals()/locals())`

#### 2.4.3 LEGB规则

Python 使用 LEGB 的顺序来查找一个符号对应的对象

```
locals -> enclosing function -> globals -> builtins
```

- locals，当前命名空间
- enclosing，外部嵌套函数的命名空间（闭包中常见）
- globals，全局变量，函数定义所在模块的命名空间
- builtins，内建模块的命名空间

### 2.5 Python是动态语言

#### 2.5.1 动态语言的定义

动态语言是在运行时可以改变其结构的编程语言。如PHP、Ruby、JavaScript、Python等都属于动态语言，而C、C++则不属于。

#### 2.5.2 运行时给对象绑定（添加）属性

```python
class Persion(object):
    def __init__(self, name=None, age=None):
        self.name = name
        self.age = age
        

p1 = Person("Molly", 26)
```

如果想添加性别这个属性，可以：

```python
p1.sex = "male"

p1.sex  # 'male'
```

虽然定义的类里面没有sex这个属性，但是Python是动态语言，可以动态给实例绑定属性。

#### 2.5.3 运行时给类绑定（添加）属性

```python
p2 = Person("Tim", 27)
p2.sex
# 会报错，提示实例p2没有属性sex,上面的代码只是给实例p1绑定了属性sex，不具有通用性

# 因此，可以给Person类添加sex属性
Person.sex = None
p2 = Person("Tim", 27)
print(p2.sex)  # 如果p2这个实例对象没有sex属性，则会去类中查找类属性
# 会打印None
```

#### 2.5.4 运行时给类绑定（添加）方法

```python
import types

# 定义了一个类
class Person(object):
    num = 0
    def __init__(self, name = None, age = None):
        self.name = name
        self.age = age
    def eat(self):
        print("eat food")

# 定义一个实例方法
def run(self, speed):
    print("%s在移动, 速度是 %d km/h"%(self.name, speed))

# 定义一个类方法
@classmethod
def testClass(cls):
    cls.num = 100

# 定义一个静态方法
@staticmethod
def testStatic():
    print("---static method----")

# 创建一个实例对象
P = Person("老王", 24)
# 调用在class中的方法
P.eat()

# 给这个对象添加实例方法
P.run = types.MethodType(run, P)
# 调用实例方法
P.run(180)

# 给Person类绑定类方法
Person.testClass = testClass
# 调用类方法
print(Person.num)
Person.testClass()
print(Person.num)

# 给Person类绑定静态方法
Person.testStatic = testStatic
# 调用静态方法
Person.testStatic()
```

#### 2.5.6 \_\_slots\_\_

如果想要限制实例的属性，比如，只允许对Person实例添加name和age属性。

```python
class Person(object):
    __slots__ = ("name", "age")
    
    
p = Person()
p.name = "Tim"
p.age = 27
p.addr = "Wuhan"  # 报错AttributeError: Person instance has no attribute 'addr'
```

注意：

`__slots__`定义的属性只对当前类的实例起作用，对继承的子类不起作用。

### 2.6 生成器

#### 2.6.1 什么是生成器-generator

跟普通函数不同的是，**生成器**是一个返回迭代**器**的函数，只能用于迭代操作，更简单点理解**生成器**就是一个迭代**器**。 在调用**生成器**运行的过程中，每次遇到yield 时函数会暂停并保存当前所有的运行信息，返回yield 的值, 并在下一次执行next() 方法时从当前位置继续运行。 调用一个**生成器**函数，返回的是一个迭代**器**对象。

#### 2.6.2 创建生成器的方法

- 把列表生成式的`[]`改成`()`
- 通过yield关键字

```python
#  方法一
G = (x * 2 for x in range(5))

In [19]: next(G)
Out[19]: 0

In [20]: next(G)
Out[20]: 2

In [21]: next(G)
Out[21]: 4

In [22]: next(G)
Out[22]: 6

In [23]: next(G)
Out[23]: 8

In [24]: next(G)
--------------------------------------------------------
StopIteration                             Traceback (most recent call last)
<ipython-input-24-380e167d6934> in <module>()
----> 1 next(G)

StopIteration: 
    
#  方法二
def fib(times):
    n = 0
    a, b = 0, 1
    while n < times:
        yield b
        a, b = b, a + b
        n += 1
    return "done"
        
```

#### 2.6.3 send关键字

```python
def gen():
    i = 0
    while i < 5:
        temp = yield i
        print(temp)
        i += 1
        

g = gen()  # g is a generator
g.send("helloWorld")
```

当执行到yield时，gen函数作用暂时保存，返回i的值，temp可以接受来自`g.send("helloWorld")`中send方法传递过来的值，`g.next()`等价于`g.send(None)`

```python
#  next函数的使用
g = gen()
next(g)  # 0
next(g)  # 1
#  ... 直到抛出异常StopIteration

#  使用__next__()方法
g.__next__()  # 0
#  ... 直到抛出异常StopIteration

#  使用send方法
g.__next__()  # 0
g.send("helloWorld")  # helloWorld 1
```

#### 2.6.4 总结

生成器就是这样一个函数，记住返回时函数上一次执行的位置。

生成器的特点：

- 节约内存
- 提升代码可读性



































## 第2节 Linux系统编程

## 第3节 网络编程

## 第4节 web服务器案例

## 第5节 正则表达式

