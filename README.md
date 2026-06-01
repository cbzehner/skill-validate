# Validate

Prove that a concrete artifact works before continuing or shipping. The skill turns “looks done” into evidence through targeted tests, UI checks, bug repros, or release checks.

## Use It For

- Checking a fix before calling it done
- Validating UI, migrations, plans, or release candidates
- Collecting evidence without also doing delivery work

## Install

Clone the repo and run the installer:

```bash
git clone https://github.com/cbzehner/skill-validate.git
cd skill-validate
./install.sh all
```

Install targets:

- `./install.sh claude` installs to `~/.claude/skills/validate`
- `./install.sh codex` installs to `~/.codex/skills/validate`
- `./install.sh agents` installs to `~/.agents/skills/validate`
- `./install.sh opencode` installs to `~/.config/opencode/skills/validate`
- `./install.sh all --copy` copies files instead of symlinking

Manual install works too: symlink or copy `skills/validate` into your agent's skills directory.

## Agent Support

This repo uses the plain `skills/validate/SKILL.md` layout. Claude Code and Codex also get small plugin manifests at `.claude-plugin/plugin.json` and `.codex-plugin/plugin.json`.

Other agents can read the same `SKILL.md` file. If a host does not support a frontmatter field or tool name, ignore that field and follow the workflow text.

## Layout

```text
.claude-plugin/plugin.json
.codex-plugin/plugin.json
install.sh
skills/validate/SKILL.md
README.md
LICENSE
```

## Public Notes

These repos are public. Keep private repo names, secrets, customer data, raw logs, cookies, and absolute filesystem paths out of examples.

## License

MIT
