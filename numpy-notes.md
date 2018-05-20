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
