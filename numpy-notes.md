- 求矩阵范数

范数，首先需要注意的是范数是对向量（或者矩阵）的度量，是一个标量（scalar）

```Python
help(np.linalg.norm)  # 可以查看说明文档

norm(x, ord=None, axis=None, keepdims=False)

```
**参数说明：**

 The following norms can be calculated:

    =====  ============================  ==========================
    ord    norm for matrices             norm for vectors
    =====  ============================  ==========================
    None   Frobenius norm                2-norm
    'fro'  Frobenius norm                --
    'nuc'  nuclear norm                  --
    inf    max(sum(abs(x), axis=1))      max(abs(x))
    -inf   min(sum(abs(x), axis=1))      min(abs(x))
    0      --                            sum(x != 0)
    1      max(sum(abs(x), axis=0))      as below
    -1     min(sum(abs(x), axis=0))      as below
    2      2-norm (largest sing. value)  as below
    -2     smallest singular value       as below
    other  --                            sum(abs(x)**ord)**(1./ord)
    =====  ============================  ==========================
    
- 求矩阵逆，以及行列式

```Python
np.linalg.inv()：矩阵求逆

np.linalg.det()：矩阵求行列式（标量）

```

- 数组拼接方法

1. 使用stack函数系列

这个系列的函数接受list、tuple、以及np数组,最后返回一个np数组
**np.stack**

把多元list转化为多维数组,第二个参数指示扩展的轴

**np.vstack()： 纵向拼接**
```Python
print(np.vstack([np.arange(0,6), np.arange(6,12)]))  # 它是垂直（按照行顺序）的把数组（list）给堆叠起来
>>>
array([[ 0,  1,  2,  3,  4,  5],
       [ 6,  7,  8,  9, 10, 11]])
```

**np.hstack()： 横向拼接**
```Python
a=[[1],[2],[3]]  # 列表默认以列排列
b=[[1],[2],[3]]
c=[[1],[2],[3]]
d=[[1],[2],[3]]
print(np.hstack((a,b,c,d)))  #  水平堆叠数组或者list
>>>
[[1 1 1 1]
 [2 2 2 2]
 [3 3 3 3]]
```

2.

**np.concatenate():**

**axis=1时要保证数组必须有第二个维度即shape为（m,n）而不是（m, ）**
```Python
np.concatenate((a1, a2, …), axis=0)
"""
参数解释
第一个参数为由数组构成的元组，第二个参数表示拼接方向,默认axis=0
传入的数组必须具有相同的形状，这里的相同的形状可以满足在拼接方向axis轴上数组间的形状一致即可
"""
#  以下为示例
print(np.concatenate([np.arange(0,6), np.arange(6,12)]),'\n') # 沿仅有的一个轴拼接
# 沿着垂直0轴的方向拼接，即不改变0轴的大小，横向拼接
print(np.concatenate([np.arange(0,6).reshape(1,-1), np.arange(6,12).reshape(1,-1)]),'\n') 
# 沿着垂直1轴的方向拼接，即不改变1轴的大小，纵向拼接
print(np.concatenate([np.arange(0,6).reshape(1,-1), np.arange(6,12).reshape(1,-1)], axis=1))

>>>
[ 0  1  2  3  4  5  6  7  8  9 10 11] 

[[ 0  1  2  3  4  5]
 [ 6  7  8  9 10 11]] 

[[ 0  1  2  3  4  5  6  7  8  9 10 11]]

```

- 数组沿不同轴向累加 np.cumsum

**返回给定axis上的累计和**

` numpy.cumsum(a, axis=None, dtype=None, out=None)  # Return the cumulative sum of the elements along a given axis.`
  
 对于一维输入a（可以是list，可以是array，假设a=[1, 2, 3, 4, 5, 6, 7] ，就是当前列之前的和加到当前列上，如下：
 
 ```Python
    >>>import numpy as np  
    >>> b=[1,2,3,4,5,6,7]  
    >>> np.cumsum(a)  
    array([  1,   3,   6,  10,  15,  21,  28,  36,  45,  55,  75, 105])  
 ```

2.对于二维输入a，axis可以是0(第一行不动，其他行累加)， 1(第一列列不动，其他列累加)，如下：

```Python
    >>>import numpy as np  
    >>> c=[[1,2,3],[4,5,6],[7,8,9]]  
    >>> np.cumsum(c,axis=0)  
    array([[ 1,  2,  3],  
           [ 5,  7,  9],  
           [12, 15, 18]])  
    >>> np.cumsum(c,axis=1)  
    array([[ 1,  3,  6],  
           [ 4,  9, 15],  
           [ 7, 15, 24]])  
 ```
 
 3.对于三维输入a, axis可以是 0(第0维不动，其他维累加)， 1(第1维不动，其他维累加)， 2(第2维不动，其他维累加)，注意维数从外到内是0-2编号，如下：
 
 ```Python
    >>>import numpy as np  
    >>> a  
    [[[1, 2, 3], [4, 5, 6]], [[7, 8, 9], [10, 20, 30]]]  
    >>> np.cumsum(a,axis=0)  
    array([[[ 1,  2,  3],  
            [ 4,  5,  6]],  
      
           [[ 8, 10, 12],  
            [14, 25, 36]]])  
    >>> np.cumsum(a,axis=1)  
    array([[[ 1,  2,  3],  
            [ 5,  7,  9]],  
      
           [[ 7,  8,  9],  
            [17, 28, 39]]])  
    >>> np.cumsum(a,axis=2)  
    array([[[ 1,  3,  6],  
            [ 4,  9, 15]],  
      
           [[ 7, 15, 24],  
            [10, 30, 60]]])  
 ```
