# Salaros plugin marketplace

Plugins live in `plugins/<name>/`, each listed in `.claude-plugin/marketplace.json`.

## Writing skills

- Each skill must stand on its own. Never reference, defer to, or tell Claude to load another skill: the reader may not have it installed, and a skill that points at a missing one leaves Claude guessing.
- If a skill depends on a tool (an MCP server, a CLI), name the tool and say what to do when it is absent.
- Run `claude plugin validate .` after changing a plugin or the marketplace.
