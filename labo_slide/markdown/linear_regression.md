### regression model
2018/07/02  
Shun Tomizawa

---

### table of contents
1. linear regression  
2. ridge regression
3. lasso regression
4. generalization
5. <font size='6'>minimize sum-of-squares error and maximize likelihood </font>
6. bayesian regression
7. other regression models

---

### notation
<font size ='10'>
$x, w, t$ : scalar  
$\mathrm{x}, \mathrm{w}, \mathrm{t}$ : vector  
$\mathrm{X}, \mathrm{W}, \mathrm{T}$ : matrix
</font>

---

### linear regression model
the simplest model is  
$y(\mathrm{x}, \mathrm{w}) = w_0 + w_1x_1 + \dots + w_Dx_D$
<font size='5'>※ $\mathrm{x}=(x_1,\dots,x_D)^T$</font>

---

### feature
* linear with respect to $\mathrm{w}$  
* linear with respect to $\mathrm{x}$  
* model is too simple (poor expressive power)  

---

### extend the model
add linear combination of the non-linear function  
$y(\mathrm{x},\mathrm{w})=w\_0 + \sum\_{j=1}^{M-1} w\_j \phi\_j(\mathrm{x})$

<font size='5'>
* $\phi_j(\mathrm{x})$ called basis function    
* $w_0$ called bias parameter  
</font>

---

### linear regression model
if we add dummy basis function($\;\phi\_0(\mathrm{x})\;$)  
$y(\mathrm{x}, \mathrm{w}) = \sum\_{j=1}^{M-1}w\_j \phi\_j(\mathrm{x}) = \mathrm{w}^T \mathrm{\phi}(\mathrm{x})$  
<font size='5'>
※ $\mathrm{w} = (w\_0,\dots,w\_{M-1})^T$,
$\mathrm{\phi}=(\phi\_0,\dots,\phi\_{M-1})^T$
</font>

---

### basis function
there are various options for the basis function  
* polynomial basis  
* gaussian basis  
* logistic sigmoid basis  

---

### polynomial basis
$\phi_j(x) = x^j$
<p><img src='./figures/progress_2_png/Figure3.1a.jpg'</p>

---

### gaussian basis
$\phi\_j(x) = \exp{\Bigl\( -\frac{(x-\mu\_j)^2}{2s^2} \Bigr\)}$  
<p><img src='./figures/progress_2_png/Figure3.1b.jpg'</p>

---

### logistic sigmoid basis
$\phi_j(x) = \sigma\Bigl\( \frac{x-\mu_j}{s}  \Bigr\)$  
$\sigma (a) = \frac{1}{1 + \exp{(-a)}}$
<p><img src='./figures/progress_2_png/Figure3.1c.jpg'</p>

---

### linear regression model

$y(\mathrm{x}, \mathrm{w}) =  \mathrm{w}^T \mathrm{\phi}(\mathrm{x})$  

---

### feature
* linear with respect to $\mathrm{w}$  
* non-linear with respect to $\mathrm{x}$  
* we can choose a favorite basis  

---

### linear regression
we want to minimize the error function  
$\min\_{\mathrm{w}} E(\mathrm{w})$  
$E(\mathrm{w}) = \frac{1}{2}\sum\_{i=1}^N (\mathrm{w}^T \mathrm{\phi}(\mathrm{x}^{(i)}) - y^{(i)})^2 = \frac{1}{2}\|\| \Phi\mathrm{w} - \mathrm{y}\|\|^2$  
<font size='5'>
※ $\Phi = (\phi(\mathrm{x}^{(1)}), \phi(\mathrm{x}^{(2)}), \dots ,\phi(\mathrm{x}^{(N)}))$  
※ this error function called sum-of-squares error
</font>

---

### linear regression
$E(\mathrm{w}) = \frac{1}{2}\|\| \Phi \mathrm{w} - \mathrm{y}  \|\|^2$  
<font size='5'> partial differential in a </font>  
$\frac{E(\partial \mathrm{w})}{\partial \mathrm{w}} =  \Phi^T (\Phi \mathrm{w} - \mathrm{y})$  
<font size='5'> put with $\frac{E(\partial \mathrm{w})}{\partial \mathrm{w}} = 0$</font>  
$\Phi^T \Phi \mathrm{w} = \Phi^T \mathrm{y}$  
$\therefore \mathrm{w} = (\Phi^T \Phi)^{-1} \Phi^T \mathrm{y}$  

<font size=5><a href="http://nbviewer.jupyter.org/github/yagimimi/progress_slide/blob/gh-pages/ipynb/LinearRegression.ipynb">link</a></font>
---

### redge regression
$E(\mathrm{w}) =  \frac{1}{2} \|\| \Phi\mathrm{w} - \mathrm{y}\|\|^2 + \frac{\lambda}{2} \|\| \mathrm{w} \|\|^2$  
$\frac{E(\partial \mathrm{w})}{\partial \mathrm{w}} = \Phi^T (\Phi \mathrm{w} - \mathrm{y}) + \lambda\mathrm{w} = 0$  
$(\Phi^T \Phi -\lambda \mathrm{I}) \mathrm{w} = \Phi^T \mathrm{y}$  
$\therefore \mathrm{w} = (\Phi^T \Phi - \lambda \mathrm{I})^{-1} \Phi^T \mathrm{y}$  

<font size=5><a href="http://nbviewer.jupyter.org/github/yagimimi/progress_slide/blob/gh-pages/ipynb/RidgeRegression.ipynb">link</a></font>

---

### lasso regression
$E(\mathrm{w}) = \frac{1}{2} \|\| \Phi\mathrm{w} - \mathrm{y}\|\|^2 + \frac{\lambda}{2} \sum\_{j=1}^{M-1} \| \mathrm{w} \|$  

<font size=5><a href="http://nbviewer.jupyter.org/github/yagimimi/progress_slide/blob/gh-pages/ipynb/LassoRegression.ipynb">link</a></font>

---

### feature
* not be solved analytically (nondifferentiable)
  * solved by coordinate descent
* perform variable selection(some of the parameters to 0)

---

### generalization
general of the redge and lasso  
$E(\mathrm{w}) = \frac{1}{2} \|\| \Phi\mathrm{w} - \mathrm{y}\|\|^2 + \frac{\lambda}{2} \sum\_{j=1}^{M-1} \| \mathrm{w} \|^q$  

<p><img src='./figures/progress_2_png/Figure3.3.jpg' width='500px'></img></p>


---

### aaa

$E(\mathrm{w}) = \frac{1}{2} \|\| \Phi\mathrm{w} - \mathrm{y}\|\|^2 + \frac{\lambda}{2} \sum\_{j=1}^{M-1} \| \mathrm{w} \|^q$  
is equal to  
$\min\_{\mathrm{w}}\frac{1}{2}\sum\_{n=1}^N ( t\_n - \mathrm{w}^T \phi(\mathrm{x}) )^2$ s.t.
$ \sum\_{j=1}^M \|w_j\|^q \leq \eta $
<font size='5'>
※ $\eta$ is calculated from lagrange multiplier method
</font>

---

### image
<p><img src='./figures/progress_2_png/Figure3.4a.jpg' width='300px'></img>
<img src='./figures/progress_2_png/Figure3.4b.jpg' width='300px'></img></p>

---

### proof

$\min\_{\mathrm{w}}\frac{1}{2}\sum\_{n=1}^N ( t\_n - \mathrm{w}^T \phi(\mathrm{x}) )^2$ s.t.
$ \sum\_{j=1}^M \|w_j\|^q \leq \eta $

$L(\mathrm{w},\lambda) = \frac{1}{2}\sum\_{n=1}^N ( t\_n - \mathrm{w}^T \phi(\mathrm{x}) )^2 + \frac{\lambda}{2} \bigl\(\sum\_{j=1}^{M} \| w\_j \|^q - \eta \bigr\)$
$\frac{\partial L(\mathrm{w}, \lambda)}{\partial \lambda} = \sum\_{j=1}^{M} \| w\_j \|^q - \eta = 0$  
$\therefore \sum\_{j=1}^{M} \| w\_j \|^q = \eta $

---

### maximize likelihood
we think $t$ is represented by sum of $y(\mathrm{x},\mathrm{w})$ and Gaussian noise  
$t = y(\mathrm{x}, \mathrm{w}) + \varepsilon$

---
