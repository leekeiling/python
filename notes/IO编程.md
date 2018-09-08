## IO编程

### 文件读写

1. 打开文件

   ```python
   open(name[.mode[.buffering]])
   ```

2. 文件模式

   | 值   | 描述                         |
   | ---- | ---------------------------- |
   | 'r'  | 读模式                       |
   | 'w'  | 写模式                       |
   | 'a'  | 追加模式                     |
   | 'b'  | 二进制模式（图片等影像文件） |
   | '+'  | 读/写模式                    |

3. 文件缓冲区

   设置了缓冲大小后，数据先写入内存，只有使用了flush或者close函数才会将数据更新到磁盘。

4. 文件读取

   ```python
   with open(filepath, 'r') as fileReader:
   	print fileReader.read()
   	#print fileReader.read(size)
   ```

5. 文件写入

   ```python
   f = open(r'c:\text\input.txt', 'w')
   f.write('ouput')
   f.close()
   ```

   可以反复调用write写入文件，最后必须使用close方法关闭文件。使用write方法是，操作系统先将数据写入缓存，等到空闲时候再写入文件中。

### 序列化操作

序列化：把内存中的变量变成可存储或可传输的过程。

python模块：cPickle和pickle

方法：序列化使用dumps或者dump方法。dumps方法可以将任意对象序列化成一个str，然后可以将这个str写入文件进行保存。反序列化使用的是loads方法或者load方法。

```python
import cPickle as pickle
d = dict(url = 'index.html', title = '首页')
pickle.dumps(d)
f = open(r'D:\dump.txt', 'wb')
pickle.dump(d,f)
f.close()
```

