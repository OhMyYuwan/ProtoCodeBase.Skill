# ProtoCodeBase Skills

> [中文版](./README.md)

Agent skills for ProtoCodeBase ecosystem.

## Skills

### protocodebase

Unified skill covering:

1. **pcb CLI** — search and download ACP-compatible codebases from ProtoCodeBase
2. **Protocol activation** — bootstrap supported protocols (currently ACP) for projects

**Trigger words:** `acp`, `pcb`, "use acp", "acp protocol", "pcb search", "pcb pull", "pcb map"

**Installation:**

```bash
# Via npx skills (if published)
npx skills add github.com/[your-org]/agent-skills/skills/protocodebase

# Manual installation
cp -r skills/protocodebase ~/.agents/skills/
```

## Usage

Once installed, agents can activate the skill by:

- User typing `acp` or `pcb`
- User requesting "search protocodebase"
- User requesting "initialize acp"
- Agent detecting `acp-protocol/` in project root

## Requirements

- `protocodebase-cli` npm package (for pcb CLI commands)
- ProtoCodeBase token (for search/download operations)

## License

MIT
