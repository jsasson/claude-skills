# claude-skills

A collection of slash commands for [Claude Code](https://claude.ai/code).

Each skill lives in its own folder with a `README.md` explaining what it does and how to install it.

## Skills

| Skill | Description |
|-------|-------------|
| [new-tab](new-tab/) | Open a new terminal tab and start a Claude Code session in it |

## Installation

Each skill is a single `.md` file. Copy it to your global Claude commands directory:

```bash
mkdir -p ~/.claude/commands
cp <skill-name>/<skill-name>.md ~/.claude/commands/<skill-name>.md
```

The command is available immediately in any Claude Code session — no restart needed.

## License

MIT — see [LICENSE](LICENSE).
