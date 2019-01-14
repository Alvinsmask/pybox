1.字符串格式化

`<format string>%<dautm>`分别是格式化字符串、格式化运算符以及要格式化的单个字符串
对于多个字符串输出的格式化来说，`<format string>%（<dautm-1>,... ,<dautm-n>）`其中格式化字符串由%起始
- int类型的格式化(**d代表整型数**)
宽度格式化：**当宽度表示为负数时，数据左对齐；宽度表示为正数时，数据右对齐。** 如果字段的宽度小于或等于字符中的数据的打印长度时，不添加对齐信息。
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

**使用'W'**，文件若存在，首先要清空，然后（重新）创建，

**使用'a'模式 **，把所有要写入文件的数据都追加到文件的末尾，即使你使用了seek（）指向文件的其他地方，如果文件不存在，将自动被创建。

- 文本文件的读取

常规方法：
```Python
f=open('myfile.txt', 'r')  
text=f.read()  # 将整个文件作为一个单独的字符串输入。如果文件包含了多行文本，换行字符会嵌入到字符串中
# 在读取结束后再次调用read方法，会返回一个空的字符串，表明已经到达了文件的末尾。

text2=f.readline()  # 只读取一行输入，并返回字符串，包括换行符。如果readline遇到了文件末尾，返回空字符串
#  常与while语句搭配以读取文本文件
f.close()  
```
**readlines**方法：依次读取每行，最后返回列表

```Python
with open('myfile.txt', 'w') as f:
   for data in f.readlines():
        <statements>
```
**pickle模块** 该模块可以直接存储对象而不用进行字符串的类型转换

例如，可以使用pickle模块把一个列表中的对象保存到.dat文件中再进行恢复。 我们不需要知道列表中的对象是什么类型以及对象的数目。

```Python
import pickle

# 写入模块
lyst = [60, 'a string object', 190.3]
fileobj = open('items.dat', 'wb')  # 打开items.dat文件，如果不存在则创建， ‘wb’字节标志表示以写入方式打开
for item in lyst:
    pickle.dump(ite,,fileobj)
fileogj.close()

#读取模块
lyst=list()
fileobj = open('items.dat', 'rb')  # 打开items.dat文件，如果不存在则创建， ‘wb’字节标志表示以读取方式打开
while True：
    try:
        item=pickle.load(fileobj)
        lyst.append(item)
    except EOFError:  # 捕获到达文件末尾时产生的EOFError
        flieobj.close()
        break
```

7. str类型的一些方法

- 字符串分割函数

    Python split() 通过指定分隔符对字符串进行切片，如果参数 num 有指定值，则仅分隔 num 个子字符串

    `str.split(str="", num=string.count(str))`

    str -- 分隔符，默认为所有的空字符，包括空格、换行(\n)、制表符(\t)等; num -- 分割次数。

- 移除字符串首尾指定的字符（默认为空格或换行符）或字符序列

    该方法只能删除开头或是结尾的字符，不能删除中间部分的字符。

    `str.strip([chars])`

    **chars -- 移除字符串头尾指定的字符序列。**

-  `str.find('chars')  #返回子字符的下标位置，若不存在返回-1 `

8. 有关类

- 定义python类的语法为：
    ```Python
    class <class name>(<parent class name>):
        <class variable assignment>
        <instance method definition>
    ```
    按照惯例，类名需要大写； 实例方法__init__叫做构造方法（类比构造函数）

**@classmethod 用法** classmethod 修饰符对应的函数不需要实例化，不需要 self 参数，但第一个参数需要是表示自身类的 cls 参数，可以来调用类的属性，类的方法，实例化对象等。cls表示类对象，而不是类实例

例如：

    ```Python
    class A(object):
        bar = 1
        def func1(self):  
            print ('foo')
        @classmethod
        def func2(cls):
            print ('func2')
            print (cls.bar)
            cls().func1()   # 调用 foo 方法

    A.func2()               # 不需要实例化

    >>> func2
        1
        foo
    ```

- super方法

    super() 函数是用于调用父类(超类)的一个方法。

    super 是用来解决多重继承问题的，直接用类名调用父类方法在使用单继承的时候没问题，但是如果使用多继承，会涉及到查找顺序（MRO）、重复调用（钻石继承）等种种问题。

    MRO 就是类的方法解析顺序表, 其实也就是继承父类方法时的顺序表。

    Python3.x 和 Python2.x 的一个区别是: Python 3 可以使用直接使用 super().xxx 代替 super(Class, self).xxx :

    ```Python
    #!/usr/bin/python
    # -*- coding: UTF-8 -*-

    class FooParent(object):
        def __init__(self):
            self.parent = 'I\'m the parent.'
            print ('Parent')

        def bar(self,message):
            print ("%s from Parent" % message)

    class FooChild(FooParent):
        def __init__(self):
            # super(FooChild,self) 首先找到 FooChild 的父类（就是类 FooParent），然后把类B的对象 FooChild 转换为类 FooParent 的对象
            super(FooChild,self).__init__()    
            print ('Child')

        def bar(self,message):
            super(FooChild, self).bar(message)
            print ('Child bar fuction')
            print (self.parent)

    if __name__ == '__main__':
        fooChild = FooChild()
        fooChild.bar('HelloWorld')

    >>>
        Parent
        Child
        HelloWorld from Parent
        Child bar fuction
        I'm the parent.

    ```


9. 有关路径与目录的函数

有关获取项目位置的方法
```Python
sys.path  # 表示当前工程下系统的额环境变量
sys.path[0]  # 表示当前文档（项目）所在的绝对路径
```

有关获取当前目录下所有文件信息的方法
```Python
os.listdir(path)  # 获取path下所有文件的文件名
```

拼接路径的方法
```Python
os.path.join(path1，path2,....,filename)  # 获取path下所有文件的文件名
```

Python 读取指定目录及其子目录下所有文件名

在此使用 python 的`os.walk()` 函数实现遍历指定目录及所有子目录下的所有文件。

`walk()`函数返回目录树生成器(迭代器)。通过自顶向下遍历目录来生成目录树中的文件名。对于根目录顶部（包括顶部本身）树中的每个目录，它产生一个3元组`（dirpath，dirnames，filenames）`。dirpath是一个字符串，即目录的路径。

dirnames是dirpath中子目录的名称列表。filenames是dirpath中非目录文件名称的列表。但列表中的名称不包含路径，要得到一个完整路径（从顶部开始）到dirpath中的文件或目录，请执行`os.path.join（dirpath，name）`。更多详情可查看 python 标准库文档os.walk() 。

实现代码如下
```Python
import os
def all_path(dirname):
    filelistlog = dirname + "\\filelistlog.txt"  # 保存文件路径
    # postfix = set(['pdf','doc','docx','epub','txt','xlsx','djvu','chm','ppt','pptx'])  # 设置要保存的文件格式
    for maindir, subdir, file_name_list in os.walk(dirname):
        for filename in file_name_list:
            apath = os.path.join(maindir, filename)
            if True:        # 保存全部文件名。若要保留指定文件格式的文件名则注释该句
            #if apath.split('.')[-1] in postfix:   # 匹配后缀，只保存所选的文件格式。若要保存全部文件，则注释该句
                try:
                    with open(filelistlog, 'a+') as fo:  # 'w+'表示如果已存在就覆盖  'a+'表示追加写入
                        fo.writelines(apath)
                        fo.write('\n')
                except:
                    pass    # 所以异常全部忽略即可


if __name__ == '__main__':
    dirpath = "D:"  # 指定根目录
    all_path(dirpath)
```

10. 进行函数编程的时候：常常会给一些参数赋初始值。我们把这些初始值叫作Default Argument Values。一般情况下，我们可以很自由的给参数赋初值，而不需要考虑任何异常的情况或者陷阱。但是当你给这些参数赋值为可变对象（mutable object)，比如list，dictionary，很多类的实例时，那么你要小心了，因为函数参数的初值只能被计算一次（在函数定义的时间里）

```Python
    def bad_foo(item, my_list=[]):
    my_list.append(item)
    return my_list

    print bad_foo('a')

    print bad_foo('b')

    print bad_foo('c')

    >>>


    ['a']
    ['a', 'b']
    ['a', 'b', 'c']



```

11. 文件读写标志位以及功能说明

<table style="height: 300px; width: 1022px" border="0">
<tbody>
<tr>
<td style="text-align: center">r</td>
<td>以只读模式打开文件</td>
<td>光标在文件开头</td>
<td>如果文件不存在，则出错</td>
</tr>
<tr>
<td style="text-align: center">&nbsp; &nbsp; &nbsp; r+ &nbsp; &nbsp; &nbsp; &nbsp;&nbsp;</td>
<td>以读写模式打开文件</td>
<td>光标在文件开头</td>
<td>如果文件不存在，则出错。读写都可以移动光标。写入时，如果光标不在文件末尾，则会覆盖源文件</td>
</tr>
<tr>
<td style="text-align: center">w</td>
<td>以只写模式打开文件</td>
<td>光标在文件开头</td>
<td>如果文件不存在，则创建文件，如果文件已存在，则从文件头开始覆盖文件。如果写入内容比源文件少，则会保留未覆盖的内容</td>
</tr>
<tr>
<td style="text-align: center">w+</td>
<td>以读写模式打开文件</td>
<td>光标在文件开头</td>
<td>如果文件不存在，则会创建文件。文件已存在，从光标位置覆盖文件。读写都可以移动光标。</td>
</tr>
<tr>
<td style="text-align: center">a</td>
<td>以只写模式打开文件</td>
<td>光标在文件结尾，追加模式</td>
<td>文件不存在是，创建文件。文件存在时，打开时，光标在文件末尾，写入不覆盖源文件</td>
</tr>
<tr>
<td style="text-align: center">a+</td>
<td>以读写模式打开文件</td>
<td>光标在文件结尾，追加模式</td>
<td>文件不存在是，创建文件。文件存在时，打开时，光标在文件末尾，写入不覆盖源文件。</td>
</tr>
<tr>
<td style="text-align: center">b</td>
<td>与前面六种结合使用，以二进制方式读或者写</td>
<td>&nbsp;</td>
<td>&nbsp;</td>
</tr>
</tbody>
</table>


f.read([size])：默认一次性读入打开的文件内容。如果有size参数，则指定每次读入字符数。注意，此处按字符来读入，一个汉字为一个字符

f.readline([size])：一次读入一行文件内容

f.readlines([size])：将文件内容全部读入，保存在一个列表中，每行为一个元素。

f.writ(str,encoding=)：将str写入文件，可以指定写入的编码格式，默认为utf-8

f.writlines()

f.readable() ： 判断是否可读，返回布尔值。如果是在只写模式下打开文件， 也是返回false

f.writable()：判断是否可写

f.tell() ：  返回当前光标位置

f.seek(offset,whence=0)：将光标位置移至所需位置。offset为偏移量。whence定义开始偏移的位置。0为从文件开头偏移。1为从当前位置开始偏移。2为从文件末尾开始偏移，默认为0。注意，此处偏移量是按字节计算，也就是一个汉字最少需要两个偏移量。如果偏移量正好讲一个汉字分开，则会报错。

f.truncate(数值)   从光标位置截断/删除后面内容。

f.flush()  将内存内容立即写入硬盘

12. 使用内置iter函数用于文档读取

```Python
# 逐行读取文件直到遇到空行，或者达到文件末尾

with open('mydata.txt') as fp:
    for line in iter(fp.readline, '\n'):  # iter函数第二个参数为哨符 当可调用的对象返回这个值的时候 会触发迭代器抛出 StopIteration 异常
        print(line)
```

