# AetherRoute-Agent: 端云协同智能路由与任务编排引擎

[![License MIT](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![Python 3.10+](https://img.shields.io/badge/python-3.10+-green.svg)](https://www.python.org/)

AetherRoute 是一个专为企业级应用设计的**端云协同智能路由与任务编排 Agent**。它旨在通过智能语义分析与动态任务拆解，解决多模型部署环境下高昂的 Token 成本和响应延迟问题。

## 🌟 核心特性

- **智能语义路由 (Semantic Routing)**: 核心路由 Agent 能够对用户 Prompt 进行实时语义解析、意图识别与复杂度评估（Complexity Scoring）。
- **任务动态拆解 (Agentic Decomposition)**: 将复杂的长链推理任务拆解为子任务。简单任务路由至边缘端小模型（SLM），核心复杂推理任务则分发至云端高性能大模型 API。
- **状态同步与上下文对齐**: 边缘端与云端共享统一的 Session 上下文与 Key-Value 缓存，确保多 Agent 协同中的长链记忆不丢失。
- **动态 Token 消耗预算控制 (Token-Budget Control)**: 实时监控云端 API 调用频次与 Token 消耗，防止异常流量导致的费用超支。

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
      |  Edge Local SLM     |   |   Cloud Core LLM    |
      |  (Ollama/vLLM)      |   |   (High Performance)|  <-- 申请此处的 API 额度
      +---------------------+   +---------------------+
