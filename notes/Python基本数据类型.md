## Python基本数据类型

#### 列表 list类

列表是由一系列特定元素顺序排列的元素组成的，它的元素可以是任何数据类型即数字、字符串、列表、元组、布尔值等，同时元素可以修改

形式为

```python
names = ["name", "jame", "tom"]
或者
names = list(["name", "jame", "tom"])
```

1.   **索引、切片‘**

   ```python
   #索引-->从0开始，而不是从一开始
   name =["xiaowu","little-five","James"]
   print(name[0:-1])
   ```

2. **追加 append() 扩展 extend()**

   追加appen和扩展extend区别是：前者为添加将元素作为一个整体添加，后者为数据类型的元素分解添加至列表内

   ```python
   #extend-->扩展
   name =["xiaowu","little-five","James"]
   name.extend(["hello","world"])
   print(name)
   输出为-->['xiaowu', 'little-five', 'James', 'hello', 'world']
    
   #append -->追加
   name.append(["hello","world"])
   print(name)
   输出为 -->['xiaowu', 'little-five', 'James', ['hello', 'world']]
   ```

3. **insert() -->插入**

4. **pop() -->取出**

   ```python
   #pop()--取出，可将取出的值作为字符串赋予另外一个变量
   name =["xiaowu","little-five","James"]
   special_name =name.pop(1)
   print(name)
   print(special_name,type(special_name))
   
   #输出为：['xiaowu', 'James']
   #           little-five <class 'str'>
   ```

5. **remove()-->移除、del -->删除**

   ```python
   #remove -->移除，其参数为列表的值的名称
   name =["xiaowu","little-five","James"]
   name.remove("xiaowu")
   print(name)
   
   #其输出为：['little-five', 'James']
   
   #del -->删除
   name =["xiaowu","little-five","James"]
   #name.remove("xiaowu")
   del name[1]
   print(name)
   
   #其输出为：['xiaowu', 'James']
   ```

6. **sorted()-->排序，默认正序，加入reverse =True，则表示倒序**

   ```python
   #正序
   num =[11,55,88,66,35,42]
   print(sorted(num)) -->数字排序
   name =["xiaowu","little-five","James"]
   print(sorted(name)) -->字符串排序
   #输出为：[11, 35, 42, 55, 66, 88]
   #    ['James', 'little-five', 'xiaowu']
   
   #倒序
   num =[11,55,88,66,35,42]
   print(sorted(num,reverse=True))
   #输出为：[88, 66, 55, 42, 35, 11]
   ```

### 元组 tuple类

元组即为不可修改的列表。其与特性跟list相似，其使用圆括号而不是方括号来标识

```python
#元组
name = ("little-five","xiaowu")
print(name[0])
```

### 字典 dict类

```python
#字典的定义
info ={
    1:"hello world",  #键为数字
    ("hello world"):1, #键为元组
    False:{ 
        "name":"James"
    },
    "age":22
}
```

遍历 items keys values

```python
info ={
   "name":"little-five",
   "age":22,
   "email":"99426353*@qq,com"
}
#键
for key in info:
    print(key)
print(info.keys())
#输出为：dict_keys(['name', 'age', 'email'])

#键值对
print(info.items())
#输出为-->dict_items([('name', 'little-five'), ('age', 22), ('email', '99426353*@qq,com')])

#值
print(info.values())
#输出为：dict_values(['little-five', 22, '99426353*@qq,com'])
```

### 集合

   关于集合set的定义：在我看来集合就像一个篮子，你可以往里面存东西也可往里面取东西，但是这些东西又是无序的，你很难指定单独去取某一样东西；同时它又可以通过一定的方法筛选去获得你需要的那部分东西。故集合可以 创建、增、删、关系运算。

1. **创建：set、frozenset**

   ```python
   #1、创建，将会自动去重,其元素为不可变数据类型，即数字、字符串、元组
   test01 ={"zhangsan","lisi","wangwu","lisi",666,("hello","world",),True}
   #或者
   test02 =set({"zhangsan","lisi","wangwu","lisi",666,("hello","world",),True})
   
   #2、不可变集合的创建 -->frozenset()
   test =frozenset({"zhangsan","lisi","wangwu","lisi",666,("hello","world",),True})
   ```

2. **增：   add、update**

   ```python
   #更新单个值 --->add
   names ={"zhangsan","lisi","wangwu"}
   names.add("james") #其参数必须为hashable类型
   print(names)
   
   #更新多个值 -->update
   names ={"zhangsan","lisi","wangwu"}
   names.update({"alex","james"})#其参数必须为集合
   print(names)
   ```

3. **删除：pop、remove、discard**

   ```python
   #随机删除 -->pop
   names ={"zhangsan","lisi","wangwu","alex","james"}
   names.pop()
   print(names)
   
   #指定删除，若要删除的元素不存在，则报错 -->remove
   names ={"zhangsan","lisi","wangwu","alex","james"}
   names.remove("lisi")
   print(names)
   
   #指定删除，若要删除的元素不存在，无视该方法 -->discard
   names ={"zhangsan","lisi","wangwu","alex","james"}
   names.discard("hello")
   print(names)
   ```
