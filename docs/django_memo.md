# Django メモ



## 仮想環境の作成

ディレクトリの作成

```bash
mkdir [プロジェクト名]
```



カレントディレクトリを作成したディレクトリに移動させる

```bash
cd [プロジェクト名]
```

次のコマンドを入力して，venvを使って仮想環境を作成する

```bash
python3 -m venv myvenv
```

次のコマンドを入力して仮想環境を起動する

```bash
source myvenv/bin/activate
```

次のコマンドを入力して実行し，pip を最新バージョンにする

```bash
python -m pip install --upgrade pip
```

次のコマンドでrequirementsファイルを作成する

```bash
touch requirements.txt
```

`requirements.txt`をエディタで開き，以下のテキストを追加して保存する

```bash
Django~=2.2.4
```



次のコマンドを実行してDjangoをインストールする

```bash
pip install -r requirements.txt
```







## プロジェクト作成

プロジェクトの骨格を作成

```bash
django-admin startproject mysite .
```

最後の . はカレントディレクトリを表す



タイムゾーンの設定 : 正しい時刻が表示されるようになる

settings.pyを開きTIME_ZONEの行を探し，

```python
TIME_ZONE = 'Asia/Tokyo' 
```

に書き換える



言語コードの設定 : デフォルトのボタンや通知が設定した言語に変更される

settings.pyを開き，LANGUAGE_CODEの行を探し，次のように書き換える

```python
LANGUAGE_CODE = 'ja'
```



静的ファイルの追加

ファイルの一番下に移動し，`STATIC_URL`の下に`STATIC_ROOT`を追加する

```python
STATIC_ROOT = os.path.join(BASE_DIR, 'static')
```



デプロイして使うhostnameの設定

settimgs.pyのファイル内で，ALLOWED_HOSTSの行を探し，次のように書き換える

```python
ALLOWED_HOSTS = ['127.0.0.1', '.pythonanywhere.com']
```



## データベースのセットアップ

データベースの設定は`mysite/settings.py`ファイルの中の`DATABASES ={}`の中に既に書かれている．

ターミナルで次のコードを実行する

```bash
python manage.py migrate
```



## WEBサーバの起動

次のコマンドを実行する

```bash
python manage.py runserver
```

ブラウザでアドレスバーに次のアドレスを入力して開く

`http://127.0.0.1:8000/`

ターミナルは一度にひとつのコマンドしか実行できないので，WEBサーバが動いている間は新たなウインドウでターミナルを開き，仮想環境を起動する．



## オブジェクト指向

実際の物を，プロパティ(状態)と命令(メソッド)を持つコードで表現するという考え方



## Djangoモデル

Djangoのモデルはデータベースに格納される．データベースはデータの集まりであり，ここにユーザーやブログポストの情報を格納する．データを格納するのにSQLiteを使う．これはDjangoのデフォルトのデータベースである．データベースの中のモデルは，列と行があるスプレッドシートと思っても問題ない．



## 新しいアプリケーションの作成

次のコマンドを実行する

```bash
python manage.py startapp blog
```

`blog`ディレクトリが作成され，中に沢山のファイルが格納されている状態になる．

アプリケーションを作ったら，　Djangoにそれを使うように伝えないといけない．

`mysite/settings.py`を開き，`INSTALLED_APPS`を見つけ，[ ]内に次の記述を追加する．

```python
'blog.apps.BloConfig',
```



## ブログポストモデルの作成

`blog/models.py`に以下のコードを記述し，保存する．

```python
from django.conf import settings
from django.db import models
from django.utils import timezone


class Post(models.Model):
    author = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.CASCADE)
    title = models.CharField(max_length=200)
    text = models.TextField()
    created_date = models.DateTimeField(default=timezone.now)
    published_date = models.DateTimeField(blank=True, null=True)

    def publish(self):
        self.published_date = timezone.now()
        self.save()

    def __str__(self):
        return self.title
```

`models.Model`はポストがDjango Modelだという意味で，Djangoがこれはデータベースに保存すべきものだとわかるようにしている．

`def publish(self)`はブログを公開するメソッド

## データベースにモデルのためのテーブルを作成する

まず，モデルに少し変更があったことをDjangoに知らせる．ターミナルで次のコードを実行する

```bash
python manage.py makemigrations blog
```

上のコマンドでDjangoが作った移行ファイルをデータベースに追加するてめに，以下のコードをターミナルで実行する．

```bash
python manage.py migrate blog
```



## Django admin

作成したポストを追加，編集，削除するのにDjango adminを使う

`blog/admin.py`をエディタで開き，内容を以下のコードに書き換える．

```python
from django.contrib import admin
from .models import Post

admin.site.register(Post)
```



ターミナルで次のコマンドを実行する．

```bash
python manage.py runserver
```



ブラウザで`http://127.0.0.1:8000/admin`を開くとログインページが表示されることを確認する．

ログインするためには，`superuser`を作る必要がある．ターミナルで次のコマンドを実行する．

```bash
python manage.py createsuperuser
```

ターミナルの表示に従って，Username, Email address Passwordを入力する．

Postsをクリックして移動し，２つか３つ記事を作る．

## デプロイ

サーバープロバイダとして`PythonAnywhere`を使用する

GitHubも使う．GitHubはコードのホスティングサービス

`djangogirls` ディレクトリで以下のコマンドを実行する

```bash
git init
```

これでgitはこのディレクトリ内のすべてのファイルとディレクトリの変更を追跡する．

追跡してほしくないファイルを指定するには，ベースディレクトリ内で`.gitignore`ファイルを作成して設定する．

```bash
touch .gitignore
```

エディタで`.gitignore`を開き次のコードを打ち込む．

```text
*.pyc
*~
/.vscode
__pycache__
myvenv
db.sqlite3
/static
.DS_Store
```



`git status`コマンドでブランチの状態などのさまざまな情報を確認できる

変更内容を保存するには次の２つのコマンドを順に実行する

```bash
git add --all .
git commit -m 'My Django Girls app, first commit'
```





`db.sqlite3`はローカルデータベース．標準的なウェブプログラミングでは，ローカルのテストサイトとPythonAnyWhere上の本番のウェブサイトでデータベースを分ける監修がある．通常はウェブサイト上ではMySQLというSQLiteよりも多くの訪問者に対処できるデータベースを使用する．

GitHubでリポジトリを作る．新しいリポジトリを作り，名前を'my-first-blog'にする．publicで，READMEで初期化する．

次のコマンドでリモートリポジトリにプッシュする

```bash
git remote add origin https://github.com/koki534/my-first-blog.git
git push -u origin master
```



## Django secret keyについて



https://medium.com/@aadibajpai/deploying-to-pythonanywhere-via-github-6f967956e664

http://www.codingwadditude.com/2020/11/the-secretkey-to-django-girls-tutorial.html

https://medium.com/chingu/an-introduction-to-environment-variables-and-how-to-use-them-f602f66d15fa

https://docs.djangoproject.com/en/3.1/howto/deployment/checklist/

https://www.django-academy.net/lecture/95/52/

https://itstudio.co/2020/09/23/11066/

シークレットキー　ユーザのパスワードの設定において使われる ユーザのパスワードは一度決めると管理者側からも見ることができない 見ることができないものを管理する パスワードを作る時にシークレットキーの情報を使ってパスワードを作る 一方でこれが知られるとこれを元にパスワードを作ってることがわかってしまうのでデプロイする場合には第三者には知られないようにする

WSGI : サーバとWEBアプリケーションをつなぐ共通のインターフェースをPythonで定義したもの







## PythonAnywhere APIトークンの作成

PythonAnywhereでアカウントを作成し，右上にあるAccountをクリックする

API tokenというタブを選んで，「Create new API token」をクリックする

## PythonAnywhereでサイトを設定する

上のDashboardをクリックし，New console欄の「Bash」をクリックする

PythonAnywhereにWebアプリケーションをデプロイするには、コードをGitHubからプルし、PythonAnywhereがそれを認識してWebアプリケーションのサーバを動かし始めるように設定する必要があります。

次のコマンドを開いたBashターミナルに打ち込んで実行する

```bash
pip3.6 install --user pythonanywhere
```



次のコマンドをBashターミナルに打ち込み，GitHubからアプリを自動的に構成するためのヘルパーを実行する( `https`以降は自分のGitHubからクローンするときのURL)

```bash
pa_autoconfigure_django.py --python=3.6 https://github.com/<your-github-username>/my-first-blog.git
```

※ --nukeオプションを付けると上書きできる

GitHubのデフォルトのブランチがmainなのでmasterに切り替える．settingを開き branches Default の欄のbranchを選択し，masterに切り替える 

仮想環境はcondaではなくてvirtualenvを使う必要がある

以下のコマンドを入力し，管理者アカウントを初期化する

```bash
python manage.py createsuperuser
```



これでwebサイトが表示できるようになったので，　PythonAnywhereのwebページをクリックして，リンクを開く



## Django URL

Djangoは`リクエストされたURL`と`URLconf内のパターン`を照合し，適切なビューを見つける．

`mysite/urls.py`の中に`urlpatterns`があるので，そこにURLとビューのパターンを入力する．

### ブログの入り口ページをブログポストのリストページに変更する

`mysite/urls.py`の本文を以下のように書き換える．include関数でblog.urlsにリダイレクトするように設定している

```python
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('blog.urls')),
]
```



### blogのURL

blogディレクトリの中に，urls.pyという空のファイルを作る

```bash
touch urls.py
```

url.pyの中に以下のように書く

```python
from django.urls import path
from . import views  # blogアプリのすべてのビューをインポート
```

次に以下のURLパターンを追加する．ここではpost_listという名前のビューをルートURLに割り当てている．

```python
urlpatterns = [
    path('', views.post_list, name='post_list'),
]
```



## Django ビュー

ビューはアプリのロジックを書いていくところ．ビューはモデルに情報を要求し，それをテンプレートにわたす．

### blog/views.py

`blog/views.py`を開き，以下のコードを追加する．

```python
def post_list(request):
    return render(request, 'blog/post_list.html', {})
```

renderがテンプレートを組み立てる．

## HTMLテンプレート

blogディレクトリの中に，次のようにディレクトリを２つ作成する

```
blog
└───templates
    └───blog
```

`blog/templates/blog`の中に`post_list.html`ファイルを作成する．

```bash
touch post_list.html
```

これでwebページに白紙のページが表示される．

`post_list.html`に以下のコードを書く

```html
<html>
<body>
    <p>Hi there!</p>
    <p>It works!</p>
</body>
</html>
```



### HeadとBody

- head : ページ設定に関する要素で，画面には表示されない
- body : Webページの一部として表示されるすべてを含む要素

`post_list.html`に次のように<head>要素を加える．保存して更新すると，ブラウザにタイトルが表示される

```html
<html>
    <head>
        <title>Ola's blog</title>
    </head>
    <body>
        <p>Hi there!</p>
        <p>It works!</p>
    </body>
</html>
```



いろんな要素をまとめた以下のテンプレートを貼り付ける．

```html
<html>
    <head>
        <title>Django Girls blog</title>
    </head>
    <body>
        <div>
            <h1><a href="/">Django Girls Blog</a></h1>
        </div>

        <div>
            <p>published: 14.06.2014, 12:14</p>
            <h2><a href="">My first post</a></h2>
            <p>Aenean eu leo quam. こんにちは！ よろしくお願いします！ </p>
        </div>

        <div>
            <p>公開日: 2014/06/14, 12:14</p>
            <h2><a href="">2番目の投稿</a></h2>
            <p> こんにちは！ よろしくお願いします！ </p>
        </div>
    </body>
</html>
```



githubでプッシュする

pythonanywhereのコンソールを開き，`git pull`する

filesタブから，ファイルが正しくアップロードできているか確認できる

webページに移動し，リンクから自分のサイトに行き，表示を確認する



## DjangoのORMとクエリセット



### Django shell

ローカルのターミナルを開いて，次のコマンドを入力してDjangoのインタラクティブコンソールを起動する.pythonだけが起動したように見えるが，Djangoも動いている

```bash
python manage.py shell
```



### ポストデータの表示

Django shellに次のコマンドを打ち込む

```python
from blog.models import Post
Post.objects.all()
```

### オブジェクトの作成

登録されているユーザを確認する．

```python
User.objects.all()
```

ユーザのなかから　作成したスーパーユーザを以下のコマンドを使って取り出しておく`<username>`には自分で作ったスーパーユーザの名前に変更する

```python
from django.contrib.auth.models import User
me = User.objects.get(username = '<username>')
```



データベースに新しいPostを作成するには次のようにコマンドを入力する

```python
Post.objects.create(author=me, title='Sample title', text='Test')
```

```python
Post.objects.all()
```



## オブジェクトの抽出

一部のポストを取り出す場合は`Post.objects.filter()`を使う

```python
Post.objects.filter(author=me)
```

titleフィールドでフィルターする場合

```python
Post.objects.filter(title__contains='title')
```



公開済みの全ポストを取り出す

```python
from django.utils import timezone
Post.objects.filter(published_date__lte=timezone.now())
```

公開するポストを決める

```python
post = Post.objects.get(title="Sample title")
```

公開する

```python
post.publish()
```

## オブジェクトの並び替え

追加した順

```python
Post.objects.order_by('created_date')
```

逆順

```python
Post.objects.order_by('-created_date')
```



メソッドをつなげて実行する(メソッドチェーン)

```python
Post.objects.filter(published_date__lte=timezone.now()).order_by('published_date')
```



プロンプトを閉じる

```python
exit()
```



## テンプレート内の動的データ

ビューファイルにモデルとテンプレートの関連付けを記述する．

`blog/views.py`をエディタで開き，次の行を追加する．

```python
from .models import Post
```



## クエリセット

Django ORM = クエリセット

公開したブログ記事を`published_date`で並べ替える.Django shellを開き，以下のコマンドを実行する

```python
Post.objects.filter(published_date__lte=timezone.now()).order_by('published_date')
```



`blog/views.py`の中を次のコードに書き換える．

```python
from django.shortcuts import render
from django.utils import timezone
from .models import Post

# Create your views here.

def post_list(request):
    posts = Post.objects.filter(published_date__lte=timezone.now()).order_by('published_date')
    return render(request, 'blog/post_list.html', {})
```



次はクエリセットを参照している変数`posts`をテンプレートに渡す作業．

renderの中の{}に`'posts': posts`を入力する．最初の`'posts'`が名前で，後ろの`posts`がクエリセットの値

```python
from django.shortcuts import render
from django.utils import timezone
from .models import Post

def post_list(request):
    posts = Post.objects.filter(published_date__lte=timezone.now()).order_by('published_date')
    return render(request, 'blog/post_list.html', {'posts': posts})
```



## Django テンプレート

### テンプレートタグとは

- ブラウザはHTMLだけ分かる

- HTMLはどちらかというと静的

- Pythonは動的

*Djangoテンプレートタグ*はHTMLにPythonのようなコードを埋め込むことができて，動的なウェブサイトがより早く簡単に作れる

`blog/templates/blog/post_list.html`に以下のコードを打ち込む．すると二重カッコ内の変数を表示できる

```python
{{ posts }}
```

`blog/templates/blog/post_list.html`の２つ目のdivと3つめのdivを消して，divではさんだ`{{ posts }}`を打ち込む.ブラウザでページを更新して以下のような表示が出るのを確認する

`<QuerySet [<Post: My second post>, <Post: My first post>]>`

posts変数を書くには以下のように書く

```python
{% for post in posts %}
    {{ post }}
{% endfor %}
```

ブラウザを更新して確認



以下のようにhtmlと混ぜて書く

```html
<div>
    <h1><a href="/">Django Girls Blog</a></h1>
</div>

{% for post in posts %}
    <div>
        <p>published: {{ post.published_date }}</p>
        <h2><a href="">{{ post.title }}</a></h2>
        <p>{{ post.text|linebreaksbr }}</p>
    </div>
{% endfor %}
```

git にpush して，pythonanywhereのコンソールでgit pullする

admin画面で新しいポストを作る．忘れずに公開日時も設定する．正しく表示されるか確認する．

## CSS

WEBサイトの見た目や書式を記述するための言語

## Bootstrap

Bootstrapは美しいWebサイトを開発するためのHTMLとCSSのフレームワーク

http://getbootstrap.com/

## Bootstrapのインストール

Bootstrap をインストールするため、`.html` ファイル (blog/templates/blog/post_list.html) をコードエディタで開き `<head>` の中に以下のコードを追加する

```html
<link rel="stylesheet" href="//maxcdn.bootstrapcdn.com/bootstrap/3.2.0/css/bootstrap.min.css">
<link rel="stylesheet" href="//maxcdn.bootstrapcdn.com/bootstrap/3.2.0/css/bootstrap-theme.min.css">

```

これは自分のプロジェクトにファイルを追加しているわけではなく，インターネット上にあるファイルを刺しているだけである．これだけでずいぶん見た目がよくなる



## Djangoの静的ファイル

静的ファイル : CSSファイルや画像といった，動的な変更が発生しないファイルのこと．リクエストに依存しない．

## プロジェクト内での静的ファイルの置き場

Djangoは、全てのアプリのフォルダ内の "static" と名づけられた全てのフォルダを自動的に探して、その中身を静的ファイルとして使えるようにする．

`djangogirls/blog`の中に`static`ディレクトリを作成する．

```bash
mkdir static
```

 ### CSSファイルを置く

staticディレクトリのなかに`css`ディレクトリを作成する

``` bash
mkdir css
```

cssディレクトリの中に`blog.css`という新規ファイルを作成する

```bash
touch blog.css
```

エディタで`blog.css`を開く

`blog.css`に次のコードを追加する

```css
h1 a, h2 a {
    color: #C25100;
}
```



次に，CSSを追加したことをHTMLテンプレートに教える．

`blog/templates/blog/post_list.html`を開いて，戦闘に次の行を追加する

```html
{% load static %}
```

`<head>`の中にあるBootstrap CSSファイルのリンクの下に、次の行を追加する

```html
<link rel="stylesheet" href="{% static 'css/blog.css' %}">
```



ブラウザは上から書いた順番でファイルを読み込むので、記述する箇所はよく確かめる必要がある． 順番が逆になると、私たちが書いたファイルがBootstrapのファイルに上書きされてしまうかもしれない．これで、テンプレートにCSSファイルがある場所を教えた．



〜中略〜



HTMLコードの一部に名前をつける．ヘッダーを含む`div`要素に，`page-header`というクラス名をつける

```html
<div class="page-header">
    <h1><a href="/">Django Girls Blog</a></h1>
</div>
```



同様にブログ投稿を含む `div` 要素に `post `というクラス名をつける

```html
<div class="post">
    <p>published: {{ post.published_date }}</p>
    <h2><a href="">{{ post.title }}</a></h2>
    <p>{{ post.text|linebreaksbr }}</p>
</div>
```

 

さまざまなセレクタに宣言ブロックを追加する． `.` で始まるセレクタはクラスに関連する．

blog/static/css/blog.cssファイルに以下の内容を貼り付ける

```css
.page-header {
    background-color: #C25100;
    margin-top: 0;
    padding: 20px 20px 20px 40px;
}

.page-header h1, .page-header h1 a, .page-header h1 a:visited, .page-header h1 a:active {
    color: #ffffff;
    font-size: 36pt;
    text-decoration: none;
}

.content {
    margin-left: 40px;
}

h1, h2, h3, h4 {
    font-family: 'Lobster', cursive;
}

.date {
    color: #828282;
}

.save {
    float: right;
}

.post-form textarea, .post-form input {
    width: 100%;
}

.top-menu, .top-menu:hover, .top-menu:visited {
    color: #ffffff;
    float: right;
    font-size: 26pt;
    margin-right: 20px;
}

.post {
    margin-bottom: 70px;
}

.post h2 a, .post h2 a:visited {
    color: #000000;
}
```



投稿を表示しているHTMLコードをクラス宣言で囲む

`blog/templates/blog/post_list.html`の次の部分を

```html
{% for post in posts %}
    <div class="post">
        <p>published: {{ post.published_date }}</p>
        <h2><a href="">{{ post.title }}</a></h2>
        <p>{{ post.text|linebreaksbr }}</p>
    </div>
{% endfor %}
```

次のコードに置き換える

```html
<div class="content container">
    <div class="row">
        <div class="col-md-8">
            {% for post in posts %}
                <div class="post">
                    <div class="date">
                        <p>published: {{ post.published_date }}</p>
                    </div>
                    <h2><a href="">{{ post.title }}</a></h2>
                    <p>{{ post.text|linebreaksbr }}</p>
                </div>
            {% endfor %}
        </div>
    </div>
</div>
```



## テンプレートの拡張

テンプレートの拡張 : HTMLの共通部分をウェブサイトの異なるページで使えるということ．同じ情報やレイアウトを複数の場所で利用したい時に役立つ

### 基本テンプレートを作成する

`blog/templates/blog/`以下に`base.html`ファイルを作る

エディタで開き，`post_list.html`のコードをコピペする．

それから`base.html`内の`<body>`全体(`<body>`と`</body>`の間のすべて)を次で置き換える．

```html
<body>
    <div class="page-header">
        <h1><a href="/">Django Girls Blog</a></h1>
    </div>
    <div class="content container">
        <div class="row">
            <div class="col-md-8">
            {% block content %}
            {% endblock %}
            </div>
        </div>
    </div>
</body>
```



## Deployment checklistによるセキュリティの強化

https://docs.djangoproject.com/en/3.1/howto/deployment/checklist/

### SECRET_KEY

`/etc/secret_key.txt`を作成し，シークレットキーの文字列を打ち込む.

`settings.py`のSECRET_KEYの行を以下のコードに書き換える

```python
with open('./etc/secret_key.txt') as f:
    SECRET_KEY = f.read().strip()
```



`.gitignore`に`/etc`を追加する

## 開発用と本番用に分ける

https://www.django-academy.net/lecture/95/52/



`settings.py`のある場所に`settings`ディレクトリを作成する

`settings`ディレクトリに`settings.py`を移動させる

`settings`ディレクトリをPythonのパッケージとしてPythonに認識させるために，`settings`ディレクトリ内に`__init__.py`を作成し，以下のコードを書き込む

```python
from .settings import *
```

同じ場所に`local_settings.py`を作成し，`settings.py`ファイルの中身をコピペする



次に，`__init__.py`でどういった場合にどちらのsettingファイルを使うか設定する



本番用の`settings.py` の`DEBUG`の行をFalseに書き換える

`DEBUG=False`



## dotenvの使い方

https://www.django-academy.net/lecture/56/18/

あるコードを書くことによって別のところに描かれた変数を参照できるようになる仕組み

`.env`ファイルをプロジェクトのルートディレクトリに作成する．

`.env`ファイルの中に変数を設定する ．

`.env`ファイルをロードすると環境変数へ反映される

os.getenvというメソッドで呼び出せる．

.envのインストール

```pip install python-dotenv```

secretkeyを.envファイルに貼り付ける．クオーテーションはつけない

`settings.py`のSECRET_KEYの行を次のように書き換える

```python
SECRET_KEY = os.getenv('SECRET_KEY')
```

dotenvのインポート.

settings.pyに貼り付ける

```python
from dotenv import load_dotenv
load_dotenv()
```



## How to set environment variables for your web apps (for SECRET_KEY etc)

https://help.pythonanywhere.com/pages/environment-variables-for-web-apps/

環境変数を以下の二箇所に置く

- Bashコンソールで動くpostactivate script

- webアプリで動くWSGIファイル

  重複を避けるために`.env`ファイルを使い`python-dotenv`を使って読み込むことを推奨する

pythonanywhereのWEB TABへ行き，WSGIファイルのリンクをクリックする．

そこで，Pythonコードを使って環境変数を配置できる．これは自分のwebサイトを読み込む前に行う必要がある．

`.env.sample`というファイルを作成し，.envの書き方の例を書いておく．こちらはgithubにpushして参考にする



## pa_autoconfigure_django.pyを使わずにデプロイ







djangoのプロジェクトのソースコードをpythonanywhere のコンソールにクローンする

```
mkbirtualenv myvenv --python=python3.6
```



```bash
pip install django==2.2.4
pip install python-dotenv
```



web tabに移動して`Add a new web app`をクリックする．

Nextを押した後,`Manual configuration`をクリック，python3.6を選択する．Nextを押す．

設定ページが開くので`Virtualenv`の欄の`Enter path to a vitualenv, if desired`を選択し，

`/home/koki534/.virtualenvs/myvenv`と書く

WSGIファイルの以下のDJANGOの部分をコメントアウトを使って以下のようにする．pathとsettingsは各自編集する

```python
# +++++++++++ DJANGO +++++++++++
# To use your own django app use code like this:
import os
import sys
#
## assuming your django settings file is at '/home/koki534/mysite/mysite/settings.py'
## and your manage.py is is at '/home/koki534/mysite/manage.py'
path = '/home/koki534/mysite'
if path not in sys.path:
    sys.path.append(path)

os.environ['DJANGO_SETTINGS_MODULE'] = 'mysite.settings'

# then:
from django.core.wsgi import get_wsgi_application
application = get_wsgi_application()
```

work



こっちにしてみる

```python
# This file contains the WSGI configuration required to serve up your
# Django app
import os
import sys

# Add your project directory to the sys.path
settings_path = '/home/koki534/my-first-blog/mysite'
sys.path.insert(0, settings_path)  # モジュールをimportする際のpathを追加

# dotenv
# import os
from dotenv import load_dotenv
project_folder = os.path.expanduser('/home/koki534/my-first-blog')  # adjust as appropriate
load_dotenv(os.path.join(project_folder, '.env'))

# os.path.expanduser はパス中のホームディレクトリの記号を絶対パスにして返す

# Set environment variable to tell django where your settings.py is
os.environ['DJANGO_SETTINGS_MODULE'] = 'mysite.settings'

# Set the 'application' variable to the Django wsgi app
from django.core.wsgi import get_wsgi_application
application = get_wsgi_application()

```



 ```python

 ```

