## pandas性能优化

### 时间戳
#### 使用Datetime数据节省时间
Datetime类型是属于object dtype类型   
object类型像一个大容器，不仅仅可以承载str，也可以包含哪些不能很好地融进一个数据类型的任何  
特征列。 而如果我们将日期作为str类型就会极大的影响效率。  
对于时间序列的数据而言， 我们需要让上面的date_time列格式化为datetime对象数组。  
<center>
<img src="https://upload-images.jianshu.io/upload_images/14581851-ec4fe081d2036a99?imageMogr2/auto-orient/strip%7CimageView2/2/w/565/format/webp"/>
</center>

#### 设置转化格式
<center>
<img src = "https://upload-images.jianshu.io/upload_images/14581851-2a1470853e8ebd88?imageMogr2/auto-orient/strip%7CimageView2/2/w/580/format/webp"/>
</center>
我们设置了转化的格式format。由于在CSV中的datetimes并不是 ISO 8601 格式的，如果不进行设置的话，那么pandas将使用 dateutil 包把每个字符串str转化成date日期。  
相反，如果原始数据datetime已经是 ISO 8601 格式了，那么pandas就可以立即使用最快速的方法来解析日期。这也就是为什么提前设置好格式format可以提升这么多。  


### pandas数据循环操作

**一个错误案例**
<center>
<img src = "https://upload-images.jianshu.io/upload_images/14581851-0b97c7ddeea7d368?imageMogr2/auto-orient/strip%7CimageView2/2/w/640/format/webp"/>
</center>
性能低下原因  
- 首先， 他需要初始化一个将记录输出的列表
- 其次， 它使用不透明对象(0 , len(df))循环， 然后再应用apply_tariff()之后， 必须将结果附加到用于创建新大DataFrame列的列表中。
- 使用df.iloc[i]['data_time']执行所谓的链式索引


#### itertuples() iterrows()循环，
实际上可以通过pandas引入itertuples和iterrows方法可以使效率更快。这些都是一次产生一行的生成器方法，类似scrapy中使用的yield用法。  

.itertuples为每一行产生一个namedtuple，并且行的索引值作为元组的第一个元素。  
nametuple是Python的collections模块中的一种数据结构，其行为类似于Python元组，  
但具有可通过属性查找访问的字段。  

.iterrows为DataFrame中的每一行产生（index，series）这样的元组。  

<center>
<img src = "https://upload-images.jianshu.io/upload_images/14581851-1d0442f13019c62b?imageMogr2/auto-orient/strip%7CimageView2/2/w/640/format/webp"/>
</center>

#### pandas的 apply()方法
我们可以使用.apply方法而不是.iterrows进一步改进此操作。  
Pandas的.apply方法接受函数(callables)并沿DataFrame的轴(所有行或所有列)应用它们。  
在此示例中，lambda函数将帮助你将两列数据传递给apply_tariff()：  
<center>
<img src = "https://upload-images.jianshu.io/upload_images/14581851-c51a9e317727c9c5?imageMogr2/auto-orient/strip%7CimageView2/2/w/640/format/webp"/>
</center>

.apply的语法优点很明显，行数少，代码可读性高。在这种情况下，所花费的时间大约是.iterrows方法的一半。  
但是，这还不是“非常快”。一个原因是.apply()将在内部尝试循环遍历Cython迭代器。  
但是在这种情况下，传递的lambda不是可以在Cython中处理的东西，因此它在Python中调用，因此并不是那么快。    
如果你使用.apply()获取10年的小时数据，那么你将需要大约15分钟的处理时间。  
如果这个计算只是大型模型的一小部分，那么你真的应该加快速度。  
也就是矢量化操作派上用场的地方。    

####矢量化操作：使用isin()选择数据 

<center>
<img src = "https://upload-images.jianshu.io/upload_images/14581851-fd9d0cc43defec77?imageMogr2/auto-orient/strip%7CimageView2/2/w/817/format/webp"/>
</center>
这些值标识哪些DataFrame索引(datetimes)落在指定的小时范围内。  
然后，当你将这些布尔数组传递给DataFrame的.loc索引器时，你将获得一个仅包含与这些小时匹配的行的DataFrame切片。  
在那之后，仅仅是将切片乘以适当的费率，这是一种快速的矢量化操作。  

这与我们上面的循环操作相比如何？首先，你可能会注意到不再需要apply_tariff()，因为所有条件逻辑都应用于行的选择。  
因此，你必须编写的代码行和调用的Python代码会大大减少。  

#### 使用numpy
使用pandas时不应忘记一点是Pandas Series 和 DataFrames是在NumPy库之上设计的。  
下面，我们将使用NumPy的 digitize() 函数。  
它类似于Pandas的cut()，因为数据将被分箱，但这次它将由一个索引数组表示，这些索引表示每小时所属的bin。  
然后将这些索引应用于价格数组：   

<center>
<img src = "https://upload-images.jianshu.io/upload_images/14581851-dc587d9932c1838e?imageMogr2/auto-orient/strip%7CimageView2/2/w/650/format/webp" />
</center>

### 使用HDFStore防止重新处理
通常，在构建复杂数据模型时，可以方便地对数据进行一些预处理。  
例如，如果您有10年的分钟频率耗电量数据，即使你指定格式参数，只需将日期和时间转换为日期时间可能需要20分钟。  
你真的只想做一次，而不是每次运行你的模型，进行测试或分析。  

你可以在此处执行的一项非常有用的操作是预处理，然后将数据存储在已处理的表单中，以便在需要时使用。  
但是，如何以正确的格式存储数据而无需再次重新处理？如果你要另存为CSV，则只会丢失datetimes对象，并且在再次访问时必须重新处理它。  

Pandas有一个内置的解决方案，它使用 HDF5，这是一种专门用于存储表格数据阵列的高性能存储格式。  
 Pandas的 HDFStore 类允许你将DataFrame存储在HDF5文件中，以便可以有效地访问它，同时仍保留列类型和其他元数据。  
 它是一个类似字典的类，因此您可以像读取Python dict对象一样进行读写。  
 
<center>
<img src = "https://upload-images.jianshu.io/upload_images/14581851-4b395367ccbbe4d9?imageMogr2/auto-orient/strip%7CimageView2/2/w/501/format/webp" />
<center>

### 总结
- 尝试尽可能使用矢量化操作，而不是在df 中解决for x的问题。如果你的代码是许多for循环，那么它可能更适合使用本机Python数据结构，因为Pandas会带来很多开销。
- 如果你有更复杂的操作，其中矢量化根本不可能或太难以有效地解决，请使用.apply方法。
- 如果必须循环遍历数组（确实发生了这种情况），请使用.iterrows()或.itertuples()来提高速度和语法。
- Pandas有很多可选性，几乎总有几种方法可以从A到B。请注意这一点，比较不同方法的执行方式，并选择在项目环境中效果最佳的路线。
- 一旦建立了数据清理脚本，就可以通过使用HDFStore存储中间结果来避免重新处理。
- 将NumPy集成到Pandas操作中通常可以提高速度并简化语法。


 
 









