# 第9章 Python标准库概览

考纲考点

- 标准库：turtle
- 标准库：random、time

## 9.1 turtle库概述

- 进行基本图形绘制
- 基本框架：一个小海龟在坐标系中爬行，其爬行轨迹构成了绘制图形。对于小海龟来说，有前进、后退、旋转等爬行行为

## 9.2 turtle库与基本绘图

- turtle库包含100多个功能函数，主要包括窗体函数、画笔状态函数、画笔运动函数等三类

### 9.2.1 窗体函数

- ![image-20220225165514236](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\image-20220225165514236.png)

### 9.2.2 画笔状态函数

![image-20220225165614470](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\image-20220225165614470.png)

![image-20220225165748829](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\image-20220225165748829.png)

![image-20220225165804461](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\image-20220225165804461.png)

### 9.2.3 画笔运动函数

![image-20220225165835422](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\image-20220225165835422.png)

![image-20220225165909622](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\image-20220225165909622.png)

![image-20220225165926220](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\image-20220225165926220.png)

![image-20220225165938700](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\image-20220225165938700.png)

## 9.3 random库概述

random.random()，生成[0.0, 1.0)之间的随机小数

## 9.4 random库与随机函数运用

- random库的常用函数

![image-20220225173626579](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\image-20220225173626579.png)

- 设置随机数种子的好处是可以准确复现随机数序列，用户重复程序的运行轨迹

## 9.5 time库概述

![image-20220225173852682](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\image-20220225173852682.png)

- time.localtime(secs)获取当前时间戳对应的当地时间的struct_time对象

![image-20220228153451224](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\image-20220228153451224.png)

- 使用time.ctime(secs)获取当前时间戳对应的易读字符串表示，内部会调用time.localtime()函数以输出当地时间

![image-20220228153606222](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\image-20220228153606222.png)

- time库使用time.mktime()、time.strftime()、time.strptime()进行时间格式化

- 使用time.mktime(t)将struct_time转换为时间戳，t代表当地时间。struct_time对象的元素如下：

![image-20220228153837727](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\image-20220228153837727.png)

- 调用time.mktime(t)函数

![image-20220228153932213](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\image-20220228153932213.png)

- time.strftime()函数是时间格式化最常用的方法，几乎可以以任何通用格式输出时间。该方法利用一个格式字符串，对时间格式进行表达。

![image-20220228154050221](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\image-20220228154050221.png)

- ![image-20220228154111949](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\image-20220228154111949.png)

- ![image-20220228154202616](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\image-20220228154202616.png)

## 9.6 time库与程序计时

time.perf_counter()