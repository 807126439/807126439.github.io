---
title: python笔记
date: 2020-07-09 00:54:18
tags:
---
### 01.初步内容

python运行:python XX.py 直接文件名
注释:#空格开头,三"""注释多行
*IPython*: python内置编辑
Jupyter Notebook : web端编辑器

```bash
pip install jupyter
jupyter notebook
```
<!-- more -->
### 02.语言元素

变量类型:
>**整型**:python2有int,long,3只有int,0b100二进制,0o100八进制,100十进制,0x100十六进制
**浮点**,小数支持科学计数1.23e2
**字符**:单/双引号,三个单/双引号多行字符
**布尔**:大写开头
**复数**:3+5j,虚数

变量规则:
>数字不能开头

PEP 8要求：
>用小写字母拼写，多个单词用下划线连接。
受保护的实例属性用单个下划线开头（后面会讲到）。
私有的实例属性用两个下划线开头（后面会讲到）。

数据类型相关:
>type(a)获取类型
int(),float(),str(),chr()数转字符,ord()字符转数字,转换类型
身份运算符:is ,is not
成员运算符:in,not in
逻辑运算符:not,or,and

### 03.分支结构

	if XXXX:
	elif XX:
	else:

### 04.循环结构

	for x in range(10):0-9
	range(1,101):1-100
	range(1,101,2):步长2,range(100,0,-2)
	while循环:
		break/continue中断


### 05.程序逻辑

### 06.函数

	def fun(num):
		return result

方法参数,可定义预设,可以顺序不同,方法没有重载,后面会覆盖前面得定义
```
def roll_dice(n=2):
```
可变参数,方法内遍历args
```
def add(*args):
```
非本模块运行,不会调用一下方法,导入不运行
```
if __name__ == '__main__':
```
作用域:
>Python查找一个变量时会按照“***局部作用域***”、“***嵌套作用域***”、“***全局作用域***”和“***内置作用域***”的顺序进行搜索
方法内指定***global*** a  全局作用域变量,***nonlocal***嵌套作用域

### 07.字符串和常用数据结构

[x:y:z]     x:开始,从0开始,y:结束位置,不包含,z:步长,-1反转字符:
+ \转义字符
+ +连接
+ *重复输出
+ in,not in 是否包含
+ []和[:]获取切片  切片操作: **[x:y:z]     x:开始,从0开始,y:结束位置,不包含,z:步长,-1反转字符**

**格式化输出**:

		print('%d * %d = %d' % (a, b, a * b))
		print('{0} * {1} = {2}'.format(a, b, a * b))字符格式化
		print(f'{a} * {b} = {a * b}') 3.6之后,直接写参数

***列表***list()
`[]`
***字典***dict
`{a:b}`
***元组***类似列表,初始化之后不能修改
`()`
***集合***set(),相当于set,不能重复,可以计算交并差,可使用符号&|-^
`{}`

**生成式和生成器**

> 生成式,直接生成对应对象
> ```
> f = [x for x in range(1, 10)]
> ```
> 生成器,不执行,只有使用再执行,创建不是列表,是生成器对象
>```
> f = (x ** 2 for x in range(1, 1000))
> ```

### 08.面向对象编程基础

python对象默认可见性都可访问,不同于java,没有访问控制,绝对相信开发者,但是可以用__ 开头,隐藏属性名

	class Student(object):
		def __init__(self, name, age):

属性都可以访问,可以__开头隐藏,
person = Person('王大锤', 22)创建对象

### 09.面向对象进阶

	@property,属性get
	@age.setter,属性修改器
	__slots__ = ('_name', '_age', '_gender')限定绑定属性,防止语言的动态绑定其他属性


	@staticmethod 静态方法
	
	@classmethod类对象,cls固定,可以创建对象
	def now(cls):
	
	继承:
		class Student(Person):

### 11.文件和异常

	r读,w写,a追加,w+ +代表读和写,具体查询,b二进制(例如:rb,wb)
	f = open('致橡树.txt', 'r', encoding='utf-8')
	print(f.read())
	f.close()


	异常:
		try:
			with open('致橡树.txt', 'r', encoding='utf-8') as f:    资源写法,最终会关闭资源
		except FileNotFoundError:
		finally:
	
	文件读取另外:
		1.for line in f:
		2.lines = f.readlines()
	写入:
		f.write(XX)
		
	json和字典类型对应,json模块处理
		dump序列化到文件,dumps到字符串
		load文件到对象,loads字符串到对象
### 13.进程和线程

	python一进程只有一线程工作,io密集型效率更差,差10倍
	多进程:
		1.from multiprocessing import Process
		p1 = Process(target=download_task, args=('Python从入门到住院.pdf', ))
		传入方法参数
		2.继承Process,重写run
	通信使用multiprocessing的类,因为子进程复制父进程所有属性


	多线程:
	from threading import Thread,Lock
	self._lock = Lock()
	global 属性多线程可通信
### 15.图像和办公文档处理

	图像:pillow(PIL) image类 from PIL import Image
	Excel电子表格:openpyxl
	Word:python-docx
	PDF:

### 16.Python语言进阶

	生成式（推导式）的用法
	prices2 = {key: value for key, value in prices.items() if value > 100}


	heapq模块（堆排序）nlargest,nsmallest获取列表排序按元素属性也可以
	print(heapq.nlargest(2, list2, key=lambda x: x['price']))
	
	itertools模块,全排列,组合,笛卡尔积,无限序列
	
	collections模块

### 17.部分常用语法使用示例:

获取类型
```
type(a)
```
获取输入
```
x = int(input('x='))
```
x将y的值赋给x, 将x的值赋给y
```
x,y = y,x
(x, y) = (y, x) if x > y else (x, y)
```
**3,次方乘法

三元表达式
```
return True if num != 1 else False 
```
pass为空语句,不做任何工作

`print(s1, s2, s3, end='')`不换行,end参数决定print的最后输出,默认换行因为end='\n'

字符:
长度:`len(str1)`
首字母大写:`str1.capitalize()`




	