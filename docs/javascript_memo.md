# JavaScriptメモ



参考

https://www.youtube.com/watch?v=E08jeQBa1D0

JavaScriptの「基礎」が1時間で分かる「超」入門講座【初心者向け】



JavaScriptとは

​	ブラウザ上で実行される唯一のプログラミング言語

できること

​	アニメーション

​	ユーザー操作に合わせた見た目の変更

​		ex.靴の画像の色を変える。色の選択によって

​	API

​		バックエンドでも実行できる

​		バックエンド実行環境はNode.js



JavaScriptを学ぶ意義

​	フロントエンジニアとして活躍

​	バックエンドにまで応用が効く

​	フロントエンドの市場価値が向上

​	UI/UXの差別化

​	React, Vue, Angular といったJavaScriptのフレームワークが流行



環境

​	VS Code

​	Live Server  ファイルを保存して、修正するとすぐ反映され、ブラウザでの動作確認が便利になる。VSCodeの拡張機能を入れれば良い。



TODOフォルダを作成

TODOフォルダの中にindex.htmlを作成

index.htmlのいち行目で `! tabキー`を押すとコードの補完ができ、基本的なhtmlの雛形が作成される。

langをenからjaに書き換える。

chromeを立ち上げておく。

index.html上で右クリックし、「open with liveserver」をクリックする。

例えばbodyタグにh1タグでTODOと書いて保存すると、ブラウザで再読み込みしなくてもTODOと表示される。



### Javascriptのコードを書く

htmlのscriptタグの中に書く

head, bodyのどちらかに入れる

ベストプラクティスはbodyの一番下。なぜかというと、headタグが先に読み込まれ、その後にbodyタグが読み込まれるので、headにscriptを置くとwebページの表示が遅くなるから。



手順

bodyタグの中にscriptタグを書く。

```html
<body>
    <h1>TODO</h1>
    <script>console.log('Hello')</script>
</body>
```

ブラウザで、右クリック　検証を選択

コンソールタブを選択

console.logを使うとコンソールタブの中に値を出力することができる。

これで、javascriptの動作確認がしやすくなる。

htmlの中にjavascriptを書き込むこともできるが、htmlｔjavascriptを別々のファイルにしたほうが管理しやすい。



プロジェクトディレクトリの中にindex.jsを作成する。

htmlに書いたscriptタグの中身をindex.jsの中に移す。文末のセミコロンも忘れずに入力する。

コメントも以下のように書ける。

```js
// コメント
console.log('Hello');
```

元のindex.htmlのscriptタグを以下のように書き換え、index.jsを読み込む。

```html
<body>
    <h1>TODO</h1>
    <script src="index.js"></script>
</body>
```



### 作る順番

HTML -> JavaScript

javascriptはhtmlの内容を動的に変更していくものなので、先にhtmlを書いたほうが開発がスムーズになる。



### プログラムを書くコツ

小さく作って小さく動かす

最初にタスクばらしをする

​	どういうタスクがあるか

​	どういう順番でタスクを作るか



### タスクばらし

【html】TODO入力フォームを作成

【Bootstrap】見た目を整える  BootstrapはCSSのフレームワーク。htmlにクラス名を当てることで簡単にデザインを当てることができる。

【JavaScript】TODOを表示・保存

【JavaScript】TODOを削除

【JavaScript】TODOを完了



#### TODO入力フォームを作成

formタグを作成

bodyタグの中に以下のようにformとinputタグを書く。

```html
<body>
    <h1>TODO</h1>
    <form id="form">
        <input type="text" id="input" placeholder="TODOを入力" autocomplete="off">
    </form>
    <script src="index.js"></script>
</body>

```



### 見た目を整えよう

Bootstrapを使う

​	CSSのフレームワーク

​	htmlのクラス名を指定するだけで簡単にデザイン



手順

Bootstrapのホームページを開く

https://getbootstrap.jp/



CSS onlyのコードをコピーする。

```html
<!-- CSS only -->
<link href="https://cdn.jsdelivr.net/npm/bootstrap@5.0.0-beta1/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-giJF6kkoqNQ00vy+HMDP7azOuL0xtbfIcaT9wjKHr8RbDVddVHyTfAAsrekwKmP1" crossorigin="anonymous">
```

これをheadタグの中に貼り付ける。

```html
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.0.0-beta1/dist/css/bootstrap.min.css" rel="stylesheet"
        integrity="sha384-giJF6kkoqNQ00vy+HMDP7azOuL0xtbfIcaT9wjKHr8RbDVddVHyTfAAsrekwKmP1" crossorigin="anonymous">
    <title>Document</title>
</head>
```



bodyタグにclass="bg-light"を当てる

```html
<body class="bg-light">
```



h1タグからformタグの終わりまでをコンテナでくくる。範囲を選択し、F1キーを押してコマンドパレットを表示させる。Emmetのラップ変換(Emmet: Wrap with Abbreviation)を選択。divと入力してエンター。

以下のようにdivタグにクラスを書き加え、コンテナ化する。

```html
<div class="container w-75">
```

以下のようにタグにclassを当てていくのがBootstrapの使い方となる。

```html
<body class="bg-light">
    <div class="container w-75">
        <h1 class="text-center text-info my-4">TODO</h1>
        <form id="form" class="mb-4">
            <input type="text" id="input" class="form-control" placeholder="TODOを入力" autocomplete="off">
        </form>
    </div>
    <script src="index.js"></script>
</body>
```



次にやりたいこと

ユーザーの入力値を記録したい

どうやったらできる？->documentでできる。



document

​	ブラウザAPIの一種  ブラウザに組み込まれていて、ブラウザの情報をプログラムから簡単にアクセスできるように用意してくれているもの

​	ブラウザに読み込まれたウェブページの情報を扱っている

​	JavaScriptの機能ではないので注意

javascriptからブラウザAPIであるdocumentを使うという仕組み



手順

chromeのコンソールで、javascriptを動かすことができる。

chromeのコンソールにdocumentと入力し、エンターを押す。

documentの横の三角を押すと、htmlの情報が閲覧できる。

これにより、formに入力された情報も取得できる

以下をコンソールに入力する。idはjavascriptなどで、情報を取ってくるために、与えたもの

```javascript
document.getElementById("input")
```



TODOの入力欄にaaaと入力し、その値を取り出してみる。そのために、以下のコードを入力し、実行する。

```javascript
document.getElementById("input").value
```



次にやりたいこと

index.jsのファイルから値を取りたい

どうやったらできる？どのタイミングで取る？



#### データを取るタイミング

ユーザーがエンターを押した時

必要な知識 : 変数、関数、addEventListener



#### 変数

変数の種類

​	let, const, var

let  :  値の再代入が可能

const  :   値の再代入が不可   定数の宣言

var  :  letとほぼ同じ　昔の記法　今は使わない



```javascript
let height = 168;
```



#### 関数

```javascript
function double(num) {
	return num * 2;
}
```

シフトを押しながらエンターを押すと実行せずに改行できる！



```javascript
double(4)
```



#### addEventListener

特定のイベントが起きた時に、JavaScriptの処理を追加するためのブラウザAPIの機能

使い所：イベントが起きた時に処理を追加したい時

書き方

`ターゲット.addEventListener(イベント名, 関数);`



```javascript
form.addEventListener("submit", function () {
    console.log();
})
```





index.jsに以下のコードを入力する。元のコードは消す。

```js
const form = document.getElementById("form");
const input = document.getElementById("input")
form.addEventListener("submit", function () {
    console.log(input.value);
});
```



リロードのイベントを無しにするには、関数の引数にeventを入れ、event.preventDefault()を入力する。

```js
const form = document.getElementById("form");
const input = document.getElementById("input")
form.addEventListener("submit", function (event) {
    event.preventDefault();
    console.log(input.value);
});
```



次にやりたいこと

取得したフォームの値をHTML上に追加しよう

ｈｔｍｌでどのような構造で情報を追加すればいい？

ulとliタグを使って、リストとして追加



```html
<body class="bg-light">
    <div class="container w-75">
        <h1 class="text-center text-info my-4">TODO</h1>
        <form id="form" class="mb-4">
            <input type="text" id="input" class="form-control" placeholder="TODOを入力" autocomplete="off">
        </form>
        <ul class="list-group text-secondary" id="ul">
        </ul>
    </div>
    <script src="index.js"></script>
</body>
```



```js
const form = document.getElementById("form");
const input = document.getElementById("input")
const ul = document.getElementById("ul")
form.addEventListener("submit", function (event) {
    event.preventDefault();
    console.log();
    add();

});

function add() {
    const li = document.createElement("li");
    li.innerText = input.value;
    li.classList.add("list-group-item");
    ul.appendChild(li);
    input.value = "";   //空にする
}
```

空でもリストが追加されるので、それを対策するために条件分岐を使う。

条件分岐

特定の条件を満たすかで処理を変える

if文



```js
function add() {
    let todoText = input.value;
    if (todoText.length > 0) {
        const li = document.createElement("li");
        li.innerText = todoText;
        li.classList.add("list-group-item");
        ul.appendChild(li);
        input.value = "";   //空にする
    }
```



暗黙的型変換　if文の条件式を真偽値に変換するなど、データ型を意識しなくてもプログラムが書けるようにする仕組み

真偽値の判定もいろいろある。空文字はfalseになるので、以下のようにしてもよい。



```js
function add() {
    let todoText = input.value;
    if (todoText {
        const li = document.createElement("li");
        li.innerText = todoText;
        li.classList.add("list-group-item");
        ul.appendChild(li);
        input.value = "";   //空にする
    }
```





課題

画面をリロードするとデータが消える

どうすればデータを残せる？

ローカルストレージに保存 -> ブラウザにデータを保存する仕組み



配列

```js
const array = [
	"one",
	"two",
	"three"
];

array[0]; // => "one"
array.push("four");
array[3];  // => "four"
```



forEach



```js
const array = [1, 2, 3];

array.forEach(value => {
	console.log(value * 2);
})
// 2, 4, 6
```





```js
function saveData() {
    const lists = document.querySelectorAll("li");
    let todos = [];
    lists.forEach(list => {
        todos.push(list.innerText);
        console.log(list.innerText);
    })
    console.log(lists);
}
```



localStrage

​	いままで　：データを保存しておく場所がなかった

localstrageはブラウザにデータを保存しておく仕組み



```js
// データの保存
localStrage.setItem('キー', '値');

// データの取得
localStorage.getItem('キー');
```



```js
function saveData() {
    const lists = document.querySelectorAll("li");
    let todos = [];
    lists.forEach(list => {
        todos.push(list.innerText);
        console.log(list.innerText);
    });
    localStorage.setItem("todos", JSON.stringify(todos));
}
```



chromeのApplicationタブを選択し、LocalStrageを選択する。保存した変数を見ることができる。

```js
const form = document.getElementById("form");
const input = document.getElementById("input")
const ul = document.getElementById("ul")
const todos = JSON.parse(localStorage.getItem("todos"));

if (todos) {
    todos.forEach(todo => {
        add(todo);
    })
}

form.addEventListener("submit", function (event) {
    event.preventDefault();
    console.log();
    add();

});

function add(todo) {
    let todoText = input.value;

    if (todo) {
        todoText = todo;
    }
    if (todoText) {
        const li = document.createElement("li");
        li.innerText = todoText;
        li.classList.add("list-group-item");
        ul.appendChild(li);
        input.value = "";   //空にする
        saveData();
    }
}

function saveData() {
    const lists = document.querySelectorAll("li");
    let todos = [];
    lists.forEach(list => {
        todos.push(list.innerText);
        console.log(list.innerText);
    });
    localStorage.setItem("todos", JSON.stringify(todos));
}
```



次にやりたいこと

TODOを削除できる機能

右クリックされたらliタグを削除

右クリックされたら〇〇はどう実現する？　ー＞addEventListener

```js
function add(todo) {
    let todoText = input.value;

    if (todo) {
        todoText = todo;
    }
    if (todoText) {
        const li = document.createElement("li");
        li.innerText = todoText;
        li.classList.add("list-group-item");
        li.addEventListener("contextmenu", function
            (event) {
            event.preventDefault();
            li.remove();
            saveData();
        })
        ul.appendChild(li);
        input.value = "";   //空にする
        saveData();
    }
}

```



次にやりたいこと

TODOに完了マークをつける機能

左クリックしたら取り消し線をつける



```js
function add(todo) {
    let todoText = input.value;

    if (todo) {
        todoText = todo;
    }
    if (todoText) {
        const li = document.createElement("li");
        li.innerText = todoText;
        li.classList.add("list-group-item");
        li.addEventListener("contextmenu", function
            (event) {
            event.preventDefault();
            li.remove();
            saveData();
        });

        li.addEventListener("click", function () {
            li.classList.toggle(
                "text-decoration-line-through"
            );
        })
        ul.appendChild(li);
        input.value = "";   //空にする
        saveData();
    }
}
```



オブジェクト

```js
function saveData() {
    const lists = document.querySelectorAll("li");
    let todos = [];

    lists.forEach(list => {
        let todo = {
            text: list.innerText,
            completed: list.classList.contains(
                "text-decoration-line-through")
        }
        todos.push(todo);
        console.log(list.innerText);
    });
    localStorage.setItem("todos", JSON.stringify(todos));
}
```

