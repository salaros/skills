# Salaros skills

A [Claude Code plugin marketplace](https://docs.claude.com/en/docs/claude-code/plugin-marketplaces) of small, self-contained skills. Each plugin lives in `plugins/<name>/` and is listed in [`.claude-plugin/marketplace.json`](.claude-plugin/marketplace.json).

## Plugins

| Plugin | Skills | What it does |
|--------|--------|--------------|
| [`loose-ends`](plugins/loose-ends) | `loose-ends` | Keeps a `TODO.md` ledger at the repo root, in [todo-md](https://github.com/todo-md/todo-md) format, for open questions, unverified assumptions and deliberately deferred work. Needs no issue tracker. |
| [`pdd`](plugins/pdd) | `pdd` | Writes a Product Discovery Document: an opportunity assessment of whether an idea is worth pursuing, ending in a `Go`, `No-go` or `Parked` verdict before any BRD is written. |
| [`windows-workstation`](plugins/windows-workstation) | `wsl-bash`, `edge-browser` | Windows conventions: run shell work through WSL2 Bash and Linux tools rather than cmd.exe or PowerShell, and use Microsoft Edge (via the Claude extension) as the browser. |

## Install

Add the marketplace, from a local clone or from its Git URL:

```
/plugin marketplace add <path-or-git-url>
```

Then install the plugins you want:

```
/plugin install loose-ends@Salaros
/plugin install pdd@Salaros
/plugin install windows-workstation@Salaros
```

## Contributing

- Each skill must stand on its own: never reference or defer to another skill, since the reader may not have it installed.
- If a skill depends on a tool (an MCP server, a CLI), name the tool and say what to do when it is absent.
- After changing a plugin or the marketplace, run:

  ```
  claude plugin validate .
  ```
