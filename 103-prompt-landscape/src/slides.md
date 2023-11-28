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
download: 'https://github.com/webup/agi-talks/raw/master/pdfs/103-prompt-landscape.pdf'
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

# Hub 提示词面面观

LangChain Hub Prompt Landscape

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
  <a href="https://github.com/webup/agi-talks/raw/master/pdfs/103-prompt-landscape.pdf" target="_blank" alt="Download"
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
layout: iframe-right
url: https://blog.langchain.dev/the-prompt-landscape/
---

# Hub 热门提示词 🔥

Popular Prompts in LangChain Hub

社区中各种不同用例的提示词在不断涌现
<T>A variety of prompts for different uses-cases have emerged.</T>

- [Prompt Engineering Guide](https://www.promptingguide.ai/) - [@dair_ai](https://twitter.com/dair_ai) 
- [Prompt Engineering](https://lilianweng.github.io/posts/2023-03-15-prompt-engineering/) - [Lilian Weng](https://lilianweng.github.io/)

<br/>

LangChain Hub 推出一个多月以来也积累了丰富的提示词，大致可以被总结为十类用例
<T>Below we provide an overview of the major themes in prompting that we’ve seen since the launch of LangChain Hub and highlight interesting examples.</T>

---
layout: image
image: https://blog.langchain.dev/content/images/size/w1000/2023/10/image-19.png
---

---
layout: image
image: https://blog.langchain.dev/content/images/size/w1000/2023/10/image-21.png
---

---

# ✍️ 场景写作（Writing）

Keywords: Writing Editor / Style, Content Generation

随着提示工程的普及，制作多样化内容的提示也不断增多（例如，邮件、博客文章、推文、教育学习材料）
<T>There's been a proliferation of prompts for producing diverse content (e.g. emails, blog posts, tweet threads, learning materials).</T>

- [SaaS User On-Boarding Email](https://smith.langchain.com/hub/gitmaxd/onboard-email)： 生成引人注目的 SaaS 平台注册欢迎邮件
- [Blog Generator](https://smith.langchain.com/hub/hardkothari/blog-generator)：根据提供的上下文创建结构良好的博客文章
- [Tweet from Text](https://smith.langchain.com/hub/hardkothari/tweet-from-text)：在字数限制内、面向特定受众，制作简洁有效的推文
- [Podcaster Tweet Thread](https://smith.langchain.com/hub/julia/podcaster-tweet-thread)：给定播客的文字脚本，编写一条吸引眼球的推文
- [Test Question Making](https://smith.langchain.com/hub/gregkamradt/test-question-making)：为特定问题的测试（考试）创建一组精心设计的答案

<br/>另外，随着大语言模型的不断成熟，还有一些“绑定特定语言模型“的通用写作提示也在不断涌现
<T>Prompts to improve writing have widespread appeal given the impressive displays of creativity from specific LLMs.</T>

- Matt Shumer 编写的基于 GPT-4 模型的 [写作助手](https://smith.langchain.com/hub/rlm/matt-shumer-writing) 和 [风性化写作助手](https://smith.langchain.com/hub/rlm/matt-shumer-writing-style)

<T>@mattshumer_’s popular GPT-4 prompts provide ways to improve writing clarity or customize the style of LLM-generated text.</T>

---
layout: iframe
url: https://smith.langchain.com/hub/gitmaxd/onboard-email
---

---
layout: iframe
url: https://smith.langchain.com/hub/rlm/matt-shumer-writing
---

---
layout: iframe
url: https://smith.langchain.com/hub/rlm/matt-shumer-writing-style
---

---

# ⏳ 信息总结（Summarization）

Keywords: Chain-of-density (CoD), Document Summerization

[内容总结](https://blog.langchain.dev/llms-to-improve-documentation/) 是 LLM 的一个强大用例：例如 [Anthropic Claude 2](https://www.anthropic.com/index/claude-2)，可以吸收超过 70 页的内容进行直接总结
<T>Summarization of content is a powerful LLM use-case. Longer context LLMs, e.g. Claude2, can absorb > 70 pages for direct summarization.</T>

[Chain of Density](https://browse.arxiv.org/pdf/2309.04269.pdf) 之类的提示技术提供了一种补充方法，从而产生密集且人性化的更好的摘要
<T>Prompting techniques like chain of density offer a complimentary approach, resulting in dense yet human-preferable summaries.</T>

- [Chain of Density](https://smith.langchain.com/hub/lawwu/chain_of_density)：对给定文章（多次循环）生成越来越简洁、实体密集的摘要
- [Anthropic Paper QA](https://smith.langchain.com/hub/hwchase17/anthropic-paper-qa)：基于 Claude 2 的论文内容总结和审阅
- [YouTube Transcript to Article](https://smith.langchain.com/hub/muhsinbashir/youtube-transcript-to-article)：获取给定的 YouTube 视频文字并将其转换为结构良好且引人入胜的文章

<br/>此外，摘要可以应用于多种内容类型，例如聊天对话或特定于领域的数据（[财务表摘要](https://smith.langchain.com/hub/hwchase17/financial-table-insights)）
<T>In addition, summarization can be applied to diverse content types like chat conversations or domain specific data</T>

---
layout: iframe
url: https://smith.langchain.com/hub/lawwu/chain_of_density
---

---
layout: iframe
url: https://smith.langchain.com/hub/hwchase17/anthropic-paper-qa
---

---
layout: iframe
url: https://smith.langchain.com/hub/muhsinbashir/youtube-transcript-to-article
---

---
layout: iframe
url: https://smith.langchain.com/hub/hwchase17/financial-table-insights
---

---
layout: two-cols
---

# 📋 信息提取（Extraction）

Keywords: Knowledge Graph Triples, Function Calling

LLM 可以是提取特定格式文本的强大工具，目前比较有代表性的是 OpenAI 的 [Function Calling](https://openai.com/blog/function-calling-and-other-api-updates) 功能
<T>LLMs can be powerful tools for extracting text in particular formats, often aided by OpenAI's function calling.</T>

有些类库提供基于 Function Calling 的信息抽取功能，例如 [Instructor](https://jxnl.github.io/instructor/)  
<T>Some frameworks developed to support it, such as Instructor.</T>

Hub 中也有针对特定提取任务设计的提示词，例如进行知识图谱三元组提取
<T>We've also seen prompts designed for specific extraction tasks, such as knowledge graph triple extraction.</T>

- [Instagraph](https://instagraph.ai/), [Text-to-Graph Playground](https://twitter.com/RLanceMartin/status/1691880034058064365)

::right::

<Tweet id="1701351068817301922" />

---
layout: iframe
url: https://smith.langchain.com/hub/langchain/knowledge-triple-extractor
---

---

# 👩‍💻 代码分析和生成（Code Analysis and Generation）

Keywords: Code Review / Analysis，Code Generation

代码分析是最流行的 LLM 用例之一，Hub 中也有不少提示词是在这方面起作用的：
<T>Code analysis is one of the most popular LLM use-cases, we've seen a number of prompts related to this theme:</T>

- [Open Interpreter System](https://smith.langchain.com/hub/chuxij/open-interpreter-system)：驱动 [Open Interpreter](https://www.pluralsight.com/resources/blog/data/chatgpt-code-interpreter-plugin-guide) 通过执行代码完成用户提出的各种目标
- [Virtual Github PR Reviews](https://smith.langchain.com/hub/homanp/github-code-reviews)：对 GitHub 代码仓库中的 Pull Request 进行代码审查

---
layout: iframe
url: https://smith.langchain.com/hub/chuxij/open-interpreter-system
---

---
layout: iframe
url: https://smith.langchain.com/hub/homanp/github-code-reviews
---

---

# 🤖️ 提示优化（Prompt Optimization）

Keywords: Image Creation, Instruction Improvement

[Deepmind 的论文](https://arxiv.org/pdf/2309.03409.pdf) 指出：LLM 可以优化提示。
<T>The Deepmind work showing that LLMs can optimize prompts. </T>

我们沿着这些思路看到了许多有趣的提示词；一个很好的例子是 [Midjourney](https://www.midjourney.com/) 的提示优化，如下所示：
<T>We’ve seen a number of interesting prompts along these lines; one good example is for Midjourney, as shown below:</T>

<div class="flex">
  <img src="https://blog.langchain.dev/content/images/size/w1000/2023/10/mj.png" width="450" class="flex-1" />
  <span class="flex-1 ml-5">
    <p>Freddie Mercury electrifying the San Francisco Pride Parade stage, shining in a gleaming golden outfit, iconic microphone stand in hand, evoking the hyper-realistic style of Caravaggio, vivid and dynamic --ar 16:9 --q 2)</p>
    <T>Freddie Mercury performing at the 2023 San Francisco Pride Parade hyper realistic</T>
  </span>
</div>

---
layout: iframe
url: https://smith.langchain.com/hub/aemonk/midjourney_prompt_generator
---

---
layout: iframe
url: https://smith.langchain.com/hub/hardkothari/prompt-maker
---

---
layout: iframe-right
url: https://smith.langchain.com/hub/rlm/rag-prompt
title: 📓 检索增强生成（RAG）
---

# 📓 检索生成（RAG）

Keywords: Adding Context

检索增强生成是现在非常流行的 LLM 应用场景
<T>RAG is a popular LLM application.</T>

![](https://blog.langchain.dev/content/images/2023/10/image-15.png)

<br/>它将 LLM 的推理能力与外部数据源的内容结合起来，这对于 [企业数据](https://www.glean.com/blog/how-to-build-an-ai-assistant-for-the-enterprise) 来说尤其强大
<T>RAG has particular promise because it marries the reasoning capability of LLMs with the content of external data sources, which is particularly powerful for enterprise data.</T>

---
layout: iframe
url: https://smith.langchain.com/hub/rlm/rag-prompt-llama
---

---
layout: iframe
url: https://smith.langchain.com/hub/rlm/rag-prompt-mistral
---

---
layout: iframe-right
url: https://smith.langchain.com/hub/rlm/text-to-sql
---

# 🗄️ SQL

Keywords: Text-to-SQL Query

由于企业数据通常从 SQL 数据库中获取，因此使用 LLM 作为 SQL 查询的自然语言交互入口是一个合理的应用场景
<T>Because enterprise data is often captures in SQL databases, there is great interest in using LLMs as a natural language interface for SQL.</T>

目前已经有[一些论文](https://arxiv.org/pdf/2204.00498.pdf)指出：给定数据表的一些特定信息，LLM 可以生成 SQL，包括每个 `CREATE TABLE` 描述、`SELECT` 语句的三个示例行
<T> A number of papers have reported that LLMs can generate SQL given some specific information about the table, including a CREATE TABLE description for each table followed by three example rows in a SELECT statement.</T>


LangChain 也提供了 [查询 SQL 数据库](https://python.langchain.com/docs/use_cases/qa_structured/sql) 的工具
<T>LangChain has numerous tools for querying SQL databases.</T>


---

# 📝 评价打分（LLM Graders）

Keywords: LLM Evaluators, Bias Checking

把 LLM 用作评分器是一个很有趣的想法，核心思想是利用 LLM 评判一个响应结果和基准答案的匹配度
<T>The central idea of using LLMs as graders is to utilize the discrimination of an LLM to grade a response relative to a ground truth answer.</T>

- [Model Evaluator](https://smith.langchain.com/hub/simonp/model-evaluator)：根据自定义标准对模型或现有的 LangSmith 运行/数据集进行评分
- [Assumption Checker](https://smith.langchain.com/hub/smithing-gold/assumption-checker)：识别查询中的关键假设，然后形成可检查的事实并评估这些假设的问题

<br/>事实上，包括 [OpenAI Cookbook](https://github.com/openai/openai-cookbook/blob/main/examples/evaluation/How_to_eval_abstractive_summarization.ipynb)，以及 [LangChain](tps://twitter.com/hwchase17/status/1692220493657485674)、[LlamaIndex](https://twitter.com/jerryjliu0/status/1703074710077260092) 都展示过这种使用 LLM 来打分的技巧
<T>In fact, this idea that has been broadly showcased in OpenAI cookbooks and open source projects.</T>

<br/>LangSmith 的评价系统也做了很多类似的 [测试和评估](https://docs.smith.langchain.com/evaluation) 功能探索
<T>Much of the work on LangSmith has focused on evaluation support.</T>

---
layout: iframe
url: https://smith.langchain.com/hub/simonp/model-evaluator
---

---
layout: iframe
url: https://smith.langchain.com/hub/smithing-gold/assumption-checker
---

---
layout: two-cols
title: 📚 合成数据生成（Synthetic Data Generation）
---

# 📚 合成数据生成（Synthetic Data Gen）

Keywords: Fine-tuning Data, Evaluation Tests

微调 LLM 是引导 LLM 行为的主要方法之一，但是收集用于微调的训练数据是一个挑战
<T>Fine-tuning LLMs is one of the primary ways to steer LLM behavior, while gathering training data for fine-tuning is a challenge.</T>

<br/>一个思路：使用 LLM 生成微调训练所需要的合成数据集
<T>Many work has focused on using LLMs to generate synthetic datasets.</T>

- [Synthetic Training Data](https://smith.langchain.com/hub/gitmaxd/synthetic-training-data)：OpenAI 训练数据生成
- [Question Answer Pair](https://smith.langchain.com/hub/homanp/question-answer-pair)：基于上下文的问答集合生成

::right::

<Tweet id="1649792913109397508" />

---
layout: iframe
url: https://smith.langchain.com/hub/gitmaxd/synthetic-training-data
---

---
layout: iframe
url: https://smith.langchain.com/hub/homanp/question-answer-pair
---

---
layout: image-right
image: https://pbs.twimg.com/media/F5_OeQdXoAAkAxo?format=jpg&name=medium
---

# 🧠 推论（Reasoning）

Keywords: Chain-of-Thought, Agents (ReAct)

[论文](https://arxiv.org/pdf/2309.03409.pdf)研究表明：[思考链](https://arxiv.org/abs/2201.11903)有助于提升 LLM 的准确率
<T>This has found broad appeal because it improves many reasoning tasks by a large margin and is easy to implement. </T>

- `Let's think step by step`
- `Take a deep breath and work on this problem step-by-step`
- [Tree of Thought](https://arxiv.org/abs/2305.10601)：思考链 ➡️ 思考树

[思考链](https://twitter.com/cwolferesearch/status/1657122778984660993)提示还可以附加到许多任务中，并且对于 [Agent](https://lilianweng.github.io/posts/2023-06-23-agent/) 来说变得尤为重要。例如，[ReAct Agent](https://www.promptingguide.ai/techniques/react) 以交错的方式将工具使用与推理结合起来
<T>Reasoning prompts as shown above can be appended as simple instructions to many tasks and have become particularly important for agents.</T>

---
layout: image
image: https://pbs.twimg.com/media/Fv9G3cFWwAA6yTV?format=jpg&name=4096x4096
---

---
layout: iframe
url: https://smith.langchain.com/hub/shoggoth13/react-chat-agent
---

---
layout: iframe
url: https://smith.langchain.com/hub/jacob/langchain-tsdoc-research-agent
---

---
src: ../../pages/common/refs.md
---

---
src: ../../pages/common/end.md
---
