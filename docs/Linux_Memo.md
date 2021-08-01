# Linux メモ

- WSLでUbuntuを使う
<https://www.atmarkit.co.jp/ait/articles/1903/18/news031.html>

## Linux コマンド

ls -ltr timestampで昇順に表示 一番最近のファイルを確認したいときによく使う
https://github.com/koki534/Memo.githttps://github.com/koki534/Memo.git
cp -p ファイルのパーミッション，所有者情報，タイムスタンプを保持　cpを使うときは基本的に-pを付ける

su - ハイフンを付けるとルートユーザ特有の設定を読み込んでくれるのでなるべく付ける　suだけでもrootユーザになれる





## Shell Script

## shell scriptとは

- Linux OSを操作するための簡易なプログラミング言語
- カーネルに対して命令する
- bash

## サポートしているシェルを一覧表示

```bash
cat /etc/shells
```

## 使われているシェルの確認

```bash
echo $SHELL
```

## 環境変数

$が環境変数であることの目印

### 環境変数の作成

```bash
export AGE=20
```

環境変数の表示
```bash
echo $age
```
## ファイル作成(viコマンドでファイル名末尾に.shを付ける)

↓ファイルの中身

```bash
#! /bin/bash #1行目でシェルの場所を指定する

echo 'Hello World' #Hello World表示
#コメント文は#を文頭に記載
exit 0 #処理終了，それ以外は異常終了
```

## 実行

```bash
chmod 755 ファイル名 #パーミッション変更
```

ファイルに実行するための権限をつける

```bash
./[ファイル名.sh] #シェルスクリプトの実行
```

## 変数

```bash
var1='変数1'
```

変数の宣言

```bash
var2=`command`  #コマンドの実行結果を変数に格納バッククォートで囲む`.var3=$(command)`も同じ
```

```bash
echo $var1
```

変数の中身を取り出す場合は$を付ける

### システム変数の表示

システム変数は初めから定義されている変数
&BASH,$BASH_VERSION,$HOME,$PWD #システム変数

## 配列

### 配列の作成

```bash
fruits=('banana''apple'grape')
```

### 配列の表示方法

```bash
echo "${fruits[@]}"  #配列の中の値を全て表示
echo "${fruits[0]}"  #インデックス0の要素を表示
echo "${!fruits[@]}" #インデックスを表示
echo "${#fruits[@]}" #配列の要素数を表示

fruits[3]='lemon' #配列のインデックス3にlemonを追加
unset fruits[2] #配列のインデックス2を削除
```

## 引数

実行する際に，シェルに渡す値

```bash
./〇〇.sh a b  #このa,bがそれぞれ第1引数，第2引数
```

```bash
echo $1 $2 $2  #第1引数，第2引数，第3引数を表示
```

```bash
echo $0  #シェルスクリプトのファイル名
```

```bash
echo $@  #全引数を表示
```

```bash
echo $#  #引数の数を表示
```

```bash
$?
```

直前に実行したコマンドの戻り値(成功の場合0)

シェルスクリプトから別のシェルスクリプトを呼び出すことも可能

## 標準入出力

- 標準入力 : ターミナルから値を読み取って，それを変数に格納すること readコマンド
- 標準出力 : ターミナル上に値を表示すること echoコマンド

```bash
read var #標準入力
echo var1 =$var  #入力結果表示
```

```bash
read -p 'var1:'var1  #文字付の標準入力
read -sp 'password:'password  #シークレットモードでの標準入力
read -a names  #配列(Array)入力
```

## ファイル出力

実行結果をファイルに書き込むこと
`>:上書き`

```bash
echo 'hello' > hello.txt  #現在いるディレクトリのhello.txtファイルを作成(上書き)してhelloを書き込む
```

`>>: 追記`  

```bash
echo 'hello' >> hello.txt  #現在いるディレクトリのhello.txtファイルを作成(追記)してhelloを書き込む
```

## if文

1. if test 条件文; 2. if [ 条件文 ]; 3. if [[ 条件文]]；

```bash
if test 条件文; or [ 条件文 ]; or [[ 条件文 ]];  #ifの条件文を満たしていない場合に実行される
then
    プログラム  #elifの条件文を満たす場合に処理が実行される
else
    プログラム  #ifもelifも満たさない場合に実行される
fi

=!!  #文字列で等しいか等しくないか
-eq(=), -ne(!=), -le(<=), -gt(>), -ge(>=)  #数値比較
```

le -> less than
ge -> greater than

### if文の and or

条件文1 and 条件文2の場合，条件文1,条件文2を両方満たす場合に実行
書き方は以下の4通り

```bash
if [ 条件文1 ] && [ 条件文 ];
if [ 条件文1 -a 条件文2 ];
if [[ 条件文1 && 条件文2 ]];
if test 条件文1 && 条件文2;
```

条件文1 or 条件文2の場合，条件文1,条件文2のどちらかを満たす場合に実行
書き方は以下の4通り

```bash
if [ 条件文1 ] || [ 条件文 ];
if [ 条件文1 -o 条件文2 ];
if [[ 条件文1 || 条件文2 ]];
if test 条件文1 || 条件文2;
```

否定を表す条件文(3つ)
if ! test 条件文, if[ ! 条件文], if [[ ! 条件文]]

```bash
cmd1 && cmd2  #cmd1が正しく実行されたらcmd2を実行
cmd1 || cmd2  #cmd1がエラーならcmd2を実行
cmd1 && cmd2 || cmd3  #cmd1が正しく実行されたらcmd2が実行されて，cmd1がエラーならcmd3を実行
```

## ファイルの存在チェック

```bash
if [ -e ファイル名 ];  #ファイルもしくはディレクトリの存在確認
then
else
fi

if [ -f ファイル名 ]  #ファイルが存在しディレクトリでなくファイル
if [ -d ファイル名 ] #ディレクトリが存在するか
if [ -s ファイル名 ];  #ディレクトリ or 中身のあるファイルか
if [ -w ファイル名];  #書き込み権限あるか
if [ -x ファイル名];  #実行権限あるか
if [ fileA -nt fileB ]  #fileAがfileBより新しいか
if [ fileA -ot fileB ]  #fileAがfileBより古いか

```bash
#!/bin/bash

if [ -e 'sample' ];
then
        echo 'sampleが存在します'
fi
```



```bash
#!/bin/bash

#if [ -e 'sample' ];  #ディレクトリ or ファイル
#if [ -d 'sample' ]  #ディレクトリ
if [ -f 'sample' ];  #ファイルの場合中の処理を実行
then
        echo 'sampleが存在します'
        rm sample
fi
```

sampleがある場合削除する



## フォルダのフラット化

https://chiilabo.com/2020/09/mac-subfolder-flatten-automator-quick-action/



下位フォルダのすべてのファイルを一箇所のフォルダに移動する操作のことを、「ディレクトリ構造のフラット化」と呼ぶ。　日本語だと検索しづらいが、「flatten folder」などと検索すると検索しやすい。



```bash
find . -mindepth 2 -type f -exec mv -i '{}' . ';'
```

`find .`　：カレントディレクトリの検索

`-mindepth 2`：検索対象を2階層に制限

`-type -f` ：検索対象をファイルに制限

`-exec <コマンド> <オプション> {} ;`：検索したファイルに対して処理をするコマンドを指定して実行

`{}` :検索結果のファイルやディレクトリのパスが入り、指定したコマンドの引数として渡される

`;`：コマンドの終了を示す。

`mv -i` :-iを付けると上書きされる場合は確認メッセージを表示 interectiveの意味



parent_dir オリジナル

parent_dir_1 上記の通り実行　狙い通り親フォルダにファイルが集まった。

parentdir_2 {}のみ''を外す  問題なし

parent_dir_3 ;のみ''を外す エラー発生 find: -exec: no terminating ";" or "+"

parent_dir_4 cd  ';'を\;に書き換える 問題なし



## 空のフォルダをまとめて削除する

```bash
find . -type d -empty -delete
```



