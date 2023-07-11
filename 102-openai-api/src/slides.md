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
download: 'https://github.com/webup/agi-talks/raw/master/pdfs/102-openai-api.pdf'
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

# OpenAI API 浅出

OpenAI API Walkthrough

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
  <a href="https://github.com/webup/agi-talks/raw/master/pdfs/102-openai-api.pdf" target="_blank" alt="Download"
    class="text-xl icon-btn opacity-50 !border-none !hover:text-white">
    <carbon-download />
  </a>
</div>

---
src: ../../pages/common/toc.md
---

---
layout: iframe
url: https://www.youtube.com/embed/RQgBnQUaKG0?start=44
hideInToc: true
---

---

# 说到 OpenAI 会想到什么？ 

大家是否还记得当年 <Wiki id="GitHub_Copilot">GitHub Copilot</Wiki> 的惊艳亮相，它背后的开发团队就是 OpenAI

###### OpenAI 实验室 / 公司

- <Wiki id="OpenAI">OpenAI</Wiki> 是一家美国的人工智能研究实验室，它由两部分组成：
  - 非营利性的 OpenAI Incorporated 
  - 营利性子公司 OpenAI Limited Partnership
- OpenAI 开展人工智能研究，其宣称的目的是促进和发展友好的人工智能

<br/>

###### ChatGPT 聊天机器人

- <Wiki id="ChatGPT">ChatGPT</Wiki> 是由 OpenAI 开发的人工智能聊天机器人应用，于 2022 年 11 月 30 日推出
- 它的显着特点是使用户能够改进和引导对话达到所需的长度、格式、风格、细节级别和使用的语言

<br />

###### GPT-3/4 大语言模型

- <Wiki id="GPT-3">GPT-3</Wiki> 是由 OpenAI 在 2020 年推出的 <Wiki id="Large_language_model">大语言模型</Wiki>，是 ChatGPT 使用的默认模型
- <Wiki id="GPT-4">GPT-4</Wiki> 是由 OpenAI 在 2023 年 3 月 推出的 <Wiki id="Multimodel_learning">多模态</Wiki> 大语言模型

---
src: ../../pages/playground/openai.md
hideInToc: true
---

---
layout: quote
hideInToc: true
---

# OpenAI API 概览

Chat Completions，Completions, Embeddings，Moderations

---

# 对话补全 API：Chat Completions

ChatGPT（及高仿）对话机器人应用的基石

<mdi-import /> 一段对话的一组消息文本；<mdi-export /> 模型推测的下一条对话内容

```ts {all|14-20}
import { Configuration, OpenAIApi } from 'openai';

export const chat = async (prompt = "Hello world", options = {}) => {
  // Initialize OpenAI API stub
  const configuration = new Configuration({
    apiKey: process.env.OPENAI,
  });
  const openai = new OpenAIApi(configuration);

  // Prepare the messages
  const messages = typeof prompt === "string"
    ? [{ role: "user", content: prompt }] : prompt;

  // Request chat completion
  const chatCompletion = await openai.createChatCompletion({
    model: "gpt-3.5-turbo",
    messages,
    ...options,
  });
  return chatCompletion.data.choices[0].message.content;
};
```

---
level: 2
---

# Chat Completions API 的常用参数

请访问 [官方文档](https://platform.openai.com/docs/api-reference/chat/create) 了解完整的参数；最新发布的 Functions Call 功能详见 后续章节

###### Required

- `model`：目前的可用模型包括 GPT-3.5 和 GPT-4 的一些模型
  - `gpt-3.5-turbo`, `gpt-3.5-turbo-0613`, <B>`gpt-3.5-turbo-16k`</B>, `gpt-3.5-turbo-16k-0613`
  - `gpt-4`, `gpt-4-0613`, <B>`gpt-4-32k`</B>, `gpt-4-32k-0613`
- `messages`：用于描述对话的具体内容，每条消息一般包含以下两个字段
  - `role`：三种角色 <B>`system`（“对话语境架构师”）</B>, `user`（“我”）, `assistant`（“对话机器人”）
  - `content`：消息的内容，也可以理解成是各个角色说的话


<br />

###### Optional

- `temperature`：采样温度，介于（且包含） `0` 和 `2` 之间，默认为 `1`，<B>温度越高输出越随机</B>
  - `top_p`：从一组累计概率不超过 P 的词中选择（避免概率很低的词被选中），一般和“温度”二选一使用
- `max_tokens`：所生成的内容的最多可以承载多少 Token；<B>输入 + 输出 Token ≤ 模型的 Token 限量</B>

---
level: 3
---

# Token 是怎么计算的？

GPT 系列模型使用 Token 处理文本，模型了解这些 Token 之间的统计关系，并擅长生成序列中的下一个 Token

###### Token 的切分

- 对于英文输入，一个 Token 一般对应 4 个字符或者 3/4 个单词
- 对于中文输入，一个 Token 一般对应一个或半个词

<br />

###### Token 的计算

- OpenAI 提供了一个计算和可视化 Token 的 [页面](https://platform.openai.com/tokenizer)
- 官方 Python 类库： [openai/tiktoken: tiktoken is a fast BPE tokeniser for use with OpenAI's models.](https://github.com/openai/tiktoken)
- 社区 JavaScript 类库：[latitudegames/GPT-3-Encoder: JavaScript BPE Encoder Decoder for GPT-2 / GPT-3](https://github.com/latitudegames/GPT-3-Encoder)

<br />

###### Token 的用量（[计费](https://openai.com/pricing#language-models)）

- 不同模型有不同的 Token 限制，<B>Token 限制是输入的 Prompt 和输出的 Completion 的 Token 数量之和</B>
  - 因此输入的 Prompt 越长，能输出的 Completion 的上限就越低
- `gpt-3.5-turbo` 的 Token 上限是 `4k`；`gpt-3.5-turbo-16k` 的上限是 `16k`

---
level: 3
---

# 关于 Token 切分的迷思

以英文为例，怎么理解 “一个 Token 一般对应 4 个字符或者 3/4 个单词”；参见 [更多示例](https://github.com/openai/openai-cookbook/blob/main/examples/How_to_count_tokens_with_tiktoken.ipynb)

<Val id="webup.chatSampleReverseToken" height="40%" /><br />

<v-click><Val id="webup.chatSampleReverseTokenDelim" height="40%" /></v-click>

---
level: 2
---

# 幕后的游戏规则制定者：`role:system`

OpenAI Playground 的默认系统提示是 “You are a helpful assistant”

`system` 信息用于制定模型的规则，例如设定、回答准则一类的，目前只能通过 API 来进行设置。

<Val id="webup.chatSampleSystemRoleSimple" height="40%" />
<Val id="webup.chatSampleSystemRoleSimpleLine" height="40%" />

---
level: 3
---

# 🌰 高阶版的 `system` 提示工程示例

[LangGPT](https://github.com/yzfly/LangGPT): Empowering everyone to become a prompt expert!🚀 Structured Prompt，结构化提示词

<Val id="webup.chatSampleSystemRoleExpert" />

💡 参考：[‌‌⁢‍⁢⁡⁢⁡⁡‌⁤群友 Arthur 等整理的 Prompt 最佳实践](https://ywh1bkansf.feishu.cn/wiki/JTjPweIUWiXjppkKGBwcu6QsnGd)，[如何写好 Prompt: 结构化](https://www.lijigang.com/posts/chatgpt-prompt-structure/)

---

# 文本补全 API：Completions

常用于给定主题的文章编写、代码片段的解读

<mdi-import /> 一条或一组提示文本；<mdi-export /> 根据提示和参数推测的输出内容

```ts {all|10-15}
import { Configuration, OpenAIApi } from 'openai';

export const complete = async (prompt, options = {}) => {
  // Initialize OpenAI API stub
  const configuration = new Configuration({
    apiKey: process.env.OPENAI,
  });
  const openai = new OpenAIApi(configuration);

  // Request completion
  const completion = await openai.createCompletion({
    model: "text-davinci-003",
    prompt,
    ...options,
  });
  return completion.data.choices[0].message.content;
};
```

---
level: 2
---

# Completions API 的常用参数

请访问 [官方文档](https://platform.openai.com/docs/api-reference/completions) 了解完整的参数；该接口及其模型 [逐步退役](https://openai.com/blog/gpt-4-api-general-availability) 中

###### Required

- `model`：目前的可用模型包括 <Wiki id="GPT-3">GPT-3</Wiki> 的一些模型
  - <B>`text-davinci-003`</B>（最强）, `text-davinci-002`
  - `text-curie-001`, `text-babbage-001`, <B>`text-ada-001`</B>（最快最便宜）
- `prompt`：编码为字符串、字符串数组、Token 一维或者二维数组
  - 注意 `<|endoftext|>` 是模型在训练期间看到的文档分隔符
  - 因此如果在提示中没有另外指定分隔符，模型将像从新文档开始一样生成

<br />

###### Optional

基本和 Chat Completions 接口所用参数一致

---
level: 2
---

# 🌰 基于 Completions 的代码解读示例

<Val id="webup.completeSampleExplainCode" height="90%" />

---

# 文本向量 API：Embeddings

面向文本问答、文本检索这类功能的基石；可访问 [官方文档](https://platform.openai.com/docs/api-reference/embeddings) 了解完整参数

<mdi-import /> 一条或一组文本；<mdi-export /> 文本或 Token 对应的向量数组表达

```ts {all|10-12}
import { Configuration, OpenAIApi } from 'openai';

export const embed = async (input, model = "text-embedding-ada-002") => {
  // Initialize OpenAI API stub
  const configuration = new Configuration({
    apiKey: process.env.OPENAI,
  });
  const openai = new OpenAIApi(configuration);

  // Request embedding
  const { data } = await openai.createEmbedding({ model, input });
  return data.embedding;
};
```

###### Required

- `model`：<B>`text-embedding-ada-002`</B>, `text-search-ada-doc-001`
- `input`：输入要 <Wiki id="Word_embedding">Embed</Wiki> 的文本，编码为字符串或 Token 数组
  - 要在单个请求中嵌入多个输入，可传递字符串数组或二维 Token 数组


---
level: 2
---

# Emeddings 如何用于文本问答

这部分内容会在 201 LangChain Components 分享中详细介绍

![Pinecone](https://miro.medium.com/v2/resize:fit:1920/format:webp/1*Wpfc5762Y_GGmrcRaiDXmw.png)


---

# 文本审查 API：Moderations

识别和过滤各种类别的违禁内容，例如仇恨、自残、色情和暴力等；可访问 [官方文档](https://platform.openai.com/docs/api-reference/moderations) 了解完整参数

<mdi-import /> 一条或一组文本；<mdi-export /> OpenAI [监督标准](https://platform.openai.com/docs/guides/moderation) 评分

```ts {all|10-12}
import { Configuration, OpenAIApi } from 'openai';

export const moderate = async (input, model) => {
  // Initialize OpenAI API stub
  const configuration = new Configuration({
    apiKey: process.env.OPENAI,
  });
  const openai = new OpenAIApi(configuration);

  // Request moderation
  const { results } = await openai.createModeration({ model, input });
  return results;
};
```

###### Required

- `input`：待检验的文本

###### Optional

- `model`: `text-moderation-stable` 和 `text-moderation-latest`（默认，自更新）

---
level: 2
---

# 🌰 一个非常简单的文本内容审核的示例

<Val id="webup.moderateSampleViolence" height="40%" /><br />
<Val id="webup.moderateSampleHate" height="40%" />


---
layout: quote
hideInToc: true
---

# 函数调用（Function Calling）

Chat Completions 的新神器 🚀

---
title: Chat Completions 神器：Function Calling
---

# 函数调用的点睛之处

并非直接调用函数，而是告诉你应该怎么调用函数（包括用什么参数）

⛓️ 函数调度的基本流程：

1. 使用 Chat Completions <B>接口请求参数中定义的一组函数</B>来调用模型（参见 [定义格式](https://platform.openai.com/docs/api-reference/chat/create#chat/create-functions)）
2. <B>模型可以选择调用函数</B> —— 输出一个符合用户自定义模式的字符串化 JSON 对象
   - 注意：模型可能会生成无效的 JSON 或幻觉参数
   - 幻觉参数输出通常可以通过系统消息来缓解，例如请尝试使用系统消息：“仅使用已经提供的函数”
3. 【可选】用户可以将字符串解析为代码中的 JSON，并<B>使用提供的参数调用实际的函数</B>（如果它们确实存在） 
4. 【可选】将实际的<B>函数的响应作为新的输入消息再次调用模型</B>，并让模型将结果汇总（总结、解释）给用户

<br />

🪄 函数调用的重要意义：

- 为用户函数定义了官方统一的描述格式（基于 [JSON Schema](https://json-schema.org/understanding-json-schema/)，代价是计入输入 Token）
- 为用户提供了一个“后门”确保对话可以输出 JSON 格式数据（有效简化了提示）

---
level: 2
---

# 🌰 函数调用的基本流程示例

在如下示例中，我们要求模型查询波士顿的温度，并提供了对应的可用函数供调用

<Val id="webup.chatSampleFunctionSingle" />

---
level: 2
---

# 🌰 自主选择函数进行问答的示例（函数准备）

在如下示例中，我们准备两个函数定义，分别是：获取当日天气预报、获取多日的天气预报

<Val id="webup.schemasWeather" />

---
level: 2
---

# 🌰 自主选择函数进行问答的示例（问答过程）

在问答阶段，针对不同的提示，模型可以自主选择合适的函数调用并提供参数

<Val id="webup.chatSampleFunctionMultiple" />

---
layout: quote
---

# OpenAI Functions 的衍生

巧用模型自主填充函数参数的特性可以做些什么？

---
level: 2
---

# 🌰 函数调用能力的衍生：文本提取

在如下示例中，利用函数提取文本中目标内容并进行 JSON 格式化输出

<Val id="webup.chatSampleFunctionExtraction" />

---
level: 2
---

# 🌰 函数调用能力的衍生：文本标签

在如下示例中，利用函数推测文本相关内容并进行 JSON 格式化输出

<Val id="webup.chatSampleFunctionTagging" />

---
layout: quote
hideInToc: true
---

# 还有哪些接口值得关注？

还有一些看似“平平无奇”的接口 😄

---

# 模型调优 API：Fine-tunes

面向私域数据优化模型；可访问 [使用指南](https://platform.openai.com/docs/guides/fine-tuning) 及 [接口明细](https://platform.openai.com/docs/api-reference/fine-tunes) 了解所有接口及其参数应用

微调（Fine-tuning）的收益主要包括：

- 相比提示工程的有限输入，可以<B>训练更多数据</B>（通常是文件级别）
- 训练后针对私域数据，可以使用<B>更短的提示词，得到更快更好的结果</B>

<br />

哪些模型可以被微调：

- 目前只支持 4 个<B>基础模型</B>：`davinci`、`curie`、`babbage`、`ada`
  - 注意这些不是其它接口所使用的指令优化模型，例如 `text-davinci-003`
  - 模型微调的费用详见 OpenAI [官方报价](https://openai.com/pricing#language-models)（分为两类：训练、使用）
- 训练数据需要使用 [JSONL](https://jsonlines.org/) 格式，每行都是一对 `prompt`-`completion` 数据（参见 [训练数据准备工具](https://platform.openai.com/docs/guides/fine-tuning)）
- 用户微调过的模型可以持续地 [再次进行微调](https://platform.openai.com/docs/guides/fine-tuning/continue-fine-tuning-from-a-fine-tuned-model)

---

# 图片生成 API：Images

基于文化或图片来生成图片；可访问 [使用指南](https://platform.openai.com/docs/guides/images) 及 [接口明细](https://platform.openai.com/docs/api-reference/images) 了解所有接口及其参数应用

🎯 目前使用的模型是 [DALL·E](https://platform.openai.com/docs/models/dall-e)，其能力主要包括：

- 根据文本提示从零开始创建图像
- 根据新文本提示创建修改现有图像
- 根据现有图像（最大 4MB）生成它的变体

<br />

> 💡 可访问 [DALL·E 预览应用](https://labs.openai.com/) 来体验图片生成效果（竟然还收费卖积点 😼）

<br />

🚧 目前接口在使用上的一些限制：

- 图片数量：`1` 至 `10` 张
- 图片尺寸：`256x256`，`512x512`，`1024x1024`（越小生成的越快）
- 提示工程：修改图片需要重新描述完整的图片（即使已通过输入遮罩指定修改区域，[参考样例](https://github.com/openai/openai-cookbook/blob/main/examples/dalle/How_to_create_dynamic_masks_with_DALL-E_and_Segment_Anything.ipynb)）

---

# 语音成文 Audio

 STT（Speech to Text）语音转文本；可访问 [使用指南](https://platform.openai.com/docs/guides/speech-to-text) 及 [接口明细](https://platform.openai.com/docs/api-reference/audio) 了解所有接口及其参数应用

 🎯 目前使用的模型是 [Whisper](https://openai.com/research/whisper)（`whisper-1`），其能力主要包括：

- 将音频转录为音频所使用的任何语言文本
- 将音频翻译并转录为英语文本

<br />

🚧 目前接口在使用上的一些限制：

- 文件大小：上次最大不超过 25 MB
- 文件格式：`mp3`、`mp4`、`mpeg`、`mpga`、`m4a`、`wav`、`webm`
- 提示工程：使用提示来提高生成的转录文本的质量，仅提供对生成的音频的有限控制（[参考样例](https://github.com/openai/openai-cookbook/blob/main/examples/Whisper_prompting_guide.ipynb)）
  - 对于纠正模型在音频中经常错误识别的特定单词或首字母缩略词非常有帮助
  - 有些语言可以以不同的方式书写（例如简体中文和繁体中文），可以通过提示写作风格来选定
  - 为了保留被拆分为段的文件的上下文，可以使用前一段的记录提示模型

---
src: ../../pages/common/refs.md
---

---
src: ../../pages/common/end.md
---