# MYSQL_python メモ

インポート

```python
import mysql.connector as mysql
```



設定

```python
config = {
        'user': 'root',
        'password': 'password',
        'host': 'localhost',
        'database': 'database_name'
        }
```



接続できるか確認

```python
try:
    dbconnector = mysql.connect(**config)

# 例外が発生の場合、エラーメッセージを表示    
except mysql.Error as errmsg:
    print('MySQLサーバーへの接続が失敗しました。')
    print('エラーメッセージ：', errmsg)
# 成功した場合、メッセージを表示、接続を切断
else:
    print('MySQLサーバーへの接続が成功しました。')
    # dbconnector.close()
```



cursorオブジェクトの生成

```python
cursor = dbconnector.cursor()
```



SQL命令を実行

```python
cursor.execute("DROP DATABASE IF EXISTS sampledb")  # データベースの作成
cursor.execute('CREATE DATABASE kanataso_db')
cursor.execute('SHOW databases')
```



実行結果データを取得、表示

```python
tuples = cursor.fetchall()
for tpl in tuples:
    print(tpl[0])
```

コミットする。（実際にMySQLに保存する）

```python
dbconnector.commit()
```

接続を切断する

```python
dbconnector.close()
```

