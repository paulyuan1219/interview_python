#### 全局变量
在上面的例子中,known	这个字典是在函数外创建的,所以它属于主函数内,这是一个特殊
的层。在主函数中的变量也叫全局变量,因为所有函数都可以访问这些变量。局部变量在所
属的函数结束后就消失了,而主函数在其他函数调用结束后依然还存在。
一般常用全局变量作为	flag,也就是标识;比如用来判断一个条件是否成立的布尔变量之类
的。比如有的程序用名字为	verbose	的标识变量,来控制输出内容的详细程度:
```python
verbose	=	True
def	example1():
				if	verbose:
								print('Running	example1')
```
如果你想给全局变量重新赋值,结果会很意外。下面的例子中,本来是想要追踪确定函数是
否被调用了:
```python
been_called	=	False
def	example2():
				been_called	=	True									#	WRONG
```
你可以运行一下,并不报错,只是	been_called	的值并不会变化。这个情况的原因是
example2这个函数创建了一个新的名为	been_called	的局部变量。函数结束之后,局部变量
就释放了,并不会影响全局变量。
要在函数内部来给全局变量重新赋值,必须要在使用之前声明这个全局变量:
```python
been_called	=	False
				def	example2():
								global	been_called
								been_called	=	True
```
global	那句代码的效果是告诉解释器:『在这个函数内,been_called	使之全局变量;不要创
建一个同名的局部变量。』


下面的例子中,试图对全局变量进行更新:
```python
count	=	0
def	example3():
				count	=	count	+	1										#	WRONG
```		
运行的话,你会得到如下提示:
UnboundLocalError:	local	variable	'count'	referenced	before	assignment
(译者注:错误提示的意思是未绑定局部错误:局部变量	count	未经赋值就被引用。)
Python	会假设这个	count	是局部的,然后基于这样的假设,你就是在写出该变量之前就试图
读取。这样问题的解决方法依然就是声称count	为全局变量。
```python
def	example3():
				global	count
				count	+=	1
```
如果全局变量指向的是一个可修改的值,你可以无需声明该变量就直接修改:
```python
known	=	{0:0,	1:1}
def	example4():
				known[2]	=	1

```
所以你可以在全局的列表或者字典里面添加、删除或者替换元素,但如果你要重新给这个全
局变量赋值,就必须要声明了:
```python
def	example5():
				global	known
				known	=	dict()
```
全局变量很有用,但不能滥用,要是总修改全局变量的值,就让程序很难调试了。


12.4	参数长度可变的元组
函数的参数可以有任意多个。用星号*开头来作为形式参数名,可以将所有实际参数收录到一
个元组中。例如	printall	就可以获取任意多个数的参数,然后把它们都打印输出:
```python
def	printall(*args):
				print(args)
```
你可以随意命名收集来的这些参数,但	args	这个是约定俗成的惯例。下面展示一下这个函数
如何使用:
```python
>>>	printall(1,	2.0,	'3')
(1,	2.0,	'3')

```
与聚集相对的就是分散了。如果有一系列的值,然后想把它们作为多个参数传递给一个函
数,就可以用星号*运算符。比如	divmod	要求必须是两个参数;如果给它一个元组,是不能
进行运算的:
```python
>>>	t	=	(7,	3)
>>>	divmod(t)
TypeError:	divmod	expected	2	arguments,	got	1
```
但如果拆分这个元组,就可以了:
```python
>>>	divmod(*t)
(2,	1)
```
很多内置函数都用到了参数长度可变的元组。比如	max	和	min	就可以接收任意数量的参数:


12.5	列表和元组
zip	是一个内置函数,接收两个或更多的序列作为参数,然后返回返回一个元组列表,该列表
中每个元组都包含了从各个序列中的一个元素。这个函数名的意思就是拉锁,就是把不相关
的两排拉锁齿连接到一起。
下面这个例子中,一个字符串和一个列表通过	zip	这个函数连接到了一起:
```python
>>>	s	=	'abc'
>>>	t	=	[0,	1,	2]
>>>	zip(s,	t)
<zip	object	at	0x7f7d0a9e7c48>
```
该函数的返回值是一个	zip	对象,该对象可以用来迭代所有的数值对。zip	函数经常被用到	for
循环中:
```python
>>>	for	pair	in	zip(s,	t):	...
print(pair)	...
('a',	0)	('b',	1)	('c',	2)
```
zip	对象是一种迭代器,也就是某种可以迭代整个序列的对象。迭代器和列表有些相似,但不
同于列表的是,你无法通过索引来选择迭代器中的指定元素。
如果想用列表的运算符和方法,可以用	zip	对象来构成一个列表:
```python
>>>	list(zip(s,	t))
[('a',	0),	('b',	1),	('c',	2)]
```
返回值是一个由元组构成的列表;在这个例子中,每个元组都包含了字符串中的一个字母,
以及列表中对应位置的元素。
在长度不同的序列中,返回的结果长度取决于最短的一个。
```python
>>>	list(zip('Anne',	'Elk'))
[('A',	'E'),	('n',	'l'),	('n',	'k')]
用	for	循环来遍历一个元组列表的时候,可以用元组赋值语句:
t	=	[('a',	0),	('b',	1),	('c',	2)]
for	letter,	number	in	t:
				print(number,	letter)
```
每次经历循环的时候,Python	都选中列表中的下一个元组,然后把元素赋值给字母和数字。
该循环的输出如下:
0	a	1	b	2	c
如果结合使用	zip、for	循环以及元组赋值,就能得到一种能同时遍历两个以上序列的代码组
合。比如下面例子中的	has_match	这个函数,接收两个序列t1和	t2作为参数,然后如果存在
一个索引位置	i	使得	t1[i]	==	t2[i]就返回真:
```python
def	has_match(t1,	t2):
				for	x,	y	in	zip(t1,	t2):
								if	x	==	y:
												return	True
				return	False
```
如果你要遍历一个序列中的所有元素以及它们的索引,可以用内置的函数	enumerate:
```python
for	index,	element	in	enumerate('abc'):
				print(index,	element)
```
enumerate	函数的返回值是一个枚举对象,它会遍历整个成对序列;每一对都包括一个索引
(从0开始)以及给定序列的一个元素。在本节的例子中,输出依然如下:
0	a	1	b	2	c




However, we can tell the sorted() function which attribute to sort on by specifying a key to be used. Let’s define one:
```python
  >>> def byName_key(person):
  ...     return person.name
  ... 

  >>> sorted(people, key = byName_key)
  [<name: Adam, age: 43>, <name: Becky, age: 11>, <name: Jack, age: 19>]
```


copy	模块包含了一个名叫	copy	的函数,可以
复制任意对象:
```python
>>>	p1	=	Point()
>>>	p1.x	=	3.0
>>>	p1.y	=	4.0
>>>	import	copy
>>>	p2	=	copy.copy(p1)
```
p1和	p2包含的数据是相同的,但并不是同一个点对象。
```python
>>>	print_point(p1)
(3,	4)
>>>	print_point(p2)
(3,	4)
>>>	p1	is	p2
False
>>>	p1	==	p2
False
```
如果你用	copy.copy	复制了一个矩形,你会发现该函数复制了矩形对象,但没有复制内嵌的
点对象。
```python
>>>	box2	=	copy.copy(box)
>>>	box2	is	box
False
>>>	box2.corner	is	box.corner
True
```
所幸的是	copy	模块还提供了一个名为	deepcopy	(深复制)的方法,这样就能把内嵌的对象
也复制了。你肯定不会奇怪了,这种运算就叫深复制了。
```python
>>>	box3	=	copy.deepcopy(box)
>>>	box3	is	box
False
>>>	box3.corner	is	box.corner
False

```

#### 类和对象
```python
#	inside	class	Time:
def __init__(self,	hour=0,	minute=0,	second=0):
    self.hour	=	hour
    self.minute	=	minute
    self.second	=	second
#	inside	class	Time:
def __str__(self):
    return	'%.2d:%.2d:%.2d'	%	(self.hour,	self.minute,	self.second)

#	inside	class	Time:
def	__add__(self,	other):
    seconds	=	self.time_to_int()	+	other.time_to_int()
    return	int_to_time(seconds)
    
>>>	start	=	Time(9,	45)
>>>	duration	=	Time(1,	35)
>>>	print(start	+	duration)
11:20:00

#	inside	class	Time:
def	__add__(self,	other):
    if	isinstance(other,	Time):
        return	self.add_time(other)
    else:
        return	self.increment(other)
def	add_time(self,	other):
    seconds	=	self.time_to_int()	+	other.time_to_int()
    return	int_to_time(seconds)
def	increment(self,	seconds):
    seconds	+=	self.time_to_int()
    return	int_to_time(seconds)
```

```python
#### Map Reduce and Filter
说说例子吧，我一般会在sorted, max, 这类函数里的key用lambda.比如有一个比较复杂的数组结构
```python
s= [('a', 3), ('b', 2), ('c', 1)]
sorted(s, key=lambda x:x[1])
[很短的小文，很清楚](http://book.pythontips.com/en/latest/map_filter.html)


number_list = range(-5, 5)
less_than_zero = list(filter(lambda x: x < 0, number_list))
print(less_than_zero)

# Output: [-5, -4, -3, -2, -1]

from functools import reduce
product = reduce((lambda x, y: x * y), [1, 2, 3, 4])

# Output: 24
```
[很不错的coolshell](http://coolshell.cn/articles/10822.html)
