### RAG

------

[TOC]

##### 01：RAG（Retrieval-Augmented Generation）

- 不需要训练和微调大模型，只需要**提供和用户提问相关的额外的信息到提示词中**，从而可以获得更高质量的回答。

###### 工作模式

- <img src="https://github.com/likang315/Algorithms-and-Data-Structures/blob/master/21%EF%BC%9ALLM/photos/RAG.png" alt="RAG" style="zoom:67%;" />

###### 存在的问题

- 检索一次，生成一次。这意味着如果检索到的上下文不够，LLM**就无法**动态搜索更多信息。
- 如果查询需要多个检索步骤，无法通过复杂的查询进行推理。
