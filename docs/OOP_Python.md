# クラス

オブジェクト指向において、最も基本的な仕組みは**クラス**である

クラスの機能として「**カプセル化**」と「**インスタンス（オブジェクト）の生成**」がある

## カプセル化

**変数と関数をまとめた上で、外部に公開する情報を制限する機能のこと**

**「変数と関数をまとめる」とは？**

複数の変数と関数をまとめて一つのクラスにすることで、

**プログラムを整理整頓する**ことができる

例：半径を渡すと円周の長さと面積を求める関数


```
pi = 3.14 #円周率

def circle_length(r):
  return r * 2 * pi

def circle_area(r):
  return r * r * pi
```

**piというグローバル変数が使われている**ことに注意

クラスを使うと以下のようになる


```
class Circle:
  pi = 3.14 #円周率

  def length(self, r):
    return r * 2 * Circle.pi

  def area(self, r):
    return r * r * Circle.pi
```

クラスを使うと**変数と関数をまとめる**ことができる

上の例では、変数piと関数lengthと関数areaを、Circleという一つのクラスにまとめている



クラス名の最初の文字は慣例的に**大文字**で記述する

詳しくは、[PEP 8](https://pep8-ja.readthedocs.io/ja/latest/)（Pythonのコーディング規約）参照

クラスが持つ変数を**クラス変数**（属性）、クラスが持つ関数を**メソッド**という名前で呼ぶ

上の例だとCircleクラスはクラス変数piとlengthメソッド、areaメソッドを持つクラスであると言える

メソッドの引数に**self**というものが追加されていることに注目、後ほど説明

変数と関数をクラスによってまとめると次のようなメリットがある

*   変数と関数をまとめて、**意味のある1つの部品**にすることができる
*   変数や関数の名前がシンプルになる


**「外部に公開する情報を制限する」とは？**

クラスの中に入れた変数は、外部からのアクセスを制限できる機能を使える

クラス変数piは、単に変数名を書くだけでは呼び出せないようになっている

クラス変数は「**クラス名.クラス変数名**」と書くことで、呼び出すことができる


```
class Circle:
  pi = 3.14 #円周率

  def length(self, r):
    return r * 2 * Circle.pi

  def area(self, r):
    return r * r * Circle.pi

#print(pi)
print(Circle.pi) #クラス変数の呼び出し
```

    3.14


piという変数は、areaとlengthという2つのメソッドでのみ使用される変数なので、

この**2つのメソッド以外からはアクセス出来ないようにした方が保守性が高くなる**

→　クラス変数をクラスの中（のメソッド）からしか呼び出せないようにするには、

　　「**__クラス変数名**」というように書く（クラス変数名の前にアンダーバーを2つ書く）


```
class Circle:
  __pi = 3.14 #クラスの中のメソッドからしか呼び出せない

  def length(self, r):
    return r * 2 * Circle.__pi

  def area(self, r):
    return r * r * Circle.__pi

#print(Circle.__pi) #クラス変数の呼び出し
Circle().length(1) #メソッドの呼び出し（後ほど説明）
```




    6.28



クラスの中からしかアクセスできない変数を**プライベート変数**と呼ぶこともある

プライベート変数を利用することで、**複数の関数（areaとlength）で利用する必要のある変数（pi）を、**

**グローバル変数として保持する必要がなくなった！**

→　**グローバル変数問題の解決！**

## インスタンス（オブジェクト）の生成

クラスからは**インスタンス（オブジェクト）を作る**ことができる

**クラス（class）**→　**分類**、種類

**インスタンス（instance）**→　**実例**、具体的なもの


**クラス：インスタンス**

人間　：太郎くん、花子さん、次郎くん、…

国　　：日本、アメリカ、オランダ、…

クラスからインスタンスを作ることができるので、言い換えるならば

**人間というクラス（分類）**から**太郎くんや花子さんというインスタンス（実例）**を作ることができる

クラスとは**変数とメソッドを意味のある1つの部品としてまとめたもの**

人間は、「名前」「年齢」「出身地」という**変数**を持っているし、

「話す」「歩く」「寝る」という**メソッド**（処理）を持っていると言える

→　人間を**クラス**として考えることができる


```
class Human:
  name = ""
  age = 0
  born = ""
  
  def talk(self):
    print("私の名前は" + self.name + "です")
  
  def walk(self):
    print("歩く")
   
  def sleep(self):
    print("寝る")
```

クラス（分類）とインスタンス（実例）の違いは？

→　**変数に具体的な値を持つこと**

太郎くんなら「name=”太郎”」だし、花子さんは「name="花子"」というような感じ

クラスから作られる、実例ごとの具体的な値を入れるための入れ物を**インスタンス**と呼び、

このようなインスタンス固有の値が入る変数を**インスタンス変数**と呼ぶ

「**クラス名()**」と記述することでクラスからインスタンスを作ることができる

インスタンス変数は「**インスタンス.インスタンス変数名**」、

メソッドは「**インスタンス.メソッド名()**」とすることで使うことができる


```
class Human:
  name = ""
  age = 0
  born = ""
  
  def talk(self):
    print("私の名前は" + self.name + "です")
  
  def walk(self):
    print("歩く")
   
  def sleep(self):
    print("寝る")
    
taro = Human() #インスタンスの生成
taro.name = "太郎" #インスタンス変数の定義
taro.age = 20
taro.born = "東京"
taro.talk() #メソッドの呼び出し

hanako = Human()
hanako.name = "花子"
hanako.age = 30
hanako.born = "大阪"
hanako.talk()

jiro = Human()
jiro.talk()
```

    私の名前は太郎です
    私の名前は花子です
    私の名前はです


メソッドの中にある**selfという引数には、メソッドを呼び出したインスタンスが入る**

なので、「taro.talk()」とメソッドを呼び出すと、selfには「taro」が入っているので、

talk()メソッドの中の「self.name」は「taro.name」を意味することになる

こういった理由で「私の名前は太郎です」という出力が行われている

クラス変数は、**そのクラスから作られたインスタンスからもアクセスできる**

なので、「jiro」というインスタンスのインスタンス変数を定義しなくても、

「jiro.talk()」とすると「self.name」にはクラス変数の「name = ""」が使われて、結果が出力される

同名のインスタンス変数が定義されていると、インスタンス変数が優先になる

**クラス変数とインスタンス変数の使い分け**

クラス変数　　　　→　**インスタンスで共通する値（クラス固有の値）**を持たせる

インスタンス変数　→　**インスタンスごとに異なる値（インスタンス固有の値）**を持たせる

人間をクラスとして表現するときの説明のためにクラス変数を定義しているが、

Humanクラスにはインスタンスで共通する変数がないので、クラス変数は特に必要ない


```
class Human:
  def talk(self):
    print("私の名前は" + self.name + "です")
  
  def walk(self):
    print("歩く")
   
  def sleep(self):
    print("寝る")
    
hanako = Human()
hanako.name = "花子"
hanako.age = 30
hanako.born = "大阪"
hanako.talk()
```

「taro.インスタンス変数名」と何行か書くことによってインスタンス変数を定義したが、

インスタンスごとにこの記述を繰り返すのは面倒

→　以下のように書けると楽（インスタンス生成時にまとめて定義）


```
taro = Human("太郎", 20, "東京")
hanako = Human("花子", 30, "大阪")
```

このような簡単な記述で、インスタンス変数を設定するための機能が**コンストラクタ**である

**コンストラクタとは？**

インスタンス生成時に自動的に呼び出されるメソッド


```
class Human:
  def __init__(self, name, age, born): #コンストラクタ
    self.name = name
    self.age = age
    self.born = born
    
  def talk(self):
    print("私の名前は" + self.name + "です")
  
  def walk(self):
    print("歩く")
   
  def sleep(self):
    print("寝る")
    
hanako = Human("花子", 30, "大阪") #インスタンス生成とインスタンス変数の定義
hanako.talk() #メソッドの呼び出し

taro = Human("太郎", 20, "東京")
taro.talk()
```

    私の名前は花子です
    私の名前は太郎です


「**\_\_init__**」というメソッドを書くとこれがコンストラクタになる

コンストラクタはインスタンス生成のタイミングで呼び出される

→　インスタンス生成時に渡している引数("花子", 30, "大阪")は、コンストラクタに渡される引数

コンストラクタの中ではインスタンス変数の定義がまとめて行われている

第一引数selfには、生成したインスタンス（上の例ではhanako）が入り、

self以外の引数にはインスタンス生成時に渡された引数が入るので、以下の意味になる（taroの場合も同様）


```
hanako.name = "花子"
hanako.age = 30
hanako.born = "大阪"
```

コンストラクタの中の左辺と右辺のどちらにも「name」や「age」という記述があるが、

**左辺はインスタンス変数、右辺は引数**であることに注意

コンストラクタのような特別な意味を持つメソッドを**特殊メソッド**という、コンストラクタの他にも、

インスタンスが消去されるときに呼び出されるデストラクタ「\_\_del__」というものなどがある

## まとめ・ポイント

クラスの2つの機能「**カプセル化**」と「**インスタンスの生成**」について学んだ


**カプセル化**

変数と関数をまとめた上で、外部に公開する情報を制限する機能のこと

カプセル化によって、

*   変数と関数を意味のある一つの部品としてまとめる
*   クラスの外部からのアクセスを制限する

ことが可能になり**グローバル変数問題を解決**することができた

**インスタンスの生成**

クラスからはインスタンスを複数作ることができた

クラス・インスタンスを利用すると、共通の処理をもつ複数の実体を扱うときに、

**クラスの中に共通処理をまとめて記述できる**というメリットがあった

## 演習問題①

合計金額を算出するOrderクラスを定義しましょう！

Orderクラスは以下のようなクラスであるとします

*   インスタンス変数として単価（price）と数量（count）を持つ
*   インスタンス変数の定義はコンストラクタで行われている
*   クラス変数として消費税（tax）を持つ、消費税の値は1.1（10%）とする
*   単価×数量×消費税で合計金額を算出するtotal_priceメソッドを持つ



```
#解答はここに記入

```

## Pythonはすべてがオブジェクト

**Pythonはすべてのデータ型がオブジェクトである**

それぞれのデータ型は以下のクラスから作られたオブジェクト

文字列　→　class str

整数　　→　class int

リスト　→　class list

文字列型のデータは、strクラスのオブジェクトなので**メソッドを持つ**


```
print(type("Hello"))   #type()関数でどのクラスのオブジェクトか確認できる
print("Hello".upper()) #メソッドの呼び出し
```

    <class 'str'>
    HELLO


それぞれのデータ型がどんなメソッドを持つのかは公式ドキュメントで確認できる

[公式ドキュメント（str型のメソッド）](https://docs.python.org/ja/3/library/stdtypes.html#string-methods)

## Pythonにおける値渡しと参照渡し

**値渡しとは？**

関数に実引数の値を**コピーして渡す**方法


```
def hoge(a):
  a += 1
  
b = 10
hoge(b)
print(b)
```

    10


関数hogeには変数bそのものではなくコピーした値を渡すので、

関数hogeが実行されても変数bの値は変わらない

**参照渡しとは？**

関数に実引数の**メモリ上のアドレスを渡す**方法

コンピュータからするとオブジェクトは、

インスタンス変数を格納するために確保されたメモリ領域に過ぎない

**id()関数**を使うとオブジェクトが格納されているメモリアドレスを確認できる


```
c = 1
print(id(c))
```

    10968800



```
def foo(arr):
  arr.append(3)
  
d = [0 ,1, 2]
foo(d)
print(d)
```

    [0, 1, 2, 3]



```
def foo(arr):
  arr.append(3)
  print(id(arr))
  
d = [0 ,1, 2]
print(id(d))
foo(d)
```

    140512000658888
    140512000658888


変数dと関数の仮引数arrそれぞれのメモリアドレスを、id()関数で確認してみると、

どちらも**同じメモリアドレス**を参照していることがわかる　→　**同じオブジェクト**を表している

なので関数fooが実行されると変数dの値が変更されている

ちなみに先程の値渡しの例では、メモリアドレスが異なることも確認できる


```
def hoge(a):
  a += 1
  print(id(a))
  
b = 10
hoge(b)
print(b)
print(id(b))
```

    10969120
    10
    10969088


しかし実は、**Pythonはすべて参照渡し**である（値渡しがない）

なぜ値渡しをしているかのような振る舞いをしているのか？

→　オブジェクトの種類の違いによる（ミュータブルとイミュータブル）

オブジェクトには**変更可能（mutable）な型**と、**変更不可能（immutable）な型がある**

**ミュータブルなデータ型**　　→　リスト型（list）、辞書型（dict）、集合型（set）など

**イミュータブルなデータ型**　→　int,floatなどの数値型、文字列型（str）、タプル型（tuple）など

イミュータブルなデータが更新されると、新たなメモリ領域を確保してもとの値は変更されない


```
def hoge(a):
  print(id(a)) #更新前を見ると参照渡しであることが確認できる
  a += 1
  print(id(a)) #更新をすると新たなメモリアドレスが割り振られる
  
b = 10
print(id(b))
hoge(b)
```

    10969088
    10969088
    10969120


スライドの説明で値渡しによって関数の独立性を高めるという話をした

ミュータブルな型で値渡し（のような振る舞い）をさせるには、**copyモジュールのdeepcopy()関数**を利用する


```
import copy #copyモジュールのインポート

def foo(arr):
  print(id(arr))
  arr = copy.deepcopy(arr) #別のメモリアドレスにオブジェクトのコピーを作成
  print(id(arr))
  arr.append(3)
  print(id(arr))
  
d = [0 ,1, 2]
foo(d)
print(d)
print(id(d))
```

    139914830524680
    139914830524872
    139914830524872
    [0, 1, 2]
    139914830524680


copyモジュールについてより詳しくは[公式ドキュメント](https://docs.python.org/ja/3/library/copy.html)をご参照ください。

# 継承

**既存のクラスが持つ変数とメソッドを引き継いで（継承して）、新しいクラスを作る仕組み**

継承を実装するには以下のように書く（ClassAクラスを継承したClassBクラスを定義）


```
class ClassB(ClassA):
  クラス内部の記述
```

例：SuperClassクラスを継承したSubClassクラスを定義する


```
class SuperClass:
  spr = "スーパー"
  
  def super_print(self):
    print("スーパークラスのメソッドです")


class SubClass(SuperClass): #SuperClassを継承
  def sub_print(self):
    print("サブクラスのメソッドです")
    
  def spr_print(self):
    print(self.spr)
    
    
obj = SubClass()
obj.super_print()
obj.sub_print()
obj.spr_print()
```

    スーパークラスのメソッドです
    サブクラスのメソッドです
    スーパー


上のプログラムを見るとSubClassのオブジェクト（obj）が、

SuperClassのメソッド（super_print）とクラス変数（spr）を使っているのがわかる

このように継承を用いると、継承元のクラスの変数とメソッドを継承先のクラスで利用できる

→　**既存のクラスを修正せずに機能追加が可能になる**　→　**保守性・再利用性の向上**

継承先のクラスをサブクラス（子クラス）、継承元のクラスをスーパークラス（親クラス）と呼ぶ

また継承は見方を変えると、**クラスの共通部分を別クラスとしてまとめる仕組み**とも言える

以下の2つのクラスを見てみると、プログラムの内容に共通する部分が多いことがわかる


```
class Teacher:
  def __init__(self, name):
    self.name = name
    
  def hello(self):
    print("私の名前は" + self.name + "です")

  def teach(self, student):
    print(student + "に教えた")
    
    
class Programmer:
  def __init__(self, name):
    self.name = name
    
  def hello(self):
    print("私の名前は" + self.name + "です")
  
  def programming(self, lang):
    print(lang + "でプログラミング")
```

継承を使うとこのコードの共通部分をまとめることができる


```
class Worker: #共通部分を抽出したWorkerクラスを定義
  def __init__(self, name):
    self.name = name
    
  def hello(self):
    print("私の名前は" + self.name + "です")
    

class Teacher(Worker): #Workerクラスの継承でシンプルな記述に
  def teach(self, student):
    print(self.name + "は、" + student + "に教えた")
    
class Programmer(Worker):
  def programming(self, lang):
    print(self.name + "は、" + lang + "でプログラミング")
    
teacher = Teacher("金八")
programmer = Programmer("松本")
teacher.hello()
teacher.teach("杉田")
programmer.hello()
programmer.programming("Ruby")
```

    私の名前は金八です
    金八は、杉田に教えた
    私の名前は松本です
    松本は、Rubyでプログラミング


TeacherクラスとProgrammerクラスはWorkerクラスを継承している

2つのクラスの共通部分をWorkerクラスにまとめた上で、Workerクラスを継承している



共通部分をまとめると、以下のようなメリットがある

*   プログラムの記述量が少なくなり、見通しが良くなる
*   他の職業のクラスを定義するのが簡単になる
*   共通部分の変更が容易になる

共通部分をまとめることで、**プログラムの保守性・再利用性を向上**させている

継承は「**is-a関係**」と言われる「Teacher is a Worker.（教師は労働者の一種である）」

プログラムの共通部分があっても、**is-a関係が存在しないならば継承でまとめるのは不適切**な場合が多い

## オーバーライド

**スーパークラスのメソッドをサブクラスが上書きする機能**

以下のプログラムを見てみると、スーパークラスとサブクラスで同じ名前のメソッドを持っている


```
class Human:
  def hello(self):
    print("hello")
    
class Japanese(Human):
  def hello(self): #helloメソッドを上書き
    print("I'm Japanese")
    
japanese = Japanese()
japanese.hello()
```

    I'm Japanese


スーパークラスとサブクラスで同じ名前のメソッドがあるとき、**サブクラスのメソッドが優先**された

→　スーパークラスのメソッドを、サブクラスのメソッドで**上書き**していると見ることもできる

→　これをメソッドの**オーバーライド**と呼ぶ

**スーパークラスを表すsuper()関数**を使うと、

スーパークラスのメソッドをサブクラスから利用できる


```
class Human:
  def hello(self):
    print("hello")
    
class Japanese(Human):
  def hello(self):
    super().hello() #スーパークラスのhelloメソッドを呼び出し
    print("I'm Japanese")
    
japanese = Japanese()
japanese.hello()
```

    hello
    I'm Japanese


オーバーライドは、**ポリモーフィズムを実装する**上で極めて重要な機能（後ほど説明）

## まとめ・ポイント

**継承とは**

既存クラスの変数とメソッドを受け継いで新たなクラスを定義する機能のこと

継承を使うメリットとして、以下のようなメリットがあった

*   既存のプログラムを利用したプログラミングが可能になる
*   共通部分をまとめてシンプルなプログラムを書くことができる

このようなメリットによって、保守性と再利用性の高い開発を行うことができる


**オーバーライドとは**

スーパークラスのメソッドをサブクラスが上書きすること

オーバーライドの活躍は、ポリモーフィズムの章で

## 演習問題②

以下の要件を満たすDecoDisplayクラスを定義しましょう！

*   Displayクラスを継承している
*   唯一のメソッドとして、displayメソッドを持つ（オーバーライドしている）
*   引数として渡された文章の先頭と末尾に「～」を追加して表示するメソッド

クラスが定義できたら、インスタンスを生成してメソッドを呼び出してみましょう！


```
class Display:
  def __init__(self):
    print("文章を出力できるよ")
    
  def display(self, sentence):
    print(sentence)
    
#解答はここに記入

```

## 関数に機能を追加する関数（デコレータ）

Pythonはすべてがオブジェクトであるので、

**関数の引数として関数を渡すこともできる**

例：obj関数とfree関数を引数にとるpy関数


```
def obj():
  return "オブジェクト"

def free(word):
  return word

def py(func): #機能を追加するための関数
  def wrapper(*args, **kwargs):
    return "Pythonは" + func(*args, **kwargs)
  return wrapper #関数py()の戻り値は関数

pyobj = py(obj) #関数objを引数としてpy関数を呼び出し、戻り値をpyobjに
pyfree = py(free)
print(pyobj())
print(pyfree("楽しい"))
```

    Pythonはオブジェクト
    Pythonは楽しい


上のプログラムのポイントは、


*   py関数には、関数のオブジェクトが引数として渡されている
*   py関数の戻り値は、py関数の中で定義されたwrapper関数
*   wrapper関数の中の処理では、引数（func）として受け取った関数を呼び出している
*   \*\*argsと\*\*kwargsはそれぞれ、任意の数の引数・任意の数のキーワード引数という意味
*   引数として受け取った関数の戻り値に「Pythonは」という文字を追加している（**機能追加**）



このように関数を引数とする関数を定義することで、

**すでにある関数に機能を追加する**ことができる

このような関数を**デコレータ**と呼ぶ

一般化すると以下のようになる


```
機能追加された関数オブジェクト = デコレータ(機能追加したい関数オブジェクト)
```

デコレータは、**「@デコレータ名」と、修飾する（decorate）関数の上に書く**ことでも利用できる


```
@機能追加の関数（デコレータ）
def 機能追加したい関数():
  処理
```


```
def py(func): #デコレータの定義
  def wrapper(*args, **kwargs):
    return "Pythonは" + func(*args, **kwargs)
  return wrapper #関数py()の戻り値は関数

@py #デコレータの利用
def obj():
  return "オブジェクト"

@py #デコレータの利用
def free(word):
  return word

print(obj())
print(free("楽しい"))
```

    Pythonはオブジェクト
    Pythonは楽しい


デコレータは上のように自分で定義できる他にも、

Pythonにあらかじめ用意されているものや、モジュールとして提供されているものもある

ここでは**クラスメソッド**を定義するために用いるデコレータの**@classmethod**について扱う

**クラスメソッドとは？**

クラス内で定義されたメソッドで、**インスタンス化しなくても呼び出すことのできるメソッド**

クラスからもインスタンスからも呼び出すこともできる


```
class Hoge:
  @classmethod #クラスメソッドを定義
  def foo(cls):
    print("インスタンス化しなくても呼び出せるメソッド")
    
Hoge.foo()
Hoge().foo()
```

    インスタンス化しなくても呼び出せるメソッド
    インスタンス化しなくても呼び出せるメソッド


**クラス（分類）固有**の処理は、クラスメソッドとして定義

**インスタンス（実例）固有**の処理は、メソッドとして定義

# ポリモーフィズム

**呼び出し側のロジック（インターフェース）を一本化すること**

以下のプログラムを見ると、

Dogクラス、Catクラス、Humanクラスがすべてcry()メソッドを持つ


```
class Dog:
  def cry(self):
    print("わんわん")
    
class Cat:
  def cry(self):
    print("にゃーにゃー")
    
class Human:
  def cry(self):
    print("えーんえーん")
    
def nake(animal):
  animal.cry() #どのクラスのオブジェクトか気にすることなくcry()メソッドを使うことができる
  
dog = Dog()
cat = Cat()
human = Human()

#どのクラスのオブジェクトか気にすることなくnake()関数を使うことができる
nake(dog)
nake(cat)
nake(human)
```

    わんわん
    にゃーにゃー
    えーんえーん


このように異なるクラスに共通の名前のメソッドを定義することで、

**どのクラスのオブジェクトかを気にせずcry()メソッドを使用できるようになっている**

**ポリモーフィズムとは？**

オブジェクトの種類を意識しなくてもいいように、オブジェクト間で共通のインターフェースをもたせること

演算子の「＋」にもポリモーフィズムが利用されている

→　数値のオブジェクトなら「足し算」を意味するし、文字列のオブジェクトなら「連結」を意味する

→　**オブジェクトに共通のインターフェースをもたせるが、中身の実装が異なるので振る舞いが変わる**


先程のプログラムのように実装することでポリモーフィズムは実現できるが、

どのクラスにもcry()メソッドを定義する強制力がない

→　**ポリモーフィズムを使わないで実装を行う余地が残ってしまっている**

**ポリモーフィズムによる実装を強制する**方法として、**抽象クラス・抽象メソッド**を利用する方法がある

**抽象（基底）クラスとは？**

継承されることを前提としたクラス、インスタンスを生成できない

**抽象メソッドとは？**

実装を持たない、**インターフェースを規定**するためのメソッド、抽象クラスに定義できる

抽象メソッドは**サブクラスの中のオーバーライドによって実装されなければいけない**

Pythonで抽象クラス・抽象メソッドを利用するには、**ABC (Abstract Base Class)モジュール**を使う

抽象クラスを定義するには**ABCクラス**を継承し、

抽象メソッドを定義するには**@abstractmethodデコレータ**を使用する

以下のプログラムでは、抽象クラスのAnimalを継承しているHumanクラスが、

cry()メソッドの実装を行っていないので、Humanクラスのインスタンス生成時にエラーが発生する


```
from abc import ABC, abstractmethod

class Animal(ABC): #Animalクラスを抽象クラスとして定義
  @abstractmethod #抽象メソッドにするデコレータ
  def cry(self):
    #ここに実装を記述することはできない、抽象メソッドに書けるのはpassのみ
    pass

class Dog(Animal):
  def cry(self):
    print("わんわん")
    
class Cat(Animal):
  def cry(self):
    print("にゃーにゃー")
    
class Human(Animal):
  def cry(self):
    print("えーんえーん")
  
human = Human()
human.cry()
```

    えーんえーん


抽象クラスを継承したクラスは、サブクラス内でオーバーライドして抽象メソッドの実装を行う必要がある

→　Dog、Cat、Humanクラスに、cry()メソッドの実装が強制されている

→　**ポリモーフィズムによる実装を強制できる**

## まとめ・ポイント

**ポリモーフィズムとは**

オブジェクト間で共通のメソッドをもたせることで、インターフェースを統一すること


ポリモーフィズムの実装を強制する方法として抽象メソッド・抽象クラスがあった

抽象メソッドは抽象クラスに定義できるメソッドで、**実装を持たないメソッド**

抽象クラスを継承した**サブクラスでのオーバーライドによる具体的な実装を強制する**機能を持つ

→　ポリモーフィズムを強制するプログラミングが可能に！


抽象クラス・抽象メソッドは、PythonではABCモジュールで提供されている

**ABCクラス**を継承したクラスは抽象クラスになり、

**@abstractmethodデコレータ**をつけたメソッドは抽象メソッドになる

## 演習問題③


```
class Worker:
  def __init__(self, name):
    self.name = name
    
  def hello(self):
    print("私の名前は" + self.name + "です")
    

class Teacher(Worker):
  def teach(self):
    print(self.name + "は、教えた")
    
class Programmer(Worker):
  def programming(self):
    print(self.name + "は、プログラミングした")
```

継承のセクションで登場した上のプログラムを、ポリモーフィズムを使って改善しましょう！

以下の修正を行ってください

*   Workerクラスを抽象クラスに変更
*   Workerクラスに、新たにdo_workメソッドを追加
*   do_workメソッドは、抽象メソッド
*   Teacherクラスのteachメソッドと、Programmerクラスのprogrammingメソッドをdo_workメソッドに置き換える
*   Workerクラスをスーパークラスにもつクラスのオブジェクトを引数として受け取る、instract関数を定義
*   instract関数は渡されたオブジェクトのdo_workメソッドを呼び出す関数

instract関数にTeacherクラスとProgrammerクラスのオブジェクトを渡して、実行してみましょう！



```
#解答はここに記入

```

# OOPがもたらした再利用技術

OOPによって「**プログラムの再利用**」と「**アイデアの再利用**」が可能になった

プログラムの再利用　→　モジュール、フレームワーク

アイデアの再利用　　→　設計原則とデザインパターン

## モジュール

**クラスや関数などを、外部から取り込んで再利用できるようにしたもの**

モジュールは**特定の強みを持つ再利用可能部品**とも言える

openpyxlモジュールであれば、Excel形式のファイルを扱うのに便利なクラスや関数がまとめられている

例：Excel形式のファイルをPythonで扱うことができるopenpyxlモジュール


```
import openpyxl
wb = openpyxl.Workbook() #オブジェクトの生成
wb.create_sheet()        #メソッドの呼び出し
wb.active                #属性の呼び出し
```

上記の記述では、openpyxl.pyに書かれたクラスを利用している


```
#openpyxl.py

class Workbook:
  def __init__(...):
    self.active = ...
  def create_sheet(...):
    ワークシートを追加するようなロジック
  def ...
```

openpyxl.pyにどのような内容のプログラムが書かれているか知らなくても、

*   openpyxl.pyの中に定義されているWorkbookクラスからワークブックを表すオブジェクトを作る
*   そのオブジェクトはアクティブシートを返すactive属性や、新しいシートを作るcreate_sheetメソッドなどを持つ

といった情報**（インターフェース）さえ知っていれば使うことができる**

→　モジュールはプログラムの再利用性の象徴的な存在

## フレームワーク

**よくある処理の流れをひな形として用意しておくことで、**

**開発者はプロジェクト固有の内容を埋めるだけで、開発が可能になるもの**

Webアプリケーションフレームワーク　→　Djangoなど

Webスクレイピングフレームワーク　　→　Scrapyなど

共通の処理の流れを抽象クラスが持つメソッドで実装（**インターフェースに対して実装**）しておいて、

プロジェクト固有の内容は、オーバーライドによって開発者が実装する（ひな形を埋める）

## 設計原則とデザインパターン

**オブジェクト指向設計原則とは？**

オブジェクト指向を使って保守性と再利用性の高い開発を行うための設計原則

**SOLIDの原則**

*   単一責任の原則（**S**ingle Responsibility Principle）
*   オープン・クローズドの原則（**O**pen-Closed Principle）
*   リスコフの置換原則（**L**iskov Substitution Principle）
*   インタフェース分離の原則（**I**nterface Segregation Principle）
*   依存性逆転の原則（**D**ependency InversionPrinciple）



**デザインパターンとは？**

オブジェクト指向の設計原則を、具体的な23個の実装パターンとしてカタログ化したもの

オブジェクト指向のアイデアを開発に最大限活かすためには**必須の知識**

この講座でオブジェクト指向の概要を理解することができた後に学習すべき内容

# OOPを最大限活用するために

## OOPはあくまでも手段である

OOPはあくまでも生産性の高い開発を行うための手段に過ぎない

開発に無理やりOOPのアイデアを取り込もうとして、生産性が落ちては本末転倒


## 設計原則とデザインパターンを学ぼう

概念を学んだだけではOOPを理解した「つもり」になってしまう

OOPを実際の開発に使うためには、設計原則やデザインパターンの学習など、

より踏み込んだ学習が不可欠

## Python以外の言語も学ぼう

オブジェクト指向の良書はJavaで書かれたものが多い

Javaを読めるようになっておくと、アクセスできるOOPの情報が増えるのでおすすめ

他の言語を学ぶことでPythonの特徴が浮き彫りになる

## 参考文献・おすすめ書籍

平澤 章「オブジェクト指向でなぜつくるのか 第2版」日経BP社 2011年

谷本 心 ほか「Java本格入門 ~モダンスタイルによる基礎からオブジェクト指向・実用ライブラリまで」技術評論社 2017年

コーリー・アルソフ「独学プログラマー Python言語の基本から仕事のやり方まで」清水川 貴之 監訳ほか 日経BP社 2018年

# 演習問題の解答（復習にご活用ください^^）

## 演習問題①

合計金額を算出するOrderクラスを定義しましょう！

Orderクラスは以下のようなクラスであるとします

*   インスタンス変数として単価（price）と数量（count）を持つ
*   インスタンス変数の定義はコンストラクタで行われている
*   クラス変数として消費税（tax）を持つ、消費税の値は1.1（10%）とする
*   単価×数量×消費税で合計金額を算出するtotal_priceメソッドを持つ



```
class Order:
  tax = 1.1
  
  def __init__(self, price, count):
    self.price = price
    self.count = count
    
  def total_price(self):
    total = self.price * self.count * Order.tax
    total = int(total)
    print("合計金額は" + str(total) + "円です")
    
order1 = Order(100, 10)
order1.total_price()
order2 = Order(128, 8)
order2.total_price()
```

    合計金額は1100円です
    合計金額は1126円です


## 演習問題②

以下の要件を満たすDecoDisplayクラスを定義しましょう！

*   Displayクラスを継承している
*   唯一のメソッドとして、displayメソッドを持つ（オーバーライドしている）
*   引数として渡された文章の先頭と末尾に「～」を追加して表示するメソッド

クラスが定義できたら、インスタンスを生成してメソッドを呼び出してみましょう！


```
class Display:
  def __init__(self):
    print("文章を出力できるよ")
    
  def display(self, sentence):
    print(sentence)
    
class DecoDisplay(Display):
  def display(self, sentence):
    print("～" + sentence + "～")
    
deco = DecoDisplay()
deco.display("キャビアを添えて")
```

    文章を出力できるよ
    ～キャビアを添えて～


## 演習問題③

継承のセクションで登場した上のプログラムを、ポリモーフィズムを使って改善しましょう！

以下の修正を行ってください

*   Workerクラスを抽象クラスに変更
*   Workerクラスに、新たにdo_workメソッドを追加
*   do_workメソッドは、抽象メソッド
*   Teacherクラスのteachメソッドと、Programmerクラスのprogrammingメソッドをdo_workメソッドに置き換える
*   Workerクラスをスーパークラスにもつクラスのオブジェクトを引数として受け取る、instract関数を定義
*   instract関数は渡されたオブジェクトのdo_workメソッドを呼び出す関数

instract関数にTeacherクラスとProgrammerクラスのオブジェクトを渡して、実行してみましょう！



```
from abc import ABC, abstractmethod

class Worker(ABC):
  def __init__(self, name):
    self.name = name
    
  def hello(self):
    print("私の名前は" + self.name + "です")
    
  @abstractmethod
  def do_work(self):
    pass

  
class Teacher(Worker): #Workerクラスの継承でシンプルな記述に
  def do_work(self):
    print(self.name + "は、教えた")
    
class Programmer(Worker):
  def do_work(self):
    print(self.name + "は、プログラミングした")
    
    
def instract(worker):
  worker.do_work()
  
    
teacher = Teacher("金八")
programmer = Programmer("松本")
instract(teacher)
instract(programmer)
```
