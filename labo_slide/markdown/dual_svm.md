
## SVMと双対問題

---

### 目次

1. SVMの双対表現
2. 主問題と双対問題の関係
3. SVMの最適性条件
4. カーネルによる一般化

---

## 1. SVMの双対表現

---

SVMは以下のように最適化問題として定式化できます．  

\begin{align}
    \min\_{\boldsymbol{w}, b, \boldsymbol{\xi}}
    & & &
    \frac{1}{2}||\boldsymbol{w}||^2 + C \sum\_{i\in[n]} \xi\_i
    \\\\
    \mathrm{s.t.}
    & & &
    y\_i(\boldsymbol{w}^{\mathrm{T}}\boldsymbol{x}\_i + b) \geq 1 - \xi\_i
    \quad
    i \in [n]
    \\\\
    & & &
    \xi\_i \geq 0
    \quad
    i \in [n].
\end{align}

※ 変数の表記の意味がわからなければ聞いてください。

---

この問題はSVMの<font color='firebrick'>主問題(primal problem)</font>と呼ばれています。  

---

この主問題に対して<font color='firebrick'>双対問題(dual problem)</font>という問題を導くことで、  
同じ最適化問題に対して違った見方を与えられることがあります。

---

SVMでは多くの場合双対問題を考えます。  
理由は多分以下の２つです。  

- 双対問題のほうが解きやすい場合がある
- 非線形化が考えやすい

---

双対問題を導くために先程の最適化問題を以下のように書き換えます。（移項しただけ）

\begin{align}
    \min\_{\boldsymbol{w}, b, \boldsymbol{\xi}}
    & & &
    \frac{1}{2}||\boldsymbol{w}||^2 + C \sum\_{i\in[n]} \xi\_i
    \\\\
    \mathrm{s.t.}
    & & &
    -(y\_i(\boldsymbol{w}^{\mathrm{T}}\boldsymbol{x}\_i + b)- 1 + \xi\_i) \leq 0
    \quad
    i \in [n]
    \\\\
    & & &
    -\xi\_i \leq 0
    \quad
    i \in [n].
\end{align}

---

ここで新たに$\alpha\_i \in \mathcal{R}^{+}, i \in [n]$と$\mu\_i \in \mathcal{R}^{+}$という非負の変数を導入し、  
以下の<font color='firebrick'>ラグランジュ関数(Lagrange function)</font>を考えます。  

\begin{align}
L(\boldsymbol{w}, b, \boldsymbol{\xi}, \boldsymbol{\alpha}, \boldsymbol{\mu}) =
\frac{1}{2}\|\| \boldsymbol{w} \|\|^2 + C\sum\_{i\in[n]}\xi\_i
-\sum\_{i\in[n]}\alpha\_i(y\_i(\boldsymbol{w}^{\mathrm{T}}\boldsymbol{x}\_i + b -1 + \xi\_i)) - \sum\_{i\in[n]}\mu\_i\xi\_i
\end{align}

$\boldsymbol{w}, b ,\boldsymbol{\xi}$を<font color='firebrick'>主変数(primal variable)</font>、  
$\boldsymbol{\alpha}, \boldsymbol{\mu}$を<font color='firebrick'>双対変数(dual variable)</font>と呼びます。  

---

次にラグランジュ関数を双対変数について最大化した関数を  
$\mathcal{P}(\boldsymbol{w}, b, \boldsymbol{\xi})$として以下のように定義します。   

\begin{align}
\mathcal{P}(\boldsymbol{w}, b, \boldsymbol{\xi}) = \\\\ \max\_{\boldsymbol{\alpha}\geq\boldsymbol{0},\boldsymbol{\mu}\geq \boldsymbol{0}} L(\boldsymbol{w}, b, \boldsymbol{\xi}, \boldsymbol{\alpha}, \boldsymbol{\mu})
\end{align}

$\boldsymbol{\alpha}\geq\boldsymbol{0},\boldsymbol{\mu}\geq \boldsymbol{0}$はそれぞれの要素が全て0以上という制約を表します。  

---

さらにこの関数を主変数について最小化する以下の最適化問題を考えます。  

\begin{align}
\min\_{\boldsymbol{w}, b, \boldsymbol{\xi}}\mathcal{P}(\boldsymbol{w}, b, \boldsymbol{\xi}) =
\min\_{\boldsymbol{w}, b, \boldsymbol{\xi}}\max\_{\boldsymbol{\alpha}\geq\boldsymbol{0},\boldsymbol{\mu}\geq \boldsymbol{0}} L(\boldsymbol{w}, b, \boldsymbol{\xi}, \boldsymbol{\alpha}, \boldsymbol{\mu})
\end{align}

実はこの最適化問題はもとの最適化問題と等価になります。   
次にそれを説明します。  


---

まず前提知識として、最適化問題において制約条件をすべて満たしていることを  
<font color='firebrick'>実行可能(feasible)</font>であるといいます。  
<br>
例えば主変数に$-y\_i(\boldsymbol{w}^{\mathrm{T}}\boldsymbol{x}\_i + b -1 +\xi\_i)>0$か$-\xi\_i>0$となる$i$が  
存在する場合、実行可能ではなくなります。   
(これを踏まえて次の式を見てください。)

---

関数$\mathcal{P}(\boldsymbol{w}, b, \boldsymbol{\xi})$内の$\max$は$L$の後ろ2項にのみ関わるため、  
以下の関係が成立します。  

\begin{eqnarray}
\mathcal{P}(\boldsymbol{w}, b, \boldsymbol{\xi}) &=& \frac{1}{2}||\boldsymbol{w}||^2 + C \sum\_{i\in[n]} \xi\_i \\\\
 &\;&+\max\_{\boldsymbol{\alpha}\geq\bÓoldsymbol{0}, \boldsymbol{\mu}\geq\boldsymbol{0}} \Bigr( -\sum\_{i\in[n]} \alpha\_i (y\_i(\boldsymbol{w}^{\mathrm{T}}\boldsymbol{x}\_i + b)- 1 + \xi\_i) -\sum\_{i\in[n]}\mu\_i \xi\_i \Bigr) \\\\
 &=&
\begin{cases}
    \frac{1}{2}\|\|\boldsymbol{w}\|\|^2 + C\sum\_{i\in[n]}\xi\_i & (主変数が実行可能な場合) \\\\
    定義なし & (主変数が実行可能でない場合)
  \end{cases}
\end{eqnarray}

※ 実行可能ではない場合、$L$をどこまでも大きくすることができてしまいます。  

---

一方で実行可能な場合は、全ての$i$で$-y\_i(\boldsymbol{w}^{\mathrm{T}}\boldsymbol{x}\_i + b -1 +\xi\_i)\leq 0$かつ$-\xi\_i \leq 0$なので、  
後半の2つの項の最大値は0であり、もとの最適化問題の目的関数と一致します。  

\begin{eqnarray}
\mathcal{P}(\boldsymbol{w}, b, \boldsymbol{\xi}) &=& \frac{1}{2}||\boldsymbol{w}||^2 + C \sum\_{i\in[n]} \xi\_i \\\\
 &\;&+\max\_{\boldsymbol{\alpha}\geq\boldsymbol{0}, \boldsymbol{\mu}\geq\boldsymbol{0}} \Bigr( -\sum\_{i\in[n]} \alpha\_i (y\_i(\boldsymbol{w}^{\mathrm{T}}\boldsymbol{x}\_i + b)- 1 + \xi\_i) -\sum\_{i\in[n]}\mu\_i \xi\_i \Bigr) \\\\
 &=&
\begin{cases}
    \frac{1}{2}\|\|\boldsymbol{w}\|\|^2 + C\sum\_{i\in[n]}\xi\_i & (主変数が実行可能な場合) \\\\
    定義なし & (主変数が実行可能でない場合)
  \end{cases}
\end{eqnarray}

---

要するに、もとの最適化問題が以下の最適化問題と等価だったってことです。  

\begin{align}
\min\_{\boldsymbol{w}, b, \boldsymbol{\xi}}\mathcal{P}(\boldsymbol{w}, b, \boldsymbol{\xi}) =
\min\_{\boldsymbol{w}, b, \boldsymbol{\xi}}\max\_{\boldsymbol{\alpha}\geq\boldsymbol{0},\boldsymbol{\mu}\geq \boldsymbol{0}} L(\boldsymbol{w}, b, \boldsymbol{\xi}, \boldsymbol{\alpha}, \boldsymbol{\mu})
\end{align}

---

今度は、ラグランジュ関数を主変数について最小化した関数を定義します。  

\begin{align}
\mathcal{D}(\boldsymbol{\alpha},\boldsymbol{\mu}) = \min\_{\boldsymbol{w},b,\boldsymbol{\xi}}L(\boldsymbol{w}, b, \boldsymbol{\xi}, \boldsymbol{\alpha}, \boldsymbol{\mu})
\end{align}

主変数には何も制約がないことに注意しましょう。  

---

そして、$\mathcal{D}(\boldsymbol{\alpha},\boldsymbol{\mu})$を双対変数$\boldsymbol{\alpha},\boldsymbol{\mu}$について最大化する  
以下の問題を双対問題と呼ぶことにします。  

\begin{align}
\max\_{\boldsymbol{\alpha}\geq\boldsymbol{0},\boldsymbol{\mu}\geq\boldsymbol{0}}\mathcal{D}(\boldsymbol{\alpha},\boldsymbol{\mu}) =\max\_{\boldsymbol{\alpha}\geq\boldsymbol{0}, \boldsymbol{\mu} \geq \boldsymbol{0}} \min\_{\boldsymbol{w},b,\boldsymbol{\xi}}L(\boldsymbol{w}, b, \boldsymbol{\xi}, \boldsymbol{\alpha}, \boldsymbol{\mu})
\end{align}

主問題と似た形なので関係が気になりますよね。  
(主問題の$\min$と$\max$を入れ替えた形になっています。)

---

この最適化問題は整理すると双対変数のみを使って表現できます。  
詳細な式変形は割愛しますが、雰囲気だけ紹介しようと思います。  

---

まずは内側の最小化について考えます。  
$L$の$\boldsymbol{w}, b, \xi\_i$の偏微分が0になるという条件を導出すると以下のようになります。  

\begin{eqnarray}
\frac{\partial L}{\partial \boldsymbol{w}} &=& \boldsymbol{w} - \sum\_{i\in[n]} \alpha\_i y\_i \boldsymbol{x}\_i = \boldsymbol{0} \\\\
\frac{\partial L}{\partial b} &=& - \sum\_{i\in[n]}\alpha\_i y\_i = 0 \\\\
\frac{\partial L}{\partial \xi\_i} &=& C - \alpha\_i - \mu\_i = 0, \; i\in[n]
\end{eqnarray}

---

これらを$L$に代入すれば$\boldsymbol{\alpha}$のみの関数になります。  

\begin{eqnarray}
L &=& \frac{1}{2}\|\|\boldsymbol{w}\|\|^2 - \sum\_{i\in[n]}\alpha\_iy\_i\boldsymbol{w}^{\mathrm{T}}\boldsymbol{x}\_i
-b \sum\_{i\in[n]}\alpha\_i y\_i + \sum\_{i\in [n]}\alpha\_i + \sum\_{i\in[n]}(C-\alpha\_i-\mu\_i)\xi\_i \\\\
&=& -\frac{1}{2}\sum\_{i,j\in[n]}\alpha\_i \alpha\_j y\_i y\_j \boldsymbol{x}\_i^{\mathrm{T}}\boldsymbol{x}\_j + \sum\_{i\in[n]}\alpha\_i
\end{eqnarray}

さらに$C - \alpha\_i = \mu\_i \geq 0$から、$C - \alpha \geq 0$という制約が得られます。  

---

これらをまとめるとSVMの双対問題は以下のように表現できます。  


\begin{align}
    \max\_{\boldsymbol{\alpha}}
    & & &
    -\frac{1}{2} \sum\_{i,j\in[n]}\alpha\_i \alpha\_j y\_iy\_j \boldsymbol{x}\_i^{\mathrm{T}} \boldsymbol{x}\_j + \sum\_{i\in[n]}\alpha\_i
    \\\\
    \mathrm{s.t.}
    & & &
    \sum\_{i\in[n]}\alpha\_i y\_i = 0
    \\\\
    & & &
    0 \leq \alpha\_i \leq C,
    \quad
    i \in [n].
\end{align}

---

## 2. 主問題と双対問題の関係

---

主問題と双対問題の関係について考えます。  
それぞれ以下のように定義しました  

\begin{align}
\min\_{\boldsymbol{w}, b, \boldsymbol{\xi}}\mathcal{P}(\boldsymbol{w}, b, \boldsymbol{\xi}) =
\min\_{\boldsymbol{w}, b, \boldsymbol{\xi}}\max\_{\boldsymbol{\alpha}\geq\boldsymbol{0},\boldsymbol{\mu}\geq \boldsymbol{0}} L(\boldsymbol{w}, b, \boldsymbol{\xi}, \boldsymbol{\alpha}, \boldsymbol{\mu})
\end{align}

\begin{align}
\max\_{\boldsymbol{\alpha}\geq\boldsymbol{0},\boldsymbol{\mu}\geq\boldsymbol{0}}\mathcal{D}(\boldsymbol{\alpha},\boldsymbol{\mu}) =\max\_{\boldsymbol{\alpha}\geq\boldsymbol{0}, \boldsymbol{\mu} \geq \boldsymbol{0}} \min\_{\boldsymbol{w},b,\boldsymbol{\xi}}L(\boldsymbol{w}, b, \boldsymbol{\xi}, \boldsymbol{\alpha}, \boldsymbol{\mu})
\end{align}

---

簡単のため、主問題と双対問題ともに最適な値を達成する  
主変数$\boldsymbol{w},b,\boldsymbol{\xi}$と双対変数$\boldsymbol{\alpha},\boldsymbol{\mu}$の組が  
存在することをあらかじめ仮定しておきます。  

---

主問題の最適解を$\boldsymbol{w}^{\*},b^{\*},\boldsymbol{\xi}^{\*}$、双対問題の最適解を$\boldsymbol{\alpha}^{\*},\boldsymbol{\mu}^{\*}$と  
すると以下の関係が成立します。  

\begin{eqnarray}
\mathcal{D}(\boldsymbol{\alpha}^{\*}, \boldsymbol{\mu}^{\*}) &=& \min\_{\boldsymbol{w},b,\boldsymbol{\xi}}L(\boldsymbol{w},b,\boldsymbol{\xi},\boldsymbol{\alpha}^{\*},\boldsymbol{\mu}^{\*})  \\\\
&\leq& L(\boldsymbol{w}^{\*}, b^{\*}, \boldsymbol{\xi}^{\*}, \boldsymbol{\alpha}^{\*}, \boldsymbol{\mu}^{\*}) \\\\
&\leq& \max\_{\boldsymbol{\alpha} \geq \boldsymbol{0}, \boldsymbol{\mu} \geq \boldsymbol{0}} L(\boldsymbol{w}^{\*}, b^{\*}, \boldsymbol{\xi}^{\*}, \boldsymbol{\alpha}, \boldsymbol{\mu})
= \mathcal{P}(\boldsymbol{w}^{\*}, b^{\*}, \boldsymbol{\xi}^{\*})
\end{eqnarray}

---

ここから双対問題の最適値が主問題の最適値以下という以下の関係がわかります。  

\begin{eqnarray}
\mathcal{D}(\boldsymbol{\alpha}^{\*}, \boldsymbol{\mu}^{\*}) \leq \mathcal{P}(\boldsymbol{w}^{\*}, b^{\*}, \boldsymbol{\xi}^{\*})
\end{eqnarray}

これは<font color='firebrick'>弱双対性(weak duality)</font>と呼ばれる性質で、  
どのような最適化問題でも成立します。  

---

ところがSVMでは、より強い以下の<font color='firebrick'>強双対性(strong duality)</font>と呼ばれる  
性質が成り立つことが知られています。  

\begin{eqnarray}
\mathcal{D}(\boldsymbol{\alpha}^{\*}, \boldsymbol{\mu}^{\*}) = \mathcal{P}(\boldsymbol{w}^{\*}, b^{\*}, \boldsymbol{\xi}^{\*})
\end{eqnarray}

(証明は分かんないです．許して下さい．）

---

要はSVMでは主問題の最適解と双対問題の最適解が一致します。  
なので、解きやすいほうを解けば良いってことです。  


もう少しだけ関係性について考えてみることにします。  

---

強双対性が成立する時、以下の等式が得られます。  

\begin{eqnarray}
\mathcal{P}(\boldsymbol{w}^{\*}, b^{\*}, \boldsymbol{\xi}^{\*})
=L(\boldsymbol{w}^{\*}, b^{\*}, \boldsymbol{\xi}^{\*},\boldsymbol{\alpha}^{\*}, \boldsymbol{\mu}^{\*})
=\mathcal{D}(\boldsymbol{\alpha}^{\*}, \boldsymbol{\mu}^{\*})
\end{eqnarray}


それから、定義から以下の不等式が成り立ちます。

\begin{eqnarray}
\mathcal{P}(\boldsymbol{w}^{\*}, b^{\*}, \boldsymbol{\xi}^{\*})
&=&\max\_{\boldsymbol{\alpha}\geq\boldsymbol{0}, \boldsymbol{\mu}\geq\boldsymbol{0}}L(\boldsymbol{w}^{\*}, b^{\*}, \boldsymbol{\xi}^{\*},\boldsymbol{\alpha}, \boldsymbol{\mu})
\geq L(\boldsymbol{w}^{\*}, b^{\*}, \boldsymbol{\xi}^{\*},\boldsymbol{\alpha}, \boldsymbol{\mu})\\\\
\mathcal{D}(\boldsymbol{\alpha}^{\*}, \boldsymbol{\mu}^{\*})
&=&\min\_{\boldsymbol{w},b,\boldsymbol{\xi}}L(\boldsymbol{w}, b, \boldsymbol{\xi},\boldsymbol{\alpha}^{\*}, \boldsymbol{\mu}^{\*})
\leq L(\boldsymbol{w}, b, \boldsymbol{\xi},\boldsymbol{\alpha}^{\*}, \boldsymbol{\mu}^{\*})
\end{eqnarray}

---

下２つの不等式を上の等式に代入することで、以下の不等式が得られます。

\begin{eqnarray}
L(\boldsymbol{w}^{\*}, b^{\*}, \boldsymbol{\xi}^{\*},\boldsymbol{\alpha}, \boldsymbol{\mu})
\leq L(\boldsymbol{w}^{\*}, b^{\*}, \boldsymbol{\xi}^{\*},\boldsymbol{\alpha}^{\*}, \boldsymbol{\mu}^{\*})
\leq L(\boldsymbol{w}, b, \boldsymbol{\xi},\boldsymbol{\alpha}^{\*}, \boldsymbol{\mu}^{\*})
\end{eqnarray}

この不等式は
$L(\boldsymbol{w}^{\*}, b^{\*}, \boldsymbol{\xi}^{\*},\boldsymbol{\alpha}^{\*}, \boldsymbol{\mu}^{\*})$
が、主変数については極小値で、  
双対変数については極大値であることを意味しています。  

---

このような点は関数の<font color='firebrick'>鞍点（saddle point)</font>と呼ばれます。  

図

よって主問題と双対問題は最適解であるラグランジュ関数の鞍点に対して、  
異なる方向からアプローチするものと考えることができます。  

---

## 3. SVMの最適性条件

---

最適化問題の解を得る際には、  
解の最適性を判定するための条件がわかっていると良いです。  

---

SVMの解の最適性は<font color='firebrick'>KKT条件（Karush-Kuhn-Tucker condition）</font>が  
必要十分条件になることが知られています。  

KKT条件はSVMの計算や解がもつ性質を考える上で非常に重要です。  

---

KKT条件は以下の式たちです。  

\begin{eqnarray}
\frac{\partial L}{\partial \boldsymbol{w}} &=& \boldsymbol{w} - \sum\_{i\in[n]} \alpha\_i y\_i \boldsymbol{x}\_i = \boldsymbol{0} \tag{1.1} \\\\
\frac{\partial L}{\partial b} &=& \sum\_{i\in[n]}\alpha\_i y\_i = 0 \tag{1.2} \\\\
\frac{\partial L}{\partial \xi\_i} &=& C - \alpha\_i - \mu\_i = 0, \; i\in[n] \tag{1.3}\\\\
\end{eqnarray}

\begin{eqnarray}
-(y\_i(\boldsymbol{w}^{\mathrm{T}}\boldsymbol{x}\_i +b)-1 - \xi\_i) &\leq&0, \; i\in[n] \tag{1.4}\\\\
-\xi\_i &\leq&0, \; i\in [n] \tag{1.5} \\\\
\alpha\_i &\geq& 0, \; i\in[n] \tag{1.6} \\\\
\mu\_i &\geq&0,\; i\in [n] \tag{1.7} \\\\
\alpha\_i(y\_i(\boldsymbol{w}^{\mathrm{T}}\boldsymbol{x}\_i +b)-1 - \xi\_i) &=&0, \; i\in[n] \tag{1.8} \\\\
\mu\_i \xi\_i &=&0, \; i \in[n]] \tag{1.9}
\end{eqnarray}

証明はそこまで難しくないですが、長いので省略します。  

---

式(1.1)〜(1.3)はラグランジュ関数の主変数に関する微分、  
式(1.4)〜(1.5)は主問題の制約条件、  
式(1.6)〜(1.7)は双対変数の非負条件、  
式(1.8)〜(1.9)は双対変数と不等式制約の乗算によって定義される  
 <font color='firebrick'>相補性条件(complementarity condition)</font>という条件です。  

---

式(1.1)から得られる$\boldsymbol{w}=\sum\_{i\in[n]}\alpha\_i y\_i\boldsymbol{x}\_i$の関係を用いれば、  
决定関数は$\boldsymbol{\alpha}$を用いて以下のように表現できることがわかります。  

\begin{eqnarray}
f(\boldsymbol{x}) = \sum\_{i\in[n]}\alpha\_i y\_i \boldsymbol{x}\_i^{\mathrm{T}}\boldsymbol{x} + b
\end{eqnarray}


---

$b$は相補性条件から求めることができます。  

まず式(1.8)から、$\alpha\_i>0$のとき
$y\_i(\boldsymbol{w}^{\mathrm{T}}\boldsymbol{x}\_i +b)-1 - \xi\_i =0$です。   
さらに式(1.3)から得られる$\mu\_i = C - \alpha\_i$を相補条件(1.9)に代入すると、  
$(C-\alpha\_i)\xi\_i=0$となり、$\alpha\_i < C$であれば$\xi\_i=0$であることがわかります。  

---

まとめると、以下の関係性が得られます。  

\begin{eqnarray}
y\_i(\boldsymbol{w}^{\mathrm{T}}\boldsymbol{x}\_i + b)-1=0, \; i\in\\{ i\in[n] | 0<\alpha\_i < C \\}
\end{eqnarray}

$\boldsymbol{w}=\sum\_{i\in[n]}\alpha\_i y\_i \boldsymbol{x}\_i$を代入して変形すれば、  
$b$も$\boldsymbol{\alpha}$から計算できることがわかります。  

\begin{eqnarray}
b = y\_i - \sum\_{i'\in[n]} \alpha\_{i'}y\_{i'}\boldsymbol{x}\_{i'}^{\mathrm{T}}\boldsymbol{x}\_{i'},\; i\in\\{ i\in[n] | 0 < \alpha < C \\}
\end{eqnarray}

---

集合$\\{i\in[n] | 0 < \alpha\_i < C\\}$内の任意$i$について先程の四季は成立しますが、  
数値計算上の安定性を考慮して、条件を満たす全ての$i$で平均をとることがあります。  

とにかく、双対問題の解さえわかれば决定境界が求まることが分かりました。  

---

## 4. カーネルによる一般化

---

ここまでのSVMは决定境界が線形の場合を考えていました。  
SVMでは非線形な决定境界も考えることができます。  

---

ちなみに双対問題はSVMの非線形化を考える上で重要な役割を果たしていて、  
双対問題であるからこそカーネルトリックという技を使うことができます。  

---

まず、入力$\boldsymbol{x}$を何らかの特徴空間$\mathcal{F}$へ写像する関数$\phi : \mathbb{R}^d \rightarrow \mathcal{F}$を考えます。  

この$\phi(\boldsymbol{x})$を新たな特徴ベクトルだと解釈すると$f(\boldsymbol{x})$は以下のよう変化します。  

\begin{eqnarray}
f(\boldsymbol{x}) = \boldsymbol{w}^{\mathrm{T}}\phi(\boldsymbol{x}) + b
\end{eqnarray}

パラメータ$\boldsymbol{w}$も特徴空間$\mathcal{F}$ないの要素として定義されます。($\boldsymbol{w}\in\mathcal{F}$)

---

写像$\phi$による変換が非線形なら、分類境界はもとの$\boldsymbol{x}$の空間では非線形になりえます。  
一方、$\phi(\boldsymbol{x})$の空間$\mathcal{F}$では分類境界は線形になります。  

---

変換後の$\phi(\boldsymbol{x})$を新たな特徴ベクトルとみなして$\boldsymbol{x}$と置き換えれば、  
双対問題の導出は線形の場合と同様です。 以下の式が導けます。   

\begin{align}
    \max\_{\boldsymbol{\alpha}}
    & & &
    -\frac{1}{2} \sum\_{i,j\in[n]}\alpha\_i \alpha\_j y\_iy\_j \phi(\boldsymbol{x}\_i)^{\mathrm{T}} \phi(\boldsymbol{x}\_j) + \sum\_{i\in[n]}\alpha\_i
    \\\\
    \mathrm{s.t.}
    & & &
    \sum\_{i\in[n]}\alpha\_i y\_i = 0
    \\\\
    & & &
    0 \leq \alpha\_i \leq C,
    \quad
    i \in [n].
\end{align}

この最適化問題において$\phi$は内積の形でのみ現れていることが大事です。  

---

$\phi$は内積$\phi(\boldsymbol{x}\_i)^{\mathrm{T}} \phi(\boldsymbol{x}\_j)$の形でのみ現れているので、  
$\boldsymbol{x}\_i, \boldsymbol{x}\_j$から直接$\phi(\boldsymbol{x}\_i), \phi(\boldsymbol{x}\_j)$を計算する必要は必ずしもなく、  
とにかく内積$\phi(\boldsymbol{x}\_i)^{\mathrm{T}} \phi(\boldsymbol{x}\_j)$さえ計算できればよいです。  

---

そこで<font color='firebrick'>カーネル関数（kernel function）</font>を定義します。  

\begin{eqnarray}
K(\boldsymbol{x}\_i, \boldsymbol{x}\_j) = \phi(\boldsymbol{x}\_i)^{\mathrm{T}}\phi(\boldsymbol{x}\_j)
\end{eqnarray}

ある特定の性質を満たす関数を用いると$\phi(\boldsymbol{x})$の計算を行うことなく、  
$K(\boldsymbol{x}\_i, \boldsymbol{x}\_j)$を計算できることが知られています。

---

よく用いられるカーネル関数として  
以下の<font color='firebrick'>RBF(radial basis function)カーネル</font>が知られています。  
(ガウスカーネルとかガウシアンカーネルとも呼ばれます。)  

\begin{eqnarray}
K(\boldsymbol{x}\_i, \boldsymbol{x}\_j) = \exp(-\gamma ||\boldsymbol{x}\_i - \boldsymbol{x}\_j||^2)
\end{eqnarray}

$\gamma>0$はハイパーパラメータ事前に設定する必要があります。  

---

カーネル関数を使うと双対問題は以下のように記述できます。  

\begin{align}
    \max\_{\boldsymbol{\alpha}}
    & & &
    -\frac{1}{2} \sum\_{i,j\in[n]}\alpha\_i \alpha\_j y\_iy\_j K(\boldsymbol{x}\_i, \boldsymbol{x}\_j) + \sum\_{i\in[n]}\alpha\_i
    \\\\
    \mathrm{s.t.}
    & & &
    \sum\_{i\in[n]}\alpha\_i y\_i = 0
    \\\\
    & & &
    0 \leq \alpha\_i \leq C,
    \quad
    i \in [n].
\end{align}

---

同様に$f(\boldsymbol{x})$もカーネル関数によって以下のように表現できます。  

\begin{eqnarray}
f(\boldsymbol{x}) = \sum\_{i\in[n]}\alpha\_i y\_i K(\boldsymbol{x}\_i, \boldsymbol{x}) + b
\end{eqnarray}

---

以上です。  
