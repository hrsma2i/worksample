**selfはインスタンスオブジェクト自身**を指しています。クラスの中でインスタンスオブジェクトを呼び出す際は、selfを使用します。以下のコードを用いて、具体的に説明します。

```python
# 車のクラス
class Car:
    def __init__(self, position, speed):
        # 現在位置
        self.position = position
        # 速さ
        self.speed = speed

    # 車をtime時間走らせ、その位置を変える。
    def run(self, time):
        self.position += self.speed * time

# 位置0, 速さ30km/hで走る車をインスタンス化
my_car = Car(0, 30)

# 2時間走らせる
my_car.run(2)

# 走らせた後の距離を出力
print(my_car.position + "km")

# 結果
# 60km 
```

selfはインスタンス自身を指すので、この例では、**selfはmy_car**を指します。これを踏まえた上で、runメソッドの動きを見てみましょう。

`my_car.run(2)`を実行すると、`self.position += self.speed * time`が実行されますが、このselfが`my_car`自身(self)になります。

なのでrunによって、`my_car`自身の位置`my_car.position`に、(`my_car`自身の速さ`my_car.speed`)×走行時間time = 走行距離が足されていることがわかります。

以上のように、**selfはインスタンスオブジェクト自身**を指し、class内でそのインスタンスを呼び出す際にselfと記述します。
