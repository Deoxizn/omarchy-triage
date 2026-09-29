# omarchy-triage

An agent **skill** (not a Quickshell bar plugin) that turns
"my device is doing ___" into two things:

1. a ≤8-line **TLDR** in chat (title, summary, route, next step), and
2. a full **`triage-<slug>.md`** report file (repro, logs, HW/SW, ready-to-post
   Discord / GitHub drafts).

## Why a skill?

- **Skill** = reusable agent instructions (`SKILL.md` + `references/`), triggered
  by describing the problem. That's this flow.
- **OpenCode plugin** = a distributable package that *can contain* skills. Wrap
  this folder later to share it — no rewrite needed.
- **Omarchy Quickshell plugin** (`bar-widget`/`panel` in
  `~/.config/omarchy/plugins/`) = unsandboxed QML UI inside `omarchy-shell`.
  Wrong layer for diagnostic reasoning.

## Layout

```
omarchy-triage/
  SKILL.md                    # workflow: clarify → classify → collect → repro → search → emit
  references/routing.md       # discord-help vs github-issue vs discussion vs upstream vs security
  references/diagnostics.md   # base + symptom-grouped read-only commands, privacy notes
  references/templates.md     # Discord + GitHub templates, gh commands
  references/sources.md       # upstream links (getting-help guide, omarchy skills)
  assets/report-template.md   # triage-<slug>.md skeleton
```

## Install

Prerequisites: `git` and any coding agent. No build step.

```bash
git clone <this-repo-url> ~/Work/omarchy-triage
ln -s ~/Work/omarchy-triage ~/.agents/skills/omarchy-triage
ls ~/.agents/skills/omarchy-triage   # SKILL.md, references/, assets/
```

Update later with `git -C ~/Work/omarchy-triage pull`.
Keep it standalone — don't move it into dotfiles; the symlink is the install.
Generated `triage-*.md` reports are git-ignored; write them from wherever you
run the agent (e.g. `~/Work/`).

Verify: ask your agent what skills it sees, or confirm the symlink resolves.
On this machine `~/.agents/skills/` is the shared skills dir (verified; there
is no `~/.claude/skills` or `~/.config/opencode/skills` here).

## Use

Open an agent (`omarchy agent`, or your agent directly) and describe the symptom:

> My external monitor goes black after suspend, needs a replug. TLDR + where to report?

Expect back: a short TLDR in chat plus a `./triage-<slug>.md` file.
Nothing is filed or posted without your explicit yes to the exact title + body.

## Agent compatibility

`omarchy agent` launches your default coding agent — currently supported:
opencode, agy, copilot, crush, claude, grok, openclaw, codex, cursor-agent,
hermes, muse, omp, ori, pi (see `bin/omarchy-agent` upstream).
This skill is plain `SKILL.md` frontmatter with no harness-specific code, so any
of them can consume it wherever that agent discovers skills. Verified install
path on this box: `~/.agents/skills/omarchy-triage`. If your agent uses a
different skills dir, symlink there instead — the repo itself doesn't change.

Composes with, never replaces:

- `diagnose-crash` (+ `reporting.md`) — crash lens and upstream filing rules.
- `omarchy` skill topic guides — `capture.md`, `hyprland.md`, `theming.md`,
  `hooks.md`, `plugins.md` for evidence gathering and stock-vs-custom isolation.

## Example 1 — display bug → Discord

**You:** "External monitor goes black after every suspend, replug fixes it. TLDR + where to report?"

**Agent (chat):**

```text
Title: External monitor goes black after suspend on NVIDIA
Summary: DP monitor stays black post-resume; panel fine; 100% reproducible.
Route: discord-help — sounds like a bug but unconfirmed, starts in #omarchy-help.
Next: run `omarchy debug` and attach the logs.omarchy.org URL.
Full report: ./triage-monitor-black-after-suspend.md
```

**File** `./triage-monitor-black-after-suspend.md` (excerpt):

```markdown
## TLDR
- **Title:** External monitor goes black after suspend on NVIDIA
- **Summary:** DP monitor black after resume, laptop panel fine.
- **Route:** discord-help — unconfirmed, needs sorting before GitHub.
- **Next step:** post filled template + `omarchy debug` URL to #omarchy-help.

## Reproduction
1. Connect Dell U2723QE over DP at 4K/60.
2. `systemctl suspend`, wake the machine.
3. External monitor is black. Frequency: always.

## System / hardware
- Omarchy: 4.0.4-1, Kernel: 6.12.5-arch1-1
- CPU/GPU: Ryzen 9 9950X / RTX 5090, proprietary driver 570.86.16

## Diagnostics
- `omarchy debug`: <logs.omarchy.org/...> + attached omarchy-debug.log
- `journalctl -b` lines around resume: (inline)
- Tried: `hyprctl reload` (no change), reverted custom keybinding (no change).
```

## Example 2 — crash → upstream check

**You:** "Nautilus keeps vanishing when I open folders with images. Why did it crash?"

**Agent (chat):**

```text
Title: Nautilus disappears opening image-heavy folders
Summary: Likely thumbnailer crash in flight; checking coredump + journal.
Route: upstream (GNOME) unless Omarchy packaging is implicated — TBD.
Next: `coredumpctl list` then `coredumpctl info <pid>`.
Full report: ./triage-nautilus-image-crash.md
```

**File** notes the crash lens: command line from `coredumpctl info`, OOM ruled
out via `free -h`/journal, mtime + journal correlation, all-threads read,
symbolization attempt (or "unsymbolized, shape-only"), proves-vs-infers split,
trash checked for data loss, and the one-program `omarchy-crash-mute` offer.

## Sources

Full links in `references/sources.md`:

- SignalDirective getting-help guide: <https://signaldirective.github.io/omarchy-getting-help.html>
- `diagnose-crash` SKILL + `reporting.md`, `omarchy` skill guides (`quattro`):
  <https://github.com/omacom/omarchy/tree/quattro/default/agents/skills>
- Omarchy Discord / repo / discussions: <https://omarchy.org/discord>,
  <https://github.com/omacom/omarchy>,
  <https://github.com/omacom/omarchy/discussions>
