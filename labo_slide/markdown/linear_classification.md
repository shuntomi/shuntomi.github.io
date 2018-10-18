### 線形判別モデル
2018/07/04
富澤駿
---

<font size='5'>
4章の前半をまとめました
<p><img src='./figures/progress_3_png/prml.jpg' width='200'></img>  
質問歓迎です．
</font>

---

### 目次
1. 分類の目的  
2. 識別関数  
  2.1 2クラス  
  2.2 多クラス  
3. 最小二乗
4. フィッシャーの線形判別
5. パーセプトロン　
6. ロジスティック回帰

---

#### 分類の目的
<font size='6'>
ある入力$\mathrm{x}$を$K$個の離散クラス$c_k \;\; (k=1,\dots,K)$の  
一つに割り当てること  
</font>
<font size='4'>
(ただし，各クラスは互いに重ならない -> 各データは一つのクラスに割り当てられる)
</font>

---

<font size ='5'>
入力空間は決定領域に分離される  
決定領域の境界を決定面と呼ぶ
</font>
<p><img src='./figures/progress_3_png/fig1-1.png' width='300'></img></p>

---

<font size ='5'>
入力空間は決定領域に分離される  
決定領域の境界を決定面と呼ぶ
</font>
<p><img src='./figures/progress_3_png/fig1-2.png' width='300'></img></p>


---

datasetに適用
<font size='5'>
クラスが割り当てられたデータデータセットから代表点を求める  
</font>
<p><img src='./figures/progress_3_png/fig1-3.png' width='300'></img></p>

---

#### 識別関数

- <font size='5'> 2クラス </font>  
 - <font size='5'> $y(\mathrm{x})=\mathrm{w}^T \mathrm{x} + w_0$</font>  
 - <font size='5'> $y(\mathrm{x})>0$ -> class1 </font>  
 - <font size='5'> $y(\mathrm{x})<0$ -> class2 </font>  

- <font size='5'> 他クラス </font>  
 - <font size='5'> $y\_{c\_k}(\mathrm{x})=\mathrm{w}\_{c\_k}^T \mathrm{x} + w\_{c\_k0}$ </font>  
 - <font size='5'> one-versus-the-rest </font>  
 - <font size='5'> one-versus-one </font>  
 - <font size='5'> $\rm{arg}\max\_{c\_j} y\_{c\_k}(\mathrm{x})$ -> classj </font>

---

#### 2クラス
<font size='5'> $y(\mathrm{x})=\mathrm{w}^T \mathrm{x} + w_0$ </font>
<p><img src='./figures/progress_3_png/Figure4.1.jpg' width='300'></img></p>
<font size='5'>
決定境界は$y(\mathrm{x})=0$で，$D$次元入力空間中の$(D-1)$次元超平面に対応する．
</font>

---

wは決定面に対して直交する．  
<font size='5'>
決定面上にある2点$\mathrm{x}_A$と$\mathrm{x}_B$を考える．
$y(\mathrm{x}_A) = y(\mathrm{x}_B) = 0$であるので，  
$\mathrm{w}^T(\mathrm{x}_A - \mathrm{x}_B) =0$であり，  
ベクトル$w$は決定面上にあるすべてのベクトルに直交する．
</font>

---

原点から決定面までの距離  
<font size='5'>
$\mathrm{x}$が決定面上にある場合，$y(\mathrm{x})=0$となる．  
ゆえに，決定面までの距離は  
$\frac{\mathrm{w}^T \mathrm{x}}{\|\| \mathrm{w} \|\|} = - \frac{w_0}{\|\| \mathrm{w} \|\|}$
</font>

---

任意の点$\mathrm{x}$から決定面までの距離  
<font size='5'>
$\mathrm{x}\_{\perp}$を決定面上への直交射影であるとすると，  
$\mathrm{x} = \mathrm{x}\_{\perp} + r \frac{\mathrm{w}}{\|\| \mathrm{w} \|\|}$  
両辺に$\mathrm{w}^T$を掛け，$w\_0$を加え，  
$y(\mathrm{x})=\mathrm{w}^T \mathrm{x} + w\_0$と$y(\mathrm{x}\_{\perp})=\mathrm{w}^T \mathrm{x}\_{\perp} + w\_0 =0$を利用すると  
$r = \frac{y(\mathrm{x})}{\|\| \mathrm{w} \|\|}$



---

#### 多クラス(その1)
1対他分類器(one-versus-the-rest classifier)  
<font size='5'>
ある特定のクラス$c_k$に入る点とそのクラスに入らない点とに分類する  
２クラス分類器を$K-1$個利用する．
</font>
<p><img src='./figures/progress_3_png/Figure4.2a.jpg' width='300'></img></p>

---

#### 多クラス(その2)

1対1分類器(one-versus-one classifier)
<font size='5'>
すべての可能なクラスの組の２クラス識別関数を考え，  
$K(K-1)/2$個の２クラス識別関数を利用する．
</font>
<p><img src='./figures/progress_3_png/Figure4.2b.jpg' width='300'></img></p>

---

#### 多クラス(その3)
<font size='5'>
すべての$j \neq k$に対して$y\_{c\_k}(\mathrm{x}) > y\_{c\_j}(\mathrm{x})$である場合，  
点$\mathrm{x}$はクラス$c_k$に割り当てられる．
</font>

---

境界  
<font size='5'>
クラス$c\_k$とクラス$c\_j$間の決定境界は$y\_{c\_k} (\mathrm{x}) = y\_{c\_j} (\mathrm{x})$で与えられる  
したがって，境界は  
$(\mathrm{w}\_{c\_k}-\mathrm{w}\_{c\_j})^T \mathrm{x} + (w\_{c\_k0} - w\_{c\_j0})=0$  
で定義される$(D-1)$次元の超平面になる
</font>

---

決定領域　　
<font size='5'>
決定領域は凸領域となる
</font>．
<p><img src='./figures/progress_3_png/Figure4.3.jpg' width='300'></img></p>

---

凸領域  
<font size='5'>
決定領域$R_k$にある2点$\mathrm{x}_A$と$\mathrm{x}_B$を考える  
2点$\mathrm{x}_A$と$\mathrm{x}_B$を結ぶ直線上にある任意の点$\mathrm{\hat{x}}$は，  
$\mathrm{\hat{x}} = \lambda \mathrm{x}_A + (1 - \lambda)\mathrm{x}_B \;\; (1\leq \lambda \leq 1)$ で表せる．
</font>

---

凸領域  
<font size='5'>
関数の線形性から以下が成立する．  
$y\_{c\_k}(\mathrm{\hat{x}}) = \lambda y\_{c\_k}(\mathrm{x}\_A) + (1 - \lambda)y\_{c\_k}(\mathrm{x}\_B)$  
2点$\mathrm{x}\_A$と$\mathrm{x}\_B$はいずれも決定領域$R\_k$内にあるので，すべての$j \neq k$に対して  
$y\_{c\_k}(\mathrm{x}\_A) > y\_{c\_j}(\mathrm{x}\_B)$ と $y\_{c\_k}(\mathrm{x}\_B) > y\_{c\_j}(\mathrm{x}\_B)$が成立し，  
$y\_k(\mathrm{\hat{x}}) > y\_j(\mathrm{\hat{x}})$となるため，任意の$\mathrm{\hat{x}}$も決定領域$R_k$内にあることがわかる
</font>

---

<font size= '5'>
ユークリッド距離による1NN法は線形識別関数による識別と同等である.
</font>
<p><img src='./figures/progress_3_png/fig1-3.png' width='300'></img></p>

---

<font size='5'>
ユークリッド距離における1NN法は$\mathrm{x}$を  
$\|\| \mathrm{x} - \mathrm{\mu}\_{c\_k} \|\| = \|\| \mathrm{x} \|\|^2 -2\mathrm{\mu}\_{c\_k}^T \mathrm{x} + \|\| \mathrm{\mu}\_{c\_k} \|\|^2$  
を最小にするクラス$c\_k$に割り当てる  
これは,線形識別関数  
$y\_{c\_k}(\mathrm{x}) = \mathrm{\mu}\_{c\_k}^T \mathrm{x} - \frac{1}{2}\|\| \mathrm{\mu}\_{c\_k} \|\|^2$  
が最大になるときと等価である
</font>

---

<font size='5'>
線形識別関数のパラメータを学習する3つのアプローチについて紹介する
</font>
* <font size='6'> 最小二乗 </font>　  
* <font size='6'> フィッシャーの線形判別 </font>
* <font size='6'> パーセプトロン

---

### k符号化

<font size='5'>
クラスが全部で$K$個あるとして，それらを$c\_1,c\_2,\dots,c\_K$とする．  
各$c\_i$に対して，要素数が$K$で$i$番目だけが1のベクトル（他は全て0)
を定める．  
$\mathrm{p}\_{c\_i} = (0,0,\dots,0,1,0,\dots,0)^T$  
</font>

---

<font size='5'>
学習データ$D={(\mathrm{x}\_1, y\_1),(\mathrm{x}\_2, y\_2),\dots,(\mathrm{x}\_n, y\_n)}$に対して，  
$\mathrm{y}(\mathrm{x}) = (y\_{c\_1}(\mathrm{x}), y\_{c\_2}(\mathrm{x}), \dots,y\_{c\_K}(\mathrm{x}))$，$\mathrm{W}=(\mathrm{w}\_{c\_1}, \mathrm{w}\_{c\_2}, \dots, \mathrm{w}\_{c\_K})$とすると，  
\begin{align}
\sum\_{i=1}^{n} \|\| \mathrm{y}(\mathrm{x}\_i) - \mathrm{p}\_{y\_i} \|\|^2 &= \sum\_{i=1}^{n} \|\| \mathrm{W}^T \mathrm{x\_i} - \mathrm{p}\_{y\_i} \|\|^2 \\\\    
&= \sum\_{i=1}^{n}(\mathrm{x}\_i^T \mathrm{W} - \mathrm{p}\_{y\_i}^T)(\mathrm{W}^T \mathrm{x}\_i - \mathrm{p}\_{y\_i}) \\\\  
&= \sum\_{i=1}^T(\mathrm{x}\_i^T \mathrm{W} \mathrm{W}^T \mathrm{x}\_i -2\mathrm{x}\_i^T \mathrm{W} \mathrm{p}\_{y\_i} + \|\| \mathrm{p}\_{y\_i} \|\|^2) \\\
\end{align}

これを$E(\mathrm{W})$と置いて行列$W$で偏微分すると  
\begin{align}
\frac{\partial E(\mathrm{W})}{\partial \mathrm{W}} &= 2\sum\_{i=1}^{n}(\mathrm{x}\_i \mathrm{x}\_i^T \mathrm{W} - \mathrm{x}\_i \mathrm{p}\_{y\_i})  \\\\
&= 2(\mathrm{X}^T\mathrm{X}\mathrm{W} - \mathrm{X}^T \mathrm{P})      
\end{align}
※ $\mathrm{X}=(\mathrm{x}\_1, \mathrm{x}\_2,\dots,\mathrm{x}\_n)^T \;\; \mathrm{P}=(\mathrm{p}\_{y\_1}, \mathrm{p}\_{y\_2},\dots, \mathrm{p}\_{y\_n})$と置いた．
</font>

---

行列の微分  
<font size='5'>
$\frac{\partial}{\partial X} \mathrm{a}^T \mathrm{X} \mathrm{X}^T \mathrm{b} = (\mathrm{a}\mathrm{b}^T + \mathrm{b}\mathrm{a}^T)\mathrm{X}$  
$\frac{\partial}{\partial \mathrm{X}} \mathrm{a}^T \mathrm{X} \mathrm{b} = \mathrm{a}\mathrm{b}^T$
</font>

---

最小二乗  
<font size='5'>
$\frac{\partial E(\mathrm{W})}{\partial \mathrm{W}} = 0$と置くと  
$\mathrm{X}^T \mathrm{X} \mathrm{W} = \mathrm{X}^T \mathrm{P}$  
$\mathrm{W} = (\mathrm{X}^T \mathrm{X})^{-1} \mathrm{X}^T \mathrm{P}$  
となり，パラメータ$W$が解析的に求まることがわかる．
</font>

---

実装  
<p><img src='./figures/progress_3_png/fig2-0.png' width='300'></img></p>

---

実装  
<p><img src='./figures/progress_3_png/fig2-3.png' width='300'></img></p>

---

最小二乗の欠点  
<font size='5'>
出力$\mathrm{y}(\mathrm{x}) = (y\_{c\_1}(\mathrm{x}), y\_{c\_2}(\mathrm{x}), \dots,y\_{c\_K}(\mathrm{x}))$を確率と解釈できない．  
($\mathrm{y}(\mathrm{x})$の要素の和が1と各要素が(0,1)の範囲であることを仮定していないため)  
</font>

---

最小二乗の欠点  
<font size='5'>
外れ値に弱い
<p><img src='./figures/progress_3_png/Figure4.4a.jpg' width='300'></img>
<img src='./figures/progress_3_png/Figure4.4b.jpg' width='300'></img></p>赤線：最小二乗，緑線：ロジスティック回帰
</font>

---

実装  
<font size='5'>
<p><img src='./figures/progress_3_png/fig2-1.png' width='300'></img>
<img src='./figures/progress_3_png/fig2-2.png' width='300'></img></p>
</font>

---

最小二乗の欠点  
<font size='5'>
最小二乗法は条件付き確率分布にガウス分布を仮定した場合の最尤法  
しかし，誤差$(\mathrm{y}(\mathrm{x}) - \mathrm{p}\_{y\_i})$は正規分布に従わない（両方0-1)  
</font>
<p><img src='./figures/progress_3_png/Figure4.5a.jpg' width='300'></img>
<img src='./figures/progress_3_png/Figure4.5b.jpg' width='300'></img></p>

---

最小二乗の欠点  
<font size='5'>
実装　
<p><img src='./figures/progress_3_png/fig2-3.png' width='300'></img>
<img src='./figures/progress_3_png/fig2-4.png' width='300'></img></p>
複数のクラス(K>2)が一直線に並んでいるような分布をしている場合に上手くいかない．
</font>

---

最小二乗の欠点  
<font size='5'>
実装　
<p><img src='./figures/progress_3_png/fig2-5.png' width='300'></img>
<img src='./figures/progress_3_png/fig2-6.png' width='300'></img>
<img src='./figures/progress_3_png/fig2-7.png' width='300'></img></p>
複数のクラス(K>2)が一直線に並んでいるような分布をしている場合に上手くいかない．
</font>

---

最小二乗まとめ
<font size='5'>
最小二乗は分類問題においては使うべきではない．
</font>

---

フィッシャーの線形判別  
<font size='5'>
$D$次元入力ベクトルを得て，それを一次元に射影するとする.　  
$y = \mathrm{w}^T \mathrm{x}$  
$y$に設定し閾値を設定し，  
$y \ge - w_0$ -> クラス1  
$y < -w_0$ -> クラス2  
とすれば，標準的な2クラスの線形分類器が得られる．  
</font>
---

<font size='5'>
一時的に，一次元への射影は情報の損失を発生させるので，もとの$D$
次元空間では分離されていたクラスが一次元では大きく重なり合ってしまうかもしれない．
<p>  </p>
<p>  </p>
重みベクトル$\mathrm{w}$を調節し，クラスの分離を最大にする射影を選択する．  
---> フィッシャーの線形判別
</font>

---

<font size='5'>
左図より右図の方がクラスを上手く分離できている．
<p><img src='./figures/progress_3_png/Figure4.6a.jpg' width='300'></img>
<img src='./figures/progress_3_png/Figure4.6b.jpg' width='300'></img></p>
このような射影方向を見つけたい．
</font>

---

平均の分離度を見る  
<font size='5'>
クラス$C\_1$の点が$N\_1$個，クラス$C\_2$の点が$N\_2$個ある2クラス問題を考える．  
2つのクラスの平均ベクトルは以下で与えられる．  
$\mathrm{m}\_1 = \frac{1}{N\_1} \sum\_{n \in C\_1} \mathrm{x}\_n,\;\; \mathrm{m}\_2 = \frac{1}{N\_2} \sum\_{n \in C\_2} \mathrm{x}\_n$  
最も簡単な分離度の測定方法は，射影されたクラス平均の分離度をみること．  
$m_2 - m_1 = \mathrm{w}^T(\mathrm{m}_2 - \mathrm{m}_1)$  
($m_k = \mathrm{w}^T \mathrm{m}_k$はクラス$C_k$から射影されたデータの平均)
</font>

---

制約付き最大化問題    
<font size='5'>
$m_2 - m_1 = \mathrm{w}^T(\mathrm{m}_2 - \mathrm{m}_1)$に    
$\sum_i w_i^2 = 1$の制約を加えて最大化する．  
ラグランジュ未定乗数法を用いれば，ラグランジュ関数は以下のように表せる．  
$L = \mathrm{w}^T(\mathrm{m\_2} - \mathrm{m\_1}) + \lambda(\mathrm{w}^T \mathrm{w} -1)$  
$\frac{\partial L}{\partial \mathrm{w}} = \mathrm{m}\_2 - \mathrm{m}\_1 + 2\lambda \mathrm{w}=0$より，  
$\mathrm{w} = - \frac{1}{2\lambda}(\mathrm{m}\_2 - \mathrm{m}\_1)$  
以上より，$\mathrm{w} \propto (\mathrm{m}\_2 - \mathrm{m}\_1)$が得られる．
</font>

---

実装  
<p><img src='./figures/progress_3_png/fig3-2.png' width='300'></img></p>  
<font size='5'>
これだと，線形分離できる場合にも重なり合う部分が多くなる
</font>

---

分散も考慮  
<p><img src='./figures/progress_3_png/fig-mv.png' width='500'></img></p>
<font size='5'>
小さい分散であることが望ましい <- フィッシャーが提案   
</font>

---

クラス内分散  
<font size='5'>
全データに対するクラス内分散を$s\_1^2 + s\_2^2$と定義する．  
ここで，$s\_k^2 = \sum\_{n \in C_k}(y\_n - m\_k)^2$としている．  
<p> </p>
フィッシャーの判別基準はクラス内分散とクラス間分散の比で定義され  
$J(\mathrm{w}) = \frac{(m\_1 - m\_2)^2}{s\_1^2 + s\_2^2}$で与えられる．
</font>

---

フィッシャーの判別基準  

<font size='5'>
判別基準は$J(\mathrm{w}) = \frac{\mathrm{w}^T \mathrm{S}\_B \mathrm{w}}{\mathrm{w}^T \mathrm{S}\_W \mathrm{w}}$と書き直せる．  
ここで，  
$\mathrm{S}\_B = (\mathrm{m}\_2 - \mathrm{m}\_1)(\mathrm{m}\_2 - \mathrm{m}\_1)^T$  
$\mathrm{S}\_W = \sum\_{n \in C\_1}(\mathrm{x}\_n - \mathrm{m}\_1)^T + \sum\_{n \in C\_2}(\mathrm{x}\_n - \mathrm{m}\_2)(\mathrm{x}\_n - \mathrm{m}\_2)^T$  
$\mathrm{S}\_B$はクラス間共分散行列，$\mathrm{S}\_W$はクラス内共分散行列と呼ぶ
</font>

---

<font size='5'>
$J(\mathrm{w}) = \frac{\mathrm{w}^T \mathrm{S}\_B \mathrm{w}}{\mathrm{w}^T \mathrm{S}\_W \mathrm{w}}$  
$\frac{\partial J(\mathrm{w})}{\partial \mathrm{w}} = (2(\mathrm{S}_B \mathrm{w})(\mathrm{w}^T \mathrm{S}_W \mathrm{w})(\mathrm{w}^T \mathrm{S}_W \mathrm{w}) - 2(\mathrm{w}^T \mathrm{S}_B \mathrm{w})(\mathrm{S}_W \mathrm{w}))/(\mathrm{w}^T \mathrm{S}_w \mathrm{w})^2=0$  
よって，  
$(\mathrm{w}^T \mathrm{S}_B \mathrm{w})\mathrm{S}_W\mathrm{w} = (\mathrm{w}^T \mathrm{S}_W \mathrm{w})\mathrm{S}_B \mathrm{w}$  
ここで，  
$\mathrm{S}_B \mathrm{w} = (\mathrm{m}_2 - \mathrm{m}_1)((\mathrm{m}_2 - \mathrm{m}_1)^T \mathrm{w}) \propto (\mathrm{m}_2 - \mathrm{m}_1)$  
なので  
$\mathrm{w} \propto \mathrm{S}_W^{-1}\mathrm{S}_B \mathrm{w} \propto \mathrm{S}_W^{-1}(\mathrm{m}_2 - \mathrm{m}_1)$  
これをフィッシャーの線形判別という．
</font>

---

実装  
<p><img src='./figures/progress_3_png/fig3-4.png' width='300'></img></p>  
---

最小二乗との関連  
<font size='5'>
最小二乗：目的変数にできるだけ近いように（誤差最小化）  
フィッシャーの判別基準：クラスの分離を最大化するように  
<p></p>
2クラス分類の最小二乗の特別な場合がフィッシャーの判別基準と等価になる．
</font>

---

<font size='5'>
データの個数を$N = N\_1 + N\_2$，  
($N\_i$はクラス$C\_i$に属するデータの個数)  
$C\_1$に対する目的変数値を$N/N\_1$，$C\_2$に対する目的変数値を$-N/N\_2$とする  
この条件下で二乗和誤差  
$E = \frac{1}{2}\sum\_{n=1}^N (\mathrm{w}^T \mathrm{x}\_n + w\_0 - t\_n)^2$  
の最小化を考えると，  
$\mathrm{w} \propto \mathrm{S}\_W^{-1}(\mathrm{m}\_2 - \mathrm{m}\_1)$  
が導ける．（計算は難しくないけど長いので省略）
</font>


---

フィッシャーの線形判別まとめ
<font size='5'>
実際には判別を行っているのではなく，射影方向の方向のみを与えていることに注意  
識別の手法ではなく，次元を削減する手法であると判断することができる
</font>

---

パーセプトロン
<font size='5'>
以下の一般化線形モデルで表される2クラスのモデル  
$y(\mathrm{x}) = f(\mathrm{w}^T \phi(\mathrm{x}))$  
<p></p>
$f(a)$は非線形活性化関数  
* $f(a)=+1$ ($a \ge 0$)  
* $f(a)=-1$ ($a < 0$)  

---

目的変数値  
<font size='5'>
クラス$C_1$に対して，目的変数値$t=+1$  
クラス$C_2$に対して，目的変数値$t=-1$  
とする．（こうしたほうが都合がいい）  
</font>

---

<font size='5'>
クラス$C_1$の$\mathrm{x}_n$に対して$\mathrm{w}^T \phi(\mathrm{x}_n) > 0$  
クラス$C_2$の$\mathrm{x}_n$に対して$\mathrm{w}^T \phi(\mathrm{x}_n) < 0$  
となるように重みベクトル$\mathrm{w}$を求めていることに注目すると  
目的変数値$t \in \\{-1, +1\\}$を使用して，  
$\mathrm{w}^T \phi(\mathrm{x}_n) t_n > 0$  
となるような重みベクトル$\mathrm{w}$を求めていることと等価  
</font>

---

誤差関数  
<font size='5'>
正しく分類されたデータに対しては誤差$0$を割り当て，  
誤分類されたデータに対しては誤差$-\mathrm{w}^T \phi(\mathrm{x}\_n)t\_n$を割り当てる．   
<p></p>
このとき誤差関数は以下のように表せる．   
$E(\mathrm{w}) = -\sum\_{n \in M} \mathrm{w}^T \phi(\mathrm{x}\_n)t\_n$  
($M$は誤分類されたデータの集合)  
この誤差関数をパーセプトロン基準と呼ぶ   
</font>

---

誤差関数最小化  
<font size='5'>
誤差関数の最小化に確率的最急降下法を用いると，  
$\mathrm{w}$の更新式は以下のように表せる．  
$\mathrm{w}^{(\tau+1)} = \mathrm{w}^{(\tau)} - \eta \nabla E(\mathrm{w}) = \mathrm{w}^{(\tau)} + \eta \phi(\mathrm{x}_n)t_n$  
（$\tau$はステップ数，$\eta$は学習率パラメータ）
</font>

---

更新式の解釈  
<font size='5'>
データを正しく分類 -> $\mathrm{w}$に変化を加えない  
$C_1$のデータを誤分類 -> $\mathrm{w}$に$\eta \phi(\mathrm{x}_n)$を加える  
$C_2$のデータを誤分類 -> $\mathrm{w}$から$\eta \phi(\mathrm{x}_n)$を引く    
</font>

---

<p><img src='./figures/progress_3_png/Figure4.7a.jpg' width='200'></img>
<img src='./figures/progress_3_png/Figure4.7b.jpg' width='200'></img></p>  
<p><img src='./figures/progress_3_png/Figure4.7c.jpg' width='200'></img>
<img src='./figures/progress_3_png/Figure4.7d.jpg' width='200'></img></p>  

---

<font size='5'>
$\mathrm{w}^{(\tau+1)} =  \mathrm{w}^{(\tau)} + \eta \phi(\mathrm{x}_n)t_n$の関係式から，  
$-\mathrm{w}^{(\tau+1)T}\phi(\mathrm{x}_n)t_n = -\mathrm{w}^{(\tau)T}\phi(\mathrm{x}_n)t_n - \eta (\phi(\mathrm{x}_n) t_n)^T \phi(\mathrm{x}_n)t_n < - \mathrm{w}^{(\tau)T}\phi(\mathrm{x}_n)t_n$  
となるので，誤分類されたデータに対しての誤差を減少させていることがわかる   
<p></p>
総誤差関数を減少させることは保証していないので注意．
</font>

---

パーセプトロンの収束定理  
<font size='5'>
厳密解が存在する場合（学習データ集合が線形に分離可能な場合）は  
有限回の繰り返しで厳密解に収束することが保証されている  
</font>

---

注意点
<font size='5'>
必要な繰り返し回数はかなり多く，実用的には，分離できない問題なのか，  
単に収束が遅い問題なのかの区別が収束するまでわからない  
<p></p>
線形分離可能な場合でも，パラメータの初期値やデータの提示順に依存して  
様々な解に収束してしまう．
</font>

---

ロジスティック回帰  
<font size='5'>
2クラスのロジスティック回帰モデル  
$p(C_1 | \mathrm{x}) = \sigma(\mathrm{w}^T \phi(\mathrm{x}))$  
のパラメータ$\mathrm{w}$を最尤法で決定する問題を考える．  
以降のスライドでは簡単のため$\phi(\mathrm{x})=\phi$とかく．  
$p(C_1 | \mathrm{x}) = \sigma(\mathrm{w}^T \phi)$  
</font>

---

<font size='5'>
学習データの一組$(\phi, t)(t \in \\{ 0,1\\})$を，  
$\mathrm{x}$がクラス$C_1$の時に$t=1$と定めれば，  
尤度は  
$L(\mathrm{w}|\phi,t) = \sigma(\mathrm{w}\phi)^t \bigl\\{1 - \sigma(\mathrm{w}^T \phi) \bigr\\}^{1-t}$  
とかける．
</font>

---

<font size='5'>
よって，学習データ$D=\\{(\phi_1,t_1),(\phi_2,t_2),\dots,(\phi_n,t_n)\\}$に対する対数尤度は  
$L(\mathrm{w}|D)=\prod_i \sigma(\mathrm{w}^T \phi_i)^{t_i} \bigl\\{ 1-\sigma(\mathrm{w}^T \phi_i) \bigr\\}^{1-t_i}$  
より，  
$\mathrm{ln}L(\mathrm{w}|D) = \sum_i \Bigl[ t_i \mathrm{ln}\sigma(\mathrm{w}^T \phi_i) + (1-t_i)\mathrm{ln}\bigl\\{ 1 -\sigma(\mathrm{w}^T \phi_i)\bigr\\} \Bigr]$  
とかける．
</font>

---

準備  
<font size='5'>
\begin{align}
\frac{d}{dx}\sigma(x) &= \frac{d}{dx} \frac{1}{1+\exp(-x)} \\\\
&= \frac{\exp(-x)}{(1 + \exp(-x))^2}  \\\\
&= \frac{1}{1 + \exp(-x)}\Bigl\\{ 1 - \frac{1}{1 + \exp(-x)} \Bigr\\} \\\\
&= \sigma(x)(1 - \sigma(x))
\end{align}
</font>

---

準備  
<font size='5'>
\begin{align}
\frac{d}{d\mathrm{w}} \mathrm{ln} \sigma(\mathrm{w}^T \phi) &= \frac{1}{\sigma(\mathrm{w}^T \phi)} \frac{d}{d\mathrm{w}}\sigma(\mathrm{w}^T \phi) \\\\
&= \frac{1}{\sigma(\mathrm{w}^T \phi)}\sigma(\mathrm{w}^T \phi) \bigl\\{1 - \sigma(\mathrm{w}^T \phi) \bigr\\} \frac{d}{d\mathrm{w}}\mathrm{w}^T \phi \\\\
&= \bigl\\{ 1 - \sigma(\mathrm{w}^T \phi) \bigr\\}\phi
\end{align}  
同様に，  
$\frac{d}{d\mathrm{w}} \mathrm{ln} \bigl\\{ 1 - \sigma(\mathrm{w}^T \phi) \bigr\\} = -\sigma(\mathrm{w}^T \phi) \phi = \bigl\\{ 0 - \sigma(\mathrm{w}^T \phi) \bigr\\}\phi$  
となる
</font>

---

<font size='5'>
対数尤度を$(-1)$倍したものを誤差関数とし，以下のように表す，  
$E(\mathrm{w}) = -\sum_i \Bigl[ t_i \mathrm{ln}\sigma(\mathrm{w}^T \phi_i) + (1-t_i)\mathrm{ln}\bigl\\{ 1 -\sigma(\mathrm{w}^T \phi_i)\bigr\\} \Bigr]$  
これを$\mathrm{w}$で微分すると，  
$\frac{d}{d\mathrm{w}} E(\mathrm{w}) = -\sum_i \Bigl\[ t_i\bigl\\{ 1 - \sigma(\mathrm{w}^T \phi_i) \bigr\\}\phi_i + (1- t_i) \bigl\\{ 0- \sigma(\mathrm{w}^T \phi_i)  \bigr\\}\phi_i \Bigr\]$  
と表せるが，これはさらに，  
$\frac{d}{d\mathrm{w}} E(\mathrm{w}) = -\sum_i \bigl\\{t_i - \sigma(\mathrm{w}^T \phi_i) \bigr\\}\phi_i$  
とかける
</font>

---

<font size='5'>
さらに，  
\begin{align}
\mathrm{t} &= (t_1, t_2, \dots, t_n)^T \\\\
\mathrm{y} &= (\sigma(\mathrm{w}^T \phi_1), \sigma(\mathrm{w}^T \phi_2), \dots, \sigma(\mathrm{w}^T phi_n))^T \\\\
\mathrm{X} &= (\phi_1, \phi_2, \dots, \phi_n)^T
\end{align}
と置けば，誤差関数の勾配は以下のように表せる．  
$\frac{d}{d \mathrm{w}} E(\mathrm{w}) = -\mathrm{X}^T (\mathrm{t} - \mathrm{y}) = \mathrm{X}^T (\mathrm{y} - \mathrm{t})$
</font>

---

<font size='5'>
ところで，  
$\frac{d}{d\mathrm{w}} E(\mathrm{w}) = -\sum_i \bigl\\{t_i - \sigma(\mathrm{w}^T \phi_i) \bigr\\}\phi_i$  
を再び$\mathrm{w}$で微分すると以下のヘッセ行列を得る．  
$\mathrm{H} =  \sum_i \sigma(\mathrm{w}^T \phi_i) \bigl\\{ 1 - \sigma(\mathrm{w}^T \phi_i) \bigr\\} \phi_i \phi_i^T$  
これを整理すると，以下のように表せる．  
$\mathrm{H} = \mathrm{X}^T \mathrm{R} \mathrm{X}$  
<br>
$\mathrm{R} = \begin{pmatrix}
\sigma(\mathrm{w}^T \phi_1)(1 - \sigma(\mathrm{w}^T \phi_1)) & \cdots & 0\\\\
\vdots & \ddots& \vdots  \\\\
0 & \cdots & \sigma(\mathrm{w}^T \phi_n)(1-\sigma(\mathrm{w}^T \phi_n)) \\\\
\end{pmatrix}$
</font>

---

<font size='5'>
$\mathrm{H}$は$\mathrm{R}$の対角成分が正であることに注意すると，  
任意のベクトル$\mathrm{u}$に対して$\mathrm{u}^T \mathrm{H} \mathrm{u}>0$なので，  
$\mathrm{H}$が正定値行列であることがわかる．
<br>
したがって，誤差関数は唯一の極小解を持つ．  
</font>

---

誤差関数の最小化  
<font size='5'>
解析的に解を求めることはできないので数値解法を用いる  
最急降下法  
ニュートン・ラフソン法  
</font>

---

最急降下法  
<font size='5'>
$\mathrm{w}^{(\tau+1)} = \mathrm{w}^{(\tau)} - \eta \nabla E(\mathrm{w}^{(\tau)})$  
</font>

---

ニュートン・ラフソン法  
<font size='5'>
$\mathrm{w}^{(\tau+1)} = \mathrm{w}^{(\tau)} - \mathrm{H}^{-1} \nabla E(\mathrm{w}^{(\tau)})$  
$\mathrm{H}^{-1}$は$\mathrm{w}$に依存しているため，
$\mathrm{w}$が新たに求められるたびに  
$\mathrm{H}^{-1}$(正確には$\mathrm{R}$)を再計算する必要がある．  
<br>
これが理由で，このアルゴリズムは反復最重み付け最小二乗法  
またはIRLS(iterative reweighted least squares method)として知られている．    
</font>

---

固定基底関数  
<font size='5'>
もとの入力ベクトル$\mathrm{x}$を，基底関数ベクトル$\phi(\mathrm{x})$に変換する意味を考えてみる．  
決定境界は，特徴空間$\phi$においては線形であるが，  
もとの入力空間$\mathrm{x}$においては非線形になる．
<p><img src='./figures/progress_3_png/Figure4.12a.jpg' width='300'></img>
<img src='./figures/progress_3_png/Figure4.12b.jpg' width='300'></img></p>  
</font>

---

固定基底関数の限界  
<font size='5'>
基底関数を固定するという仮定から，  
入力次元数$D$に対して，指数的に基底関数の数を  
増やしていく必要がある．(次元の呪い)  
<br>
サポートベクトルマシンやニューラルネットワークは，  
これらの問題を上手く克服する．    
</font>

---

まとめ  
<font size='5'>
今回はPRMLの4章前半をまとめました．  
難しい内容ですが，非常に勉強になるので，  
今後も時間をかけて読み進めていこうと思います．  
</font>
