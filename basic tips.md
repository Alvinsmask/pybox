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