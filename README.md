# omarchy-triage

Tell your AI agent "my computer is doing ___" and get back two things:

1. A short answer in chat: what's wrong, and exactly where to ask for help.
2. A detailed report file with steps to reproduce, logs, system info, and a ready-to-paste help post.

## Before you start

Have these in mind — the agent will ask if you skip them:

- What you were doing when it happened
- What happened vs what you expected
- When it started, and what changed just before (update, new hardware, new settings)

## Install

```bash
git clone https://github.com/Deoxizn/omarchy-triage ~/.agents/skills/omarchy-triage
```

Update later:

```bash
git -C ~/.agents/skills/omarchy-triage pull
```

## Use

1. Open your agent.
2. Describe the problem in plain words.
3. Answer a question or two if it asks.
4. Get your short answer in chat plus a `triage-*.md` file.

Nothing gets posted to Discord or GitHub without you saying so.

## Examples

**Monitor problem → the help channel:**

> My external monitor goes black after suspend and I have to unplug it to fix it. What should I do?

```text
Title: External monitor goes black after suspend on NVIDIA
Problem: monitor stays black after waking up; unplugging fixes it until next time.
Where to go: Discord #omarchy-help — looks like a bug but needs confirming first.
Next step: run `omarchy debug` and attach the log link it gives you.
Full details saved to: ./triage-monitor-black-after-suspend.md
```

Here's what that saved file looks like inside:

```markdown
## TLDR
- **Title:** External monitor goes black after suspend on NVIDIA
- **Where to go:** Discord #omarchy-help
- **Next step:** post the text below + your log link

## What happened
After waking from suspend, the external monitor is black but the
laptop screen works. Unplugging and replugging fixes it until next time.

## Steps to reproduce
1. Plug in the monitor over DisplayPort.
2. Suspend, then wake the machine.
3. External monitor is black. Happens every time.

## System
- Omarchy 4.0.4-1, kernel 6.12.5
- Ryzen 9, NVIDIA RTX 5090, Dell 27" monitor over DP

## Logs (important lines)
  <the key error lines go here>

## Ready-to-paste Discord post
  <filled-in post goes here — just copy it>
```

**Mac problem → the right hardware channel:**

> I put Omarchy on my M2 MacBook Air and the wifi doesn't work. Where do I ask?

```text
Title: Wifi not working on M2 MacBook Air
Problem: no wireless networks show up after install.
Where to go: Discord #m-series — it's Apple-hardware specific.
Next step: run `inxi -Farz` and share the network section.
Full details saved to: ./triage-m2-wifi.md
```

**Crashing app → the app's own developers:**

> My file manager keeps disappearing when I open folders full of pictures. Why?

```text
Title: File manager disappears opening picture folders
Problem: likely crashing while making image thumbnails; checking crash logs.
Where to go: the file manager's own developers, unless Omarchy caused it.
Next step: `coredumpctl list` to see the recent crashes.
Full details saved to: ./triage-file-manager-crash.md
```

**Feature idea → suggestions, not a bug report:**

> It would be nice if the screen recorder could also capture the webcam. Is that a bug?

```text
Title: Screen recorder + webcam overlay (feature idea)
Problem: not a bug — nothing is broken, it's a request for something new.
Where to go: GitHub Discussions → Suggestions. Not Discord, not an issue.
Next step: describe what you'd want it to do; the skill drafts the post for you.
Full details saved to: ./triage-screenrecord-webcam-idea.md
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
