# MySQLメモ

## mysqlをローカルに環境構築

https://prog-8.com/docs/mysql-env

macにmysqlが入っているか確認

```bash
mysql --version
```

MySQLのインストール

```bash
brew install mysql@5.7
```

パスを通す インストールが完了した際に表示される

```bash
echo 'export PATH="/usr/local/opt/mysql@5.7/bin:$PATH"' >> ~/.zshrc
```

ちゃんと入力されているかzshrcを開いて確認すると良い



変更を反映

```bash
sosurce ~/.zshrc
```

MySQLが入っているか確認 

```bash
mysql --version
```



MySQLの起動 

```bash
brew services start mysql@5.7
```

パスワード等の設定 

```bash
mysql_sequre_installation
```

MySQLにログイン 

```bash
mysql --user=root --password
```

ログアウト

```mysql
exit
```

MySQLの停止 

```bash
brew services stop mysql@5.7
```

## MySQL内での主な操作

存在するユーザの確認

```mysql
select host,user from mysql.user;
```



データベースの確認

```mysql
show databases;
```

データベースの作成

```mysql
CREATE DATABASE <データベース名>
```

使用するデータベースの選択

```mysql
use <データベース名>
```

テーブルの一覧表示

```mysql
show tables;
```



テーブルの作成(一例)

```mysql
CREATE TABLE test(
			id INTEGER NOT NULL AUTO_INCREMENT,
			title VARCHAR(20) NOT NULL,
			PRIMARY KEY(id)
			);
```

データを1行追加

```mysql
INSERT INTO test
	(
		id,
		title
	)
	VALUES
	(
		1,
		'ClassMethod'
	);
```

結果の確認

```mysql
SELECT * FROM test;
```

テーブル構造の確認

```mysql
DESCRIBE test
```





// Add the IDs of extensions you want installed when the container is created.

​    "extensions": [

​        "ms-python.python",

​        "ms-azuretools.vscode-docker"

​    ]



docker内では既にmysqlが立ち上がっており、

```bash
mysql -u root -p
```

で入れる

vscodeのmysqlの拡張機能が便利



https://hub.docker.com/_/mysql?tab=description

