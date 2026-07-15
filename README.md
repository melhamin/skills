# skills

My personal collection of agent skills, installable with the [`skills`](https://github.com/vercel-labs/skills) CLI.

## Install

Install all skills:

```bash
npx skills@latest add melhamin/skills
```

Install a single skill:

```bash
npx skills@latest add melhamin/skills/commit-with-conventional-message
```

The CLI fetches skills from this GitHub repo and installs them into your
agent's skills directory (e.g. `.claude/skills/`), symlinked by default.

## Available skills

| Skill | Description |
| --- | --- |
| [`commit-with-conventional-message`](skills/commit-with-conventional-message/SKILL.md) | Commit staged changes with a Conventional Commits message. |
| [`example-skill`](skills/example-skill/SKILL.md) | Template to copy when creating a new skill. |

## Adding a skill

1. Create a folder under `skills/` named in `kebab-case`.
2. Add a `SKILL.md` with YAML frontmatter:

   ```markdown
   ---
   name: my-skill
   description: A specific description of when to use this skill.
   ---

   # My Skill

   Instructions for the agent...
   ```

3. Commit and push. It's immediately installable via
   `npx skills@latest add melhamin/skills/my-skill`.

## License

MIT
