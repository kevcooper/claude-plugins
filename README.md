# claude-plugins

A [Claude Code plugin marketplace](https://code.claude.com/docs/en/plugin-marketplaces) for my plugins.

## Install

Add the marketplace once:

```
/plugin marketplace add kevcooper/claude-plugins
```

Then install any plugin from it:

```
/plugin install <plugin-name>@kevcooper
```

Run `/plugin marketplace update kevcooper` to pull new versions.

## Plugins

| Plugin | Description |
| --- | --- |
| [example-plugin](plugins/example-plugin) | Starter plugin with one command and one skill. Copy it to make a new plugin. |

## Layout

```
.claude-plugin/marketplace.json   # marketplace catalog (lists every plugin)
plugins/
  <plugin-name>/
    .claude-plugin/plugin.json    # plugin manifest
    commands/                     # slash commands (*.md)
    skills/<skill>/SKILL.md       # skills
    agents/                       # subagents (*.md)
    hooks/hooks.json              # hooks
    .mcp.json                     # MCP servers
```

Only `plugin.json` is required; include whichever component directories the plugin needs.

## Adding a plugin

1. Copy `plugins/example-plugin` to `plugins/<new-name>` and edit its `plugin.json`.
2. Add an entry to the `plugins` array in `.claude-plugin/marketplace.json` with
   `"source": "./plugins/<new-name>"`.
3. Add a row to the table above.
4. Validate locally:

   ```
   claude plugin validate .
   claude plugin validate plugins/<new-name>
   ```

5. Test it before pushing:

   ```
   /plugin marketplace add ./
   /plugin install <new-name>@kevcooper
   ```

Bump `version` in both `plugin.json` and the marketplace entry when releasing changes, so installed copies pick up the update.

A plugin can also live in its own repo. Point its marketplace entry at it instead of a local path:

```json
{ "name": "my-plugin", "source": { "source": "github", "repo": "kevcooper/my-plugin" } }
```

CI (`.github/workflows/validate.yml`) runs `claude plugin validate` on the marketplace and every plugin for each push and PR.
