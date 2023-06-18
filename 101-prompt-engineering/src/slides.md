---
# See all frontmatter configurations: https://sli.dev/custom/#frontmatter-configures
# theme id or package name, see also: https://sli.dev/themes/use.html
theme: 'seriph'
# titleTemplate for the webpage, `%s` will be replaced by the page's title
titleTemplate: '%s｜WebUP'
# some information about the slides, markdown enabled
info: |
  AGI 学习笔记，仅供个人学习使用# favicon, can be a local file path or URL
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
  hideInToc: true

# slide configurations
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
layout: two-cols
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

- 策略5⃣️：追定完成任务的步骤
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
---

---

# 策略1⃣️：使用分隔符清楚地限定输入的不同部分

常见的分隔符可以是：<code>```</code>，`""`，`<>`，`<tag></tag>` 等

- 输入里面可能包含其他指令，会覆盖掉你的指令
- 可以使用任何明显的标点符号将特定的<B>文本</B>部分与<B>提示</B>的其余部分分开

<Val id="webup.chatSampleDelimeter" />
