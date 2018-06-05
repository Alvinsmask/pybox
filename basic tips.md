1.字符串格式化

`<format string>%<dautm>`分别是格式化字符串、格式化运算符以及要格式化的单个字符串
对于多个字符串输出的格式化来说，`<format string>%（<dautm-1>,... ,<dautm-n>）`其中格式化字符串由%起始
- int类型的格式化(**d代表整型数**)
宽度格式化：**当宽度表示为负数时，数据左对齐；宽度表示为正数时，数据右对齐。**如果字段的宽度小于或等于字符中的数据的打印长度时，不添加对齐信息。
```Python
>>>for exp in range(7,11):
    print('%3d%3s%-12d'%(exp, ' ',10**exp))
>>> 7   10000000    
    8   100000000   
    9   1000000000  
   10   10000000000 
```
- float类型的格式化（涉及到打印精度）
其格式化信息为`<field width>.<precision>f` 小数点后的precision表示数字精度
```Python
>>> print('%6.sf'%3.14)
'3.140'
```
 2. Python operator.itemgetter函数
 
 `from operator import itemgetter` 作用：itemgetter 用于获取对象的哪些位置的数据，参数即为代表位置的序号值
 
 itemgetter 获取的不是值，而是定义了一个函数，通过该函数作用到目标对象上，取出目标对象对应维度的值例如：
 
 ```Python
   >>>a = [1,2,3]
   >>>b = [[1,2,3],[4,5,6],[7,8,9]] 
   >>>get_1 = itemgetter(1)
   >>>get_1(a)  
   >>> 2
   >>>get_1(b)  
   >>> [4,5,6]
 ```
