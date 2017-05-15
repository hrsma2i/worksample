### lamdaについて
**lamda式**とは、関数定義の簡単な書き方です。

#### 目次
1. 使い方
2. いつ使うのか
3. sorted
4. reduce
5. 無闇に使わない

---

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

---

#### いつ使うのか
- 関数の呼び出しの記述を**一回しか**しない
- 関数の処理の内容が**簡単**

上の条件を満たすときが、lamdaを使う主なタイミングです。

これだけだとわかりづらいので、次の項から具体的な使い方を見ていきます。

---

#### sorted
objectのlistを並べ替えるときに、objectのある値に着目しなければなりません。その場合、以下の用に記述することができます。

sorted関数について
- 引数: list
- 返り値: 受け取ったlistの中身が数値なら昇順、文字列なら辞書順に並べ替えたlist  
e.g. sorted([4,2,5]) -> [2,4,5]

key引数に、ある関数を渡すことにより、引数で与えられたlistの要素を並べ替えるときに、各要素にその関数が施され、その返り値の値によって並べ替えが行われます。

以下のコードで、生徒のオブジェクトを持つlistを身長順に並べ替えます。

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
#
# 上の例では、l1の要素であるStudentオブジェクトに、
# lamda式で定義した関数を施すことにより、
# 各Studentのheightの値によって昇順にならべかえられたlistがl2に格納されている。
# e.g. satoに与えたlamda式を施すと、sato.heigtの172が返り、その値を見てsortedが並べ替えを行っている。

# 身長順に表示
for student in l2:
    print(student.name + ': ' + str( student.height ))

# l2の結果
#
# sasaki: 168
# sato: 172
# takahashi: 176
```

---

#### reduce
reduce関数は、以下のようなときに使います。

listを受け取って、その要素の値を全て足しあわせた値を返す関数には、sumがあります。
しかし、listの要素を全て掛け合わすような組み込み関数はありません。
そこでforを使うことによって以下のように実装できます。

```python
l1 = [1, 2, 3, 4]

def f(ls):
    # ls: 掛け合わたいlist

    # res: 返り値
    # 初めにlsの最初の要素を格納
    res = ls[0]

    # resにlsの2つ目（indexは1）の要素をかけ合わせていく
    for x in ls[1:]:
        res *= x

    return res

print(f(l1))

# 結果
# 24
```

しかし、reduce関数とlamdaを使うと以下のように、よりsimpleに実装できます。

```python
from functools import reduce

l1 = [1, 2, 3, 4]

print(reduce(lambda x, y: x*y, l1))

# 結果
# 24
```

reduce関数について解説していきます。まず、引数と返り値は以下のようになります。

- 引数
	1. 二項演算（引数が2つで、返り値が1つ）の関数
	2. list
- 返り値
	1. 数値

次にreduce関数がどう動くのかについて説明します。
1. 「listの0番目」と「1番目の要素」に二項演算を施す。(e.g. `l1[0]*l2[1]` -> `1*2 -> 2`)
2. listの要素が尽きるまで以下を繰り返す
	1. 「二項演算の結果」と、「listの次の要素」に二項演算を施す。(e.g. `2*l2[2] -> 2*3 -> 6`)
3. 二項演算の結果を返す。(e.g. 24)

---

#### 無闇に使わない
lamda関数を使うことで文字数を少なく書くことができます。しかし、多用すると、コードの可読性（読みやすさ）が下がります。なので、復習になりますが、以下のタイミングで使うようにしましょう。

- 関数の呼び出しの記述を**一回しか**しない
- 関数の処理の内容が**簡単**

今回の例で取り扱ったsorted, reduceでの利用も、これらの条件を満たしています。この条件を満たさない場合は、defで関数を定義するよにしましょう。
