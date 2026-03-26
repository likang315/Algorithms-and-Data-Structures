### Skills

------

[TOC]

##### 01：概述

- Skills 是封装了**特定领域功能的可调用单元**，供 LLM 在推理过程中**按需调用**，以完成其自身无法直接执行的任务。

###### 特征

- **原子性**：每个 Skill 完成单一、明确的任务；
- **可发现性**：带有清晰描述，LLM 能理解何时调用；
- **参数化**：输入为结构化参数（非自由文本）；
- **安全性**：运行在沙箱（隔离、可控的环境）中，无副作用风险

##### 02：全量加载 VS 按需加载

- 单 Agent：所有 tools，加载到 SystemPrompt中；
- Multi Agent：每个 Agent 单独加载自己领域内知识，局部看还是数据全量加载；
- Skills 机制：让 Agent 先知道"有什么能力"，**需要时再学习"如何使用"（按需加载）**，而不是一开始就把所有知识加载进上下文。

##### 03：核心组成

1. **结构化指令**：用 Markdown 编写的SOP
   - 定义何时使用此 Skill（触发条件）
   - 描述具体的执行步骤（操作流程）
   - 说明可用的工具和资源（支撑材料）
2. **资源文件**：支撑指令执行的参考材料
   - 详细的 API 文档和技术规范
   - 使用示例和最佳实践指南
   - 模板文件和配置样例
3. **可执行脚本**：提供确定性操作的代码
   - 数据处理和转换脚本
   - 验证和校验工具
   - 与外部系统的集成接口

###### Skill 结构规范

- Skill 以文件系统的目录结构组织，核心是 `SKILL.md` 文件：

  - ```shell
    skill-name/
    ├── SKILL.md          # 必需：包含元数据和指令
    ├── references/       # 可选：详细参考文档
    │   └── api-doc.md
    ├── scripts/          # 可选：可执行脚本
    		│   └── process.py
    └── assets/           # 可选：模板和资源
    		└── template.html
    ```

- `SKILL.md` 文档的标准结构：

  - ```shell
    # === 必需字段 ===
    name: skill-name  # 技能的唯一标识符，使用 kebab-case 命名
    description:   	  # 注意：description 是智能体选择技能的唯一依据，必须写清楚！
      简洁但精确的描述，说明：
      1. 这个技能做什么
      2. 什么时候应该使用它
      3. 它的核心价值是什么
    # === 可选字段 ===
    version: 1.0.0   												# 语义化版本号
    allowed_tools: [tool1, tool2]   				# 此技能可以调用的工具列表（白名单）
    required_context: [context_item1]   		# 此技能需要的上下文信息
    license: MIT   													# 许可协议
    author: Your Name <email@example.com>   # 作者信息
    tags: [database, analysis, sql]         # 便于分类和搜索的标签
    ```

##### 04：渐进式披露（Progressive Disclosure）：破解上下文困境

- 将Skill信息分为**三层**，Agent按需加载，既确保必要时**不遗漏细节**，又避免一**次性将过多内容塞入上下文中**。
  - **启动时轻量**：只加载必要的元数据，支持大量 Skill 注册
  - **执行时精准**：只加载相关的 Skill，避免无关知识干扰
  - **使用时完整**：保持 SOP 的逻辑连贯性，不碎片化
  - <img src="https://github.com/likang315/Algorithms-and-Data-Structures/blob/master/21%EF%BC%9ALLM/photos/progressive-disclosure.png" alt="progressive-disclosure" style="zoom:25%;" />

##### 05：Skill 示例

- 该 SKILL.md 文件展示了一个完整技能的结构：
  - 清晰的元数据（智能体用于发现和匹配）
  - 完整的数据库结构说明
  - 详细的工作流程指导
  - 丰富的查询模式示例（可直接复用的 SQL 模板）
  - 实用的注意事项和故障排查

~~~markdown
---
name: mysql-employees-analysis
description: >
  将中文业务问题转换为 SQL 查询并分析 MySQL employees 示例数据库。
  适用于员工信息查询（如"工号12345的员工信息"）、
  薪资统计（如"平均薪资最高的部门"）、
  部门分析（如"各部门人数分布"）、
  职位变动历史（如"某员工的晋升路径"）等场景。
version: 1.0.0
allowed_tools: [execute_sql]
tags: [database, mysql, sql, employees, analysis]
---

# MySQL 员工数据库分析技能

## 概述

这个技能专门用于分析 MySQL 官方提供的 `employees` 示例数据库。
该数据库包含约 300,000 名虚拟员工的记录，涵盖 1985-2000 年的数据。

**核心能力**：
- 理解中文自然语言的业务问题
- 转换为高效的 SQL 查询
- 执行查询并解读结果
- 提供业务洞察和数据解读

## 数据库结构

### 核心表结构

| 表名           | 说明         | 关键字段                                                     |
| -------------- | ------------ | ------------------------------------------------------------ |
| `employees`    | 员工基本信息 | emp_no, birth_date, first_name, last_name, gender, hire_date |
| `salaries`     | 薪资历史     | emp_no, salary, from_date, to_date                           |
| `titles`       | 职位历史     | emp_no, title, from_date, to_date                            |
| `dept_emp`     | 员工部门关系 | emp_no, dept_no, from_date, to_date                          |
| `dept_manager` | 部门经理     | emp_no, dept_no, from_date, to_date                          |
| `departments`  | 部门信息     | dept_no, dept_name                                           |

### 关键约定

⚠️ **重要**：`to_date = '9999-01-01'` 表示"当前有效"的记录。
查询"当前"状态时（如现任员工、当前薪资），必须加此过滤条件。

完整的表结构参见：`db_schema.sql`

## 工作流程

### 第一步：理解需求

仔细分析用户的中文描述，识别：
- **查询目标**：要查什么数据？（员工、薪资、部门...）
- **筛选条件**：有什么限制？（特定部门、时间范围、薪资区间...）
- **聚合维度**：需要统计吗？（平均值、总数、排名...）
- **时间范围**：是历史数据还是当前状态？

### 第二步：构建 SQL

根据需求选择合适的查询模式（见下方"常见查询模式"）。

**编写原则**：
1. 使用明确的表别名（如 `e` for employees）
2. JOIN 时优先使用主键/外键
3. 注意日期过滤（特别是 `to_date`）
4. 合理使用索引字段
5. 大结果集要加 LIMIT

### 第三步：执行查询

调用 `execute_sql` 工具执行构建好的 SQL。

```python
# 示例调用（智能体会自动转换为工具调用）
result = execute_sql(query="SELECT ...")


### 第四步：解读结果

将查询结果转化为自然语言回答：
- 用表格呈现结构化数据
- 突出关键数据点
- 提供业务洞察（如趋势、异常）
- 如果结果为空，说明可能的原因

## 常见查询模式

### 模式 1：基础信息查询

-- 查询特定员工的基本信息
SELECT emp_no, CONCAT(first_name, ' ', last_name) AS full_name,
       gender, birth_date, hire_date
FROM employees
WHERE emp_no = <员工号>;


### 模式 2：当前状态查询

-- 查询当前薪资最高的员工（TOP 10）
SELECT e.emp_no,
       CONCAT(e.first_name, ' ', e.last_name) AS name,
       s.salary
FROM employees e
JOIN salaries s ON e.emp_no = s.emp_no
WHERE s.to_date = '9999-01-01'  -- 当前薪资
ORDER BY s.salary DESC
LIMIT 10;


### 模式 3：历史趋势分析

-- 查询某员工的薪资变化历史
SELECT emp_no, salary, from_date, to_date,
       salary - LAG(salary) OVER (ORDER BY from_date) AS increase
FROM salaries
WHERE emp_no = <员工号>
ORDER BY from_date;

## 注意事项

### ⚠️ 时间字段的正确处理

- <strong>当前状态</strong>：必须使用 `to_date = '9999-01-01'` 过滤
- <strong>历史查询</strong>：注意 `from_date` 和 `to_date` 的范围
- <strong>时间计算</strong>：使用 `TIMESTAMPDIFF`、`DATEDIFF` 等函数

### ⚠️ 数据质量

- <strong>NULL 值处理</strong>：使用 COALESCE 或 IFNULL 处理空值
- <strong>重复记录</strong>：注意员工可能多次调岗，查询时考虑去重
- <strong>数据范围</strong>：数据库只包含 1985-2000 年的数据，查询时注意时间边界

## 故障排查

<strong>问题 1：查询结果为空</strong>
- 检查是否正确使用了 `to_date = '9999-01-01'`
- 验证员工号或部门号是否存在
- 检查日期范围是否合理

<strong>问题 2：查询速度慢</strong>
- 检查是否缺少索引字段的 WHERE 条件
- 考虑将复杂查询拆分为多步
- 使用 EXPLAIN 分析查询计划
~~~
