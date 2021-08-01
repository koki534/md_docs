# sqlalchemyメモ

vol.032 データベース操作がめっちゃ楽になる！PythonのORM SQLAlchemyとは？ | 中学生でもわかるPython入門シリーズ

https://www.youtube.com/watch?v=inEqP234Dpc&t=10s

インポート

```python
from sqlalchemy import create_engine
from sqlalchemy.orm import scoped_session, sessionmaker
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy import Column, Integer, Float
```

データベースファイルのパスを作成

```python
database_file = os.path.join(os.path.abspath(os.getcwd())
, 'data.db')
```

エンジンの作成

エンジン：どのDBにどうやって接続するか、設定を行い、その設定内容を保存したもの

```python
engine = create_engine('sqlite:///' + database_file, convert_unicode=True, echo=True)
```

セッションの定義

セッション：DBとのつながりを確立してから、切断するまでの一連の流れ

```python
db_session = scoped_session(
    sessionmaker(
        autocommit = False,  # 自動でセーブするか
        autoflush = False,   # 自動で反映するか
        bind = engine
    )
)
```

データベースの基盤を作成

```python
Base = declarative_base()
Base.query = db_session.query_property()
```

テーブルの作成

```python
class Buy(Base):
    __tablename__ = "buy_class"
    id = Column(Integer, primary_key=True)  # 主キー:DBのテーブル内でレコード(行)を一意に識別することができるように指定するもの
    buy_column_0 = Column(Integer, unique=False)
    buy_column_1 = Column(Integer, unique=False)
    buy_column_2 = Column(Integer, unique=False)
    buy_column_3 = Column(Integer, unique=False)

    def __init__(self, wine_class=None, buy_column_0=None, buy_column_1=None,buy_column_2=None,buy_column_3=None):
        self.buy_columun_0 = buy_columun_0
        self.buy_columun_1 = buy_columun_1
        self.buy_columun_2 = buy_columun_2
        self.buy_columun_3 = buy_columun_3
```



データベースの初期化

```python
Base.metadata.create_all(bind=engine)
```

データベースには1行ずつ追加する

```python
def read_data():
    buy_df = pd.read_csv('')
    for index, _df in buy_df.iterrows():
        row = Buy(buy_column_0=_df['aaa'], buy_column_1=_df['bbb'], buy_column_2=_df['ccc'], buy_column_3=_df['ddd'])  # 1行取ってくる
        db_session.add(row) # 追加
    db_session.commit()  # 反映
```

READ

```python
db = db_session.query(Buy).all()
for row in db:
    print(row.buy_class_0)

db = db_session.query(Buy.column1, Buy.column2).all()
# db = db_session.query(Buy.column1).limit(20).all()  量を制限

# order by
from  sqlalchemy import desc
db = db_session.query(Buy).order_by(desc(Buy.buy_class_0))
```

CREATE

```python
Wine(wine_class=1, alchol=1, ash=1, proline=1)
db_session.add(wine)
db_session.commit()

db = db_session.query(Wine).filter(Wine.proline==1).all()
```

UPDATE

```python
db.wine_class = 10
db_session.commit()
```

DELETE

```python
db_session.query(Wine).filter(Wine.proline == 1).delete()
```



engineの作り方簡単バージョン

```python
db_file = os.path.join(os.path.abspath(os.getcwd())
, 'database_name.db')

e = create_engine('sqlite:///' + db_file)

q = 'select title from any_table limit 5'

res = e.execute(q)
res.fetchall()
# dir(res)
# list(res) # 1レコード1タプルのリストになる

# res = e.execute(q)
# for row in res:
#     print(row['title'])

```

typo
