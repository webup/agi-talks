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
download: 'https://github.com/webup/agi-talks/raw/master/pdfs/202-langchain-chains.pdf'
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

# LangChain 模块解析<sub>（中）</sub>

LangChain Chains Hands-on

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
  <a href="https://github.com/webup/agi-talks/raw/master/pdfs/202-langchain-chains.pdf" target="_blank" alt="Download"
    class="text-xl icon-btn opacity-50 !border-none !hover:text-white">
    <carbon-download />
  </a>
</div>

---
src: ../../pages/common/series.md
---

---
hideInToc: true
---

# 教程大纲

<Toc maxDepth="2" listClass="!list-disc"/>

---
layout: iframe
url: https://embeds.onemodel.app/d/iframe/cl8GryOek12pB78SZZYZCsJSJsS159pNgqjS19zQWC3tka5TXz9rqLmqXORI
hideInToc: true
---

---
layout: iframe
url: //player.bilibili.com/player.html?aid=275180495&bvid=BV1SF411C7Zn&cid=1244328447
hideInToc: true
---

---
layout: quote
level: 1
---

# Runnable Sequence

LangChain Expression Language (LCEL) 的核心

---
level: 2
---

# Runnable 对象简介

把一个个 Runnable 对象串联起来，就构成了 Runnable Sequence

<p><B>Runnable 对象的定义</B>：支持以下三个实例方法的对象</p>

- `invoke`：对输入直接进行链式调用 
- `batch`：对输入列表进行批量的链式调用
- `stream`：（流式）分块返回响应（需要 Model、Parser 等支持）

<br/>

<p><B>Runnable 对象的输入输出</B>：各有不同，并非统一（以下以 JS/TS SDK 为例）</p>

- Prompt：`Object` ➡️ `PromptValue`
- LLM Model：`String` ➡️ `String` 
- Chat Model：一组 `ChatMessage` ➡️ `ChatMessage`
- Parser：Model 的输出 ➡️ 不同 Parser 对应的数据结构
- Retriever：`String` ➡️ 一组 `Document`

---
level: 2
---

# Runnable 对象的串联

- JS/TS 中可用的实例方法：`runnable.pipe(runnable)`
- JS/TS 中可用的静态方法：`RunnableSequence.from([...runnables ])`

```ts {6-9|11-13|15-17|all}
import { PromptTemplate } from "langchain/prompts";
import { OpenAI } from "langchain/llm/openai";
import { RunnableSequence } from "langchain/schema/runnable";
import { StringOutputParser } from "langchain/schema/output_parser";

/* 准备 Runnable 对象 */
const model = new OpenAI({});
const promptTemplate = PromptTemplate.fromTemplate("Tell me a joke about {topic}");
const outputParser = new StringOutputParser();

/* 串联 Runnable 对象 */
// const chain = promptTemplate.pipe(model).pipe(outputParser);
const chain = RunnableSequence.from([promptTemplate, model, outputParser]);

/* 调用 Runnable Sequence */
const result = await chain.invoke({ topic: "bears" });
// const result = await chain.batch([{ topic: "bears" }, { topic: "dears" }]);
```

---
layout: iframe
url: https://smith.langchain.com/public/d6e1b8d9-bd25-431b-9e5a-6b852cf1e0e0/r
---

---
level: 2
---

# Runnable 对象绑定参数

- JS/TS 中可用的实例方法：`runnable.bind( ...kwargs )`

```ts {8-28|30-36|all} {maxHeight:'80%'}
import { PromptTemplate } from "langchain/prompts";
import { ChatOpenAI } from "langchain/chat_models/openai";

/* 准备 Runnable 对象 */
const prompt = PromptTemplate.fromTemplate(`Tell me a joke about {subject}`);
const model = new ChatOpenAI({});

/* 准备 Runnable 对象的附加参数 */
const functionSchema = [
  {
    name: "joke",
    description: "A joke",
    parameters: {
      type: "object",
      properties: {
        setup: {
          type: "string",
          description: "The setup for the joke",
        },
        punchline: {
          type: "string",
          description: "The punchline for the joke",
        },
      },
      required: ["setup", "punchline"],
    },
  },
];

/* 串联 Runnable 对象，同时附加参数 */
const chain = prompt.pipe(
  model.bind({
    functions: functionSchema,
    function_call: { name: "joke" },
  })
);

/* 调用 Runnable Sequence */
const result = await chain.invoke({ subject: "bears" });
```

---
layout: iframe
url: https://smith.langchain.com/public/4f429737-3bf8-4ca6-b29f-f5dcb25ee07a/r
---

---
level: 2
---

# Runnable Map

当 Runnable 对象接收的输入需要被预处理时，可以使用一个 Map 结构提供支持

- Map 中的每个属性都接收相同的参数，使用这些参数来调用各个属性对应的 Runnable 对象或函数
- 调用的返回值用于填充 Map 对象，然后将该对象传递到序列中的下一个 Runnable 对象

```ts {9-11,14-18|14-22|all} {maxHeight:'75%'}
import { PromptTemplate } from "langchain/prompts";
import { RunnableSequence } from "langchain/schema/runnable";
import { StringOutputParser } from "langchain/schema/output_parser";
import { OpenAI } from "langchain/chat_models/openai";

const prompt1 = PromptTemplate.fromTemplate(`What is the city {person} is from? Only respond with city name.`);
const prompt2 = PromptTemplate.fromTemplate(`What country is the city {city} in? Respond in {language}.`);

/* 构建第一个 Chain */
const model = new OpenAI({});
const chain = prompt1.pipe(model).pipe(new StringOutputParser());

const combinedChain = RunnableSequence.from([
  /* 将第一个 Chain 的返回值和原始输入中的部分数据进行组合 */
  {
    city: chain,
    language: (input) => input.language,
  },
  /* 组合后的对象成为第二个 Chain 的输入 */
  prompt2,
  model,
  new StringOutputParser(),
]);

const result = await combinedChain.invoke({ person: "Chairman Mao", language: "Chinese" });
```

---
layout: iframe
url: https://smith.langchain.com/public/a013b8e0-bf9a-44d2-be05-d67975b8812b/r
---

---
level: 2
---

# Runnable Passthrough

当我们要从一个长长的 Runnable Sequence 中提取最头部的输入时，可以使用 Passthrough 透传机制

```ts {11-17|24,27-33|all} {maxHeight:'80%'}
import { ChatOpenAI } from "langchain/chat_models/openai";
import { HNSWLib } from "langchain/vectorstores/hnswlib";
import { OpenAIEmbeddings } from "langchain/embeddings/openai";
import { PromptTemplate } from "langchain/prompts";
import { RunnableSequence, RunnablePassthrough } from "langchain/schema/runnable";
import { StringOutputParser } from "langchain/schema/output_parser";
import { Document } from "langchain/document";

const model = new ChatOpenAI({});

/* 为 RAG 场景准备 Retriever */
const vectorStore = await HNSWLib.fromTexts(
  ["mitochondria is the powerhouse of the cell"],
  [{ id: 1 }],
  new OpenAIEmbeddings()
);
const retriever = vectorStore.asRetriever();

const prompt = PromptTemplate.fromTemplate(`Answer the question based only on the following context:
{context}

Question: {question}`);

const serializeDocs = (docs: Document[]) => docs.map((doc) => doc.pageContent).join("\n");

const chain = RunnableSequence.from([
  /* 根据 RAG 提示词模版的需求准备上下文和问题 */
  {
    /* 将 Retriever 返回的一组文档拼接为上下文段落 */
    context: retriever.pipe(serializeDocs),
    /* 通过 Passthrough 透传获取原始输入 */
    question: new RunnablePassthrough(),
  },
  prompt,
  model,
  new StringOutputParser(),
]);

const result = await chain.invoke("What is the powerhouse of the cell?");
```

---
layout: iframe
url: https://smith.langchain.com/public/3fbc6896-9fbf-4555-aa39-a08095ce1834/r
---

---
level: 2
---

# Tool 也是一种 Runnable 对象

因此 Tool 是可以在 Runnable Sequence 中被串联的

```ts {12-16|all}
import { SerpAPI } from "langchain/tools";
import { OpenAI } from "langchain/llm/anthropic";
import { PromptTemplate } from "langchain/prompts";
import { StringOutputParser } from "langchain/schema/output_parser";

const prompt = PromptTemplate.fromTemplate(`Turn the following user input into a search query for a search engine:

{input}`);

const model = new OpenAI({});

/* 准备 Web Search 工具 */
const search = new SerpAPI();

/* 直接将 Web Search 工具传入并调用 */
const chain = prompt.pipe(model).pipe(new StringOutputParser()).pipe(search);

const result = await chain.invoke({ input: "Who is the current prime minister of Malaysia?" });
```

---
layout: iframe
url: https://smith.langchain.com/public/6a5c08ee-96d8-4c97-8cfc-038990c7260f/r
---

---
level: 2
---

# 在 Runnable Sequence 中引入 Memory

Memory 可以被添加到任何 Runnable Sequence 中，本质和就是生成上下文文本填充到提示词中

```ts {13-19|22-31|39-40|all} {maxHeight:'80%'}
import { ChatPromptTemplate, MessagesPlaceholder } from "langchain/prompts";
import { RunnableSequence } from "langchain/schema/runnable";
import { ChatAnthropic } from "langchain/chat_models/anthropic";
import { BufferMemory } from "langchain/memory";

const model = new ChatAnthropic();
const prompt = ChatPromptTemplate.fromMessages([
  ["system", "You are a helpful chatbot"],
  new MessagesPlaceholder("history"),
  ["human", "{input}"],
]);

/* 初始化 Memory 对象 */
const memory = new BufferMemory({
  returnMessages: true,
  inputKey: "input",      // by default
  outputKey: "output",    // by default
  memoryKey: "history",   // by default
});

const chain = RunnableSequence.from([
  /* 第一个 Map 用于透传原始输入和加载 Memory 对象 */ 
  {
    input: (initialInput) => initialInput.input,
    memory: () => memory.loadMemoryVariables({}),
  }, 
  /* 第二个 Map 用于继续透传原始输入和加载 Memory 的 history 部分数据 */ 
  {
    input: (previousOutput) => previousOutput.input,
    history: (previousOutput) => previousOutput.memory.history,
  },
  prompt,
  model,
]);

const inputs = { input: "Hey, I'm Bob!" };
const response = await chain.invoke(inputs);

/* 完成一次对话后进行 Memory 数据数据 */
await memory.saveContext(inputs, { output: response.content });
/*
  {
    history: [
      HumanMessage {
        content: "Hey, I'm Bob!",
        additional_kwargs: {}
      },
      AIMessage {
        content: " Hi Bob, nice to meet you! I'm Claude, an AI assistant created by Anthropic to be helpful, harmless, and honest.",
        additional_kwargs: {}
      }
    ]
  }
*/

const inputs2 = { input: "What's my name?" };
const response2 = await chain.invoke(inputs2);
```

---
level: 2
---

# 为 Runnable Sequence 提供条件路由

路由允许您创建非确定性的 Chain，其中上一步的输出定义了下一步的路由方向

在 JS/TS 中可使用 `RunnableBranch` 来构建条件路由，路由会依次匹配条件直至选择默认 Runnable 返回：

```ts {23-36|43-44|all} {maxHeight:'75%'}
const langChainChain = PromptTemplate.fromTemplate(
  `You are an expert in langchain. Always answer questions starting with "As Harrison Chase told me".
Respond to the following question:

Question: {question}
Answer:`
).pipe(model);

const anthropicChain = PromptTemplate.fromTemplate(
  `You are an expert in anthropic. Always answer questions starting with "As Dario Amodei told me". 
Respond to the following question:

Question: {question}
Answer:`
).pipe(model);

const generalChain = PromptTemplate.fromTemplate(`Respond to the following question:

Question: {question}
Answer:`
).pipe(model);

/* 构建一个条件路由，依次选择 Anthropic -> LangChain -> General Chain */
const branch = RunnableBranch.from([
  [
    (x: { topic: string; question: string }) =>
      x.topic.toLowerCase().includes("anthropic"),
    anthropicChain, 
  ],
  [
    (x: { topic: string; question: string }) =>
      x.topic.toLowerCase().includes("langchain"),
    langChainChain,
  ],
  generalChain,
]);

const fullChain = RunnableSequence.from([
  {
    topic: classificationChain,
    question: (input: { question: string }) => input.question,
  },
  /* 可以直接将路由串联进一个 Runnable Sequence 中 */
  branch,
]);
```

---
level: 2
---

# Runnable 的 Fallback 机制

Fallback 机制可以提供优雅的异常处理，通过“以优充次”的方式尽可能保障链路执行继续执行

在 JS/TS 中，可使用 `runnable.withFallbacks({ fallbacks: [runnable] })` 来构建 Fallback 支撑

```ts
const fakeOpenAIModel = new ChatOpenAI({
  modelName: "potato!",   // Fake model name will always throw an error
  maxRetries: 0,
});

const modelWithFallback = fakeOpenAIModel.withFallbacks({
  fallbacks: [new ChatAnthropic({})],
});
```

不仅可以支撑 Runnable 对象，还可以直接 Fallback 整个 Runnable Sequence：

```ts
const badChain = chatPrompt.pipe(fakeOpenAIChatModel).pipe(outputParser);
const goodChain = prompt.pipe(openAILLM).pipe(outputParser);

const chain = badChain.withFallbacks({
  fallbacks: [goodChain],
});
```

---
src: ../../pages/common/refs.md
---

---
src: ../../pages/common/end.md
---
