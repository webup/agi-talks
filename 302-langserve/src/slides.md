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
download: 'https://github.com/webup/agi-talks/raw/master/pdfs/302-langserve.pdf'
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

# LangServe: Serve LangChain

LangServe Intro and Walkthrough

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
  <a href="https://github.com/webup/agi-talks/raw/master/pdfs/302-langserve.pdf" target="_blank" alt="Download"
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
url: https://embeds.onemodel.app/d/iframe/955y5DDdxGVRezQ2ZzVL718O7iuBgmQepg1aXLS7I6JvFP793OV9GySY5UoZ
---

---
layout: iframe
url: //player.bilibili.com/player.html?aid=492239144&bvid=BV1oN41147Ak&cid=1301106610&p=1
---

---
layout: iframe-right
url: https://blog.langchain.dev/introducing-langserve/
---

# LangServe 之诞生

<Tweet id="1712526285313102091" />

---
layout: two-cols-header
---

# 使用 LangServe 构建生产可用的 Web API

Building Production-Ready Web APIs with LangServe

::left::

<mdi-code /> `my_package/chain.py`:

```py {7-}
"""A conversational retrieval chain."""

from langchain.chains import ConversationalRetrievalChain
from langchain.chat_models import ChatOpenAI
from langchain.embeddings import OpenAIEmbeddings
from langchain.vectorstores import FAISS

vectorstore = FAISS.from_texts(
    ["cats like fish", "dogs like sticks"],
    embedding=OpenAIEmbeddings()
)
retriever = vectorstore.as_retriever()

model = ChatOpenAI()

chain = ConversationalRetrievalChain.from_llm(model, retriever)
```

任意构建一个 Chain（完全可以是基于 [LCEL](https://python.langchain.com/docs/expression_language/) 构建）

<T>Build a Chain as you like (especially via LCEL)</T>

::right::

<mdi-code /> `my_package/server.py`:

```py {8-}
#!/usr/bin/env python
"""A server for the chain above."""

from fastapi import FastAPI
from langserve import add_routes

from my_package.chain import chain

app = FastAPI(title="Retrieval App")

add_routes(app, chain)

if __name__ == "__main__":
    import uvicorn

    uvicorn.run(app, host="localhost", port=8000)
```

通过 `add_routes` 方法把 Chain 注册到 Web 服务

<T>Register chain to web server via <code>add_routes</code></T>

---
layout: iframe-right
url: https://replit.com/@webup/langserve-replit-template?embed=true
---

# LangServe Replit 模板

<carbon-logo-github /> [LangServe Replit Template](https://github.com/langchain-ai/langserve-replit-template)

该模板展示了如何使用 LangServe 将 LangChain Expression Language <B>Runnable 部署为一组 HTTP 端点</B>到 [Replit](https://replit.com/) 上，并支持流和批处理

<T>This template shows how to deploy a LangChain Expression Language Runnable as a set of HTTP endpoints with stream and batch support using LangServe onto Replit.</T><br/>

###### Notice

在运行示例前，需要通过左下角的 `Tools > Secrets` 来设置 `OPENAI_AP​​I_KEY` 
<T>Need to set an <code>OPENAI_API_KEY</code> environment variable by going under <code>Tools > Secrets</code> in the bottom left corner</T>

---
layout: iframe-right
url: https://langserve-launch-example-vz4y4ooboq-uc.a.run.app/docs
---

# LangServe 特性概览

LangServe Endpoints and Features 

📚 `//docs` 通过 [Swagger UI](https://swagger.io/tools/swagger-ui/) 展示和调试 API

<T><code>//docs</code> endpoint serves API docs with Swagger UI</T>

📞 4 个“写”端点用于调用 Chain：`/invoke`，`/batch`，`/stream`，`/stream_log`

<T>4 endpoints to call your chain in various approaches</T>

🔍 2 个“读”端点用于获取 Chain 的输入输出结构：`/input_schema`，`/output_schema`

<T>2 endpoints to retrieve <a href="https://python.langchain.com/docs/expression_language/interface#input-schema" target="_blank">input</a> and <a href="https://python.langchain.com/docs/expression_language/interface#output-schema" target="_blank">output</a> schemas (<a href="https://docs.pydantic.dev/" target="_blank">Pydantic</a> models) auto-generated from the structure of the chain</T>

📂 支持在同一服务的不同路径下托管多个 Chain

<T>Support for hosting multiple chains in the same server under separate paths, e.g. <code>/chat/invoke</code>, <code>/say/invoke</code></T>

---

# 调用 LangServe 端点接口的多种方式

Calling hosted chain from various clients

```py
from langserve import RemoteRunnable

pirate_chain = RemoteRunnable("https://your_url.repl.co/chat/")

pirate_chain.invoke({"question": "how are you?"})
await pirate_chain.ainvoke({"question": "how are you?"})
```

<br/>

```js
import { RemoteRunnable } from "langchain/runnables/remote"; // Introduced in LangChain.js 0.0.166+

const pirateChain = new RemoteRunnable({ url: `https://your_url.repl.co/chat/` });
const result = await pirateChain.invoke({ "question": "what did i just say my name was?" });
```

<br/>

```sh
curl --location --request POST 'https://your_url.repl.co/chat/invoke' \
--header 'Content-Type: application/json' \
--data-raw '{
    "input": { "question": "what did i just say my name was?" }
}'
```

---

# LCEL 对于 LangServe 的重要支撑

The journey to build LangServe really started when LCEL and the Runnable protocol launched

###### Streaming Capacity

🌊 一流的流式传输支持：使用 LCEL 时，您可以获得最佳的首次 Token 时间；正在尝试支持 [流式 JSON 解析器](https://twitter.com/LangChainAI/status/1709690468030914584)

<T>First-class support for streaming: build your chains with LCEL you get the best possible time-to-first-token, streaming JSON parser is WIP</T>

🔍 流式输出中间结果：添加了对 [流式输出中间结果](https://python.langchain.com/docs/expression_language/interface#async-stream-intermediate-steps) 的支持，并且在每个 LangServe 服务上都可用

<T>Accessing intermediate results: added support for streaming intermediate results, and it’s available on every LangServe server</T>

<br/>

###### Control Flow

⚡️ 优化的并行执行：只要 LCEL 链具有 [可以并行执行](https://python.langchain.com/docs/expression_language/how_to/map) 的步骤，就会在同步和异步接口中自动并行执行此操作

<T>Optimized parallel execution: whenever LCEL chains have steps that can be executed in parallel, will do it in both sync and async interfaces</T>

🔁 支持重试和回退：为 LCEL 链的增加了 [重试和回退](https://python.langchain.com/docs/expression_language/how_to/fallbacks) 的支持；目前正在努力添加对重试/回退的流支持

<T>Retries and fallbacks: added support for any part of your LCEL chain; currently working on adding streaming support for retries/fallback</T>

---
src: ../../pages/common/refs.md
---

---
src: ../../pages/common/end.md
---
