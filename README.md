# nk-design

An agent skill for [Claude Code](https://code.claude.com) and [OpenAI Codex](https://developers.openai.com/codex). Build a single-file data tool from one sentence (a review queue, a ledger, a tracker, an admin panel) in a design system where every number shows where it came from and what was not checked.

**What you get.** The starter page every run adapts: 40 invented rows, a first screen that adds up to the cent, and a source on every row. An agent run rewrites the rows, the labels and the arithmetic for the tool you ask for. Recorded on 2026-09-30 with 0.1.9.

![nk-design: a ledger page: a reconciliation bar that adds up to the cent, and rows that each show their source cell](https://raw.githubusercontent.com/NickkkLian/nickkk-skills/main/gallery/results/nk-design.png)

## Try it

Nothing is installed and nothing under `~/.claude` changes: clone, run the self-tests, run the example. It writes only `demo*` files inside the clone.

```bash
git clone https://github.com/NickkkLian/nk-design && cd nk-design
python3 scripts/numsrc.py --selftest
python3 scripts/ui_check.py --selftest
mkdir -p demo && cp assets/starter.html demo/index.html && cp assets/design-tokens.css demo/
python3 scripts/ui_check.py demo/index.html
python3 -c "s = open('demo/index.html').read(); open('demo/planted.html', 'w').write(s.replace('class=\"btn\"', 'class=\"btn btn-primary\"', 1))"
python3 scripts/ui_check.py demo/planted.html
```

Each self-test ends on its own line:

```text
selftest: 50/50
ui_check selftest · 34/34 passed
```

The example commands print this (recorded in a fresh copy with an empty home folder; the path of the clone is taken out):

```text
$ python3 scripts/ui_check.py demo/index.html
✔ demo/index.html: 0 findings
$ python3 scripts/ui_check.py demo/planted.html
✘ demo/planted.html: 1 findings
    C01  2 primary buttons (want exactly 1)
```

Open `demo/index.html`: it is the page in the picture above. The last command exits 1 on purpose: the line before it gave a second button the primary class in a copy, and rule C01 allows one.

### What to type

With the skill installed ([Install](#install)), ask in plain words. This is the request a recorded test run used; it never names the skill:

> I run a two-person bookkeeping practice. Every week we export the week's invoice rows from the bank and one of us has to go through them before anything is booked: some rows have no amount, some match none of our naming rules, and the export usually contains a few exact duplicates.
>
> Build me one page I can open in a browser to work through a week. I want to see at a glance how the week's rows split and that the parts still add up, decide row by row, and be able to answer my colleague when she asks where a number came from. Use made-up rows for now.

![nk-design](https://raw.githubusercontent.com/NickkkLian/nickkk-skills/main/gallery/social/nk-design.png)

Part of [nickkk-skills](https://github.com/NickkkLian/nickkk-skills) — agent skills that ship a self-test with every script; the Verify
section below says which of them were broken on purpose before release to prove they react.

![nk-design demo: one idea in, a finished page out](https://raw.githubusercontent.com/NickkkLian/nickkk-skills/main/gallery/nk-design.gif)

The demo above is a real agent run recorded with 0.1.2 on 2026-09-23. Its second line copies the starter page because that is step 1 of the procedure; the run then rewrote the rows, the labels and the arithmetic for a bookkeeping practice, in 16 turns. 0.1.2 had fourteen machine rules and that page passed all of them; the fifteenth (every marked number opens its source) came with 0.1.5, and that page predates it. Since 0.1.2 the page also draws its icons as line SVG instead of characters.

## What it does

- Seven invariants: a top bar on the anchor band, a mark built by rule, a signature plate whose reconciliation bar is computed from the loaded data and adds up, a provenance chip beside every derived value, a fixed status vocabulary with a second channel besides colour, an honesty footer, one token file.
- `assets/design-tokens.css` (three palettes, each in light and dark, text colours solved for contrast) and `assets/starter.html`, a working workbench with synthetic data: filter, sortable table, row inspector, theme picker, and totals computed on the page.
- `scripts/ui_check.py`: fifteen machine rules, including the appearance contract that restores a saved theme before first paint. `references/acceptance.md` is the separate list of fifteen checks a finished page passes: ten of them name a machine rule, five have none, and only one is settled without opening a browser.
- Every number in the starter's reconciliation bar can be clicked: its rows, how it was computed and what was not checked, from the shared number-sources layer (`numsrc.py`, C15).

The full procedure, the boundaries and where the rules came from are in [SKILL.md](SKILL.md).

## How it works

1. Start from the starter page, not from a blank file: copy `assets/starter.html` and `assets/design-tokens.css` into the same folder.
2. Map the sentence onto the starter's three parts: what one row is (its fields, statuses and source), what the first screen has to add up (the reconciliation bar's segments and total), and what a person decides for a row (the inspector's actions).
3. Keep the seven invariants in `references/invariants.md`.
4. Spend boldness in one place. The deep anchor colour carries the top bar, the signature plate, the footer and the one primary button; one point colour stays on a small share of the page; everything else converges: one display face for titles, one sans for the interface, one monospace face for numbers and identifiers, small radii, borders before shadows.
5. Use the status vocabulary as given (`references/components.md` §tag): Needs review · Approved / Booked / Sent · Rejected / Failed · Draft · Simulated · Excluded · Blocked.
6. Be honest by construction (`references/honesty.md`): synthetic data is invented and the page is labelled `Demo ·`, every number on the page is computed from the loaded data, and nothing simulated shows as sent.
7. Keep the appearance contract. A small script in `<head>`, before the first stylesheet, reads the saved palette and scheme (or `?theme=` / `?scheme=` in the URL) and sets `data-theme` / `data-scheme` before the first paint.
8. Check before you show it.
9. Ship with the page: a 1280-wide screenshot in the README, the demo link, the one-line positioning sentence with its guard clause ("… — every row stays traceable").

## Why it is built this way

**The idea.** A data tool is believed when its numbers can be traced, added up and questioned on the screen, not when it is pretty.

**Where it came from.** The first version was written after four public data tools each shipped with a different look and none of them showed where their numbers came from; the seven invariants, the signature elements and the honesty constraints were written for what a stranger checks in the first thirty seconds.

**Evidence.** What was broken on purpose to show that the self-tests can fail is under [Verify](#verify); what was run end to end, and in which agent, is under [Compatibility](#compatibility).

## Install

Pick one of four ways: three for Claude Code, one for OpenAI Codex. Skills load when a session starts, so open a **new** session after installing.

### 1 · Terminal, one command

```bash
git clone https://github.com/NickkkLian/nk-design ~/.claude/skills/nk-design
```

1. Run the command above (for one project only, clone into `.claude/skills/nk-design` inside that project).
2. Start a new Claude Code session.
3. Check it loaded: type `/nk-design` — it appears in the slash-command menu. Or just ask for the task; the skill triggers on its own.

### 2 · Claude Code in a terminal session (plugin)

The plugin route goes through the [nickkk-skills](https://github.com/NickkkLian/nickkk-skills) marketplace. Add it once; after that each skill is one command.

```
/plugin marketplace add NickkkLian/nickkk-skills
/plugin install nk-design@nickkk-skills
```

1. In a Claude Code session, run the first line (once per machine).
2. Run the second line.
3. Start a new session (or run `/reload-plugins`). The skill shows up as `nk-design:nk-design`.

Without opening a session, the same two steps work from a shell: `claude plugin marketplace add NickkkLian/nickkk-skills` then `claude plugin install nk-design@nickkk-skills`.

### 3 · Claude desktop app (Code tab)

**Add the marketplace first — Discover only searches marketplaces you have already added.**

<img src="https://raw.githubusercontent.com/NickkkLian/nickkk-skills/main/gallery/panel-route/panel-route.gif" alt="Adding the marketplace and installing a skill in the desktop app" width="640">

<sub>Recorded on 2026-09-16, when the marketplace listed ten skills, all at version 0.1.0; it lists more now. The repository list in this recording shows the recorder's own repositories because a GitHub account is connected; yours will show yours. Type the full name as in step 4.</sub>

1. In the chat box, type `/plugin marketplace` and press Enter (or open **Settings → Customize → Plugins**). The **Plugins** panel opens.
   <br><img src="https://raw.githubusercontent.com/NickkkLian/nickkk-skills/main/gallery/panel-route/step1-type-plugin-marketplace.png" alt="/plugin marketplace typed in the chat box" width="480">
2. Top right, open **Add ▾** and choose **Add marketplace**.
   <br><img src="https://raw.githubusercontent.com/NickkkLian/nickkk-skills/main/gallery/panel-route/step2-add-menu.png" alt="The Add menu with Add marketplace" width="480">
3. Choose **Add from a repository**.
   <br><img src="https://raw.githubusercontent.com/NickkkLian/nickkk-skills/main/gallery/panel-route/step3-add-from-repository.png" alt="Add marketplace dialog: Add from a repository" width="480">
4. In **URL**, type the full `NickkkLian/nickkk-skills`. At the bottom of the list choose the row **Use "NickkkLian/nickkk-skills"**, then press **Sync**.
   <br><img src="https://raw.githubusercontent.com/NickkkLian/nickkk-skills/main/gallery/panel-route/step4-url-then-sync.png" alt="URL filled in, Sync button" width="480">
5. You land on **Discover**, filtered to the new marketplace (**Filter · 1**). Find **Nk design** and press **Add**. Installed ones show **✓ Added**.
   <br><img src="https://raw.githubusercontent.com/NickkkLian/nickkk-skills/main/gallery/panel-route/step5-discover-add.png" alt="Discover list with Added and Add buttons" width="480">
6. Close the panel and start a new session.

To try it for one session without installing anything: `claude --plugin-dir ./nk-design` from a clone.

### 4 · OpenAI Codex CLI

```bash
git clone https://github.com/NickkkLian/nk-design.git ~/.agents/skills/nk-design
```

1. Run the command above (for one project only, clone into `.agents/skills/nk-design` inside that project).
2. Start a new Codex session.
3. Check it loaded, without spending a model call: `codex debug prompt-input | grep -o -- '- nk-design[a-z0-9:-]*' | sort -u` prints `- nk-design:nk-design:`. Codex adds the `nk-design:` prefix because this repository also carries a Claude Code plugin manifest. Ask for the task and the skill triggers on its own, or type `$` and pick it from the list.

## Compatibility

| Agent | Tested | What was checked |
|---|---|---|
| Claude Code (CLI 2.1.173, macOS) | yes | In a fresh project with an isolated Claude config, inside a macOS sandbox that blocked reading the tester's ~/.claude folder (settings, session history, memory), Desktop, Documents and Downloads, SSH keys and git identity, a plain request that never names the skill triggered it and it ran its bundled script. The route 2 plugin commands were also run from a shell with an isolated config: marketplace add, install, list. Here the request was a bookkeeping review page, and the run produced an adapted product rather than a copy of the starter: its own product name and status words, a reconciliation equation computed from its own rows, and a provenance chip on every row. This describes the run of 2026-09-23 on version 0.1.2: it finished in 16 turns of a 24-turn limit, the test harness's final assertion passed, the token file came through byte for byte, and scripts/ui_check.py reported 0 findings with the fourteen rules that version had. The fifteenth rule (C15, every marked number opens its source) came with 0.1.5: that page predates it, and today's checker reports it there. An earlier run, on 2026-09-17, ended on a 14-turn limit before the harness's assertion ran. No recorded agent run uses 0.1.5 or later. |
| OpenAI Codex CLI (0.154.0-alpha.6.2, gpt-5.6-sol, low reasoning, macOS) | yes | Copied into `~/.agents/skills` of a temporary home (the folder route 4 clones into), in a fresh project, without the user's Codex config. From a plain request that never names the skill, Codex read SKILL.md, built the page from the starter and ran `scripts/ui_check.py` itself; the page it produced reports 0 findings. |
| Cursor, Gemini CLI | no | Not tested. Their documentation says both read `~/.agents/skills`, the folder route 4 clones into; Gemini CLI asks before it activates a skill. |

In this skill's Codex run, every call into the skill folder's scripts/ used that folder's absolute path. Route 4 was checked for this repository: cloned from GitHub into a temporary home's `~/.agents/skills`, it was listed by the step 3 command. This skill's frontmatter uses only name, description, license and metadata.

## Verify

```bash
python3 scripts/numsrc.py --selftest
python3 scripts/ui_check.py --selftest
```

Standard library only, Python 3.9+. Before publishing, the guarded lines of each script were
mutated one at a time in a sandbox copy and the self-test was confirmed to go red on the named
assertion, without a traceback; the unmutated control stayed green.
numsrc.py, the number-sources layer shared with four other skills: each of its 16 lines that report a
finding was disabled in a sandbox copy, found by reading the source rather than listed by hand, and its self-test went
red each time; the unmutated copy stayed green.

## Limits

- The checker reads the file; it cannot see layout, contrast on a rendered page, or a screenshot. `references/acceptance.md` lists the checks that need a browser and how to do them.
- The token file is a starting palette: change the brand names, and if you change colours keep the contrast pairs (every text colour in the file was solved against the hardest surface of its theme).
- No external component library and no CDN scripts. Web fonts are the only optional external request: the page must open offline with system fonts in their place and nothing else changed.
- The starter ships synthetic data only. Loading real records is the user's decision, and it changes what the footer has to say.

## License

MIT. Read a script before letting it run in your environment.
