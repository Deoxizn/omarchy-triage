# Routing: where does this report go?

Distilled from the getting-help guide §0 + §3.1 and `reporting.md`.
Pick exactly one. When in doubt, start in Discord.

| Situation | Route |
| --- | --- |
| "How do I…?", "Is this expected?", "Something's off but I'm not sure it's a bug" | `discord-help` → Discord `#omarchy-help` forum |
| Reproducible bug on latest version, in Omarchy itself (scripts, configs, themes, shell + plugins, installer/migrations, its packaging of what it ships) | `github-issue` → GitHub issue on `omacom/omarchy` |
| Feature idea, change to existing feature | `github-discussion` → GitHub Discussions → Suggestions |
| Security vulnerability | `security-private` → repo's private security-reporting policy, never public |
| Bug in an app Omarchy merely ships (browser, editor, library) with no Omarchy packaging/config fault | `upstream` → that app's own support/channel first |

## Decision rules

1. **Discord is the front door.** Most "bugs" are config mistakes, HW quirks, or
   misunderstandings. Confirm there first.
2. **GitHub issues are for verified bugs only.** Template header: "NOT FOR
   SUPPORT REQUESTS". All three required: (a) verified bug in Omarchy's sphere
   on evidence, (b) user explicitly agreed to exact title + body, (c) machine
   can file it (`gh auth status` succeeds).
3. **One problem per post/issue.** Split unrelated symptoms.
4. **Search first.** Discord search + pinned messages, then `gh search issues`
   (open AND closed). Add details to the existing thread when it matches;
   duplicates get closed and split discussion.
5. **App crashes are usually upstream**, not Omarchy — unless Omarchy's own
   packaging/config is implicated. Say so and stop; suggest the right upstream,
   don't file there yourself.

## Name the exact destination

Every recommendation names the precise place, never just "Discord" or "GitHub":

- Discord route → the exact channel name (e.g. `#omarchy-help`).
- GitHub route → issue tracker vs Discussions, plus the category
  (e.g. `Discussions → Suggestions`).
- Never invent channel or category names. If you know the server/repo but not
  the exact channel, say so plainly and let the user pick.

## What to tell the user per route

- `discord-help`: `#omarchy-help` is a forum channel (title + body, stays
  findable). Give them the filled template from `templates.md`, the
  `logs.omarchy.org` URL or attached `omarchy-debug.log`, and 8 essentials
  coverage. Etiquette: don't ask-to-ask, don't ping, don't mark urgent, stay in
  one place, mark solution + close the loop.
- `github-issue`: bug template asks only System details + What's wrong, so
  quality is on us — include summary, repro, expected/actual, frequency,
  diagnostics, minimal repro, regression, workaround, captures. `gh` cannot
  upload media: create via CLI, then drag-drop images/video in web. Paste key
  lines inline because `logs.omarchy.org` links expire after 24h.
- `github-discussion` / `upstream` / `security-private`: explain why, link the
  right place (`https://omarchy.org/discord`, `https://github.com/omacom/omarchy`,
  `/discussions`), and still hand over the full report file so they arrive prepared.
