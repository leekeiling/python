## Python cls

一般来说，要使用某个类的方法，需要先实例化一个对象再调用方法。

而使用@staticmethod或@classmethod，就可以不需要实例化，直接类名.方法名()来调用。

这有利于组织代码，把某些应该属于某个类的函数给放到那个类里去，同时有利于命名空间的整洁。

```c++
class A(object):
    a = 'a'
    @staticmethod
    def foo1(name):
        print 'hello', name
    def foo2(self, name):
        print 'hello', name
    @classmethod
    def foo3(cls, name):
        print 'hello', name
```

只有函数foo2需要类对象来调用，foo1和foo3函数可以用类名直接调用