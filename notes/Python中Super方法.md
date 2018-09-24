## Python中Super方法

### 经典类和新式类

- 经典类是python2.2之前的东西,但是在2.7还在兼容,但是在3之后的版本就只承认新式类了
- 新式类在python2.2之后的版本中都可以使用

### 为什么不用经典类

> 原因在于经典类在类多重继承的时候是采用`从左到右深度优先`原则匹配方法的，而新式类是采用`C3算法`(不同于广度优先)进行匹配的，经典类最容易出现的就是子类越过父类的方法，以下两个例子

```
# 当前，我们先定义一个大的父类，用Bird来创建，里面有个构造方法和定义了两个方法，分别是eat，sing方法，然后创建了一个Sparrow的子类，继承自父类Bird，自己也拥有构造方法，首先进行测试
class Bird:
    def __init__(self):
        print '我是父类的初始化'
        self.cansing = True
    def eat(self):
        print 'i can eat'

    def sing(self):
        if self.cansing:
            print 'i can sing'

class Sparrow(Bird):
    def __init__(self):
        print '我就是要重写父类的init'

    def jump(self):
        print 'i can jump'

S = Sparrow()
S.jump()
S.eat()
S.sing()1234567891011121314151617181920212223
```

> 然后很喜闻乐见的报错了，因为子类重写了父类的构造方法，我的理解是被覆盖了而不是像append那样接上，所以结果如图，问题的原因在于Sparrow这个类已经重写了其父类的init，导致cansing的方法被绕过

```shell
我就是要重写父类的init
i can jump
i can eat
---------------------------------------------------------------------------
AttributeError                            Traceback (most recent call last)
<ipython-input-9-f4eaf99ef216> in <module>()
     21 S.jump()
     22 S.eat()
---> 23 S.sing()

<ipython-input-9-f4eaf99ef216> in sing(self)
      7 
      8     def sing(self):
----> 9         if self.cansing:
     10             print 'i can sing'
     11 

AttributeError: Sparrow instance has no attribute 'cansing'
12345678910111213141516171819
```

> 我们再来看一个例子,关于经典类的深度优先查找的规则，看完你就知道为什么py3要使用新式类了

```python
class A():
    def foo1(self):
        print "A"
class B(A):
    def foo2(self):
        pass
class C(A):
    def foo1(self):
        print "C"
class D(B, C):
    pass

d = D()
d.foo1()

# A12345678910111213141516
```

> 按照经典类的查找顺序`从左到右深度优先`的规则，在访问`d.foo1()`的时候,D这个类是没有的..那么往上查找,先找到B,里面没有,深度优先,访问A,找到了foo1(),所以这时候调用的是A的foo1()，从而导致C重写的foo1()被绕过

### 补救经典类方法

方法一：按照类名访问

> 调用未绑定的超类构造方法，这个方法在python2.2使用较多，之后随着super出现后，被取缔，原因也有很多，这个具体自己百度吧。

```
class Bird:
    def __init__(self):
        print '我是父类的初始化'
        self.cansing = True
    def eat(self):
        print 'i can eat'

    def sing(self):
        if self.cansing:
            print 'i can sing'

class Sparrow(Bird):
    def __init__(self):
        Bird.__init__(self)
        print '我就是要重写父类的init'

    def jump(self):
        print 'i can jump'

S = Sparrow()
S.jump()
S.eat()
S.sing()1234567891011121314151617181920212223
我是父类的初始化
我就是要重写父类的init
i can jump
i can eat
i can sing12345
```

 **这个方法个人来看并不推荐，因为如果要修改很多很多子类，若是父类名字一变，修改量太大，而且，对父类的访问着实很多次。**

## 方法二：使用super

> 另一种方式就是使用super，记得这是个只能在新式类中才起作用，所以代码块前需要加

```
__metaclass__ = type1
```

> 完整代码块如下：

```
__metaclass__ = type
class Bird:
    def __init__(self):
        print '我是父类的初始化'
        self.cansing = True
    def eat(self):
        print 'i can eat'

    def sing(self):
        if self.cansing:
            print 'i can sing'

class Sparrow(Bird):
    def __init__(self):
        super(Sparrow,self).__init__()
        print '我就是要重写父类的init'

    def jump(self):
        print 'i can jump'

S = Sparrow()
S.jump()
S.eat()
S.sing()123456789101112131415161718192021222324
```

 **效果和上述采用未绑定的超类方法一样，但是更加直观。下面盗用一下为什么super方法那么超级：** 
**即使类已经继承多个超类，它也只需要使用一次super函数**

**注意：建议就是要么一直用super,要么一直用按照类名访问**



**Super类用来弥补旧式类缺陷。旧式类在多重继承方面因为采用深度优先搜索方式来访问父类方法而导致某些父类方法被绕过。super方法采用另一种算法来搜索父类方法，可以通过super方法来调用父类被覆盖的方法。**