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

3. sorted()函数 返回类型为list

4. 高阶函数

高阶函数接受另一个函数作为参数，并且以某种方式应用这个函数。 **Python包含了內建的高阶函数， map 和 filter 用于处理可迭代对象**

- map() 函数

map() 会根据提供的函数对指定序列做映射。第一个参数 function 以参数序列中的每一个元素调用 function 函数，返回包含每次 function 函数返回值的新列表。

`map(function, iterable, ...)` **Python 2.x 返回列表。Python 3.x 返回迭代器。**

代码示例：

```Python
data=map(lambda x : x**2, [1, 2, 3])
print(data.__next__())
print(data.__next__())
print(data.__next__())
>>>
    1
    4
    9
```

- filter() 函数

Python内建的filter()函数用于过滤序列。

和map()类似，filter()也接收一个函数和一个序列。和map()不同的时，**filter()把传入的函数依次作用于每个元素，然后根据返回值是True还是False决定保留还是丢弃该元素。**

```Python
def is_odd(n):
     return n % 2 ==1
     
f = filter(is_odd, [1,2,3,4])

print(f.__next__())
print(f.__next__())
>>>
    1
    3
```

5. 捕获异常

try-except语句允许程序捕获异常并执行相应的恢复操作。语法形式如下：

```Python
try:
    <statements>
except <exception type>:
    <statements>
```
执行到这条语句的时候，try子句中的语句将会执行。如果语句中的一条引发异常，执行会立即转到except子句。**如果引发的异常和子句中的异常类型一致，将会执行其语句**，否则会转到try-except语句的调用者，并且进一步沿着调用链向上传递，直到异常成功得到处理，或者程序终止并产生一条错误消息。如果try子句中的语句没有引发异常，就会跳过except子句，并且继续执行到try-except语句的末尾。

示例：使用一个**递归函数**约束用户输入行为，捕获到valueerror异常，就会强制用户再次输入，直到正确为止。

6. 文件及其操作 

**可以使用with语句，自动关闭文件**

```Python
with open('myfile.txt', 'w') as f: 
    <statements>
```

- 文本文件的输出
write方法接受字符串作为参数，因此其他类型的数据，例如整数或者浮点数必须先转换成字符串，然后才能输出到文件中。
```Python
f=open('myfile.txt', 'w')  # 第二个参数'w'表示在内存中打开文件用于输出，'r'表示打开文件用于读取
f.write('first line. \nseconf line.\n')
f.close()  #  不进行关闭操作的话可能会造成数据丢失
```
