PEP: 8
标题：Python代码样式指南
版本：修订版
最近一次修改：
作者：Guido van Rossum <guido@python.org>,
    Barry Warsaw <barry@python.org>,
    Nick Coghlan <ncoghlan@gmain.com>
状态：活跃
类型：进行
内容-类型: text/x-rst
创建时间：2001-07-05
发表时间：2001-07-05，2013-08-01

介绍
=============
这篇文档为Python代码在主要的Python分支中构成标准库提供了编码规定。请在Python中的C语言实现中参见同类信息的C语言PEP描述风格指南[1]_。

这篇文档和PEP 257（文档字符串风格规定）都是源自Guido原来的Python风格指南手稿，其中加入了一些Barry的风格指南说明。

这份编码风格指南历经多年时间发展至今，确认并更新了一些新的编码约束，同时过往的一些风格约束已经随着编码语言本身的改变过时了。

很多项目工程都有他们自己的编码风格指南。如果与本文的编码风格产生了歧义和冲突，那么请优先参见根据具体项目而制定的编码风格指南。

不要一味地遵循教条主义
===============================

Guido的关键思想之一是认为代码被阅读理解多于编写。此处提供的这份指南是为了提升代码的可读性，同时使其在Python编码的广泛应用中变得通用。正如PEP 20中所提到的，“可读性举足轻重”。

一份风格指南通常是关于一致性的。因此，风格指南的一致性尤为重要。在一个工程项目中保持一致性更为重要。最重要的则是在一个模块和功能中保持一致性。

然而，要知道何时需要变更一致性，因为有时候风格指南的建议并不实用。当有疑惑时，务必好好判定。查看其他的例子然后思考决定哪种最优。同时，不要吝啬你的提问！

值得注意的是：不要为了遵守这份PEP编码风格指南而破坏过去的兼容性！

还有一些原因需要你忽略掉某一条特定的指南：

1. 当采用这份编码指南会让你的代码可读性变差，甚至对于那些已经习惯于阅读PEP规范的代码的人来说。

2. 尽管这是一次清理他人留下的混乱代码的绝佳机会（在真实的XP风格中），但为了和上下文的代码保持一致性，也需要打破这个原则（可能是由于某些历史原因）

3. 由于那些存疑的代码开发时间早于这份说明指南，因此没理由去修正原来的代码。

4. 当Python旧版本不支持这份风格指南中的某些特性，而代码仍需要和该版本保持兼容性时。


代码布局
=======

缩进
----

每一个缩进级别请使用4个空格。

当采用连续行时，应当与使用了Python隐式行并在其中加入小括号，中括号和大括号封装的元
素保持垂直对齐，或者用一个悬浮缩进。当使用悬浮缩进时，接下来的行内容就应该被考虑在内；不应该有参数在第一行，同时后续的缩进应该作为一个连续行用来明显区分自身。

``` python
    # 正确用法：
    # 与开口分隔符对齐。
    foo = long_function_name(var_one, var_two,
                             var_three, var_four) 
    # 添加4个空格（另外的一个缩进级别）用来从剩余的参数中区分。
    def long_function_name(
            var_one, var_two, var_three,
            var_four):
        print(var_one)
    
    # 悬浮缩进应当增加一个级别
    foo = long_function_name(
        var_one, var_two,
        var_three, var_four)

    # 错误：

    # 当不使用垂直对齐时，首行的参数是禁止的。
    foo = long_function_name(var_one, var_two,
        var_three, var_four)
    
    # 后续需要进行的缩进方式无法达到区分的目的
    def long_function_name(
        var_one, var_two, var_three,
        var_four):
        print(var_one)
```

后续的代码行可以选择性地应用“四空格”缩进规则。

可选择的方式：

```python
# 悬挂缩进可以不使用4个空格
foo = long_function_name(
  var_one, var_two,
  var_three, var_four)
```

多行 if 表达式：

当条件部分的 if 表达式足够长，需要跨多行书写时，注意可以在两个字符关键字的连接处（比如 if），加一个单独的空格，再加一个左括号，来给多行条件语句的后续代码行，创造一个合理的4-空格缩进。这可能会与嵌套在`if`表达式中的其他4-空格缩进代码产生视觉冲突。这份PEP中，没有明确指明在`if`表达式中如何（或者是否）进一步区分条件代码行和内嵌的代码行。

可使用的可选项包括但不限于以下几种情况：

```python
# 没有额外的缩进
if (this_is_one_thing and
    that_is_another_thing):
    do_something()
    
# 添加一条注释，在能提供语法高亮的编辑器中能提供一些区分
if (this_is_one_thing and
    that_is_another_thing):
    # 既然所有的条件都为真，我们可以操作了
    do_something()

# 在后续条件判断的语句中添加额外的缩进
if (this_is_one_thing
   		and that_is_another_thing):
    do_something()
```

（也可以参见下面关于是否在二进制操作符之前或之后截断的讨论）

在多行结构中，大括号、中括号、小括号的右括号可以与内容列表的最后一行对齐后，另起一行作为第一个字符，就像这样：

```python
my_list = [
    1, 2, 3,
    4, 5, 6
	]

result = some_function_that_takes_arguments(
	'a', 'b', 'c',
	'd', 'e', 'f'
	)
```

或者可能与开始多行结构的第一个字符行对齐，就像下面这样：

```python
my_list = [
    1, 2, 3,
    4, 5, 6
]
result = some_function_that_takes_arguments(
	'a', 'b', 'c',
	'd', 'e', 'f'
)
```

