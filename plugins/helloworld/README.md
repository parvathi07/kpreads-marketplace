# helloworld

A minimal Claude Code plugin showing all three component types. Each one prints
log lines prefixed with `[helloworld]`.

```
helloworld/
├── .claude-plugin/plugin.json   # manifest
├── skills/hello/SKILL.md        # /helloworld:hello
├── agents/greeter.md            # helloworld:greeter subagent
└── hooks/hooks.json             # lifecycle hooks
```

| Component | What it logs |
| --- | --- |
| Skill `/helloworld:hello` | `[helloworld] skill: ...` lines, then calls the greeter agent |
| Agent `helloworld:greeter` | `[helloworld] agent: ...` lines |
| Hooks | `[helloworld] hook: <Event>` on SessionStart, UserPromptSubmit, PreToolUse (Skill), SubagentStart/Stop (greeter), Stop |

Each hook is a plain `echo` of a `{"systemMessage": ...}` JSON object, so it runs
unchanged in bash (macOS, Linux, Git Bash) and in PowerShell (Windows without Git Bash).

## Try it

```
/plugin install helloworld@kpreads-marketplace
/helloworld:hello
```

Or load it straight from this folder without installing:

```
claude --plugin-dir ./plugins/helloworld
```
