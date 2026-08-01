# Token Atlas — 静态看板

本地优先的 AI token 用量看板（静态快照版）。

- **数据口径**：聚合 token 元数据，来自本机完整扫描（`usage:scan`），不含 prompt/response 内容。
- **隐私**：不包含具体项目名、项目标签、session 明细；MCP 工具名已移除；无本地路径泄漏。
- **指标**：Fresh = 非缓存 input + output + reasoning；Processed = Fresh + cache reads。7/30/90 天单调。
- **运行方式**：纯静态站点，无需后端；数据打包进前端快照，API 不可达时自动回退显示。

生成流程（在 `token-atlas-fix` 仓库）：

```bash
npm run usage:scan                 # 生成本机完整扫描快照
node scripts/sanitize-public-data.mjs   # 剔除 tools/agents/sessions（防项目标识泄漏）
npm run build:atlas                # 构建前端（打包快照）
node scripts/build-static-site.mjs dist输出路径   # SSR 渲染静态 index.html + client assets
```
