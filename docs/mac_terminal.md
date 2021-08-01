# mac terminal操作

### カレントディレクトリをクリップボードにコピー．
```bash
pwd|pbcopy
```
pbcopyはpaste boardの意味

### 一つ下のディレクトリにファイルを移動
```bash
mv <ファイル名> <ディレクトリ名>
```
### ２つ上の階層に移動
```bash
cd ../../
```

### helpの表示
```bash
man <コマンド名>
```
manはmanualの意味

### ディレクトリのコピー
```bash
cp -r <コピーしたいディレクトリ名> <移動先ディレクトリ>

### ひとつ下のディレクトリの中身を見る
```bash
ls <ディレクトリ名>
```



### カレントディレクトリ変更
```bash
cd
```
### ファイル名一括変更
以下ではファイル名を"test(連番).png"に変更
```bash
ls | awk '{ printf "mv %s test%03d.png\n", $0, NR }' | sh
```



## Typoraをターミナルから開く

`open -a /Applications/Typora.app journal.md`
で可能だが，エイリアスを設定する

1. ホームディレクトリに移動
`cd ~`
2. .zshrcを開く
`subl . zshrc`
3. aliasを記述して保存
`alias typora="open -a /Applications/Typora.app"`
4. 変更を反映
`source.zshrc`



### カレントディレクトリをクリップボードにコピー．
```bash
pwd|pbcopy
```
pbcopyはpaste boardの意味

### 一つ下のディレクトリにファイルを移動
```bash
mv <ファイル名> <ディレクトリ名>
```
### ２つ上の階層に移動
```bash
cd ../../
```

### helpの表示
```bash
man <コマンド名>
```
manはmanualの意味

### ディレクトリのコピー
```bash
cp -r <コピーしたいディレクトリ名> <移動先ディレクトリ>

### ひとつ下のディレクトリの中身を見る
```bash
ls <ディレクトリ名>
```