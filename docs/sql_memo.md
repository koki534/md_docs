# SQL メモ



参考　Udemy 3時間で学ぶSQL・データベース超入門

## コメントアウト

```sql
-- ハイフン２つと半角スペースでコメントが記入できる
```



## select文

テーブルからデータを抽出、取得するために用いる命令文

カラム名のところに*(アスタリスク)を書くとすべてのカラムを指定できる。

基本形

```sql
select <カラム名> from <テーブル名>;
```

カラム名には定数や式も書くことができる

```sql
select height / 100, '2018/04/01', '吉田' from members;
```



## 一部の行だけ取得するためのwhere句

書き順はselect → from → whereの順に書く

```sql
select カラム名 from テーブル名 where 条件式;
```

例

```sql
select name from members where height >= 180;
```



## レコード（行）の数を数えるためのcount関数

count関数のような集計に用いる関数を集約関数と呼ぶ。SUMやAVG、MAX、MINなどがある。

```sql
select count(*) from テーブル名;
```



## カラム名に別名をつけるASキーワード

```sql
select count(*) as "50歳以上の人数" from members where age >= 50;
```



## 昇順、降順に並べ替えて表示するORDER BY

ORDER BYで並べ替えを行っていない場合、レコードに順番という概念はない。

書き順はselect → from → where → order byの順に書く

```sql
select カラム名 from テーブル名 order by 並べ替えの基準となるカラム名;
```

例 : membersテーブルの名前(name)と年齢(age)を年齢の若い順に取得

```sql
select name, age from members order by age;
```

例 : 年齢の高い順に並べる

```sql
select name, age from members order by age;
```

## テーブルをグループに切り分けるGROUP BY

書き順はselect → from → where → group by → order byの順に書く

```python
select カラム名 from テーブル名 group by 集約キー
```

例：職種(job_id)ごとの人数をカウントする

```sql
select job_id, count(*) from members group by job_id;
```

実行の順番としては、

1. membersテーブルを、job_idが共通のもので切り分けてグループ化する

2. グループごとにjob_idカラムとcount(*)カラムを取得する



注意点①

group byや集約関数を用いるときには、select句には以下のいずれかしか書くことができない

- 定数
- group by句で指定したカラム名(集約キー)
- 集約関数

なぜなら表示する値を１つに定めることができなくなる場合があるから。

注意点②

group by句にasで命名した別名は指定できない。なぜならsqlではselect句がgroup by句よりもあとに実行されるため

注意点③

group by句に並べ替えの機能はない

## グループ化したテーブルに対して条件を指定するHAVING句

書き順はselect → from → where → group by → having → order byの順に書く。

```sql
select カラム名 from テーブル名
group by 集約キー having グループの値に対する条件式;
```

例 : job_idごとの人数を数えて2人いたjob_idだけを取得する。

```sql
select job_id, count(*) from members
group by job_id having count(*) = 2;
```



WHERE句 : レコードに対する条件指定

HAVING句 : グループに対する条件指定



## テーブルを結合するJOIN

書き順はselect → from → join→ where → group by → having → order byの順に書く。

```sql
select カラム名 from テーブル名1 inner join テーブル名2 on 結合の条件;
```

例 : 

```sql
select * from members inner join jobs on jobs.id = members.job_id;
```



ここからまだ制作していない

insert

update 

delete

## 文字列の部分一致検索をするのに便利なlike

例：TシャツとYシャツの値段を取得する

```sql
select name, selling_price from Products where name like '%シャツ';
```



between 

in

ビュー

サブクエリ

スカラサブクエリ

case

