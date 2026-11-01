<div align="center">
  <img src="./assets/readme/hero.svg" width="100%" alt="梁迪臻 — 构建可观测、可审计、可回放的长任务 Agent 系统">
</div>

---

## 我做什么

我构建**长任务 Agent 运行时**：把一次需求拆成可执行的工作流，交给多个 Agent 协作完成，并把每一步落成**可查询、可回放**的记录。当执行到第 87 步失败时，你能知道是谁、在哪一步、为什么失败，而不是从头再跑一遍。

核心难点不在「让模型会调用工具」，而在**长任务的正确性**——崩溃后能否确定性恢复、并发时状态以谁为准、副作用会不会重复执行。

## 代表作 · Apex-Agent

> 私有项目。以下为可公开的架构与工程事实；欢迎邮件交流或索取演示。

**单权威架构**：执行权威只有一个（Temporal Workflow），其余全部是派生视图。架构上不给双写留口子，而不是靠约定。

```text
需求 → Temporal Workflow（唯一权威）→ Activity（单写入口）
     → PostgreSQL 派生投影 → Live Canvas / 看板 / 轨迹
     失败时从 Workflow 确定性重放
```

### 工程规模

| 指标 | 数值 |
|---|---:|
| 提交数 | 1,205 |
| 自研代码行数 | 231,810（`src` + `tests` + `scripts`） |
| API 路由 | 61 |
| 测试文件 | 307 |
| Temporal 工作流 / 活动 | 15 / 19 |

<sub>数字由 `git rev-list` / `git ls-files` 在工作区 HEAD 复算，非估算。</sub>

### 四条不变量，都被「会变红的守卫」钉住

| 不变量 | 怎么钉住 |
|---|---|
| **执行权威唯一** | 工作流状态由 Temporal 持久化，崩溃后可确定性重放；工作流体不读 `process.env`、只 type-only 引用 Activity |
| **写入方唯一** | 看板 / 轨迹 / 会话树都是派生投影，只能由同步 Activity 写入（静态卡口 `test:tc11`） |
| **副作用可追踪** | 所有外部副作用集中在 Activity 层，重放时不会重复执行 |
| **边界显式** | 工具「能调用」与「被授权」是两个平面（MCP 工具面 × Eunomia 授权面） |

## 工程信条

- **断言要能变红** —— 一条永远不会失败的断言不是测试，是装饰。
- **静态卡口 > 运行时约定** —— 写路径收敛在单文件，静态检查可证伪、零运行时成本。
- **fail-closed > fail-open** —— allowlist 门禁 fail-open 等于没有门禁。
- **诚实面对统计效力** —— 没有样本量支撑的结论，宁可不报，也不编造显著性。

## 技术栈

`TypeScript` · `Next.js` · `Temporal` · `PostgreSQL` · `Kafka` · `Milvus` · `Redis` · `Docker` · `Kubernetes` · `MCP`

## 联系

- 在线主页：[dizhenliang.github.io/apex-agent-cloud](https://dizhenliang.github.io/apex-agent-cloud/)
- GitHub：[@DizhenLiang](https://github.com/DizhenLiang)
- 邮箱：[liangdizhen@qq.com](mailto:liangdizhen@qq.com)

<!--
  发布说明（不属于渲染内容）：
  1. 本目录 github-profile/ 即 GitHub Profile 仓库结构（README.md + assets/readme/hero.svg）。
  2. 新建同名仓库 DizhenLiang/DizhenLiang 并推送本目录内容即可生效。
  3. Apex-Agent / apex-agent-cloud 当前为私有仓库；若日后转为公开，
     可在「代表作 · Apex-Agent」标题下补上仓库与部署链接。
-->
