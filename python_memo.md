# Python メモ

## スクリプト

インポート時の実行回避

```python
if __name__ == '__main__':
```





## 例外処理 try except

エラーが出た時に、どういう処理をするかを記述する

tryの中でエラーが怒るとexceptの中が実行される

```python
try:
	result = calc_tri(base, height)
	print('計算結果{}'.format(result))
except Exception as e:
	print('計算できません')
  print(e)  # エラーの内容を見ることができる。エラーの内容を見てexceptの中身をどうするかの参考にする
```



```python
try:
  a = 10
  b = 0
  result = a/b
  except ZeroDivisionError:
    print('0で割ってます')
  finally:	# 最後にかならず行いたい処理がある場合はfinallyを使用する
    print('ここは必ず通ります')
```



## ライブラリのリロード

```python
import importlib
importlib.reload(ライブラリ名)
```





## 正規表現

```python
target = 'v='
idx = url.find(target)
video_id = url[idx+len(target):]
```



## subprocess

カレントディレクトリの表示

```python
subprocess.check_output(['pwd'])
```



## tqdm

for文の進捗を可視化する

```bash
pip install tqdm
```



インポート

```python
from tqdm import tqdm
```

使い方

````python
for i in tqdm(iterator):
````

