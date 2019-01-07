## apply transform
相同点：  
都能针对dataframe完成特征的计算，并且常常与groupby()方法一起使用  

不同点：  
apply()里面可以跟自定义函数，包括简单的求和函数以及复杂的特征建的差值函数等。  

transform()里面不能跟自定义的特征交互函数，因为transform是针对每一元素进行计算：  
- 它只能对每一列计算，所以在groupby()之后， transform()之前是要指定要操作的列，这点与apply有很大的不同  

- 由于是只能对每一列计算，所以方法的通用性相比apply()就局限很多


### 各方法耗时

agg() + python内置方法的计算速度最快，其次是transform() + python内置方法   
transform() + 自定义函数组合方法最慢  
python自带的stats统计模块在pandas结构中的计算也非常慢，也需要避免使用  

### 小技巧
使用apply(0方法处理大数据级时，可以考虑使用joblib中的多线程/多进程模块改造相应函数  
执行计算。  
