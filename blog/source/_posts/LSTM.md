---
layout: post
title: LSTM
tags: 人工智能
date: 2018-11-13 12:03:12 #创建时间
---

$\sum_i(n+k_i)$
# LSTM的理解  
## 概述  
LSTM（`Long Short-Term Memory`）是长短期记忆网络，是一种特殊类型的RNN(`Recurrent Neural Network`)  
下图是RNN的简单示意:  
![01][01]  
与RNN不同的是，LSTM在时间流向上有2个输入和输出，其重复的模块也并非一个单一神经网络层(tanh 层),而是由四个神经网络层以一种非常特殊的方式进行交互，如下图示意:     
![02][02] 
LSTM 有通过精心设计的“门”结构(`gate`)来去除或者增加信息到cell状态的能力。门是一种让信息选择式通过的方法,它们包含一个 sigmoid 神经网络层和一个 elementwise product 操作。Sigmoid 层输出 0 到 1 之间的数值，描述每个部分有多少量可以通过。0 代表“不许任何量通过”，1 就指“允许任意量通过”！ 

接下来梳理一下这些结构(`主要是三个门结构`)和公式整理

## 整体公式一览
$$\begin{array}{lrr}
f_t=\sigma(W_f\cdot[h_{t-1},x_t]+b_f)&(1)\\
i_t=\sigma(W_i\cdot[h_{t-1},x_t]+b_i)&(2)\\
\tilde{C}_t=\tanh(W_C\cdot[h_{t-1},x_t]+b_C)&(3)\\
C_t=f_t \odot C_{t-1} + i_t \odot \tilde{C}_t &(4)\\
o_t=\sigma(W_o\cdot[h_{t-1},x_t]+b_o)&(5)\\
h_t=o_t \odot \tanh(c_t)&(6)
\end{array}$$

## 遗忘门  
![03][03]  
对应公式（1）：$$f_t=\sigma(W_f\cdot[h_{t-1},x_t]+b_f)$$  
forget gate 会读取 $$h_{t-1}$$ 和 $$x_t$$,输出一个在 0 到 1 之间的数值给每个在cell状态$$C_{t-1}$$中的数字。1 表示“完全保留”，0 表示“完全舍弃”。  
这个门决定丢弃信息。
## 输入门   
![04][04]  
对应公式（2）和（3）：  
$$i_t=\sigma(W_i\cdot[h_{t-1},x_t]+b_i)$$  
$$\tilde{C}_t=\tanh(W_C\cdot[h_{t-1},x_t]+b_C)$$  
input gate 中，sigmoid层(输出为0到1的数值)将决定那些值将被更新，之后，一个 tanh 层创建一个新的候选值向量，$$\tilde{C}_t$$将会被加到状态中  
新的状态由遗忘信息和需要更新的信息叠加而成，对应公式（4）：  
$$C_t=f_t \odot C_{t-1} + i_t \odot \tilde{C}_t$$  
![06][06]  
此时$$C_{t-1}$$更新为$$C_t$$  
输入门对cell的状态进行更新
## 输出门
![05][05]  
对应公式（5）：  
$$o_t=\sigma(W_o\cdot[h_{t-1},x_t]+b_o)$$  
这里运行一个 sigmoid 层来确定当前输入的哪些信息将被输出，与当前cell状态的tanh结果做elementwise product后将得到最终需要输出的部分，即公式（6）：  
$$h_t=o_t \odot \tanh(c_t)$$  
输出门可以输出信息，具有学习词形的能力  
## 总结
LSTM在算法中加入了一个判断信息有用与否的“处理器”，在反向传播过程中由于公式（4），也解决了传统RNN算法的梯度消失的问题（因为RNN在时间维度不断的对同一个权重矩阵进行更新且为乘积形式更新，而公式（4）为加法） 

关于LSTM也可以通过下图理解  
![08][08]  

$$\begin{pmatrix}
i\\
f\\
o\\
g
\end{pmatrix}=\begin{pmatrix}
\sigma\\
\sigma\\
\sigma\\
\tanh
\end{pmatrix}W\begin{pmatrix}
h_{t-1}\\
x_t
\end{pmatrix}
$$

$$C_t = f\odot C_{t-1}+i\odot g$$

$$h_t = o\odot \tanh(C_t)$$

LSTM还有一些变体，后面有时间深入研究的话再行整理

[01]: https://raw.github.com/ice-melt/picture-set/master/blog/LSTM/lstm-rnn.png
[02]: https://raw.github.com/ice-melt/picture-set/master/blog/LSTM/lstm-lstm.png
[03]: https://raw.github.com/ice-melt/picture-set/master/blog/LSTM/lstm-ft.png
[04]: https://raw.github.com/ice-melt/picture-set/master/blog/LSTM/lstm-it.png
[05]: https://raw.github.com/ice-melt/picture-set/master/blog/LSTM/lstm-ot.png
[06]: https://raw.github.com/ice-melt/picture-set/master/blog/LSTM/lstm-ct.png
[07]: https://raw.github.com/ice-melt/picture-set/master/blog/LSTM/lstm-c.png
[08]: https://raw.github.com/ice-melt/picture-set/master/blog/LSTM/lstm-type2.png