# Migration Notice for Original Repositories

This document provides migration notices to be added to the original standalone repositories.

## For cue-console Repository

Add this to the top of `README.md`:

```markdown
# ⚠️ Repository Migrated / 仓库已迁移

**This repository has been merged into the CueStack monorepo.**

**本仓库已合并至 CueStack monorepo。**

---

**New location / 新地址**: [github.com/nmhjklnm/cue-stack](https://github.com/nmhjklnm/cue-stack/tree/main/cue-console)

**Installation unchanged / 安装方式不变**:
```bash
npm install -g cue-console
```

Package name, CLI commands, and functionality remain the same. Only the source repository location has changed.

包名、命令行工具和功能保持不变，仅源码仓库位置变更。

---
```

## For cue-command Repository

Add this to the top of `README.md`:

```markdown
# ⚠️ Repository Migrated / 仓库已迁移

**This repository has been merged into the CueStack monorepo.**

**本仓库已合并至 CueStack monorepo。**

---

**New location / 新地址**: [github.com/nmhjklnm/cue-stack](https://github.com/nmhjklnm/cue-stack/tree/main/cue-command)

**Installation unchanged / 安装方式不变**:
```bash
npm install -g cueme
```

Package name, CLI commands, and functionality remain the same. Only the source repository location has changed.

包名、命令行工具和功能保持不变，仅源码仓库位置变更。

---
```

## For cue-mcp Repository

Add this to the top of `README.md`:

```markdown
# ⚠️ Repository Migrated / 仓库已迁移

**This repository has been merged into the CueStack monorepo.**

**本仓库已合并至 CueStack monorepo。**

---

**New location / 新地址**: [github.com/nmhjklnm/cue-stack](https://github.com/nmhjklnm/cue-stack/tree/main/cue-mcp)

**Installation unchanged / 安装方式不变**:
```bash
uvx --from cuemcp cuemcp
```

Package name, CLI commands, and functionality remain the same. Only the source repository location has changed.

包名、命令行工具和功能保持不变，仅源码仓库位置变更。

---
```

## GitHub Repository Settings

After adding migration notices:

1. **Archive the repositories** (Settings → General → Archive this repository)
2. **Update repository description** to: "⚠️ Migrated to github.com/nmhjklnm/cue-stack"
3. **Add topics**: `archived`, `migrated`, `monorepo`
