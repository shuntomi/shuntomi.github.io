# Kendon Model-Based Gesture Recognition Using Hidden Markov Model and Learning Vector Quantization
### 2018/12/06 富澤駿

---

## 参考文献
* **De Felice, Domenico, and Francesco Camastra. "Kendon Model-Based Gesture Recognition Using Hidden Markov Model and Learning Vector Quantization." Italian Workshop on Neural Nets. Springer, Cham, 2017.**
* 矢田部学. "クォータニオン計算便利ノート." MSS 技報 (18) (2007): 29-34.
* https://www.tutorialspoint.com/artificial_neural_network/artificial_neural_network_learning_vector_quantization.htm

---

## 目次
1. <font color="Red">Abstract</font>
1. Introduction
1. Kendonモデル
1. ジェスチャ認識器
1. 実験結果
1. Conclution

---

Kendonモデルによるジェスチャー認識器の構築を目指す．

以下の4つで構成されている．

* Kinect,NITEライブラリによる骨格表現を使用した特徴量抽出
* LVQによる開始と終了時のハンドポーズの検知
* 次元削減
* 隠れマルコフモデルによるジェスチャー分類

最後に既存のジェスチャー認識器と，精度の比較を行う．

---

## 目次
1. Abstract
1. <font color="Red">Introduction</font>
1. Kendonモデル
1. ジェスチャ認識器
1. 実験結果
1. Conclution

---

### ジェスチャーの種類
ジェスチャーは2つに分類できる．
* 静的ジェスチャー
    * 静的ジェスチャーはハンドポーズとも呼ばれる．
    * ハンドポーズは形状と向きに関連する．
* **動的ジェスチャー**
    * 手の位置と動きに関連する．

---

### この論文の目的

人間の動的ジェスチャーを記述できるモデルの一つがKendonモデルである．

この論文の目的はKendonモデルを前提とした，

動的ジェスチャー認識器を構築する事である．

---

この認識器は2つのタスクに分けられる．

1. 始点と終点の検知．
2. 認識されたジェスチャーの分類．

---

## 目次
1. Abstract
1. Introduction
1. <font color="Red">Kendonモデル</font>
1. ジェスチャ認識器
1. 実験結果
1. Conclution

---

### Kendonモデルの復習
* ジェスチャーは統合的な構造を持つ
* ジェスターの1つの単位を<font color="Red">ジェスチャーユニット(G-unit)</font>と呼ぶ．
  * 腕がレストポジションから離れる時が開始位置
  * レストポジションに戻る時が終了位置
* このG-Unitはさらに<font color="Red">ジェスチャーフェーズ</font>に分解する事が可能

---

### ジェスチャーフェーズの詳細
1. 準備: 手がジェスチャの開始位置まで移動
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
1. 実験結果
1. Conclution

---

### 4. ジェスチャ識別器
1. <font color="Red">特徴量抽出</font>
1. LVQによるレストポジションの特定
1. 次元削減
1. HMMによる動的ジェスチャー分類

---

### 補足:　四元数(クォータニオン)
このあと，**四元数**が登場するので簡単に補足しておきます．

四元数を使うと3次元での回転計算が簡単に行えます．

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

四元数における単位ベクトル$\boldsymbol{n}=\left( q\_1,q\_2,q\_3\right)$を軸とした，角度$\theta$の回転を考える．

この時回転を表す四元数は，

\begin{equation}
    q = \cos{\frac{\theta}{2}} + \left( q\_{1} i + q\_{2} j + q\_{3} k \right) \sin{\frac{\theta}{2}}
\end{equation}

で表現される．

---

この$\boldsymbol{q}$を使い以下の式に従えば，

ベクトル$\boldsymbol{x}$を，任意の方向に回転させる事ができる．

\begin{equation}
    \boldsymbol{q} \boldsymbol{x} \boldsymbol{\overline{q}}
\end{equation}

---

<img src="./image/paper02/qtani.png" width="500"></img>
### 四弦数による回転

---

話を戻します．三次元ベクトルから四元数が求まる事を覚えておいてください．

今は特徴量を抽出する事が目標です．

---

Kinectを使用する．なおリフレッシュレートは$50Hz$とした．

特徴量抽出は以下の2つのステップに分けられる．

1. NITEライブラリによる，ジェスチャー部分の骨格表現の抽出(16部位の座標or四元数が得られる)
1. その中から動的ジェスチャーに寄与する部分をを選ぶ

---

<img src="./image/paper02/fig_16_1.png" width="450"></img>
### NITEライブラリによる骨格表現

---

最終的に得られるのは以下の20の特徴量である．

* 両手の座標(3 $\times$ 2)
* 両肘の座標(3 $\times$ 2)
* 両手の四元数(4 $\times$ 2)
* 両肘の四元数(4 $\times$ 2)

---

### 4. ジェスチャ識別器
1. 特徴量抽出
1. <font color="Red">レストポジションの特定</font>
1. 次元削減
1. HMMによる動的ジェスチャー分類

---

### 学習ベクトル量子化(LVQ)の概要
* 教師あり学習の一種
* クラスの代表点(参照ベクトル)を選びたい
* いくつかバージョンがある
* 今回使うのはLVQ1,LVQ2,LVQ3
* K-meanに似てる

---

### LVQのアルゴリズム

---

### 記号の準備
* 入力ベクトル: $\boldsymbol{x}=\left( x_1, x_2, \dots, x_n \right)$
* $\boldsymbol{x}$のラベル: $T$
* クラス数: $C$
* クラスCの代表ベクトル: $\boldsymbol{m_c}$

---

#### LVQ1のアルゴリズム
1. 代表ベクトル$\boldsymbol{m_i}$を初期化  $\(i=1,\dots,C\)$
1. $c= arg\displaystyle\min\_{i}\(\| \boldsymbol{x}-\boldsymbol{m\_i} \|\)$
1. $\boldsymbol{m\_i}\(t+1\) = \boldsymbol{m\_i}\(t\) - \alpha\(t\) \left(\boldsymbol{x}\(t\)-\boldsymbol{m\_i}\(t\)\right) \quad \(cが正しい時\)$
1. $\boldsymbol{m\_i}\(t+1\) = \boldsymbol{m\_i}\(t\) + \alpha\(t\) \left(\boldsymbol{x}\(t\)-\boldsymbol{m\_i}\(t\)\right) \quad \(cが正しく無い時\)$

ただし$0 < \alpha < 1$

---

## 目次
1. Abstract
1. Introduction
1. Kendonモデル
1. ジェスチャ認識器
1. <font color="Red">実験結果</font>
1. Conclution

---

## 目次
1. Abstract
1. Introduction
1. Kendonモデル
1. ジェスチャ認識器
1. 実験結果
1. <font color="Red">Conclution</font>

---
