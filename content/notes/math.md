+++
title = 'Math'
date = 2024-01-20T12:46:39-05:00
draft = true
+++

```katex
$$ \begin{array} {lcl} L(p,w_i) &=& \dfrac{1}{N}\Sigma_{i=1}^N(\underbrace{f_r(x_2\rightarrow x_1 \rightarrow x_0)G(x_1\longleftrightarrow x_2)f_r(x_3\rightarrow x_2 \rightarrow x_1)}_{sample\, radiance\, evaluation\, in\, stage2} \\\\\\ &=& \prod_{i=3}^{k-1}(\underbrace{\dfrac{f_r(x_{i+1}\rightarrow x_i\rightarrow x_{i-1})G(x_i\longleftrightarrow x_{i-1})}{p_a(x_{i-1})}}_{stored\,in\,vertex\, during\, light\, path\, tracing\, in\, stage1})\dfrac{G(x_k\longleftrightarrow x_{k-1})L_e(x_k\rightarrow x_{k-1})}{p_a(x_{k-1})p_a(x_k)}) \end{array} $$
```
