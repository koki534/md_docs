# Django



## startproject

- Djangoに必要なファイル群をコピーしてくる

vscodeのターミナルからcondaの仮想環境に入って

`django-admin startproject プロジェクト名



サーバーが動くか確認　runserver

`python manage.py runserver`

`__init__.py`というファイルをフォルダにいれることで，このフォルダにはPythonのパッケージやファイルが入っているということを認識させることができる．



`wsgi.py` webサーバーとアプリケーションのファイルが入っている場所の仲介役を担うファイル

## urls.py

urlpatternsの中にurlのPATHとクラスを書き込む

```Python
urlpatterns = [
    path('admin/', admin.site.urls),
    path('helloworld/', helloworldfunction),
    path('helloworld2/', HelloWorldView.as_view())  #クラスの場合，as_viewというメソッドを指定して使う
]
```



## BASE_DIRとテンプレートフォルダ

`os.path.abspath(__file__)` settings.pyが入っている絶対path

`os.path.dirname` ファイルが入っているディレクトリの場所を示す

BASE_DIRはmanage.pyが入っているフォルダ



## Class based view

httpリクエストを受け取ってurls.pyへ渡される

urls.pyがviews.pyを呼び出して

views.pyがhtmlファイルを呼び出してhttpレスポンスに返す



## アプリ

プロジェクトの中に複数アプリ

`python manage.py startappアプリ名  でアプリを作る

プロジェクトが受け取ったURLをアプリのURLにつなぎこむ

## TODOアプリ



`django-admin startproject プロジェクト名 .`



## models.py



データベースを使ってデータを取ってきて操作する際の設計書

## データベース

どういったデータなのか指示を出す必要がある

モデルのフィールドの種類は公式サイトやqiitaで検索

## makemigrations とmigrate

`python manage.py makemigrations`

`python manage.py migrate`

## 管理画面とcreatesuperuser

runserverしてから/adminにアクセス

管理者画面にアクセスするためにユーザーを作る

`python manage.py createsuperuser`

admin.pyで管理画面でデータを扱えるようになるコマンドを打ち込む