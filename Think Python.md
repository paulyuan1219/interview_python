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
