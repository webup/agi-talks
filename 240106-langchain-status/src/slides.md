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
download: 'https://github.com/webup/agi-talks/raw/master/pdfs/240106-langchain-status.pdf'
# syntax highlighter, can be 'prism' or 'shiki'
highlighter: 'shikiji'
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

# LangChain 生态综述

LangChain Ecosystem Walkthrough

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
  <a href="https://github.com/webup/agi-talks/raw/master/pdfs/240106-langchain-status.pdf" target="_blank" alt="Download"
    class="text-xl icon-btn opacity-50 !border-none !hover:text-white">
    <carbon-download />
  </a>
</div>

---
hideInToc: true
layout: intro
---

# 张海立

<div class="leading-8 opacity-80">
KubeSphere Ambassador, CNCF OpenFunction TOC Member<br>
驭势科技 UISEE&copy; 云平台研发总监，开源爱好者<br>
云原生专注领域: Kubernetes, DevOps, FaaS, Observability<br>
AGI 专注领域：LangChain, RAG, Prompt Engineering, GenAI<br>
</div>

<div class="my-10 grid grid-cols-[40px_1fr] w-max gap-y-4 justify-center">
  <ant-design-wechat-outlined class="opacity-50 my-auto"/>
  <div><a href="https://s2.loli.net/2022/10/22/wjYvKo4EWagbJ9L.jpg" target="_blank">zhanghaili</a></div>
  <ant-design-github-outlined class="opacity-50 my-auto"/>
  <div><a href="https://github.com/webup" target="_blank">webup</a></div>
  <ant-design-twitter-outlined class="opacity-50 my-auto"/>
  <div><a href="https://twitter.com/zhanghaili0610" target="_blank">zhanghaili0610</a></div>
  <ant-design-video-camera-filled class="opacity-50 my-auto"/>
  <div><a href="https://space.bilibili.com/28357052" target="_blank">沧海九粟 · Bilibili</a></div>
</div>

<img src="https://s2.loli.net/2022/05/08/XRTdnohGA9rlewu.png" class="rounded-full w-40 abs-tr mt-16 mr-12"/>

---
layout: quote
---

# 🗺️ 开局一张图

从 LangChain 生态全景（架构）图说起

---
layout: iframe
url: https://python.langchain.com/docs/get_started/introduction
---

---
layout: iframe-right
url: https://blog.langchain.dev/the-new-langchain-architecture-langchain-core-v0-1-langchain-community-and-a-path-to-langchain-v0-1/
---

# 🦜️🔗 LangChain 类库

定位：本地构建 LLM 原型应用

- <B>langchain-core</B> 包含了核心抽象和 [LangChain Expression Language](https://python.langchain.com/docs/expression_language/)（LCEL）
  - 版本已经达到 0.1, 未来的任何破坏性变更都会带来小版本升级（0.x）
  - 模块化的抽象为第三方集成提供标准接口
- <B>langchain-community</B> 包含 [第三方集成](https://python.langchain.com/docs/integrations/providers)
  - 主要集成将被进一步拆分为独立软件包
  - 目前，LangChain 拥有近 700 个集成
- <B>langchain</B> 包含了实际运用的 Chain、Agent 和 Retriever 流程策略
  - 官方计划 1/9 推出 0.1 稳定版本

---
layout: iframe
url: https://integrations.langchain.com/
---

---
layout: iframe-right
url: https://blog.langchain.dev/langchain-templates/
---

# 🦜️🔗 LangChain 模板

定位：提供预制的可参考的 LLM 原型应用 

通常配合 LangServe 一起使用（三步走）：

1️⃣ 安装 `langchain-cli`

```sh
pip install -U langchain-cli
```

2️⃣ （创建新项目）导入指定模版

```sh
langchain app new my-app --package $PACKAGE_NAME
langchain app add $PACKAGE_NAME
# adding custom GitHub repo packages
langchain app add --repo $OWNER/$REPO
```

3️⃣ 绑定 LangServe 服务端点（`server.py`）

```py
add_routes(app, chain, path="/chain-path")
```

🚀 启动！`langchain serve`

---
layout: iframe
url: https://templates.langchain.com/
---

---
layout: iframe-right
url: https://blog.langchain.dev/introducing-langserve/
---

# 🦜️🏓 LangServe 类库

定位：一键 REST 服务化 LLM 原型应用 <a href="https://agi-talks.vercel.app/302-langserve/" target="_blank"><mdi-home-export-outline /></a>

<Tweet id="1712526285313102091" />

---
layout: iframe-right
url: https://blog.langchain.dev/announcing-langsmith/
---

# 🦜️⚒️ LangSmith 平台

定位：提供全生命周期的可观测能力

<iframe src="https://docs.smith.langchain.com/" width="100%" height="80%" />


---
layout: quote
---

# 🌰 LangServe + LangSmith 演示

Talk is cheap, show me the [code](https://github.com/webup/langserve-app-demo) or [data](https://smith.langchain.com/public/452ccafc-18e1-4314-885b-edd735f17b9d/d) or [whatever](https://chat.langchain.com/) 🤣

---
src: ../../pages/common/end.md
---
