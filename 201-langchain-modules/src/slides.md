---
# See all frontmatter configurations: https://sli.dev/custom/#frontmatter-configures
# theme id or package name, see also: https://sli.dev/themes/use.html
theme: 'seriph'
# titleTemplate for the webpage, `%s` will be replaced by the page's title
titleTemplate: '%s｜WebUP'
# some information about the slides, markdown enabled
info: |
  AGI 学习笔记，仅供个人学习使用
# favicon, can be a local file path or URL
favicon: https://files.codelife.cc/user-website-icon/20220523/5hyKeZxOknU2owAPvnSWD1388.png?x-oss-process=image/resize,limit_0,m_fill,w_25,h_25/quality,q_92/format,webp
# enabled pdf downloading in SPA build, can also be a custom url
download: 'https://github.com/webup/agi-talks/raw/master/pdfs/201-langchain-modules.pdf'
# syntax highlighter, can be 'prism' or 'shiki'
highlighter: 'shiki'
# controls whether texts in slides are selectable
selectable: false
# enable slide recording, can be boolean, 'dev' or 'build'
record: 'build'
# define transition between slides
transition: fade
# default frontmatter applies to all slides
defaults:

# slide configurations
hideInToc: true
layout: cover
class: text-center
background: https://source.unsplash.com/collection/94734566/1920x1080 
---

# LangChain 模块解析<sub>（上）</sub>

LangChain Modules Hands-on

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
  <a href="https://github.com/webup/agi-talks/raw/master/pdfs/201-langchain-modules.pdf" target="_blank" alt="Download"
    class="text-xl icon-btn opacity-50 !border-none !hover:text-white">
    <carbon-download />
  </a>
</div>

---
src: ../../pages/common/series.md
---

---
src: ../../pages/common/toc.md
---

---
layout: iframe
url: https://embeds.onemodel.app/d/iframe/IDSmkjp32ePfdFqKcuCSFsyUZyTi0DkPJ3xWpKhcoIsAfQzzYdTlo4KYZimY
hideInToc: true
---

---
layout: iframe
url: //player.bilibili.com/player.html?aid=786400485&bvid=BV1214y1X7Aq&cid=1211541734
hideInToc: true
---

---

# LangChain 核心模块概览

基于 [LangChain Modules Overview](https://embeds.onemodel.app/d/iframe/IDSmkjp32ePfdFqKcuCSFsyUZyTi0DkPJ3xWpKhcoIsAfQzzYdTlo4KYZimY) 模块架构图一起来认识一下六大模块，参见 [官方文档](https://docs.langchain.com/docs/category/components)

###### 201 教程的关注模块

- <B>Model I/O</B>：管理大语言模型（[Models](https://docs.langchain.com/docs/components/models/)），及其输入（[Prompts](https://docs.langchain.com/docs/components/prompts/)）和格式化输出（[Output Parsers](https://docs.langchain.com/docs/components/prompts/output-parser)）
  - <mdi-cable-data /> 数据流：（Parser 生成的输出格式提示 ➡️）Prompt ➡️ Model ➡️ Output Parser
- <B>Data Connection</B>：管理主要用于建设私域知识（库）的向量数据存储（[Vector Stores](https://docs.langchain.com/docs/components/indexing/vectorstore)）、内容数据获取（[Document Loaders](https://docs.langchain.com/docs/components/indexing/document-loaders)）和转化（[Transformers](https://docs.langchain.com/docs/components/indexing/text-splitters)），以及向量数据查询（[Retrievers](https://docs.langchain.com/docs/components/indexing/retriever)）
  - <mdi-cable-data /> 数据流：Loaders ➡️（Transformers ➡️）Embeddings Model ➡️ Vector Stores ➡️ Retrievers
- <B>Memory</B>：用于存储和获取 [对话历史记录](https://docs.langchain.com/docs/components/memory/) 的功能模块

<br/>

###### 202 教程的关注模块

- <B>Chains</B>：用于串联 Memory ↔️ Model I/O ↔️ Data Connection，以实现 [串行化](https://docs.langchain.com/docs/components/chains/) 的连续对话、推测流程
- <B>Agents</B>：基于 Chains [进一步串联](https://docs.langchain.com/docs/components/agents/) 工具（[Tools](https://docs.langchain.com/docs/components/agents/tool)），从而将大语言模型的能力和本地、云服务能力结合
- <B>Callbacks</B>：提供了一个回调系统，可连接到 LLM 申请的各个阶段，便于进行日志记录、追踪等数据导流

---
src: ../../pages/playground/langchain.md
---

---
layout: quote
---

# Model I/O · 选择你的模型

同时也准备好你的输入、设定好输出的格式（结构）

---
level: 2
---

# Model I/O 三元组

- Prompts：模板化、动态选择和管理模型输入
- Language Models：通过通用接口调用语言模型
- Output Parsers：从模型输出中提取信息

![](https://js.langchain.com/assets/images/model_io-1f23a36233d7731e93576d6885da2750.jpg)

---
level: 2
---

# Models：一切的缘起

在 LangChain 中，把模型分成三类：LLM 大语言模型，Chat 对话模型，Embeddings 嵌入模型

- [LLM](https://js.langchain.com/docs/modules/model_io/models/llms/)：将文本字符串作为输入，并返回续写文本的应用模型

<Val id="webup.modelSampleLLMCall" />

---
level: 2
---

# Models：一切的缘起 <sub>(cont.)</sub>

在 LangChain 中，把模型分成三类：LLM 大语言模型，Chat 对话模型，Embeddings 嵌入模型
  
- [Chat](https://js.langchain.com/docs/modules/model_io/models/chat/)：基于 LLM 支持并将聊天消息序列作为输入，并返回聊天消息的应用模型

<Val id="webup.modelSampleChatCall" />

---
level: 2
---

# Models 提高多个通用接口

通用接口也支持批处理调用，并返回更丰富的响应内容

<Val id="webup.modelSampleLLMGenerate" height="90%" />

---
level: 2
---

# Prompts 的模板能力

[Prompt Template](https://js.langchain.com/docs/modules/model_io/prompts/prompt_templates/) 是一个带标记的文本字符串，可以接收来自最终用户的一组参数并生成提示

<Val id="webup.promptSampleTemplates" />

---
level: 2
---

# Prompts 的模板能力：Partial

模板也提供 [Partial](https://js.langchain.com/docs/modules/model_io/prompts/prompt_templates/partial) 的形式，用于分阶段的注入参数（变量内容）

<Val id="webup.promptSampleTemplatesPartial" />

---
level: 2
---

# Prompts 模板的组合

可以通过 [Pipeline](https://js.langchain.com/docs/modules/model_io/prompts/prompt_templates/prompt_composition) 来实现组合，它是一组提示模板，其中的每个模板将格式化，并按最终模板格式进行组合

<Val id="webup.promptSampleTemplatesPipeline" />

---
level: 2
---

# Prompts 的示例选择器

当您有大量示例时，可能需要选择哪些要包含在提示中，示例选择器是负责执行此操作的工具

如下示例选择器 [根据长度选择](https://js.langchain.com/docs/modules/model_io/prompts/example_selectors/length_based) 要使用的示例，当我们担心构建的提示会超过上下文窗口的长度时，这非常有用

<Val id="webup.promptSampleSelectorLength" />

---
level: 2
---

# Output Parsers 结构化输出：列表

如下示例中，我们将模型的输出解析为 [具有特定长度和分隔符的列表](https://js.langchain.com/docs/modules/model_io/output_parsers/custom_list_parser)

<Val id="webup.parserSampleListCustom" />

---
level: 2
---

# Output Parsers 结构化输出：JSON（粗粒度）

如下示例中，我们将模型的输出解析为一个 [包含多个字段的 JSON 对象](https://js.langchain.com/docs/modules/model_io/output_parsers/structured)

<Val id="webup.parserSampleJSON" />

---
level: 2
---

# Output Parsers 结构化输出：JSON（细粒度）

如下示例中，我们将模型的输出解析为一个结构被严格要求的 JSON 对象（<NPM id="zod">Zod</NPM> 格式）

<Val id="webup.parserSampleJSONZod" />

---
level: 2
---

# Expression Language <Version>0.0.121</Version>: LLM Chain

现在可以用管道直接把 Prompt ➡️ Model ➡️ Output Parser 串联起来

<Val id="webup.pipeSampleLLM" />

---
layout: quote
---

# Data Connection · 准备你的数据

获取数据，整理数据，存储数据，查询数据

---
level: 2
---

# Data Connection 的核心组件

- Document Loaders & Transformers：从许多不同的源加载文档；然后拆分文档、删除冗余文档等
- Embedding Models：获取非结构化文本并将其转换为向量数据
- Vector Stores：存储和搜索向量数据
- Retrievers：从 Vector Stores 和其它数据源查询您的数据

![](https://js.langchain.com/assets/images/data_connection-c42d68c3d092b85f50d08d4cc171fc25.jpg)

---
level: 2
---

# Document Loaders 的丰富数据源

Document Loaders 从文档源加载数据，并将数据转化成“文档”格式（一段文本和关联的元数据）

数据源可以是 [本地文件](https://js.langchain.com/docs/modules/data_connection/document_loaders/integrations/file_loaders/)，也可以是 [在线文件](https://js.langchain.com/docs/modules/data_connection/document_loaders/integrations/web_loaders/)；在如下示例中，我们分别从 GitHub 和网页加载我们教程的内容

<Val id="webup.loaderSampleWeb" />

---
level: 2
---

# Document Transforms 的文本切分

文本切分的主要方式大体包括 “[按字符](https://js.langchain.com/docs/modules/data_connection/document_transformers/text_splitters/recursive_text_splitter)”、“[按 Token](https://js.langchain.com/docs/modules/data_connection/document_transformers/text_splitters/token)”、“[按代码](https://js.langchain.com/docs/modules/data_connection/document_transformers/text_splitters/code_splitter)”；在如下示例中，我们主要演示前两种切分方式

<Val id="webup.splitterSampleCharToken" height="90%" />

---
layout: iframe
url: //player.bilibili.com/player.html?aid=489796063&bvid=BV1mN411z7EZ&cid=1234039938
hideInToc: true
---

---
level: 2
---

# Vector Stores 向量存储

存储和搜索非结构化数据的最常见方法之一是嵌入它并存储生成的嵌入向量

[向量存储](https://js.langchain.com/docs/modules/data_connection/vectorstores/) 负责存储嵌入式数据并为您执行向搜索 —— 查询“最相似”的嵌入向量；向量由 [Embeddings](https://js.langchain.com/docs/modules/data_connection/text_embedding/) 模型生成

<Val id="webup.getVectorStoreBuilder" />

---
level: 2
---

# 向量存储的相似度搜索

在如下示例中，我们展示 [Pinecone 云服务](https://js.langchain.com/docs/modules/data_connection/vectorstores/integrations/pinecone) 向量存储的使用（写入和搜索）

<Val id="webup.vectorStoreSamplePinecone" />

---
level: 2
---

# Retrievers 从各个文档源检索

[Retrievers](https://js.langchain.com/docs/modules/data_connection/retrievers/) 比向量存储更通用，它不需要能够存储文档，只需返回（或检索）文档

- 向量存储是最常用的文档源，但也可以从其它本地、云上的文档源进行数据的检索
- [Self-Query Retriever](https://js.langchain.com/docs/modules/data_connection/retrievers/how_to/self_query/)：顾名思义，这是一种具有查询自身能力的检索器
  - 即给定任何自然语言查询，Retriever 通过构造 LLM Chain 编写结构化查询，并将其应用于底层向量存储

![](https://s2.loli.net/2023/07/30/WQSGv8iDU9w4Fy6.jpg)

---
level: 2
---

# 🌰 Self-Query Retriever

<Val id="webup.retrieverSampleSelfQuery" height="90%" />

---
level: 2
---

# Retrievers 从各个文档源检索（续）

检索的一项挑战是，通常不知道将数据引入系统时，文档存储系统将面临哪些特定查询

- 与查询最相关的信息可能被隐藏在包含大量不相关文本中，传递完整文档成本更高且响应较差
- [Contextual Compression Retriever](https://js.langchain.com/docs/modules/data_connection/retrievers/how_to/contextual_compression/)：上下文压缩旨在解决此问题
  - 使用给定查询的上下文来压缩它们，以便只返回相关信息，而不是立即按原样返回检索到的文档

![](https://s2.loli.net/2023/07/30/HQx5JIGrEVPfz8R.jpg)

---
level: 2
---

# Expression Language <Version>0.0.121</Version>: LLM Chain + Retriever

可以用管道直接把 Prompt ➡️ Model ➡️ Output Parser ➡️ Retriever 串联起来

<Val id="webup.pipeSampleLLMRetriever" />

---
level: 2
---

# Expression Language <Version>0.0.121</Version>: LLM Chain + Retriever

在上个示例中，我们将字符串输入直接传递到链路中；如希望链路接受多个输入，可以传递函数映射来解析输入

<Val id="webup.pipeSampleLLMRetrieverInputs" />

---
level: 2
---

# Expression Language <Version>0.0.121</Version>: LLM Chain + Retriever

在上个示例中，我们使链路接受了多个输入；我们还可以通过格式化函数添加对话历史记录

<Val id="webup.pipeSampleLLMRetrieverConversation" />

---
layout: quote
---

# Memory · 记录你的对话

对话记录是保持链路状态的基石

---
level: 2
---

# Memory 对话状态存储

Memory 负责在用户与大语言模型的交互过程中保留状态

- [Memory](https://js.langchain.com/docs/modules/memory/) 的作用可以归结为从一系列聊天消息中摄取、捕获、转换和提取知识
- Memory 可以返回多条信息（例如最近的 N 条消息和摘要）或者所有以前的消息

<Val id="webup.getMemoryBuilder" height="70%" />

---
level: 2
---

# Memory 的多样化存取方式

Memory 不仅可以整存整取，还可以利用大语言模型加工并返回处理后的内容

- 通过 LLM 模型对记录内容进行总结

<Val id="webup.memorySampleSummary" />

---
level: 2
---

# Memory 的多样化存取方式：Vector Store

通过 Vector Store 也可以对记录内容进行相似度匹配后返回

<Val id="webup.memorySampleVector" />

---
src: ../../pages/common/refs.md
---

---
src: ../../pages/common/end.md
---
