---
layout: post
title: RoPE vs CoPE - Part 1
subtitle: 
tags: [coreml, ml, deep learning]
---

The way the attention mechanism works, we need to provide positional information to the input tokens. There are quite a few ways to give such information to the model, and this series we will explore Rotary embeddings and Contextual embeddings for the positional information.


## Rotary Embeddings

Rotary embeddings was introduced in [RoFormer paper](https://arxiv.org/pdf/2104.09864), and it has had quite a few success, esp in Llama 3 models. 

Consider a set of word tokens $ W = \{w_i\}_{i=1}^N $ and $ X = \{x_i\}_{i=1}^N $ be its corresponding word embeddings. $ x_i $ is a $ d $ dimensional vector. Let's define a function $ f $ which adds position information to the word embeddings in some manner. In the attention module, query and key vectors with its positional information can be defined as 

$$ q_m=f_q(x_q,m) \\ k_n=f_k(x_k,n) \tag{1}$$

To capture the context between query and key, we take the dot product between them. $$qk^T=<f_q(x_q,m), f_k(x_k,n)>$$ Let's a function $g$ which capture the relative position between them we take the dot product. $$qk^T=g(x_q,x_k,n-m) \tag{2}$$ Also, at position 0, we should be able to recover the original query and key embeddings. $$q = f_q(x_q,0) \\ k = f_k(x_k,0) \tag{3}$$

For simplicity, let's consider $d=2$ and represent $f_q$, $f_k$ and $g$ in polar form as 
$$\begin{align*} f_q(x_q,m) &= R_q(x_q,m)e^{i\theta _q(x_q,m)} \\ f_k(x_k,n) &= R_k(x_k,n)e^{i\theta _k(x_k,n)} \tag{4} \\ g(x_q,x_k,n-m) &= R_g(x_q,x_k,n-m)e^{i\theta _g(x_q,x_k,n-m)} \end{align*}$$

Let's combine above equations, $$qk^T = R_q(x_q,m)e^{i\theta _q(x_q,m)} \cdot R_k(x_k,n)e^{i\theta _k(x_k,n)} = R_q(x_q,m)R_k(x_k,n)e^{i(\theta _q(x_q,m) - \theta _k(x_k, n))} \tag{5}$$

Thus, $$\begin{align*} R_q(x_q,m)R_k(x_k,n) = R_g(x_q,x_k,n-m) \\ \theta _q(x_q,m) - \theta _k(x_k, n) = \theta _g(x_q,x_k,n-m) \tag{6} \end{align*}$$

Now, let's look at query and vector at position 0. 
$$ q = f_q(x_q,0) = R_q(x_q, 0)e^{i\theta _q(x_q,0)} = ||q||e^{i\theta _q} \\ k = f_k(x_k,0) = R_k(x_k, 0)e^{i\theta _k(x_k,0)} = ||k||e^{i\theta _k} \tag{7}$$

We get $ R_g(x_q, x_k, 0) $ and $ \theta _g(x_q, x_k, 0) $ when $m = n$ as well as $ m = n = 0$. Thus, 

$$ R_q(x_q, m)R_k(x_k, m) = R_g(x_q, x_k, 0) = R_q(x_q, 0)R_k(x_k, 0) = ||q|| \cdot ||k|| \tag{8}$$ 
$$\theta _q(x_q,m) - \theta _k(x_k, m) = \theta _g(x_q, x_k, 0) = \theta _q(x_q,0) - \theta _k(x_k, 0) = \theta _k - \theta _q \tag{9}$$


Since, eqn(8) doesn't depend on any position information, we can set $ R_f(x, m) = \lVert x \lVert $.

We are re-write eqn(9), as $\theta _q(x_q,m) - \theta _q = \theta _k(x_k, m) - \theta _k$. And this doesn't depend on magnitude of query or key vector, but depends on the position $m$. Let's denote  $\theta _q(x_q,m) - \theta _q$ as $\phi (m)$. For the case $n = m + 1$, eqn(6) $$\theta _k(x_k, m + 1) - \theta _q(x_q, m) = \theta _g(x_k, x_q, 1) \\ \phi (m + 1) + \theta _k - \phi(m) - \theta _q = \theta _g(x_k, x_q, 1) \\ \phi (m + 1) - \phi(m) = \theta _g(x_k, x_q, 1) + \theta _q - \theta _k \tag{10}$$

RHS of eqn(10) doesn't depend on $m$, thus it can be formulated as arithmetic progression. $$\phi(m) = m\theta + \gamma$$

Thus, $$f(x_{\{q,k\}},m) = ||x_{\{q,k\}}||\cdot e^{im\theta + \gamma} $$ Setting $\gamma = 0$, we get $$f(x_{\{q,k\}}, m) = (W_{\{q,k\}}x_m)e^{im\theta}$$
Writing it is matrix form,
$$f(x_{\{q,k\}}, m) = \begin{pmatrix}cos\ m\theta & -sin\ m\theta \\ sin\ m\theta & cos\ m\theta\end{pmatrix} \begin{pmatrix} W^{(11)} & W^{(12)} \\ W^{(21)} & W^{(22)}\end{pmatrix} \begin{pmatrix}x_m^{(1)} \\ x_m^{(2)}\end{pmatrix}$$

Extending this to $d$ dimension, $$f(x_{\{q,k\}}, m) = R_{\theta,m}^d W x_m$$ where $$R_{\theta,m}^d = \begin{pmatrix}R_{\theta _1}^` & 0 & \cdots & 0 \\ 0 & R_{\theta _2}^` & \cdots & 0 \\ \vdots & \vdots & \ddots & 0 \\ 0 & 0 & \cdots & R_{\theta _{d/2}}^`\end{pmatrix} ;\ R_{\theta _x}^` = \begin{pmatrix} cos\ m\theta _x & -sin\ m\theta _x \\ sin \ m\theta_x & cos\ m\theta_x\end{pmatrix}$$

The dot product between query and key, $$ q^Tk = x_m^TW_q R_{\theta,n-m}^dW_kx_n$$ We can pre-define theta as $$\theta_i = 10000^{-2(i - 1)/d}\ \ ,i \in [1, \cdots, d/2]$$

With rotation matrix, we should be able to capture relative position between the query and key vectors in the attention mechanism.