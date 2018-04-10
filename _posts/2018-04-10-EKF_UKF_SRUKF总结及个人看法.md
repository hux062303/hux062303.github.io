---
layout:     post
title:      EKF\&UKF\&SRUKF总结及个人看法
subtitle:   
date:       2018-04-10
address:    DTU, Lyngby
author:     hux0620303
header-img: img/statistics.jpg
catalog: true
tags:
    - filter
---


## EKF\&UKF\&SRUKF总结及个人看法

主要routine:

1. predict; 
2. update;

针对非线性系统存在的问题：输入随机变量是Gaussian，进过非线性变换之后的分布不再是Gaussian。

解决方案的不同产生了不同的filter:

1. based on first-order Taylor series extension: 取非线性系统泰勒展开的第一项，也就是Jacobian, 考虑系统在展开点附近是线性系统。解决方案：求Jacobian，做1阶近似，但是近似的效果取决于展开点附近的curvature，curvature越大越差。另外，某些函数的Jacobian很难求~
2. based on so-called unscented transform: 基本思路是，近似一个非线性函数是很难得，但是近似一个Gaussian的分布比较容易。从输入分布上进行1-sigma采样，对每个样本点进行非线性变换之后，再加全求取平均和方差。UT变换可以使得随机变量的mean达到展开点附近的2阶近似，但方差水平和EKF是一样的。这里特别要注意实现方式。Matlab的control toolbox不做任何状态提升，那么在做prediction和update的时候要分别加上过程噪声Q和量测噪声R。Julier 2000的原文做了过程噪声的状态提升，那么predict的时候Q就不需要显式出现了。Eric的一些文章里面同时做了量测和过程噪声的状态提升，那么Q,R都不需要显式出现。但是在f和h的时候需要把对应的噪声放进去，**这里看文献的时候要特别注意，状态是提升了，但是prediction, update的对象不是所有提升的状态，而仅仅是x**。UKF不需要求取Jacobian，代价是增加了随机变量的个数。**如果仔细观察对covariance的Cholesky分解，类似于根据1-sigma standard deviation做扰动，mean和P的求取其实是基于1-sigma扰动的数值解。如果EKF想达到同样的精度，需要同时求取Jacobian和Hessian。** 基于以上的思考，我个人理解UKF，或者是是UT变换，其实是数值求解mean和covariance的一种方式，那么一个很自然的思考就是，EKF的Jacobian和Hessian也可以通过数值解法给出，以达到同样的精度。**另外一个值得注意的点是，UKF参数设计的不合理，很容易导致P非正半定。**

EKF-UKF复杂度基本上在一个level。

Eric Wan在UKF的基础上，进一步提出了Square-root ukf，直接在P的square root上操作，避免了每一轮都做Cholesky分解。SRUKF的关键三步：qr取上三角部分，cholupdate做cholesky的rank1更新，其实就是更新P的square root。K的求解用到least-square。

这里也要注意是否做了状态提升。

some comments:

* Eric Wan文章非常不厚道，几篇文章里面，状态提升，不提升，qr是左右与A还是Atranspose，取得是上三角部分还是下三角部分变来变去，可能是写文章的套路？？？看的时候一定一定要仔细。
* Julier Simon的UKF文章理论方面更加完备。
* Eric Wan的CDKF部分是有错误的。
