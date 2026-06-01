##### Openclaw

------

[TOC]

##### 01：概述

- 一个**以本地优先(Local-First)，多端联动**为核心，建立一个高度灵活且可拓展的个人AI助手系统。

###### Gateway(网关)：核心控制平面

- 负责管理会话(Sessions)、状态感知(Presence)、配置、定时任务(Cron)、网络钩子(Webhooks)以及控制界面(Control UI)和Canvas宿主

##### 02：核心机制

- 每次对话前，把一堆 md 文件拼进 prompt；对话后，让 agent 把新学到的东西写回这些 md 文件。

###### 骨架：7 个核心 md 文件

1. SOUL.md：Agent 是谁
   - 定义了 agent 的人格：语气、风格、边界、价值观
2. USER.md：用户是谁
   - 这是 agent 对你的画像：你的名字、时区、工作习惯、技术偏好、沟通风格。
3. AGENTS.md：做事的规矩和踩过的坑
   - 定义了 agent 的行为规范，更重要的是，**记录了所有踩过的坑**。
4. TOOLS.md：环境备忘
   - 记录你的工作环境：SSH 主机名、摄像头设备名、文件路径习惯等。
5. SKILL.md × N：技能
   - 每个 SKILL.md 定义了一个特定领域的操作规范。
   - Skill 从 6 个来源扫描，优先级从低到高：
     1. 插件提供的 skill
     2. 内置 skill
     3. 托管 skill（`~/.openclaw/skills/`）
     4. 个人 skill（`~/.agents/skills/`）
     5. 项目 skill（`{workspace}/.agents/skills/`）
     6. Workspace skill（`{workspace}/skills/`）
6.  memory/\*.md：日常记忆
   - Agent 每天会写一个日期命名的 md 文件，记录当天的对话要点、做了什么、学到什么。
7.  MEMORY.md：提炼后的长期记忆
   - Agent 会定期把 daily memory 里的重要内容提炼到这个文件里。

##### 03：自我进化的闭环

- **外层循环：md 文件读写。** 每次对话加载，对话中更新文件。
- **内层循环：向量索引检索。** 当 memory 文件越来越多，agent 不可能把所有内容都塞进 prompt，所以 OpenClaw 用 SQLite 的 FTS5 全文搜索和 sqlite-vec 向量检索做了一个混合搜索引擎。
- 两层循环合在一起，就是**一个完整的"学习-记忆-检索-应用"系统**。而这个系统的存储介质，全是 md 文件。

##### 04：workspace 文件夹

- 代码是公开的，模型是通用的。真正属于你的、不可替代的部分，是**你 workspace 里那堆 md 文件**。
- 普适性启示：任何 AI agent 产品，如果想要做到"越用越好用"，最终都要**解决知识持久化和检索的问题**。
