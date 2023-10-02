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

<Toc maxDepth="2" listClass="!list-disc" columns="2" />

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

在 JS/TS 中可使用 [`RunnableBranch`](https://js.langchain.com/docs/expression_language/how_to/routing) 来构建条件路由，路由会依次匹配条件直至选择默认 Runnable 返回：

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

[Fallback](https://js.langchain.com/docs/guides/fallbacks) 机制可以提供优雅的异常处理，通过“以优充次”的方式尽可能保障链路执行继续执行

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
layout: quote
---

# Chains

LangChain 为“链式”应用提供了 Chain 接口

---
level: 2
---

# 链路调试的“文韬武略”

除了接入强力的 LangSmith，也可以使用 Console 输出调试信息

和调试 Runnable Sequence 一样，首选的调试方式肯定还是接入 LangSmith，有两种方式：

```sh
export LANGCHAIN_TRACING_V2=true
export LANGCHAIN_ENDPOINT=https://api.smith.langchain.com
export LANGCHAIN_API_KEY=<your-api-key>  # still in closed beta
export LANGCHAIN_PROJECT=<your-project>  # if not specified, defaults to "default"
```
```ts {4-}
import { Client } from "langsmith";
import { LangChainTracer } from "langchain/callbacks";

const client = new Client({ apiUrl: "https://api.smith.langchain.com", apiKey: "YOUR_API_KEY" });
const tracer = new LangChainTracer({ projectName: "YOUR_PROJECT_NAME", client });

await model.invoke("Hello, world!", { callbacks: [tracer] })
```

退而求其次，可以开启 `verbose` 选项，有两种方式：

```sh
export LANGCHAIN_VERBOSE=true
```
```ts
const chain = new ConversationChain({ llm: chat, verbose: true });
```

---
level: 2
---

# 一条链路贯天地

类似 Runnable Sequence 的链路构建，但相比之下 [Seqential](https://js.langchain.com/docs/modules/chains/foundational/sequential_chains) 操作的复杂度更加高

- `SimpleSequentialChain`：最简单形式，每个步骤都有单个输入/输出，一个步骤的输出是下一个的输入
- `SequentialChain`：顺序链的更通用形式，允许多个输入/输出

```ts {12-13,21-22|24-|all} {maxHeight:'70%'}
import { SequentialChain, LLMChain } from "langchain/chains";
import { OpenAI } from "langchain/llms/openai";
import { PromptTemplate } from "langchain/prompts";

// This is an LLMChain to write a synopsis given a title of a play and the era it is set in.
const llm = new OpenAI({ temperature: 0 });
const template = `You are a playwright. Given the title of play and the era it is set in, it is your job to write a synopsis for that title.

  Title: {title}
  Era: {era}
  Playwright: This is a synopsis for the above play:`;
const promptTemplate = new PromptTemplate({template, inputVariables: ["title", "era"] });
const synopsisChain = new LLMChain({ llm, prompt: promptTemplate, outputKey: "synopsis" });

// This is an LLMChain to write a review of a play given a synopsis.
const reviewTemplate = `You are a play critic from the New York Times. Given the synopsis of play, it is your job to write a review for that play.
  
  Play Synopsis:
  {synopsis}
  Review from a New York Times play critic of the above play:`;
const reviewPromptTemplate = new PromptTemplate({ template: reviewTemplate, inputVariables: ["synopsis"] });
const reviewChain = new LLMChain({ llm, prompt: reviewPromptTemplate, outputKey: "review" });

const overallChain = new SequentialChain({
  chains: [synopsisChain, reviewChain],     // 串联两个 LLMChain
  inputVariables: ["era", "title"],         // 定义需要传入的变量
  outputVariables: ["synopsis", "review"],  // 定义需要输出的变量
});
const chainExecutionResult = await overallChain.call({ title: "Tragedy at sunset on the beach", era: "Victorian England" });
```

---
layout: iframe
url: https://smith.langchain.com/public/63376249-1360-4cea-9886-da72b3abbfd4/r
---

---
level: 2
---

# 两类模型舞前沿

最基础的链式调用是 [`LLMChain`](https://js.langchain.com/docs/modules/chains/foundational/llm_chain)，它同时支持 LLM 和 Chat 模型

```ts
/* 构建基于 LLM 模型的 LLMChain */
const model = new OpenAI({ temperature: 0 });
const prompt = PromptTemplate.fromTemplate("What is a good name for a company that makes {product}?");
const chainA = new LLMChain({ llm: model, prompt });

// 通过 .call(object) 返回输出对象
const resA = await chainA.call({ product: "colorful socks" });  // { text: '\n\nSocktastic!' }
// 通过 .run(string) 返回输出字符串
const resA2 = await chainA.run("colorful socks");   // '\n\nSocktastic!'
```
<br/>

```ts
/* 构建基于 Chat 模型的 LLMChain */
const chat = new ChatOpenAI({ temperature: 0 });
const chatPrompt = ChatPromptTemplate.fromMessages([
  ["system", "You are a helpful assistant that translates {input_language} to {output_language}."],
  ["human", "{text}"],
]);
const chainB = new LLMChain({ prompt: chatPrompt, llm: chat });

const resB = await chainB.call({ input_language: "English", output_language: "French", text: "I love programming." });
// { text: "J'adore la programmation." }
```

---
level: 2
---

# 三种巧思治文档

LangChain 提供处理文档的工具链，它们对于总结文档、回答文档问题、从文档中提取信息等很有用

[三种文档处理工具链](https://js.langchain.com/docs/modules/chains/document/) 的使用方式基本是一样的，如下所示：

```ts {10-}
import { OpenAI } from "langchain/llms/openai";
import { loadQAStuffChain, loadQAMapReduceChain } from "langchain/chains";
import { Document } from "langchain/document";

const docs = [
  new Document({ pageContent: "Harrison went to Harvard." }),
  new Document({ pageContent: "Ankush went to Princeton." }),
];

/* 构建并使用 StuffDocumentsChain */
const llmA = new OpenAI({});
const chainA = loadQAStuffChain(llmA);
const resA = await chainA.call({ input_documents: docs, question: "Where did Harrison go to college?" });

/* 构建并使用 MapReduceChain */
const llmB = new OpenAI({ maxConcurrency: 10 });
const chainB = loadQAMapReduceChain(llmB);
const resB = await chainB.call({ input_documents: docs, question: "Where did Harrison go to college?" });
```

---
level: 3
---

<img src="https://js.langchain.com/assets/images/stuff-818da4c66ee17911bc8861c089316579.jpg" width="400" />
<img src="https://js.langchain.com/assets/images/map_reduce-c65525a871b62f5cacef431625c4d133.jpg" width="800" />

---
level: 3
---

# 层叠递进的 Refine 模式

- <B>对于每个文档，它将所有非文档输入、当前文档以及最新的中间答案传递给 LLM 链以获得新的答案</B>
- 由于 Refine 链一次仅将单个文档传递给 LLM，因此很适合需要分析的文档数量多于模型上下文的任务
- 明显的缺点是：将比 Stuff 文档链进行更多的 LLM 调用；当文档频繁地互相交叉引用时很可能表现不佳

![](https://js.langchain.com/assets/images/refine-a70f30dd7ada6fe5e3fcc40dd70de037.jpg)

---
level: 3
---

# 🌰 Refine 文档链的二阶段提示词应用

```ts {8-18|20-34|36-39|all} {maxHeight:'90%'}
import { loadQARefineChain } from "langchain/chains";
import { OpenAI } from "langchain/llms/openai";
import { TextLoader } from "langchain/document_loaders/fs/text";
import { MemoryVectorStore } from "langchain/vectorstores/memory";
import { OpenAIEmbeddings } from "langchain/embeddings/openai";
import { PromptTemplate } from "langchain/prompts";

/* 最终提问时使用的提示词 */
export const questionPromptTemplateString = `Context information is below.
---------------------
{context}
---------------------
Given the context information and no prior knowledge, answer the question: {question}`;

const questionPrompt = new PromptTemplate({
  inputVariables: ["context", "question"],
  template: questionPromptTemplateString,
});

/* 中间 Refine 过程使用的提示词 */
const refinePromptTemplateString = `The original question is as follows: {question}
We have provided an existing answer: {existing_answer}
We have the opportunity to refine the existing answer
(only if needed) with some more context below.
------------
{context}
------------
Given the new context, refine the original answer to better answer the question.
You must provide a response, either original answer or refined answer.`;

const refinePrompt = new PromptTemplate({
  inputVariables: ["question", "existing_answer", "context"],
  template: refinePromptTemplateString,
});

/* 构建 Refine 文档链，并导入两份提示词模板 */
const embeddings = new OpenAIEmbeddings();
const model = new OpenAI({ temperature: 0 });
const chain = loadQARefineChain(model, { questionPrompt, refinePrompt });

// Load the documents and create the vector store
const loader = new TextLoader("./state_of_the_union.txt");
const docs = await loader.loadAndSplit();
const store = await MemoryVectorStore.fromDocuments(docs, embeddings);

// Select the relevant documents
const question = "What did the president say about Justice Breyer";
const relevantDocs = await store.similaritySearch(question);

const res = await chain.call({ input_documents: relevantDocs, question });
```

---
level: 2
---

# 四套应用展威力：Retrieval QA

[Retrival QA Chain](https://js.langchain.com/docs/modules/chains/popular/vector_db_qa) 通过从检索器检索文档，然后使用文档工具链，根据检索到的文档回答问题

```ts {18-22|24-|all} {maxHeight:'85%'}
import { OpenAI } from "langchain/llms/openai";
import { RetrievalQAChain, loadQAStuffChain } from "langchain/chains";
import { HNSWLib } from "langchain/vectorstores/hnswlib";
import { OpenAIEmbeddings } from "langchain/embeddings/openai";
import { RecursiveCharacterTextSplitter } from "langchain/text_splitter";
import { PromptTemplate } from "langchain/prompts";
import * as fs from "fs";

const promptTemplate = `Use the following pieces of context to answer the question at the end. If you don't know the answer, just say that you don't know, don't try to make up an answer.

{context}

Question: {question}
Answer in Italian:`;
const prompt = PromptTemplate.fromTemplate(promptTemplate);
const model = new OpenAI({});

/* 构建文档检索器 */
const text = fs.readFileSync("state_of_the_union.txt", "utf8");
const textSplitter = new RecursiveCharacterTextSplitter({ chunkSize: 1000 });
const docs = await textSplitter.createDocuments([text]);
const vectorStore = await HNSWLib.fromDocuments(docs, new OpenAIEmbeddings());

/* 构建基于 Stuff 文档链和自定义提示词的 Retrieval QA Chain */
const chain = new RetrievalQAChain({
  combineDocumentsChain: loadQAStuffChain(model, { prompt }),
  retriever: vectorStore.asRetriever(),
  returnSourceDocuments: true,  // 要求一并返回原始文档
});
const res = await chain.call({ query: "What did the president say about Justice Breyer?" });
```

---
level: 2
---

# 四套应用展威力：Converational Retrieval QA

[Conversational Retrieval QA Chain](https://js.langchain.com/docs/modules/chains/popular/chat_vector_db) 建立在 Retrieval QA Chain 的基础上，并接入 Memory 组件

它首先将聊天历史记录和问题组合成一个独立的问题，然后将检索得到的文档和问题传递给问答链以返回响应

```ts {35-49|51-|all} {maxHeight:'75%'}
import { ChatOpenAI } from "langchain/chat_models/openai";
import { ConversationalRetrievalQAChain } from "langchain/chains";
import { HNSWLib } from "langchain/vectorstores/hnswlib";
import { OpenAIEmbeddings } from "langchain/embeddings/openai";
import { BufferMemory } from "langchain/memory";

const CUSTOM_QUESTION_GENERATOR_CHAIN_PROMPT = `Given the following conversation and a follow up question, return the conversation history excerpt that includes any relevant context to the question if it exists and rephrase the follow up question to be a standalone question.
Chat History:
{chat_history}
Follow Up Input: {question}
Your answer should follow the following format:
\`\`\`
Use the following pieces of context to answer the users question.
If you don't know the answer, just say that you don't know, don't try to make up an answer.
----------------
<Relevant chat history excerpt as context here>
Standalone question: <Rephrased question here>
\`\`\`
Your answer:`;

const model = new ChatOpenAI({ temperature: 0 });

const vectorStore = await HNSWLib.fromTexts(
  [
    "Mitochondria are the powerhouse of the cell",
    "Foo is red",
    "Bar is red",
    "Buildings are made out of brick",
    "Mitochondria are made of lipids",
  ],
  [{ id: 2 }, { id: 1 }, { id: 3 }, { id: 4 }, { id: 5 }],
  new OpenAIEmbeddings()
);

/* 构建 Conversational Retrieval QA Chain */
const chain = ConversationalRetrievalQAChain.fromLLM(
  model,
  vectorStore.asRetriever(),
  /* 导入 Memory 和自定义提示词模板 */
  {
    memory: new BufferMemory({
      memoryKey: "chat_history",
      returnMessages: true,
    }),
    questionGeneratorChainOptions: {
      template: CUSTOM_QUESTION_GENERATOR_CHAIN_PROMPT,
    },
  }
);

const res1 = await chain.call({ question: "I have a friend called Bob. He's 28 years old. He'd like to know what the powerhouse of the cell is?" });
// { text: "The powerhouse of the cell is the mitochondria." }

const res2 = await chain.call({ question: "How old is Bob?" });
// { text: "Bob is 28 years old." }
```

---
layout: iframe
url: https://smith.langchain.com/public/517b2110-349f-4089-8051-9b5203e34c0b/r
---

---
layout: iframe
url: https://smith.langchain.com/public/69a66e55-e00d-4aae-9953-8e42f4feaeea/r
---

---
level: 2
---

# 四套应用展威力：SQL QA

利用 LLM 的 SQL 生成能力，可以构建 [SQL Chain](https://js.langchain.com/docs/modules/chains/popular/sqlite) 来进行面向数据库的问答

```ts {24-26|28-36|all} {maxHeight:'80%'}
import { DataSource } from "typeorm";
import { OpenAI } from "langchain/llms/openai";
import { SqlDatabase } from "langchain/sql_db";
import { SqlDatabaseChain } from "langchain/chains/sql_db";
import { PromptTemplate } from "langchain/prompts";

const template = `Given an input question, first create a syntactically correct {dialect} query to run, then look at the results of the query and return the answer.
Use the following format:

Question: "Question here"
SQLQuery: "SQL Query to run"
SQLResult: "Result of the SQLQuery"
Answer: "Final answer here"

Only use the following tables:

{table_info}

If someone asks for the table foobar, they really mean the employee table.

Question: {input}`;
const prompt = PromptTemplate.fromTemplate(template);

/* 构建数据库连接 */
const appDataSource = new DataSource({ type: "sqlite", database: "data/Chinook.db" });
const db = await SqlDatabase.fromDataSourceParams({ appDataSource });

/* 构建并执行 SQL QA Chain */
const chain = new SqlDatabaseChain({
  llm: new OpenAI({ temperature: 0 }),
  database: db,
  sqlOutputKey: "sql",  // 要求返回 SQL 语句
  prompt,               // 使用自定义提示词
});
await chain.call({ query: "How many employees are there in the foobar table?" });
/*
  {
    result: ' There are 8 employees in the foobar table.',
    sql: ' SELECT COUNT(*) FROM Employee;'
  }
*/
```

---
layout: iframe
url: https://smith.langchain.com/public/89aaa750-4115-4ad0-a987-39ea0b2e52f4/r
---

---
level: 3
---

# 用 Runnable Sequence 实现 SQL QA

```ts {38-51} {maxHeight:'90%'}
import { DataSource } from "typeorm";
import { SqlDatabase } from "langchain/sql_db";
import { RunnableSequence } from "langchain/schema/runnable";
import { PromptTemplate } from "langchain/prompts";
import { StringOutputParser } from "langchain/schema/output_parser";
import { ChatOpenAI } from "langchain/chat_models/openai";

const datasource = new DataSource({ type: "sqlite", database: "Chinook.db" });
const db = await SqlDatabase.fromDataSourceParams({ appDataSource: datasource });

const prompt =
  PromptTemplate.fromTemplate(`Based on the table schema below, write a SQL query that would answer the user's question:
{schema}

Question: {question}
SQL Query:`);

const model = new ChatOpenAI();

const sqlQueryGeneratorChain = RunnableSequence.from([
  {
    schema: async () => db.getTableInfo(),
    question: (input: { question: string }) => input.question,
  },
  prompt,
  model.bind({ stop: ["\nSQLResult:"] }),
  new StringOutputParser(),
]);

const finalResponsePrompt =
  PromptTemplate.fromTemplate(`Based on the table schema below, question, sql query, and sql response, write a natural language response:
{schema}

Question: {question}
SQL Query: {query}
SQL Response: {response}`);

const fullChain = RunnableSequence.from([
  {
    question: (input) => input.question,
    query: sqlQueryGeneratorChain,
  },
  {
    schema: async () => db.getTableInfo(),
    question: (input) => input.question,
    query: (input) => input.query,
    response: (input) => db.run(input.query),
  },
  finalResponsePrompt,
  model,
]);

const finalResponse = await fullChain.invoke({ question: "How many employees are there?" });
```

---
layout: iframe
url: https://smith.langchain.com/public/6d79dcfb-e501-45f5-ad31-0d605233627b/r
---

---
level: 2
---

# 四套应用展威力：Web API

[API Chain](https://js.langchain.com/docs/modules/chains/popular/api) 允许使用 LLM 与 API 交互以检索相关信息，通过提供与提供的 API 文档相关的问题来构建链

```ts {34-} {maxHeight:'90%'}
import { OpenAI } from "langchain/llms/openai";
import { APIChain } from "langchain/chains";

const OPEN_METEO_DOCS = `BASE URL: https://api.open-meteo.com/

API Documentation
The API endpoint /v1/forecast accepts a geographical coordinate, a list of weather variables and responds with a JSON hourly weather forecast for 7 days. Time always starts at 0:00 today and contains 168 hours. All URL parameters are listed below:

Parameter	Format	Required	Default	Description
latitude, longitude	Floating point	Yes		Geographical WGS84 coordinate of the location
hourly	String array	No		A list of weather variables which should be returned. Values can be comma separated, or multiple &hourly= parameter in the URL can be used.
daily	String array	No		A list of daily weather variable aggregations which should be returned. Values can be comma separated, or multiple &daily= parameter in the URL can be used. If daily weather variables are specified, parameter timezone is required.
current_weather	Bool	No	false	Include current weather conditions in the JSON output.
temperature_unit	String	No	celsius	If fahrenheit is set, all temperature values are converted to Fahrenheit.
windspeed_unit	String	No	kmh	Other wind speed speed units: ms, mph and kn
precipitation_unit	String	No	mm	Other precipitation amount units: inch
timeformat	String	No	iso8601	If format unixtime is selected, all time values are returned in UNIX epoch time in seconds. Please note that all timestamp are in GMT+0! For daily values with unix timestamps, please apply utc_offset_seconds again to get the correct date.
timezone	String	No	GMT	If timezone is set, all timestamps are returned as local-time and data is returned starting at 00:00 local-time. Any time zone name from the time zone database is supported. If auto is set as a time zone, the coordinates will be automatically resolved to the local time zone.
past_days	Integer (0-2)	No	0	If past_days is set, yesterday or the day before yesterday data are also returned.
start_date
end_date	String (yyyy-mm-dd)	No		The time interval to get weather data. A day must be specified as an ISO8601 date (e.g. 2022-06-30).
models	String array	No	auto	Manually select one or more weather models. Per default, the best suitable weather models will be combined.

Variable	Valid time	Unit	Description
temperature_2m	Instant	°C (°F)	Air temperature at 2 meters above ground
snowfall	Preceding hour sum	cm (inch)	Snowfall amount of the preceding hour in centimeters. For the water equivalent in millimeter, divide by 7. E.g. 7 cm snow = 10 mm precipitation water equivalent
rain	Preceding hour sum	mm (inch)	Rain from large scale weather systems of the preceding hour in millimeter
showers	Preceding hour sum	mm (inch)	Showers from convective precipitation in millimeters from the preceding hour
weathercode	Instant	WMO code	Weather condition as a numeric code. Follow WMO weather interpretation codes. See table below for details.
snow_depth	Instant	meters	Snow depth on the ground
freezinglevel_height	Instant	meters	Altitude above sea level of the 0°C level
visibility	Instant	meters	Viewing distance in meters. Influenced by low clouds, humidity and aerosols. Maximum visibility is approximately 24 km.`;

const model = new OpenAI({ modelName: "text-davinci-003" });
const chain = APIChain.fromLLMAndAPIDocs(model, OPEN_METEO_DOCS, { headers: {} });
await chain.call({ question: "What is the weather like right now in Munich, Germany in degrees Farenheit?" });
```

---
layout: iframe
url: https://smith.langchain.com/public/8d7555a1-66bc-4f03-843e-261a4f576f4d/r
---

---
level: 2
---

# 种类繁多的“预制菜”

相比 Runnable Sequence 的自定义能力强，Chain 的最大特点就是预制化程度高

> 下面以 JS/TS SDK 展示一些有特色的 Chain

<br/>

- 基于 [OpenAI Functions](https://platform.openai.com/docs/guides/gpt/function-calling) 能力底座的 Chain：
  - [`createExtractionChainFromZod`](https://js.langchain.com/docs/modules/chains/additional/openai_functions/extraction)：从输入文本和所需信息的模式中提取对象列表
  - [`createTaggingChain`](https://js.langchain.com/docs/modules/chains/additional/openai_functions/tagging)：根据模式中定义的属性来标记输入文本
  - [`createOpenAPIChain`](https://js.langchain.com/docs/modules/chains/additional/openai_functions/openapi)：基于 Open API 规范自动选择和调用 API
- [`OpenAIModerationChain`](https://js.langchain.com/docs/modules/chains/additional/moderation)：审核链基于 OpenAI Moderation 对于检测可能仇恨、暴力等的文本很有用
- [`ConstitutionalChain`](https://js.langchain.com/docs/modules/chains/additional/constitutional_chain)：自我批判链是一条确保语言模型的输出遵守一组预定义准则的链

此外，也提供用于条件路由的 Chain：
- [`MultiPromptChain`](https://js.langchain.com/docs/modules/chains/additional/multi_prompt_router)：使用多提示链创建一个问答链，选择与给定问题最相关的提示并进行回答
- [`MultiRetrievalQAChain`](https://js.langchain.com/docs/modules/chains/additional/multi_retrieval_qa_router)：该链选择与给定问题最相关的 Retrieval QA Chain，然后使用它回答问题

---
src: ../../pages/common/refs.md
---

---
src: ../../pages/common/end.md
---
