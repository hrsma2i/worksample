最小二乗法についてのご質問に回答させていただきます。

>・なぜ2乗しているのか

最小二乗法の目的は、観測値（training dataの正解label）に最も近い予測値を出力するmodelのparameterを決めることでした。なので、損失関数（観測値と予測値がどれくらいはなれているかを測る関数）を最小化することになります。  

なぜ2乗しているか、を理解する上で、**2乗しなかったらどうなるか**、を考えます。

以下の2つのtraining dataにfitするmodelを考えます。
<!--
\begin{array}{rcl}
	(x_1,y_1) = (1,0)\\
	(x_2,y_2) = (0,1)
\end{array}
-->
<img src="http://latex.codecogs.com/svg.latex?\begin{array}{rcl}&space;(x_1,y_1)&space;=&space;(1,0)\\&space;(x_2,y_2)&space;=&space;(0,1)&space;\end{array}" title="\begin{array}{rcl} (x_1,y_1) = (1,0)\\ (x_2,y_2) = (0,1) \end{array}" />

線形回帰だとすると、損失関数は以下のようになります。
1. `y = f(x) = 1*x+0`のとき
<!--
\begin{array}{cccc}
	& {y_1 - f(x_1)} & + & {y_2 - f(x_2)}\\
	=& {y_1 - x_1} & + & {y_2 - x_2}\\
	=& {0 - 1} & + & {1 - 0}\\
	=& -1 & + & 1 \\
	=& 0 & &
\end{array}
-->
<img src="http://latex.codecogs.com/svg.latex?\begin{array}{cccc}&space;&&space;{y_1&space;-&space;f(x_1)}&space;&&space;&plus;&space;&&space;{y_2&space;-&space;f(x_2)}\\&space;=&&space;{y_1&space;-&space;x_1}&space;&&space;&plus;&space;&&space;{y_2&space;-&space;x_2}\\&space;=&&space;{0&space;-&space;1}&space;&&space;&plus;&space;&&space;{1&space;-&space;0}\\&space;=&&space;-1&space;&&space;&plus;&space;&&space;1&space;\\&space;=&&space;0&space;&&space;&&space;\end{array}" title="\begin{array}{cccc} & {y_1 - f(x_1)} & + & {y_2 - f(x_2)}\\ =& {y_1 - x_1} & + & {y_2 - x_2}\\ =& {0 - 1} & + & {1 - 0}\\ =& -1 & + & 1 \\ =& 0 & & \end{array}" />

2. `y = f(x) = -1*x+1`のとき
<!--
\begin{array}{cccc}
	& {y_1 - f(x_1)} & + & {y_2 - f(x_2)}\\
	=& {y_1 - (-x_1+1)} & + & {y_2 - (-x_2+1)}\\
	=& {0 - (-1+1)} & + & {1 - (-0+1)}\\
	=& 0 & + & 0 \\
	=& 0 & &
\end{array}
-->
<img src="http://latex.codecogs.com/svg.latex?\begin{array}{cccc}&space;&&space;{y_1&space;-&space;f(x_1)}&space;&&space;&plus;&space;&&space;{y_2&space;-&space;f(x_2)}\\&space;=&&space;{y_1&space;-&space;(-x_1&plus;1)}&space;&&space;&plus;&space;&&space;{y_2&space;-&space;(-x_2&plus;1)}\\&space;=&&space;{0&space;-&space;(-1&plus;1)}&space;&&space;&plus;&space;&&space;{1&space;-&space;(-0&plus;1)}\\&space;=&&space;0&space;&&space;&plus;&space;&&space;0&space;\\&space;=&&space;0&space;&&space;&&space;\end{array}" title="\begin{array}{cccc} & {y_1 - f(x_1)} & + & {y_2 - f(x_2)}\\ =& {y_1 - (-x_1+1)} & + & {y_2 - (-x_2+1)}\\ =& {0 - (-1+1)} & + & {1 - (-0+1)}\\ =& 0 & + & 0 \\ =& 0 & & \end{array}" />

どちらのparameterでも、**損失関数の値は同じ0なので、どちらも同じくらい予測できている**modelということになってしまいます。

しかし、
1. のとき、**それぞれの誤差は1, -1**
2. のとき、**それぞれの誤差は0, 0**

であり、2.の方が良いmodelだということは明らかです。

なぜ、このようなことが起きるかというと、1.では、最後の誤差の足し合わせで、**誤差どうしが正負で相殺**されてしまっているからです。

そこで、**各誤差を2乗**することにより、
1. のとき
<!--
\begin{array}{cccc}
	& (-1)^2 & + & 1^2 \\
	=& 1 & + & 1 \\
	=& 2 & &
\end{array}
-->
<img src="http://latex.codecogs.com/svg.latex?\begin{array}{cccc}&space;&&space;(-1)^2&space;&&space;&plus;&space;&&space;1^2&space;\\&space;=&&space;1&space;&&space;&plus;&space;&&space;1&space;\\&space;=&&space;2&space;&&space;&&space;\end{array}" title="\begin{array}{cccc} & (-1)^2 & + & 1^2 \\ =& 1 & + & 1 \\ =& 2 & & \end{array}" />

2. のとき
<!--
\begin{array}{cccc}
	& 0^2 & + & 0^2 \\
	=&  & &
\end{array}
-->
<img src="http://latex.codecogs.com/svg.latex?\begin{array}{cccc}&space;&&space;0^2&space;&&space;&plus;&space;&&space;0^2&space;\\&space;=&&space;0&space;&&space;&&space;\end{array}" title="\begin{array}{cccc} & 0^2 & + & 0^2 \\ =& 0 & & \end{array}" />

となり、**1.より2.の方が良いmodelである、と正当に評価**できます。なお、損失関数は観測値と予測値がどれくらい離れているかを表せればいいので、負の誤差を正としてカウントしても大丈夫なのです。

最終的な答えは、**2乗しないと、誤差の正負で相殺が起き、損失関数がmodelの良し悪しを適切に測ることができなくなるから**です。

また、今回のように、**なぜする/あるのかわからない**時は、**しない/無い場合に何が起きるか**を考えてみることが、新しい概念を理解するための1つのテクニックです。

---

>・なぜ最後に,2で割っているのか

損失関数をコンピュータで最小化させるには、**勾配降下法**というアルゴリズムを使うことが多いです。

このアルゴリズムを使う際に、**損失関数の微分係数を扱う**場面がでてきます。（詳しくは、勾配降下法のところで学んでください。）

モデルを`y=w*x+b`とした場合、損失関数のparameter wでの微分係数は、
<!--
\begin{array}{rl}
	& \frac{\partial}{\partial w} \frac{1}{2} \sum_{i} \{y_i - f(x_i)\}^2\\
	=& \frac{1}{2} \sum_{i} \frac{\partial}{\partial w}  \{y_i - (wx_i+b)\}^2\\
	=& \frac{1}{2} \sum_{i} 2 \{y_i - (wx_i+b)\} \cdot\frac{\partial}{\partial w}  \{y_i - (wx_i+b)\}\\
	=& {\bf \frac{1}{2} \cdot 2}  \sum_{i} \{y_i - (wx_i+b)\} \cdot (-x_i)\\
	=& \sum_{i} \{(wx_i+b)-y_i\} \cdot x_i
\end{array}
-->
<img src="http://latex.codecogs.com/svg.latex?\begin{array}{rl}&space;&&space;\frac{\partial}{\partial&space;w}&space;\frac{1}{2}&space;\sum_{i}&space;\{y_i&space;-&space;f(x_i)\}^2\\&space;=&&space;\frac{1}{2}&space;\sum_{i}&space;\frac{\partial}{\partial&space;w}&space;\{y_i&space;-&space;(wx_i&plus;b)\}^2\\&space;=&&space;\frac{1}{2}&space;\sum_{i}&space;2&space;\{y_i&space;-&space;(wx_i&plus;b)\}&space;\cdot\frac{\partial}{\partial&space;w}&space;\{y_i&space;-&space;(wx_i&plus;b)\}\\&space;=&&space;{\bf&space;\frac{1}{2}&space;\cdot&space;2}&space;\sum_{i}&space;\{y_i&space;-&space;(wx_i&plus;b)\}&space;\cdot&space;(-x_i)\\&space;=&&space;\sum_{i}&space;\{(wx_i&plus;b)-y_i\}&space;\cdot&space;x_i&space;\end{array}" title="\begin{array}{rl} & \frac{\partial}{\partial w} \frac{1}{2} \sum_{i} \{y_i - f(x_i)\}^2\\ =& \frac{1}{2} \sum_{i} \frac{\partial}{\partial w} \{y_i - (wx_i+b)\}^2\\ =& \frac{1}{2} \sum_{i} 2 \{y_i - (wx_i+b)\} \cdot\frac{\partial}{\partial w} \{y_i - (wx_i+b)\}\\ =& {\bf \frac{1}{2} \cdot 2} \sum_{i} \{y_i - (wx_i+b)\} \cdot (-x_i)\\ =& \sum_{i} \{(wx_i+b)-y_i\} \cdot x_i \end{array}" />

となり、途中で2が出てきますが、1/2をかけておくことによって、それがキャンセルされ、微分係数が単純な形になります。微分係数が単純になると、最小化の計算が楽になります。

さらに、損失関数Eを正の数aで定数倍した関数aEの値は、もとの損失関数の値の大小関係と同じです。(E'=aEが単調増加のため)

よって、aEを最小化するparamterは、元の損失関数Eも最大化します。なので、ある損失関数を正の定数倍したものはすべて、損失関数として扱えます。

最終的な答えは、
- 1/2をかけておくと、**損失関数の微分係数が単純な形になるから**で、
- **損失関数を正の定数倍した関数も、損失関数として扱える**ので大丈夫です。
