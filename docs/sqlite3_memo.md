# sqlite3 メモ

インポート

```python
import sqlite3
```

データベースに接続

```python
db_name = 'データベース名'
conn = sqlite3.connect(db_name)
```



データベースを操作するためのカーソルの作成

```python
c = conn.cursor()
```

実行するクエリの入力

```python
query = 'select * from <テーブル名>'
```

クエリの実行

```python
c.execute(query)
```

実行結果を1行取り出す

```python
c.fetchone()
```

for文を使って順番に取り出す

```python
for row in c.fetchall():
    print(row)
```



実行結果をすべて取り出す

```python
c.fetchall()
```



データベースとの接続を解除する

```python
conn.close()
```

テーブルの削除

```sql
drop table テーブル名;
```





like句の中に変数を入れる

```python
search_word = '%<検索したい文字列>%'

query = 'select video_num, start_time, end_time, video_id, title,caption from ' + table_name + \
    " where caption like '{}'".format(search_word)
  
  c.execute(query)
```





## データベースがロックされているか確認

以下のコマンドを入力するとError: database is lockedと表示されればlockされている

```bash
sqlite3 <database名.db> "vacuum;"
```

ロックの解除

名前を変更　コピー　名前を戻す　余分なファイルを削除する

```bash
mv database.db database_temp.db
```

```bash
cp -p database_temp.db database.db
```

```bash
rm database_temp.db
```

