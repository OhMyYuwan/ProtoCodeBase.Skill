# Skills

Install only the protocol skill required by the target project.

## Registry

Machine-readable mapping:

```text
skills/registry.yaml
```

## Available Skills

### `acp-v1-0-0`

Agent Content Protocol v1.0.0 workflow skill.

Use it for repositories that declare:

```yaml
protocol: acp
protocol_version: "1.0.0"
```

or legacy ACP projects with:

```yaml
acp_version: "1.0.0"
```

Install:

```bash
npx skills add https://github.com/OhMyYuwan/ProtoCodeBase.Skill.git --skill acp-v1-0-0
```

Manual install:

```bash
cp -R skills/acp-v1-0-0 ~/.agents/skills/
```

## Skill Layout

Each protocol/version skill should follow this structure:

```text
skills/<protocol-version>/
├── SKILL.md
├── references/
├── templates/
└── scripts/
```

`SKILL.md` should stay lightweight. Put complete protocol documents, compiled
playbooks, trace artifacts, and long-form reference material under
`references/`.

## Roadmap

Future protocol versions can be added independently:

```text
skills/acp-v1-1-0/
skills/other-protocol-v0-1-0/
```
