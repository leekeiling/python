## Python面向对象

#### 创建类

```python
#!/usr/bin/python
# -*- coding: UTF-8 -*-
 
class Employee:
   '所有员工的基类'
   empCount = 0
 
   def __init__(self, name, salary):
      self.name = name
      self.salary = salary
      Employee.empCount += 1
   
   def displayCount(self):
     print "Total Employee %d" % Employee.empCount
 
   def displayEmployee(self):
      print "Name : ", self.name,  ", Salary: ", self.salary
```

- empCount是类的静态变量
- 创建类时会调用初始化方法\__init__
- self 代表类的实例，self 在定义类的方法时是必须有的，虽然在调用时不必传入相应的参数。

#### 创建实例对象

```python
"创建 Employee 类的第一个对象"
emp1 = Employee("Zara", 2000)
"创建 Employee 类的第二个对象"
emp2 = Employee("Manni", 5000)
```

#### 访问属性

您可以使用点号 . 来访问对象的属性。

可以添加，删除，修改类的属性，如下所示

```python
emp1.age = 7  # 添加一个 'age' 属性
emp1.age = 8  # 修改 'age' 属性
del emp1.age  # 删除 'age' 属性
```

你也可以使用以下函数的方式来访问属性：

- getattr(obj, name[, default]) : 访问对象的属性。
- hasattr(obj,name) : 检查是否存在一个属性。
- setattr(obj,name,value) : 设置一个属性。如果属性不存在，会创建一个新属性。
- delattr(obj, name) : 删除属性。

```python
hasattr(emp1, 'age')    # 如果存在 'age' 属性返回 True。
getattr(emp1, 'age')    # 返回 'age' 属性的值
setattr(emp1, 'age', 8) # 添加属性 'age' 值为 8
delattr(emp1, 'age')    # 删除属性 'age'
```

#### Python内置类属性

```python
__dict__ : 类的属性（包含一个字典，由类的数据属性组成）
__doc__ :类的文档字符串
__name__: 类名
__module__: 类定义所在的模块（类的全名是'__main__.className'，如果类位于一个导入模块mymod中，那么className.__module__ 等于 mymod）
__bases__ : 类的所有父类构成元素（包含了一个由所有父类组成的元组）
```

#### Python对象销毁（垃圾回收）

python使用了引用计数这一简单技术来跟踪和回收垃圾。

在python内部记录着所有使用中的对象各有多少引用。

一个内部跟踪变量，称为一个引用计数器

当对象被创建时，就创建一个引用计数，当这个对象不再需要时，也就是说，这个对象的引用计数变为0 时， 它被垃圾回收。但是回收不是"立即"的， 由解释器在适当的时机，将垃圾对象占用的内存空间回收。

垃圾回收机制不仅针对引用计数为0的对象，同样也可以处理循环引用的情况。循环引用指的是，两个对象相互引用，但是没有其他变量引用他们。这种情况下，仅使用引用计数是不够的。Python 的垃圾收集器实际上是一个引用计数器和一个循环垃圾收集器。作为引用计数的补充， 垃圾收集器也会留心被分配的总量很大（及未通过引用计数销毁的那些）的对象。 在这种情况下， 解释器会暂停下来， 试图清理所有未引用的循环。

\__del\__, 在对象销毁时候被调用，当对象不再使用时，\__del__方法运行 

#### 类的继承

**继承语法**

```c++
class 派生类名(基类名)
    ...
```

在python中继承中的一些特点：

- 1、如果在子类中需要父类的构造方法就需要显示的调用父类的构造方法，或者不重写父类的构造方法。详细说明可查看：[python 子类继承父类构造函数说明](http://www.runoob.com/w3cnote/python-extends-init.html)。
- 2、在调用基类的方法时，需要加上基类的类名前缀，且需要带上 self 参数变量。区别在于类中调用普通函数时并不需要带上 self 参数
- 3、Python 总是首先查找对应类型的方法，如果它不能在派生类中找到对应的方法，它才开始到基类中逐个查找。（先在本类中查找调用的方法，找不到才去基类中找）。

**issubclass()或者isinstance()**

- issubclass() - 布尔函数判断一个类是另一个类的子类或者子孙类，语法：issubclass(sub,sup)
- isinstance(obj, Class) 布尔函数如果obj是Class类的实例对象或者是一个Class子类的实例对象则返回true。

#### 方法重写

如果你的父类方法的功能不能满足你的需求，你可以在子类重写你父类的方法

#### 基础重载方法

```c++
__init__ ( self [,args...] )
构造函数
简单的调用方法: obj = className(args)
__del__( self )
析构方法, 删除一个对象
简单的调用方法 : del obj
__repr__( self )
转化为供解释器读取的形式
简单的调用方法 : repr(obj)
__str__( self )
用于将值转化为适于人阅读的形式
简单的调用方法 : str(obj)
__cmp__ ( self, x )
对象比较
简单的调用方法 : cmp(obj, x)
```

#### 运算符重载

```python
#!/usr/bin/python
 
class Vector:
   def __init__(self, a, b):
      self.a = a
      self.b = b
 
   def __str__(self):
      return 'Vector (%d, %d)' % (self.a, self.b)
   
   def __add__(self,other):
      return Vector(self.a + other.a, self.b + other.b)
 
v1 = Vector(2,10)
v2 = Vector(5,-2)
print v1 + v2
```

#### 类属性和方法

**类的私有属性**

\__private_attrs: 两个下划线开头，声明该属性为私有，不能在类的外部被使用或直接访问。在类内部的方法使用时sefl.__private_attrs

**类的方法**

在类的内部，使用def关键字可以为类定义一个方法，与一般函数定义不同，类方法必须包含参数self，且为第一个参数

**类的私有方法**

\__private_method: 两个下划线 开头，声明该方法为私有方法，不能在类外部调用。在类的内部调用self.__private_method

```python
#!/usr/bin/python
# -*- coding: UTF-8 -*-
 
class JustCounter:
    __secretCount = 0  # 私有变量
    publicCount = 0    # 公开变量
 
    def count(self):
        self.__secretCount += 1
        self.publicCount += 1
        print self.__secretCount
 
counter = JustCounter()
counter.count()
counter.count()
print counter.publicCount
print counter.__secretCount  # 报错，实例不能访问私有变量
```

python不允许实例化的类访问私有数据，但可以使用object._className_attrName访问属性，参考一下实例：

```python
#!/usr/bin/python
# -*- coding: UTF-8 -*-

class Runoob:
    __site = "www.runoob.com"

runoob = Runoob()
print runoob._Runoob__site
```

### 单下划线、双下划线、头尾双下划线说明：

- **\__foo__**: 定义的是特殊方法，一般是系统定义名字 ，类似 __init__() 之类的。
- **_foo**: 以单下划线开头的表示的是 protected 类型的变量，即保护类型只能允许其本身与子类进行访问，不能用于 from module import *
- **__foo**: 双下划线的表示的是私有类型(private)的变量, 只能是允许这个类本身进行访问了。

