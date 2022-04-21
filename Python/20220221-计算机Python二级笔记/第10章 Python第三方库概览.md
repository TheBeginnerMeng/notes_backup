# 第10章 Python第三方库概览

- 第三方库的获取和安装（pip）
- 脚本程序转变为可执行程序的第三方库：PyInstaller库（必选）
- 第三方库：jieba库（必选）、wordcloud（可选）

## 10.1 第三方库的获取和安装

- Python第三方库依照安装方式灵活性和难易程度有三个方法：pip工具安装、自定义安装和文件安装

### 10.1.1 pip工具安装

pip install <拟安装库名>

### 10.1.2 自定义安装

按照第三方库提供的步骤和方式安装

### 10.1.3 文件安装

下载whl格式文件，然后pip install xxx.whl

pip -h 列出pip常用的命令

![image-20220228161335845](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\image-20220228161335845.png)

## 10.2 PyInstaller库概述

## 10.3 PyInstaller库与程序打包

- 使用PyInstaller库对Python源文件打包方式如下：

PyInstaller <Python源程序文件名>

- 执行完毕后，源文件所在目录将生成dist和build两个文件夹。最终的打包程序在dist内部与源文件同名的目录中。

- 可以通过-F参数对Python源文件生成一个独立的可执行文件，如下：

PyInstaller -F <Python源程序文件名>

- 执行后在dist目录中出现的xxx.exe文件没有任何依赖库，执行即可

![image-20220228162040533](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\image-20220228162040533.png)

## 10.4 jieba库概述

- jieba是Python中一个重要的第三方中文分词函数库
- jieba库分词的原理是利用一个中文词库，将待分词的内容与分词词库进行比对，通过图结构和动态规划方法找到最大概率的词组。除了分词，jieba还提供增加自定义中文单词的功能
- jieba库支持三种分词模式：精确模式，将句子最精确的切开，适合文本分析；全模式，把句子中所有可以成词的词语都扫描出来，速度非常快，但是不能解决歧义；搜索引擎模式，在精确模式基础上，对长词再次切分，提高召回率，适合勇于搜索引擎分词

![image-20220228162808189](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\image-20220228162808189.png)

## 10.5 jieba库与中文分词

- jieba.lcut(s)是最常用的中文分词函数，用于精确模式，即将字符串分割成等量的中文词组，返回结果是列表类型。

![image-20220228163124258](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\image-20220228163124258.png)

- jieba.lcut(s, cut_all=True)用于全模式，即将字符串的所有分词可能都列出来，返回结果是列表，冗余性最大

![image-20220228163113187](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\image-20220228163113187.png)

- ![image-20220228163149755](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\image-20220228163149755.png)

- jieba.add_word()函数，用来向jieba词库增加新的单词

## 10.6 wordcloud库概述

![image-20220228163535332](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\image-20220228163535332.png)

![image-20220228163555420](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\image-20220228163555420.png)

## 10.7 wordcloud库与可视化词云

![image-20220228164313931](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\image-20220228164313931.png)

![image-20220228164506310](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\image-20220228164506310.png)

![image-20220228164848620](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\image-20220228164848620.png)