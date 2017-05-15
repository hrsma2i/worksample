### lamdaについて
**lamda式**とは、関数定義の簡単な書き方です。

#### 目次
1. 使い方
2. いつ使うのか
3. 具体的な使うタイミング1 - sorted
4. 具体的な使うタイミング2 - reduce
5. 無闇に使わない

---

#### 使い方
例えば、以下の関数を定義するとします。
- 引数: 数値x, y
- 返り値: xの2乗にyを足した値

**def**を用いた場合
```python
def f(x, y):
    return x**2 + y
```
**lambda**を用いた場合
```python
f = lamda x, y: x**2 + y
```
結局、**defでできることと本質は変わりません**。では、どんなときにわざわざlamda関数を使うのでしょうか。それを次の項目で見ていきます。

---

#### いつ使うのか

- **一回しか**関数の呼び出しの記述をしない
- **簡単**な処理

を満たす関数を定義するときに使います。

これだけだとわかりづらいので、次の項から具体的な使い方を見ていきます。

---

#### 具体的な使うタイミング1 - sorted
objectのlistを並べ替えるときに、**objectのある値に着目**しなければなりません。**sorted**と**lamda**を用いて行います。

**sorted**関数
- 引数: list
- 返り値: 並べ替えたlist

e.g. `sorted([4,2,5])` -> `[2,4,5]`

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
# sorted関数は、l1を並べ替えるときに、
# keyで渡した関数をl1の各要素に施した値を見て、並べ替えます。
# e.g. lamdaで定義した関数を、l1の各要素に施すと以下の通り。
# sato      -> 172
# sasaki    -> 168
# takahashi -> 176
# この値を見て並べ替える。

# 身長順に表示
for student in l2:
    print(student.name + ': ' + str( student.height ))

# 結果
#
# sasaki: 168
# sato: 172
# takahashi: 176
```

---

#### 具体的な使うタイミング2 - reduce
reduce関数は、以下のようなときに使います。

**sum**は足し算ですが、それの**掛け算バージョン**の関数がほしいとき、以下のように実装できます。

**for**を用いた実装
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

**reduce**を用いたsimpleな実装
```python
from functools import reduce

l1 = [1, 2, 3, 4]

print(reduce(lambda x, y: x*y, l1))

# 結果
# 24
```

reduce関数について
- 引数
	1. 二項演算（引数が2つで、返り値が1つ）の関数
	2. list
- 返り値
	1. 数値

どう処理してるか
1. 「listの0番目」と「1番目の要素」に二項演算を施す。  
e.g. `l1[0]*l2[1]` -> `1*2 -> 2`
2. listの要素が尽きるまで以下を繰り返す  
	1. 「二項演算の結果」と、「listの次の要素」に二項演算を施す。  
e.g. `2*l2[2] -> 2*3 -> 6`
3. 二項演算の結果を返す。  
e.g. 24

---

#### 無闇に使わない
lamdaは関数の記述を1行に収めます。なのでコードの**可読性（読みやすさ）が落ちる**ことがあります。以下の使うタイミングを改めて確認しましょう

**lamdaを使うタイミング**
- 関数の呼び出しの記述を**一回しか**しない
- 関数の処理の内容が**簡単**

sorted、reduceの例もこの条件を満たしています。この条件を満たさないときは、defを使いましょう。
