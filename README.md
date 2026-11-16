# MedAgent-Clinical

MedAgent-Clinical 是一个多智能体循证问答原型。它用 LangGraph 按任务依赖分波次执行，将规划、工具调用、证据汇总与断言审核串在一起。FastAPI 提供任务接口，SQLite 保存执行状态和 checkpoint，Vue 前端可查看对话、引用和任务进度。

## 运行本地演示

后端默认使用离线模拟模型和合成数据。首次启动会为本机生成 JWT 密钥，运行状态保存在本地。

**后端**

~~~powershell
cd backend
python -m pip install -r requirements-offline.txt
python run.py
~~~

在浏览器打开 http://127.0.0.1:8010/docs 可以查看接口。http://127.0.0.1:8010/health 返回当前运行模式。

**前端**

使用支持 Vite 7 的 Node.js 版本，例如 Node.js 22.12 或以上。在另一个终端运行：

~~~powershell
cd demo-frontend
npm ci
npm run dev
~~~

打开 http://127.0.0.1:5178。可以从页面查看模拟对话及任务进度，也可以连接本机后端，试用注册、对话和任务恢复。

## 代码与评测

- 后端入口和 API 在 backend/app/main.py，工作流和任务调度在 backend/app/workflow.py 和 backend/app/multitask.py。
- 引用和断言审核实现在 backend/app/evidence_context.py 和 backend/app/medical_reviewer.py。
- 公开演示沿用合成数据。后端保留可配置的检索与供应商接口，实际语料和模型由部署者自行准备。
- [60 题历史评测摘要](docs/evaluation/full_60_cases_llm_judge_report.md)提供对照结果。评测以模型裁判为主，用于原型迭代。
