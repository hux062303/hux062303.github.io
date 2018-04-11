---
layout:     post
title:      nonlinear least squares problem 总结
subtitle:   
date:       2018-04-11
address:    DTU, Lyngby
author:     hux0620303
header-img: img/statistics.jpg
catalog: true
tags:
    - least squares
    - levenberg-marquardt
---

##Least-Square optimization
待求问题：
$$ F({\bf{x}}) = \frac{1}{2} \sum^{m}_{i=1}{f_i({\bf{x}})^2} $$
其中，$$f_i(\bf{x})$$是一个$$R^n$$到$$R^1$$的映射函数，其实就是在LS问题中需要构建的error函数。m一般大于n，也就是超定方程。
举个例子，一个参数识别问题：
$$ M({\bf{x}},t)=x_3e^{x_1t}+x_4e^{x_2t}$$
待求参数有四个$${\bf{x}}={x_1,x_2,x_3,x_4}$$, 观测数据为$$y_i=M({\bf x},t)+\varepsilon_i$$, 其中$$ \varepsilon_i $$是量测的噪声，这里显式的$$ \varepsilon_i $$写出来，只是代表量测中存在误差。如果想仿真伪造数据的话，可以在truth上加上`randn(1)*sigma`,其中`sigma`为Gaussian噪声的标准差。对这个系统观测了$$m$$次，如何从这$$m$$个观测中估计$$\bf x$$，就是LS需要解决的问题。对于线性系统这个问题很简单，把问题写成$${\bf Ax=b}, {\text{then if} {\bf b \neq 0 },}\ {\bf x=(A^TA)^{-1}A^Tb}\ \text{else take the SVD} $$就是要求的结果。
待求问题一般也可以写成：
$$ F({\bf{x}}) = \frac{1}{2} \sum^{m}_{i=1}{f_i({\bf{x}})^2}=\frac{1}{2}f({\bf x})^Tf({\bf x}) $$，其中$$f({\bf x})$$是从$$R^n$$到$$R^p$$的函数。

### 全局最优/局部最优
求非线性最小二乘的全局最优解释很复杂的，后面要介绍的算法都是局部最优算法，局部最优通俗上说就是在最优解$$x^*$$附件的领域上，$$F({\bf x^*})$$是最小的。
### Gradient/Jacobian/Hessian
$$g = F^{'}({\bf x}),\ J({\bf x})_{ij}=\frac{\partial{f_i}}{\partial{x_j}}({\bf x}),\ {\bf H}=F^{''}({\bf x})$$. 为啥需要这三个矩阵，下面说明。另外，注意一般情况下，$$J^TJ \neq \bf H $$.
### stationary point
$$J({\bf x})=0$$
局部最小点一定是stationary point, 但是stationary point也可以是极大值点或者鞍点，怎么判断？用Hessian矩阵，positive definitive=局部极小，negative definitive=局部极大，indefinitive=鞍点。

### 算法的收敛速度
1. 线性，$$||e_{k+1}|| / ||e_{k}||=\alpha,\ \ 0<\alpha<1$$;
2. 二次，$$||e_{k+1}||=O(||e_{k}||^2)$$

##算法
所有的non-linear optimization算法都是迭代的，迭代的原则是$$F({\bf x_{k+1}})<F({\bf x_k})$$.

通用的Descent Method，包含关键的三步：
1. 选择descent direction $$\bf h_d$$;
2. 选择step length, $$\alpha$$;
3. $${\bf x_{k+1}}={\bf x_{k}}+\alpha {\bf {h_d}}$$
然后进行迭代，直至收敛或者最大迭代步数到达。

**那么什么是descent direction?**
$${\bf h_d}^TF^{'}({\bf x})<0$$，至于为什么，对$$F({\bf x})$$进行一阶Taylor展开就知道了。

**怎么选择$$\alpha$$?**
也就是某些文章中会写的line search步骤。理论上，最好的$$\alpha_e=argmin_{\alpha>0}{F({\bf x}+\alpha{\bf h})}$$, 要实现这一目标，也是需要迭代的。

### Steepest Descent Method
也叫做gradient method, 顾名思义，就是把第一步中的$$\bf h_{sd}=-F'(x)$$, 因为沿着梯度方向是最速的上升或者下降的方向。此方法在快到local minimum的时候收敛速度是linear的，very slow，但是在initial阶段，速度还是很快的。

### Newton Method
$$\bf Hh_n=-F'(x)$$, Newton法在最终收敛段速度是quadratic的，代价是需要求Hessian. 
**Quasi-Newton和Newton的关系**
Newton用的是真的Hessian，Quasi-Newton，顾名思义，用的是近似的Hessian？为啥要用近似的Hessian呢，因为Hessian对于很多系统来说，真的很难求。

### Soft line search
soft line search，顾名思义，就是不要求$$\alpha$$是最优的。soft line search接受一个$$\alpha$$，只要它满足：
1. 沿着当前梯度下降了一定程度；
2. target点梯度大于当前梯度的$$\gamma$$倍，$$0<\gamma<1$$;
第一点保证了，步不会选的太小，第二点保证了，步不会选到上升方向。

### Trust Region V.S Damped Method
Assume:
$$F({\bf x+h}) \approx L({\bf h})+{\bf h^Tc}+\frac{1}{2}{\bf h^TBh}$$, 其中$$L({\bf h})$$是$$F$$在$$\bf x$$附近的二阶Taylor展开。Truth region的意思是，存在一个ball，radius是$$\triangle$$, 选择$$h_{tr}=argmin_{||h|| \leq \triangle}{L({\bf h})}$$.而damped mathod，选择$$h_{dm}=argmin_{h}{L({\bf h}+\frac{1}{2}\mu {\bf h^Th})}$$.这两种方法默认$$\alpha=1$$，但是要求在基本descent算法的经典三步上，增加一步：
4. 更新$$\triangle$$或者$$\mu$$;
更新的原则取决于当前的估计评价值。直观上讲，对于TR算法，如果评价高，代表可以增大region的radius，加快收敛；反之减小。对于DM算法，如果评价好，就减小阻尼，反之就增大阻尼。

### Newton-Raphson's Method
solve $$J({\bf x_k}){\bf h}_{nr}=-f({\bf x}_k), {\bf x}_{k+1}={\bf x}_k+{\bf h}_{nr}$$

### Gauss-Newton Method
solve $$(J^TJ){\bf h}_{gn}=-J^Tf, {\bf x}_{k+1}={\bf x}_k+{\bf h}_{gn}$$
注意和Newton法的对比，GN法一般没有quadratic收敛速度。

### Levenberg-Marquardt
LM是一种DM算法。
solve $$(J^TJ+\frac{1}{2}\mu I){\bf h}_{lm}=-J^Tf, {\bf x}_{k+1}={\bf x}_k+{\bf h}_{lm}$$.退出机制：
1. $$J^Tf=0$$
2. $$||h_{lm}||$$很小了；
3. 达到最大次数；


### Dogleg 
DL是一种TR方法。依赖于GN法和SD法，根据步长是否超过了$$\triangle$$，决定是采用GN,或者SD，或者二者的加权。理论上说，DL要比LM好，但是对于一般问题，无法确定DL和LM谁的收敛速度更快。



## Toolbox for Non-linear Optimization

NLOP的基本问题就是上面所描述的，然后理论和实际又是另一回事，比如说如果待解问题维度很高，Jacobian会变得很大，如果还是dense的，内存和求解速度都会遇到很多挑战；反之，比如BA问题，待解参数虽然很多，但是是sparse的问题，如果直接按照上面的解法去处理，速度会很慢。下面是一些我个人调研过的能过求解通用NLOP问题的工具：

1. google ceres: 大全，可以解带约束的，可以解不带约束的；
2. Eigen unsupported:只能求解不带约束的问题，所有矩阵都是dense的，因此问题很大的时候，要考虑一下内存够不够；

其他诸如sparseLM, cminpack这些，我没有使用过，不过也至少是过去(ceres之前)的主流求解器。



## Reference
1. Methods for non-linear least squares problems, 2nd Edition, 2004. K.Madsen, H.B.Nielsen, O.Tingleff, Informatincs and Mathematical Modeling, Techinical University of Denmark.








