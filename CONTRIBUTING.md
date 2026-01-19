# FanClubMaker 贡献指南（工程化协作规范）

> 适用团队规模：5 人（2 后端 Java + 3 前端）。
> 目标：**接口契约先行、可验收里程碑推进、CI 质量门禁、主干稳定可发布**。

---

## 1. 分支策略（必须遵守）

### 分支说明
- `main`：发布分支（稳定、可回滚）。**受保护**。
- `dev`：集成分支（每天集成、持续可演示）。
- 功能分支：从 `dev` 拉出，完成后 PR 回 `dev`。

### 分支命名规范
- 功能：`feat/<scope>-<short-desc>`
  - 例：`feat/auth-rbac`、`feat/live-session-api`、`feat/dashboard-streamer`、`feat/obs-overlay`
- 修复：`fix/<scope>-<short-desc>`
  - 例：`fix/pagination-bug`
- 文档：`docs/<short-desc>`
- 重构：`refactor/<short-desc>`

### 合并规则
- **禁止**直接 push 到 `main`。
- `dev -> main` 仅在里程碑（M1/M2/M3）验收通过后合并。
- 所有合并必须走 PR，并满足：
  1) 关联 Issue
  2) CI 全绿
  3) 至少 1 名 Reviewer 通过

---

## 2. 任务管理与 Owner 责任制

### Owner 划分（建议）
- **BE Owner A（平台基础）**：工程骨架、RBAC、审计、导出、统一规范
- **BE Owner B（数据链路）**：事件接入、聚合口径、明细查询、实时推送
- **FE Owner C（主播侧 + 小窗）**：主播面板、桌面小窗 UI、组件与图表
- **FE Owner D（经纪人侧）**：经纪人面板、权限态、筛选对比、导出入口
- **Platform/Delivery Owner E（交付与联调）**：OpenAPI 契约、Mock、CI、验收清单、OBS Browser Source

### Owner 的交付责任
Owner 对其模块的以下事项负责：
- 接口/数据口径对齐
- 单测与边界条件
- 文档更新（README/接口说明/验收口径）
- 联调与验收 demo 可用

---

## 3. 接口契约（OpenAPI）先行

### 规则
- **先定契约再写业务**：字段名、单位、时间口径、分页规范、错误码。
- 前端允许使用 Mock Server 开发；后端先返回 mock 数据也可，但**结构必须与契约一致**。
- 契约变更：必须发 PR，更新 `docs/api-contract.md` 与 OpenAPI 描述，并 @相关 Owner。

### 统一协议（建议）
- 时间：ISO8601、明确时区（默认 UTC+8）
- 金额：明确单位（分/厘/元），建议使用 `long` 表示最小货币单位
- 分页：`page`（从 1 开始）、`size`、`sort=field,asc|desc`
- 过滤：`startTime` / `endTime`（闭区间/开区间在文档写清）

---

## 4. 代码规范与质量门禁（CI）

### 后端（Java）
- 统一格式化：Spotless / Checkstyle（二选一也行，但必须统一）
- 单测：核心 service 必须有（聚合口径/权限判断/幂等等）
- 构建：`mvn -q test` 必须通过

### 前端（Next.js）
- 统一格式化：Prettier
- Lint：ESLint
- 构建：`pnpm -s lint && pnpm -s build`

### PR 必须满足
- CI 全绿
- 无大段未使用代码
- 不引入敏感信息（token/密钥/个人信息）

---

## 5. 提交信息规范（Conventional Commits）

格式：`type(scope): subject`
- `feat:` 新功能
- `fix:` 修复
- `docs:` 文档
- `refactor:` 重构
- `chore:` 工程杂项

例：
- `feat(dashboard): add session summary cards`
- `fix(auth): prevent token refresh loop`

---

## 6. 环境与配置

### 必须做到
- 本地一键启动：`docker-compose up` 起 DB/Redis；前后端各自启动。
- 配置分环境：`application-dev.yml` / `application-prod.yml`（或 `.env`）
- 禁止提交密钥：使用示例文件 `*.example`。

---

## 7. 联调与演示标准

### 每天必须有一个“可演示”的 dev 版本
- 面板至少能打开
- 接口至少能返回（允许 mock）
- 关键路径有降级提示（断线/延迟/无权限）

### 里程碑合入 main 的标准
- 录屏 + 验收 checklist 全部通过
- 部署说明可复现
- 数据口径说明定版