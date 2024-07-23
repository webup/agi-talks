---
# See all frontmatter configurations: https://sli.dev/custom/#frontmatter-configures
# theme id or package name, see also: https://sli.dev/themes/use.html
theme: seriph
# titleTemplate for the webpage, `%s` will be replaced by the page's title
titleTemplate: '%s｜WebUP'
# some information about the slides, markdown enabled
info: |
  # LangChain Biweekly Updates
  Sharing key product updates & use case examples.
# favicon, can be a local file path or URL
favicon: https://files.codelife.cc/user-website-icon/20220523/5hyKeZxOknU2owAPvnSWD1388.png?x-oss-process=image/resize,limit_0,m_fill,w_25,h_25/quality,q_92/format,webp
# enabled pdf downloading in SPA build, can also be a custom url
download: https://github.com/webup/agi-talks/raw/master/pdfs/langchain-biweekly.pdf
# syntax highlighter, can be 'prism' or 'shiki'
highlighter: shiki
# controls whether texts in slides are selectable
selectable: false
# enable slide recording, can be boolean, 'dev' or 'build'
record: build
# define transition between slides
transition: fade
# default frontmatter applies to all slides
defaults:

# slide configurations
hideInToc: true
layout: cover
class: text-center
background: https://s2.loli.net/2024/07/22/sv4EPguXlNT7xBS.jpg 
---

# 🦜🔗 LangChain Biweekly

Sharing key product updates & use case examples 

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
  <a href="https://github.com/webup/agi-talks/raw/master/pdfs/langchain-biweekly.pdf" target="_blank" alt="Download"
    class="text-xl icon-btn opacity-50 !border-none !hover:text-white">
    <carbon-download />
  </a>
</div>

---
layout: quote
---

# LangChain Updates 📍 July 01 <sup>2024</sup>

🚀 Releases x 2, ✨ Updates x 3，📝 Tutorials x 1

---
layout: iframe-right
url: https://blog.langchain.dev/langgraph-cloud/
---

# 🚀 实用 5️⃣，难度 4️⃣

- 🦜🕸️ [LangGraph](http://langchain.com/langgraph) 首个稳定版（v0.1）发布
- 🦜☁️ [LangGraph Cloud](https://langchain-ai.github.io/langgraph/cloud/) 封闭测试版现已上线

<br>

###### Feature Recap

LangGraph Cloud 这一全新基础设施将为大规模部署基于 LangChain 的 Agent 铺平道路。

想了解更多？[官方博客](https://blog.langchain.dev/langgraph-cloud/) 已发布博文，不容错过。

<T>LangGraph Cloud was launched in closed beta as a new infrastructure for deploying agents at scale. Read the announcement blog.</T>

---
layout: iframe
url: //player.bilibili.com/player.html?isOutside=true&aid=1206121699&bvid=BV1vf421z7cp&cid=1603007386&p=1&autoplay=0
---

---
layout: iframe-right
url: https://blog.langchain.dev/aligning-llm-as-a-judge-with-human-preferences/
---

# ✨ 实用 4️⃣，难度 2️⃣

- 🦜⚒️ LangSmith 现已升级自我完善评估系统

<br>

###### Feature Recap

您可以直接修正 LLM-as-a-Judge 的评估反馈，系统会自动将其保存为少量样本（Few Shot），从而让 AI 评判更贴合你的需求。

无需手动调整提示。更多信息请参阅 [官方博客](https://blog.langchain.dev/aligning-llm-as-a-judge-with-human-preferences/)。

<T>Self-improving evaluators in LangSmith let you make corrections to LLM evaluator feedback, storing them as few-shot examples to align the LLM-as-a-Judge with your preferences. No manual prompt tweaking needed. Learn more in the blog.</T>

---
layout: iframe
url: https://www.youtube.com/embed/3gCTa0Li4ew?si=xs4GWbiJeAjoUmKQ
---

---
layout: iframe-right
url: https://docs.smith.langchain.com/how_to_guides/tracing/mask_inputs_outputs#rule-based-masking-of-inputs-and-outputs
---

# ✨ 实用 5️⃣，难度 3️⃣

- 🦜⚒️ LangSmith 新增个人信息屏蔽功能

<br>

###### Feature Recap

LangSmith 的个人信息屏蔽功能让您可以通过指定正则表达式列表或为提取的字符串值提供转换方法来创建匿名器。

详细操作指南已在 [官方文档](https://docs.smith.langchain.com/how_to_guides/tracing/mask_inputs_outputs#rule-based-masking-of-inputs-and-outputs) 中提供。

<T>PII masking in LangSmith​­ ​lets you create anonymizers by specifying a list of regular expressions or providing transformation methods for extracted string values. Read the docs.</T>

---
layout: iframe-right
url: https://docs.smith.langchain.com/how_to_guides/playground/custom_endpoint
---

# ✨ 实用 5️⃣，难度 3️⃣

- 🦜⚒️  LangSmith Playground 支持自定义模型

<br>

###### Feature Recap

一旦部署模型服务器，您就可以在 LangSmith Playground 中使用自定义模型。

想知道如何操作？[官方文档](https://docs.smith.langchain.com/how_to_guides/playground/custom_endpoint) 已为您备好细节。

<T>Custom models in LangSmith Playground are available once you deploy a model server. See the docs.</T>

---
layout: two-cols-header
---

# 📝 基于 LangSmith 实施 AI Agent 评测评估 

::left::

🤖 全新三集视频系列 + [详细教程](https://docs.smith.langchain.com/concepts/evaluation#agents)，带您深入了解 AI Agent 评测评估的奥秘。

从响应质量、单步操作到完整工具调用链，全方位剖析评估技巧，帮助您找到评估评测 AI Agent 的要点。

<T>This 3-part video series and tutorial show how to evaluate agent performance — including response eval, single step eval, and the trajectory of tool calls.</T>

::right::

<Youtube id="NbQKDfSw3gM" width="450" />
<Youtube id="AVPflFmRkd4" width="450" />
<Youtube id="pvlT056DAHs" width="450" />