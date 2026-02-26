### Agent

------

[TOC]

##### 01：什么是 Agent

- Agent：让大模型**代理/模拟「人」的行为，使用某些「工具/功能」来完成某些任务**的能力。（与 WorkFlow相比，将决策交给AI）

- Agent = 大模型（LLM）+ 规划（Planning）+ 记忆（Memory）+ 工具使用（Tool Use）；

  -  <img src="https://github.com/likang315/Algorithms-and-Data-Structures/blob/master/21%EF%BC%9ALLM/photos/Agent.png" alt="Agent" style="zoom:35%;" />

  -  规划（Planning） ：任务拆解、子任务生成及自我反思；
  -  记忆（Memory） ：长期记忆存储与上下文管理，用于处理复杂任务
  -  工具使用（Tool Usage） ：通过API或外部工具增强Agent的能力（如搜索、文件操作，代码执行等）；

##### 02：Multi-Agent（多 Agent）

- 面向复杂任务场景，多智能体会**将复杂任务分解为子任务，让不同的智能体完成不同的子任务**，即专业“人”做专业“事”
- Agent之间也可以进行**竞争**，多个子任务Agent给出了多版不同方案，由一个**决策Agent或者人来最终决定**要使用哪款子任务Agent给出的方案等等。

##### 03：Agent 主流协议

- MCP协议：解决AI Agent和外部工具交互问题；
- A2A协议：解决Agent间通信问题；
- AG-UI协议：解决AI Agent与前端应用之间的交互标准化问题。

##### 04：Function Calling (函数调用) 

- 允许大型语言模型(LLM)根据用户输入**识别它需要的工具并决定何时调用该工具**的机制。实现了非结构化 -> 结构化的转变。

- <img src="https://github.com/likang315/Algorithms-and-Data-Structures/blob/master/21%EF%BC%9ALLM/photos/Function-Calling.jpg" alt="Function-Calling" style="zoom:80%;" />

- ```json
  // LLM返回结构化的工具调用，系统解析执行
  {
    "id": "chatcmpl-abc123",
    "object": "chat.completion",
    "choices": [
      {
        "index": 0,
        "message": {
          "role": "assistant",
          "content": null,
          "tool_calls": [
            {
              "id": "call_abc123",
              "type": "function",
              "function": {
                "name": "get_weather",
                "arguments": "{\"city\": \"北京\", \"date\": \"today\"}"
              }
            }
          ]
        },
        "finish_reason": "tool_calls"
      }
    ]
  }
  ```


##### 05：MCP（Model Context Protocol，模型上下文协议）

- 标准化交互协议和 Server架构，让 **AI 模型能够与不同的数据源和工具进行无缝交互**。

###### MCP 架构

- **主机**：代表任何提供 AI 交互环境的应用程序，它能访问工具和数据，并运行 MCP 客户端。在 Function-Calling的基础上，增加一层JSON-RPC 协议转化。
- **MCP 客户端**：在主机内运行，使其能与 MCP 服务器通信。
- **MCP 服务器**：**暴露特定功能并提供数据访问**。例如：
  - 工具：使 LLM 能通过服务器执行操作；
  - 资源：向 LLM 公开服务器中的数据和内容；
  - 提示：创建可重用的提示模板和工作流；

###### MCP 流程

- 首先需要在**主机上自动或手动配置 MCP 服务**，当用户输入问题时， MCP 客户端让 大语言模型选择 MCP 工具，大模型选择好 MCP 工具以后， MCP 客户端寻求用户同意（很多产品支持配置自动同意），MCP 客户端请求 MCP 服务器， MCP 服务调用工具并将工具的结果返回给 MCP 客户端， MCP 客户端将模型调用结果和用户的查询发送给大语言模型，大语言模型组织答案给用户。
- <img src="https://github.com/likang315/Algorithms-and-Data-Structures/blob/master/21%EF%BC%9ALLM/photos/MCP.jpg" alt="MCP" style="zoom:85%;" />

##### 06：A2A （Agent2Agent）协议

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

- <img src="https://github.com/likang315/Algorithms-and-Data-Structures/blob/master/21%EF%BC%9ALLM/photos/A2A.png" alt="A2A" style="zoom:50%;" />

##### 07：AG-UI（Agent-User Interaction Protocol，智能体用户交互协议）

- 解决AI Agent与前端应用之间的交互标准化问题，提供一个轻量级、事件驱动的开放协议，实现AI Agent与用户界面的实时双向通信。

###### AG-UI流程

- 客户端通过 POST 请求发起一次 AI Agent 会话；建立 HTTP 流，如 SSE 或 WebSocket 等协议，实现事件的实时监听与传输；
- 每个事件都包含类型和元信息 Metadata，用于标识和描述事件内容；
- AI Agent 持续以流式方式将事件推送至 UI 端；
- UI 端根据收到的每条事件，实时动态更新界面；
- 同时，UI 端也可以反向发送事件或上下文信息，供 AI Agent 实时处理和响应；
- <img src="https://github.com/likang315/Algorithms-and-Data-Structures/blob/master/21%EF%BC%9ALLM/photos/AG-UI.png" alt="AG-UI" style="zoom:45%;" />

##### 08：RAG（Retrieval Augmented Generation ，检索增强生成）

- 不需要训练和微调大模型，只需要**提供和用户提问相关的额外的信息到提示词中**，从而可以获得更高质量的回答。

###### 工作模式

- <img src="https://github.com/likang315/Algorithms-and-Data-Structures/blob/master/21%EF%BC%9ALLM/photos/RAG.png" alt="RAG" style="zoom:67%;" />

###### 存在的问题

- 检索一次，生成一次。这意味着如果检索到的上下文不够，LLM**就无法**动态搜索更多信息。
- 如果查询需要多个检索步骤，无法通过复杂的查询进行推理。

##### 09：Agent Skills

- 思想：让 Agent 先知道"有什么能力"，需要时再学习"如何使用"，而不是一开始就把所有知识装进上下文。【**按需加载**】

**渐进式披露（Progressive Disclosure）**：是一种上下文管理策略，将知识加载分为三个层次：

| 层次     | 加载时机       | 内容                   | 成本         |
| -------- | -------------- | ---------------------- | ------------ |
| 元数据层 | Agent启动时    | Skill的名称和描述      | 100tokens/个 |
| 指令层   | 任务触发时     | 完整的SOP文档          | 2-3ktokens   |
| 资源层   | 执行过程中按需 | 参考文档或脚本执行结果 | 按需计算     |

- 启动时轻量：只加载必要的元数据，支持大量 Skill 注册；
- 执行时精准：只加载相关的 Skill，避免无关知识干扰；
- 使用时完整：保持 SOP 的逻辑连贯性，**不碎片化**。

##### Skill 结构

- ```
  skill-name/
  ├── SKILL.md          # 必需：包含元数据和指令
  ├── references/       # 可选：详细参考文档
  │   └── api-doc.md
  ├── scripts/          # 可选：可执行脚本
  │   └── process.py
  └── assets/           # 可选：模板和资源    
  		└── template.html
  ```

  

##### 10：Agentic AI

- 定义：能够理解高层次目标、拆解目标、自主制定计划（规划）、执行多步骤任务、并能在过程中动态调整决策的人工智能系统。

###### 特征

- **自主性（Autonomy）：**
  - 这是最核心的特征。系统能够在**最少的人工干预下自行运作，做出决策**。
- **规划与执行（Planning & Execution）：*
  - 能够将复杂目标分解为一系列子任务（规划），然后按顺序或并行地执行这些任务（执行）。
- **工具使用（Tool Use）：**
  - 能够调用外部工具和API来扩展其能力。
- **记忆与状态（Memory & State）：** 
  - 能够记住之前的交互、执行过程和结果，并在后续决策中使用这些信息。这使得**智能体能够处理长期且复杂的任务**。
- **反思与迭代（Reflection & Iteration）：**
  - 能够评估自己行动的结果。如果结果不理想，它可以**自我批判**、**分析错误原因**、**调整计划**并重新尝试。

###### 工作模式

- <img src="https://github.com/likang315/Algorithms-and-Data-Structures/blob/master/21%EF%BC%9ALLM/photos/Agentic-AI.png" alt="Agentic-AI" style="zoom:50%;" />
- 第六步只有一个检索 Agent，如果任务复杂，可以拆分为多个 Agent，进行协作。
