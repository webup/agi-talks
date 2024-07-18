---
theme: seriph
titleTemplate: '%s｜WebUP'
background: https://s2.loli.net/2024/07/17/nU2EpACXOPoc3qh.png
favicon: https://files.codelife.cc/user-website-icon/20220523/5hyKeZxOknU2owAPvnSWD1388.png?x-oss-process=image/resize,limit_0,m_fill,w_25,h_25/quality,q_92/format,webp
download: 'https://github.com/webup/agi-talks/raw/master/pdfs/240720-langchain-minio.pdf'
class: text-center
highlighter: shiki
lineNumbers: false
selectable: false
info: |
  LangChain 与 MinIO：基于 GenAI 的数据管理可行性探索
drawings:
  persist: false
transition: slide-left
title: LangChain 与 MinIO 的创新之旅
---

# LangChain 🤝 MinIO

基于 GenAI 的数据管理可行性探索

---
layout: intro
---

# 张海立

<div class="leading-8 opacity-80">
KubeSphere Ambassador, CNCF OpenFunction TOC Member<br>
<a href="https://item.jd.com/14523254.html" target="_blank">《LangChain 实战》</a>作者，开源爱好者和布道师<br>
云原生专注领域：Kubernetes, DevOps, FaaS, Observability<br>
GenAI 专注领域：LangChain, RAG, Evaluation, Prompt + Flow Engineering<br>
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

# 引言：重塑 AI 应用中的数据交互模式
在当今的 AI 应用开发中，如何让用户以更自然、更直观的方式与复杂的数据存储系统交互，是一个关键挑战

<PB>自然语言交互的力量</PB>
- LangChain 使开发者能够构建允许用户通过自然语言与 MinIO 等专业数据存储系统交互的应用
- 这极大地简化了用户操作，使得复杂的数据管理变得直观易懂

<PB>开发效率的跃升</PB>
- 通过 LangChain，开发者可以快速构建智能数据管理接口，它提供了连接 AI 模型和数据存储的简洁框架
- 这大幅缩短了开发周期，使得从概念到上线的过程更加顺畅

<PB>用户体验的质的提升</PB>
- 借助 LangChain 的能力，用户无需学习复杂的查询语言或了解底层存储结构，就能轻松管理和检索数据
- 这为非技术用户打开了数据分析的大门

---
layout: image-right
image: https://s2.loli.net/2024/07/17/kNQHM3Fdoc4Uqrz.png
backgroundSize: 28em 
---

# GenAI 应用开发的助手

LangChain 提供了一套全面的工具和框架

- <B>RAG（检索增强生成）</B>
  - 集成外部知识源，大幅提升信息检索能力
  - 感知上下文，提高输出准确性和相关性

- <B>Agent（智能代理）</B>
  - 构建能自主决策和执行任务的 LLM 实体
  - 灵活调用多种工具和 API，处理复杂请求

- <B>Evaluation（评测评估）</B>
  - 提供评估框架，帮助开发者持续优化流程
  - 自定义评估指标，确保输出符合领域需求


---

<div class="grid grid-cols-2 gap-4 p-4">
  <div v-for="item in [
    {icon: '🦜🔗', title: 'LangChain', url: 'https://langchain.com', description: '构建 LLM 应用的核心框架，提供丰富的组件和工具，支持从简单的对话系统到复杂的 LLM 应用开发'},
    {icon: '🦜🏓', title: 'LangServe', url: 'https://github.com/langchain-ai/langserve', description: '专为 LangChain 应用设计的部署解决方案，简化了 LLM 应用服务的部署和扩展过程，提高了开发效率和系统可靠性'},
    {icon: '🦜⚒️', title: 'LangSmith', url: 'https://smith.langchain.com', description: '强大的调试和监控平台，为开发者提供深入的洞察和优化工具，助力 LLM 应用的性能提升和质量保证'},
    {icon: '🦜🕸️', title: 'LangGraph', url: 'https://langchain-ai.github.io/langgraph/', description: '用于构建复杂多代理系统的创新工具，支持开发者设计和实现高度智能化的协作 LLM 应用系统'}
  ]" 
  class="bg-$slidev-theme-background-secondary border-2 border-$slidev-theme-primary rounded-xl p-4 flex flex-col justify-center items-center text-center transition-all duration-300 hover:scale-105 hover:shadow-lg">
    <div class="text-5xl mb-2">{{ item.icon }}</div>
    <a href="{{ item.url }}" target="_blank"><h2 class="text-$slidev-theme-primary text-xl mb-2">{{ item.title }}</h2></a>
    <p class="text-$slidev-theme-text text-sm">{{ item.description }}</p>
  </div>
</div>

---
layout: image
image: https://python.langchain.com/v0.2/svg/langchain_stack_062024_dark.svg
---

---

# 🌰 打造 AI 驱动的数据助手

以下是实验步骤的简要说明

1. <B>设置 MinIO 客户端</B>：配置连接参数，建立与 MinIO 存储的安全通信通道，为后续操作奠定基础

2. <B>创建自定义工具</B>：设计专门的工具函数，实现对 MinIO 存储的精确操作，如文件上传、下载和元数据管理

3. <B>实现 MinIO Agent</B>：利用 LangChain 框架构建智能代理，将自然语言处理能力与 MinIO 操作工具无缝集成

4. <B>使用 LangServe 创建服务</B>：使用 LangServe 将 Agent 转换为 API 服务，浏览自动生成的 API 端点

<br>

###### 以下内容因为时间关系不做演示

- 设计提示工程：迭代调优提示模板，确保 Agent 能准确理解用户意图，并执行相应的 MinIO 操作
- 测试和优化：通过多轮测试和反馈，不断优化 Agent 性能和用户体验，提高系统的可靠性和效率


---

# 1️⃣ 设置 MinIO 客户端

MinIO 提供了多语言的开发框架，如 Python、JavaScript、Go 等

```python {1-5|8-|all}
from minio import Minio

# 首先初始化 MinIO 客户端
# play.min.io 是一个公共测试服务器，在生产环境中请替换为您自己的服务器
minio_client = Minio('play.min.io:443', access_key='minioadmin', secret_key='minioadmin', secure=True)


# 接下来检查指定 bucket 是否存在，如果不存在则创建
bucket_name = "test"

try:
    # 检查 bucket 是否存在  
    if not minio_client.bucket_exists(bucket_name):
        # 如果 bucket 不存在，则创建
        minio_client.make_bucket(bucket_name)
        print(f"Bucket '{bucket_name}' created successfully.")
    else:
        print(f"Bucket '{bucket_name}' already exists.")
except S3Error as err:
    print(f"Error encountered: {err}")
```

---

# 2️⃣ 创建自定义 MinIO 工具

这些自定义工具允许我们的 LangChain 代理与 MinIO 存储进行精确而高效的交互

```python {4-16|18-|all}{maxHeight:'400px'}
from langchain.agents import tool
import io

@tool
def upload_file_to_minio(bucket_name: str, object_name: str, data_bytes: bytes):
    """
    将文件上传到 MinIO
    
    参数：
        bucket_name (str)：目标存储桶的名称，用于组织和管理对象存储中的文件
        object_name (str)：在存储桶中创建的对象名称，代表文件在 MinIO 中的唯一标识
        data_bytes (bytes)：要上传的文件的原始字节数据，确保数据完整性和安全传输
    """
    data_stream = io.BytesIO(data_bytes)
    minio_client.put_object(bucket_name, object_name, data_stream, length=len(data_bytes))
    return f"文件 {object_name} 成功上传到桶 {bucket_name}。上传过程确保了数据的完整性和安全性。"

@tool
def list_objects_in_minio_bucket(file_info):
    """
    列出 MinIO 桶中的对象，提供存储内容的全面概览
    
    期望 file_info 字典包含'bucket_name'键，指定要查询的存储桶
    返回包含'ObjectKey'和'Size'键的字典列表，详细展示每个对象的名称和大小信息
    """
    bucket_name = file_info['bucket_name']
    response = minio_client.list_objects(bucket_name)
    return [{'ObjectKey': obj.object_name, 'Size': obj.size} for obj in response]
```

---

# 3️⃣ 实现 MinIO Agent

功能目标：基于用户输入执行精确的 MinIO 操作，实现 AI 与云存储的无缝集成

```python {7-12|14-19|21-|all}{maxHeight:'400px'}
from langchain_core.prompts import ChatPromptTemplate
from langchain.agents.format_scratchpad.openai_tools import format_to_openai_tool_messages
from langchain.agents.output_parsers.openai_tools import OpenAIToolsAgentOutputParser
from langchain_core.messages import MessagesPlaceholder
from langchain_core.runnables import RunnableLambda

# 定义提示模板
prompt_template = ChatPromptTemplate.from_messages([
    ("system", "您是一个配备先进文件管理能力的 AI 助手，能够精确理解用户需求并高效执行复杂的数据操作任务。"),
    ("user", "{input}"),
    MessagesPlaceholder(variable_name="agent_scratchpad"),
])

# 绑定工具
upload_file_runnable = RunnableLambda(upload_file_to_minio)
list_objects_runnable = RunnableLambda(list_objects_in_minio_bucket)

tools = [upload_file_to_minio, list_objects_in_minio_bucket]
llm_with_tools = llm.bind_tools(tools)

# 创建代理
agent = (
    {
        "input": lambda x: x["input"],
        "agent_scratchpad": lambda x: format_to_openai_tool_messages(x["intermediate_steps"]),
    }
    | prompt_template
    | llm_with_tools
    | OpenAIToolsAgentOutputParser()
)

# 初始化代理执行器
agent_executor = AgentExecutor(agent=agent, tools=tools, verbose=True)
```

---
layout: two-cols-header
---

# 4️⃣ 用 LangServe 创建应用服务

将 MinIO Agent 作为 REST API 端点暴露在微服务的 /minio 路径

::left::

```python
from fastapi import FastAPI
from langserve import add_routes

# 初始化 FastAPI 应用
app = FastAPI(
    title="MinIO 智能数据管理 API",
    version="1.0",
)

# 添加 LangServe 路由
add_routes(
   app,
   agent_executor.with_config(
       {"run_name": "minio_data_agent"}
   ),
   path="/minio"
)

# 这段代码设置了一个集成 LangServe 的 FastAPI 服务器
# 使得 LLM 驱动的 MinIO 操作可以通过 HTTP 请求轻松调用
```

::right::

- <B>自动 API 生成</B>：LangServe 能够自动为 LangChain 应用创建 RESTful API 端点，大大简化了从开发到部署的过程，加速了 AI 服务的上线和迭代

- <B>内置模式验证</B>：提供强大的输入输出验证机制，确保数据的一致性和安全性，有效防止因数据格式错误导致的系统故障

- <B>无缝集成</B>：与现有 LangChain 代码完美兼容，使得开发者能够轻松将复杂的 LLM 应用转化为可部署的微服务，提高开发效率

- <B>监控和日志</B>：无缝集成 LangSmith 的监控和日志功能，便于开发者跟踪服务性能，快速定位和解决问题

---
layout: iframe-right
url: https://langserve-launch-example-vz4y4ooboq-uc.a.run.app/docs
---

# 调用 MinIO Agent

通过 LangServe SDK 调用 REST API 端点

```python
from langserve import RemoteRunnable

# 连接到已部署的 API
remote_runnable = RemoteRunnable(
  "http://localhost:8000/minio/"
)

# 向代理发送请求
response = remote_runnable.invoke({
  "input": "请列出桶中的所有文件，并计算它们的总大小"
})

print(response)
# 输出 Agent 与 MinIO 交互后的详细响应
# 包括文件列表和总大小统计
```

---
layout: iframe
url: https://smith.langchain.com/public/99e01003-c745-48c3-8563-7f6d7967772d/r
---

---
layout: iframe-right
url: https://blog.langchain.dev/langgraph-cloud/
---

# 未来展望：LangGraph 助力复杂云原生应用

- <B>复杂工作流编排</B>：LangGraph 允许开发者设计和实现复杂的多步骤 LLM 工作流，能够处理需要多个阶段决策和执行的复杂任务

- <B>状态管理优化</B>：通过图结构管理应用状态，使得长期运行的 LLM 任务能够更加稳定和可靠，提高了系统的鲁棒性

- <B>并行处理支持</B>：能够设计并执行并行任务，大大提高了复杂应用的处理效率，特别适合需要多任务协同的云原生环境

- <B>可视化和调试</B>：提供工作流程的可视化工具，更直观地设计和调试复杂的 LLM 应用逻辑


---
layout: iframe
url: //player.bilibili.com/player.html?isOutside=true&aid=1206121699&bvid=BV1vf421z7cp&cid=1603007386&p=1&autoplay=0
---


---
layout: cover
class: text-center
background: https://s2.loli.net/2024/07/17/nU2EpACXOPoc3qh.png
---

# 感谢聆听 🙏

有问题吗？让我们一起讨论！

[LangChain 文档](https://python.langchain.com/docs/get_started/introduction.html) | [MinIO 文档](https://min.io/docs/minio/linux/index.html) | [KubeSphere 文档](https://kubesphere.com.cn/documents/)
