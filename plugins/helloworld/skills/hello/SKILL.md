---
name: hello
description: Prints [helloworld]-prefixed log lines and hands off to the helloworld greeter agent. Use when the user says hello world or asks to test the helloworld plugin.
---

# Hello world (skill)

Print these lines exactly, in a code block, each one starting with `[helloworld]`:

```
[helloworld] skill: hello started
[helloworld] skill: Hello, World!
```

Then launch the `helloworld:greeter` agent with the prompt "Say hello world".
When it returns, print its lines unchanged, then finish with:

```
[helloworld] skill: hello finished
```

The plugin's hooks print their own `[helloworld] hook: ...` messages around the
agent run, so you'll see all three components log.
