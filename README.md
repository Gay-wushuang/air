# 工程化落地说明（执行版）

本仓库提供 FanClubMaker 的工程化协作文件：
- `CONTRIBUTING.md`：分支/PR/质量门禁/协作规范
- `ROADMAP.md`：M0~M3 里程碑与验收产物
- `docs/api-contract.md`：接口契约与数据口径（冻结字段）
- `.github/*`：Issue/PR 模板

## 快速使用
1) 把本包文件复制到你的仓库根目录（保持目录结构不变）
2) 在 GitHub 打开分支保护：保护 `main`、要求 PR + CI
3) 把 Roadmap 贴到项目看板，把 M0/M1/M2/M3 作为里程碑
4) 第一日任务：冻结 `docs/api-contract.md` v0.1 + 生成 Swagger