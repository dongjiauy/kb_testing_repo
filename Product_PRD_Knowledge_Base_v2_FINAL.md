# 产品需求文档 (PRD): 智能知识库 (Knowledge Base)

## 1. 产品概述 (Overview)
*   **名称**：Knowledge Base (智能知识库)
*   **核心价值**：将用户在不同场景下的被动输入（对话、文件、会议）转化为结构化的主动资产。AI 不再只是“听过”，而是“记住了”并“整理好了”。
*   **解决痛点**：用户与 AI 交互产生大量高价值信息，但散落在历史记录中，难以回顾、难以修正，且 AI 对用户的理解是一个黑盒。

## 2. 数据来源 (Input Sources)
Knowledge Base 自动从以下渠道持续摄取数据：
1.  **用户上传的文件 (User Uploads)**：PDF, PPT, Word, Excel 等。
    *   *处理*：提取文本，识别文档类型（合同、财报、技术文档），提取关键实体和摘要。
2.  **实时对话 (Conversate - Transcripts)**：语音转文字的会议记录、通话记录。
    *   *处理*：提取会议主题、决议、待办事项、提及的人/项目。
3.  **AI 对话历史 (Even AI Conversations)**：用户与 AI 的文本交互。
    *   *处理*：提取用户偏好、指令习惯、解决过的问题、生成的代码/文案。
4.  **Onboarding 信息**：用户初始设置的个人画像（职位、公司、兴趣）。

## 3. 核心功能逻辑 (Core Logic)

### 3.1. 智能分级与摘要 (Intelligent Hierarchy & Summarization)
这是 Knowledge Base 的核心骨架。区别于传统的固定层级，我们采用 **AI 自适应分级 (AI-Adaptive Hierarchy)** 策略。

*   **核心逻辑**：AI 根据上传文件的内容结构、信息密度和上下文语义，**自动决定**拆分的层级结构和粒度，而不强制套用“主题-实体-事件”的固定模板。
*   **深度约束 (Depth Constraint)**：为了防止无限拆分导致信息碎片化，系统限制**最大深度为 5 层 (Max Level 5)**。
*   **原子层 (Atomic Level)**：无论 AI 拆分到第几层（Level 3, 4 或 5），**最后一层必然是 Fact Level (不可拆分的事实层)**，作为最小的信息存储单位。

| 层级 (Level) | 定义 (Definition) | 核心展示内容 (Display Content) | 交互与溯源 (Interaction & Traceability) |
| :--- | :--- | :--- | :--- |
| **Level 1**<br>**(Root Level)**<br>根节点/宏观主题 | **AI 识别的顶层入口**<br>可能是业务线、大项目或宏观场景。<br>*e.g. `融资战略`, `Q3 产品规划`* | **Global Summary (全局摘要)**<br>AI 综合该根节点下所有子节点生成的简报。<br>*示例：“本季度重点推进了 3 个核心模块的重构...”* | **点击卡片 -> 进入下一层**<br><br>**溯源**：鼠标悬停在 Summary，高亮下方来源。 |
| **Level 2 ~ 4**<br>**(Intermediate)**<br>中间层 (动态)** | **AI 自适应拆分节点**<br>根据内容复杂度，可能是子项目、阶段、具体会议或文档章节。<br>*e.g. `后端架构设计` -> `API 规范`* | **Node Summary (节点摘要)**<br>针对该层级内容的阶段性总结。<br>*AI 智能决策：如果内容较少，可直接跳过中间层直达 Fact。* | **点击节点 -> 展开下一层**<br><br>**导航**：提供清晰的面包屑导航 (Breadcrumbs)。 |
| **Level 3 ~ 5**<br>**(Fact Level)**<br>原子事实/知识点 | **不可再分的最小信息单元**<br>无论处于第几层，这都是叶子节点。<br>*e.g. 核心数据, 决策结论, Action Item* | **Fact Extraction (事实提取)**<br>结构化的 Key-Value 或短句。<br>*示例：“估值: 2亿”, “决策: 采用 gRPC”, “TODO: 周五前提交”* | **引用/复制**<br>作为 RAG 的基础语料，可被直接引用。 |

### 3.2. "AI First, Human Confirm" 交互流程
这是最关键的用户体验设计（UX），确保用户从第一天开始就能感受到价值。

1.  **冷启动与主动引导 (Cold Start & Onboarding)**
    *   **Step 1: 角色设定 (必选)**
        *   用户首次进入时，通过自然对话完成 Onboarding：“Hi Jessie, 为了更好地为你整理知识，我想了解一下你的核心关注点。你是负责产品研发、市场融资，还是团队管理？”
        *   **产出**：系统根据回答，立即生成空的 **Level 1 框架**（如 `融资战略`, `产品 R&D`）。这解决了“空白页恐惧”。
    *   **Step 2: 关键资料投喂 (选填)**
        *   AI 接着引导：“有些现成的资料吗？比如 BP、财报或者会议录音？发给我，我帮你填进去。”
        *   **产出**：用户上传文件后，系统解析并填充 **Level 2/3/4** 的内容。用户立刻看到一个结构化的知识库诞生。

2.  **日常持续生成 (Continuous Generation)**
    *   当日常对话或新文件积累到一定程度，AI 发现新模式（如频繁讨论“出海战略”），会**弹窗提示**：“我发现你最近在关注 [出海战略]，是否为你创建一个新的知识主题？”

3.  **可视化呈现 (Visualization)**
    *   **分级摘要视图**：默认展示 Level 1 卡片，卡片上直接显示 Global Summary 的前两行（关键信息预览）。
    *   **来源溯源 (Traceability)**：鼠标悬停在 Summary 的某句话上（例如“目标规模 3000 万”），AI 会高亮显示这句话是根据哪个 Source 推断出来的（例如高亮 `BP_v5.pdf`）。**这建立了用户对 AI 总结的信任。**

4.  **用户编辑 (Human-in-the-loop)**
    *   **修正摘要**：如果 AI 总结说“目标 5000 万”，用户可以直接修改 Summary 文本，或者标记“不准确”，让 AI 重新生成。
    *   **调整来源**：用户发现某个文件被错误归类到了 `融资`（其实是 `招聘`），可以手动将其移出，AI 会立即重新生成 Summary。

### 3.3. 源文件与数据管理 (Source File & Data Management)
用户对 Knowledge Base 的数据拥有完全控制权。系统需支持对源文件的增删改查，并确保知识库与源文件状态实时同步。

| 功能模块 (Module) | 核心逻辑 (Core Logic) | 级联影响 (Cascade Effect) |
| :--- | :--- | :--- |
| **Source Manager**<br>**(源文件列表)** | 提供一个统一视图，按时间/类型展示所有已摄取的数据源（文件、对话、链接）。<br>*操作：查看详情, 重新解析, 删除* | - |
| **Delete & Revoke**<br>**(删除与撤回)** | 用户可删除某个文件（如误传的 `个人薪资单.pdf`）。<br>**技术要求**：物理删除文件 + 从向量数据库 (Vector DB) 中彻底剔除对应的 Chunks。 | **触发 Re-Index**：<br>1. AI 重新计算受影响的 Level 1/2 Summary。<br>2. 移除所有仅由该文件支撑的 Level 4 Fact。<br>3. 界面提示：“已删除文件，相关知识点已同步清除。” |
| **Version Control**<br>**(版本管理)** | 当用户上传同名新文件（如 `BP_v3.pdf`）时，询问是 **覆盖** 还是 **新增**。<br>*策略：默认保留多版本，但在 Summary 中优先引用最新版。* | **更新权重**：<br>最新版本的文件在 RAG 检索中拥有更高权重 (Higher Priority)。 |

### 3.4. 对话式校准 (Conversational Calibration)
Knowledge Base 的可视化视图对用户是 **只读 (Read-Only)** 的。用户不需要学习复杂的图谱编辑工具（拖拽/右键/合并），所有对知识结构的修正都通过 **自然语言对话** 完成。

*   **交互逻辑**：用户在浏览 Knowledge Base 时发现错误或需要更新，直接点击“校准 (Calibrate)”按钮（或唤起语音助手），说出修改指令。AI 理解意图后，自动在后台执行复杂的图谱操作。

| 用户指令示例 (User Instruction) | AI 意图识别 (Intent Recognition) | 后台图谱操作 (Graph Operation) |
| :--- | :--- | :--- |
| **"把 'Project Titan' 改名为 'Project Apollo'。"** | **Rename Node** | 1. 检索 `Project Titan` 节点 ID。<br>2. 更新 Title 字段。<br>3. 重新生成该节点的 Summary 以匹配新语境。 |
| **"这个 '红杉资本' 的卡片不对，它应该属于 'B轮融资'，不是 'A轮'。"** | **Move / Re-classify** | 1. 解除 `红杉资本` 与 `A轮` 的父子关系。<br>2. 建立 `红杉资本` 与 `B轮` 的新关联。<br>3. 触发 Parent Node (`B轮`) 的 Global Summary 重新生成。 |
| **"这两个 '李总' 其实是同一个人，合并一下。"** | **Merge Entities** | 1. 识别两个 `李总` 节点。<br>2. 拓扑合并：将两者的子节点 (Events/Facts) 挂载到同一个新节点。<br>3. 调用 LLM 重新生成合并后的 Persona Summary。 |
| **"把关于 '团建' 的那些乱七八糟的都删了，我不关心这个。"** | **Prune / Delete** | 1. 语义搜索所有 `团建` 相关节点。<br>2. 执行软删除 (Soft Delete)。<br>3. 将涉及的源文件标记为 "Ignored" (不参与后续生成)。 |
| **"不管是哪个文件说的，现在的估值就是 2 亿，记下来。"** | **Force Overwrite** | 1. 在 `融资` 主题下强制写入一个高优先级的 **Fact Node**。<br>2. 标记为 `User_Overridden`，权重高于所有文件提取的冲突数据。 |

**工程化可行性方案 (Engineering Feasibility):**

1.  **当前视图上下文 (View Context Injection)**：
    *   当用户发起校准时，系统会将当前 **屏幕上可见的节点结构 (Visible Graph JSON)** 作为 Context 注入给 LLM。
    *   *Prompt:* "User is looking at Node A (Finance) and Node B (Hiring). User says: 'Move B into A'."

2.  **Function Calling (工具调用)**：
    *   定义一组标准的图谱操作 API：`graph.rename_node(id, new_name)`, `graph.move_node(id, new_parent_id)`, `graph.merge_nodes([id1, id2])`。
    *   利用 LLM (GPT-4o/Claude) 的 Tool Use 能力，将自然语言直接转换为 API 调用。

3.  **乐观更新 (Optimistic UI)**：
    *   用户说完指令后，前端 **立即** 模拟操作结果（例如卡片瞬间移动），而不等待后台繁重的 RAG 重算。
    *   后台异步完成 Summary 的重新生成 (Re-summarization) 后，再静默刷新内容。

### 3.5. 后台无感生长机制 (Silent Growth Mechanism)
Knowledge Base 的大部分数据积累应在后台静默完成，避免频繁打扰用户。系统采用 **Real-time Parsing + Daily Batch** 的策略。

| 阶段 (Stage) | 触发条件 (Trigger) | 执行逻辑 (Execution Logic) | 用户感知 (User Perception) |
| :--- | :--- | :--- | :--- |
| **Stage 1**<br>**实时解析**<br>(Real-time Parsing) | **用户完成一次交互**<br>(Conversate 会议结束 / Even AI 对话结束 / 文件上传完成) | **Snippet Extraction**：<br>立即提取 Level 3/4 的碎片化事实 (Snippet)，存入临时缓存区 (Buffer)。<br>*示例：提取出“红杉估值 2 亿”这个事实。* | **Zero Disturbance**<br>(完全无感，后台静默运行) |
| **Stage 2**<br>**每日聚合**<br>(Daily Batch) | **定时任务 (Cron Job)**<br>e.g. 每日凌晨 03:00 | **Clustering & Linking**：<br>1. 将缓存区的 Snippet 尝试归类到现有 Level 1/2 主题。<br>2. 若归类成功，更新对应主题的 Summary。<br>3. 若无法归类且数量较多，生成新的候选主题 (Candidate Theme)。 | **Low Disturbance**<br>(次日查看时可见更新) |
| **Stage 3**<br>**更新提示**<br>(Notification) | **有重要更新生成**<br>(New Theme / Significant Summary Change) | **Passive Notification**：<br>在 Knowledge Base 入口显示红点或 "New" 标记。<br>**仅在**发现极高置信度的新主题 (Confidence > 0.9) 时，才在 Conversate 中主动推送：“昨天的会议里，我整理出了一个新的 [海外市场] 主题，要看看吗？” | **Gentle Reminder**<br>(轻量提示，不打断工作流) |

## 4. 功能模块详述 (Feature Specs)

| 模块 | 功能点 | 描述 |
| :--- | :--- | :--- |
| **Ingestion Engine** | 多模态解析 | 解析 PDF/Doc/Audio Transcript，并进行语义向量化 (Embedding)。 |
| **Auto-Tagger** | 智能打标 | 为每条信息自动打上 `Topic` 标签。 |
| **Cluster AI** | 主题聚类 | 每晚运行一次批处理，分析是否有新的 Level 1 主题诞生。 |
| **KB UI - 首页** | 场景概览 | 展示 Level 1 主题卡片。AI 自动生成的会有 "New" 标记，等待用户确认。 |
| **KB UI - 详情页** | 知识流 | 点击主题进入，以时间轴或项目维度展示具体的 Documents 和 Insights。 |
| **Edit Mode** | 修正知识 | 用户可以右键某条知识 -> "这是错误的/不相关的"，AI 在后台更新权重或删除。 |

## 5. 典型用户故事 (User Stories)
*   **Story A (融资场景)**：Jessie 上传了 5 份 BP 和 3 份财务预测表，并开了 2 次关于估值的电话会。第二天打开 Knowledge Base，发现 AI 自动生成了一个 `融资战略` 的主题卡片，里面聚合了所有相关文件和会议摘要。Jessie 确认了这个主题，并手动把一张错误的报销单从这里移除。
*   **Story B (研发回顾)**：Jessie 想回顾上个月关于 "后端架构" 的所有讨论。她不需要去翻聊天记录，直接点开 `软件 R&D` -> `架构`，看到了当时上传的架构图和会议中关于技术选型的决议。

---

## 相关笔记

- [[Knowledge_Base_Organization_Prompt]] — 本 PRD 对应的 AI 组织 Prompt（V1）
- [[Knowledge_Base_Organization_Prompt_V2]] — 本 PRD 对应的 AI 组织 Prompt（V2）
- [[01_PRD_Mobile_App_v2]] — 同公司的另一个产品 PRD，两者共享同一后端

## 6. 生态集成与应用 (Ecosystem Integration)
Knowledge Base 不是一个孤立的功能模块，而是整个 AI 系统的 **"Central Intelligence Layer" (中央智能层)**。它必须被所有 AI 触点深度调用。

### 6.1. Conversate (Real-time Meeting Assistant)
*   **Context Injection (上下文注入)**：
    *   **场景**: 用户正在开“融资进展会”。
    *   **动作**: 系统识别会议主题后，自动将 KB 中 `融资战略` (Level 1) 下的 `红杉资本` (Level 2) 摘要注入 Context Window。
    *   **价值**: AI 能听懂“上次 Mike 说的那个顾虑”是指“AI Moat”，而不需要用户重复解释。
*   **Active Recall (主动召回)**：
    *   **场景**: 会议中提到模糊数据：“上次财报里的那个增长率”。
    *   **动作**: Conversate 后台检索 KB，在侧边栏弹窗提示：“Q3 增长率为 125% (Source: Q3_Financials.pdf)”。

### 6.2. Even AI (Async Chat Interface)
*   **Fact-Grounded RAG (基于事实的问答)**：
    *   **机制**: 任何用户提问（“我们要不要招人？”），Even AI 首先检索 KB 中的 **Fact Nodes**。
    *   **价值**: 答案不再是通用的废话，而是基于公司实际情况（“根据 `2026招聘计划`，我们尚有 5 个 HC，但预算吃紧”）。
*   **Writing Assistant (写作辅助)**：
    *   **场景**: 用户写邮件给投资人。
    *   **动作**: 输入“@融资数据”，自动补全 KB 中的最新估值和增长数据。

### 6.3. Global Search (Command Bar)
*   **Unified Entry (统一入口)**：
    *   **机制**: 在系统任何界面按 `Cmd+K`。
    *   **结果**: 不仅搜文件（File Level），更搜 **知识点 (Fact Level)**。
    *   **示例**: 搜“红杉估值”，直接展示卡片：“$7.2亿 (Source: Term_Sheet.md)”，点击可跳转至 KB 详情页。

## 7. AI 服务端架构设计与应用 (AI Server Architecture & Application)
Knowledge Base 的核心价值不仅在于“存储”，更在于被 Conversate (实时) 和 Even AI (离线) **实时调用**。

### 7.1. 数据结构策略 (Data Structuring for RAG)
研发需构建分层索引 (Hierarchical Index)，以支持不同粒度的检索：
*   **Summary Index (L1 & L2)**：用于回答宏观问题（“我们要不要跟红杉签？”）。
    *   *存储*：Level 1 Global Summary + Level 2 Entity Summary 的向量 embedding。
*   **Event Log Index (L3)**：用于回答时间线问题（“上个月我们跟红杉开了几次会？”）。
    *   *存储*：时间戳 + 事件类型的倒排索引。
*   **Fact Index (L4)**：用于回答精确事实问题（“红杉上次说估值多少？”）。
    *   *存储*：Level 4 Fact (Key-Value Pairs) 的向量 embedding + 关键词索引。

### 7.2. Conversate (实时助手) 应用场景
*   **Context Injection (上下文注入)**：
    *   当识别到用户正在进行“融资”相关的会议时，Conversate **预加载** `融资战略` (L1) 下的 `红杉` (L2) Summary 进入 Context Window。
    *   *效果*：AI 能听懂“上次 John 提的那个问题”，因为它已经知道 John 是谁、上次提了什么 (L2 Summary)。
*   **Real-time Fact Check (实时事实核对)**：
    *   当会议中提到关键数据（如“估值 2 亿”），Conversate 后台检索 **Level 4 Fact**。
    *   *交互*：如果当前说法与库中数据不符，Conversate 在界面侧边栏弹出提示：“注意：上次会议记录显示估值是 2.5 亿 (Source: 03/01 会议)”。

### 7.3. Even AI (离线任务) 应用场景
*   **Report Generation (自动报告)**：
    *   用户指令：“写一份本周融资进展周报”。
    *   *逻辑*：Even AI 遍历 **Level 3 Event** 中本周新增的记录，结合 **Level 2 Summary** 的变化，自动生成一份结构清晰的报告。
*   **Conflict Detection (冲突检测)**：
    *   后台任务：每天晚上扫描 **Level 4 Fact**，对比不同来源的事实。
    *   *效果*：如果 `Pitch Deck.pdf` 说“市场规模 100 亿”，但 `财务预测.xlsx` 说“50 亿”，Even AI 主动提示用户：“发现数据冲突，请确认以哪个为准？”并提供修正入口。
