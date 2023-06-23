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
download: 'https://github.com/webup/agi-talks/raw/master/pdfs/101-prompt-engineering.pdf'
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

# AGI 提示工程指北

Prompt Engineering in Action

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
  <a href="https://github.com/webup/agi-talks/raw/master/pdfs/101-prompt-engineering.pdf" target="_blank" alt="Download"
    class="text-xl icon-btn opacity-50 !border-none !hover:text-white">
    <carbon-download />
  </a>
</div>

---
src: ../../pages/common/toc.md
---

---
layout: two-cols
title: 基础 LLM vs. 指令微调 LLM
---

# 基础 LLM

Base LLM

- 模型行为：预测下一个（系列）单词
- 训练内容：互联网和其它来源的大量数据

```md {2-}
Once upon a time, there was a unicorn
that lived in a magical forest with all her friends
```

> 基础 LLM 的默认行为是 “续写” 文本

<br>

```md {2-}
What is the capital of France?
What is France's largest city?
What is France's population?
What is the currency of France?
```

> 训练数据（比如互联网上的文章）很可能是关于法国的问答列表

::right::

# 指令微调 LLM

Instruction Tuned LLM

- 模型行为：接收指令，输出预测结果
- 训练过程：
  - 从大量文本数据中训练出一个基础 LLM
  - 使用指令和良好尝试的输入输出进行微调和优化
  - 使用“[人类反馈强化学习](https://huggingface.co/blog/rlhf)”的技术进行进一步细化

```md {2-}
What is the capital of France?
The capital of France is Paris.
```

💡 现在互联网上大家使用到的 LLM 基本都是这类模型

---
layout: two-cols
---

# 提示工程关键原则

Principles of Prompting

原则一：<B>编写清晰而具体的指令</B>

- 策略1⃣️：使用分隔符清楚地限定输入的不同部分
- 策略2⃣️：要求模型结构化输出
- 策略3⃣️：要求模型检查是否满足条件
- 策略4⃣️：提供 <Wiki id="In-context_learning_(natural_language_processing)">少样本提示（Few Shot）</Wiki>

原则二：<B>给模型充足的思考时间</B>

- 策略5⃣️：指定完成任务所需要的步骤
- 策略6⃣️：指导模型制定自己的解决方法

::right::

# 模型的局限性

Model Limitations

可能会产生 <Wiki id="Hallucination_(artificial_intelligence)">幻觉（Hallucination）</Wiki>

- 不清楚自己的知识边界
- 虚构听起来很有道理但实际伤感不正确的东西

减少幻觉的策略

- 要求模型首先从文本中找到任何相关的引文
- 然后要它使用那些引文来回答问题
- 并将答案追溯回源文件

---
src: ../../pages/playground/openai.md
hideInToc: true
---

---
level: 2
---

# 策略一：使用分隔符清楚地限定输入的不同部分

常见的分隔符可以是：<code>```</code>，`""`，`<>`，`<tag></tag>` 等

- 输入里面可能包含其他指令，会覆盖掉你的指令
- 可以使用任何明显的标点符号将特定的<B>文本</B>部分与<B>提示</B>的其余部分分开

<Val id="webup.chatSampleDelimeter" />

---
level: 2
---

# 策略二：要求一个结构化的输出

常见的结构化输出可以是 JSON、HTML 等格式

在以下示例中，我们要求 GPT 生成三本书的标题、作者和类别，并要求以 JSON 的格式返回给我们。

<Val id="webup.chatSampleFormatOutput" />

---
level: 2
---

# 策略三：要求模型检查是否满足条件

如果任务做出的假设不一定满足，我们可以告诉模型先检查这些假设，如果不满足，指示并停止执行

在如下示例中，我们将给模型一段没有明确步骤的文本。模型在判断后应回答未提供步骤。

<Val id="webup.chatSampleSatisfication" />

---
level: 2
---

# 策略三：要求模型检查是否满足条件（提供出路）

如果模型无法完成分配的任务，有时为模型提供备用路径可能会有所帮助。

<Val id="webup.chatSampleHallucination" height="40%" /><br/>

<Val id="webup.chatSampleHallucinationExit" height="40%" />

---
level: 2
---

# 策略四：提供少量示例

即在要求模型执行实际任务之前，提供给它少量成功执行任务的示例

在如下示例中，我们要求模型以一致的风格作答；由于已经有了少量示例，它将以类似的语气回答。

<Val id="webup.chatSampleFewShot" />

---
level: 2
---

# 策略五：指定完成任务所需的步骤

接下来我们将通过给定一个复杂任务，给出完成该任务的一系列步骤，来展示这一策略的效果

首先我们描述了杰克和吉尔的故事，并给出一个指令。由于提示不充分，输出的内容带了不需要的序号。

<Val id="webup.chatSampleStepByStep" />

---
level: 2
---

# 策略五：指定完成任务所需的步骤（续）

我们给出一个更好的 Prompt，该 Prompt 指定了输出的格式

<Val id="webup.chatSampleStepByStepFormat" />

---
level: 2
---

# 策略六：指导模型在下结论前找出自己的解法

有时候，明确地指导模型在做决策之前要思考解决方案，我们会得到更好的结果

在如下示例中，我们给出一个问题和一个学生的错误解答，要求模型判断解答是否正确。

<Val id="webup.chatSampleCheckAnswer" />

---
level: 2
---

# 策略六：指导模型在下结论前找出自己的解法（续）

我们可以通过指导模型先自行找出一个解法来解决这个问题。通过明确步骤，让模型有更多时间思考

<Val id="webup.chatSampleCheckAnswerWithStep" />

---

# 提示工程需要迭代

当使用 LLM 构建应用程序时，很少在第一次尝试中就成功使用最终应用程序中所需的提示词

我们将以从产品说明书中生成营销文案这一示例，展示一些框架，以提示你思考如何迭代地分析和完善提示词。

<Val id="webup.chatSamplePolishNothing" />

---
level: 2
---

# 问题一：生成文本太长

解决方法：要求模型限制生成文本长度

因为它太长了，所以我会澄清我的提示，并说最多使用 50 个字。

<Val id="webup.chatSamplePolishTooLong" />

---
level: 2
---

# 问题二：文本关注在错误的细节上

解决方法：要求模型专注于与目标受众相关的方面

修改提示让它更精确地描述椅子的技术细节，并要求在描述的结尾包括对应的 7 个字符产品 ID。

<Val id="webup.chatSamplePolishBadFocus" />

---
level: 2
---

# 问题三：需要一个更好的呈现形式

解决方法：要求模型抽取信息并组织成表格，并指定表格的列、表名和格式（如 HTML）

<Val id="webup.chatSamplePolishHTMLTable" />

---
layout: quote
hideInToc: true
---

# 大语言模型能力概览

概括（Summerize）、推断（Infer）、转换（Transform）、扩展（Expand）

---

# 模型能力：概括（Summerize）

子能力：限制输出长度，侧重关键角度，提取关键信息

如果我们只想要提取某一角度的信息，并过滤掉其他所有信息，可以要求模型进行文本提取（Extract）。

<Val id="webup.chatSampleExtract" />

---

# 模型能力：推断（Infer）· 情绪

大语言模型可以从一段文本中提取正面或负面情感

在如下示例中，我们要求模型对一份客户反馈进行情绪推断，并抽取相关信息后进行格式化输出。

<Val id="webup.chatSampleInferEmotion" />

---

# 模型能力：推断（Infer）· 主题

大语言模型也可以推断一段文本是关于什么的、有什么话题

在如下示例中，我们要求模型从一片虚构的文章中推断出其中的五个主题。

<Val id="webup.chatSampleInferTopic" />

---

# 模型能力：推断（Infer）· 分类

基于对于主题的推断，大语言模型还可以帮忙把主题和给定的分类进行映射

<Val id="webup.chatSampleInferCategory" />

---

# 模型能力：转换（Transform）

大语言模型非常擅长将输入转换成不同的格式，如多语种文本翻译、拼写及语法纠正、语气调整、格式转换等

如下的示例展示了一个综合样例，包括了文本翻译、拼写纠正、风格调整和格式转换。

<Val id="webup.chatSampleTransform" />

---

# 模型能力：扩展（Expand）

扩展是将短文本，例如一组说明或主题列表，输入给大型语言模型，让它生成更长的文本

如下示例中，我们将根据客户评价和对应的情感，要求模型撰写自定义电子邮件响应。

<Val id="webup.chatSampleExpandEmail" />

---
src: ../../pages/common/refs.md
---

---
src: ../../pages/common/end.md
---