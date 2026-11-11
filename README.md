# Medical-Literature-Agent · 医药专业文献问答 Agent

> 状态：开发中（第一周）

输入一个化合物、靶点、疾病或医药专业问题，系统自动检索医药文献（PubMed 摘要），
抽取研究设计、剂量、模型/人群、研究终点和结果，生成**带原文引用**的结构化证据回答。

## 定位

- 面向医药科研与研发人员的专业文献证据问答工具；
- 覆盖药效学、药代动力学、安全性/毒理、临床研究等多维度问题；
- 这是科研**调研辅助工具**，不是医学/药学结论生成器；
- 所有结论保留证据来源和不确定性说明，输出不能替代专业医学、药学和毒理学判断。

## 技术路线（详见 docs/architecture.md）

- 语料：PubMed 摘要；起始垂直主题「对乙酰氨基酚」综合文献（药效、药代、安全性、临床使用）
- 架构：单体 RAG 流水线（离线建库 + 在线问答），模块边界按可服务化预留
- 模型：DeepSeek LLM + 本地 BGE embedding
- 检索：FAISS 向量检索 + BM25 关键词检索（混合），后期加 rerank

## 目录结构

```text
medical-literature-agent/
├── data/            # raw=下载的文献摘要; processed=切分后数据; gold_set=人工标注评测集
├── docs/            # 设计文档、dataset card、评测报告等
├── src/
│   ├── ingestion/   # 文献下载、解析、切分
│   ├── retrieval/   # embedding、索引、混合检索、rerank
│   ├── agents/      # （后期）多步工作流
│   ├── tools/       # （后期）Agent 工具封装
│   ├── evaluation/  # 评测脚本（自写指标）
│   └── ui/          # （第 4 周）Streamlit 界面
├── tests/           # 单元测试
├── notebooks/       # 探索性分析
├── scripts/         # 一次性脚本（建库、评测入口等）
└── demo/            # 演示材料
```

## 快速开始（随开发进度更新）

```bash
# 1. 创建并激活虚拟环境
py -3.12 -m venv .venv
.venv\Scripts\activate

# 2. 安装依赖
pip install -r requirements.txt

# 3. 配置密钥
copy .env.example .env   # 然后编辑 .env 填入 API key
```
