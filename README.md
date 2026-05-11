# AetherRoute-Agent: 端云协同智能路由与任务编排引擎

[![License MIT](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![Python 3.10+](https://img.shields.io/badge/python-3.10+-green.svg)](https://www.python.org/)
[![Built with LangChain](https://img.shields.io/badge/Built%20with-LangChain-red.svg)](https://github.com/langchain-ai/langchain)

AetherRoute 是一个专为企业级应用设计的**端云协同智能路由与任务编排 Agent**。它旨在通过智能语义分析与动态任务拆解，解决多模型部署环境下高昂的 Token 成本和响应延迟问题。

---

## 🌟 核心特性

* **智能语义路由 (Semantic Routing)**：核心路由 Agent 能够对用户输入（Prompt）进行实时语义解析、意图识别与复杂度评估（Complexity Scoring）。
* **任务动态拆解 (Agentic Decomposition)**：将复杂的长链推理任务拆解为子任务。简单对话和基础查询自动路由至边缘本地小模型（SLM），而高难度的长链推理、逻辑分析任务则分发至云端高性能大模型 API。
* **端云上下文同步**：边缘端与云端共享统一的 Session 上下文与 KV 缓存，确保多 Agent 协同中的长链记忆不丢失。
* **动态 Token 预算控制 (Token-Budget Control)**：实时监控云端 API 调用频次与 Token 消耗，防止异常流量导致的费用超支，保障企业用量安全。

---

## 🏗️ 系统架构图

```text
                  +-----------------------+
                  |  User Query (Prompt)  |
                  +-----------+-----------+
                              |
                     [ AetherRoute Agent ]
                              |
                 +------------+------------+
                 | (Evaluating Complexity) |
                 v                         v
       [ Complexity < 0.75 ]     [ Complexity >= 0.75 ]
                 |                         |
                 v                         v
      +---------------------+   +---------------------+
      |   Edge Local SLM    |   |   Cloud Core LLM    |
      |   (Ollama/vLLM)     |   | (High Performance)  | <-- 申请此处的 API 额度
      +---------------------+   +---------------------+
├── README.md               # 项目说明文档
├── requirements.txt        # 依赖配置文件
└── src
    └── router.py           # 核心智能路由逻辑
git clone [https://github.com/your-username/AetherRoute-Agent.git](https://github.com/your-username/AetherRoute-Agent.git)
cd AetherRoute-Agent
pip install -r requirements.txt
export CLOUD_LLM_API_KEY="your_api_key_here"
python src/router.py
