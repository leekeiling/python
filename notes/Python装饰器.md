## Python装饰器

### 装饰器基础：

在python中函数是对象，因此函数可以赋值给一个变量、可以在其他函数中返回以及作为参数传递给其他函数

函数可以在函数内声明

### 实现

装饰器是在不改变一个函数实现的基础下，为这个函数添加其他特性，然后封装返回包含新特性的函数。

基于此，我们可以通过以下例子来理解装饰器是怎样实现的。

怎么做才能让一个函数同时用两个装饰器,像下面这样:

```python
@makebold
@makeitalic
# 上面两个是python的语法糖
def say():
   return "Hello"
```

我希望得到

```python
<b><i>Hello</i></b>
```

我只是想知道装饰器怎么工作的!

------

去看看[文档](https://docs.python.org/2/reference/compound_stmts.html#function),答在下面:

```python
def makebold(fn):
    def wrapped():
        return "<b>" + fn() + "</b>"
    return wrapped

def makeitalic(fn):
    def wrapped():
        return "<i>" + fn() + "</i>"
    return wrapped

@makebold
@makeitalic
def hello():
    return "hello world"

print hello() ## returns <b><i>hello world</i></b>
```

@makebold
@makeitalic

上面两个是python的语法糖，它们的真正实现是：

```python
# 字体变粗装饰器
def makebold(fn):
    # 装饰器将返回新的函数
    def wrapper():
        # 在之前或者之后插入新的代码
        return "<b>" + fn() + "</b>"
    return wrapper

# 斜体装饰器
def makeitalic(fn):
    # 装饰器将返回新的函数
    def wrapper():
        # 在之前或者之后插入新的代码
        return "<i>" + fn() + "</i>"
    return wrapper

@makebold
@makeitalic
def say():
    return "hello"

print say()
#输出: <b><i>hello</i></b>

# 这相当于
def say():
    return "hello"
say = makebold(makeitalic(say))

print say()
#输出: <b><i>hello</i></b>
```

