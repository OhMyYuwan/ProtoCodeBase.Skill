<div align="center">

# ProtoCodeBase Skills

> [![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
<br>

ProtoCodeBase 生态系统的 Agent 技能集合。

> [English](./README_EN.md)

</div>


## 技能列表

### protocodebase

统一技能，涵盖：

1. **pcb CLI** — 从 ProtoCodeBase 搜索和下载 ACP 兼容代码库
2. **协议激活** — 为当前项目引导支持的协议（目前支持 ACP）

**触发词：** `acp`、`pcb`、"use acp"、"acp protocol"、"pcb search"、"pcb pull"、"pcb map"、"使用 acp"、"acp 协议"、"搜索代码"

**安装方式：**

```bash
# 通过 npx skills 安装（发布后可用）
npx skills add https://github.com/OhMyYuwan/ProtoCodeBase.Skill --skill protocodebase

```

手动安装
```
cp -r skills/protocodebase ~/.agents/skills/
```

## 使用方式

安装后，以下情况 Agent 会自动激活技能：

- 用户输入 `acp` 或 `pcb`
- 用户请求"搜索 ProtoCodeBase"
- 用户请求"初始化 ACP"或"使用 ACP 协议"
- 项目根目录存在 `acp-protocol/` 目录

## 前置要求

- `protocodebase-cli` npm 包（用于 pcb CLI 命令）
- ProtoCodeBase Token（用于搜索和下载操作）
