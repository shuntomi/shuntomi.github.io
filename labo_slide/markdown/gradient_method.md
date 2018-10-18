## Gradient method
###### 2018/07/04
###### Shun Tomizawa

---

### table of contents  
1. where it is used  
    1.1 what is regression  
    1.2 how to regression  
3. introduce some of gradient methods
4. comparison of methods (benchmark)

---

regression analysis    
<p><img src="./figures/progress_1_png/3-0.png" width="500px"></img></p>
Continue to fitting little by little

---

regression analysis    
<p><img src="./figures/progress_1_png/3-100.png" width="500px"></img></p>
Continue to fitting little by little

---

regression analysis    
<p><img src="./figures/progress_1_png/3-200.png" width="500px"></img></p>
Continue to fitting little by little

---

regression analysis    
<p><img src="./figures/progress_1_png/3-300.png" width="500px"></img></p>
Continue to fitting little by little

---

regression analysis    
<p><img src="./figures/progress_1_png/3-400.png" width="500px"></img></p>
Continue to fitting little by little

---

regression analysis    
<p><img src="./figures/progress_1_png/3-500.png" width="500px"></img></p>
Continue to fitting little by little

---

regression analysis    
<p><img src="./figures/progress_1_png/4-0.png" width="500px"></img></p>
Continue to fitting little by little

---

regression analysis    
<p><img src="./figures/progress_1_png/4-100.png" width="500px"></img></p>
Continue to fitting little by little

---

regression analysis    
<p><img src="./figures/progress_1_png/4-200.png" width="500px"></img></p>
Continue to fitting little by little

---

regression analysis    
<p><img src="./figures/progress_1_png/4-300.png" width="500px"></img></p>
Continue to fitting little by little

---

regression analysis    
<p><img src="./figures/progress_1_png/4-400.png" width="500px"></img></p>
Continue to fitting little by little

---

regression analysis    
<p><img src="./figures/progress_1_png/4-500.png" width="500px"></img></p>
Continue to fitting little by little

---

### how to regression
reducing the error
<p><img src="./figures/progress_1_png/error.png" width="500px"></img></p>

---

### The main idea of regression
Minimization of the error function  
$E = \frac{1}{2}\sum_{i=1}^{n} ( y_i - f(x_i))^2$  
it is called least squares method  
<p><img src="./figures/progress_1_png/error2.png" width="500px"></img></p>

---

### solve $f'(\mathbf{x}) = 0$ ??
it is difficult to solve this equation  
instead, we use optimization technique

---

### where it is used ?
optimizaiton technique is used in optimization problem
machine learning is a kind of optimization problem

---

### Kind of optimaization algorithm
* using gradient
  * steepest descent method
  * newtons method
  * conjugate gradient method
* not using gradient
  * genetic algorithm (GA)
  * simulated annealing (SA)
  * tabu search (TS)

<u>I will introduce optimaization algorithm using a gradient</u>

---

### there are algorithms which I intoroduce
<div align='center'>
<font size="5">
* steepest descent method<br>
* momentum method<br>
* nesterovs accelerated gradient method<br>
* newton-raphson method<br>
* conjugate gradient method<br>
* quasi newton method<br>
* AdaGrad<br>
* RMSprop<br>
* AdaDelta<br>
* Adam<br>
</font>
</div>

---

### Today's goal
* introduce each algorithm simply
* perform a benchmark test

---

### Configuration
we think about minimization of $f(\mathbf{x})$
we want to know $\mathbf{x}$ which takes minimum of $f(\mathbf{x})$
* $\mathbf{x} $ is n-dimensional vector
* $\nabla f$ is gradient of $f$
* $\mathbf{H}$ is Hessian matrix

---

### steepest descent method
Representative example of optimaization algorithm using a gradient
<p><img src="./figures/progress_1_png/sgd1.png" width="600px"></img></p>

---

### steepest descent method
<div style = "border:white solid 4px;padding: 10px;">
<div align="left">
<font size="5">
1. initialize $\mathbf{x}$    
2. update $\mathbf{x}$    
<div align="center">
<p><font color="yellow">$\mathbf{x}^{k+1} \leftarrow \mathbf{x}^k - \alpha \nabla f(\mathbf{x}^k)$</font></p>
</div>
3. back to step2  until $\| \mathbf{x}^{k+1} - \mathbf{x}^k\| < \varepsilon$  
4. return $\mathbf{x}$
</font>
</div>
</div>

<font size="5">
※ $\nabla f(\mathbf{x}) = \left( \frac{\partial f(\mathbf{x})}{\partial x_1},\frac{\partial f(\mathbf{x})}{\partial x_2},\dots,\frac{\partial f(\mathbf{x})}{\partial x_n} \right)$
</font>

---

### steepest descent method
<div style = "border:white solid 4px;padding: 10px;">
<div align="left">
<font size="5">
1. initialize $\mathbf{x}$    
2. update $\mathbf{x}$    
<div align="center">
<p><font color="yellow">$\mathbf{x}^{k+1} \leftarrow \mathbf{x}^k - \alpha \nabla f(\mathbf{x}^k)$</font></p>
</div>
3. back to step2  until$\| \mathbf{x}^{k+1} - \mathbf{x}^k\| < \varepsilon$  
4. return $\mathbf{x}$
</font>
</div>
</div>

<font size="5">
※ $\nabla f(\mathbf{x}) = \left( \frac{\partial f(\mathbf{x})}{\partial x_1},\frac{\partial f(\mathbf{x})}{\partial x_2},\dots,\frac{\partial f(\mathbf{x})}{\partial x_n} \right)$
</font>

later I will introduce only update expression

---

### steepest descent method
<div style = "border:white solid 4px;padding: 10px;">
<div align="center">
<font size="5">
$\mathbf{x}^{k+1} \leftarrow \mathbf{x}^k - \alpha \nabla f(\mathbf{x}^k)$
</font>
</div>
</div>
<p><img src="./figures/progress_1_png/sgd.png" width="600px"></img></p>
<font size="5">$\alpha$ is step size</font>

---

### feature

* implementation is easy
* easy to arrive at local optimal solution
  * explore from a lot of initial value
  * add randomness (Stochastic Gradient Descent)
* we need to  calculate $\nabla f$

---

### momentum method
<div style = "border:white solid 4px;padding: 20px;">
<div align = "center">
<font size="5">
1. $\mathbf{v}^{k+1} \leftarrow \beta \mathbf{v}^k - \alpha \nabla f(\mathbf{x}^k)$<br>
2. $\mathbf{x}^{k+1} \leftarrow \mathbf{x}^k + \mathbf{v}^{k+1}$
</font>
</div>
</div>
<p><img src="./figures/progress_1_png/mo-mentamu.png" width="600px"></img></p>

<font size="5">
use previous gradient
</font>

---

### nesterov's accelerated gradient method
<div style = "border:white solid 4px;padding: 20px;">
<div align = "center">
<font size="5">
1. $\mathbf{v}^{k+1} \leftarrow \beta \mathbf{v}^k - \alpha \nabla f(\mathbf{x}^k + \beta \mathbf{v}^k)$<br>
2. $\mathbf{x}^{k+1} \leftarrow \mathbf{x}^k + \mathbf{v}^{k+1}$
</font>
</div>
</div>
<p><img src="./figures/progress_1_png/kasoku.png" width="600px"></img></p>

<font size="5">
similar method of momentum method
</font>

---

### feature
* use the previous gradient
  * gradient direction is same -> big step
  * gradient direction is not same -> small step　

---

### newton method
<div style = "border:white solid 4px;padding: 20px;">
<div align = "center">
<font size="5">
$x^{k+1} \leftarrow x^k - \frac{f'(x^k)}{f''(x^k)}$
</font>
</div>
</div>
<p><img src="./figures/progress_1_png/nyu-ton.png" width="600px"></img></p>

<font size="5">
move to the extreme value of second-order approximate curve<br>
</font>

---

### newton-raphson method
<div style = "border:white solid 4px;padding: 20px;">
<div align = "center">
<font size="5">
$\mathbf{x}^{k+1} \leftarrow \mathbf{x}^k - \mathbf{H}^{-1} \nabla f(\mathbf{x}^k)$
</font>
</div>
</div>

<font size="5">
extend newton method to many variables<br>
</font>

---

### hessian matrix
<p><img src="./figures/progress_1_png/H.png" width="500px"></img></p>

---

### feature
* quadratic convergence (execution time is fast)
* require inverse of hessian matrix（computational cost is high）
  * preparing hessian matrix is difficult
  * need calcuration of inverse matrix（$\mathcal{O}(n^3)$）

---

### conjugate gradient method
<div style = "border:white solid 4px;padding: 20px;">
<div align = "center">
<font size="5">
1. $\beta^k \leftarrow -\frac{(\mathbf{m}^{k-1},\mathbf{H}^k \nabla f(\mathbf{x}^k))}{(\mathbf{m}^{k-1},\mathbf{H}^k \mathbf{m}^{k-1})}$<br>
2. $\mathbf{m}^k \leftarrow \nabla f(\mathbf{x}^k) + \beta^k \mathbf{m}^{k-1}$<br>
3. $\mathbf{x}^{k+1} \leftarrow \mathbf{x}^k + \alpha \mathbf{m}^k$<br>
</font>
</div>
</div>
<p><img src="./figures/progress_1_png/kyouyaku.png" width="500px"></img></p>

<font size="5">
$(\mathbf{a},\mathbf{b})$ is inner product of $\mathbf{a}$ and $\mathbf{b}$
</font>

---

### feature
* there is no need to calculate Hessian matrix
* execution time is fast

---

### quasi newton method
<p><img src="./figures/progress_1_png/quasi.png" width="500px"></img></p>
<font size="5">
$\mathbf{H}$ is approximate
</font>

---

### AdaGrad
<div style = "border:white solid 4px;padding: 20px;">
<div align = "center">
<font size="5">
1. $\mathbf{r}^{k+1} \leftarrow \mathbf{r}^k + \nabla f(\mathbf{x}^k)^2$<br>
2. $\mathbf{x}^{k+1} \leftarrow \mathbf{x}^k - \frac{\alpha}{\sqrt{\mathbf{r}^{k+1}}+\varepsilon}\nabla f(\mathbf{x}^k)$
</font>
</div>
</div>

---

### RMSprop
<div style = "border:white solid 4px;padding: 20px;">
<div align = "center">
<font size="5">
1. $\mathbf{r}^{k+1} \leftarrow \gamma \mathbf{r}^k + (1 - \gamma) \nabla f(\mathbf{x}^k)^2$<br>
2. $\mathbf{x}^{k+1} \leftarrow \mathbf{x}^k - \frac{\alpha}{\sqrt{\mathbf{r}^{k+1}}+\varepsilon}\nabla f(\mathbf{x}^k)$
</font>
</div>
</div>

---

### AdaDelta
<div style = "border:white solid 4px;padding: 20px;">
<div align = "center">
<font size="5">
1. $\mathbf{r}^{k+1} \leftarrow \gamma \mathbf{r}^k + (1 - \gamma) \nabla f(\mathbf{x}^k)^2$<br>
2. $\mathbf{v}^{k+1} \leftarrow \frac{\sqrt{s}^k+\varepsilon}{\sqrt{r}^{k+1}+\varepsilon} \nabla f(\mathbf{x}^k)$<br>
3. $\mathbf{s}^{k+1} \leftarrow \gamma \mathbf{s}^k + (1 - \gamma){\mathbf{v}^{k+1}}^2$<br>
4. $\mathbf{x}^{k+1} \leftarrow \mathbf{x}^k - \mathbf{v}^{k+1}$
</font>
</div>
</div>

---

### Adam
<div style = "border:white solid 4px;padding: 20px;">
<div align = "center">
<font size="5">
1. $\mathbf{v}^{k+1} \leftarrow \beta \mathbf{v}^k + (1-\beta)\nabla f(\mathbf{x}^k)$<br>
2. $\mathbf{r}^{k+1} \leftarrow \gamma \mathbf{r}^{k} + (1 - \gamma){f(\mathbf{x}^k)}^2$<br>
3. $\mathbf{x}^{k+1} \leftarrow \mathbf{x}^k - \frac{\alpha}{\sqrt{\frac{\mathbf{r}}{1-\gamma^t}}+\varepsilon} \frac{\mathbf{v}}{1-\beta^t}$
</font>
</div>
</div>

---

### benchmark test1 by MNIST
* compared method
  * steepest descent method<br>
  * momentum method<br>
  * nesterovs accelerated gradient method<br>
  * AdaGrad<br>
  * RMSProp<br>
  * AdaDelta<br>
  * Adam<br>
<font size='5'>
※  we can not use method using Hessian matrix
</font>

---

### MNIST
<p><img src="./figures/progress_1_png/processing.png" width="500px"></img></p>
we can download data from <a href="http://yann.lecun.com/exdb/mnist/")target="_blank">here</a>

---

### neural network
I used neural network model in this benchmark test
<p><img src="./figures/progress_1_png/NN.002.png" width="500px"></img></p>

---

### The structure of NN
* input layer : 784 neurons(28*28 dimension)
* hidden layer : 100 neurons
* output layer : 10 neurons (10 class)
* activating function of hidden layer : sigmoid function
* activating function of output layer : softmax function
* optimization method : each method

---

### Parameter
I set parameters in each method by reference to <a href="http://docs.chainer.org/en/stable/reference/optimizers.html")target="_blank">this website</a>
I select default parameters of each method

---

### result1
<p><img src="./figures/progress_1_png/benchmark.png" width="600px"></img></p>

---

### result1
* AdaDelta, Adam, RMSProp showed good result
    * Note that I do not parameter tuning
    * in many cases, adam seems the best result

---

### is Adam always best approach??
* optimal method must be selected depending on the problem
    * method using hessian matrix is very fast
    * it is better to chose, if you can choose

---

### benchmark test2 by regression
* compared method
    * steepest descent method
    * Adam
    * quasi newton method

---

### The structure of NN
* input layer : 1 neurons
* hidden layer : 3 neurons
* output layer : 1 neurons
* activating function of hidden layer : sigmoid function
* activating function of output layer : identity function
* optimization method : each method

---

### result2-1
<p><img src="./figures/progress_1_png/fig-quadratic.png" width="600px"></img></p>

---

### result2-2
<p><img src="./figures/progress_1_png/fig-sin.png" width="600px"></img></p>

---

### result2-3
<p><img src="./figures/progress_1_png/fig-abs.png" width="600px"></img></p>

---

### result2-4
<p><img src="./figures/progress_1_png/fig-heaviside.png" width="600px"></img></p>

---

### result2
* quasi newton method showed best result
* it is better to chose, if you can choose

---

## conclusion
we should choise method depending on the problem

---

thank you
