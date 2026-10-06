# claude-plugins

A [Claude Code plugin marketplace](https://code.claude.com/docs/en/plugin-marketplaces) for my plugins.

This repo is only the catalog. Each plugin lives in its own repo, and
`.claude-plugin/marketplace.json` points at it.

## Install

Add the marketplace once:

```
/plugin marketplace add kevcooper/claude-plugins
```

Then install any plugin from it:

```
/plugin install <plugin-name>@kevcooper
```

To update, refresh the catalog and then the plugin, and restart Claude Code:

```
claude plugin marketplace update kevcooper
claude plugin update <plugin-name>@kevcooper
```

## Plugins

| Plugin | Description |
| --- | --- |
| [cheapshot](https://github.com/kevcooper/cheapshot) | One-shot Claude inference as an MCP tool, cached locally so repeat requests are free. Needs `uv` and a logged-in `claude` CLI. |

## Adding a plugin

1. Build the plugin in its own repo. The simplest layout puts the plugin at the repo root:

   ```
   .claude-plugin/plugin.json    # plugin manifest (name, version, description, author)
   commands/                     # slash commands (*.md)
   skills/<skill>/SKILL.md       # skills
   agents/                       # subagents (*.md)
   hooks/hooks.json              # hooks
   .mcp.json                     # MCP servers
   ```

   Only `plugin.json` is required. Check it with `claude plugin validate .` in that repo.

2. Add an entry to the `plugins` array in `.claude-plugin/marketplace.json`.

   Plugin at the repo root:

   ```json
   {
     "name": "my-plugin",
     "source": { "source": "github", "repo": "kevcooper/my-plugin" },
     "description": "What it does"
   }
   ```

   Plugin in a subdirectory of its repo (how cheapshot is laid out):

   ```json
   {
     "name": "cheapshot",
     "source": {
       "source": "git-subdir",
       "url": "https://github.com/kevcooper/cheapshot.git",
       "path": "plugins/cheapshot"
     },
     "description": "What it does"
   }
   ```

   Add `"ref": "<tag or branch>"` to the source to pin a release instead of tracking the default branch.

3. Add a row to the table above.

4. Validate and test before pushing:

   ```
   claude plugin validate .
   /plugin marketplace add ./
   /plugin install my-plugin@kevcooper
   ```

Leave `version` out of the marketplace entry so the plugin's own `plugin.json` is the
single source of truth. Bump it there when releasing changes.

CI (`.github/workflows/validate.yml`) runs `claude plugin validate .` on every push and PR.
