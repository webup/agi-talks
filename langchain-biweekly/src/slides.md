---
# See all frontmatter configurations: https://sli.dev/custom/#frontmatter-configures
# theme id or package name, see also: https://sli.dev/themes/use.html
theme: seriph
# titleTemplate for the webpage, `%s` will be replaced by the page's title
titleTemplate: '%s｜WebUP'
# some information about the slides, markdown enabled
info: |
  # LangChain Biweekly Updates
  Sharing key product updates & use case examples.
# favicon, can be a local file path or URL
favicon: https://files.codelife.cc/user-website-icon/20220523/5hyKeZxOknU2owAPvnSWD1388.png?x-oss-process=image/resize,limit_0,m_fill,w_25,h_25/quality,q_92/format,webp
# enabled pdf downloading in SPA build, can also be a custom url
download: https://github.com/webup/agi-talks/raw/master/pdfs/langchain-biweekly.pdf
# syntax highlighter, can be 'prism' or 'shiki'
highlighter: shiki
# controls whether texts in slides are selectable
selectable: false
# enable slide recording, can be boolean, 'dev' or 'build'
record: build
# define transition between slides
transition: fade
# default frontmatter applies to all slides
defaults:

# slide configurations
hideInToc: true
layout: cover
class: text-center
background: https://s2.loli.net/2024/07/22/sv4EPguXlNT7xBS.jpg 
---

# 🦜🔗 LangChain Biweekly

Sharing key product updates & use case examples 

<div class="pt-12">
  <span @click="$slidev.nav.next" class="px-2 py-1 rounded cursor-pointer" hover="bg-white bg-opacity-10">
    Press Space for next page <carbon:arrow-right class="inline"/>
  </span>
</div>

<div class="abs-br m-6 flex gap-2">
  <a href="https://muselink.cc/zhanghaili" target="_blank" alt="Auhtor"
    class="text-xl icon-btn opacity-50 !border-none !hover:text-white">
    <mdi-lecture />
  </a>
  <a href="https://github.com/webup/agi-talks" target="_blank" alt="GitHub"
    class="text-xl icon-btn opacity-50 !border-none !hover:text-white">
    <carbon-logo-github />
  </a>
  <a href="https://github.com/webup/agi-talks/raw/master/pdfs/langchain-biweekly.pdf" target="_blank" alt="Download"
    class="text-xl icon-btn opacity-50 !border-none !hover:text-white">
    <carbon-download />
  </a>
</div>

---
layout: quote
---

# LangChain Updates 📍 July 15 <sup>2024</sup>

🌰 Samples x 3，📚 Guides x 4，🎙️ Presentations x 2，💼 Cases x 2

---
layout: iframe-right
url: https://www.youtube.com/embed/EKNiz_fWrDk?si=G8sGUKOzQVtrzl3w
---

# 🌰 实用 4️⃣，难度 4️⃣

- 🦜🔗 LangChain Python 构建生成式 UI

[本应用](https://github.com/bracesproul/gen-ui-python) 旨在为使用 LangChain Python 构建生成式用户界面应用提供一个模板。它预先内置了一些用户界面功能，让您可以轻松体验和探索生成式界面。界面组件采用 [Shadcn](https://ui.shadcn.com/) 构建而成。

<T>This application aims to provide a template for building generative UI applications with LangChain Python. It comes pre-built with a few UI features which you can use to play about with gen ui. The UI components are built using Shadcn.</T>

---
layout: iframe
url: //player.bilibili.com/player.html?isOutside=true&aid=1306027197&bvid=BV1yM4m117vK&cid=1603751247&p=1&autoplay=0
---

---
layout: iframe-right
url: https://www.youtube.com/embed/ORAecR4hXsQ?si=Kf_sXhf4mcCFxLU7
---

# 🌰 实用 4️⃣，难度 4️⃣

- 🦜🕸️ LangGraph 构建的 MemGPT 示例

[本应用](https://github.com/langchain-ai/lang-memgpt) 使用 LangGraph Cloud 创建一个受 [MemGPT](https://github.com/cpacker/MemGPT) 启发的 Discord Agent，使聊天机器人能够在多次对话中记住用户互动，从而提供更加个性化的体验。

MemGPT Agent 的主要组成部分：核心记忆（持久化的用户信息），语义记忆（基于上下文的信息检索），通过工具使用进行管理。

<T>We create a MemGPT-inspired Discord agent using LangGraph Cloud, enabling chatbots to remember user interactions across conversations for a more personalized experience.<br>
Key components of the MemGPT agent: core memories (persistent user information), semantic memories (context-based retrieval), management via tool use.</T>

---
layout: iframe-right
url: https://www.youtube.com/embed/hpIOx2eGQS4?si=cG1RhPctzbuKaq2C
---

# 🌰 实用 5️⃣，难度 3️⃣

- 🦜🕸️ LangGraph 构建的 Self-Corrective RAG

[本应用](https://github.com/vbarda/pandas-rag-langgraph) 使用 LangGraph Cloud 创建一个 Self-Corrective RAG 应用，用于回答关于 Pandas 文档的问题。

我们结合了 Self-RAG 和 Corrective RAG 的理念，灵活处理模型产生的幻觉。您将看到如何在生成答案后检查幻觉，以及在返回用户问题之前检查答案的相关性。

<T>We create a Self-Corrective RAG application for answering questions about Pandas documentation using LangGraph Cloud. We implement ideas from both self-RAG and corrective RAG to flexibly handle model hallucinations. You'll see how to check for hallucinations after an answer is generated, and check for answer relevancy before returning the user question.</T>

---
layout: iframe
url: //player.bilibili.com/player.html?isOutside=true&aid=1556178744&bvid=BV1v1421b75U&cid=1617378259&p=1&autoplay=0
---

---
layout: iframe-right
url: https://langchain-ai.github.io/langgraph/how-tos/
---

# 📚 LangGraph 指南上新

- 增补了多份 [使用指南](https://langchain-ai.github.io/langgraph/how-tos/)
  - 可操控性：[Controllability](https://langchain-ai.github.io/langgraph/how-tos/branching/)
  - 人机交互：[Human-in-the-loop](https://langchain-ai.github.io/langgraph/how-tos/human_in_the_loop/time-travel/)
  - 流式传输：[Streaming](https://langchain-ai.github.io/langgraph/how-tos/streaming-content/)
- 以及 [Agentic 概念指南](https://langchain-ai.github.io/langgraph/concepts/high_level/)

<T>We now have LangGraph how-to guides on human-in-the-loop, streaming, controllability, and ReAct-style agents — plus agentic conceptual guides.</T>

---
layout: iframe-right
url: https://blog.langchain.dev/tag/in-the-loop/
---

# 🎙️ In the Loop 系列

新推出的《[In the Loop](https://blog.langchain.dev/tag/in-the-loop/)》系列博文，由 LangChain CEO Harrison Chase 主笔，解答有关代理应用的常见问题。

<T>New "In the Loop" blog series from Harrison Chase tackles commonly-asked questions on agentic apps.</T>

![](https://blog.langchain.dev/content/images/size/w1000/2024/06/Screenshot-2024-06-28-at-7.33.10-PM.png)

---
layout: iframe-right
url: https://www.youtube.com/embed/XiySC-d346E?si=69obD5Zve197ZNaw
---

# 🎙️ 构建可靠的 Agent

- [使用 LangGraph 设计和构建可靠的 Agent](https://github.com/langchain-ai/langgraph/blob/main/examples/tutorials/rag-agent-testing.ipynb)

[本演讲](https://docs.google.com/presentation/d/1QWkXi4DYjfw94eHcy9RMLqpQdJtS2C_kx_u7wAUvlZE/edit) 通过比较 LangGraph 构建的与传统的 ReAct Agent 在 RAG 任务中的表现，展示 LangGraph 构建 Agent 所带来的可靠性优势。

同时介绍使用 LangSmith 测试 Agent 的方法，不仅检查 Agent 的最终响应，还会审视 Agent 使用工具的轨迹。

<T>Here, we'll show how to design and build reliable agents using LangGraph. We’ll cover ways to test agents using LangSmith, examining both agent's final response as well as agent tool use trajectory. We'll compare a custom LangGraph agent to a ReAct agent for RAG to showcase the reliability benefit associated with building custom agents using LangGraph.</T>

---
layout: iframe-right
url: https://blog.langchain.dev/jockey-twelvelabs-langgraph/
---

# 💼 [Jockey](https://github.com/twelvelabs-io/tl-jockey) <Version>OPENSOURCE</Version>

- 🦜🕸️ LangGraph 和 [Twelve Labs](https://www.twelvelabs.io/?ref=blog.langchain.dev) 联合打造的会话式视频 Agent

想深入了解多代理设置吗？看看 Jockey 如何使用 LangGraph 优化他们的词元使用和视频处理。请查阅 [博客文章](https://blog.langchain.dev/jockey-twelvelabs-langgraph/)。

<T>Want to dive into a multi-agent setup? See how Jockey uses LangGraph to optimize their token usage and video processing. Check out the blog.</T>

![](https://blog.langchain.dev/content/images/size/w1000/2024/07/LangGraph-UI.png)

---
layout: iframe-right
url: https://blog.langchain.dev/customers-wordsmith/
---

# 💼 [Wordsmith](https://wordsmith.ai/) <Version>COMMERICAL</Version>

- 🦜⚒️ LangSmith 贯穿全产品研发生命周期

了解 WordSmith（一款面向法律团队的 AI 助手）如何在整个产品生命周期中使用 LangSmith —— 从原型设计、评估、调试到实验。

<T>Learn how WordSmith, an AI assistant for legal teams, uses LangSmith across its entire product lifecycle — from prototyping, to evaluation, to debugging, to experimentation.</T>

![](https://lh7-us.googleusercontent.com/docsz/AD_4nXeSucMnovSrs1Naa3Unc8ETGBDvShbWn3i5yhibYRho5-OZDZ4HrHEv_MOu9SL58ipHOjahSUyr94E2CKoqWZ6uCqYrpupxDaXkAnwad1z2KFra18wnCZ7FI1N6SUFkrTc6lDLpif-EIcGcR9TLJDuV_UfH?key=tPPiqBfvojwqkmxQrIaAUA)

---
layout: quote
---

# LangChain Updates 📍 July 01 <sup>2024</sup>

🚀 Releases x 2, ✨ Updates x 3，📝 Tutorial x 1

---
layout: iframe-right
url: https://blog.langchain.dev/langgraph-cloud/
---

# 🚀 实用 5️⃣，难度 4️⃣

- 🦜🕸️ [LangGraph](http://langchain.com/langgraph) 首个稳定版（v0.1）发布
- 🦜☁️ [LangGraph Cloud](https://langchain-ai.github.io/langgraph/cloud/) 封闭测试版现已上线

<br>

###### Feature Recap

LangGraph Cloud 这一全新基础设施将为大规模部署基于 LangChain 的 Agent 铺平道路。

想了解更多？[官方博客](https://blog.langchain.dev/langgraph-cloud/) 已发布博文，不容错过。

<T>LangGraph Cloud was launched in closed beta as a new infrastructure for deploying agents at scale. Read the announcement blog.</T>

---
layout: iframe
url: //player.bilibili.com/player.html?isOutside=true&aid=1206121699&bvid=BV1vf421z7cp&cid=1603007386&p=1&autoplay=0
---

---
layout: iframe-right
url: https://blog.langchain.dev/aligning-llm-as-a-judge-with-human-preferences/
---

# ✨ 实用 4️⃣，难度 2️⃣

- 🦜⚒️ LangSmith 现已升级自我完善评估系统

<br>

###### Feature Recap

您可以直接修正 LLM-as-a-Judge 的评估反馈，系统会自动将其保存为少量样本（Few Shot），从而让 AI 评判更贴合你的需求。

无需手动调整提示。更多信息请参阅 [官方博客](https://blog.langchain.dev/aligning-llm-as-a-judge-with-human-preferences/)。

<T>Self-improving evaluators in LangSmith let you make corrections to LLM evaluator feedback, storing them as few-shot examples to align the LLM-as-a-Judge with your preferences. No manual prompt tweaking needed. Learn more in the blog.</T>

---
layout: iframe
url: https://www.youtube.com/embed/3gCTa0Li4ew?si=xs4GWbiJeAjoUmKQ
---

---
layout: iframe-right
url: https://docs.smith.langchain.com/how_to_guides/tracing/mask_inputs_outputs#rule-based-masking-of-inputs-and-outputs
---

# ✨ 实用 5️⃣，难度 3️⃣

- 🦜⚒️ LangSmith 新增个人信息屏蔽功能

<br>

###### Feature Recap

LangSmith 的个人信息屏蔽功能让您可以通过指定正则表达式列表或为提取的字符串值提供转换方法来创建匿名器。

详细操作指南已在 [官方文档](https://docs.smith.langchain.com/how_to_guides/tracing/mask_inputs_outputs#rule-based-masking-of-inputs-and-outputs) 中提供。

<T>PII masking in LangSmith​­ ​lets you create anonymizers by specifying a list of regular expressions or providing transformation methods for extracted string values. Read the docs.</T>

---
layout: iframe-right
url: https://docs.smith.langchain.com/how_to_guides/playground/custom_endpoint
---

# ✨ 实用 5️⃣，难度 3️⃣

- 🦜⚒️  LangSmith Playground 支持自定义模型

<br>

###### Feature Recap

一旦部署模型服务器，您就可以在 LangSmith Playground 中使用自定义模型。

想知道如何操作？[官方文档](https://docs.smith.langchain.com/how_to_guides/playground/custom_endpoint) 已为您备好细节。

<T>Custom models in LangSmith Playground are available once you deploy a model server. See the docs.</T>

---
layout: two-cols-header
---

# 📝 基于 LangSmith 实施 AI Agent 评测评估 

::left::

🤖 全新三集视频系列 + [详细教程](https://docs.smith.langchain.com/concepts/evaluation#agents)，带您深入了解 AI Agent 评测评估的奥秘。

从响应质量、单步操作到完整工具调用链，全方位剖析评估技巧，帮助您找到评估评测 AI Agent 的要点。

<T>This 3-part video series and tutorial show how to evaluate agent performance — including response eval, single step eval, and the trajectory of tool calls.</T>

::right::

<Youtube id="NbQKDfSw3gM" width="450" />
<Youtube id="AVPflFmRkd4" width="450" />
<Youtube id="pvlT056DAHs" width="450" />