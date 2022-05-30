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

#### 2.6.1 什么是生成器generator

跟普通函数不同的是，**生成器**是一个返回迭代器的函数，只能用于迭代操作，更简单点理解**生成器**就是一个迭代器。 在调用**生成器**运行的过程中，每次遇到yield 时函数会暂停并保存当前所有的运行信息，返回yield 的值, 并在下一次执行next() 方法时从当前位置继续运行。 调用一个**生成器**函数，返回的是一个迭代器对象。

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

## 03. Python高级三

### 3.1 元类

#### 3.1.1 类也是一种对象

通俗来说，类就是一段用来生成对象的代码段。本质上，类也是一种对象，只要你使用`class`关键字，Python解释器就会在执行的时候创建一个对象，这个对象拥有创建实例的能力。

#### 3.1.2 动态地创建类

因为类也是对象，故而可以在使用的时候动态地创建。

```python
def choose_class(name):
    if name == "foo":
        class Foo(object):
            pass
        return Foo  # 返回的是类对象，而不是类的实例
    else:
        class Bar(object):
            pass
        return Bar
    
    
MyClass = choose_class("foo")
print(MyClass)  # <class '__main__.choose_class.<locals>.Foo'>
# 函数返回的是一个类，而不是类的实例对象
print(MyClass())  # 可以通过这个类创建类实例，即类的实例对象
#  <__main__.choose_class.<locals>.Foo object at 0x000001FD820B0550>
```

#### 3.1.3 使用type来创建类

虽然通过上面的方式，一定程度上实现了动态创建不同的类，但是仍然需要编码定义整个类，由于类也是一种对象，是否有一种方法可以生成类对象呢？

此时，type出场了，殊不知，它还有一种特殊的作用，**动态创建类**。

type可以接受一个类的描述作为参数，然后返回一个类。

type(类名, 由父类名称组成的元组(针对继承的情况而定，可以为空), 包含属性的字典)

```python
# 传统方式定义一个类
class Test(object):
    pass

Test()  # 创建了一个Test类的实例对象


Test2 = type("Test2", (), {})  # 定义了一个Test2类
Test2()  # 创建了一个Test2类的实例对象
```

#### 3.1.4 使用type创建带有属性的类

type接收一个字典类为类定义属性

```python
Foo = type("Foo", (), {"bar": True})

# 等价于
class Foo(object):
    bar = True
    
FooChild = type("FooChild", (Foo,), {})
# FooChild子类继承自Foo父类
```

#### 3.1.5 使用type创建带有方法的类

添加实例方法

```python
def echo_bar(self):
    """定义了一个普通的函数"""
    print(self.bar)
    
FooChild = type("FooChild", (Foo,), {"echo_bar": echo_bar})

hasattr(Foo, "echo_bar")  # False
hasattr(FooChild, "echo_bar")  # True

my_foo = FooChild()
my_foo.echo_bar()  # True
```

添加静态方法

```python
@staticmethod
def testStatic():
    print("static method")
    
    
FooChild = type("FooChild", (Foo,), {"testStatic": testStatic})
```

添加类方法

```python
@classmethod
def testClass(cls):
    print(cls.bar)

    
# 后面的同上
```

#### 3.1.6 什么是元类

元类就是用来创建类的东西，通俗的说，元类就是类的类

```python
MyClass = MetaClass()  # 使用元类创建一个对象，这个对象就是类
MyObject = MyClass()  # 使用类来创建出实例对象
```

上面提到过，type可以创建类：

```python
MyClass = type("MyClass", (), {})
```

本质上type也是一个元类，类似于str是创建字符串对象的类，int是创建整数对象的类，可通过`__class__`属性检查。

```python
>>> age = 35
>>> age.__class__
<type 'int'>
>>> name = 'bob'
>>> name.__class__
<type 'str'>
>>> def foo(): pass
>>>foo.__class__
<type 'function'>
>>> class Bar(object): pass
>>> b = Bar()
>>> b.__class__
<class '__main__.Bar'>
```

然后，那么对于任何一个`__class__`的`__class__`属性又是什么呢？

```python
>>> a.__class__.__class__
<type 'type'>
>>> age.__class__.__class__
<type 'type'>
>>> foo.__class__.__class__
<type 'type'>
>>> b.__class__.__class__
<type 'type'>
```

因此，type就是用来创建类对象的元类。

#### 3.1.7 \__metaclass__属性

可以在定义一个类的时候添加`__mataclass_`属性

```python
class Foo(object):
    __metaclass__ = something...
```

在创建Foo类时，Python解释器会先在类的定义代码块中查看是否有\__metaclass__属性，如果有，就会用它来创建Foo类；如果没有，则会用内建的type类创建这个类

#### 3.1.8 自定义元类

元类的目的是为了在创建一个类时可以灵活的改变，从而得到符合当前上下文的类。

```python
#coding=utf-8

class UpperAttrMetaClass(type):
    # __new__ 是在__init__之前被调用的特殊方法
    # __new__是用来创建对象并返回之的方法
    # 而__init__只是用来将传入的参数初始化给对象
    # 你很少用到__new__，除非你希望能够控制对象的创建
    # 这里，创建的对象是类，我们希望能够自定义它，所以我们这里改写__new__
    # 如果你希望的话，你也可以在__init__中做些事情
    # 还有一些高级的用法会涉及到改写__call__特殊方法，但是我们这里不用
    def __new__(cls, future_class_name, future_class_parents, future_class_attr):
        #遍历属性字典，把不是__开头的属性名字变为大写
        newAttr = {}
        for name,value in future_class_attr.items():
            if not name.startswith("__"):
                newAttr[name.upper()] = value

        # 方法1：通过'type'来做类对象的创建
        # return type(future_class_name, future_class_parents, newAttr)

        # 方法2：复用type.__new__方法
        # 这就是基本的OOP编程，没什么魔法
        # return type.__new__(cls, future_class_name, future_class_parents, newAttr)

        # 方法3：使用super方法
        return super(UpperAttrMetaClass, cls).__new__(cls, future_class_name, future_class_parents, newAttr)

# python3的用法
# class Foo(object, metaclass = UpperAttrMetaClass):
#     bar = 'bip'

print(hasattr(Foo, 'bar'))
# 输出: False
print(hasattr(Foo, 'BAR'))
# 输出:True

f = Foo()
print(f.BAR)
# 输出:'bip'
```

元类的作用：

- 拦截类的创建
- 修改类
- 返回修改之后的类

### 3.2 垃圾回收

#### 3.2.1 小整数对象池

为了避免频繁为整数申请和销毁内存空间，Python使用了小整数对象池，小整数对象池的定义是[-5, 257)，在这个范围内的整数对象都是提前建立好的，在一个Python程序中，所有位于这个范围内的整数使用的都是同一个对象。

对于单个字母和单词也是这样的。

#### 3.2.2 大整数对象池

每一个大整数，均创建一个新的对象。

#### 3.2.3 intern机制

对于单个单词，不可修改，默认开启intern机制，共用对象，当对象的引用计数为0时，则销毁。

注意：如果字符串含有空格，则不开启intern机制。

#### 3.2.4 垃圾回收

现在的很多高级语言为了方便用户使用，不再由用户自己手动管理内存，而是通过**垃圾回收机制**来分配和回收内存。

Python也采用了垃圾回收机制，而不一样的是，Python是通过引用**计数机制**为主，**标记-清除机制**为辅的策略。

Python中的每一个东西都是对象，核心本质是一个结构体。当引用计数为0时，该对象的生命就结束了。

引用计数机制的优点：

1. 简单
2. 实时：一旦没有引用计数，内存就自动释放

缺点：

1. 维护引用计数消耗资源
2. 循环引用会带来问题（由此引入了标记-清除和分代收集机制）

#### 3.2.5 Ruby与Python的垃圾回收

GC（Garbage Collection）系统负责的工作其实远不止垃圾回收，它们的工作主要有：

- 为新生成的对象分配内存
- 识别垃圾对象，回收垃圾对象的内存

如果把应用程序比作人的身体，代码部分则是人的大脑，而垃圾回收就是应用程序那颗跳动的心，为身体其他器官提供血液和营养。

> Ruby的对象分配

在创建对象时，Ruby其实已经预创建了成百上千的对象，并串联在一个链表上，如果需要创建一个新对象，就从这些未使用的对象中选取一个。

> Python的对象分配

在创建对象时，Python解释器立即向操作系统申请内存空间。

> Ruby就像身处凌乱的房间

Ruby会把已经无用的对象留在内存里，直到下一次GC。也就是说，Ruby会一直在预可用列表对象中选取可用的对象进行使用，而用过的则会一直保留，直到预可用列表越来越短。

那么，当Ruby中的预可用列表用完时，怎么进行垃圾回收呢？

> Ruby的房间脏到一定程度，它也开始要打扫了…

此时，**标记-清除**（Mark and Sweep）机制就诞生了，由McCarthy发明。当预可用列表用尽时，Ruby会中断程序，同时开始遍历所有的指针，变量和其他引用，Ruby也会使用虚拟机遍历内部指针。最终，Ruby标记出这些指针引用的每个对象。被标记的对象代表是“干净”的，剩下的未被标记的对象就是垃圾对象，接下来Ruby就会清除这些剩下的垃圾对象，并将它们送回预可用列表中。

> Python身处干净卫生的房间

由于Python有引用计数机制，如果一个对象的引用计数为0，Python会立即释放它占用的内存空间，就像房间产生了垃圾，就立即打扫。

> 标记-删除机制 VS 引用计数

既然Python的引用计数这么好用，那么Ruby为什么宁可忍受“脏乱差”也不采用引用计数呢？原因如下：

1. 引用计数不好实现。首先，Python为每个对象预留一块空间处理引用计数；与此同时，Python对象修改或者引用时，都会随着引用计数的变化成为一个复杂的操作，甚至可能还需要随时准备释放对象，极大影响了效率。
2. 引用计数机制相对较慢。比如当Python需要释放一个很大的对象时，可能需要一次性释放很多内存空间，会占用较多时间。
3. 引用计数无法处理含有循环引用的数据结构。

#### 3.2.6 Python中的循环数据结构以及引用计数

如果存在循环数据结构（如双链表），在Python中会有一个孤岛：存在一组未使用，但是互相指向（引用计数不为0，一般为1）的对象，其实谁都没有外部的引用。然而，现有的引用计数机制无法处理掉这些孤岛对象，因此，**隔代回收**机制诞生了。

> 标记-清除机制

Ruby中给使用的对象加了一个标记，被标记的对象说明是存活的，剩下未被标记的对象只能是垃圾对象。Ruby将不再被使用到的垃圾对象重新放入一个预可用链表中。如下图所示：

![image-20220530143721582](https://raw.githubusercontent.com/TheBeginnerMeng/notes_backup/main/imgs/image-20220530143721582.png)

Python中则使用了一种不同的链表来追踪活跃的对象，称为零代（Generation Zero），每当创建一个新对象的时候，Python会将其加入到零代链表中。

![image-20220530144158036](https://raw.githubusercontent.com/TheBeginnerMeng/notes_backup/main/imgs/image-20220530144158036.png)

到了一定时机，Python内部会遍历零代链表上的每一个对象，如果发现有内部引用，但是没有外部引用的对象，Python会将“活跃的对象”转移到一代链表上，并将零代链表上的垃圾对象（存在内部引用，无外部引用的对象）的引用计数减少，直到多次触发导致引用计数为0，让其被GC回收。

从本质上来说，Python的隔代回收类似于Ruby的标记-清除，都是周期性的分析对象的引用情况，并将活跃对象进行保留。

#### 3.2.7 Python中的GC阈值

理论上，Python解释器持续对新创建对象的数目、以及因为引用计数为零而被释放掉的对象数目进行追踪，这两个值应该始终一致。

然而，由于循环引用，以及一些存在时间更长的对象，从而会导致被分配对象的计数值和被释放对象的计数值之间的差异逐渐增大。一旦这个差异值到达某个阈值，Python的收集机制就会启动，并触发上面提到的零代算法，释放一些垃圾对象，并将活跃的对象移动到**一代链表**上。

再过一段时间，会采用同样的处理方法，将一代链表上的活跃对象移动到**二代链表**上（最多只有二代）。

通过这种方式，Python解释器会频繁处理零代链表上的对象，其次是一代，然后才是二代。

> 弱代假说

隔代回收算法的核心行为：垃圾回收器在频繁处理新对象。而一个新对象从零代转移到一代链表时，Python解释器会Promote提升这个对象。

这么处理的原因是什么呢？弱代假说（weak generation hypothesis）。这个假说的观点是：年轻的对象通常死的快，而老的对象可能存活更长的时间。

> 总结

通过频繁地处理零代链表中的新对象，Python的垃圾回收器可以把时间花在更有意义的地方，因为新对象往往成为垃圾对象的概率更高。同时，只有在很少的时候，即满足阈值条件的时候，才会去处理一代和二代上的那些老对象。（哈哈哈，和互联网公司完全相反，淘汰新的，留下老的）

#### 3.2.8 引用计数

> 导致引用计数+1的情况

- 对象被创建，例如a=1
- 对象被引用，例如a=b
- 对象被作为参数，传入到一个函数中，例如func(a)
- 对象作为一个元素，存储在容器中，例如list1 = [a, a]

> 导致引用计数-1的情况

- 对象的别名被显式销毁，例如del a
- 对象的别名被赋予了新的对象，例如a = 2
- 一个对象离开了它的作用域，例如f函数执行完毕时，func函数中的局部变量（全局变量不会）
- 对象所在的容器被销毁，或者从容器中删除对象

> 查看一个对象的引用计数

```python
import sys
a = "hello world"
sys.getrefcount(a)
```

> 触发垃圾回收的情况

- 调用gc.collect()
- 当gc模块的计数器达到阈值的时候
- 程序退出的时候

> gc模块常用功能解析

gc模块提供一个接口给开发者设置垃圾回收的选项。采用引用计数的方法管理内存存在的问题是无法处理循环引用的问题，而gc模块的一个主要功能就是解决循环引用。

gc模块中会有一个长度为3的列表的计数器，可以通过`gc.get_count()`获取。

例如：(488, 3, 0)，其中488是距离上一次一代垃圾检查，Python分配内存的数目减去释放内存的数目。

3是指距离上一次二代垃圾检查，已经执行过的一代垃圾检查的次数。0是指距离上一次三代垃圾检查，已经执行过的二代垃圾检查的次数。

## 第2节 Linux系统编程

## 第3节 网络编程

## 第4节 web服务器案例

## 第5节 正则表达式

