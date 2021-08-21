# OpenCVメモ


```python
import cv2
```

画像をNotebook上にインライン表示


```python
import cv2
from IPython.display import Image, display

def imshow(img):
    '''画像を Notebook 上にインラインで表示する。
    '''
    img = cv2.imencode('.png', img)[1]
    display(Image(img))

img = cv2.imread('image/lena.png')
imshow(img)
```


![png](Opencv_Memo_files/Opencv_Memo_3_0.png)



```python

```
