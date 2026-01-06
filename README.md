# CueStack

<div align="center">

<strong><a href="#english">English</a></strong>
 ·
<strong><a href="#中文">中文</a></strong>

</div>

---

## English

Open the inbox.
Run your agent.
When it needs you, type `cue` — the request appears here.

One inbox for every runtime.

Agents can run for hours. At that point they stop feeling like “tools” and start feeling like “coworkers”.
Coworkers don’t dump their entire context on you — they bring results, questions, and requests. HAP defines that contract; `cue-mcp` implements it.

| Repo | What it is | Link |
| --- | --- | --- |
| `cue-console` | UI inbox (desktop + mobile) | [github.com/nmhjklnm/cue-console](https://github.com/nmhjklnm/cue-console) |
| `cue-mcp` | HAP implementation (MCP server) | [github.com/nmhjklnm/cue-mcp](https://github.com/nmhjklnm/cue-mcp) |

---

## Start here (copy/paste runway)

### 1) UI (`cue-console`)

```bash
npm install -g cue-console
cue-console dev --port 3000
```

Open `http://localhost:3000`.

### 2) MCP server (`cuemcp`)

Recommended runtime command:

- `command`: `uvx`
- `args`: `--from cuemcp cuemcp`

Claude Code:

```bash
claude mcp add --transport stdio cuemcp -- uvx --from cuemcp cuemcp
```

<details>
<summary>Other runtimes (Windsurf / Cursor / Codex / VS Code)</summary>

Windsurf (`~/.codeium/mcp_config.json`):

```json
{
  "mcpServers": {
    "cuemcp": {
      "command": "uvx",
      "args": ["--from", "cuemcp", "cuemcp"]
    }
  }
}
```

Cursor (`mcp.json` in your project):

```json
{
  "mcpServers": {
    "cuemcp": {
      "command": "uvx",
      "args": ["--from", "cuemcp", "cuemcp"],
      "env": {}
    }
  }
}
```

Codex:

```bash
codex mcp add cuemcp -- uvx --from cuemcp cuemcp
```

VS Code:

```json
{
  "servers": {
    "cuemcp": {
      "type": "stdio",
      "command": "uvx",
      "args": ["--from", "cuemcp", "cuemcp"]
    }
  }
}
```

</details>

---

## 中文

打开 inbox。
运行你的 agent。
当它需要你时，输入 `cue` —— 请求就会出现在这里。

一套 inbox，统一所有 runtime。

当 agent 可以离线运行数十个小时，它就不再像“工具”，而更像“同事”。
同事不会把全部上下文砸给你——他们只会带来结果、问题、以及需要你拍板的请求。HAP 定义这份协作契约；`cue-mcp` 把它落到工程里。

| Repo | 作用 | 链接 |
| --- | --- | --- |
| `cue-console` | UI inbox（桌面 + 手机） | [github.com/nmhjklnm/cue-console](https://github.com/nmhjklnm/cue-console) |
| `cue-mcp` | HAP 的具象实现（MCP server） | [github.com/nmhjklnm/cue-mcp](https://github.com/nmhjklnm/cue-mcp) |

---

## 从这里开始（可复制路径）

### 1) UI（`cue-console`）

```bash
npm install -g cue-console
cue-console dev --port 3000
```

打开 `http://localhost:3000`。

### 2) MCP server（`cuemcp`）

推荐 command：

- `command`: `uvx`
- `args`: `--from cuemcp cuemcp`

Claude Code:

```bash
claude mcp add --transport stdio cuemcp -- uvx --from cuemcp cuemcp
```

<details>
<summary>其他 runtime（Windsurf / Cursor / Codex / VS Code）</summary>

Windsurf（`~/.codeium/mcp_config.json`）:

```json
{
  "mcpServers": {
    "cuemcp": {
      "command": "uvx",
      "args": ["--from", "cuemcp", "cuemcp"]
    }
  }
}
```

Cursor（项目内 `mcp.json`）:

```json
{
  "mcpServers": {
    "cuemcp": {
      "command": "uvx",
      "args": ["--from", "cuemcp", "cuemcp"],
      "env": {}
    }
  }
}
```

Codex:

```bash
codex mcp add cuemcp -- uvx --from cuemcp cuemcp
```

VS Code:

```json
{
  "servers": {
    "cuemcp": {
      "type": "stdio",
      "command": "uvx",
      "args": ["--from", "cuemcp", "cuemcp"]
    }
  }
}
```

</details>

### 3) 系统提示词（HAP 规则）

把 HAP 规则写入 runtime 的持久规则/系统提示词：

- Windsurf：Global Rules
- Codex：`agents.md`
- Claude Code：`Claude.md`

<details>
<summary>复制/粘贴：HAP 规则全文</summary>

```text
# Human Agent Protocol

消息表达方式有两种：一种通过 cue mcp 输出，另一种通过非 cue mcp 的方式输出。

当用户接受 Human Agent Protocol 协议时，表示他已完全放弃通过非 cue mcp 的消息交互方式。这意味着你必须使用 cue mcp 才能与其进行交流；该限制仅影响交流方式，不影响你执行 command、使用其他 tool、mcp。

**强制规则**：每次回复前必须调用 `cuemcp` 工具，无例外。任何总结、任何提问、任何请求、任何选项都必须使用 cue mcp来给我。

**约定**：当用户发送 `cue` 即代表他接受Human Agent Protocol协议（支持断开重连）。

- 首次连接 / 无 agent_id：先执行 `join()` 获取 agent_id，再执行 `cue()`。

- 重连 / 已有 agent_id（例如网络波动导致 cue 中断但对话仍在继续）：用户再次发送 `cue` 时，优先直接用已有 agent_id 执行 `cue()`；仅当无法确认 agent_id 时才重新 `join()`。

### 调用时机

- 无历史会话、用户首次输入时调用 `join()` 获取 agent_id
- 每次对话完成后调用 `cue()`
- 任何回复结束前调用 `cue()`

### 禁止行为

- 不调用 `join()` 就使用自定义名字 ❌
- 不调用 `cue()` 就结束你给我的回复 ❌
- 用"有问题请告诉我"替代调用 ❌
- 假设用户没有后续需求 ❌

### 说明

不确定是否调用时默认采用调用 cue mcp 的方式。
```

</details>

### 4) 连接

在你的 agent/runtime 聊天里输入：

`cue`

如果 UI 里出现一条 pending item，就完成了。

### 如果不行（30 秒排错清单）

- `cue-console` 已运行，并打开了 `http://localhost:3000`
- `uvx` 可用（已安装 `uv`），runtime 能启动 MCP server
- 两边都能访问同一个邮箱 DB：`~/.cue/cue.db`
- HAP 规则已注入（否则 agent 不会在回复末尾调用 `cue()`）

---

<details>
<summary>架构图（快速理解）</summary>

```text
Agent/Runtime  ⇄  (MCP stdio)  ⇄  cuemcp  ⇄  ~/.cue/cue.db  ⇄  cue-console
```

```mermaid
flowchart LR
  A[Agent / Runtime
  Claude Code • Cursor • Windsurf • Codex] -->|MCP stdio| B[cuemcp
  MCP server]
  B -->|writes requests
polls responses| C[(~/.cue/cue.db
SQLite mailbox)]
  D[cue-console
UI (desktop/mobile)] <-->|reads/writes| C
  D -->|human responds| C
  C -->|response available| B
  B -->|MCP tool result| A
```

</details>
