---
name: omarchy-triage
description: >
  Triage any Omarchy / Arch desktop symptom into a short TLDR plus a full
  report with repro steps, logs, HW/SW info, and routing advice.
  Use when the user says "my device is doing X", asks where to report an
  Omarchy problem, what logs to attach, or wants help writing a Discord
  #omarchy-help post or GitHub issue. Triggers: where to report, bug report,
  discord or github, how to reproduce, what logs, omarchy help, TLDR.
---

# Omarchy Triage

Turn a vague symptom into a report a stranger could act on without
follow-up questions. Work from evidence, not guesses.

Sources distilled here: the Omarchy getting-help guide (Discord `#omarchy-help`
vs GitHub issues, 8 essentials, `omarchy debug`) and the `diagnose-crash`
skill + `reporting.md` (evidence-first crash analysis, strict upstream rules).
Full links in `references/sources.md`.
Details live in `references/` — read the one you need, not all of them.

## Compatibility

Plain `SKILL.md` frontmatter (`name` + `description`), no harness-specific
code. Runs under any coding agent `omarchy agent` can launch (opencode, claude,
codex, crush, copilot, agy, grok, hermes, muse, omp, ori, pi, cursor-agent,
openclaw) wherever that agent discovers skills — on this machine that is
`~/.agents/skills/omarchy-triage` (see README install). Composes with, never
replaces: `diagnose-crash` (+ `reporting.md`) for the crash lens, and the
`omarchy` skill topic guides (`capture.md`, `hyprland.md`, `theming.md`,
`hooks.md`, `plugins.md`) for evidence gathering.

## Non-goals

- Diagnosis reads; it does not fix, tidy, or reconfigure unless the user
  explicitly asks for a fix. Leave the system as found.
- Never file a GitHub issue, comment on one, or post to Discord unprompted.
  Draft text, show exact title + body, wait for yes. See `references/routing.md`.
- Never invent function names, log lines, or stack frames. If symbols or logs
  are missing, say so.

## Workflow

### 0. Clarify the symptom (1 round, then proceed)

Ask only for what blocks triage: what they were doing, what happened vs
expected, when it started / what changed, frequency (always / sometimes / once).
One problem per report — if they list three, triage the first and note the rest.

Test question before finishing: *could a stranger with the same HW/SW
reproduce this using only what I wrote?*

### 1. Classify

Read `references/routing.md` and pick exactly one route:

`discord-help` | `github-issue` | `github-discussion` | `upstream` | `security-private`

Default to `discord-help` when unsure. A GitHub issue requires a reproduced bug
in Omarchy's sphere of control on the latest version — not a "is this a bug?".

If it looks like a crash (segfault, SIGSEGV/SIGABRT, core dump, "process
disappeared", "Process crashed:" notification), apply the crash lens from
`references/diagnostics.md` (coredumpctl, OOM check, timeline correlation,
whole-core read, debuginfod symbolization). Otherwise use the general lens.

### 2. Collect the minimum sufficient evidence

Read `references/diagnostics.md`. Rules:

- Base set first, then only the symptom-relevant group. Do not dump everything.
- Always use `omarchy debug --no-sudo --print` in scripted contexts (never hang
  on a sudo prompt). If `omarchy` is absent (e.g. plain Arch/CachyOS dev box),
  degrade to `inxi -Farz`, `uname -r`, `journalctl -b -p 3`.
- Prefer a time window around the failure (`journalctl --since "10 min ago"`,
  `journalctl -f` while reproducing) over full-boot dumps.
- Cores are verbatim process memory (passwords/tokens/docs). Extract to a fresh
  `mktemp` path, delete when done, never leave in `/tmp`.
- Redact tokens, API keys, emails, MACs, IPs, serials before output. Keep
  structure readable.

### 3. Build a minimal reproduction

Smallest numbered steps from a known starting state. Note stock vs customized
config (`hyprctl configerrors`, revert theme/hook/plugin, `omarchy refresh <app>`
as a test, exact config diff). For crashes: command line from
`coredumpctl info`, recurrence from `coredumpctl list`.

### 4. Search before recommending a new report

Run (when `gh` is authed; otherwise tell the user what to run):

```bash
gh search issues --repo omacom/omarchy "<program> <symptom>"
gh issue list --repo omacom/omarchy --state open --search "<distinctive words>"
```

Include closed issues — a fixed-then-rebroken match is a regression and worth
more than a duplicate. If a match is genuinely the same failure (same trigger +
stack, not just same program), recommend adding a comment with only new evidence
rather than opening a new issue. A bare "me too" is noise — file nothing.

### 5. Emit both outputs

1. **Chat TLDR** (≤8 lines): proposed title (`<object> — <deviation>`, ≤60 chars),
   one-line summary, route with the exact destination (channel name or
   tracker + category, e.g. `Discord #omarchy-help`, `Discussions → Suggestions`)
   + why, single most useful next command or link.
2. **Full report file**: write `./triage-<slug>.md` from
   `assets/report-template.md`. Paste key log lines inline (survives the 24h
   `logs.omarchy.org` expiry); link or attach the full `omarchy-debug.log`.
   Report the file path. Never screenshot text; reserve captures for visual bugs.

If `gh` is missing or unauthenticated, hand over finished title + body text and
say so — do not install or auth it. Sign any filed body with `Filed by <model>
via <harness>` only when certain of the names.

### 6. Offer the next step, once

- Discord route: give the filled `#omarchy-help` template (see
  `references/templates.md`), formatting notes (code fences, short + attached
  full log), and etiquette (no ask-to-ask, no pings, mark solution).
- GitHub route: give the filled issue template + `gh issue create --body-file`
  command, and note media must be drag-dropped via web after.
- Crash explained but recurring: offer `omarchy-crash-mute '<program>'` for that
  one program only, with the unmute command in the same breath. Never run it
  unprompted. Interpreter-keyed programs (e.g. `python3.13`) mute everything on
  that interpreter — say so.
