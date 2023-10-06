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
src: ../../pages/common/refs.md
---

---
src: ../../pages/common/end.md
---