---
layout : post
title : Online Covariance Estimation - paper - 4
subtitle : paper reading
date : 2021-10-26
#categories:
tags : [estimation, statistical computing, machine_learning]
toc_sticky : true
use_math : true
comments: true
---


# 4. Simulation Studies

<br>

**evaluate the empirical performance of the proposed online approach**  

two class of example : linear regression & logistic regression  

##### setting

data( *iid* sequence of pair ) : $ \{ \xi \equiv (a_i, b_i)\} $ & $a_i \sim N(0, \mathbf{I}_d)$  
$x^*$ : true parameter  
$\eta_j = 0.5 j^{-\alpha}$ : step size  
$\alpha = 0.505$ (chosen)  

$a_m = [Cm^{2/(1-\alpha)}]$ : seqence ==> 이것을 가지고 block B = $ \{B_i : a_m \leq i < a_{m+1} \} $ 를 만들어 낸다.

$+$  

200번 run 이후 average = ASGD  


<br>


### 4.1 Empirical performance of the porposed online approach

linear regression 가정 : $b_i = a_i^Tx^* + \epsilon_i $  

$f(x^*)$는 negative log-likelihood function


#### 4.1.1 Convergence of the recursive estimator

limmiting covariance matrix

$$
\Sigma = A^{-1}SA^{-1} = \mathbf{I}_d
$$

where

$$
A = E[\nabla^2f(x^*)] = E(aa^T) = \mathbf{I}_d  \\  

S = E([\nabla f(x^*, \xi)][\nabla f(x^*, \xi)]^T)=E(\epsilon^2)E(aa^T) = \mathbf I_d

$$

이므로, online estimator의 convergence를 확인하기 위해서는 분산행렬의 operator norm estimate에 대한 Loss 값의 변화를 계산해본다.

Figure 1 은 이를 나타낸 것이다.  
y축은 $log loss = || \hat \Sigma_n - \Sigma ||_2$  
x축은 log of total number of steps = $log \eta_i$ 를 의미한다


<img src='{{"/assets/img/paper_figure1.png"| relative_url}}'  width="70%" height="70%" title="1" alt='relative'>

<br>

<br>




(limiting covariance 가 위와 같이 나오는 것은 $\dot l(\theta) = \dot l(\hat \theta) + \ddot  l(\hat \theta)(\theta - \hat \theta) $ 식을 통해 limiting covariance를 찾는 과정에 기인.)




아래의 Figure 2 는 relative efficiency(MSE of full overlapping / MSE of non-overlapping)를 나타낸 것이다. 그림에서도 알 수 있듯 C 값은 performance와 그리 큰 관련이 있어보이진 않는다. 

<img src='{{"/assets/img/paper_figure2.png"| relative_url}}'  width="70%" height="70%" title="1" alt='relative'>

<br>

<br>


#### 4.1.2 Asymtotic normality and CI coverage

공분산을 추정했으니 이를 활용해서 신뢰구간을 구할 수 있다.

$$
[1^T \bar x_n - z_{1-q/2}\sqrt{1^T \hat \Sigma_n 1 /n} , 1^T \bar x_n + z_{1-q/2}\sqrt{1^T \hat \Sigma_n 1 /n} ]
$$


true limiting covariance matrix 를 활용해서 CI를 구할 수 있는데 아래 그림인 figure3 에서 보듯, empirical coverage rate이 95%로 수렴하고, SE와 CI length 또한 참값과 유사하다.

<img src='{{"/assets/img/paper_figure3.png"| relative_url}}'  width="70%" height="70%" title="1" alt='relative'>