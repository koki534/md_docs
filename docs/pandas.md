# Pandas

### DataFrame 読み書き

```  python
df = read_csv('train.csv')
```



### データ構造

```python
df.shape
```



### 列のすべての要素を一括操作

```python
df['age_group'] = df['age'].apply(lambda x: str(x)[0] + '0s')
```






### 該当文字列要素のインデックスの取得

```python
mask = df.index[df['column name'] == 'strength']
```



### 列に含まれている要素の名前を取得

```python
df[columns].unique()
```



### Pandas Profiling

```python
import pandas_profiling
df.profile_report()
```



### 欠損値

##### NAN集計
```python
df.isna().sum()
```



##### NANを平均で埋める

```python
df.fillna(df.mean(), inplace=True
```



### 統計処理
- 尖度(Kurtosis)
        - フィッシャーの定義

```python
pd.kurtosis()
```



### メモリ使用量削減

DataDrame内の数字を，適切なint型,float型に変えることでメモリ使用量を削減できる

```python
def reduce_mem_usage(df, verbose=True):
	numerics = ['int16', 'int32', 'int64', 'float16', 'float32', 'float64']
	start_mem = df.memory_usage().sum() / 1024**2    # 合計メモリ使用量の確認
	for col in df.columns:
		if col!='open_channels':
			col_type = df[col].dtypes   #dtypeの取得
			if col_type in numerics:
        c_min = df[col].min()
        c_max = df[col].max()
        if str(col_type)[:3] == 'int':
					if c_min > np.iinfo(np.int8).min and c_max < np.iinfo(np.int8).max:  # np.iinfoでintのサイズを# 確認できる
					df[col] = df[col].astype(np.int8)
          elif c_min > np.iinfo(np.int16).min and c_max < np.iinfo(np.int16).max:
          df[col] = df[col].astype(np.int16)
          elif c_min > np.iinfo(np.int32).min and c_max < np.iinfo(np.int32).max:
          df[col] = df[col].astype(np.int32)
          elif c_min > np.iinfo(np.int64).min and c_max < np.iinfo(np.int64).max:
          df[col] = df[col].astype(np.int64)  
				else:
          if c_min > np.finfo(np.float16).min and c_max < np.finfo(np.float16).max:
          df[col] = df[col].astype(np.float16)
          elif c_min > np.finfo(np.float32).min and c_max < np.finfo(np.float32).max:
          df[col] = df[col].astype(np.float32)
          else:
          df[col] = df[col].astype(np.float64)    
  end_mem = df.memory_usage().sum() / 1024**2
  if verbose: print('Mem. usage decreased to {:5.2f} Mb ({:.1f}% reduction)'.format(end_mem, 100 * (start_mem - end_mem) start_mem))
  return df
```



