### lamdaについて
**lamda式**とは、関数定義の簡単な書き方です。

#### 使い方
以下は「xとyを受け取り、xの2乗にyを足した値を返す」関数を、今まで通り**defを用いて定義**したコードになります。
```python
def f(x, y):
    return x**2 + y
```
そして、lamdaを用いて同じことをすると以下のようになります。
```python
f = lamda x, y: x**2 + y
```
結局、defでできることと本質は変わりません。では、どんなときにわざわざlamda関数を使うのでしょうか。それを次の項目で見ていきます。

#### いつ使うのか
- 関数の呼び出しの記述を一回しかしない
- 関数の処理の内容が簡単

上の条件を満たすときが、lamdaを使う主なタイミングです。

これだけだとわかりづらいので、次の項から具体的な使い方を見ていきます。

#### sorted
objectのlistをsortするときに、objectのある値に着目しなければなりません。その場合、以下の用に記述することができます。
```python
# Studentクラスの定義
class Student:
    def __init__(self, height, name):
        self.height = height
        self.name = name

# Studentオブジェクトのインスタンス化
sato = Student(172, 'sato')
sasaki = Student(168, 'sasaki')
takahashi = Student(176, 'takahashi')

l1 = [sato, sasaki, takahashi]

# l1を身長順に並べ替え
l2 = sorted(l1, key=lambda s:s.height)
# sorted関数とは
# 引数: list
# 返り値: 受け取ったlistの中身が数値なら昇順、文字列なら辞書順に並べ替えたlist
# e.g. sorted([4,2,5])  -> [2,4,5]
#
# key引数に、ある関数を渡すことにより、
# 引数で与えられたlistの要素を並べ替えるときに、
# 各要素にその関数が施され、その返り値の値によって並べ替えが行われる。
#
# 下の例では、l1の要素であるStudentオブジェクトに、
# lamda式で定義した関数を施すことにより、
# 各Studentのheightの値によって昇順にならべかえられたlistがl2に格納されている。

# 身長順に表示
for student in l2:
    print(student.name + ': ' + str( student.height ))

# l2の結果
#
# sasaki: 168
# sato: 172
# takahashi: 176
```

##### map
##### filter
##### reduce

#### 無闇に使わないほうがいい

1回こっきりしか使わないとき
