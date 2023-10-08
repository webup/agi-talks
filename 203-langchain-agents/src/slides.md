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
download: 'https://github.com/webup/agi-talks/raw/master/pdfs/203-langchain-agents.pdf'
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

# LangChain 模块解析<sub>（下）</sub>

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
  <a href="https://github.com/webup/agi-talks/raw/master/pdfs/203-langchain-agents.pdf" target="_blank" alt="Download"
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

# Agent vs. Chain

如果说 Agent 是 Chain 的进化形态，那哪些方面是对等的？哪些地方又存在质变？

<PB>➡️ Agent 继承了 Chain 的所有能力</PB>

- Chain 的能力构成了 Agent 的 “推理链” 能力基础
  - Agent 的执行器（Executor）就是基于 LLM Chain 实现的
- Agent 自身具备和 Chain 类似的 “调用 / 执行” 接口的能力

<br/>

⬆️⬆️ Agent 在 Chain 的能力基础上<B>进化出了两个核心能力</B>

- 一条<B>思考链</B>（<Wiki id="Prompt_engineering#Chain-of-thought">Chain of Thought</Wiki>）：Agent 可以规划和调度推理链，会思考分步骤的、每一步的行动
- 一个<B>工具箱</B>（Toolbox）：Agent 自带工具体系，可以接入并调用外部工具（工具可以按既定接口规范定制）
  - Agent 只关注每个工具的输入输出（格式及内容），工具内部可以自行进行本地和 Web 接口调用

---

# Agent 的思考链模式：ReAct

[ReAct](https://react-lm.github.io/): Synergizing Reasoning and Acting in Language Models

![](https://react-lm.github.io/files/diagram.png)

ReAct 使用大语言模型同时生成推理链和行动，两者交替出现，相互支持：
- 推理链帮助 Agent 制定、跟踪和更新行动计划
- 行动让 Agent 与环境上下文交互，获取更多信息支持推理

♻️流程概览：给出任务 ➡️ 推理下一步行动 ➡️ 行动并更新环境 ➡️ 观察并推理下一步行动 ➡️ 循环直至任务完成

---
level: 2
---

# 🌰 使用 ReAct 模式的 Agent

```ts {7-16|18-21|all} {maxHeight:'90%'} 
import { initializeAgentExecutorWithOptions } from "langchain/agents";
import { OpenAI } from "langchain/llms/openai";
import { SerpAPI } from "langchain/tools";
import { Calculator } from "langchain/tools/calculator";

const model = new OpenAI({ temperature: 0 });
const tools = [
  /* Web 搜索工具 */
  new SerpAPI(process.env.SERPAPI_API_KEY, {
    location: "Austin,Texas,United States",
    hl: "en",
    gl: "us",
  }),
  /* 本地计算器工具 */
  new Calculator(),
];

const executor = await initializeAgentExecutorWithOptions(tools, model, {
  agentType: "zero-shot-react-description",
  verbose: true,
});

const input = `Who is Olivia Wilde's boyfriend? What is his current age raised to the 0.23 power?`;
const result = await executor.call({ input });
```

---
layout: iframe
url: https://smith.langchain.com/public/99f09caa-d6e4-4424-a8cd-7daa02ee5e13/r
---

---
level: 2
---

# ReAct 模式的核心提示词

```ts
export const PREFIX = `Answer the following questions as best you can. You have access to the following tools:`;

export const FORMAT_INSTRUCTIONS = `Use the following format in your response:

Question: the input question you must answer
Thought: you should always think about what to do
Action: the action to take, should be one of [{tool_names}]
Action Input: the input to the action
Observation: the result of the action
... (this Thought/Action/Action Input/Observation can repeat N times)
Thought: I now know the final answer
Final Answer: the final answer to the original input question`;

export const SUFFIX = `Begin!

Question: {input}
Thought:{agent_scratchpad}`;
```

```ts
const executor = await initializeAgentExecutorWithOptions(tools, chat, {
  agentType: "<certain-agent-type>",
  agentArgs: { prefix, suffix },  // 可自定义前置和后置提示词
});
```

---

# Agent 的思考链模式：Plan and Execute

[Plan-and-Solve Prompting](https://arxiv.org/pdf/2305.04091.pdf): Improving Zero-Shot Chain-of-Thought Reasoning by Large Language Models

顾名思义，Agent 通过首先计划要做什么，然后执行子任务来实现目标

- 这个想法很大程度上是受到 [BabyAGI](https://github.com/yoheinakajima/babyagi) 和后来的《Plan-and-Solve Prompting》论文的启发
- <B>计划几乎总是由 LLM 完成；执行通常由单独的（带有工具的）Agent 完成</B>

<br/>

该 Agent 有两个主要的关键步骤： 

1. 首先，Agent 使用 LLM 创建一个计划，以明确的步骤回答查询
2. 一旦制定了计划，它就会使用传统的 Agent（比如 ReAct）来解决每个步骤

<br/>

> 其想法是，计划步骤通过将较大的任务分解为更简单的子任务，使 LLM 更加“步入正轨”。
> 
> 但是，与传统 Agent 相比，此方法需要更多单独的 LLM 查询，并且延迟更高。

---
level: 2
---

# 🌰 使用 Plan and Execute 模式的 Agent

该模式的 Agent 目前仅支持 Chat 模型

```ts {13-16|all}
import { Calculator } from "langchain/tools/calculator";
import { SerpAPI } from "langchain/tools";
import { ChatOpenAI } from "langchain/chat_models/openai";
import { PlanAndExecuteAgentExecutor } from "langchain/experimental/plan_and_execute";

const tools = [new Calculator(), new SerpAPI()];
const model = new ChatOpenAI({
  temperature: 0,
  modelName: "gpt-3.5-turbo",
  verbose: true,
});

const executor = PlanAndExecuteAgentExecutor.fromLLMAndTools({
  llm: model,
  tools,
});

const result = await executor.call({
  input: `Who is the current president of the United States? What is their current age raised to the second power?`,
});
```

---
layout: iframe
url: https://smith.langchain.com/public/33110a27-032f-4d6d-a121-4063931d189b/r
---

---
level: 2
---

# Plan and Execute 模式的核心提示词

```ts
export const PLANNER_SYSTEM_PROMPT_MESSAGE_TEMPLATE = [
  `Let's first understand the problem and devise a plan to solve the problem.`,
  `Please output the plan starting with the header "Plan:"`,
  `and then followed by a numbered list of steps.`,
  `Please make the plan the minimum number of steps required`,
  `to answer the query or complete the task accurately and precisely.`,
  `Your steps should be general, and should not require a specific method to solve a step. If the task is a question,`,
  `the final step in the plan must be the following: "Given the above steps taken,`,
  `please respond to the original query."`,
  `At the end of your plan, say "<END_OF_PLAN>"`,
].join(" ");

export const DEFAULT_STEP_EXECUTOR_HUMAN_CHAT_MESSAGE_TEMPLATE = `Previous steps: {previous_steps}

Current objective: {current_step}

{agent_scratchpad}

You may extract and combine relevant data from your previous steps when responding to me.`;
```

---

# ReAct 模式的工具能力强化：JSON Schema

工具的输入输出限定为单个字符串 ➡️ 可以使用工具提供的 JSON Schema 接口定义来扩展输入输出

```ts {10-20|23-25|all} {maxHeight:'85%'}
import { z } from "zod";
import { ChatOpenAI } from "langchain/chat_models/openai";
import { initializeAgentExecutorWithOptions } from "langchain/agents";
import { Calculator } from "langchain/tools/calculator";
import { DynamicStructuredTool } from "langchain/tools";

const model = new ChatOpenAI({ temperature: 0 });
const tools = [
  new Calculator(), // Older existing single input tools will still work
  new DynamicStructuredTool({
    name: "random-number-generator",
    description: "generates a random number between two input numbers",
    schema: z.object({
      low: z.number().describe("The lower bound of the generated number"),
      high: z.number().describe("The upper bound of the generated number"),
    }),
    func: async ({ low, high }) =>
      (Math.random() * (high - low) + low).toString(), // Outputs still must be strings
    returnDirect: false, // This is an option that allows the tool to return the output directly
  }),
];

const executor = await initializeAgentExecutorWithOptions(tools, model, {
  agentType: "structured-chat-zero-shot-react-description",
});

const input = `What is a random number between 5 and 10 raised to the second power?`;
const result = await executor.call({ input });
```

---
layout: iframe
url: https://smith.langchain.com/public/1b23a713-58e3-42f8-93db-60fa99cb1207/r
---

---

# ReAct 模式的工具能力强化：OpenAI Functions

OpenAI 模型（如 `gpt-3.5-turbo-0613`）已经过微调，可以检测何时应调用函数并响应应传递给函数的输入

- OpenAI Functions 的目标是比通用文本完成或聊天 API 更可靠地返回有效且有用的函数调用
- 在 API 调用中，您可以描述函数并让模型智能地选择输出包含调用这些函数的参数的 JSON 对象

```ts
import { initializeAgentExecutorWithOptions } from "langchain/agents";
import { ChatOpenAI } from "langchain/chat_models/openai";
import { SerpAPI } from "langchain/tools";
import { Calculator } from "langchain/tools/calculator";

const tools = [new Calculator(), new SerpAPI()];
const chat = new ChatOpenAI({ modelName: "gpt-4", temperature: 0 });

const executor = await initializeAgentExecutorWithOptions(tools, chat, {
  agentType: "openai-functions",
  verbose: true,
});

const result = await executor.run("What is the weather in New York?");
```

---
layout: iframe
url: https://smith.langchain.com/public/f81e509f-9a51-4f0b-9554-4b351cd8ebdb/r
---

---

# ReAct 模式下的 XML Agent

某些语言模型（例如 [Anthropic](https://www.anthropic.com/) 的 Claude）特别擅长推理/编写 XML

```ts
import { ChatAnthropic } from "langchain/chat_models/anthropic";
import { initializeAgentExecutorWithOptions } from "langchain/agents";
import { SerpAPI } from "langchain/tools";

const model = new ChatAnthropic({ modelName: "claude-2", temperature: 0.1 });
const tools = [new SerpAPI()];

const executor = await initializeAgentExecutorWithOptions(tools, model, {
  agentType: "xml",
  verbose: true,
});
console.log("Loaded agent.");

const input = `What is the weather in Honolulu?`;
const result = await executor.call({ input });
```

---
layout: iframe
url: https://smith.langchain.com/public/d0acd50a-f99d-4af0-ae66-9009de319fb5/r
---

---

# Agent 的工具箱

工具是 Agent 可以用来与世界交互的功能

Agent 只关心工具的输入输出，内部实现对 Agent 透明

- 这些工具可以是通用实用程序（例如搜索）、其它 Chain 调用，甚至其它 Agent 调用 

<br/>

Agent 定义了工具的标准接口，以实现无缝集成

- 具体来说，工具的接口具有单个文本输入和单个文本输出
- 它包括名称和描述，用于向模型传达该工具的用途以及何时使用它

```ts
abstract class Tool {
  abstract name: string;
  abstract description: string;

  abstract _call(arg: string): Promise<string>;
}
```

---
level: 2
---

# 🌰 Calculator 工具的实现

```ts {all|9-12|1,17|all}
import { Parser } from "expr-eval";
import { Tool } from "./base.js";

/**
 * The Calculator class is a tool used to evaluate mathematical
 * expressions. It extends the base Tool class.
 */
export class Calculator extends Tool {
  name = "calculator";

  description = `Useful for getting the result of a math expression. 
  The input to this tool should be a valid mathematical expression that could be executed by a simple calculator.`;

  /** @ignore */
  async _call(input: string) {
    try {
      return Parser.evaluate(input).toString();
    } catch (error) {
      return "I don't know how to do that.";
    }
  }
}
```

---
level: 2
---

# Tool 的结构化输入定义

Tool 可以通过 JSON Schema 来强化输入的定义，在 JS/TS SDK 中会使用 Zod 转换 TypeScript 类型声明

如下所示，SerpAPI 工具中对参数接口进行了详细的定义：

```ts {all} {maxHeight:'80%'}
// Copied over from `serpapi` package
interface BaseParameters {
  /**
   * Parameter defines the device to use to get the results. It can be set to
   * `desktop` (default) to use a regular browser, `tablet` to use a tablet browser
   * (currently using iPads), or `mobile` to use a mobile browser (currently
   * using iPhones).
   */
  device?: "desktop" | "tablet" | "mobile";
  /**
   * Parameter will force SerpApi to fetch the Google results even if a cached
   * version is already present. A cache is served only if the query and all
   * parameters are exactly the same. Cache expires after 1h. Cached searches
   * are free, and are not counted towards your searches per month. It can be set
   * to `false` (default) to allow results from the cache, or `true` to disallow
   * results from the cache. `no_cache` and `async` parameters should not be used together.
   */
  no_cache?: boolean;
  /**
   * Specify the client-side timeout of the request. In milliseconds.
   */
  timeout?: number;
}

export interface SerpAPIParameters extends BaseParameters {
  /**
   * Search Query
   * Parameter defines the query you want to search. You can use anything that you
   * would use in a regular Google search. e.g. `inurl:`, `site:`, `intitle:`. We
   * also support advanced search query parameters such as as_dt and as_eq. See the
   * [full list](https://serpapi.com/advanced-google-query-parameters) of supported
   * advanced search query parameters.
   */
  q: string;
  /**
   * Location
   * Parameter defines from where you want the search to originate. If several
   * locations match the location requested, we'll pick the most popular one. Head to
   * [/locations.json API](https://serpapi.com/locations-api) if you need more
   * precise control. location and uule parameters can't be used together. Avoid
   * utilizing location when setting the location outside the U.S. when using Google
   * Shopping and/or Google Product API.
   */
  location?: string;
  /**
   * Encoded Location
   * Parameter is the Google encoded location you want to use for the search. uule
   * and location parameters can't be used together.
   */
  uule?: string;
  /**
   * Google Place ID
   * Parameter defines the id (`CID`) of the Google My Business listing you want to
   * scrape. Also known as Google Place ID.
   */
  ludocid?: string;
  /**
   * Additional Google Place ID
   * Parameter that you might have to use to force the knowledge graph map view to
   * show up. You can find the lsig ID by using our [Local Pack
   * API](https://serpapi.com/local-pack) or [Places Results
   * API](https://serpapi.com/places-results).
   * lsig ID is also available via a redirect Google uses within [Google My
   * Business](https://www.google.com/business/).
   */
  lsig?: string;
  /**
   * Google Knowledge Graph ID
   * Parameter defines the id (`KGMID`) of the Google Knowledge Graph listing you
   * want to scrape. Also known as Google Knowledge Graph ID. Searches with kgmid
   * parameter will return results for the originally encrypted search parameters.
   * For some searches, kgmid may override all other parameters except start, and num
   * parameters.
   */
  kgmid?: string;
  /**
   * Google Cached Search Parameters ID
   * Parameter defines the cached search parameters of the Google Search you want to
   * scrape. Searches with si parameter will return results for the originally
   * encrypted search parameters. For some searches, si may override all other
   * parameters except start, and num parameters. si can be used to scrape Google
   * Knowledge Graph Tabs.
   */
  si?: string;
  /**
   * Domain
   * Parameter defines the Google domain to use. It defaults to `google.com`. Head to
   * the [Google domains page](https://serpapi.com/google-domains) for a full list of
   * supported Google domains.
   */
  google_domain?: string;
  /**
   * Country
   * Parameter defines the country to use for the Google search. It's a two-letter
   * country code. (e.g., `us` for the United States, `uk` for United Kingdom, or
   * `fr` for France). Head to the [Google countries
   * page](https://serpapi.com/google-countries) for a full list of supported Google
   * countries.
   */
  gl?: string;
  /**
   * Language
   * Parameter defines the language to use for the Google search. It's a two-letter
   * language code. (e.g., `en` for English, `es` for Spanish, or `fr` for French).
   * Head to the [Google languages page](https://serpapi.com/google-languages) for a
   * full list of supported Google languages.
   */
  hl?: string;
  /**
   * Set Multiple Languages
   * Parameter defines one or multiple languages to limit the search to. It uses
   * `lang_{two-letter language code}` to specify languages and `|` as a delimiter.
   * (e.g., `lang_fr|lang_de` will only search French and German pages). Head to the
   * [Google lr languages page](https://serpapi.com/google-lr-languages) for a full
   * list of supported languages.
   */
  lr?: string;
  /**
   * as_dt
   * Parameter controls whether to include or exclude results from the site named in
   * the as_sitesearch parameter.
   */
  as_dt?: string;
  /**
   * as_epq
   * Parameter identifies a phrase that all documents in the search results must
   * contain. You can also use the [phrase
   * search](https://developers.google.com/custom-search/docs/xml_results#PhraseSearchqt)
   * query term to search for a phrase.
   */
  as_epq?: string;
  /**
   * as_eq
   * Parameter identifies a word or phrase that should not appear in any documents in
   * the search results. You can also use the [exclude
   * query](https://developers.google.com/custom-search/docs/xml_results#Excludeqt)
   * term to ensure that a particular word or phrase will not appear in the documents
   * in a set of search results.
   */
  as_eq?: string;
  /**
   * as_lq
   * Parameter specifies that all search results should contain a link to a
   * particular URL. You can also use the
   * [link:](https://developers.google.com/custom-search/docs/xml_results#BackLinksqt)
   * query term for this type of query.
   */
  as_lq?: string;
  /**
   * as_nlo
   * Parameter specifies the starting value for a search range. Use as_nlo and as_nhi
   * to append an inclusive search range.
   */
  as_nlo?: string;
  /**
   * as_nhi
   * Parameter specifies the ending value for a search range. Use as_nlo and as_nhi
   * to append an inclusive search range.
   */
  as_nhi?: string;
  /**
   * as_oq
   * Parameter provides additional search terms to check for in a document, where
   * each document in the search results must contain at least one of the additional
   * search terms. You can also use the [Boolean
   * OR](https://developers.google.com/custom-search/docs/xml_results#BooleanOrqt)
   * query term for this type of query.
   */
  as_oq?: string;
  /**
   * as_q
   * Parameter provides search terms to check for in a document. This parameter is
   * also commonly used to allow users to specify additional terms to search for
   * within a set of search results.
   */
  as_q?: string;
  /**
   * as_qdr
   * Parameter requests search results from a specified time period (quick date
   * range). The following values are supported:
   * `d[number]`: requests results from the specified number of past days. Example
   * for the past 10 days: `as_qdr=d10`
   * `w[number]`: requests results from the specified number of past weeks.
   * `m[number]`: requests results from the specified number of past months.
   * `y[number]`: requests results from the specified number of past years. Example
   * for the past year: `as_qdr=y`
   */
  as_qdr?: string;
  /**
   * as_rq
   * Parameter specifies that all search results should be pages that are related to
   * the specified URL. The parameter value should be a URL. You can also use the
   * [related:](https://developers.google.com/custom-search/docs/xml_results#RelatedLinksqt)
   * query term for this type of query.
   */
  as_rq?: string;
  /**
   * as_sitesearch
   * Parameter allows you to specify that all search results should be pages from a
   * given site. By setting the as_dt parameter, you can also use it to exclude pages
   * from a given site from your search resutls.
   */
  as_sitesearch?: string;
  /**
   * Advanced Search Parameters
   * (to be searched) parameter defines advanced search parameters that aren't
   * possible in the regular query field. (e.g., advanced search for patents, dates,
   * news, videos, images, apps, or text contents).
   */
  tbs?: string;
  /**
   * Adult Content Filtering
   * Parameter defines the level of filtering for adult content. It can be set to
   * `active`, or `off` (default).
   */
  safe?: string;
  /**
   * Exclude Auto-corrected Results
   * Parameter defines the exclusion of results from an auto-corrected query that is
   * spelled wrong. It can be set to `1` to exclude these results, or `0` to include
   * them (default).
   */
  nfpr?: string;
  /**
   * Results Filtering
   * Parameter defines if the filters for 'Similar Results' and 'Omitted Results' are
   * on or off. It can be set to `1` (default) to enable these filters, or `0` to
   * disable these filters.
   */
  filter?: string;
  /**
   * Search Type
   * (to be matched) parameter defines the type of search you want to do.
   * It can be set to:
   * `(no tbm parameter)`: regular Google Search,
   * `isch`: [Google Images API](https://serpapi.com/images-results),
   * `lcl` - [Google Local API](https://serpapi.com/local-results)
   * `vid`: [Google Videos API](https://serpapi.com/videos-results),
   * `nws`: [Google News API](https://serpapi.com/news-results),
   * `shop`: [Google Shopping API](https://serpapi.com/shopping-results),
   * or any other Google service.
   */
  tbm?: string;
  /**
   * Result Offset
   * Parameter defines the result offset. It skips the given number of results. It's
   * used for pagination. (e.g., `0` (default) is the first page of results, `10` is
   * the 2nd page of results, `20` is the 3rd page of results, etc.).
   * Google Local Results only accepts multiples of `20`(e.g. `20` for the second
   * page results, `40` for the third page results, etc.) as the start value.
   */
  start?: number;
  /**
   * Number of Results
   * Parameter defines the maximum number of results to return. (e.g., `10` (default)
   * returns 10 results, `40` returns 40 results, and `100` returns 100 results).
   */
  num?: string;
  /**
   * Page Number (images)
   * Parameter defines the page number for [Google
   * Images](https://serpapi.com/images-results). There are 100 images per page. This
   * parameter is equivalent to start (offset) = ijn * 100. This parameter works only
   * for [Google Images](https://serpapi.com/images-results) (set tbm to `isch`).
   */
  ijn?: string;
}
```

---

# Agent 工具箱中的工具集

工具集（Toolkit）是旨在一起用于特定任务并具有方便的加载方法的工具的集合

```ts
const toolkit = new JsonToolkit(new JsonSpec(data));
const model = new OpenAI({ temperature: 0 });
const executor = createJsonAgent(model, toolkit);
```

值得注意的是，为了更好地串联工具，工具集一般会重新定义 ReAct Agent 的部分提示词

```ts {all} {maxHeight:'55%'}
export const JSON_PREFIX = `You are an agent designed to interact with JSON.
Your goal is to return a final answer by interacting with the JSON.
You have access to the following tools which help you learn more about the JSON you are interacting with.
Only use the below tools. Only use the information returned by the below tools to construct your final answer.
Do not make up any information that is not contained in the JSON.
Your input to the tools should be in the form of in json pointer syntax (e.g. /key1/0/key2).
You must escape a slash in a key with a ~1, and escape a tilde with a ~0.
For example, to access the key /foo, you would use /~1foo
You should only use keys that you know for a fact exist. You must validate that a key exists by seeing it previously when calling 'json_list_keys'.
If you have not seen a key in one of those responses, you cannot use it.
You should only add one key at a time to the path. You cannot add multiple keys at once.
If you encounter a null or undefined value, go back to the previous key, look at the available keys, and try again.

If the question does not seem to be related to the JSON, just return "I don't know" as the answer.
Always begin your interaction with the 'json_list_keys' with an empty string as the input to see what keys exist in the JSON.

Note that sometimes the value at a given path is large. In this case, you will get an error "Value is a large dictionary, should explore its keys directly".
In this case, you should ALWAYS follow up by using the 'json_list_keys' tool to see what keys exist at that path.
Do not simply refer the user to the JSON or a section of the JSON, as this is not a valid answer. Keep digging until you find the answer and explicitly return it.`;

export const JSON_SUFFIX = `Begin!"

Question: {input}
Thought: I should look at the keys that exist to see what I can query. I should use the 'json_list_keys' tool with an empty string as the input.
{agent_scratchpad}`;
```

---
layout: iframe
url: https://smith.langchain.com/public/e173ea9a-eb25-45bb-93c9-edf4d3bab6b0/r
---

---
level: 2
---

# 🌰 Web API 调用：OpenAPI Agent

如下所示，我们会通过 OpenAPI Agent 来调用 OpenAI 的文本补全接口（参见 [完整接口文件](https://github.com/langchain-ai/langchainjs/blob/main/examples/openai_openapi.yaml)）

```ts
const headers = {
  "Content-Type": "application/json",
  Authorization: `Bearer ${process.env.OPENAI_API_KEY}`,
};
const model = new OpenAI({ temperature: 0 });
const toolkit = new OpenApiToolkit(new JsonSpec(data), model, headers);
const executor = createOpenApiAgent(model, toolkit);
```
```ts {all} {maxHeight:'50%'}
export const OPENAPI_PREFIX = `You are an agent designed to answer questions by making web requests to an API given the OpenAPI spec.

If the question does not seem related to the API, return I don't know. Do not make up an answer.
Only use information provided by the tools to construct your response.

To find information in the OpenAPI spec, use the 'json_explorer' tool. The input to this tool is a question about the API.

Take the following steps:
First, find the base URL needed to make the request.

Second, find the relevant paths needed to answer the question. Take note that, sometimes, you might need to make more than one request to more than one path to answer the question.

Third, find the required parameters needed to make the request. For GET requests, these are usually URL parameters and for POST requests, these are request body parameters.

Fourth, make the requests needed to answer the question. Ensure that you are sending the correct parameters to the request by checking which parameters are required. For parameters with a fixed set of values, please use the spec to look at which values are allowed.

Use the exact parameter names as listed in the spec, do not make up any names or abbreviate the names of parameters.
If you get a not found error, ensure that you are using a path that actually exists in the spec.`;

export const OPENAPI_SUFFIX = `Begin!"

Question: {input}
Thought: I should explore the spec to find the base url for the API.
{agent_scratchpad}`;
export const JSON_EXPLORER_DESCRIPTION = `
Can be used to answer questions about the openapi spec for the API. Always use this tool before trying to make a request. 
Example inputs to this tool: 
    'What are the required query parameters for a GET request to the /bar endpoint?'
    'What are the required parameters in the request body for a POST request to the /foo endpoint?'
Always give this tool a specific question.`;
```

---
layout: iframe
url: https://smith.langchain.com/public/23fde4a9-e601-4d3b-93b7-152ed2dd6e9e/r
---

---
layout: iframe
url: https://smith.langchain.com/public/7466ecfb-6bc7-47de-b1e4-b7d386578c0e/r
---

---
layout: iframe
url: https://smith.langchain.com/public/719fb342-b90b-4ca2-b721-2fa4cf2497b5/r
---

---
layout: iframe
url: https://smith.langchain.com/public/c165d036-f363-4041-a652-8ade7b0893e4/r
---

---
level: 2
---

# 🌰 向量存储也可以作用工具（集）

```ts {16-} {maxHeight:'90%'}
import { OpenAI } from "langchain/llms/openai";
import { HNSWLib } from "langchain/vectorstores/hnswlib";
import { OpenAIEmbeddings } from "langchain/embeddings/openai";
import { RecursiveCharacterTextSplitter } from "langchain/text_splitter";
import { VectorStoreToolkit, createVectorStoreAgent, VectorStoreInfo } from "langchain/agents";

const model = new OpenAI({ temperature: 0 });
/* Load in the file we want to do question answering over */
const text = fs.readFileSync("state_of_the_union.txt", "utf8");
/* Split the text into chunks using character, not token, size */
const textSplitter = new RecursiveCharacterTextSplitter({ chunkSize: 1000 });
const docs = await textSplitter.createDocuments([text]);
/* Create the vectorstore */
const vectorStore = await HNSWLib.fromDocuments(docs, new OpenAIEmbeddings());

/* Create the agent */
const vectorStoreInfo: VectorStoreInfo = {
  name: "state_of_union_address",
  description: "the most recent state of the Union address",
  vectorStore,
};

const toolkit = new VectorStoreToolkit(vectorStoreInfo, model);
const agent = createVectorStoreAgent(model, toolkit);

const input = "What did biden say about Ketanji Brown Jackson is the state of the union address?";
const result = await agent.call({ input });
```

---
layout: iframe
url: https://smith.langchain.com/public/6b91b7c9-4eaa-48ae-a883-0b6a7794d707/r
---

---

# 必不可缺的 Callback 回调系统

Callback 回调系统让我们可以连接到 LLM 应用的各个阶段，这对于日志记录、监控、流传输等非常有用

`callbacks` 参数在整个 API（Model、Chain、Agent、Tool 等）的大多数对象上的两个不同位置可用

- 可以在构造器位置上接入 Callback（但不能跨对象使用），以 JS/TS 中的 Model 为例：

```ts
import { ConsoleCallbackHandler } from "langchain/callbacks";

const llm = new OpenAI({
  // These tags will be attached to all calls made with this LLM.
  tags: ["example", "callbacks", "constructor"],
  // This handler will be used for all calls made with this LLM.
  callbacks: [new ConsoleCallbackHandler()],
});
``` 

- 也可以在模块对象的 `apply()` / `run()` / `call()` 实例方法中发起请求时绑定，但仅对该请求有效：

```ts
const llm = new OpenAI({ temperature: 0 });
const response = await llm.call("1 + 1 =", {
  // These tags will be attached only to this call to the LLM.
  tags: ["example", "callbacks", "request"],
  // This handler will be used only for this call.
  callbacks: [new ConsoleCallbackHandler()],
});
```

---
level: 2
---

# 回顾：链路调试的“文韬武略”

我们既可以接入强力的 LangSmith，也可以使用 Console 输出调试信息

如何获得足够多且跨模块对象 Console 调试信息 ➡️ 开启 `verbose` 选项：

```sh
export LANGCHAIN_VERBOSE=true
```
```ts
const executor = await initializeAgentExecutorWithOptions(tools, model, { agentType: "xml", verbose: true });
```

或者直接通过环境变量接入 LangSmith：

```sh
export LANGCHAIN_TRACING_V2=true
export LANGCHAIN_ENDPOINT=https://api.smith.langchain.com
export LANGCHAIN_API_KEY=<your-api-key>  # still in closed beta
export LANGCHAIN_PROJECT=<your-project>  # if not specified, defaults to "default"
```

⚠️ 以下方式只能用于对象构造器和对象请求，不能形成完整链路：

```ts {4-}
import { Client } from "langsmith";
import { LangChainTracer } from "langchain/callbacks";

const client = new Client({ apiUrl: "https://api.smith.langchain.com", apiKey: "YOUR_API_KEY" });
const tracer = new LangChainTracer({ projectName: "YOUR_PROJECT_NAME", client });
```

---
level: 2
---

# 如何自定义 Callback 处理器

LangChain 支持通过实现基本回调处理程序接口来创建您自己的处理程序

自定义 Callback Handler 可以做一些比输出调试信息到控制台更复杂的事情，例如将事件发送到日志记录服务。作为示例，这里是一个记录到控制台的处理程序的 JS/TS 版本简单实现：

```ts {4-} {maxHeight:'70%'} 
import { BaseCallbackHandler } from "langchain/callbacks";
import { Serialized } from "langchain/load/serializable";
import { AgentAction, AgentFinish, ChainValues } from "langchain/schema";

export class MyCallbackHandler extends BaseCallbackHandler {
  name = "MyCallbackHandler";

  async handleChainStart(chain: Serialized) {
    console.log(`Entering new ${chain.id} chain...`);
  }

  async handleChainEnd(_output: ChainValues) {
    console.log("Finished chain.");
  }

  async handleAgentAction(action: AgentAction) {
    console.log(action.log);
  }

  async handleToolEnd(output: string) {
    console.log(output);
  }

  async handleText(text: string) {
    console.log(text);
  }

  async handleAgentEnd(action: AgentFinish) {
    console.log(action.log);
  }
}
```

---
src: ../../pages/common/refs.md
---

---
src: ../../pages/common/end.md
---