# Python Design Pattern
Udemy Pythonでマスターするデザインパターン



```python
ls = ['a','b','c']
```


```python
for idx, x in enumerate(ls):
    print(idx, x)
```

    0 a
    1 b
    2 c



```python
# グローバル変数

def printAnimal():
    global animal # 関数の外も変わるようになる
    animal = 'Cat'
    print('関数内animal = {}, id = {}'.format(animal, id(animal)))  # idはメモリアドレスのどこを指しているのかの確認
    
# animal = 'Dog'
printAnimal()

print('関数外animal = {}, id = {}'.format(animal, id(animal))) # 関数の外でもanimalが使える
```

    関数内animal = Cat, id = 2733771909680
    関数外animal = Cat, id = 2733771909680


## inner関数 : 関数の内部に関数を書く
- 外の関数の外からアクセスできないような関数を作成する
- 関数の中で同じ処理が何度も発生する場合に，一つにまとめる
- デコレータ関数を作成する


```python
# inner関数とノンローカル変数

def outer():
    outer_value = '外側の変数'
    def inner():
        nonlocal outer_value  # 外側の変数も書き換えられるようになる
        outer_value = '内側の変数'
        print('inner : outer_value = {}, id = {}'.format(outer_value, id(outer_value)))
#         print('A')
    inner()
    print('outer : outer_value = {}, id = {}'.format(outer_value, id(outer_value)))    
# inner()  # 外からは呼び出せない
outer()




```

    inner : outer_value = 内側の変数, id = 2733788794768
    outer : outer_value = 内側の変数, id = 2733788794768



```python
# ジェネレータ関数 : yield 
# 実行 : next()

def generator(max):
    print('ジェネレータ作成')
    for n in range(max):
        yield n  # nの値を呼び出し元に返す
        print('yield実行')
        
gen = generator(10)

for x in gen:
    print('x = {}'.format(x))
# n = next(gen)
# print('n = {}'.format(n))

# n = next(gen)
# print('n = {}'.format(n))
```

    ジェネレータ作成
    x = 0
    yield実行
    x = 1
    yield実行
    x = 2
    yield実行
    x = 3
    yield実行
    x = 4
    yield実行
    x = 5
    yield実行
    x = 6
    yield実行
    x = 7
    yield実行
    x = 8
    yield実行
    x = 9
    yield実行



```python
# ジェネレータ関数
# send() : yieldで呈している箇所に値を送る
# throw() : 指定した例外が発生して処理が終了させる
# close() : ジェネレータを正常終了させる

def generator(max):
    print('ジェネレータ作成')
    for n in range(max):
        try:
            x = yield n  # nの値を呼び出し元に返す sendを使うときには格納先の変数を作成する
            print('x = {}'.format(x))
            print('yield実行')
        except ValueError as e:
            print('Throwを実行しました')
gen = generator(10)
next(gen)
gen.send(100)
# gen.throw(ValueError('Invalid Value'))
# gen.close()
next(gen)

# for x in gen:
#     print('x = {}'.format(x))

# send
```

    ジェネレータ作成
    x = 100
    yield実行
    x = None
    yield実行





    2




```python
## サブジェネレータ yield from


```


```python
# list, generator memory  :  listよりもgeneratorの方がメモリ使用量が小さい

import sys

list_a = [i for i in range(100000)]

def num(max):
    i = 0
    while i < max:
        yield i
        i += 1
        
# for i in num(100000):
#     print(i)
gen = num(100000)
print(sys.getsizeof(list_a))  # getsizeof : メモリ使用量の表示
print(sys.getsizeof(gen))
```

    824464
    120



```python
# デコレータ関数

# 関数を引数に持つ関数(deoratorとして使う)を，引数になる関数の上の行に@decoratorとして書くと，引数として下の関数を与えることができる
# 同じ処理を使いたいときに利用

def my_decorator(func):
    def wrapper(*args, **kwargs):
        print(args[0])
        if args[0] == 1:
            return 1
        
        print('*' * 100)
        func(*args,**kwargs)
        print('*' * 100)
    return wrapper
    
@my_decorator
def func_a(*args, **kwargs):
    print('func_aを実行')
    print(args)
    
@my_decorator
def func_b(*args, **kwargs):
    print('func_bを実行')
    print(args)
    
    
func_a(1, 2, 3)
func_b(2, 2, 3)
```

    1
    2
    ****************************************************************************************************
    func_bを実行
    (2, 2, 3)
    ****************************************************************************************************



```python
# インスタンスメソッド，クラスメソッド，スタティックメソッド

class Human:
    
    class_name = "Human"  # クラス変数
    
    def __init__(self, name, age):  # コンストラクタ
        self.name = name
        self.age = age
        
    def print_name_age(self):  # インスタンスメソッド
        print('インスタンスメソッド実行')
        print('name = {}, age = {}'.format(self.name, self.age))
    
    @classmethod  # インスタンス化せずに直接実行できる．しかし，コンストラクタの変数を呼び出すことはできない.
    def print_class_name(cls, msg):  # クラスメソッド
        print('クラスメソッド実行')
        print(cls.class_name)  # クラス変数
        print(msg)
        
    @staticmethod
    def print_msg(msg): # スタティックメソッド
        print('スタティックメソッド実行')
        print(msg)
        
# クラスメソッド
Human.print_class_name('こんにちは')
        
# インスタンスメソッド
man = Human('桜木', 18)  # インスタンス化
man.print_name_age()  # インスタンスメソッド実行

# スタティックメソッド
man.print_msg('Hello static')

Human.print_msg('Hello static')
```

    クラスメソッド実行
    Human
    こんにちは
    インスタンスメソッド実行
    name = 桜木, age = 18
    スタティックメソッド実行
    Hello static
    スタティックメソッド実行
    Hello static



```python
# 特殊メソッド

class Human:
    
    def __init__(self, name, age, phone_number):
        self.name = name
        self.age = age
        self.phone_number = phone_number
        
    def __str__(self):  # 文字列変換 str()
        return self.name + ',' + str(self.age) + ',' + self.phone_number
    
    def __eq__(self, other):  # ==で呼ばれる
        return (self.name == other.name) and (self.phone_number == other.phone_number)
    
    def __hash__(self):  # ハッシュを返す  hash() 辞書型でkeyに値を入れる時，set型で値を入れる時に使う
        return hash(self.name + self.phone_number)
    
    def __bool__(self):  # ifで呼ばれる
        return True if self.age >= 20 else False
    
    def __len__(self):  # len()で呼ばれる
        return len(self.name)
    
    
man = Human('Taro', 20, '08011111111')
man2 =Human('Taro', 18, '08011111111')
man3 =Human('Jiro2', 18, '09011111111')


man_str = str(man)
print(man_str)

print(man == man2)

# setに入れる時はhashが定義されていないといけない．setに入れる時は内部的にhash化されて値が入っている
set_men = {man, man2, man3}
for x in set_men:
    print(x)
    
if man:
    print('manはTrue')
if man2:
    print('man2はTrue')

print(len(man))
print(len(man3))
```

    Taro,20,08011111111
    True
    Taro,20,08011111111
    Jiro2,18,09011111111
    manはTrue
    4
    5


## オブジェクト指向


```python
# クラスの継承

class Person:  # 親クラス
    def __init__(self, name, age):
        self.name = name
        self.age = age
        
    def greeting(self):
        print('hello {}'.format(self.name))
        
    def say_age(self):
        print('{} years old'.format(self.age))
        
class Employee(Person):  # Personの機能を継承
    
    def __init__(self, name, age, number):
        super().__init__(name, age)  # 親クラスのコンストラクタのnameとageを初期化
        self.number = number
        
    def say_number(self):
        print('my number is {}'.format(self.number))
        
    def greeting(self):  # オーバーライド
        super().greeting()
        print('I\'m employee phone number = {}'.format(self.number))
        
#     def greeting(self, age) :  # オーバーロード pythonではできない
#         print('オーバーロード')
        
man = Employee('Tonegawa', 45, '08011111111')
man.greeting()
man.say_age()
man.say_number()
```

    hello Tonegawa
    I'm employee phone number = 08011111111
    45 years old
    my number is 08011111111


### メタクラス
- メタクラスは，クラスの再定義をするクラスという風に考えればよい，
- (デフォルトのメタクラスはtypeクラスだが，自身で定義することもできる)
- class定義時に，継承と同じ形でmetalass=メタクラス名とすると自身のメタクラスを指定できます
- メタクラスは，主にその定義でよいのかクラスを検証する際に用いられます


```python
# 定義は以下のようにします
class Meta(type):
    def __new__(metacls, name, bases, clss_dict):
        # クラスのチェックを行う
        # name: クラスの名前
        # bases: 継承しているクラス
        # class_dict: クラスの持っている値，関数等
        return super().__new__(metacles, name, bases, class_dict)  #typeクラスでクラスを生成する
```


```python
# メタクラス # クラスを再定義するクラス

class MetaException(Exception):
    pass

class Meta1(type):  # type(デフォルトのメタクラス)
    
    def __new__(metacls, name, bases, class_dict):
        print('metacls = {}'.format(metacls))
        print('name = {}'.format(name))
        print('bases = {}'.format(bases))
        print('class_dict = {}'.format(class_dict))
#         if 'my_var' not in class_dict.keys():
#             raise MetaException('my_varを定義してください．')
        for base in bases:  # 継承しているクラス
            if isinstance(base, Meta1):
                raise MetaException('継承できません')  # finalクラス
        
        return super().__new__(metacls, name, bases, class_dict)


class ClassA(metaclass=Meta1):
    a ='123'
    my_var = 'AAA'
    pass

class SubClassA(ClassA):
    pass
```

    metacls = <class '__main__.Meta1'>
    name = ClassA
    bases = ()
    class_dict = {'__module__': '__main__', '__qualname__': 'ClassA', 'a': '123', 'my_var': 'AAA'}
    metacls = <class '__main__.Meta1'>
    name = SubClassA
    bases = (<class '__main__.ClassA'>,)
    class_dict = {'__module__': '__main__', '__qualname__': 'SubClassA'}



    ---------------------------------------------------------------------------

    MetaException                             Traceback (most recent call last)

    <ipython-input-9-c075e83174f0> in <module>
         25     pass
         26 
    ---> 27 class SubClassA(ClassA):
         28     pass


    <ipython-input-9-c075e83174f0> in __new__(metacls, name, bases, class_dict)
         15         for base in bases:  # 継承しているクラス
         16             if isinstance(base, Meta1):
    ---> 17                 raise MetaException('継承できません')
         18 
         19         return super().__new__(metacls, name, bases, class_dict)


    MetaException: 継承できません


### ポリモーフィズム
- サブクラスを複数作成しｍ同じメソッドを定義して呼び出す際にどのクラスか意識せずに呼び出すこと
- from abc import abstractmethod, ABCMeta




```python
# 例

class Human(metaclass=ABCMeta):  # 親クラス
    def __init__(self, name):
        self.name = name
        
    @abstructmethod  # 親クラスでは処理を定義しないメソッド．子クラスで必ずオーバーライドする(オーバーライドしないとABCMetaにはじかれる)
    def say_something(self):
        pass
    def say_name(self):  # 共通メソッド
        print(self.name)
        
class HumanA(human):
    def say_something(self):
        print("I'm A")
        
class HumanB(Human):
    def say_something(self):  # 同じ名前のものを定義
        print("I'm B")
```


    ---------------------------------------------------------------------------

    NameError                                 Traceback (most recent call last)

    <ipython-input-10-46605aade761> in <module>
    ----> 1 class Human(metaclass=ABCMeta):  # 親クラス
          2     def __init__(self, name):
          3         self.name = name
          4 
          5     @abstructmethod  # 親クラスでは処理を定義しないメソッド．子クラスで必ずオーバーライドする(オーバーライドしないとABCMetaにはじかれる)


    NameError: name 'ABCMeta' is not defined



```python
# ポリモーフィズム

from abc import abstractmethod, ABCMeta

class Human(metaclass=ABCMeta):  # 親クラス
    
    def __init__(self, name):
        self.name = name
        
    @abstractmethod
    def say_something(self):
        pass
    
    def say_name(self):
        print(self.name)
        
class Woman(Human):
    
    def say_something(self):  # 抽象メソッドを子クラスで定義しなければエラーが出る
        print('女性: 名前は{}'.format(self.name))
    pass

class Man(Human):
    
    def say_something(self): 
        print('男性: 名前は{}'.format(self.name))
    pass


# ポリモーフィズム
num = input('0か1を入力してください')
if num == '0':
    human = Man('Taro')
    
elif num == '1':
    human = Woman('Hanako')
    
else:
    print('入力が間違っています')
human.say_something()

```

    0か1を入力してください 0


    男性: 名前はTaro


### Private変数
- クラス変数，インスタンス変数は外部からアクセスして値を書き換えられる
- Private変数は外部からアクセスできない変数
- __variable  # Private変数 アンダースコア(_)を二つつけて定義
- ただしPythonは厳密なPrivate変数はなく，外部からアクセスする方法もある．


```python
# Private変数

class Human:
    
    __class_val = 'HUman'
    
    def __init__(self, name, age):
        self.__name = name  # Private変数
        self.__age = age
        
    def print_msg(self):
        print('name = {}, age = {}'.format(self.__name, self.__age))

human = Human('Taro', 15)
# print(human._Human__name)  # アクセスできなくもない

human.print_msg()
# print(human.__name)  # アクセスできない
```

    Taro
    name = Taro, age = 15


### カプセル化，setter,getter
Private変数を使う場合には，クラスの外部から変数が見えないようにする(カプセル化)必要があります．カプセル化をする場合には，getterとsetterを定義して，利用しないと変数にアクセスができないようにします．


```python
# カプセル化の方法１
# def get_変数名():
# def set_変数名():
# 変数名 = property(get_変数名,set_変数名)
        
```


```python
# getter , setter その1

class Human:
    
    def __init__(self, name, age):
        self.__name = name
        self.__age = age
        
    def get_name(self):  # 参照されるときに呼ばれる
        print('getter name を呼び出しｍした')
        return self.__name

    def get_age(self):
        print('getter age を呼び出しました')
        return self.__age

    def set_name(self, name):  # 値の代入をするときに呼ばれる
        print('setter name を呼び出しました')
        self.__name = name

    def set_age(self, age):
        print('setter age を呼び出しました')
        self.__age = age

    name = property(get_name, set_name)  # nameを指定するとget_name, set_nameが呼び出される
    age = property(get_age, set_age)

    def print_msg(self):
        print(self.name, self.age)
            
human = Human('Taro', 15)
human.name ='Jiro'
human.age = 18
        
print(human.name)
print(human.age)
```

    setter name を呼び出しました
    setter age を呼び出しました
    getter name を呼び出しｍした
    Jiro
    getter age を呼び出しました
    18


### カプセル化の方法2
- @property,@var.setter


```python
# setter, getter その２

class Human:
    
    def __init__(self, name, age):
        self.__name = name
        self.__age = age
        
    @property  # getter
    def name(self):  # getter
        print('getter nameが呼ばれました')
        return self.__name
    
    @property
    def age(self):
        print('getter ageを呼ばれました')
        return self.__age
    
    @name.setter
    def name(self, name):
        print('setter nameが呼ばれました')
        self.__name = name
        
    @age.setter
    def age(self, age):
        print('setter ageが呼ばれました')
        if age < 0:
            print('0以上の値を設定してください')
            return 
        self.__age = age 
        
human = Human('Koichi', 22)
human.name = 'Makoto'
print(human.name)
human.age = -1
print(human.age)
```

    setter nameが呼ばれました
    getter nameが呼ばれました
    Makoto
    setter ageが呼ばれました
    0以上の値を設定してください
    getter ageを呼ばれました
    22



```python

```


```python

```


```python

```


```python

```


```python

```

## SOLIDの原則

### 単一責任の原則(Single responsibility principle)
すべてのモジュールとクラスは1つの役割を提供して責任を持つべきとする原則
- 役割が複数ある場合，各クラスが何なのか複雑化してしまう．
- 1つの役割に別の役割が複雑に依存してしまい，1つの役割の変更が別の役割に影響を与える可能性があり，保守性の低下を招く

**単一責任の原則を守った場合**
- クラス，モジュールを分けることで，可読性が向上する
- 役割を分けることで他のクラスから利用できやすくなり拡張性が向上する


```python
# single_responsibility

class UserInfo:
# ユーザ情報を保持する役割   
    def __init__(self, name, age, phone_number):
        self.name = name
        self.age = age
        self.phone_number = phone_number
        
    def __str__(self):
        return "{}, {}, {}".format(
            self.name, self.age, self.phone_number
        )
# 役割から外れた関数なので別のclassを作成する
#     def write_str_to_file(self, filename):
#         with open(filename, mode='w') as fh:
#             fh.write(str(self))


class FileManager:
    
    @staticmethod
    def write_str_to_file(obj, filename):  # オブジェクトを引数に持つ
        with open(filename, mode='w') as fh:
            fh.write(str(self))
    
user_info = UserInfo('Taro', 21, '000-0000-0000')
print(str(user_info))

# FileManager.write_str_to_file(user_info, 'tmp.txt')
```

    Taro, 21, 000-0000-0000


## 解放閉鎖の原則
クラス，モジュール，関数等のソフトェアの部品は拡張に対しては開いて庵，修正に対s手は閉じていなければならない
- アプリケーションに変更があった場合，コードを追加して拡張し，用意に対応できる
- モジュールの動作を変更する場合には，ソースコードを直接修正するようなことはないようにする



### 【メリット】
- 機能拡張が容易になりソフトウェアの拡張性が向上する
- 機能拡張時にテストをする際に，拡張機能だけを実施すればよく，元のソースコードはテストをしなくてよいため，保守性も向上する




```python

```


```python

```

## Design Pattern


```python

```


```python

```


```python

```

## 6. Flyweightパターン
- ボクシングのフライ級のこと
- オブジェクトを共有することでメモリの使用量を少なくするために用いる
- すでに存在する場合は存在するものを返す

### 目的
オブジェクトを共有して，メモリの使用量を下げること

### 仕組み
- Flyweightを作成する
- Flyweightをインスタンスとして所有するFlyweightFactoryを作成する

### 構成要素
- Flyweight : 普通に用いるとメモリ使用量が大きいため，共有したほうがよいものを表す
- FlyweightFactory : Flyweightを作成する工場の役.FlyweightFactoryを利用してFlyweightを作成するとオブジェクトが共有される


```python
class User:
    
    def __init__(self, name='', age=''):
        self.__name = name
        self.__age = age
        
    @property
    def name(self):
        return self.__name
    
    @name.setter
    def name(self, name):
        self.__name = name
     
    @property
    def age(self):
        return self.__age
    
    @age.setter
    def age(self, age):
        self.__age = age
    
    def __str__(self):
        return f'name: {self.name}, age: {self.age}'
    
# FlyWeightFactory
class UserFactory:
    
    __instances = {}
    
    @classmethod
    def get_instance(cls, id):
        if id not in cls.__instances:
            user = User()
            cls.__instances[id] = user
            return user
        return cls.__instances.get(id)
    
user1 = UserFactory.get_instance(1)

user1.name = 'Taro'
user1.age = 20

user2 = UserFactory.get_instance(2)

user3 = UserFactory.get_instance(1)

print(id(user1))
print(id(user2))
print(id(user3))
print(user3)
user3.name = 'Hanako'
print(user1)
```

    2154054732424
    2154054354184
    2154054732424
    name: Taro, age: 20
    name: Hanako, age: 20



```python
class FlyweightMixin:
    
    _instances = {}
    
    @classmethod
    def get_instance(cls, *args, **kwargs):
        if (cls, *args) not in cls._instances:
            new_instance = cls(**kwargs)
            cls._instances[(cls, *args)] = new_instance
            return new_instance
        else:
            return cls._instances.get((cls, *args))


class User(FlyweightMixin):
    
    def __init__(self, name, age):
        self.name = name
        self.age = age
        
class Car(FlyweightMixin):
    
    def __init__(self, brand, model):
        self.brand = brand
        self.model = model
        
        
user = User.get_instance(1, name='Taro', age=21)
user2 = User.get_instance(1)
print(id(user), id(user2))
car = Car.get_instance(1, brand='Toyota', model='Prius')
print(car.brand)
```

    2154054723272 2154054723272
    Toyota



```python

```
