# Coding Configuration

## `[自用]` 對 OpenCode Build Agent 制定的設定檔。

### 檔案結構與說明

* **`build-agent-plus.md` (核心 + 全部政策)**
  * 涵蓋語言治理、執行模式、行為護欄、詞彙領域、工具安全、DevOps、CLI 權限分級、MCP 工具決策矩陣、完成定義 (DoD)。
  * 自主迭代工作流 INTENT → EXECUTE → VERIFY → REFLECT（意圖→執行→驗證→反思）的 Resilience Loop 彈性閉環。
  * Agent 在執行任務時，會自動建立 `./temp/` 目錄與執行期狀態檔（Runtime States），確保所有暫存檔案、腳本與測試產物（Artifacts）與主專案嚴格隔離。
  * 已建立 `.gitignore` 將 `./temp/` 排除於版本控制之外。

* **`MCP Tools` (常用 MCP)**
  * [codegraph](https://github.com/colbymchenry/codegraph)

* **`SKILL` (常用 skills)**
  * [ponytail](https://github.com/DietrichGebert/ponytail)
  * [code-review](https://github.com/awesome-skills/code-review-skill)
  * [aha-skills-finder](https://github.com/its-How/aha-skills-finder)
  * [chrome-devtools-cli](https://github.com/aeroxy/chrome-devtools-cli)


---

### opencode.json 配置

* 外部載入與設定檔 (請依 OpenCode 版本調整)

```json
{
  "$schema": "https://opencode.ai/config.json",
  "provider": {
    "api-gateway": {
      "models": {
        "openai": {
          "name": "openai"
        }
      },
      "name": "api-gateway",
      "npm": "@ai-sdk/openai-compatible",
      "options": {
        "baseURL": "https://your-gateway/v1",
        "headers": {
          "Authorization": "Bearer your-key"
        }
      }
    }
  },
  "mcp": {
    "codegraph": {
      "type": "local",
      "command": [
        "codegraph",
        "serve",
        "--mcp"
      ],
      "enabled": true
    }
  },
  "agent": {
    "build": {
      "temperature": 0.2,
      "top_p": 0.9,
      "permission": {
        "*": "allow"
      }
    }
  },
  "default_agent": "build",
  "compaction": {
    "auto": true,
    "prune": true,
    "reserved": 7000
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
    "./.opencode/plugins/ponytail.mjs"
  ],
  "instructions": [
    "https://raw.githubusercontent.com/ss-vip/opencode-agent/refs/heads/main/build-agent-plus.md"
  ]
}
```
