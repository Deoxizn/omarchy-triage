# omarchy-triage

Tell your AI agent "my computer is doing ___" and get back two things:

1. A short answer in chat: what's wrong, and where to ask for help (Discord or GitHub).
2. A detailed report file with steps to reproduce, logs, system info, and a ready-to-paste help post.

## Install

```bash
git clone https://github.com/Deoxizn/omarchy-triage ~/.agents/skills/omarchy-triage
```

Update later:

```bash
git -C ~/.agents/skills/omarchy-triage pull
```

## Use

Open your agent and describe the problem in plain words:

> My external monitor goes black after suspend and I have to unplug it to fix it. What should I do?

You'll get back something like this:

```text
Title: External monitor goes black after suspend on NVIDIA
Problem: monitor stays black after waking up; unplugging fixes it until next time.
Where to go: Discord #omarchy-help — looks like a bug but needs confirming first.
Next step: run `omarchy debug` and attach the log link it gives you.
Full details saved to: ./triage-monitor-black-after-suspend.md
```

The saved file has everything a helper needs: exact steps, your system specs,
the important log lines, what you already tried, and a filled-in post you can
copy into Discord or a GitHub issue. Nothing gets posted anywhere without you
saying so.

## Another example

> My file manager keeps disappearing when I open folders full of pictures. Why?

```text
Title: File manager disappears opening picture folders
Problem: likely crashing while making image thumbnails; checking crash logs.
Where to go: probably the file manager's own developers, unless Omarchy caused it.
Next step: `coredumpctl list` to see the recent crashes.
Full details saved to: ./triage-file-manager-crash.md
```

## Works with

Any coding agent Omarchy can open (opencode, claude, codex, crush, copilot,
and the rest). If your agent looks for skills somewhere other than
`~/.agents/skills/`, clone the repo there instead — nothing else changes.

## Sources

- How-to-ask-for-help guide: <https://signaldirective.github.io/omarchy-getting-help.html>
- Omarchy's crash-diagnosis skill: <https://github.com/omacom/omarchy/tree/quattro/default/agents/skills>
- Omarchy Discord / code: <https://omarchy.org/discord>, <https://github.com/omacom/omarchy>

More links in `references/sources.md`.
