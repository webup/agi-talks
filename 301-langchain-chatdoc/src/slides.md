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
download: 'https://github.com/webup/agi-talks/raw/master/pdfs/301-langchain-chatdoc.pdf'
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

# Chat LangChain 应用解析

[Chat LangChain](https://chat.langchain.com/) Walkthrough

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
  <a href="https://github.com/webup/agi-talks/raw/master/pdfs/301-langchain-chatdoc.pdf" target="_blank" alt="Download"
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
url: https://chat.langchain.com
---

---
layout: iframe-right
url: https://blog.langchain.dev/building-chat-langchain-2/
---

# Chat LangChain 🦜️🔗

基于 LC 开发的 LC Python 文档查询应用

<carbon-logo-github /> 项目代码构成：

- [前端](https://github.com/langchain-ai/chat-langchain/tree/master/chat-langchain)：[Next.js LangChain Starter](https://github.com/vercel/ai/tree/main/examples/next-langchain)
  - 使用 [Next.js](https://nextjs.org/)、LangChain 和 [Vercel AI SDK](https://sdk.vercel.ai/) 构建 AI 驱动的聊天应用程序的入门模板
- [后端](https://github.com/langchain-ai/chat-langchain/blob/master/main.py)：[LangChain Python](https://github.com/langchain-ai/langchain) SDK + [FastAPI](https://fastapi.tiangolo.com/)

<br/>

<carbon-accessibility-color-filled /> 核心技术摘要：

- RAG（Retrieval Augumented Generation）
- [LCEL](https://python.langchain.com/docs/expression_language/)（LangChain Expression Language）
- 基于 [LangChain Indexing API](https://python.langchain.com/docs/modules/data_connection/indexing) 的持续更新
- 基于 [LangSmith](https://smith.langchain.com/) 的追踪、监控、评估

---
layout: image-right
image: https://communitykeeper-media.s3.amazonaws.com/media/images/Complete.original.png
---

# RAG 核心流程回顾

[How do domain-specific chatbots work?](https://scriv.ai/guides/retrieval-augmented-generation-overview/) 👉

建立私域数据索引（Indexing）

1⃣️ 读取 / 加载私域数据

2⃣️ 数据预处理及存储

3⃣️ 持续数据索引及存储

<hr/>

执行私域数据问答（Retrieval & Generation）

4⃣️ 基于用户提问的数据检索

5⃣️ 基于检索内容的应答生成

---
layout: two-cols-header
---

# 1⃣️ 读取 / 加载私域数据

💡 [ingest.py#L81-L84](https://github.com/langchain-ai/chat-langchain/blob/master/ingest.py#L81-L84)｜首先通过抓取相关文档网页来加载为 `Document`

::left::

```python
docs = SitemapLoader(
    "https://python.langchain.com/sitemap.xml",
    filter_urls=["https://python.langchain.com/"],
    parsing_function=langchain_docs_extractor,
    default_parser="lxml",
    bs_kwargs={
        "parse_only": SoupStrainer(
            name=("article", "title", "html", "lang", "content")
        ),
    },
    meta_function=metadata_extractor,
).load()
```

- 使用 [`SitemapLoader`](https://python.langchain.com/docs/integrations/document_loaders/sitemap) 从站点地图 XML 中抓取所有链接
  - 很多工作都是由 [langchain_docs_extractor](https://github.com/langchain-ai/chat-langchain/blob/master/parser.py) 方法完成的
  - 这是一个很棒的自定义 HTML -> 文本解析器

<br/>

::right::

```python
api_ref = RecursiveUrlLoader(
    "https://api.python.langchain.com/en/latest/",
    max_depth=8,
    extractor=simple_extractor,
    prevent_outside=True,
    use_async=True,
    check_response_status=True,
    exclude_dirs=(
        "https://api.python.langchain.com/en/latest/_sources",
        "https://api.python.langchain.com/en/latest/_modules",
    ),
).load()
```

- 使用 [`RecursiveURLLoader`](https://python.langchain.com/docs/integrations/document_loaders/recursive_url) 加载 API 参考文档
  - 这些文档内容没有非常有用的站点地图
  - 它可以从页面递归加载子链接到一定深度

---

# 2⃣️ 数据预处理及存储

💡 [ingest.py#L86-L114](https://github.com/langchain-ai/chat-langchain/blob/master/ingest.py#L86-L114)｜下面我们把文档切片并向量化后存入 Weaviate 向量存储中

```python
docs_transformed = RecursiveCharacterTextSplitter(chunk_size=4000, chunk_overlap=200).split_documents(docs + api_ref)
```

- 使用一个简单的 [`RecursiveCharacterTextSplitter`](https://python.langchain.com/docs/modules/data_connection/document_transformers/text_splitters/recursive_text_splitter) 将内容划分为大约大小相等的块，切分原因：
  - 帮助提高检索性能：如果相关文档还包含大量不相关信息，相似性搜索可能会错过相关文档
  - 使我们不必担心检索到的文档是否适合模型的上下文窗口（如 `gpt-3.5-turbo` 默认是 4K 容量）
  
<br/>

```python
client = weaviate.Client(url=WEAVIATE_URL, auth_client_secret=weaviate.AuthApiKey(api_key=WEAVIATE_API_KEY))

vectorstore = Weaviate(
    client=client,
    index_name=WEAVIATE_DOCS_INDEX_NAME,
    text_key="text",
    embedding=OpenAIEmbeddings(chunk_size=200),
    by_text=False,
    attributes=["source", "title"],
)
```

- 使用 [`OpenAIEmbeddings`](https://python.langchain.com/docs/integrations/text_embedding/openai) 进行向量化处理并存储到 [Weaviate](https://python.langchain.com/docs/integrations/vectorstores/weaviate) 向量存储中

---

# 3⃣️ 持续数据索引及存储

💡 [ingest.py#L116-L127](https://github.com/langchain-ai/chat-langchain/blob/master/ingest.py#L116-L127)｜我们通过管理索引记录让我们不必每次都从头开始重新索引所有文档

```python
record_manager = SQLRecordManager(
    f"weaviate/{WEAVIATE_DOCS_INDEX_NAME}", db_url=RECORD_MANAGER_DB_URL
)
record_manager.create_schema()

indexing_stats = index(
    transformed_docs,
    record_manager,
    vectorstore,
    cleanup="full",
    source_id_key="source",
)
```

- 我们使用 [LangChain Indexing API](https://python.langchain.com/docs/modules/data_connection/indexing) 将任何来源的文档加载到向量存储中并保持同步
  - 它使用 `RecordManager` 来跟踪对任何向量存储的写入，并处理来自同一源的文档的重复数据删除和清理
  - 目前的实现中使用了 [Supabase](https://python.langchain.com/docs/integrations/providers/supabase) PostgreSQL 作为后台的 `SQLRecordManager`

---

# 4⃣️ 基于用户提问的数据检索

💡 [main.py#L107-L127](https://github.com/langchain-ai/chat-langchain/blob/master/main.py#L115-L126)｜准备好用户的提问，构建数据检索的链路

```python
condense_question_chain = (
    PromptTemplate.from_template(REPHRASE_TEMPLATE)
    | llm
    | StrOutputParser()
).with_config(
    run_name="CondenseQuestion",
)
retriever_chain = condense_question_chain | retriever
```

- 我们讲用户提出的问题与当前聊天会话中过去的消息结合起来，并编写一个独立的搜索查询
  - 🌰 如果用户询问 “我如何使用 Anthropic LLM” 并接着问 “Vertex AI 怎么样”，我们可以将最后一个问题重写为 “我如何使用 Vertex AI LLM” 并使用它来查询检索器而不是 “Vertex AI 怎么样”
  - 💡 可以在 [LangChain Hub](https://smith.langchain.com/hub/bagatur/chat-langchain-rephrase) 中和调试用于改写问题的提示词

---
layout: iframe
url: https://smith.langchain.com/hub/bagatur/chat-langchain-rephrase?organizationId=e9dbe5e8-c426-4df0-a8a3-da8a1fc82968
---

---

# 5⃣️ 基于检索内容的应答生成

💡 [main.py#L138-L164](https://github.com/langchain-ai/chat-langchain/blob/master/main.py#L138-L164)｜最后我们将原始问题、聊天历史记录和检索到的上下文传递给大语言模型继续应答生成

```python {1-7|8-14|16-|all}
_context = RunnableMap(
    {
        "context": retriever_chain | format_docs,
        "question": itemgetter("question"),
        "chat_history": itemgetter("chat_history"),
    }
).with_config(run_name="RetrieveDocs")
prompt = ChatPromptTemplate.from_messages(
    [
        ("system", RESPONSE_TEMPLATE),
        MessagesPlaceholder(variable_name="chat_history"),
        ("human", "{question}"),
    ]
)

response_synthesizer = (prompt | llm | StrOutputParser()).with_config(
    run_name="GenerateResponse",
)
answer_chain = _context | response_synthesizer
```

- 问答链的 [提示词](https://smith.langchain.com/hub/bagatur/chat-langchain-response) 中会提示 LLM 输出引用来源，目的是：1) 尝试减轻幻觉，2) 让用户能够自行探索相关文档

---
layout: iframe
url: https://smith.langchain.com/hub/bagatur/chat-langchain-response
---

---

# 🔀 输出增强：流式输出

💡 [main.py#L204-L220](https://github.com/langchain-ai/chat-langchain/blob/master/main.py#L204-L220)｜我们还希望最小化用户首次获得应答文本的时间，或者说 “打字机” 输出效果

```python
stream = answer_chain.astream_log(
    {
        "question": question,
        "chat_history": converted_chat_history,
    },
    config={"metadata": metadata},
    include_names=["FindDocs"],
    include_tags=["FindDocs"],
)
return StreamingResponse(
    transform_stream_for_client(stream),
    headers={"Content-Type": "text/event-stream"},
)
```

- 我们使用 [`astream_log`](https://python.langchain.com/docs/expression_language/interface#async-stream-intermediate-steps) 方法将来自问答链的响应异步地流式地传输到 Web 客户端
  - `astream_log` 的优点是除了流式传输最终响应之外，还可以实时地流式输出中间步骤中产生的内容
  - 由于我们使用 LangChain [Runnables](https://python.langchain.com/docs/expression_language/) 构建了问答链，流式传输可以自然应用到链路中所有的输出内容

---
layout: image
image: https://blog.langchain.dev/content/images/2023/09/ezgif.com-gif-maker.gif
---

---

# 🎦 监控增强：聚合指标

应用上线之后，我们希望聚合和监控相关指标，以便我们可以跟踪应用程序的运行情况

通过接入 LangSmith，我们可以检查首次 Token 时间指标（TTFT）
- TTFT：Time-to-First-Token
- 该指标捕获 “将查询发送到聊天机器人应用” 与 “将第一个响应令牌发送回用户” 之间的延迟

我们可以监控跟踪中何时出现任何错误，以确保我们的聊天机器人按预期运行
- Success Rate 指标：聚合链路调用状态 —— 成功、失败、阻塞（Pending）

我们还可以记录用户反馈并保存到 LangSmith 中

```python
client.create_feedback(run_id, "user_score", score=score)

client.update_feedback(feedback_id, score=score, comment=comment)

```

---
layout: none
---

![](https://blog.langchain.dev/content/images/size/w1000/2023/09/image-44.png)
![](https://blog.langchain.dev/content/images/size/w1000/2023/09/image-45.png)

---
src: ../../pages/common/refs.md
---

---
src: ../../pages/common/end.md
---