### 向量

------

[TOC]

##### 01：概述

- 在NLP领域，有三个基础概念：
  - 分词（Tokenization）：首先大模型会将输入内容进行分词，分割成一系列的词元（Token），形成一个词元序列。
  - 词元（Token）：指将输入的文本分割成的最小单位，词元可以是一个单词、一个词组、一个标点符号、一个字符等。
  - 嵌入（Embedding）：分词后的词元将被转换为高维空间中的向量表示，向量中包含了词元的语义信息。

##### 02：分词&词元 ID

- 将文本分词后，利用词汇表将文本词元转换成词元 ID。
- <img src="/Users/likang/work/kungFu/Algorithms-and-Data-Structures/21：LLM/photos/Token-Id.png" alt="Token-Id" style="zoom:20%;" />

###### 分词器

- 分词器通常包含两个常见的方法
- encode方法：接收文本样本，将其分词为单独的词元，然后再利用词汇表将词元转换为词元ID。
- decode方法：接收一组词元ID，将其转换回文本词元，并将文本词元连接起来，形成自然语言文本。

##### 03：Embedding

- 嵌入的本质是将**离散对象（如单词、图像甚至整个文档）映射到连续向量空间中的点**，其主要目的是将非数值的数据转换为神经网络可以处理的格式。
- 维度(dimension)：可以从一维到数千维不等。更高的维度有助于**捕捉到更细微的关系**，但这通常以牺牲计算效率为代价。
- <img src="/Users/likang/work/kungFu/Algorithms-and-Data-Structures/21：LLM/photos/Embedding.png" alt="Embedding" style="zoom:25%;" />

###### 词元 ID Embendding

- 生成一个嵌入层，实质上执行的是一种查找操作，它根据词元ID从嵌入层的权重矩阵中检索出相应的行。
- 嵌入层的工作机制是，无论词元ID在输入序列中的位置如何，**相同的词元ID始终被映射到相同的向量表示**。

###### 绝对位置嵌入(absolute positional embedding)

- 直接与序列中的**特定位置相关联**。对于输入序列的每个位置，该方法都会向对应词元的嵌入向量中添加一个独特的位置嵌入，以明确指示其在序列中的确切位置。

###### 相对位置嵌入(relative positional embedding)

- 关注的是**词元之间的相对位置或距离**，而非它们的绝对位置。

###### LLMs 输入嵌入

- 输入文本首先被分割为独立的词元。然后，这些词元通过词汇表转换为词元ID。这些词元ID继而被转换为嵌入向量，并**添加与之大小相同的位置嵌入**，最终形成用于大语言模型核心层的输入嵌入。

<img src="/Users/likang/work/kungFu/Algorithms-and-Data-Structures/21：LLM/photos/GPT-Embedding.png" alt="GPT-Embedding" style="zoom:30%;" />







