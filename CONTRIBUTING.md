# Contributing

Thanks for helping improve ProtoCodeBase.Skill.

This repository is a distribution surface for protocol skills. Contributions
should keep skills lightweight, reproducible, and easy for Agent hosts to load.

## What Belongs Here

- Skill wrappers under `skills/<protocol-version>/`
- Lightweight `SKILL.md` instructions
- Compiled protocol references under `skills/<protocol-version>/references/`
- Request / Plan / Change templates under `templates/`
- Deterministic helper scripts under `scripts/`
- Registry updates in `skills/registry.yaml`
- Documentation for installation, versioning, and usage

## What Does Not Belong Here

- Private project state
- User data, local databases, logs, or `.env` files
- Uncompiled protocol source trees
- Large generated artifacts not needed by the skill
- Changes that silently relicense bundled protocol references

## Adding a New Protocol Skill

1. Create a versioned skill directory:

   ```text
   skills/<protocol>-v<major>-<minor>-<patch>/
   ```

2. Add a lightweight `SKILL.md`.
3. Place long protocol material in `references/`.
4. Place reusable authoring templates in `templates/`.
5. Add scripts only when deterministic automation is useful.
6. Update `skills/registry.yaml`.
7. Update `skills/README.md`.

## Skill Writing Principles

- Keep `SKILL.md` short enough to load often.
- Put full protocol text, playbooks, and trace materials under `references/`.
- Be explicit about protocol version compatibility.
- Do not make a skill read an entire repository before it has routed the task.
- Prefer project manifests, project maps, and capability routes over broad scans.

## Pull Requests

Before opening a pull request:

- Verify YAML frontmatter in every `SKILL.md`.
- Verify `skills/registry.yaml` parses.
- Check that bundled references include their own license files when needed.
- Confirm no local secrets or private state are included.

## License Boundary

Repository wrapper code and docs are MIT licensed. Bundled protocol references
may have their own licenses. Do not remove or override those license files.
