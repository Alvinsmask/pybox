### 1. dict default value
- 通过dict.setdefault()方法来设置默认值：

```Python
strings = ('puppy', 'kitten', 'puppy', 'puppy',
           'weasel', 'puppy', 'kitten', 'puppy')
counts = {}

for kw in strings:
    counts.setdefault(kw, 0)
    counts[kw] += 1 # 原PPT中这里有一个笔误
```
dict.setdefault()方法接收两个参数，第一个参数是健的名称，第二个参数是默认值。
假如字典中不存在给定的键，则返回参数中提供的默认值；
反之，则返回字典中保存的值。利用dict.setdefault()方法的返回值可以重写for循环中的代码，使其更加简洁：
```Python
strings = ('puppy', 'kitten', 'puppy', 'puppy',
           'weasel', 'puppy', 'kitten', 'puppy')
counts = {}

for kw in strings:
    counts[kw] = counts.setdefault(kw, 0) + 1
```
- 使用collections.defaultdict类


defaultdict类就好像是一个dict，但是它是使用一个类型来初始化的：
```Python
>>> from collections import defaultdict
>>> dd = defaultdict(list)
>>> dd
defaultdict(<type 'list'>, {})
```
defaultdict类的初始化函数接受一个类型作为参数，当所访问的键不存在的时候，可以实例化一个值作为默认值：

```Python
>>> dd['foo']
[]
>>> dd
defaultdict(<type 'list'>, {'foo': []})
>>> dd['bar'].append('quux')
>>> dd
defaultdict(<type 'list'>, {'foo': [], 'bar': ['quux']})
```
需要注意的是，这种形式的默认值只有在通过dict[key]或者dict.__getitem__(key)访问的时候才有效。
```Python
>>> from collections import defaultdict
>>> dd = defaultdict(list)
>>> 'something' in dd
False
>>> dd.pop('something')
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
KeyError: 'pop(): dictionary is empty'
>>> dd.get('something')
>>> dd['something']
[]
```

该类除了接受类型名称作为初始化函数的参数之外，还可以使用任何不带参数的可调用函数，
到时该函数的返回结果作为默认值，这样使得默认值的取值更加灵活。
下面用一个例子来说明，如何用自定义的不带参数的函数zero()作为初始化函数的参数：
```Python
>>> from collections import defaultdict
>>> def zero():
...     return 0
...
>>> dd = defaultdict(zero)
>>> dd
defaultdict(<function zero at 0xb7ed2684>, {})
>>> dd['foo']
0
>>> dd
defaultdict(<function zero at 0xb7ed2684>, {'foo': 0})
```
利用collections.defaultdict来解决最初的单词统计问题，代码如下：

```Python
from collections import defaultdict

strings = ('puppy', 'kitten', 'puppy', 'puppy',
           'weasel', 'puppy', 'kitten', 'puppy')
counts = defaultdict(lambda: 0)  # 使用lambda来定义简单的函数

for s in strings:
    counts[s] += 1
```

### 可变集合set()与不可变集合frozenset()

**set无序排序且不重复，是可变的，有add（），remove（）等方法**。既然是可变的，所以它**不存在哈希值**。基本功能包括关系测试和消除重复元素. 集合对象还支持union(联合), intersection(交集), difference(差集)和sysmmetric difference(对称差集)等数学运算.
sets 支持 x in set, len(set),和 for x in set。作为一个无序的集合，**sets不记录元素位置或者插入点。**因此，sets不支持 indexing, 或其它类序列的操作。

**frozenset是冻结的集合，它是不可变的，存在哈希值**，好处是它可以作为字典的key，也可以作为其它集合的元素。缺点是一旦创建便不能更改，**没有add，remove方法。**

- 集合的创建

  直接创建，参数必须为可迭代对象或者是迭代器
    
  可以通过以下方法创建，与创建字典类似，不过只有值没有键
    
    ```Python
           >>>s={'chessseshop','bookshop'}直接创建，类似于list的[]和dict的{}，不同于dict的是其中的值，set会将其中的元素转换为元组
           >>>s
           >>> {'bookshop', 'chessseshop'}
           >>> type(s)
           <type 'set'>
    ```

- 更新可变集合

  ```Python
           >>> s.add('z')  #添加
           >>> s
           set(['c', 'e', 'h', 'o', 'p', 's', 'z'])
           >>> s.update('pypi') #添加
           >>> s
           set(['c', 'e', 'i', 'h', 'o', 'p', 's', 'y', 'z'])
           >>> s.remove('z') #删除
           >>> s
           set(['c', 'e', 'i', 'h', 'o', 'p', 's', 'y'])
           >>> s -= set('pypi')#删除
           >>> s
           set(['c', 'e', 'h', 'o', 's'])
           >>> del s  #删除集合
  ```
- 其他逻辑操作

1. 成员关系 in, not in

2. 子集/超集 <  >= 

3. 等价/不等价 ==  ！=

4. 遍历 `for i in  set: `

5. 并集 `a|b`
   
   交集 `a&b`
   
   两个集合(s 和 t)的差补或相对补集是指一个集合 C，该集合中的元素，只属于集合 s，而不属于集合 t。差符号有一个等价的方法，difference().` s - t`
   
   异或（XOR）
