# VSCodeメモ



## ワークスペースの設定

参考

https://qiita.com/k_bobchin/items/717c216ddc29e5fbcd43



設定や拡張機能をプロジェクトごとに設定するのが目的

プロジェクトのディレクトリを開く

command + ,で設定を開く

Workspaceのタブをクリック

検索欄にonsaveと入れ、Format On Saveのチェックを入れる。

すると自動で.vscode/settings.jsonが作成される。

.vscode/settings.jsonがあるフォルダには、ユーザ設定が上書きされて適応される。

### 拡張機能のおすすめを行う。

VSCodeの拡張機能で、入れたい拡張機能の右下の歯車をクリックし、「ワークスペースの推奨事項に追加する。」をクリックする。英語だと、"Add to Workspace Recommendations"

そうすると、.vscode/extensions.jsonファイルが作成され、中に拡張機能が追加される。

これだけではおすすめ機能を出すことができないので、.vscode/settings.jsonを開いて、`"extensions.ignoreRecommendations": false`を{}内に追記する。settings.jsonは以下のようになる。

```json
{
    "editor.formatOnSave": true,
    "extensions.ignoreRecommendations": false
}
```



上記設定がされたフォルダをvscodeで開くと右下に「このワークスペースには拡張機能の推奨事項があります。」と表示されるので、「すべてインストール」か「推奨事項を表示」を選択できるようになる。

拡張機能の検索欄で「@recommended」を入力するとワークスペースの推奨事項が表示される。

githubでも.vscode/settings.jsonと.vscode/extensions.jsonをpushすれば設定を共有できる。

終わり





## vscodeでdocker環境を使う
https://docs.microsoft.com/ja-jp/learn/modules/use-docker-container-dev-env-vs-code/1-introduction

### vscode上でgit cloneを行う
vscodeを開く
F1キーでコマンドパレットを開く
cloneと入力し、[Git: Clone]を選択
サンプルリポジトリのURLを貼り付ける
プロジェクトを複製するディレクトリを選択する
visual studio codeの右下の通知のOpenを選択する

### 開発コンテナの追加
F1を押してコマンドパレットを開く
add dev containerと入力する
[Remote-Containers: Add Development Container Configuration Files](Remote-Containers: 開発コンテナー構成ファイルの追加) を選択
次のオプションを選択
python3
3
node.jsはチェックをはずす　
OKを押す
右下の通知は無視でよい

### コンテナでプロジェクトを開く
F1を押してコマンドパレットを開く
reopen in containerと入力する
使用可能なオプションの一覧から [Remote Containers: Reopen in Container](リモート コンテナー: コンテナーでもう一度開く) を選択
コンテナの構築が始まる。初回は数分かかることがある。

### コンテナに入っているか確認
ctrl + `を押してvisual studio code で統合ターミナルを開く
次のコマンドを実行して、Pythonがあることを確認する
```bash:
	python --version
```




### 　devcontainer.jsonファイルを使う
devcontainer.json ファイルを使用してプロジェクト環境を自動的に構成

### プロジェクトをローカルで開き直す

F1を押してコマンドパレットを開く
locallyと入力し、[Remote-Containers: Reopen Locally](Remote - Containers: ローカルで開き直す) を選択
devcontainer.jsonファイルを開いていない場合は開く
拡張機能のjinjaを検索する。
wholroydのJinjaを右クリックし、「devcontainer.json に追加」を選択する
devcontainer.json内で拡張機能IDが`extensions`オプションに追加されていることを確認する

### 転送先のポート番号を明示的に指定する
devcontainer.json内の`forwardPorts`オプションのコメントを解除し、配列にポート番号`5000`を追加する。

### 依存関係のインストールを自動化する

devcontainer.json内の`postCreateCommand`オプションのコメントを解除する
```
"postCreateCommand": "pip3 install --user -r requirements.txt"
```
### 新しいコンテナを再構築する
F1を押してコマンドパレットを開く
rebuildと入力し、[Remote-Containers: Rebuild and Reopen in Container](Remote - Containers: コンテナーで再構築して開き直す) を選択
これによりdevcontainer.jsonで指定した変更を使用してコンテナが再構築される

下部のPORTSタブで転送されたポートを確認する

### ポートフォワーディング
コンテナ内のサーバーのリソースにアクセスするためにポートをローカルコンピューター(ホスト)に転送する

### 転送されたポートを表示または変更する方法

F1キーを押してコマンドパレットを開く
Forward a Portコマンドを実行すると使われているポートを表示できる

ポートの転送を停止するにはポート番号横の☓を押す