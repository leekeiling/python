# Python全局变量和局部变量及它们的作用域

1.定义的函数内部的变量名如果是第一次出现， 且在=符号前，那么就可以认为是被定义为局部变量。在这种情况下，不论全局变量中是否用到该变量名，函数中使用的都是局部变量。例如：

```c++
num = 100
def func():
  num = 123
  print num
func()
```

2.定义在函数外部的变量是全局变量，可以在函数内部读全局变量。

例如：

```c++
num = 100
def func():
  x = num + 100
  print x
func()
```

不能在函数内部直接修改或者赋值全局变量，例如一下代码：

```c++
num = 100
def func():
  num += 100
  print num
func()
```

输出结果是：UnboundLocalError: local variable 'num' referenced before assignment。提示错误：局部变量num在赋值前被应用。

这是因为，python解释器检测到函数内部全局变量num被重新赋值，num就会被定义为局部变量，由于函数内部num作为局部变量还未初始化就引用，所以出现报错。

要想在函数内部修改全局变量，需要在函数内部对全局变量增加关键字global。如：

```c++
num = 100
def func():
  global num;
  num += 100
  print num
func()
```

3.函数中使用某个变量时，如果该变量名既有全局变量也有局部变量，则默认使用局部变量。例如：

```c++
num = 100
def func():
  num = 200
  x = num + 100
  prinx x
func()
```

输出结果是300。

4.在函数中将某个变量定义为全局变量时需要使用关键字global。例如：

```c++
num = 100
def func():
  global num
  num = 200
  print num
func()
print num
```

输出结果分别是200和200。这说明函数中的变量名num被定义为全局变量，并被赋值为200

**变量作用域**

变量作用域（scope）在Python中是一个容易掉坑的地方。
Python的作用域一共有4中，分别是：

L （Local） 局部作用域
E （Enclosing） 闭包函数外的函数中
G （Global） 全局作用域
B （Built-in） 内建作用域
以 L --> E --> G -->B 的规则查找，即：在局部找不到，便会去局部外的局部找（例如闭包），再找不到就会去全局找，再者去内建中找。

Python除了def/class/lambda 外，其他如: if/elif/else/  try/except  for/while并不能改变其作用域。定义在他们之内的变量，外部还是可以访问。

```python
>>> if True:
...   a = 'I am A'
... 
>>> a
'I am A'
```

定义在if语言中的变量a，外部还是可以访问的。
但是需要注意如果if被 def/class/lambda 包裹，在内部赋值，就变成了此 函数/类/lambda 的局部作用域。
在 def/class/lambda内进行赋值，就变成了其局部的作用域，局部作用域会覆盖全局作用域，但不会影响全局作用域。

**闭包Closure**

闭包的定义：如果在一个内部函数里，对在外部函数内（但不是在全局作用域）的变量进行引用，那么内部函数就被认为是闭包(closure)

函数嵌套/闭包中的作用域：

```python
a = 1
def external():
  global a
  a = 200
  print a
 
  b = 100
  def internal():
    # nonlocal b
    print b
    b = 200
    return b
 
  internal()
  print b
 
print external()
```

一样会报错- 引用在赋值之前，这类似与在函数内部直接修改全局变量导致全局变量转为局部变量的情况。Python3有个关键字nonlocal可以解决这个问题，但在Python2中还是不要尝试修改闭包中的变量。 关于闭包中还有一个坑：

```python
from functools import wraps
 
def wrapper(log):
  def external(F):
    @wraps(F)
    def internal(**kw):
      if False:
        log = 'modified'
      print log
    return internal
  return external
 
@wrapper('first')
def abc():
  pass
 
print abc()
```

也会出现 引用在赋值之前 的错误，原因是解释器探测到了 if False 中的重新赋值，所以不会去闭包的外部函数（Enclosing）中找变量，但 if Flase 不成立没有执行，所以便会出现此错误。除非你还需要else: log='var' 或者 if True 但这样添加逻辑语句就没了意义，所以尽量不要修改闭包中的变量。

**locals() 和 globals()**

**globals()**

global 和 globals() 是不同的，global 是关键字用来声明一个局部变量为全局变量。globals() 和 locals() 提供了基于字典的访问全局和局部变量的方式

比如：如果函数1内需要定义一个局部变量，名字另一个函数2相同，但又要在函数1内引用这个函数2。

```python
def var():
  pass
 
def f2():
  var = 'Just a String'
  f1 = globals()['var']
  print var
  return type(f1)
 
print f2()
# Just a String
# <type 'function'>
```

**Locals()**

Python的locals()函数会以dict类型返回当前位置的全部局部变量。