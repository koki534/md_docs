# docker メモ

ログインする

```bash
docker login
```

dockerhubからイメージを引っ張ってくる

```bash
docker pull <image>
```

## 基本操作

イメージの一覧を表示

``` bash
docker images
```

コンテナを作る

```bash
docker run <image_id>
```

コンテナの一覧を表示(アクティブなコンテナだけ)　ps: process status

``` bash
docker ps
```

コンテナの全表示

```bash
docker ps -a
```

コンテナ内でbashを開き、コンテナに命令が出せる状態にする

```bash
docker run -it <image> bash
```

コンテナをrestartして，再度コンテナを起動する

```bash
docker restart <container ID>
```

起動されたコンテナに入る

```bash
docker exec -it <container> bash
```

detachするにはctrl + p + q

基本的にdetachよりもexitを使う

コンテナ名を指名してrun

```bash
docker run --name <name> <image>
```

コンテナを更新内容を含めてdocker imageにする

```bash
docker commit [container ID] [new image]
```

## 削除

イメージの削除

``` bash
docker rmi <image>
```

コンテナの削除。upになっているコンテナには使えない

```bash
docker rm <container>
```

- exitedになっているコンテナはたいてい用済みなので削除する

コンテナの停止

```bash
docker stop <container>
```

コンテナの全削除

```bash
docker system prune
```

## detachモードとforegroundモード

コンテナを起動後にdetachする(バックグラウンドで動かす)

```bash
docker run -d <image>
```

コンテナをExit後に削除する(1回きりのコンテナ)(short-term foreground mode)

```bash
docker run --rm <image>
```

コンテナに入っているときに`command+t`を押すと新しいタブでローカルのターミナルを開くことができる。

## Dockerfile

docker context  : Dockerfikeのあるディレクトリのこと

docker contextの中にDockerfileやdocker-compose.ymlを配置する。

docker contextの中でbuild等を行うと便利。

## Dockerfileの記述

コードはホスト側に持つ

例：ubuntu

```Dockerfile
FROM ubuntu:latest
RUN apt-get update && apt-get install -y \
	sudo \  # ubuntu上でルートユーザ以外がルート権限でコマンドを打つときに使える
	wget \  # インターネットからダウンロード(HTTPを使う)
	vim  # エディタ
WORKDIR /opt
RUN wget https://repo.continuum.io/archive/Anaconda3-2019.10-Linux-x86_64.sh
```

例：ruby

```dockerfile
# FROMでベースとなるイメージを選択
FROM ruby:2.5  
# RUNで実行するLinuxコマンドを与える。RUNコマンド１つごとにレイヤーが作成される
RUN apt-get update && apt-get install -y \
		build-essential \
		libpq-dev \
		nodejs \
		postgresql-client \
		yarn
WORKDIR /product-register

# COPYはbuild context内のファイルをImage内に追加する COPY <ファイル名> <コピー先ディレクトリ>
COPY Gemfile Gemfile.lock /product-register/
RUN bundle install
```



## docker-compose

コンテナをバックグラウンドで立ち上げる

```bash
docker-compose up -d
```

こっちになっているかもしれない

``` bash
docker compose up -d
```

コンテナの確認 ymlに書かれた設定も表示してくれるので便利

```bash
docker-compose ps
```



コンテナの中に入る

```bash
docker-compose exec <service名> bash
```

コンテナを削除する

```bash
docker-compose down
```



すでにイメージがあるとdownしてもそれを使われてしまうので、docker imagesで確認する。強制的に新しくイメージを作りたい場合は`--build`をつける

```bash
docker-compose up --build -d
```



## docker-composeの記述



```docker-compose.yml
# docker-composeのバージョン指定
version: '3'

# dockerのボリュームを作る。
# コンテナのデータをホスト側で持っておきたいし
# 他のコンテナとも共有したい時に使う
volumes:
  db-data:
  
services:
  web:
    build: .
    ports:
      - '3000:3000'
    volumes:
      - '.:/product-register'
      # 環境変数
    environment:
      - 'DATABASE_PASSWORD=postgres'
    tty: true  # コマンドの-itの部分
    stdin_open: true # コマンドの-itの部分
    # webのコンテナは指定したdbコンテナを立ててから建てる
    depends_on:
      - db
    # webコンテナがdbにアクセスできるようにする
    links:
      - db
      
  db:
    image: postgres
    volumes:
      - 'db-data:/var/lib/postgresql/data'
# host名がservice名
# linksでwebからdbにアクセスできるようになる
```





## docker volume

ホストにフォルダが作られているわけではなく、VMとしてフォルダが形成される。dockerで管理することで他のコンテナから使うときにも簡単にアクセスできる。

docker volumeの一覧表示

```bash
docker volume ls
```



docker volumeの情報を取得

```bash
docker volume inspect <volume name>
```



## Dockerfileをbuildし，Docker imageを作る

Dockerfileのあるディレクトリ(docker context)に移動する

Dockerfileからイメージを作成

```bash
docker build .
```

IDやタグのないイメージ(ダングリングイメージ)をフィルタリング

```bash
docker images f dangling=true
```



## Layer数を最小限にする

- ベストプラクティス : Docker imageのLayer数は，最小限にする

- Layerを作るのは，RUN，COPY,ADDの3つ

- コマンドを&&でつなげる
- バックスラッシュ(\)で改行する

``` dockerfile
FROM ubuntu:latest  
RUN apt-get update && apt-get install \  
xxx \  
xyz \  
yyy \  
zzz  
```



## Dockerfileを作るときは，キャッシュをうまく活用する

- RUNのキャッシュは，ホストに保存されている

- apt-get install -y というようにちゃんと　-yをつける．Dockerfileはインタラクティブではないから

- RUNごとにキャッシュが保存される

- Dockerfileをメンテナンスしていくときは，RUNを小分けして実行して，キャッシュを利用する

- 最後にまとめてRUNを1行にまとめる．

## CMD

- CMD  : コンテナのデフォルトコマンドを指定

- CMD ["executable", "param1","param2"]
    - executable  :  実行可能なコマンド
    - paramはあれば書く

- 原則Dockerfile の最後に1つだけ記述

- CMD["/bin/bash"]

## RUNとCMDの違い

- RUNはLayerを作る.CMDは作らない

- Image Layerとして保存したい  -> RUN

- 起動時に実行したい -> CMD

## docker buildは何をしているか？
-> 指定されたフォルダをdocker daemonに渡す

- dockerのフォルダ-> build context

## Docker daemonとは？
-> docker objectの管理をする

- 我々はdocker daemonにコマンドを渡していた

## build contextとは？



- ADDやCOPYでbuild contextの中にあるファイルをimageに持っていける

## COPY
-> build context内のファイルをImage内に追加

- COPY something /new_dir/
- COPY [src] [dest]
    - src : source
    - dest : destination

- docker build [build context]

## COPY vs. ADD

- 単純にファイルやフォルダをコピーする場合はCOPYを使う
- tarの圧縮ふぁいるをコピーして解凍したいときはADDを使う

- ADD [src] [dest]  -> 解凍した形でImage内に追加される

- du  -> disc usage  ファイルサイズの確認

## Dockerfileがbuild contextにない場合

- docker build -f [dockerfilename] [build context]

- dockerfileを複数用意することがある
    - ex. 機械学習の学習用と推論用など
    - ex. Dockerfile.dev, Dockerfile.text

## CMD vs ENTRYPOINT

- ENTRYPOINTでもデフォルトコマンドを指定できる

- ENTRYPOINTはrun時に上書きできない

- ENTRYPOINTがある場合は，CMDは["param1","param2"]の形をとる．つまりENTRYPOINTで指定したコマンドの引数

- RUN時に上書きできるのはCMDの部分のみ

- コンテナをコマンドのようにして使いたいときに使う

- オプションが上書きできるようになる(RUN時)

## ENV  :  環境変数を設定

- ex. pathを通す

- 環境変数  :  OS上で動くあらゆるプロセスが情報を共有するために使う変数

- ENV [key] [value]

- ENV [key] [value]

## WORKDIR  :  Docker instruction の実行ディレクトリの変更

- RUNは全てルート直下で実行される．(たとえcdで移動したとしても) -> WORKDIRを使う！

- WORKDIR [絶対path]  -> フォルダがなかったら自動で作成される

## ホストとコンテナの関係を理解する

- ファイルシステムの共有  -> コンテナからホストへのアクセス

- ファイルへのアクセス権限

- ポートをつなげる

- コンピュータリソースの上限

これらを学んでいく

- 全てdocker run コマンドの時にオプションで設定

## -v オプションを使ってファイルシステムを共有する

- -v [host]:[container]  -> オプションで，ホストのファイルシステムをコンテナにマウントする

- docker run -it -v ~/Desktop/mounted_folder:/new_dir [image] bash

- マウントしたらHOST側にあって，コンテナにはない

- マウントしたらHOST側ファイルを更新すると，コンテナのものにも反映される

## -u オプションを使ってホストとコンテナのアクセス権限を共有する

- -u [userid]:[group id]  -> ユーザIDとグループIDを指定してコンテナをrunする

- id-u  -> ユーザID確認
- id-g  -> グループID確認

- docker run -it -u [ユーザID]:[グループID]
    - ホストのユーザIDとグループIDをシェアする
    - [ユーザID]:[グループID]は　$(id_u):$(id-g)というように，ハードコーディングじゃなくて，コマンドを打ち込むと良い
    - I have no name! というユーザ名で問題ない

## -p [host_port]:[container_port]  -> ホストのポートをコンテナのポートにつなげる

- publishする　-> ホストのポートとコンテナのポートをつなげる

- docker run -it -p 8888:8888 --rm jupyter/datascience-notebook bash

- $jupyter notebook

- 自分のHOSTからポートにアクセス  -> ローカルホスト

## コンテナで使えるコンピュータリソースの上限を設定する

- --CPUs [# of CPUs]  :  コンテナがアクセスできる上限のCPUを設定

- --memory [byte]  :  コンテナがアクセスできる上限のメモリを設定

- 物理コア数の確認

- 論理コア数の確認

- メモリ量の確認

1 K byte = 1024 byte

1 M byte = 1024 * 1024 byte

1 G byte = 1024 *1024 *1024 byte

- docker run -it --rm --cpus 4 --memory 2g ubuntu bash

- docker inspect [container]  : コンテナの情報表示
- docker inspect [container] | grep -i cpu
    - -i : ignoreの意味　大文字だろうと小文字だろうと表示





## Anaconaのインストール(コンテナ内での作業)

- .shのファイル  : シェルスクリプト
    - shコマンドで実行

- /opt のフォルダにanacondaをインストール
    - /opt : 実務でもよく使う．ルート以外のディレクトリの方がなにかと都合がいい

- anacondaにpathを通す　(通さないとPythonが使えない)

    - 環境変数$PATHにパスを追加することで，コンピュータがプログラムをそのパスから探してきてくれるようになる．

    - 1. $ echo $PATH  :  今どこにパスが通っているか確認する
    - 2. $ export PATH=/path/to/something:$PATH  でpathを追加する



- -x  : オプションの一覧表示  インタラクティブな操作を回避するコマンドを調べる
    - ex. sh -x anaconda~~.sh

## Dockerfileの続きを書く

- インストーラの削除

    - rm -f anaconda3_2019~~.sh

- jupyter labの実行

```
CMD ["jupyter","lab","--ip=0.0.0.0","--allow-root","--LapApp.token=''"]
```
- ["--ip=0.0.0.0"]  :  ローカルホスト
- ["--allow-root"]  :  ルートユーザでの実行を許す
- ["--LapApp.token=''"]  :  パスワード無し，シングルクオーテーション2つにダブルコーテーション1つ

## file system の共有

- docker run -p 8888:8888 -v ~/Desktop/:/work --name my-lab[image]
- 8888:8888  -> [ローカルホストのポート]:[コンテナのポート]

















## データサイエンスの環境をAWSに作る

## AWS

- インスタンスは使わないときは必ずSTOPする！！ -> RUNNINGときに課金

## AWSのインスタンスにSSHでアクセスする

- 鍵へのアクセス権限を厳しくする
    - chmod 400 hoge.pem

## AWSのインスタンスにDockerをセットアップする
- sudo apt-get update
- sudo apt-get install docker.io

そのままだとdockerへのアクセス権限がないので，

- sudo gpasswd -a ubuntu docker -> dockerというユーザグループを作ってそこにユーザ名ubuntuを入れる

## DockerHubにリポジトリを作成する

- 1つのリポジトリに1つのイメージ
- タグ別にいろんなバージョンが保存される
- Repository名:tag名  -> IMAGE名
- イメージ名とリポジトリ名が一致している必要がある！

## Docker imageを別名で保存する

- Dockerは，イメージをpushするときに，push先をイメージの名前を見て判断する

- docker tag [source] [target]

- ex. docker tag ubuntu:updated [username]/my-first-repo

- イメージIDは同じ  -> 同じイメージに複数のタグがついた状態

## DockerHubにDocker imageをpushする

```bash
docker push <イメージ名>
```

新しいイメージがpushされ，すでにDockerHubにあるubuntuリポジトリにあるubuntuイメージはマウントされる



- DockerHubのリポジトリのページのtagページにpullコマンドをコピーできるものがある．

## Docker run は何をしているのか

- run = create + start

  - start  -> デフォルトコマンドを実行する

- docker create [イメージ名]  -> コンテナを作るだけ(statusはCreated)

- docker start [コンテナID]

  - コンテナがupされ，デフォルトコマンドを実行してexit
  - これだけだとデフォルトコマンドの実行結果が表示されない

- docker start -a [コンテナID]
  - -a  : attachの意味

  - デフォルトコマンドの実行結果も府王子される

- docker run -it hello-world {bashやls}

  - {bashやls}でデフォルトコマンドの上書きができる

## コマンドの上書き

- /bin/bashとbashは同じ
- docker run ubuntu pwd　のように書ける
- デフォルトコマンドが何かをdocker run 〇〇 で確かめることが重要

## -it って何をしてるの？

- -i  :  インプット可能になる　インタラクティブになる　-> ホストからコンテナへの入力チャネルを開く
  - チャネル : 3種類ある　STDIN STDOUT STDERR

- -t  :  表示が綺麗になる

## 



