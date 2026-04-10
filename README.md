RAG 文档智能问答系统 3.0
基于 RAG + 对话记忆（Memory）+ 重排序（Rerank） 的企业级多轮问答系统
项目简介
本项目是一套前后端分离的智能文档问答系统，通过检索增强生成（RAG）技术，实现对 PDF / DOCX / TXT 文档的精准问答。
系统支持多轮对话记忆、Rerank 重排序优化、来源引用追溯，可直接用于企业内部知识库、面试项目展示。
核心功能
支持 PDF、DOCX、TXT 多格式文档解析
基于 FAISS 向量库 + 嵌入模型实现文档检索
加入 Rerank 重排序，大幅提升回答精度
支持多轮对话记忆（Memory），理解上下文指代
回答带来源引用（文件名 + 页码），可追溯
前后端分离架构，易于部署与扩展
技术栈
后端
FastAPI
LangChain
FAISS 向量库
DashScope 通义千问（Embedding / Rerank / LLM）
前端
Vue3 + Vite
Axios
项目结构
plaintext
项目根目录/
├── backend/            # 后端服务
│   ├── app.py          # 服务入口
│   ├── rag_core.py     # RAG + Memory + Rerank 核心逻辑
│   └── requirements.txt
├── frontend/           # 前端界面
├── documents/          # 文档存放目录
├── .gitignore
└── README.md
快速启动
1. 后端启动
bash
运行
cd backend
pip install -r requirements.txt
python app.py
2. 前端启动
bash
运行
cd frontend
npm install
npm run dev
3. 访问
前端：http://localhost:5173
后端接口文档：http://127.0.0.1:8000/docs
   核心
完整 RAG 工程化实现
Rerank 重排序优化检索精度
多轮对话记忆（Memory）实现指代理解
前后端分离 + 模块化设计
支持来源追溯
基于国产大模型通义千问，适配国内环境
