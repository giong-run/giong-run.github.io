# giong ai init

> giong 1.5.2 · https://giong.run/docs/cli/ai/ · source: https://github.com/giong-run/Giong/blob/v1.5.2/giong-cli/README.md#giong-ai-init

```sh
giong ai init [--dry-run]
```

Teaches the project to coding agents: `AGENTS.md` (the Gióng rules and this project's commands, read by Codex, Copilot, Cursor, Gemini and others), `CLAUDE.md` (imports `AGENTS.md`), `.cursor/rules/giong.mdc`, the Gióng skill of this release in `.claude/skills/giong/`, and the `giong` server in `.mcp.json`. Run it again at any time: in a file you wrote it replaces only its own marked block, a whole file only while it carries its `giong:owned` line, and in `.mcp.json` it adds its server when missing and keeps everything else. A file it cannot read is left alone, and a `CLAUDE.md` that links to `AGENTS.md` stays a link; it writes through no other link (a repository can commit `.mcp.json` as a link to a file elsewhere) and says so (`GIONG-E5036`). It warns when the project uses another giong version than the command (`GIONG-E5038`).
