## Python语言特性

1. Python的函数参数传递

   所有变量可以理解是内存中一个对象引用，或者，也可以看似c中void*

   类型是属于对象的，而不是变量的。而对象有两种”可更改“和”不可更改“。在python中，string，tuples，和numbers是不可更改的对象，而list，dict，set等则是可以修改对象。

   当一个引用传递给函数时候，函数自动复制一份引用，这个函数里面的引用和外边的引用没有关系。

2. @staticmethod 和 @classmethod

   Python有三种方法：

   静态方法（staticmethod）

   类方法（classmethod）

   实例方法

   静态方法和类方法可以直接通过类名调用。

3. 类变量和实例变量

   类变量：相当于c++类的静态成员，在该类的所有实例共享，可以每个实例修改。

   实例变量：每个实例单独拥有的变量。

   ```python
   class Test(object):  
       num_of_instance = 0  # 类变量
       def __init__(self, name):  
           self.name = name  # 实例变量
           Test.num_of_instance += 1  
     
   if __name__ == '__main__':  
       print Test.num_of_instance   # 0
       t1 = Test('jack')  
       print Test.num_of_instance   # 1
       t2 = Test('lucy')  
       print t1.name , t1.num_of_instance  # jack 2
       print t2.name , t2.num_of_instance  # lucy 2
   ```

4. Python自省

   自省是面向对象语言所写的程序在运行时，所能知道对象的类型。简单一句是运行时能够获得对象的类型。

5. 字典推导式

   ```
   d =  {key: value for (key, value) in iterable}
   ```

6. python中单下划线和双下划线

   ```python
   >>> class MyClass():
   ...     def __init__(self):
   ...             self.__superprivate = "Hello"
   ...             self._semiprivate = ", world!"
   ...
   >>> mc = MyClass()
   >>> print mc.__superprivate
   Traceback (most recent call last):
     File "<stdin>", line 1, in <module>
   AttributeError: myClass instance has no attribute '__superprivate'
   >>> print mc._semiprivate
   , world!
   >>> print mc.__dict__
   {'_MyClass__superprivate': 'Hello', '_semiprivate': ', world!'}
   ```

   \__foo__ ： 一种约定，Python内部的名字，用来区分其他用户自定义的命名，以防止冲突。

   _foo：一种约定，用来指定变量私有。

   \__foo: 解析器用_  \_classname_foo 来代替这个名字，以区别和其他类型相同的命名，它无法直接像公有成员一样随便访问,通过对象名._类名__xxx这样的方式可以访问.

7. 字符串格式化

8. 迭代器和生成器

    问： 将列表生成式中[]改成() 之后数据结构是否改变？ 答案：是，从列表变为生成器

   ```
   >>> L = [x*x for x in range(10)]
   >>> L
   [0, 1, 4, 9, 16, 25, 36, 49, 64, 81]
   >>> g = (x*x for x in range(10))
   >>> g
   <generator object <genexpr> at 0x0000028F8B774200>
   ```

   通过列表生成式，可以直接创建一个列表。但是，受到内存限制，列表容量肯定是有限的。而且，创建一个包含百万元素的列表，不仅是占用很大的内存空间，如：我们只需要访问前面的几个元素，后面大部分元素所占的空间都是浪费的。因此，没有必要创建完整的列表（节省大量内存空间）。在Python中，我们可以采用生成器：边循环，边计算的机制—>generator

9. *args 和 **kwargs

   用`*args`和`**kwargs`只是为了方便并没有强制使用它们.

   当你不确定你的函数里将要传递多少参数时你可以用`*args`.例如,它可以传递任意数量的参数:

   ```
   >>> def print_everything(*args):
           for count, thing in enumerate(args):
   ...         print '{0}. {1}'.format(count, thing)
   ...
   >>> print_everything('apple', 'banana', 'cabbage')
   0. apple
   1. banana
   2. cabbage
   ```

   相似的,`**kwargs`允许你使用没有事先定义的参数名:

   ```
   >>> def table_things(**kwargs):
   ...     for name, value in kwargs.items():
   ...         print '{0} = {1}'.format(name, value)
   ...
   >>> table_things(apple = 'fruit', cabbage = 'vegetable')
   cabbage = vegetable
   apple = fruit
   ```

   你也可以混着用.命名参数首先获得参数值然后所有的其他参数都传递给`*args`和`**kwargs`.命名参数在列表的最前端.例如:

   ```
   def table_things(titlestring, **kwargs)
   ```

   `*args`和`**kwargs`可以同时在函数的定义中,但是`*args`必须在`**kwargs`前面.

   当调用函数时你也可以用`*`和`**`语法.例如:

   ```
   >>> def print_three_things(a, b, c):
   ...     print 'a = {0}, b = {1}, c = {2}'.format(a,b,c)
   ...
   >>> mylist = ['aardvark', 'baboon', 'cat']
   >>> print_three_things(*mylist)
   
   a = aardvark, b = baboon, c = cat
   ```

   就像你看到的一样,它可以传递列表(或者元组)的每一项并把它们解包.注意必须与它们在函数里的参数相吻合.当然,你也可以在函数定义或者函数调用时用*.

   <http://stackoverflow.com/questions/3394835/args-and-kwargs>

10. 装饰器

   **装饰器的作用就是为已经存在的对象添加额外的功能。**

   <http://taizilongxu.gitbooks.io/stackoverflow-about-python/content/3/README.html>

11. 鸭子类型

    python中不关心对象类型，只要对象有对应的方法，那么就不考虑类型判定。比如，向函数中传入参数，函数不关心参数类型，函数实现过程中有该类型有对应性质即可。

12. Python重载

    另外，一个基本的设计原则是，仅仅当两个函数除了参数类型和参数个数不同以外，其功能是完全相同的，此时才使用函数重载，如果两个函数的功能其实不同，那么不应当使用重载，而应当使用一个名字不同的函数。

    好吧，那么对于情况 1 ，函数功能相同，但是参数类型不同，python 如何处理？答案是根本不需要处理，因为 python 可以接受任何类型的参数，如果函数的功能相同，那么不同的参数类型在 python 中很可能是相同的代码，没有必要做成两个不同函数。

    那么对于情况 2 ，函数功能相同，但参数个数不同，python 如何处理？大家知道，答案就是缺省参数。对那些缺少的参数设定为缺省参数即可解决问题。因为你假设函数功能相同，那么那些缺少的参数终归是需要用的。

13. 新式类和旧式类

14. \__new__  和 \__init__的区别

    new是用来创建类的实例，而init在new创建一个实例后初始化。

15. ### 单例模式 （重要）

    单例模式是一种常用的软件设计模式。在它的核心结构中只包含一个被称为单例类的特殊类。通过单例模式可以保证系统中一个类只有一个实例而且该实例易于外界访问，从而方便对实例个数的控制并节约系统资源。如果希望在系统中某个类的对象只能存在一个，单例模式是最好的解决方案。

    `__new__()`在`__init__()`之前被调用，用于生成实例对象。利用这个方法和类的属性的特点可以实现设计模式的单例模式。单例模式是指创建唯一对象，单例模式设计的类只能实例 **这个绝对常考啊.绝对要记住1~2个方法,当时面试官是让手写的.**

    1 使用`__new__`方法

    ```python
    class Singleton(object):
        def __new__(cls, *args, **kw):
            if not hasattr(cls, '_instance'):
                orig = super(Singleton, cls)
                cls._instance = orig.__new__(cls, *args, **kw)
            return cls._instance
    
    class MyClass(Singleton):
        a = 1
    ```

    2 共享属性

    创建实例时把所有实例的`__dict__`指向同一个字典,这样它们具有相同的属性和方法.

    ```python
    class Borg(object):
        _state = {}
        def __new__(cls, *args, **kw):
            ob = super(Borg, cls).__new__(cls, *args, **kw)
            ob.__dict__ = cls._state
            return ob
    
    class MyClass2(Borg):
        a = 1
    ```

    3 装饰器版本

    ```python
    def singleton(cls):
        instances = {}
        def getinstance(*args, **kw):
            if cls not in instances:
                instances[cls] = cls(*args, **kw)
            return instances[cls]
        return getinstance
    
    @singleton
    class MyClass:
      ...
    ```

    4 import方法

    作为python的模块是天然的单例模式

    ```python
    # mysingleton.py
    class My_Singleton(object):
        def foo(self):
            pass
    
    my_singleton = My_Singleton()
    
    # to use
    from mysingleton import my_singleton
    
    my_singleton.foo()
    ```

16. Python作用域

    当 Python 遇到一个变量的话他会按照这样的顺序进行搜索：

    本地作用域（Local）→当前作用域被嵌入的本地作用域（Enclosing locals）→全局/模块作用域（Global）→内置作用域（Built-in）

17. GIL线程全局锁

    线程全局锁(Global Interpreter Lock),即Python为了保证线程安全而采取的独立线程运行的限制,说白了就是一个核只能在同一时间运行一个线程.**对于io密集型任务，python的多线程起到作用，但对于cpu密集型任务，python的多线程几乎占不到任何优势，还有可能因为争夺资源而变慢。**

18. 协程

    协程是轻量级进程，协程切换时，不需要CPU上下文切换和内核态与用户态的切换，它是由用户控制协程切换实际，不需要陷入系统的内核态

19. 闭包

    当一个内嵌函数引用外部作用域的变量时候，我们就会得到一个闭包。闭包创建需要以下条件：

    1. 必须有一个内嵌函数
    2. 内嵌函数必须引用外部函数中的变量
    3. 外部函数的返回值必须是内嵌函数

20. lambda函数

    匿名函数

21. Python函数式编程

    python中函数式编程支持:

    filter 函数的功能相当于过滤器。调用一个布尔函数`bool_func`来迭代遍历每个seq中的元素；返回一个使`bool_seq`返回值为true的元素的序列。

    ```python
    >>>a = [1,2,3,4,5,6,7]
    >>>b = filter(lambda x: x > 5, a)
    >>>print b
    >>>[6,7]
    ```

    map函数是对一个序列的每个项依次执行函数，下面是对一个序列每个项都乘以2：

    ```python
    >>> a = map(lambda x:x*2,[1,2,3])
    >>> list(a)
    [2, 4, 6]
    ```

    reduce函数是对一个序列的每个项迭代调用函数，下面是求3的阶乘：

    ```python
    >>> reduce(lambda x,y:x*y,range(1,4))
    6
    ```

22. Python里的拷贝

    引用和copy，deepcopy的区别

23. Python的垃圾回收机制

    Python GC主要使用引用计数（reference counting）来跟踪和回收垃圾。在引用计数的基础上，通过“标记-清除”（mark and sweep）解决容器对象可能产生的循环引用问题，通过“分代回收”（generation collection）以空间换时间的方法提高垃圾回收效率。

24. Python的is

    is是对比地址，==对比值

25. read，readline和readlines

    - read 读取整个文件
    - readline 读取下一行,使用生成器方法
    - readlines 读取整个文件到一个迭代器以供我们遍历

26. python super

    [Python2.7中的super方法浅见](http://blog.csdn.net/mrlevo520/article/details/51712440)

27. range和xrange

    都在循环中使用，但xrange性能更好。

    xrange使用了函数式编程的懒惰求值思想。