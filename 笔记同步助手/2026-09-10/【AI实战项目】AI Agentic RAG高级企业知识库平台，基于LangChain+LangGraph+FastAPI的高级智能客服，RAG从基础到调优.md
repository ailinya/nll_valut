---
author: 武哥
source: 微信公众号
url: https://mp.weixin.qq.com/s?__biz=MzAwMjk5Mjk3Mw==&mid=2247502969&idx=1&sn=9329529a64b7deacae6d4e80ddef3ad2&chksm=9b0384f4c3e7b131eae05c79d5d613b583e03de2bd61d7facc7b49b8fe865ce5fb5d69b7887a&mpshare=1&scene=1&srcid=0910JV1OZBe4WxX1ycrbF1vO&sharer_shareinfo=a553d04fd5bd954c12cd841415fe67dd&sharer_shareinfo_first=a553d04fd5bd954c12cd841415fe67dd#rd
saved: 2026-09-10 19:44:36
tags:
  - 笔记同步助手
id: af7af7f3-4871-4c2c-ac42-1b65392ea4f7
---

公众号名称：武哥聊编程

作者名称：武哥

发布时间：2026-09-07 18:03

原文链接：[https://www.bilibili.com/video/BV1oFbN6UEc5](https://www.bilibili.com/video/BV1oFbN6UEc5)

大家好，我是武哥。今天给大家分享一个纯手撸原创的AI项目实战教程：**AI Agentic RAG高级企业知识库平台**。本项目提供**完整的源码+SQL+配套喂饭级学习教程+配套面试文档**。这属于我前面发的AI时代计算机小白学习路线里面的AI实战部分，感兴趣的小伙伴可以看下完整学习路线。

[【保姆级】AI时代计算机小白学习路线，附xmind思维导图，建议收藏。AI学习路线，计算机编程学习路线](https://mp.weixin.qq.com/s?__biz=MzAwMjk5Mjk3Mw==&mid=2247502659&idx=1&sn=b8382002dfc31626aa2e0043f76b200e&scene=21#wechat_redirect)

这篇文章给大家介绍一下这个项目，它属于AI实战的地狱锤炼难度的项目，功能很饱满，认真学完你可以真正学会AI Agentic RAG实战技术，后面还会有更多的AI实战项目分享给大家。

> 01

> 项目技术栈

![[笔记同步助手/images/5b94f64faaea76e85e24da8b778513ca_MD5.png||60]]

前后端分离

**后端**：FastAPI + Uvicorn + Tortoise ORM（异步）+ LangChain 1.x + LangGraph + PyJWT + sse-starlette（Python 3.11）

**前端**：Vue3 + Vite + Element-Plus + Vue-Router + Axios + ECharts（SSE 流式问答、召回结果对比、评测柱状图、Agent 执行时间线）

**AI 技术**：混合检索（向量 + BM25 + RRF 融合）+ Rerank 重排 + 查询改写（多查询扩展 / 指代消解 / HyDE）+ 父子分块与父块回填 + 元数据过滤 + Agentic RAG（LangGraph 状态图自主决策）+ Function Calling 工具中心 + 多轮流式问答 + LLM-as-judge 四指标评测

**文档处理**：PyMuPDF + pdfplumber（PDF 与表格）+ python-docx + openpyxl + RapidOCR（扫描件识别，纯 CPU）+ jieba（中文分词）

**数据库**：MySQL（业务库）+ PostgreSQL 18 & PGVector（向量库）

**版本要求**：Python 不低于 3.11，MySQL 8，PostgreSQL 16 及以上并安装 PGVector 扩展，node.js 版本 18 以上，navicat 建议不低于 16

> 02

> 项目功能描述

![[笔记同步助手/images/5b94f64faaea76e85e24da8b778513ca_MD5.png||60]]

## 1\. 管理员

-   登录、个人资料、修改密码
    
-   用户管理：管理系统所有用户
    

知识库管理

-   知识库管理：企业资料的一级分类，绑定向量模型与切分策略
    
-   切分策略：维护递归分块与父子分块两类策略及其参数
    
-   检索策略：维护七个检索开关（向量、BM25、RRF、重排、查询改写、父块回填、两档阈值）
    
-   文档管理：上传 PDF / Word / Excel / Markdown / TXT，解析、切分、向量化
    
-   片段管理：查看与人工修正切歪了的片段
    
-   检索测试：输入一个问题，看这套策略每一阶段召回了多少条、耗时多久、分数是什么量纲
    
-   召回调试台：同一个问题最多四套策略并排跑，结果与耗时摆在一起对比
    

AI 配置

-   AI模型配置：三类模型的接入配置与连通性测试
    
-   Prompt模板：所有发给模型的提示词在页面维护，改完立刻生效
    
-   工具中心：登记 Agent 可调用的工具，启用开关是真开关
    
-   工具调用日志：查看每次工具调用的入参、返回与耗时
    
-   Agent执行：手动触发一次 Agentic RAG，观察模型自主决策的全过程
    
-   Agent运行记录：查看 Agent Run 与 Agent Step，执行过程时间线可视化
    

问答应用

-   应用管理：配置问答应用绑定的知识库、检索策略与回答 Prompt
    
-   问答测试：管理端直接测试问答效果
    
-   对话日志与反馈：查看完整对话记录，用户点踩的问题进入优化清单
    

效果评测

-   评测集管理：手工录入、从文档自动生成、从点踩沉淀三种方式维护评测用例，支持核对来源片段原文
    
-   批量评测：选定评测集与检索策略跑一轮，用 LLM-as-judge 给出四项指标
    
-   策略对比看板：多份报告并排，柱状图横向对比每一步优化带来的分数变化
    

## 2\. 用户

-   登录、注册、个人资料、修改密码、头像上传
    
-   AI 知识问答：选择问答应用后用自然语言提问，答案 SSE 流式逐字返回
    
-   引用溯源：每条回答下方列出引用的知识片段，可展开查看原文与来源文档
    
-   多轮对话：同一会话内追问，"那它呢"这类指代由系统自动补全成完整问题
    
-   会话管理：新建、切换、删除历史会话
    
-   点赞点踩：对回答做反馈，点踩可填写原因
    

  

> 03

> 项目创新点

![[笔记同步助手/images/5b94f64faaea76e85e24da8b778513ca_MD5.png||60]]

  

1.  混合检索 + RRF 融合解决"专有名词检索不到"：向量检索认识同义词但认不出 GTE-3000 这类精确型号，BM25 认得型号却听不懂同义表达，两路一起跑再用 RRF 按名次融合。融合不能直接加分数——向量给的是 0～1 的余弦相似度，BM25 没有上界，两者量纲不同，RRF 只看名次不看分数正是为此设计。中文分词还额外把 GTE3000 拆出 gte 和 3000，否则文档里写 GTE-3000 时两边一个词都对不上。
    
2.  自写 LangChain 重排组件解决"答案排在后面"：向量检索是双塔模型，问题和片段分开编码，存片段时并不知道用户会问什么，只能判断"整体像不像"，判断不了"这段能不能回答这个问题"。重排把问题和片段拼在一起送进模型做交叉编码，准得多但慢，只能用在粗筛之后。百炼的重排接口不在 OpenAI 兼容模式下，因此把它包装成 LangChain 的 BaseDocumentCompressor，作为标准组件接进检索链路。
    
3.  分数量纲显式化，阈值按量纲分开配：score 这一个字段在管线里被反复覆盖，每覆盖一次量纲就换一次——余弦相似度（相关片段实测 0.46-0.77）、BM25 分数（无上界）、RRF 名次分（0.016-0.033）、重排相关分（相关片段实测 0.14～0.20）。四种量纲取值范围差一个数量级，用同一个阈值去卡会把结果整片砍掉。所以量纲作为显式信息跟着结果一路传下去，策略表里给余弦相似度和重排相关分各配一个阈值，RRF 名次分只表示名次先后、不做阈值过滤且在页面上明确标注"本次不适用"。
    
4.  父子分块解决"找得准"与"答得全"的矛盾：片段小则语义集中命中准、但上下文不全；片段大则上下文完整、但一段混多个主题难命中。两个要求对尺寸的需求是反的，于是切两遍：只把子块向量化参与检索，命中后回填整个父块给模型生成——小块用来找，大块用来答。
    
5.  Agentic RAG：调用次数由模型决定，而不是写死在代码里：用 LangGraph 把"检索 → 评估 → 改写 → 再检索 → 生成 → 自查"编成状态图，检索几轮、改写几次、答案不合格要不要重来，全由模型每一轮的判断决定，代码只设循环上限。同一个问题问两次，走出来的步数和检索次数可能完全不同，这是 Agent 与固定流程编排的本质区别。这套 Agent 的自主性不在「调哪个工具」而在「自己的产出够不够好」，状态图上只用检索一个工具，属于 Agentic RAG（Self-RAG / CRAG 一系）范式，其中「答案没支撑就回去重写」一环是 Reflection 的简化形态。
    
6.  执行过程全程落库，自主决策可复盘：模型自主决策的副作用是过程像黑盒，所以每一步都落成 agent_step（节点名、输入、输出、状态、耗时），配合 agent_run 主记录和前端执行时间线，能完整回放"这一次模型为什么这样答"。
    
7.  实现 LLM-as-judge 四指标：Context Recall 看该找的资料找回来没有、Context Precision 看有用的资料排在前面没有、Faithfulness 看答案有没有编、Answer Relevancy 看有没有跑题。四个指标把 RAG 拆成"找资料"和"写答案"两段各测两项，分数低时先看它属于哪一段，就知道该去调检索策略还是调 Prompt。裁判模型 temperature 必须为 0，否则两次评测的差异分不清是策略变了还是裁判抖了。
    
8.  评测用例带来源片段，Context Recall 不靠模型判断：从文档自动生成用例时，要求模型逐条标出答案来自哪一个片段，这个片段 ID 回写到用例上。算 Context Recall 时直接做集合运算，比让模型判断更准也更省钱。参照必须精确到单条——记成整篇文档的全部片段会把分母撑大，答案明明只在一段里却要求整篇都召回才算满分，top\_k=5、文档 9 段时理论上限只有 5/9，这个指标就废了。
    
9.  优化效果用数据证明，不靠"我觉得变好了"：同一套评测集换不同检索策略反复跑，报告并排进对比看板。实测同一批用例：基线纯向量检索综合 0.7988，完整策略（混合检索 + 重排 + 改写 + 父块回填）0.9616，其中一条问题在基线下召回排第 12 位拿不到、重排后被顶到第 1 位。前面所有检索优化的价值，就落在这张对比图上。
    

  

> 04

> 关键页面截图

![[笔记同步助手/images/5b94f64faaea76e85e24da8b778513ca_MD5.png||60]]

![[笔记同步助手/images/c34852bf4417b98b00caf5c33cbad95c_MD5.png]]

![[笔记同步助手/images/81d9163b89e67fa3972e2012a3e9313d_MD5.png]]

![[笔记同步助手/images/a4b2c97a5399a2c874368a5daa0ce55e_MD5.png]]

![[笔记同步助手/images/39b65477170e0d34240832298a8edfec_MD5.png]]

![[笔记同步助手/images/84bfb4592674ae5caaa660de8e3f92d0_MD5.png]]

![[笔记同步助手/images/d1617e281f987d03c3b89c299c99f985_MD5.png]]

![[笔记同步助手/images/38247dc33294cc534ec3ef2dbe2fb1a4_MD5.png]]

![[笔记同步助手/images/1f13c459a91557c0ccd9015a02b2a3a5_MD5.png]]

![[笔记同步助手/images/e3c56ab6e426681c17c8eac36d2ab4a7_MD5.png]]

![[笔记同步助手/images/04f16a6045fbc2f51406063e345c835d_MD5.png]]

![[笔记同步助手/images/8c46a1539e0881bf3f10dd8d9e2af9d4_MD5.png]]

左下角阅读原文点击进入可以直接看功能详细的介绍视频以及相关原理的讲解。

如果你觉得这个项目不错，可以获取教程和源码进行学习哈，也希望大家多多转发给更多的小伙伴！

---

内容效果不满意？[点此反馈](https://feedback.notebooksyncer.com/feedback/6448dc3c_1789040675805?u=https%3A%2F%2Fmp.weixin.qq.com%2Fs%3F__biz%3DMzAwMjk5Mjk3Mw%3D%3D%26mid%3D2247502969%26idx%3D1%26sn%3D9329529a64b7deacae6d4e80ddef3ad2%26chksm%3D9b0384f4c3e7b131eae05c79d5d613b583e03de2bd61d7facc7b49b8fe865ce5fb5d69b7887a%26mpshare%3D1%26scene%3D1%26srcid%3D0910JV1OZBe4WxX1ycrbF1vO%26sharer_shareinfo%3Da553d04fd5bd954c12cd841415fe67dd%26sharer_shareinfo_first%3Da553d04fd5bd954c12cd841415fe67dd%23rd&s=obsidian)