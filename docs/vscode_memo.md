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