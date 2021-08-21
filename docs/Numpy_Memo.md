# Numpy 使い方メモ


```python
import numpy as np
```

# (n, )と(n, 1)の違い
(n, ) は1行n列の行列  
(n, 1)はn行1列の行列  


```python
a = np.arange(3)
print("a:",a)
print("a.shape:",a.shape)
a_T = a.reshape(3,1)
print("a_T:\n",a_T)
print("a_T.shape:\n",a_T.shape)
```

    a: [0 1 2]
    a.shape: (3,)
    a_T:
     [[0]
     [1]
     [2]]
    a_T.shape:
     (3, 1)


np.where(条件式, 真の場合の値, 偽の場合の値)



```python
a = np.arange(9).reshape(3,3)
print(a)
```

    [[0 1 2]
     [3 4 5]
     [6 7 8]]



```python
np.where(a <4, -1, 100)
```




    array([[ -1,  -1,  -1],
           [ -1, 100, 100],
           [100, 100, 100]])




```python

```
