# はじめに  

pipenvを導入したので，環境構築方法をまとめます．
  
余分なパッケージの入っていないrequirements.txtを作成したいので，devオプションを使って，コード内でimportするパッケージと環境構築のためにインストールしたパッケージを分けます．
  
間違いなどありましたら指摘していただけると非常に助かります．  

## 環境

- Windows 10 pro  
- PowerShell 5.1.18362.628  
- Python 3.7.6  
- pipenv 2018.11.26  

## pipenvのインストール

`pip install pipenv`
  
ローカル環境にpipenvをインストールします.

## 仮想環境の作成

仮想環境を使いたいディレクトリ内に入って
  
`pipenv install --python=3.7.6`  

で仮想環境を作成します．バージョンは適宜使いたいものを指定します.  
  
同時に**Pipfile**と**Pipfile.lock**がカレントディレクトリに作成されます.  
  
`ls`  
  
でファイルが作成されていることを確認できます.

## 必要なライブラリのインストール

`pipenv install [package]`  
  
で仮想環境に入らずに仮想環境内にパッケージをインストールできます．例えば，  
  
`pipenv install numpy`
  
とするとnumpyがインストールできます．

## --dev オプション

`pipenv install --dev [package]`  
  
--devオプションを使うと開発用パッケージとしてインストールできます.  
  
環境構築のために使うパッケージはdevオプションを使ってインストールし，  
コード上でimportするようなパッケージはこのオプション無しでインストールします．

## 仮想環境内のパッケージの確認

`pipenv lock -r`  
  
で仮想環境に入っているパッケージの一覧(--devで入れたもの以外)が表示されます．

`pipenv lock -r -d`  
  
で開発環境のパッケージが確認できます．

`pipenv run pip freeze`  
  
で，入っているすべてのパッケージが確認できます．　　

## requirements.txtの作成

`pipenv lock -r > requirements.txt`  
  
でカレントディレクトリにrequirements.txtの作成ができます  

## 削除とPipfileのクリーン

`pipenv uninstall [package]`  
  
パッケージをアンインストールします
  
`pipenv clean`
  
パッケージをuninstallしても残っている依存パッケージを削除します．  
  
[参考]<https://qiita.com/eduidl/items/c0e8256bb3a5a735d19c>　　
  
`pipenv --rm`  
  
仮想環境を削除します．  

## その他コマンド

`pipenv --venv`  
  
仮想環境のpathを取得.仮想環境があるかどうかの確認に使えます．
  
`pipenv shell`  
  
仮想環境に入ります．
  
`exit`  
仮想環境から出ます

※PowerShellを使っている場合，pipenvで仮想環境に入っても仮想環境に入っているという表記がありません(わかりにくいです)．  

## 追記

jupyter labやVScodeには，仮想環境を選択できる仕組みがあるので，  
`switch kernel`  
などをクリックして仮想環境を変更することができます．
