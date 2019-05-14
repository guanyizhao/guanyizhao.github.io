---
layout: default
title:  时间序列的平稳性检验
---


* content
{:toc}


[回到上一级](../)

*****

# 时间序列的平稳性检验

*****

## 一、单位根过程

### 1.1 定义

[平稳过程](https://zh.wikipedia.org/wiki/平稳过程)分为严（强）平稳过程和宽（弱）平稳过程。一般意义上更常用的是宽平稳过程。

宽平稳过程指的是序列的一阶和二阶矩不随时间变化，即

$$ E(x_t) = \mu $$

$$ E(x_{t_1}, x_{t_2}) = R_x(t_1, t_2) = R_x(\tau, 0), \tau = t_2 - t_1 $$

[单位根过程](https://wenku.baidu.com/view/584fa090ad51f01dc281f176.html)指的是对某一随机过程$y_t$，若

$$ y_t = \rho y_{t-1} + u_t $$

其中，$\rho = 1$，且${u_t}$为一平稳过程，满足$E(u_t)=0$，$cov(u_t, u_{t-s})=r_s < \infty$，则称$y_t$为单位根过程。

### 1.2 单位根过程与平稳过程的关系

单位根过程是非平稳过程。

考虑上述单位根过程，即$\rho = 1$且${u_t}$平稳：

$$ y_t = y_{t-1} + u_t $$

则有

$$ y_t = y_0 + \sum_{i=1}^{t} u_i $$

故此，$y_t$的期望为：

$$ E(y_t) = y_0 + t E(u_i) = y_0 $$

方差为：

$$ D(y_t) = D(\sum_{i=1}^{t} u_i) = E[\sum_{i=1}^{t} u_i - E(\sum_{i=1}^{t} u_i)]^2 = E(\sum_{i=1}^{t} u_i)^2 \\
= t r_0 + 2(t-1)r_1 + 2(t-2)r_2 + \cdots + 2r_{t-1} $$

同理，协方差为：

$$ cov(y_t, y_{t-s}) = E[y_t - E(y_t)][y_{t-s} - E(y_{t-s})] = E(\sum_{i=1}^{t} u_t)(\sum_{i=1}^{t-s} u_{t-s}) \\
= (t-s)r_0 + [2(t-s)-1]r_1 + [2(t-s)-2]r_2 + \cdots + 2r_{t-s-1} + r_{t-s} + \cdots + r_{t-1} $$

可见$y_t$的协方差不仅与$s$有关，也与$t$自身有关，因此$y_t$不是平稳过程。

（上述公式为笔者自推，若有谬误，恳请原谅）

*****

## 二、单位根过程的假设检验

既然单位根过程是非平稳过程，我们只需检验检验时间序列是单位根过程，即可排除该序列是平稳序列的可能，反之则该序列有可能是平稳序列。

### 2.1 迪基-福勒检验法
检验序列是否存在单位根的检验被称为[单位根检验](https://baike.baidu.com/item/单位根检验/5574482?fr=aladdin)，通常使用[迪基-福勒检验法(Dickey-Fuller test)](https://en.wikipedia.org/wiki/Dickey–Fuller_test)或[扩展的迪基-福勒检验法(Augmented Dickey-Fuller test)](https://en.wikipedia.org/wiki/Augmented_Dickey–Fuller_test)进行检验（Matlab对应的函数为[adftest](https://www.mathworks.com/help/econ/adftest.html))。扩展的迪基-福勒检验法即将后面所讲的差分后所得的自回归模型AR(1)扩展到AR(p)，此处仅讨论基本的迪基-福勒检验法。

DF检验一般考虑差分信号，即对$y_t = \rho y_{t-1} + u_t$差分，得：

$$ y_t - y_{t - 1} = \Delta y_t = (\rho - 1) y_{t-1} + u_t = \delta y_{t-1} + u_t $$

上式为最一般的检测模型，若考虑到偏移项和时间趋势项则上述式子需要进行相应的更改，一般考虑以下三种模型：

- 初始模型（在Matlab-adftest中参数`'model'`取`‘AR'`，为默认情况），对应零均值平稳序列

$$ \Delta y_t = \delta y_{t-1} + u_t $$

- 含偏移项（在Matlab-adftest中参数`'model'`取`‘ARD'`），对应任意均值平稳序列

$$ \Delta y_t = a_0 + \delta y_{t-1} + u_t $$

- 含偏移项和时间趋势项（在Matlab-adftest中参数`'model'`取`‘TS'`），对应趋势平稳序列

$$ \Delta y_t = a_0 + a_1 t + \delta y_{t-1} + u_t $$

考虑上述数学模型，若$\delta = 0$则可得$\rho = 1$，即序列中存在单位根。因此，取原假设为存在单位根，即$\delta = 1$；相应的，备择假设为不存在单位根。

具体的检验统计量由t-统计量和F-统计量执行，在Matlab-adftest中可以通过设置`'test'`对应的参数来选择具体的检验统计量，备选值有`'t1'`、`'t2'`、`'F'`，注意，当模型为`'AR'`时，无法选择F-统计量`'F'`，F-检验除了判断$\delta = 0$是否成立外还检验$a_0=0$和$a_1=0$的取值。

在Matlab中，若adftest返回`h = 1`则表示该检验拒绝了原假设，接受备择假设，也就是说所检测的序列不含单位根，那么该序列平稳且将向均值回归；相应的，若adftest返回`h = 0`则表示该检验无法拒绝原假设，也就是说所检测的序列含有单位根，不是平稳序列。

*****

## References

[单位根检验-百度文库](https://wenku.baidu.com/view/584fa090ad51f01dc281f176.html)

[迪基-福勒检验法-wikipedia](https://en.wikipedia.org/wiki/Dickey–Fuller_test)

[扩展的迪基-福勒检测法-wikipedia](https://en.wikipedia.org/wiki/Augmented_Dickey–Fuller_test)

[扩展的迪基-福勒检测法-Matlab帮助文档](https://www.mathworks.com/help/econ/adftest.html)

*****

[回到上一级](../)

{% include header.html %}