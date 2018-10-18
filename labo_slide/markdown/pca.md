### 主成分分析とカーネル主成分分析
2018/06/30  
富澤駿

---

目次  
1. 主成分分析
  - 主成分分析の目的  
  - 式の導出
  - 具体例
1. カーネル主成分分析
  - カーネル法とは
  - 式の導出  
  - 具体例

---

## 1. 主成分分析

---

### 主成分分析の目的  
多次元データの持つ情報をできるだけ損なうことなく，低次元空間に情報を集約すること.
$D$次元のデータをうまく$M$次元に変換する手法．$(D \ge M)$    

---

### 主成分分析の利用用途    
* 次元削減
* 非可逆データ圧縮
* データの可視化
* 前処理（特徴抽出，無相関化，ノイズ削減)

---

### 分散最大化による定式化  

射影されたデータ点の分散を最大化しながら，  
データを低次元空間上に射影することを目指す．

---

データ集合$\\{\mathrm{x}\_n\\}$を考える．  
ただし，$n=1,2,\dots,N$とし$\mathrm{x}\_n$は次元$D$のユークリッド空間中の変数とする.

各データ点を次元$D$から次元$M$に射影することを考える．

---

まず$1$次元空間$(M=1)$への射影を考える．  
この空間の方向を$D$次元ベクトル$\mathrm{u}\_1$として表す．  
<img src='./figures/pca/Figure12.2.jpg' width='300'></img>  
$\mathrm{u}\_1$を第$1$主成分と呼ぶ

---

興味があるのは$\mathrm{u}\_1$で定義される方向であるため，$\mathrm{u}\_1^T \mathrm{u}\_1 = 1$とする．  
したがって，各データ点$\mathrm{x}\_n$はスカラー値$\mathrm{u}\_1^T \mathrm{x}\_n$上に射影される.

このとき，射影されたデータの平均値は$\mathrm{u}\_1^T \bar{\mathrm{x}}$とかける．  
（ただし，$\mathrm{\bar{x}}=\frac{1}{N} \sum\_{n=1}^{N} \mathrm{x}\_n$）

---

射影されたデータの分散は

$\frac{1}{N} \sum\_{n=1}^{N} \\{ \mathrm{u}\_1^T \mathrm{x}\_n - \mathrm{u}\_1^T \mathrm{\bar{x}} \\}^2 = \mathrm{u}\_1^T \mathrm{S} \mathrm{u}\_1$

であたえられる．  

ただし，$\mathrm{S}$はデータ共分散行列であり次のように定義される．  
$\mathrm{S} = \frac{1}{N} \sum\_{i=1}^{N} (\mathrm{x}\_n - \mathrm{\bar{x}})(\mathrm{x}\_n - \mathrm{\bar{x}})^T $

---

### 分散の最大化  
射影後の分散$\mathrm{u}\_1^T \mathrm{S} \mathrm{u}\_1$を制約条件$\mathrm{u}\_1^T \mathrm{u}\_1 =1$で最大化する.

ラグランジュ乗数$\lambda\_1$を導入し，  
$\mathrm{u}\_1^T \mathrm{S} \mathrm{u}\_1 + \lambda\_1(1 - \mathrm{u}\_1^T \mathrm{u}\_1)$  
を制約なしに最大化する．  

---

$\mathrm{u}\_1^T \mathrm{S} \mathrm{u}\_1 + \lambda\_1(1 - \mathrm{u}\_1^T \mathrm{u}\_1)$  
の$\mathrm{u}\_1$に関する微分を$0$とおけば，   
$\mathrm{S} \mathrm{u}\_1 = \lambda\_1 \mathrm{u}\_1$  
という固有方程式が導ける．  

この式から，第一主成分は共分散行列$\mathrm{S}$の固有ベクトルであることがわかる．

---

$\mathrm{S} \mathrm{u}\_1 = \lambda\_1 \mathrm{u}\_1$の左から$\mathrm{u}\_1^T$を掛けて，$\mathrm{u}\_1^T \mathrm{u}\_1=1$を使うと，  
$\mathrm{u}\_1^T \mathrm{S} \mathrm{u}\_1 = \lambda\_1$  
となり，分散と固有値が等しいことがわかる.

したがって，分散は最大固有値$\lambda\_1$を選んだ時に最大となる．  
この$\lambda\_1$に対応する固有ベクトル$\mathrm{u}\_1$を第$1$主成分となる．

---

その他の主成分も，すでに得られている主成分ベクトルに直交するという条件の下で，  
射影後の分散を最大化するような方向を選ぶことで逐次的に得ることができる．

---

一般の場合として$M$次元の射影空間を考えると，データ共分散行列$\mathrm{S}$の大きい順に$M$個の固有値$\lambda\_1,\lambda\_2,\dots,\lambda\_M$に対応する.

$M$個の固有ベクトル$\mathrm{u}\_1, \mathrm{u}\_2, \dots, \mathrm{u}\_M$により$M$個の主成分を得ることができる．

---

### 帰納法による証明  

射影されたデータの分散を最大化するような$M$次元部分空間の上への線形写像が,データ共分散行列$S$の上位$M$個の固有値に属する$M$本の固有ベクトルにより定義されることを示す．

$M=1$の場合は既に示したので，ある一般的な値$M$に対してこの結果が成り立つと仮定して,その下で，$M+1$次元に対しても成り立つことを示せれば良い．

---

第$(M+1)$主成分を$\mathrm{u}\_{M+1}$とする．  
$\mathrm{u}\_{M+1}^T \mathrm{u}\_{M+1}=1$と，$\mathrm{u}\_{M+1}$は既に求めたベクトル$\mathrm{u}\_1,\dots,\mathrm{u}\_M$と直交するという$2$つの制約条件の下で，  
射影後の分散$\mathrm{u}\_{M+1}^T \mathrm{S} \mathrm{u}\_{M+1}$の最大化を考える．

ラグランジュ乗数を導入すれば，  
$\mathrm{u}\_{M+1}^T \mathrm{S} \mathrm{u}\_{M+1} + \lambda\_{M+1}(1 - \mathrm{u}\_{M+1}^T \mathrm{u}\_{M+1}) + \sum\_{i=1}^{M} \eta\_i \mathrm{u}\_{M+1}^T \mathrm{u}\_i$  
を制約式なしで最大化する問題とみなすことができる.  

---

$\mathrm{u}\_{M+1}$に関して微分して$0$とおけば，  
$2\mathrm{S} \mathrm{u}\_{M+1} -2 \lambda\_{M+1} \mathrm{u}\_{M+1} + \sum\_{i=1}^{M} \eta\_i \mathrm{u}\_i = 0$  
左から$\mathrm{u}\_{j}^T$をかければ，$\eta\_j=0 \;\;(j=1,2,\dots,M)$が得られる    
したがって，以下の式を得る．  
$\mathrm{S} \mathrm{u}\_{M+1} = \lambda\_{M+1} \mathrm{u}\_{M+1}$  
さらに，左から$\mathrm{u}\_{M+1}^T$をかければ$\mathrm{u}\_{M+1}^T \mathrm{S} \mathrm{u}\_{M+1} = \lambda\_{M+1}$が得られる．  
以上より，分散を最大にするにはまだ選択されていない固有値の中で  
最大のものに属する固有ベクトル$\mathrm{u}\_{M+1}$であることがわかる．

---

### 主成分分析のまとめ
1. データ集合から共分散行列$\mathrm{S}$を求める
1. $\mathrm{S}\mathrm{u} = \lambda \mathrm{u}$の固有値問題を解く
1. 大きい順に$M$個の固有値に対応する固有ベクトルから主成分を求める
1. 各データに対して$M$個の主成分で基底変換した座標をもとめる


---

### 実験
- 3次元から2次元への次元削減を考える

<img src='./figures/pca/figure_1.png' width='400'></img>  

---

第1主成分（赤）と第2主成分（青）で次元を削減  
<img src='./figures/pca/figure_2.png' width='350'></img>
<img src='./figures/pca/figure_3.png' width='350'></img>  
情報をうまく保持していることがわかる

---

#### スイスロールデータセットで同様の実験を行う  
<img src='./figures/pca/figure_4.png' width='500'></img>  

---

情報をうまく保持できていないことがわかる  

<img src='./figures/pca/figure_5.png' width='350'></img>
<img src='./figures/pca/figure_6.png' width='350'></img>  

※ 左右の図で対応するデータの色が違う。ごめんなさい。

---

### 主成分分析の特徴  

- 基底変換（無相関化）の手法と言える
  - 次元削減やデータの可視化，前処理などに利用
- 主成分を解析的に求めることができる  
  - $n\times n$の固有値問題の計算コストは$\mathcal{O}(n^3)$(結構大変)  
- 非線形な構造を持つデータでは良い結果が得られない
  - 基本的には高次元空間中の線形の部分構造しか取り出せない（直線や平面など)  

---

## 2. カーネル主成分分析

---

### カーネル法  
##### カーネル関数を使う手法の総称
- SVM
- カーネル主成分分析
- カーネルk-平均法
- その他いろいろ

---

### カーネル主成分分析とは  
カーネル関数を用いた主成分分析

<img src='./figures/pca/Figure12.16a.jpg' width='300'></img>
<img src='./figures/pca/Figure12.16b.jpg' width='300'></img>

高次元の特徴ベクトルに変換してから，通常の主成分分析を行う

---

### カーネル関数の定義  
2つの要素$x,x'$に対し，カーネル関数$k(x,x')$は  
$x,x'$それぞれの特徴ベクトルどうしの内積として定義される.

$k(x,x') = \phi(x)^T \phi(x') = \sum\_{m=1}^d \phi\_m(x) \phi_m(x')$

---

### カーネル関数の例
- ガウスカーネル  
  - $k(\mathrm{x}, \mathrm{x}') = \exp (-\beta \|\| \mathrm{x} - \mathrm{x}' \|\|^2)$
- 多項式カーネル
  - $k(\mathrm{x}, \mathrm{x}') = (\mathrm{x}^T \mathrm{x}' + c)^p$
- シグモイドカーネル
  - $k(\mathrm{x}, \mathrm{x}') = \frac{1}{1 + \exp(-\beta \mathrm{x} \mathrm{x}')}$

---

### 1次元のガウスカーネル  
1次元の$x$があったとき，以下のようなベクトル関数を特徴ベクトルに対応させる．  
$\phi(x) = \\{ a\exp(-\beta'(z-x)^2) \;|\; z \in\mathcal{R} \\}$  
つまり，$z$という実数の添え字を持つ  
$\phi\_z(x) = a\exp(-\beta'(z-x)^2)$  
を成分とする無限次元の特徴ベクトルを考えれば良い．  

---

無限次元のベクトルの内積は以下の積分の形でかける．  
$k(x,x') = \int\_{-\infty}^{\infty} \phi\_z(x) \phi\_z(x') dz$  
これに先ほどの式を代入すると，  
\begin{align}
k(x,x') &= a^2 \int\_{-\infty}^{\infty} \exp(-\beta'(z-x)^2 - \beta'(z-x')^2)dz \\\\
&= a^2 \sqrt{\frac{\pi}{2\beta'}} \exp(-\frac{\beta'}{2}(x-x')^2)    
\end{align}
ここで，$a = (\frac{2\beta'}{\pi})^{\frac{1}{4}},\;\; \beta' = 2\beta$とすれば  
$k(x, x') = \exp (-\beta \|\| x - x' \|\|^2)$と一致する．

---

### カーネル関数の構築方法  

$k(\mathrm{x}, \mathrm{x}') = ck\_1(\mathrm{x}, \mathrm{x}')$  
$k(\mathrm{x}, \mathrm{x}') = f(\mathrm{x})k\_1(\mathrm{x}, \mathrm{x}') f(\mathrm{x}')$   
$k(\mathrm{x}, \mathrm{x}') = q(k\_1(\mathrm{x}, \mathrm{x}'))$   
$k(\mathrm{x}, \mathrm{x}') = \exp(k\_1(\mathrm{x}, \mathrm{x}'))$   
$k(\mathrm{x}, \mathrm{x}') = k\_1(\mathrm{x}, \mathrm{x}')+k\_2(\mathrm{x}, \mathrm{x}')$   
$k(\mathrm{x}, \mathrm{x}') = k\_1(\mathrm{x}, \mathrm{x}')k\_2(\mathrm{x}, \mathrm{x}')$  

※ 他にもいろいろな方法があります。（理解できた物だけ載せていますｗ)

---

### カーネル主成分分析  
データ集合$\\{ \mathrm{x}\_n \\}$を考える．  
ここで，$n=1,\dots,N$であり，データは次元$D$の空間にあるとする．

簡単のため，$\bar{\mathrm{x}}$をそれぞれのベクトル$\mathrm{x}\_n$からあらかじめ引いているものとする．  
すなわち，$\sum\_{i=1}^{N} \mathrm{x}\_n =0$であるとする．

---

### 主成分分析の復習  

主成分は共分散行列の固有ベクトル$\mathrm{u}\_i$で  
$\mathrm{S} \mathrm{u}\_i = \lambda\_i \mathrm{u}\_i$で表される．  
ここで，$i=1,\dots,D$であり，$D \times D$の共分散行列$\mathrm{S}$は  
$\mathrm{S} = \frac{1}{N} \sum\_{n=1}^{N} \mathrm{x}\_n \mathrm{x}\_n^T$  
で定義され，固有ベクトルは$\mathrm{u}\_i^T \mathrm{u}\_i = 1$で規格化されているとする

---

ここで，$M$次元特徴空間への非線形変換$\phi(\mathrm{x})$を考える．  
各データ点$\mathrm{x}\_n$はこれにより$\phi(\mathrm{x}\_n)$の上に射影される．
まずは簡単のため，射影されたデータ集合も平均$0$であると仮定する．  
したがって$\sum\_{n=1}^{N}\phi(\mathrm{x}\_n)=0$であると仮定する．  

---

特徴空間における$M \times M$サンプル共分散行列は

$C = \frac{1}{N} \sum\_{n=1}^N \phi(\mathrm{x}\_n) \phi(\mathrm{x}\_n)^T$

で与えられ，通常の主成分分析と同様に固有方程式は$i=1,\dots,M$に対して,

$\mathrm{C} \mathrm{v}\_i = \lambda\_i \mathrm{v}\_i$  

で与えられる．

---

$C = \frac{1}{N} \sum\_{n=1}^N \phi(\mathrm{x}\_n) \phi(\mathrm{x}\_n)^T$と$\mathrm{C} \mathrm{v}\_i = \lambda\_i \mathrm{v}\_i$から    
固有ベクトル$\mathrm{v}\_i$は以下の式を満たす.

$\frac{1}{N}\sum\_{n=1}^{N} \phi(\mathrm{x}\_n) \\{ \phi(\mathrm{x}\_n)^T \mathrm{v}\_i \\} = \lambda\_i \mathrm{v}\_i$

したがって，$\mathrm{v}\_i = \sum\_{n=1}^{N} a\_{in} \phi(\mathrm{x}\_n)$のようにかける.

ただし，$a\_{in} = \frac{1}{N \lambda\_i}\phi(\mathrm{x}\_n)^T \mathrm{v}\_i$

---

$\frac{1}{N}\sum\_{n=1}^{N} \phi(\mathrm{x}\_n) \\{ \phi(\mathrm{x}\_n)^T \mathrm{v}\_i \\} = \lambda\_i \mathrm{v}\_i$と$\mathrm{v}\_i = \sum\_{n=1}^{N} a\_{in} \phi(\mathrm{x}\_n)$

より以下の式が得られる.

$\frac{1}{N} \sum\_{n=1}^{N} \phi(\mathrm{x}\_n) \phi(\mathrm{x})\_n^T \sum\_{m=1}^{N} a\_{im} \phi(\mathrm{x}\_m) = \lambda\_i \sum\_{n=1}^{N} a\_{in} \phi(\mathrm{x}\_n)$

両辺に$\phi(\mathrm{x}\_l)^T$をかければ,

$k(\mathrm{x}\_n, \mathrm{x}\_m) = \phi(\mathrm{x}\_n)^T \phi(\mathrm{x}\_m)$を用いて

$\frac{1}{N} \sum\_{n=1}^{N} k(\mathrm{x}\_l, \mathrm{x}\_n) \sum\_{m=1}^{N}a\_{im} k(\mathrm{x}\_n, \mathrm{x}\_m) = \lambda\_i \sum\_{n=1}^{N} a\_{in} k(\mathrm{x}\_l, \mathrm{x}\_n)$  
とかける．  

---

<font size=6>
$\frac{1}{N} \sum\_{n=1}^{N} k(\mathrm{x}\_l, \mathrm{x}\_n) \sum\_{m=1}^{N}a\_{im} k(\mathrm{x}\_n, \mathrm{x}\_m) = \lambda\_i \sum\_{n=1}^{N} a\_{in} k(\mathrm{x}\_l, \mathrm{x}\_n)$
</font>

は行列とベクトルを用いれば以下のようにかける．
$\mathrm{K}^2 \mathrm{a}\_i = \lambda\_i N \mathrm{K} \mathrm{a}\_i$  

ただし，$\mathrm{a}\_i$ は$N$次元ベクトルでその要素は$n=1,\dots,N$に対して$a\_{in}$   
$\mathrm{K}$は$i$行$j$列に要素$\mathrm{K}\_{ij} = k(\mathrm{x}\_i, \mathrm{x}\_j)$をもつ行列  
$\mathrm{K}$をグラム行列と呼ぶ．

---

$\mathrm{a}\_{i}$に対する解を，次の方程式を解くことにより得られる．  
$\mathrm{K} \mathrm{a}\_i = \lambda\_i N \mathrm{a}\_i$  

また，係数$\mathrm{a}\_i$に対する規格化条件は以下で得られる．
<font size=6>
$1 = \mathrm{v}\_i^T \mathrm{v}\_i = \sum\_{n=1}^{N} \sum\_{m=1}^{N} a\_{in} a\_{im} \phi(\mathrm{x}\_n)^T \phi(\mathrm{x}\_m) = \mathrm{a}\_i^T \mathrm{K} \mathrm{a}\_i = \lambda\_i N \mathrm{a}\_i^T \mathrm{a}\_i$
</font>

(次へ)

---

固有値問題を解いたとして，主成分への射影をカーネル関数で表すことができる．  
$y\_i(\mathrm{x}) = \phi(\mathrm{x})^T \mathrm{v}\_i = \sum\_{n=1}^{N} a\_{in} \phi(\mathrm{x})^T \phi(\mathrm{x}) = \sum\_{n=1}^{N} a\_{in} k(\mathrm{x},\mathrm{x}\_n)$  
で与えられ，カーネル関数だけを通して表されていることがわかる．  

---

射影されたデータ集合$\phi(\mathrm{x}\_n)$の平均が$0$でない場合も考える  
射影されたデータ点の中心化された点を$\tilde{\phi}(\mathrm{x}\_n)$で表すと，  
$\tilde{\phi}(\mathrm{x}\_n) = \phi(\mathrm{x}\_n) - \frac{1}{N} \sum\_{l=1}^{N} \phi(\mathrm{x}\_l)$  
で与えられる.(次へ)

---

グラム行列の対応する要素は，

<font size = 5>
\begin{align}
\tilde{\mathrm{K}}\_{nm} &= \tilde{\phi}(\mathrm{x}\_n)^T \tilde{\phi}(\mathrm{x}\_m) \\\
&= \phi(\mathrm{x}\_n)^T \phi(\mathrm{x}\_m) - \frac{1}{N} \sum\_{l=1}^{N} \phi(\mathrm{x}\_n)^T \phi(\mathrm{x}\_l) - \frac{1}{N} \sum\_{l=1}^{N} \phi(\mathrm{x}\_l)^T \phi(\mathrm{x}\_m) + \frac{1}{N^2} \sum\_{j=1}^{N} \sum\_{l=1}^{N} \phi(\mathrm{x}\_j)^T \phi(\mathrm{x}\_l) \\\\
&= k(\mathrm{x}\_n, \mathrm{x}\_m) - \frac{1}{N} \sum\_{l=1}^{N} k(\mathrm{x}\_l, \mathrm{x}\_m)-\frac{1}{N} \sum\_{l=1}^{N} k(\mathrm{x}\_n, \mathrm{x}\_l) + \frac{1}{N^2} \sum\_{j=1}^{N} \sum\_{l=1}^{N} k(\mathrm{x}\_j, \mathrm{x}\_l)
\end{align}
</font>


---

これは行列表記で次のようにかける．  
$\tilde{\mathrm{K}} = \mathrm{K} - \bf{1}\_N \mathrm{K} - \mathrm{K}\bf{1}\_N + \bf{1}\_N \mathrm{K} \bf{1}\_N$  
ただし，$\bf{1}\_N$は全ての要素が$\frac{1}{N}$という値をとる$N \times N$行列である．

したがって，カーネル関数だけを用いて$\tilde{\mathrm{K}}$を求めることができる．  
あとは$\tilde{\mathrm{K}}$を使って固有値と固有ベクトルを求めれば良い．  

---

#### カーネル主成分分析  
1. カーネル関数を選ぶ
1. グラム行列から　$\tilde{\mathrm{K}}$を求める
2. $\tilde{\mathrm{K}}\mathrm{v} = \lambda \mathrm{v}$の固有値問題を解く
3. 大きい順に$M$個の固有値に対応する固有ベクトルから主成分を求める
4. 各データに対して$M$個の主成分で基底変換した座標をもとめる

---

#### スイスロールでカーネル主成分分析を行う  
<img src='./figures/pca/figure_4.png' width='500'></img>  

---

情報をうまく保持していることがわかる  
<img src='./figures/pca/figure_4.png' width='350'></img>
<img src='./figures/pca/1-5.png' width='350'></img>  
※ 左右の図で対応するデータの色違う。ごめんなさい

---

- まとめ
  - 主成分分析を紹介した
  - ついにカーネル法について簡単に触れた
  - カーネル主成分分析なら，非線形なデータに対しても有効であることを確認した  
- 感想
  - カーネル法の特徴や強力さが実感できた
  - 他の次元削減の手法と比較してみたいと思った
