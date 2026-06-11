# Coding AI Agent Configuration (Resilience Loop)

## `[自用]` 對 OpenCode AI Agent 制定的設定檔。

### 檔案結構與說明

* **`coder-agent.md` (核心 + 全部政策)**
  * 含 8 個核心章節，全面涵蓋語言治理、執行模式、行為護欄、工具安全、DevOps、全端架構、CLI 權限分級、MCP 路由。
  * 自主迭代工作流 Plan → Act → Reflect（規劃、執行、反思）的 Resilience Loop 彈性閉環。
  * Agent 在執行任務時，會自動建立 ./temp/ 目錄與執行期狀態檔（Runtime States），確保所有暫存檔案、腳本與測試產物（Artifacts）與主專案嚴格隔離。

* **`MCP Tools` (常用 MCP 工具)**
  * [pluggedin](https://github.com/VeriTeknik/pluggedin-plugin)
  * [exa-multi-mcp](https://libraries.io/npm/exa-multi-mcp)

* **其他 MCP 工具**
  * [scrapling](https://github.com/D4Vinci/Scrapling)
  * [cloak-browser-mcp](https://npmx.dev/package/@devinwangd/cloak-browser-mcp)
  * [duckduckgo-mcp-server](https://github.com/nickclyde/duckduckgo-mcp-server)
  * [mcp-web-search](https://github.com/tickernelz/mcp-web-search)

* **外部 Skills 連結 (參考)**
  * [code-review](https://github.com/awesome-skills/code-review-skill)

---

### opencode.json 配置

* 外部載入

```json
{
  "$schema": "https://opencode.ai/config.json",

  "instructions": [
    "https://raw.githubusercontent.com/ss-vip/coding-agent/main/coder-agent.md"
  ]
}
```

* 其它 (請依 OpenCode 版本調整)

```json
{
  "$schema": "https://opencode.ai/config.json",

  "agent": {
    "build": {
      "temperature": 0.1,
      "permission": {
        "*": "allow",
        "skill": {
          "*": "allow"
        }
      },
      "tools": {
        "*": true
      }
    }
  },
  "default_agent": "build",
  "compaction": {
    "auto": true,
    "prune_tool_outputs": true,
    "strategy": "summarize",
    "threshold": 0.85
  },
  "watcher": {
    "ignore": [
      "node_modules/**",
      "dist/**",
      ".git/**",
      ".DS_Store",
      "Thumbs.db",
      "**/*.log",
      ".vscode/**",
      ".idea/**",
      ".env*",
      "coverage/**",
      ".wrangler/**",
      "__pycache__/**",
      "*.pyc",
      ".pytest_cache/**",
      "obj/**",
      "bin/**",
      "*.tsbuildinfo",
      ".vercel/**",
      ".netlify/**"
    ]
  },
  "plugin": [
    "opencode-timeout-continuer",
    "@franlol/opencode-md-table-formatter@latest"
  ]
}
```
