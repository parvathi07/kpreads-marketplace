# kpreads-marketplace

A sample Claude Code plugin marketplace hosted on a public GitHub repo. It has two
plugins:

- `sample-skills`: one skill (`/sample-skills:hello`).
- `helloworld`: a skill, hooks and an agent that all print `[helloworld]`-prefixed
  logs. See [plugins/helloworld/README.md](plugins/helloworld/README.md).

```
kpreads-marketplace/
├── .claude-plugin/marketplace.json        # catalog: lists the plugins
└── plugins/
    ├── sample-skills/
    │   ├── .claude-plugin/plugin.json     # plugin manifest (no "version" on purpose)
    │   └── skills/hello/SKILL.md          # the skill
    └── helloworld/
        ├── .claude-plugin/plugin.json
        ├── skills/hello/SKILL.md          # /helloworld:hello
        ├── agents/greeter.md              # helloworld:greeter
        └── hooks/hooks.json               # [helloworld] hook logs
```

## 1. Publish to GitHub

Create an empty **public** repo named `kpreads-marketplace` on github.com, then from this folder:

```bash
git init
git add .
git commit -m "Initial marketplace"
git branch -M main
git remote add origin https://github.com/<your-github-user>/kpreads-marketplace.git
git push -u origin main
```

## 2. Install it (any tester)

In a Claude Code session:

```
/plugin marketplace add <your-github-user>/kpreads-marketplace
/plugin install sample-skills@kpreads-marketplace
```

Or from a shell:

```bash
claude plugin marketplace add <your-github-user>/kpreads-marketplace
claude plugin install sample-skills@kpreads-marketplace
```

Testers without a GitHub SSH key should set `CLAUDE_CODE_PLUGIN_PREFER_HTTPS=1`
first, or the in-session install and later updates can fail.
(PowerShell: `$env:CLAUDE_CODE_PLUGIN_PREFER_HTTPS = "1"`; to persist it, add it to `env` in `~/.claude/settings.json`.)

Then run `/sample-skills:hello`.

## 3. Turn on auto-update

Auto-update is **off** by default for third-party marketplaces, and
`marketplace.json` can't turn it on. Each tester enables it once:

`/plugin` → **Marketplaces** → `kpreads-marketplace` → **Enable auto-update**

Alternatively, register it through settings (user `~/.claude/settings.json` or a
project's `.claude/settings.json`):

```json
{
  "extraKnownMarketplaces": {
    "kpreads-marketplace": {
      "source": { "source": "github", "repo": "<your-github-user>/kpreads-marketplace" },
      "autoUpdate": true
    }
  },
  "enabledPlugins": {
    "sample-skills@kpreads-marketplace": true
  }
}
```

## How updates reach testers

`plugin.json` and the marketplace entry intentionally have **no `version` field**,
so Claude Code tracks the git commit instead: **every push to `main` is a new
version**. With auto-update on, testers get the change in the background after
their next session starts; `/reload-plugins` or a restart picks it up.

If you ever add `"version": "x.y.z"`, you must bump it on every release, or
testers stay on the cached copy. Don't set it in both files.

Manual update (no auto-update):

```
/plugin marketplace update kpreads-marketplace
```

## Test the auto-update loop

1. Edit `plugins/sample-skills/skills/hello/SKILL.md`, change **revision 1** to **revision 2**.
2. `git commit -am "hello: revision 2"` and `git push`.
3. On the tester machine, start a new Claude Code session (auto-update runs in the background), then `/reload-plugins` or restart.
4. Run `/sample-skills:hello`; it should report **revision 2**.

## Adding more skills

Add a folder `plugins/sample-skills/skills/<skill-name>/SKILL.md` and push. To add
another plugin, create `plugins/<plugin>/.claude-plugin/plugin.json` and add an
entry to `.claude-plugin/marketplace.json`. Validate before pushing:

```bash
claude plugin validate .
```
