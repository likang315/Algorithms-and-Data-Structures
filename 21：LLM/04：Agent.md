### Agent

------

[TOC]

##### 01：概述

- Agent：让大模型**“代理/模拟”「人」的行为，使用某些“工具/功能”来完成某些“任务”**的能力。

- Agent = 大模型（LLM）+ 规划（Planning）+ 记忆（Memory）+ 工具使用（Tool Use）；

  -  <img src="/Users/likang/work/kungFu/Algorithms-and-Data-Structures/23：LLM/photos/Agent.png" alt="Agent" style="zoom:35%;" />

  - 规划（Planning） ：任务拆解、子任务生成及自我反思；
  - 记忆（Memory） ：长期记忆存储与上下文管理，用于处理复杂任务
  - 工具使用（Tool Usage） ：通过API或外部工具增强Agent的能力（如搜索、文件操作，代码执行等）；

##### 02：Multi-Agent（多 Agent）

- 组装、协同、竞争。
- 面向复杂任务场景，多智能体会**将复杂任务分解为子任务，让不同的智能体完成不同的子任务**，即专业“人”做专业“事”
- Agent之间也可以进行**竞争**，多个子任务Agent给出了多版不同方案，由一个**决策Agent或者人来最终决定**要使用哪款子任务Agent给出的方案等等。

##### 03：Agent 主流协议

- MCP协议：解决AI Agent和外部工具交互问题；
- A2A协议：解决Agent间通信问题；
- AG-UI协议：解决AI Agent与前端应用之间的交互标准化问题。

##### 04：RAG（Retrieval Augmented Generation ，检索增强生成）

- 不需要训练和微调大模型，只需要**提供和用户提问相关的额外的信息到提示词中**，从而可以获得更高质量的回答。
- <img src="/Users/likang/work/kungFu/Algorithms-and-Data-Structures/23：LLM/photos/RAG.png" alt="RAG" style="zoom:67%;" />

##### 05：Function Calling (函数调用) 

- 一种允许大型语言模型(LLM)根据用户输入**识别它需要的工具并决定何时调用该工具**的机制。
- <img src="/Users/likang/work/kungFu/Algorithms-and-Data-Structures/23：LLM/photos/Function-Calling.jpg" alt="Function-Calling" style="zoom:80%;" />

##### 06：MCP（Model Context Protocol，模型上下文协议）

- 统一的标准协议，让 **AI 模型能够与不同的数据源和工具进行无缝交互**。它就像 USB-C 接口一样。

###### MCP 架构

- **主机**：代表任何提供 AI 交互环境的应用程序，它能访问工具和数据，并运行 MCP 客户端。
- **MCP 客户端**：在主机内运行，使其能与 MCP 服务器通信。
- **MCP 服务器**：**暴露特定功能并提供数据访问**。例如：
  - 工具：使 LLM 能通过服务器执行操作；
  - 资源：向 LLM 公开服务器中的数据和内容；
  - 提示：创建可重用的提示模板和工作流；

###### MCP 流程

- 首先需要在**主机上自动或手动配置 MCP 服务**，当用户输入问题时， MCP 客户端让 大语言模型选择 MCP 工具，大模型选择好 MCP 工具以后， MCP 客户端寻求用户同意（很多产品支持配置自动同意），MCP 客户端请求 MCP 服务器， MCP 服务调用工具并将工具的结果返回给 MCP 客户端， MCP 客户端将模型调用结果和用户的查询发送给大语言模型，大语言模型组织答案给用户。
- <img src="/Users/likang/work/kungFu/Algorithms-and-Data-Structures/23：LLM/photos/MCP.jpg" alt="MCP" style="zoom:85%;" />

##### 07：A2A （Agent2Agent）协议

- 一种开放协议，旨在**不同架构的 AI Agent 也能无缝地互相通信、交换信息，安全高效地协同完成任务**。

###### A2A 架构

- **Agent Card（智能体卡片）**：Agent的“名片”，代表“我有怎样的任务完成能力“。
- **Client Agent**：任务发起者；
- **Server Agent**：任务的执行者。Client和Server之间通信是**以任务的粒度进行，每个Agent既可以是Client，也可以是Server**；
- **Task（任务）**：客户端交给服务端Agent需要完成的工作任务。任务过程可能需要客户端的协作；任务结果可以同步等待也可以异步获取。
- **Artifact（工件）**：服务端Agent的最终产出被称为Artifact。比如一段文字、一个报告、一个图片或者视频；
- **Notifications（通知）**：服务端Agent向客户端主动推送的信息，比如一个长期任务的中间运行状态。

###### A2A 流程

- 服务端Agent启动Server，通过Agent Card展示能力；
- 客户端Agent/应用连接服务端，并获取Agent Card；
- 根据Agent Card可以把后续的任务发送给服务端Agent；
- 服务端Agent处理任务，必要的时候会输出中间结果，或者要求客户端补充信息；
- 客户端可以等待任务完成；也可以根据任务ID异步获取任务结果，也可以设置通知回调，及时获得任务的进展与工作成果；

- <img src="/Users/likang/work/kungFu/Algorithms-and-Data-Structures/23：LLM/photos/A2A.png" alt="A2A" style="zoom:50%;" />

##### 08：AG-UI（Agent-User Interaction Protocol，智能体用户交互协议）

- 解决AI Agent与前端应用之间的交互标准化问题，提供一个轻量级、事件驱动的开放协议，实现AI Agent与用户界面的实时双向通信。

###### AG-UI流程

- 客户端通过 POST 请求发起一次 AI Agent 会话；建立 HTTP 流，如 SSE 或 WebSocket 等协议，实现事件的实时监听与传输；
- 每个事件都包含类型和元信息 Metadata，用于标识和描述事件内容；
- AI Agent 持续以流式方式将事件推送至 UI 端；
- UI 端根据收到的每条事件，实时动态更新界面；
- 同时，UI 端也可以反向发送事件或上下文信息，供 AI Agent 实时处理和响应；
- <img src="/Users/likang/work/kungFu/Algorithms-and-Data-Structures/23：LLM/photos/AG-UI.png" alt="AG-UI" style="zoom:45%;" />

