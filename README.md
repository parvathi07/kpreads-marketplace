# kpreads-marketplace

A sample Claude Code plugin marketplace hosted on GitHub at
[parvathi07/kpreads-marketplace](https://github.com/parvathi07/kpreads-marketplace).
It lists one plugin, `helloworld`: a skill, hooks and an agent that all print
`[helloworld]`-prefixed logs. See [plugins/helloworld/README.md](plugins/helloworld/README.md).

```
kpreads-marketplace/
├── .claude-plugin/marketplace.json        # catalog: lists the plugins
└── plugins/helloworld/
    ├── .claude-plugin/plugin.json
    ├── skills/hello/SKILL.md              # /helloworld:hello
    ├── agents/greeter.md                  # helloworld:greeter
    └── hooks/hooks.json                   # [helloworld] hook logs
```

## Install it (any tester)

In a Claude Code session:

```
/plugin marketplace add parvathi07/kpreads-marketplace
/plugin install <plugin>@kpreads-marketplace
```

Or from a shell:

```bash
claude plugin marketplace add parvathi07/kpreads-marketplace
claude plugin install <plugin>@kpreads-marketplace
```

Testers without a GitHub SSH key should set `CLAUDE_CODE_PLUGIN_PREFER_HTTPS=1`
first, or the in-session install and later updates can fail.
(PowerShell: `$env:CLAUDE_CODE_PLUGIN_PREFER_HTTPS = "1"`; to persist it, add it to `env` in `~/.claude/settings.json`.)

## Turn on auto-update

Auto-update is **off** by default for third-party marketplaces, and
`marketplace.json` can't turn it on. Each tester enables it once:

`/plugin` → **Marketplaces** → `kpreads-marketplace` → **Enable auto-update**

Alternatively, register it through settings (user `~/.claude/settings.json` or a
project's `.claude/settings.json`):

```json
{
  "extraKnownMarketplaces": {
    "kpreads-marketplace": {
      "source": { "source": "github", "repo": "parvathi07/kpreads-marketplace" },
      "autoUpdate": true
    }
  }
}
```

## How updates reach testers

Leave `"version"` out of `plugin.json` and the marketplace entry, and Claude Code
tracks the git commit instead: **every push to `main` is a new version**. With
auto-update on, testers get the change in the background after their next session
starts; `/reload-plugins` or a restart picks it up.

If you ever add `"version": "x.y.z"`, you must bump it on every release, or
testers stay on the cached copy. Don't set it in both files.

Manual update (no auto-update):

```
/plugin marketplace update kpreads-marketplace
```

## Adding a plugin

Create `plugins/<plugin>/.claude-plugin/plugin.json`, add components such as
`skills/<skill>/SKILL.md`, `agents/<agent>.md` or `hooks/hooks.json`, and add an
entry to `.claude-plugin/marketplace.json`:

```json
{ "name": "<plugin>", "source": "./plugins/<plugin>", "description": "..." }
```

Validate before pushing:

```bash
claude plugin validate .
```
