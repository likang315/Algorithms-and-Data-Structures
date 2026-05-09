### Transformer

------

[TOC]

##### 01：概述

- 一种完全基于**自注意力机制（Self-Attention）**的深度学习模型架构，由 Google 团队在 2017 年的经典论文《Attention Is All You Need》中首次提出。

###### 痛点

- 在 Transformer 出现之前，处理自然语言的主流模型是循环神经网络（RNN）。RNN 的工作方式非常像人类读书：**必须从左到右，一个字一个字地按顺序处理**。
- Transformer的突破在于，它彻底抛弃了按顺序处理的 RNN 结构，**完全依靠自注意力机制来并行处理整个句子**。

##### 02：Transformer 架构

<img src="https://github.com/likang315/Algorithms-and-Data-Structures/blob/master/20%EF%BC%9AML/transformer.png?raw=true" alt="transformer" style="zoom:30%;" />

- Encoder 结构：由 N=6 个相同的 encoder block 堆叠而成，每一层（ layer）主要有两个子层（sub-layers）:
  - 多头注意力机制（`Multi-Head Attention`）
  - 简单的位置全连接前馈网络（`Positionwise Feed Forward`）
  - Add & Norm层：由 Add 和 Norm 两部分组成。
    -  Add：指 X + MultiHeadAttention(X)，是一种残差连接。
    - Norm：一种常用的神经网络归一化技术，可以使得模型训练更加稳定，收敛更快。
- Decoder 结构：由 N=6 个相同的 Decoder block 堆叠而成。
  - 有两个 Multi-Head Attention 层。
    - 第一个 Multi-Head Attention 层采用了 Masked 操作。
    - 第二个 Multi-Head Attention 层的 **K, V** 矩阵使用 Encoder 的**编码信息矩阵 C** 进行计算，而 **Q** 使用上一个 Decoder block 的输出计算。

##### 03：为什么要堆叠 6 层

- 原始 Transformer 论文（《Attention Is All You Need》）中设定的一个经验值，并没有绝对的数学定理证明“6”是完美的。

###### 黄金平衡点

- **层数太少（如 1-2 层）**：模型太浅，效果很差，抓不住复杂的语言规律。
- 层数太多（如 10 层以上）：虽然理论上能提取更深的特征，但会带来两个致命问题：
  - **训练极难**：层数越深，梯度消失的风险越大（幸好我们之前聊过“残差连接”来缓解这个问题）。
  - **算力爆炸**：训练和推理的时间、内存消耗会成倍增加。
