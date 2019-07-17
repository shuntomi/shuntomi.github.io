# Kendon Model-Based Gesture Recognition Using Hidden Markov Model and Learning Vector Quantization
### 2018/12/20 富澤駿

---

## 参考文献
* **De Felice, Domenico, and Francesco Camastra. "Kendon Model-Based Gesture Recognition Using Hidden Markov Model and Learning Vector Quantization." Italian Workshop on Neural Nets. Springer, Cham, 2017.**
* 矢田部学. "クォータニオン計算便利ノート." MSS 技報 (18) (2007): 29-34.
* Kohonen, T., Hynninen, J., Kangas, J., Laaksonen, J., Torkkola, K.: Lvq-pak: The learning vector quantization program package. In: Technical Report A30, Helsinki University of Technology, Laboratory of Computer and Information Science (1996)

---

### 前回の論文との違い
* G-nuitの検出は同じ
* 今回は何のジェスチャーだったを当てる事が目標
* 最後は教師なし学習でやっている

---

### 自分の研究との差異
* この研究はG-unitにラベルが付いてる
* 自分の研究にはジェスチャーフェーズにラベルが付いてる
* 特徴量が少し違う

---

かなりラフに書いてある論文だったので、

使っていた手法の説明もそれなりにしようと思います。

---

## Key ward
* 四元数(軽く説明する)
* LVQ(学習ベクトル量子化)
* HMMとバームウェルチアルゴリズム

---

## 目次
1. <font color="Red">Abstract</font>
1. Introduction
1. Kendonモデル
1. ジェスチャ認識器
1. 実験設定と結果
1. Conclution

---

Kendonモデルによるジェスチャー認識器の構築を目指す．

以下の4つで構成されている．

1. Kinect,NITEライブラリによる骨格表現を使用した特徴量抽出
1. LVQによる開始と終了時のハンドポーズの検知
1. PCAによる次元削減
1. 隠れマルコフモデルによるジェスチャー分類

最後に既存のジェスチャー認識器と，精度の比較を行う．

---

## 目次
1. Abstract
1. <font color="Red">Introduction</font>
1. Kendonモデル
1. ジェスチャ認識器
1. 実験設定と結果
1. Conclution

---

### ジェスチャーの種類
* 静的ジェスチャー
    * 静的ジェスチャーはハンドポーズとも呼ばれる．
    * ハンドポーズは形状と向きに関連する．
* **動的ジェスチャー**
    * 手の位置と動きに関連する．

---

### この論文の目的

人間の動的ジェスチャーを記述できるモデルの一つがKendonモデルである．

この論文の目的はKendonモデルを前提とし，

動的ジェスチャー認識器を構築する事である．

---

この認識器は2つのタスクに分けられる．

1. 始点と終点の検知(G-unitの検出)
2. 認識されたジェスチャーの分類

---

## 目次
1. Abstract
1. Introduction
1. <font color="Red">Kendonモデル</font>
1. ジェスチャ認識器
1. 実験設定と結果
1. Conclution

---

### Kendonモデルの復習
* ジェスチャーは統合的な構造を持つ
* ジェスターの1つの単位を<font color="Red">ジェスチャーユニット(G-unit)</font>と呼ぶ．
  * 腕がレストポジションから離れる時が開始位置
  * レストポジションに戻る時が終了位置
* このG-Unitはさらに<font color="Red">ジェスチャーフェーズ</font>に分解する事が可能

---

### ジェスチャーフェーズの詳細(復習)
1. プリパレーション: 手がジェスチャの開始位置まで移動
1. プレストローク: 準備の終了と同時に，手の形と位置を維持したまま一時停止
1. ストローク: ジェスチャーの意味を表す動き
1. ポストストローク: ジェスチャの終了と同時に，手の形と位置を維持したまま一時停止
1. リトラクション: 手が休息位置に戻る

---

<img src="./image/paper01/fig01.jpg" width="500"></img>
### G-unitとジェスチャーフェーズの例

---

## 目次
1. Abstract
1. Introduction
1. Kendonモデル
1. <font color="Red">ジェスチャー識別器</font>
1. 実験設定と結果
1. Conclution

---

### 4. ジェスチャ識別器の構成
1. <font color="Red">特徴量抽出</font>
1. LVQによるG-unitの検出
1. 次元削減
1. HMMによる動的ジェスチャー分類

---

### 補足:　四元数(クォータニオン)
このあと，**四元数**が登場するので簡単に補足しておきます．

四元数を使うと，数学的なメリットが多くあるそうです．

四元数は回転の情報を4次元ベクトルに含める事ができます．

代表的な例の3次元での回転計算を見てみます．

---

### 四元数の定義

四元数$\boldsymbol{q}$は以下のように定義されます．

\begin{equation}
    \boldsymbol{q} = q\_0 + q\_1 i + q\_2 j + q\_3 k
\end{equation}

$q\_0,q\_1,q\_2,q\_3 \in \mathbb{R}$

---

### $i,j,k$の定義
\begin{array}{|c|cccc|}
  \hline
  \times & 1 & i & j & k \\\
  \hline
  1 & 1 & i & j & k  \\\
  i & i & -1 & k & -j \\\
  j & j & -k & -1 & i \\\
  k & k & j & -i & -1 \\\
  \hline
\end{array}

---

### (ex)四元数による利点: 回転

単位ベクトル$\boldsymbol{n}=\left( q\_1,q\_2,q\_3\right)$を軸とした，角度$\theta$の回転を考える．

この時回転を表す四元数は，

\begin{equation}
    \boldsymbol{q} = \cos{\frac{\theta}{2}} + \left( q\_{1} i + q\_{2} j + q\_{3} k \right) \sin{\frac{\theta}{2}}
\end{equation}

で表現される．

---

### 四元数による回転計算

まず，ベクトル$\boldsymbol{x} = \left( x\_{1}, x\_{2}, x\_{3} \right)$から

四弦数　$\boldsymbol{x\_{q} } = 0 + x\_{1}i + x\_{2} j + x\_{3}k $を求める．

このとき，ベクトル$\boldsymbol{x}$の回転先のベクトルは

\begin{equation}
    \boldsymbol{q} \boldsymbol{x_q} \boldsymbol{\overline{q}}
\end{equation}

という簡単な式で記述する事ができる．


---

<img src="./image/paper02/qtani.png" width="500"></img>
### 四弦数による回転

---

話を戻します．今は特徴量を抽出する事が目標でした．

---

Kinectを使用する．なおリフレッシュレートは$50Hz$．

特徴量抽出は以下の2つのステップに分けられる．

1. NITEライブラリによる，ジェスチャー部分の骨格表現の抽出
    * 16部位の座標or四元数が得られる
1. 今回は手と肘部分の特徴量を使用する．

---

<img src="./image/paper02/fig_16_1.png" width="450"></img>
### NITEライブラリによる骨格表現

---

最終的に得られるのは以下の20の特徴量である．

* 両手の座標(3 $\times$ 2)
* 両肘の座標(3 $\times$ 2)
* 両肘の四元数(4 $\times$ 2)

---

### 4. ジェスチャ識別器
1. 特徴量抽出
1. <font color="Red">LVQによるG-unitの検出</font>
1. 次元削減
1. HMMによる動的ジェスチャー分類

---

ここまでで，各フレームに対して特徴ベクトルを付与する事ができました．

次は，ジェスチャーの部分に対応するフレームを特定する事を目指します．

すなわち，レストポジションを特定する事が目標です．

今回はLVQを使ってこの問題を解きます．

---

### 学習ベクトル量子化(LVQ)の概要
* 教師あり学習の一種
* クラスの代表点(参照ベクトル)を選びたい
* いくつかバージョンがある
    * 今回使うのはLVQ1とLVQ2.1
* K-meanとk-NNに似てる

---

### LVQのアルゴリズム

---

### 記号の準備
* 時刻$t$の入力ベクトル: $\boldsymbol{x}\(t\)=\left( x_1, x_2, \dots, x_n \right)$
* 入力データ系列: $X=\(\boldsymbol{x_1},\dots,\boldsymbol{x_n} \)$
* $\boldsymbol{x}$のラベル: $T$
* クラス数: $C$
* クラスCのコードベクトル: $\boldsymbol{m_c}$

---

#### LVQ-1アルゴリズムの概要
1. コードベクトル$\boldsymbol{m_i}$を初期化  $\(i=1,\dots,C\)$
1. すべての$\boldsymbol{x}$に対して，どの$\boldsymbol{m_i}$に近いかを計算する．
1. 正しく分類出来たかどうかで，コードベクトルを更新する
    1. $\boldsymbol{m\_i} \gets \boldsymbol{m\_i} + \alpha \left(\boldsymbol{x}\(t\)-\boldsymbol{m\_i} \right) \quad \(正しい時\)$
    1. $\boldsymbol{m\_i} \gets \boldsymbol{m\_i} - \alpha \left(\boldsymbol{x}\(t\)-\boldsymbol{m\_i} \right) \quad \(正しく無い時\)$

ただし$0 < \alpha < 1$

>>>

<img src="./image/paper02/lvq1.png" width="500"></img>
### LVQ-1の解説

---

次にLVQ-2.1の説明をします．

基本的にはLVQ-1の適応後に使用されます．

LVQ-2.1はコードベクトルが一度に2つ更新されます．

---

### 記号の準備
* $\boldsymbol{m_i}$ : **ラベルの異なる**$\boldsymbol{x}$に一番近いコードベクトル
* $d_i$ : $\boldsymbol{x}$と$\boldsymbol{m_i}$の距離
* $\boldsymbol{m_j}$ : **ラベルが一致している**$\boldsymbol{x}$に一番近いコードベクトル
* $d_j$ : $\boldsymbol{x}$と$\boldsymbol{m_j}$の距離
* $N$ : サンプルデータ数
* $w$ : ウィンドウサイズを表す定数($0.2 \sim 0.3$が推奨される)

---

### LVQ-2.1アルゴリズムの概要

1. $\min{\(\frac{d_i}{d_j}, \frac{d_j}{d_i}\)} > \frac{1-w}{1+w}$　なら以下の更新をする
    1. $\boldsymbol{m\_i} \gets \boldsymbol{m\_i} - \alpha \left(\boldsymbol{x}\(t\)-\boldsymbol{m\_i} \right) \quad $
    1. $\boldsymbol{m\_j} \gets \boldsymbol{m\_j} + \alpha \left(\boldsymbol{x}\(t\)-\boldsymbol{m\_j} \right) \quad $

クラス間距離を大きくしようとしている雰囲気です

---

最終的に扱いたいデータとは別に

LVQを学習させるためのデータを導入します．

---

### LVQの学習
* まず，13個の異なるジェスチャーからなるラベル付きデータセットをつかって学習しておく
* このデータは最終的に全体に流すデータとは別のデータで完全に学習用みたいです
* 学習したことでG-unitを取り出す事ができるようになります．

---

<img src="./image/paper02/fig16_2.png" width="600"></img>
### LVQ用学習データ

---

これで本番のデータが流れてきたら

G-unit部分の特徴量を取り出す事ができるようになりました．

---

### 4. ジェスチャ識別器
1. 特徴量抽出
1. レストポジションの特定
1. <font color="Red">次元削減</font>
1. HMMによる動的ジェスチャー分類

---

このステップでは,前のステップから20次元の特徴ベクトルを受け取り，

PCAを用いて次元削減をします．

PCAの説明は省略します．[PCAの資料のリンク](https://shuntomi.github.io/labo_slide/pca.html)

>>>

### 主成分分析のおさらい
1. データ集合から共分散行列$\mathrm{S}$を求める
1. $\mathrm{S}\mathrm{u} = \lambda \mathrm{u}$の固有値問題を解く
1. 大きい順に$M$個の固有値に対応する固有ベクトルから主成分を求める
1. 各データに対して$M$個の主成分で基底変換した座標をもとめる

---

### PCAの実験結果
* 第二主成分までで累積寄与率が$60%$
* 第五主成分までで累積寄与率が$90%$

今回は複雑さ回避のため，第二主成分までを使う事にする．

---

### 4. ジェスチャ識別器
1. 特徴量抽出
1. レストポジションの特定
1. 次元削減
1. <font color="Red">HMMによる動的ジェスチャー分類</font>

---

最後は，次元削減後の特徴ベクトルを使って，

そのジェスチャーが何のジェスチャーを表しているのかを推定します．

なお，ジェスチャーの総数は既知とします．

---

要するに特徴量を入力とした

他クラス問題として扱います．

今回はHMMを使ってこの問題を解きます．

---

説明は大変なので，とりあえず簡単なサンプルを使って

大雑把に説明します．

---

### HMM概要
* 確率モデルを近似できる
* いくつか種類があり，今回使うのはleft to right 型
* 使うためにはパラメータを決める必要がある
* N個の多クラス問題を解くにはN個のHMMを準備する
    * その中で最大確率を出力するクラスを選択する

---

### 具体例(wikiより)
* アリスとボブは遠距離で連絡を取り合っている
* ボブ
    * 毎日天気(潜在変数)を参考にwalk,shop,cleanのどれかの行動を取る
    * 毎日どの行動をとったかをアリスに報告する(実際の天気については教えない)
* アリス
    * ボブから連絡を受け，どの行動をとったかを知る()
    * 一般的な天気の変化の情報は持っている(潜在変数の状態遷移確率を知っている)

これをHMMとしてモデル化してみる

---

<img src="./image/HMM/HMM.png" width="500"></img>
### HMMの例

赤矢印と青矢印はアリスによる推測結果

---

### HMMの一般的なパラメータ
* $ x\_t \in \\{ v\_1,v\_2,\dots ,v\_m \\}$　:　時刻$t$での観測結果
* $\boldsymbol{x} = x_1 x_2 \dots x_n$　:　入力系列
* $ s\_t \in \\{ \omega\_1,\omega\_2,\dots ,\omega\_c \\}$　:　時刻$t$での状態
* $\boldsymbol{s} = s_1 s_2 \dots s_n$　:　状態系列 (HMMではこれは観測出来ない)
* $a_{ij}$　:　$w_i$から$w_j$への状態遷移確率
* $\boldsymbol{A}$　:　$a_{ij}$の集合　
* $b_{jk}$　:　$w_j$で$v_k$を出力する確率
* $\boldsymbol{B}$　:　$b_{ij}$の集合
* $\rho_i$　:　初期状態が$\omega_i$である確率
* $\boldsymbol{\rho}$　:　$\\{\rho\_1,\rho\_2,\dots,\rho\_c\\}$

---

まずHMMの各パラメータは既知として，

今入力$\boldsymbol{x}=x_1 x_2 \dots x_n$があるクラスに属している確率を考えます．

$P\(\boldsymbol{x}\)= \sum_{s}{P\(\boldsymbol{x},s\)} $であり，

これは$P\(\boldsymbol{x}, s\) = \displaystyle \prod\_{t=1}^n {a\(s\_{t-1},s\_{t}\) b\(s\_{t},x\_{t}\)}$

が計算できれば求める事ができます

---

しかしHMMでは$\boldsymbol{s}$は観測する事ができないので計算量が膨大になります．

ここで少し変形して,$t$回目の観測結果の前後に分離させます．

\begin{align}
    P\(\boldsymbol{x}, s\_t =\omega\_i \) &= P\(x\_1 x\_2 \dots x\_t, s\_t = \omega\_i \)P\(x\_{t+1} x\_{t+2} \dots x\_n | s\_t = \omega\_i \) \\\
    & = \alpha\_t\(i\) \beta\_t\(i\)
\end{align}

と変形します．

---

この$\alpha\_t\(i\) \beta\_t\(i\)$を状態$s$を使わずに推定するアルゴリズムを

それぞれフォワードアルゴリズム，バックワードアルゴリズムと呼びます．

---

### フォワードアルゴリズム
1. 初期化
    * $\alpha_1\(i\) = \rho_ib\( \omega_i,x_i \)$
2. $\alpha$の再帰計算
    * $\alpha\_t\(j\) = \left( \displaystyle \sum\_{i=i}^{c} {\alpha\_{t-1}\(i\) a\_{ij} }\right) b\(\omega_j, x_t\)$
3. $P\(\boldsymbol{x}\)$の計算
    * $P\(\boldsymbol{x}\) = \displaystyle \sum\_{i=i}^{c} {\alpha\_{n}\(i\)}$

---

### バックワードアルゴリズム
1. 初期化
    * $\beta_n\(i\) = 1$
2. $\alpha$の再帰計算
    * $\beta\_t\(i\) = \displaystyle \sum\_{j=i}^{c} {a\_\{ij} b\(\omega\_j, x\_{t+1}\) \beta\_{t+1}\(j\) }$

---

### バウムウェルチアルゴリズムの概要
* HMMのバラメータを学習できる
* 基本的にはEMアルゴリズムと同じ
* それ以外に前向きアルゴリズムと後ろ向きアルゴリズムを使う
* 山登り法の一種で最大値の保証がされない

---

### バウムウェルチアルゴリズム-1
1. $\boldsymbol{A}, \boldsymbol{B}, \boldsymbol{\rho}$を初期化する
1. フォワードとバックワードで$\alpha$と$\beta$を計算

---


### 3. 以下の式に従ってHMMのパラメータを更新
\begin{align}
    \hat{\alpha}\_{ij} =& \frac{ \displaystyle \sum\_{t=1}^{n-1}{\alpha\_t\(i\) a\_{ij}} b\(\omega\_j,x\_{t+1}\)\beta\_{t+1}\(j\) }{ \displaystyle \sum\_{t=1}^{n-1}{\alpha\_t\(i\) \beta\_t\(i\) } } \\\
    \hat{\beta}\_{ij} =& \frac{ \displaystyle \sum\_{t=1}^{n-1}{ \delta\(x\_t,v\_{k}\) \alpha\_t\(j\) \beta\_{t+1}\(j\) }}{ \displaystyle \sum\_{t=1}^{n-1}{\alpha\_t\(j\) \beta\_t\(j\) } } \\\
    \hat{\rho}\_{i} =& \frac{\alpha\_1\(i\)\beta\_1\(i\)}{\displaystyle \sum\_{j=1}^{c}{\alpha\_n\(j\)} }
\end{align}

---


バウムウェルチアルゴリズムを使っってHMMの学習を行えば

多クラス問題を解く事ができます．

今回は8クラスの教師なし学習を行う事になります．

---

## 目次
1. Abstract
1. Introduction
1. Kendonモデル
1. ジェスチャ認識器
1. <font color="Red">実験設定と結果</font>
1. Conclution

---

### データセット
* 8種類のジェスチャーを100回ずつ行って作成
* 特徴量はキネクトを使って取り出す
* 訓練データとテストデータは半分ずづに分ける

---

### データセット
<img src="./image/paper02/fig16_3.png" width="300"></img>


---

### 混合行列の結果
<img src="./image/paper02/tb16_1.png" width="600"></img>

---

### 他の手法との比較結果
<img src="./image/paper02/tb16_2.png" width="600"></img>

---

## 目次
1. Abstract
1. Introduction
1. Kendonモデル
1. ジェスチャ認識器
1. 実験設定と結果
1. <font color="Red">Conclution</font>

---

### 筆者による主張
* 提案手法は他の手法より精度が優れいてた
* 将来的には日常生活での周囲支援における導入が期待できる

---

### 個人的に思った事
* もうちょいちゃんと書いて欲しかった
* 画像ベースのハンドポーズ認識器以外と比較して欲しかった．
* 特徴量として四元数を使う動機
* LVQの他の手法に対する優位性
* PCAの累積寄与率が60%は少し気になる