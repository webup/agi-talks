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

# LangChain Updates 📍 Aug 12 <sup>2024</sup>

🚀 Releases x 2，✨ Updates x 5，🎙️ Presentation x 1

---
layout: iframe-right
url: https://blog.langchain.dev/langgraph-studio-the-first-agent-ide/
---

# 🚀 实用 5️⃣，难度 2️⃣

- 🦜🕸️ LangGraph Studio：业内首个 Agent IDE

[LangGraph Studio](https://github.com/langchain-ai/langgraph-studio) 提供了一种新的开发 LLM 应用的方法，通过提供专门的 IDE，实现复杂 Agent 应用的可视化、交互和调试。

使用可视图表和编辑状态的能力，您可以更好地理解 Agent 工作流程并更快地迭代。LangGraph Studio 与 LangSmith 集成，让您能与队友协作追踪和调试故障。

<T>LangGraph Studio provides a new way to develop LLM applications by providing a dedicated IDE that enables visualizing, interacting, and debugging complex Agent workflows. With the ability to visualize workflows using charts and edit states, you can better understand the Agent's workflow and iterate faster.</T>

> 目前仅支持苹果芯片设备；需注册 LangSmith（免费）

---
layout: iframe-right
url: https://blog.langchain.dev/langgraph-v0-2/
---

# 🚀 实用 4️⃣，难度 3️⃣

- 🦜🕸️ LangGraph [v0.2](https://github.com/langchain-ai/langgraph/releases/tag/0.2.0) 引入新的 Checkpointer
- 🦜🕸️ LangGraph Cloud 开放公开测试

LangGraph 的关键支柱之一是其 [内置的持久化层](https://langchain-ai.github.io/langgraph/how-tos/persistence/)，通过 Checkpointer 实现。

当您将 Checkpointer 与图一起使用时，可以管理图的状态。Checkpointer 在每个步骤中保存图的状态，从而支持以下功能在内的几个强大功能：

- 会话存储：存储用户交互的完整历史
- 错误恢复：从最后一个成功步骤的存储点执行
- 人机协同：实施工具审批，等待人工输入等
- 时间旅行：在任何执行历史点编辑图状态，并从那个时间点创建一个替代执行（即 Fork）

> LangGraph Postgres Checkpointer 可用于生产级应用

---
layout: iframe
url: //player.bilibili.com/player.html?isOutside=true&aid=1206121699&bvid=BV1vf421z7cp&cid=1603007386&p=1&autoplay=0
---

---
layout: iframe-right
url: https://python.langchain.com/v0.2/docs/how_to/chat_model_rate_limiting/
---

# ✨ 实用 4️⃣，难度 2️⃣

- 🦜🔗 聊天模型的速率限制

LangChain 内置了一个 [内存速率限制器](https://python.langchain.com/v0.2/docs/how_to/chat_model_rate_limiting/)，可以帮助您避免超过模型提供商允许的最大请求速率。

您现在可以对任何聊天模型使用速率限制器，自 `langchain-core >= 0.2.24` 起可用。

<T>LangChain has a built-in memory rate limiter that can help you avoid exceeding the maximum rate of requests allowed by the chat model provider. <br>You can now use the rate limiter for any chat model, available as of langchain-core 0.2.24.</T>

---
layout: iframe-right
url: https://python.langchain.com/v0.2/docs/how_to/pydantic_compatibility/
---

# ✨ 实用 4️⃣，难度 3️⃣

- 🦜🔗 [Pydantic 兼容性支持](https://python.langchain.com/v0.2/docs/how_to/pydantic_compatibility/) 提升

LangChain API 现在允许使用 Pydantic v2 模型来使用 `BaseTool` 和 `StructuredTool`。这可以增强类型安全性，提高代码可读性，并简化工具和聊天模型的集成。

许多聊天模型也支持在 `bind_tools` 和 `with_structured_output` 中接受 v2 模型（包括 Anthropic、OpenAI、Mistral 等）。

这些更新需要 `langchain-core >= 0.2.23`。

<T>LangChain APIs now allow using Pydantic v2 models with `BaseTool` and `StructuredTool`. Many chat models also support accepting Pydantic v2 models in `bind_tools` and `with_structured_output`.</T>

---
layout: iframe-right
url: https://blog.langchain.dev/dynamic-few-shot-examples-langsmith-datasets/
---

# ✨ 实用 5️⃣，难度 2️⃣

- 🦜⚒️ LangSmith 数据集动态生产少样本示例

少样本示例（Few Shot）是一种常见的提高应用性能的技术。动态选择少样本提示的示例可以带来 [进一步的改进](https://blog.langchain.dev/few-shot-prompting-to-improve-tool-calling-performance/)。

在 LangSmith 中，通过动态的少样本示例，您将可以一键索引数据集中的示例，并根据用户输入动态选择最相关的少样本示例。这使您能够快速迭代并提高 LLM 应用性能。

<T>With dynamic few-shot examples in LangSmith, you can Index examples in your datasets in one click and dynamically select the most relevant few-shot examples based on user input. This lets you rapidly iterate and improve LLM app performance.</T>

> 本功能目前处于封闭测试阶段，您可以通过 [此处](https://forms.gle/in9R6t9HNSYMBt7P7) 注册进入等待名单。计划在本月稍后进行公开发布。

---
layout: iframe
url: https://www.youtube.com/embed/37VaU7e7t5o
---

---
layout: iframe-right
url: https://docs.smith.langchain.com/how_to_guides/human_feedback/annotation_queues#create-an-annotation-queue
---

# ✨ 实用 4️⃣，难度 1️⃣

- 🦜⚒️ LangSmith 标注队列支持多标注者操作

LangSmith 的标注队列现在支持允许多人审查单个运行（Run）条目。这使得协调多个标注员的数据审查更加容易。

- 对于每个标注队列，您可以指定每个运行需要审查的用户数量
- 如果启用了预订，运行结果将被第一个查看的人“预订”，防止多人进行多次审查
- 当指定数量的审查人员将其标记为完成时，每个人的队列中都会移除这个任务

<T>LangSmith's annotation queue now supports allowing multiple people to review an individual run. This makes it easier to coordinate data review across multiple annotators.</T>

---
layout: iframe-right
url: https://blog.langchain.dev/tag/in-the-loop/
---

# 🎙️ In the Loop 更新

着重讨论了如何构建和 Agent 配套的用户体验

- [即时对话](https://blog.langchain.dev/ux-for-agents-part-1-chat-2/)：介绍最常见的对话应用场景，并讨论 “流式 / 非流式” 对话模式的优缺点
- [后台运行](https://blog.langchain.dev/ux-for-agents-part-2-ambient/)：探讨如果帮助在后台运行的 Agent 与用户进行互动和建立联系
  - 一个不错的示例应用是 [Devin](https://www.cognition.ai/blog/introducing-devin)，它可以让用户看到所有步骤，可以回退到特定时间点的开发状态，并从那里进行修正
  - 对于 Agent 不知道该做什么或如何回答的情况，它需要引起用户的注意并寻求帮助
- [用户界面](https://blog.langchain.dev/ux-for-agents-part-3/)：探讨了（批量）表格式、（动态）生成式、（侧边）协作式界面对于 Agent 的适用性和影响（引导 Agent 的能力建设）

---
layout: quote
---

# LangChain Updates 📍 July 29 <sup>2024</sup>

✨ Updates x 5，📝 Tutorial x 1，🎙️ Presentation x 1

---
layout: iframe-right
url: https://blog.langchain.dev/improving-core-tool-interfaces-and-docs-in-langchain/
---

# ✨ 实用 5️⃣，难度 3️⃣

- 🦜🔗 改进工具调用相关接口和文档

我们对核心的工具接口和文档进行了优化，简化了工具集成流程，提高了对多样化输入的处理能力，并能够输出更复杂的成果。

这些优化使得在 LangChain 中更高效地使用工具，并大幅降低了编写自定义工具时的工作量。

<T>We've improved our core tool interfaces and docs to simplify tool integrations and better handle diverse inputs, plus return complex outputs. These improvements enable more robust tool use in LangChain and reduce the manual effort of writing custom wrappers or interfaces.</T>

---
layout: iframe-right
url: https://changelog.langchain.com/announcements/initialize-any-model-in-one-line-of-code
---

# ✨ 实用 5️⃣，难度 2️⃣

- 🦜🔗 初始化任何模型只需一行代码

LangChain 集成了许多聊天模型，导入方式繁多，记忆起来较为困难。

为了简化操作，我们在 LangChain 为 [Python](https://python.langchain.com/v0.2/docs/how_to/chat_models_universal_init/) 和 [JavaScript](https://js.langchain.com/v0.2/docs/how_to/chat_models_universal_init/) 增加了一个通用的模型初始化器。这样，您就可以使用任何常见的聊天模型，无需记住不同的导入路径和类名，使操作更加便捷。

使用它与 OpenAI、Anthropic、Gemini、Bedrock、Cohere 等平台一起使用，更方便！

<T>LangChain now has a universal model initializer that makes it easy to use any of the common chat models. You can use it with OpenAI, Anthropic, Gemini, Bedrock, and Cohere, making it even easier to use them with LangChain.</T>

---
layout: iframe-right
url: https://changelog.langchain.com/announcements/build-prompts-faster-and-compare-in-langsmith-playground
---

# ✨ 实用 5️⃣，难度 1️⃣

- 🦜⚒️ LangSmith Playground 可分栏对比

您现在可以在 LangSmith 的 Playground 中轻松并排比较多个提示和模型配置。

快速构建、测试和迭代：一边创建提示一边对比，实验更改，并在单一视图中评估数据集。前往 LangSmith 的 Playgroud 侧边栏，点击 `Compare` 功能进行体验。

<T>Now you can easily compare multiple prompts and model configurations in the LangSmith Playground. Build, test, and iterate faster: build prompts side-by-side, experiment with changes, and evaluate datasets in a single view.</T>

---
layout: iframe-right
url: https://changelog.langchain.com/announcements/filtering-runs-within-the-trace-view
---

# ✨ 实用 5️⃣，难度 1️⃣

- 🦜⚒️ LangSmith [在追踪视图中过滤 Run 条目](https://docs.smith.langchain.com/how_to_guides/monitoring/filter_traces_in_application#filtering-runs-within-the-trace-view)

您现在可以在 LangSmith 的跟踪视图中过滤出特定的 Run。这对于长时间运行的 Agent 特别有帮助，它们可以生成包含数百甚至数千个子运行。

不再需要在总览和详情面板之间来回切换，以查看您最关键的问题。

<T>Now you can filter out specific Runs in the LangSmith Trace View. This is especially helpful for long-running Agents that can generate thousands of sub-runs. No more switching back and forth between the runs table and the run details pane to see your most critical issues.</T>

---
layout: iframe-right
url: https://changelog.langchain.com/announcements/enhanced-key-value-search-matching-inputs-and-outputs
---

# ✨ 实用 4️⃣，难度 1️⃣

- 🦜⚒️ [增强键值搜索](https://docs.smith.langchain.com/how_to_guides/monitoring/filter_traces_in_application#filter-based-on-input--output-key-value-pairs)，匹配输入和输出

用户现在可以通过 JSON 键值对在输入或输出中过滤跟踪或运行。这为 LangSmith 提供了更强大的搜索体验，因为您现在可以匹配 JSON 输入和输出中的确切字段（而不是仅限关键字搜索）。

匹配可发生的位置：顶级 / 嵌套键值对

<T>Users can now filter traces or runs by JSON key-value pairs in the input or output. This provides a more powerful search experience for LangSmith, as you can now match exact fields in JSON inputs and outputs (instead of just keyword searches，and matching can happen at: top-level / nested key-value pairs).</T>

---
layout: iframe-right
url: https://www.youtube.com/embed/Nfk99Fz8H9k?si=1nOx70QamLiAxBH3
---

# 📝 Ollama 工具调用

工具是实用程序（例如 API 或自定义函数），可以被 LLM 调用，赋予模型新的能力。

Ollama 最近增加了 [工具调用](https://ollama.com/blog) 功能，我们将其纳入了一个新的 [langchain-ollama](https://pypi.org/project/langchain-ollama/) 合作伙伴包中。

在这个 [示例](https://github.com/webup/notebooks/blob/main/tool-calling-agent-local.ipynb) 中，我们展示了如何使用新的 Ollama 合作伙伴包来调用本地模型执行工具。

同时，我们展示了如何在 LangGraph 中创建一个简单的工具调用 Agent，它使用本地运行的工具来执行网页搜索和向量存储检索。

<T>Ollama recently added function calling and we've incorporated this into a new partner package. Here, we show how to use the new Ollama partner package to perform tool calling with a local model. We also show how to create a simple tool calling Agent in LangGraph that uses a local tool to perform web search and vector store retrieval.</T>

---
layout: iframe
url: //player.bilibili.com/player.html?isOutside=true&aid=112875666476527&bvid=BV1JxvCeBE9r&cid=500001632722029&p=1&autoplay=0
---

---
layout: iframe-right
url: https://blog.langchain.dev/few-shot-prompting-to-improve-tool-calling-performance/
---

# 🎙️ 少样本助力工具调用

实验表明 Few Shot 可显著提高工具调用准确性

我们在两个数据集上进行了实验：

- 第一个，[查询分析](https://smith.langchain.com/public/6f62ae8b-4d96-4f0f-8eef-177ae3e30a65/d) 数据集，是一个相当标准的设置，其中使用对 LLM 的单次调用根据用户问题调用不同的搜索索引。

- 第二个，[多元宇宙数学](https://smith.langchain.com/public/f8b159be-89e4-4f9f-93a9-30434fd31cbf/d)，测试了在更具有 Agent 形态的 ReAct 工作流程的上下文中的函数调用（这涉及到对 LLM 的多次调用）。

我们在多个 OpenAI 和 Anthropic 模型上进行了基准测试。我们尝试了不同的方法向模型提供少量示例，目的是看看哪种方法能产生最佳结果。

<T>We ran a few experiments, which show how few-shot prompting can significantly enhance model accuracy - especially for complex tasks. Read on for how we did it (and the results).</T>

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