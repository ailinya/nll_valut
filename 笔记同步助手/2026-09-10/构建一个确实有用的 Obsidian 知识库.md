---
author: 大菜
source: 微信公众号
url: https://mp.weixin.qq.com/s?__biz=MzIwNTc3OTAxOA==&mid=2247498833&idx=1&sn=8dec97ae4edd392133913f4a90228ddb&chksm=968ba9ca882167c6468ab4c21ed93ae615b06b1cadc2d8fff2b21dc82b1ff8e5312363f432b6&mpshare=1&scene=1&srcid=0910AzHqyloFtkwKMpZQZYN9&sharer_shareinfo=39f2e63c16b0872e236ff28307d134e1&sharer_shareinfo_first=39f2e63c16b0872e236ff28307d134e1#rd
saved: 2026-09-10 19:46:41
tags:
  - 笔记同步助手
id: 146f2a84-bdb2-4e36-896b-ce4ba834ac29
---

公众号名称：架构师修行之路

作者名称：大菜

发布时间：2026-08-28 18:52

很多号称用人工智能做知识库的产品，在演示的时候都很好用：丢了文件可以找回来，有问题能问到答案。

![[笔记同步助手/images/bdb72c35d5946abd99f1c276db4e181c_MD5.png]]

但是真正投入到工作中去之后，各种问题就出现了——信息被藏在插件缓存中，结构依靠私有的数据库来支撑，写入的过程是看不到的，老笔记被修改了也无从下手去追究责任。

> **知识好像在增长，但是可信度也在降低。**

## 它解决的是什么

`claude-obsidian` 处理的就是这个断层问题，它的定位也很清楚：针对 `Claude Code` 和可以兼容 `Agent Skills` 的本地优先 `Obsidian` 知识系统。

其中最重要的一点并不是\*\*“能够接入人工智能”\*\*，而是把它所形成的闭环知识工作过程又拉回到人们可以审查的地方来：**保留出处、使重要的结论可追踪、关联知识**、然后进行查询、研究、检索以及维护。

## 本地数据与网络边界

它比较保守，用户的钱包就是普通的文件夹，里面主要是 `markdown`、`json` 和原始文件，并且不会被插件缓存或者云端数据库所记录；项目说明也十分清晰：**不会默默地把钱包上传到云端**；

搜索默认情况下支持本地和确定性的 `BM25` 算法，在需要上下文前缀或者远程模型的情况下，则需要事先得到用户的许可才会对外发送数据；

`portable core` 不会发起任何网络请求，该项目也没有进行任何遥测或者分析工作，但是当你使用了 AI 编程代理、web 工具或者是其他外部适配器的时候，仍然有可能根据它们各自的策略来发送所选择的内容；

> **边界写得很明了。**

![[笔记同步助手/images/46fe337a86681637687d255028123f5a_MD5.png]]

  

## AI 写入也要有事务边界

另外一种写法是把多个 `worker` 直接修改数据库的行为变成只返回草稿和证据，再由一个 `orchestrator` 来合并、检查并且一次应用，并且还会记录下目标的 `SHA-256` 值，

在目标发生变化时就会被认为是**冲突而不会被静默地覆盖掉**。

> **这样就给AI添加了事务、校验以及回滚的能力。**

![[笔记同步助手/images/5c6e5610778b81fa76429b50c6aece47_MD5.png]]

  

## 安装方式

安装也很直接：

```
claude plugin marketplace add AgriciDaniel/claude-obsidian
claude plugin install claude-obsidian@agricidaniel-claude-obsidian
claude plugin list
```

## 能力边界

它没有吹嘘的意思，也不是**自动转录工具、云同步服务或者事实神谕**，不能代替备份和版本控制的功能。

`PDF/EPUB` 核心部分不包含语义抽取的内容；对于 `URL`、`YouTube` 和 `OCR` 等功能需要另外配置外部运行器。

目前仅限于只读检查和干运行，在 `Vault` 写入的时候要用到 `WSL`，在审批哈希和审阅环境绑定的情况下也要在 `WSL` 内重新审阅。

最新的正式版本为 **v2.1.1**，主要针对的是旧 `Vault` 的迁移安全性问题。

![[笔记同步助手/images/46fe337a86681637687d255028123f5a_MD5.png]]

## 写在最后

打动我的就是这样一种态度：它不替你做决定，但是会尽力使**每一条知识记录都能被检验**，而不会出现无序生成的情况。

> **人工智能早已不是稀罕物了，能够保留数据来源、结构以及修改痕迹，则是长线思维所必需的基础条件。**

https://github.com/AgriciDaniel/claude-obsidian

  

---

内容效果不满意？[点此反馈](https://feedback.notebooksyncer.com/feedback/44c6a3c8_1789040799915?u=https%3A%2F%2Fmp.weixin.qq.com%2Fs%3F__biz%3DMzIwNTc3OTAxOA%3D%3D%26mid%3D2247498833%26idx%3D1%26sn%3D8dec97ae4edd392133913f4a90228ddb%26chksm%3D968ba9ca882167c6468ab4c21ed93ae615b06b1cadc2d8fff2b21dc82b1ff8e5312363f432b6%26mpshare%3D1%26scene%3D1%26srcid%3D0910AzHqyloFtkwKMpZQZYN9%26sharer_shareinfo%3D39f2e63c16b0872e236ff28307d134e1%26sharer_shareinfo_first%3D39f2e63c16b0872e236ff28307d134e1%23rd&s=obsidian)