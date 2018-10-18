# 勉強会資料

2018/08/07  

富澤駿

---

## Reference
1. Ian Goodfellow〜他「深層学習」角川．
1. 岡谷貴之「深層学習」講談社．
1. http://www.scholarpedia.org/article/Echo_state_network
1. Jaeger, Herbert. "The “echo state” approach to analysing and training recurrent neural networks-with an erratum note." Bonn, Germany: German National Research Center for Information Technology GMD Technical Report 148.34 (2001): 13.
1. Mozer, Michael C. "Induction of multiscale temporal structure." Advances in neural information processing systems. 1992.

---

僕が担当する部分は，

長い系列の時系列データに対する問題点，改善案に関する内容でした．

時間の都合で全部を説明する事は難しいと思いますが，頑張ります．

---

質問歓迎です．

よろしくお願いします．

---

### 目次
1. RNNの問題点
1. ESN
1. その他の工夫
  1. Skip Connectionの追加
  1. Leakyユニット
  1. Removing Connectionの追加
1. ゲート付きRNN
  1. LSTM
  1. GRU
1. 長い系列に対する最適化手法

---

## RNNの問題点

---

### RNNの問題点
* 時間方向にも深くなりがち
* 時間方向に勾配爆発，消失が起こりやすい
* つまり長い系列に対応できない(10~20が限界)
* 非線形関数の合成の繰り返しは極端な挙動振る舞いをする事がある．

---

#### RNNの図
<img src="image/LSTM/RNN.png" width="60%">

---

## 時間方向に関する問題点

---

RNNの隠れ層の重み行列に代表されるように，

ニューラルネットに行列$\boldsymbol{W}$を繰り返し掛ける構造が含まれてる場合を考えます．

---

ここでは単純化のため，入力ベクトル$\boldsymbol{x}$と活性化関数を省略し，

時刻$t$の隠れ層ベクトル$\boldsymbol{h^{\(t\)}}$が

\begin{equation}
  \boldsymbol{h^{\(t\)}}=\boldsymbol{W}\boldsymbol{h^{\(t-1\)}}
\end{equation}

で与えられる場合を考えます．

---

この時$t$ステップ進む事は$\boldsymbol{W^t}$を掛ける事と等価です．

ここで$\boldsymbol{W}$を固有値分解すると，

\begin{equation}
  \boldsymbol{W}=\boldsymbol{V}diag{\left( \boldsymbol{\lambda} \right)}\boldsymbol{V}^{-1}
\end{equation}

ただし，
$
diag{\left( \boldsymbol{\lambda} \right)}
= \left(
  \begin{array}{cccc}
    \lambda\_{1} & 0 & \ldots & 0 \\\\
    0 & \lambda\_{2} & \ldots & 0 \\\\
    \vdots & \vdots & \ddots & \vdots \\\\
    0 & 0 & \ldots & \lambda\_{n}
  \end{array}
\right)
$

---

これを$\boldsymbol{W^t}$に代入すると，

\begin{equation}
\boldsymbol{W^t}=\boldsymbol{V}diag{\left( \boldsymbol{\lambda} \right)}^{t}\boldsymbol{V}^{-1}
\end{equation}

を得ます．この式を見ながら次のスライドを見てください．

---

重要な事は，$\boldsymbol{W^t}$が消失するか爆発するかは，
$\lambda_i$の大きさによって決まると言う事です．

$\lambda_i$が$1$より大きい場合，$\boldsymbol{W^t}$はいつか発散し，

$\lambda_i$が$1$より小さい場合，$\boldsymbol{W^t}$はいつか消失します．

---

話をまとめると，
$\lambda_i$が$1$に近いような$\boldsymbol{W}$だと良さそうだという事です．

この問題は同じ行列を複数回掛ける事が多いRNN特有の問題です．

---

## 非線形関数に関する問題点

---

RNNでは時間方向に何度も同じ計算が繰り返されます．

つまり同じ活性化関数が何度も呼び出される事になります．

非線形関数が繰り返し呼び出されると，極端な挙動をする事が知られています．

---

#### 例
<img src="image/LSTM/func.png" width="60%">

---

#### これらの問題点を改善する方法
* ESN
* そのほかの技
  * Skip Connectionの追加
  * Leakyユニット
  * Removing Connectionの追加
* LSTM

---

## ESN

---

## ESNの概要

---

ESNは"Echo State Network"の略です．

"Liquid State Machines"とも呼ばれる物とほぼ同じです．

---

RNNにおいて学習が困難なパラメータは以下2つ
* $\boldsymbol{x}^{\(t\)}$から$\boldsymbol{h}^{\(t\)}$への写像の重み$\boldsymbol{U}$
* $\boldsymbol{h}^{\(t-1\)}$から$\boldsymbol{h}^{\(t\)}$への写像の重み$\boldsymbol{W}$

---

#### RNNの図
<img src="image/LSTM/RNN.png" width="60%">

---

ESNではこの問題を回避するため，

出力層のみを学習するモデルの構築を目指します．

---

一般にDNNを使用した学習は，損失関数の最小化問題として定式化されます．

勘違いしやすいですが，BPとSGDはこの問題を解くための方法の一つで，

必ず使わなければいけない訳ではないです．

---

ESNではパラメータを出力層の重みベクトルのみに制約する事で，

BPを使わずに最小化問題を解く事を目指します．

つまり高速で学習する事が可能です．

丁寧に説明すると，線形回帰問題として解く事ができます．

---

テキストでは具体的なESNの動作の記述がまったくありませんでした．

せっかくの機会なので，少し数式を交えて，

単純なESNのアルゴリズムの概要を紹介しようと思います．

---

## ESNのアルゴリズム概要

---

大きく分けると以下のステップで構成されています．

1. ESNを作成する(中間層だけ)
1. 入力層を繋げて，入力重み行列を選ぶ
1. 内部状態の更新
1. 出力を繋げてと損失を計算し，学習を行う(線形回帰問題を解く)

---

#### ESNの図
<img src="image/LSTM/ESN_0.png" width="60%">

---

ESNでは従来の中間層を構成するユニットをリザーバーユニットと呼びます．

リザーバーユニットは全結合しているわけではなく，ランダムに結合されています．

---

今回は単純化のため，フィードバックが無いESNを考えます．

(当日に口で説明します)

>>>

#### ESNの図
<img src="image/LSTM/ESN_0.png" width="60%">

---

入力ユニットを$N$個，リザーバーユニットを$K$個の入力，

出力ユニットを$L$個持つESNを考えます．

また説明のため，時間ステップ$n$に関して記号をいくつか定義します．

---

* 入力ベクトル: $\quad \boldsymbol{u}\(n\)= \left( u\_{1}\(n\),\dots, u\_{K}\(n\) \right)$
* 内部ユニット: $\quad \boldsymbol{x}\(n\)= \left( x\_{1}\(n\),\dots, x\_{N}\(n\) \right)$
* 内部ユニットの活性化関数: $\quad \boldsymbol{f}\(n\)= \left( f\_{1}\(n\),\dots, f\_{N}\(n\) \right)$
* 出力ベクトル: $\quad \boldsymbol{y}\(n\)= \left( y\_{1}\(n\),\dots, y\_{L}\(n\) \right)$
* 出力ユニットの活性化関数: $\quad \boldsymbol{f}^{out}\(n\)= \left( f^{out}\_{1}\(n\),\dots, f^{out}\_{L}\(n\) \right)$
* 入力重み行列:$\quad \boldsymbol{W}^{in}$
* リザーバユニットの重み行列:$\quad \boldsymbol{W}$:
* <font color="Red">出力重み行列:$\quad \boldsymbol{W}^{out}$</font>

>>>

#### ESNの図
<img src="image/LSTM/ESN_0.png" width="60%">

---

## Step1 ESNの作成

---

個人的にはこのステップがESNの特徴的な部分だと感じます．

ESNの中間層を作成する事は，$\boldsymbol{W}$を決める事に相当します．

---

入力ベクトル集合を$\boldsymbol{U}=\left(\boldsymbol{u}\_{i},\dots,\boldsymbol{u}_{N} \right)$とします．

この時，先程説明したスペクトル半径を参考に，

$U$からヒューリスティック的に行列$\boldsymbol{W}$を作ります．

---

この手順の説明はとても証明が大変なので省略します．

作成された$\boldsymbol{W}$は$|\ \lambda_{max}\| \le 1$を満たします．

---

## Step2 入力重みベクトルの作成

---

次に入力層の重みベクトルを决定します．

結論から言うと，$\boldsymbol{W_{in}}$は任意で与えても問題ない事が分かっています．

---

正確には，ESNを関数として見なした場合に，

リプシッツ条件を満たしている必要があります．

証明は省略します．

---

## Step3 内部状態の更新

---

今の状態だと，内部ベクトル$\boldsymbol{x}$が決まっていません．

フィードバックが無いESNでは，
教師データを使わず，入力ベクトル$\boldsymbol{u}$をつかって$\boldsymbol{x}$を決めます．

---

まず，$\boldsymbol{x}\left(0\right)$を任意に初期化し，以下の式で計算します．

\begin{equation}
  \boldsymbol{x}\(n\) = \boldsymbol{f} \left(\boldsymbol{W}^{in} \left(\boldsymbol{u}\(n\)+\boldsymbol{W}\boldsymbol{x}\(n-1\)  \right) \right)
\end{equation}

---

## Step4 学習する

---

次に出力$y$を以下の式に従って計算します．

\begin{equation}
  y\(n\)=f^{out}\left( \sum\_{i=1,\dots,N}{w\_{i}^{out}x\_{i}\(n\)} \right)
\end{equation}

---

$f^{out}$は$\tanh$が採用される事が多いです．

$\tanh$のように逆関数が存在する事を仮定すれば，以下の式が成立します．

\begin{equation}
  \left( f^{out} \right)^{-1}y\(n\)=\sum\_{i=1,\dots,N}{w\_{i}^{out}x\_{i}\(n\)}
\end{equation}

---

ここまで求めれば，時間ステップ$n$の時，

教師データ$y_{teach}$を使って，誤差$\epsilon\(n\)$を求める事ができます．

\begin{equation}
  \epsilon\(n\) =  \left( f^{out} \right)^{-1}y\_{teach}\(n\) - \left( f^{out} \right)^{-1}y\(n\) \\\
\end{equation}

これまでの記号を元に戻すと以下のように変形できます．
\begin{equation}
  \epsilon\(n\) = \left( f^{out} \right)^{-1}y\_{teach}\(n\) - \sum\_{i=1,\dots,N}{ w\_{i}^{out} e\_{i}\left( \dots,\boldsymbol{u}\(n-1\),\boldsymbol{u}\(n\) \right) }
\end{equation}

---

ESNでは以下の式に従い最小二乗誤差を求め，最小化問題をときます．

\begin{equation}
  {mes}\_{train} = \frac{1}{n\_{max}-n\_{min}} \sum\_{n=n\_{min},\dots,n\_{max}} {\epsilon^2\(n\)}
\end{equation}

変数は$\boldsymbol{W}^{out}$のみなので，最小二乗法で解けます．

---

## その他の技

---

### Skip Connection
Skip Connectionは名前の通りです．

通常は時刻$t$から$t+1$にベクトルを渡しますが，

これに加えて，$t$から$t+d$への接続を追加する．

勾配の消失スピードを抑える事が出来る．

---

### Leakyユニット
各ユニットに線形な自己接続を導入する方法です．

時間ステップ$t$のとき，あるユニットの活性化関数の出力を$v^{t}$と表します．

---

ユニットの出力を$\mu^{t}$すると，通常の場合

\begin{equation}
 \mu^{\(t\)} \gets v^{\(t\)}
\end{equation}

です．普通の事を言っています．

---

Leakyユニットが導入されると，

\begin{equation}
  \mu^{\(t\)} \gets \alpha \mu^{\(t-1\)} + (1-\alpha) v^{\(t\)} \quad \left(0 \leq \alpha \leq 1 \right)
\end{equation}

のような更新を行います．

---

### Removing Connection
* 長さが$1$の接続を削除(Skip Connectionとの違い)
* より長い接続に置き換える

---

## LSTM

---

## LSTMとは

---

### まずはザックリと
LSTMは"Long Sort Term Memory"の略．

LSTMはRNNの進化版で,結構有名です．

時系列データにRNNに比べて，長いスパンの時系列データが得意です．

---

#### RNNとLSTM
<img src="image/LSTM/RNN-LSTM.png" width="60%">

---

#### LSTMユニットの図
<img src="image/LSTM/LSTM.png" width="40%">

---

#### LSTMユニットの構成要素
* 入力ゲート
* 出力ゲート
* 忘却ゲート
* 内部状態

---

#### ゲート値
各ゲートは0~1の値を取るゲート値を持っています．

通常は，$[0,1]$の範囲に入れるため，
入力されるベクトルをシグモイド関数を通します．

シグモイド関数を$\sigma$で表す事にします．

>>>

#### シグモイド関数
<img src="image/LSTM/sigmoid.png" width="60%">

---

説明のために時刻$t$におけるユニット$i$の各ゲート値と内部状態を

以下のように定めて置きます．

* $g_{i}^{\left(t\right)}$: 入力ゲート値
* $q_{i}^{\left(t\right)}$: 出力ゲート値
* $f_{i}^{\left(t\right)}$: 忘却ゲート値
* $s_{i}^{\left(t\right)}$: 内部状態

---

## LSTMの計算

---

最終目標はLSTMユニットの出力ベクトルを得る事とします．

まずは各ゲート値を求めておきます．

---

#### LSTM説明の図
<img src="image/LSTM/LSTM-2.png" width="40%">

---

#### ゲート値の計算

\begin{equation}
g\_{i}^{\left(t\right)} = \sigma \left( {b_i}^{g} + \sum\_j {U\_{i,j}^{g}{x\_j}^{\left(t\right)}} +\sum\_j {W\_{i,j}^{g}{h\_j}^{\left(t-1\right)}} \right)
\end{equation}

\begin{equation}
q\_{i}^{\left(t\right)} = \sigma \left( {b_i}^{q} + \sum\_j {U\_{i,j}^{q}{x\_j}^{\left(t\right)}} +\sum\_j {W\_{i,j}^{q}{h\_j}^{\left(t-1\right)}} \right)
\end{equation}

\begin{equation}
f\_{i}^{\left(t\right)} = \sigma \left( {b_i}^{f} + \sum\_j {U\_{i,j}^{f}{x\_j}^{\left(t\right)}} +\sum\_j {W\_{i,j}^{f}{h\_j}^{\left(t-1\right)}} \right)
\end{equation}

それぞれが独立して$\boldsymbol{U}$と$\boldsymbol{W}$を持っている事に注意してください．

---

これでゲート値の準備ができました．順番に入力から見ていきます．

入力されたベクトル$\boldsymbol{x}$は，まず図の"input"のノードを通ります．

---

"input"ノードの出力を$Z$とすると，

\begin{equation}
  Z= \sigma \left( {b_i} + \sum\_j {U\_{i,j}{x\_j}^{\left(t\right)}} +\sum\_j {W\_{i,j}{h\_j}^{\left(t-1\right)}} \right)
\end{equation}

のように計算できます．

>>>

#### LSTM説明の図
<img src="image/LSTM/LSTM-2.png" width="40%">

---

次に入力ゲート値との積を取り,

$g\_{i}^{\left(t\right)} Z$を次のノードに渡します．

---

次にするべき計算は，時刻$t$における内部状態$s_{i}^{\left(t\right)}$を求める事です．

そのためには，図の"self-loop"に保存されている一つ前の内部状態$s_{t-1}$と

忘却ゲート値の積$f\_{i}^{\left(t\right)}s\_{i}^{\left(t-1\right)}$が必要です．

>>>

#### LSTM説明の図
<img src="image/LSTM/LSTM-2.png" width="40%">

---

一つ前の内部状態と忘却ゲートの積が求まれば，

$ g\_{i}^{\left(t\right)} Z$との和を取ることで，$s\_{i}^{\left(t\right)} $を以下のように求める事が出来ます．


\begin{equation}
  s\_{i}^{\left(t\right)} = f\_{i}^{\left(t\right)}s\_{i}^{\left(t-1\right)}+g\_{i}^{\left(t\right)} Z
\end{equation}

---

求めた$s\_{i}^{\left(t\right)}$は次の為に保存しておきます．

最後に内部状態と活性化関数に通した値と出力ゲート値$q$の積をとり，

LSTMユニットの出力$h\_{i}^{\left(t\right)}$を以下のように求める事ができます．

\begin{equation}
h\_{i}^{\left(t\right)} = \tanh{\left( s\_{i}^{\left(t\right)}\right)}q\_{i}^{\left(t\right)}
\end{equation}

>>>

#### LSTM説明の図
<img src="image/LSTM/LSTM-2.png" width="40%">

---

他にもGRUという手法も紹介されています．

---

#### GRU
* LSTMの忘却ゲートを改良したモデル
* ユニット自身が忘却と状態の决定を行う
* 更新ゲート$\boldsymbol{u}$とリセットゲート$\boldsymbol{r}$を導入する事で実現

---

#### GRU
<img src="image/LSTM/GRU.png" width="80%">

---

$\boldsymbol{u}$と，$\boldsymbol{r}$は以下のように定式化出来ます．

\begin{equation}
u\_{i}^{\left(t\right)} = \sigma \left( {b_i}^{u} + \sum\_j {U\_{i,j}^{u}{x\_j}^{\left(t\right)}} +\sum\_j {W\_{i,j}^{u}{h\_j}^{\left(t-1\right)}} \right)
\end{equation}

\begin{equation}
r\_{i}^{\left(t\right)} = \sigma \left( {b_i}^{r} + \sum\_j {U\_{i,j}^{r}{x\_j}^{\left(t\right)}} +\sum\_j {W\_{i,j}^{r}{h\_j}^{\left(t-1\right)}} \right)
\end{equation}

---

この$\boldsymbol{u}$と，$\boldsymbol{r}$を使って隠れ層ベクトルは以下のように計算できます．

\begin{equation}
h\_{i}^{\left(t\right)} ={u\_i}^{\left(t-1\right)}{h\_i}^{\left(t-1\right)} + { \left( 1 - {u\_i}^{\left(t-1\right)} \right)} \sigma \left( {b_i} + \sum\_j {U\_{i,j}{x\_j}^{\left(t-1\right)}} +\sum\_j {W\_{i,j}{r\_j}^{\left(t-1\right)}{h\_j}^{\left(t-1\right)}} \right)
\end{equation}

---

式の形がLeakyユニットに似ています．

更新ゲート$\boldsymbol{u}$の値によって，

状態ベクトルをコピーするか，無視するかの度合い制御できます．

リセットゲート$\boldsymbol{r}$は各要素のどの部分が計算に使用されるかを制御する事ができる．

---

## 長い系列に対する最適化手法

---

テキストで紹介されていた手法は以下です．

* Clipping
* 正規化

それぞれ簡単に紹介します．

---

### Clipping

---

先に述べたように，RNNでは勾配が極端に大きく(または小さく)なる傾向があります．

つまり一気に更新しすぎてしまい，これまでの努力が無駄になってしまう事があります．

---

Cloppingはこの問題に対処します．

要素ごとに切り抜く方法やノルムを用いる方法など色々とあります．

---

勾配$\boldsymbol{g}$のノルムを用いたクリッピングは，

しきい値$v$を用いて以下のように表せます．

$if \quad \|\boldsymbol{g}\| > v$

\begin{equation}
\quad \boldsymbol{g} \gets \frac{\boldsymbol{g}}{\|\boldsymbol{g}\|}{v}
\end{equation}

---

#### クリッピング
<img src="image/LSTM/clipping.png" width="60%">

---

## 正規化

---

正規化は勾配爆発問題に対処するために使用されます．

今回はPascanuが提案した方法を紹介します．

---

BPでは，関連付けられた2つのベクトルの勾配の積が$1$に近いほうが良いです．

例えば，時間方向に展開されたベクトル$\boldsymbol{h^{\(t\)}}$と$\boldsymbol{h^{\(t-1\)}}$を考えます．

このとき逆伝播するベクトル${\nabla}\_{\boldsymbol{h}^{\( t \)}} L$の大きさが維持されてほしいです．

---

式で書けば

\begin{equation}
  \left( {\nabla} \_{\boldsymbol{h}^{\( t \)}} L\right) \frac{\partial \boldsymbol{h}^{\( t \)}}{\partial \boldsymbol{h}^{\( t-1 \)}} \fallingdotseq \left(\nabla \_{\boldsymbol{h}^{\( t \)} }L\right)
\end{equation}

が成り立つ事が理想です．

---

この考えを元に，この2つの比と1の最小二乗誤差を考えます．

最小化問題の目的関数に以下の正則化項を追加します．

\begin{equation}
 \Omega = \sum\_{t} {\left( \frac{ \left|\left| \left( \nabla \_{\boldsymbol{h}^{\( t \)} }L \right) \frac{\partial \boldsymbol{h}^{\( t \)}}{\partial \boldsymbol{h}^{\( t-1 \)}} \right|\right|}{ \left|\left| \nabla \_{\boldsymbol{h}^{\( t \)}}L \right|\right|} ｰ1 \right)^2 }
\end{equation}

---

# Thank you
