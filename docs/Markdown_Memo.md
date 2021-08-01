# Markdownメモ

## 画像の挿入

![説明](画像ファイル名)

## チェックボックス

`- [ ]`

`- [x]`

かっこの中は半角のエックス

## 四則演算

### 掛け算

`\times`

### 割り算

`\div`

### 分数

`\frac{分子}{分母}`
## スペース

`&nbsp;`

## 矢印

`&rarr;`

## 太字

`**`で囲う。直前にスペースを入れる

`**太字**`

## 中央寄せ

qiitaでは$$で囲って無理やり中央寄せにする.

```md
\<div style="text-align: center;">
文章
\</div>
```

```md
<div style="text-align: center;">
図 1.2.3-1 ブロックの繋がり
</div>
```

## 画像ファイルのアップロード

画像ファイルを本文にドラッグアンドドロップ

`![ファイル名](URL)`
の形式から

`<img width="表示幅" alt="ファイル名" src="URL">`

の形式に書き換えるとサイズを調整できる

`https://qiita.com/kaizen_nagoya/items/cef6ae1fcbdbec9e7be2`

## リンク

`[表示する文言](リンク先のURL)`

`[Test](#リンク先)`

## その他

``で囲うとリンクやコマンドを無効にできる

## 表の挿入

（空白行)
|ヘッダ行|
|アラインメント行|
|データ行|

例

```md
|a|
|---|
|data1|
```

|a|
|---|
|data1|

## 数式

$$P(B|A) = \frac{P(A ∩ B)}{P(A)}$$

数式で書くと，時刻 $t$ でのデータ点 $x_t$ は

## VSCode拡張機能

- markdownlint
- Markdown All in One
