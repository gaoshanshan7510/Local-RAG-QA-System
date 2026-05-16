# 🤖 自动化与机电设备智能问答系统 (Local-RAG-Expert)

![Python](https://img.shields.io/badge/Python-3.8+-blue.svg)
![LangChain](https://img.shields.io/badge/LangChain-Integration-green.svg)
![Streamlit](https://img.shields.io/badge/Streamlit-UI-red.svg)
![Ollama](https://img.shields.io/badge/Ollama-Local_LLM-orange.svg)

## 📖 项目简介
本项目是一个基于**纯本地大模型（Local LLM）**与 **RAG（检索增强生成）** 架构构建的垂直领域知识库问答系统。
系统致力于解决通用大模型在专业机电/自动化设备领域“幻觉”严重的问题，通过外挂本地知识库，为业务人员提供精准、可溯源的技术解答，并支持多轮自然对话。

## ✨ 核心特性
- **🔒 数据零泄露 (100% Local)**：基于 Ollama 本地部署 Qwen2.5 (7B) 大模型，无需调用外部 API，保障企业机密文档绝对安全。
- **🧠 意图识别与查询重写 (Query Rewrite)**：引入 LangChain 的 `history_aware_retriever` 机制。针对用户在多轮对话中的代词指代（如“那它的缺点是什么？”），大模型会结合历史上下文进行语义补全，大幅提升向量检索的准确率。
- **⚡ 极速响应与状态流转 (Performance)**：前端基于 Streamlit 构建。通过 `@st.cache_resource` 实现了大模型与向量数据库的单例模式（Singleton）驻留内存；结合 `session_state` 拦截并转换 UI 状态，实现前后端数据无缝流转与毫秒级响应。
- **🎯 检索调优 (Top-K Tuning)**：通过对 Chroma 向量数据库进行切片策略测试与 Top-K 参数（k=3）调优，在保证召回率的同时，有效控制上下文窗口，避免边缘噪音数据导致的 LLM 幻觉或崩溃。

## 🛠️ 技术栈
- **核心框架**：LangChain
- **大语言模型**：Qwen2.5:7b (Ollama 本地驱动)
- **向量数据库**：Chroma DB
- **前端交互**：Streamlit
- **文本嵌入模型**：nomic-embed-text

## 🚀 快速开始

### 1. 环境准备
确保已安装 Python 3.8+ 并启动 Ollama 服务：

```bash
# 下载并运行本地模型
ollama run qwen2.5:7b
ollama pull nomic-embed-text
```

### 2. 安装依赖

```bash
pip install -r requirements.txt
```

### 3. 放入你的知识库
将需要检索的机电设备说明书、维修手册（支持 .txt 格式）放入 `my_knowledge_docs` 文件夹中。

### 4. 运行系统

```bash
streamlit run app.py
```

在浏览器中打开 `http://localhost:8501` 即可开始与你的专属 AI 专家对话！

## 📂 项目结构 

```mermaid
graph TD
    Root[Local-RAG-Expert 项目] --> A[app.py : 前端UI与状态]
    Root --> B[rag_core.py : RAG核心逻辑]
    Root --> C[(my_knowledge_docs/ : 本地知识库)]
    Root --> D[requirements.txt : 依赖包]
    Root --> E[README.md : 说明文档]
    
    style Root fill:#f9f,stroke:#333,stroke-width:2px
    style C fill:#bbf,stroke:#f66,stroke-width:2px,stroke-dasharray: 5 5

