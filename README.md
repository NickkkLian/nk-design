# nk-design

![nk-design](https://raw.githubusercontent.com/NickkkLian/nickkk-skills/main/gallery/social/nk-design.png)

An agent skill for [Claude Code](https://code.claude.com) and [OpenAI Codex](https://developers.openai.com/codex). Build a single-file data tool from one sentence (a review queue, a ledger, a tracker, an admin panel) in a design system where every number shows where it came from and what was not checked.

Part of [nickkk-skills](https://github.com/NickkkLian/nickkk-skills) — agent skills whose scripts were broken on purpose
before release to prove their checks react.

![nk-design demo: one idea in, a finished page out](https://raw.githubusercontent.com/NickkkLian/nickkk-skills/main/gallery/nk-design.gif)

The demo above was recorded with 0.1.0. Since 0.1.2 the page draws its icons as line SVG instead of characters.

## What it does

- Seven invariants: a top bar on the anchor band, a mark built by rule, a signature plate whose reconciliation bar is computed from the loaded data and adds up, a provenance chip beside every derived value, a fixed status vocabulary with a second channel besides colour, an honesty footer, one token file.
- `assets/design-tokens.css` (three palettes, each in light and dark, text colours solved for contrast) and `assets/starter.html`, a working workbench with synthetic data: filter, sortable table, row inspector, theme picker, and totals computed on the page.
- `scripts/ui_check.py`: thirteen machine rules, including the appearance contract that restores a saved theme before first paint. `references/acceptance.md` is the separate list of fifteen checks a finished page passes: ten of them name a machine rule, five have none, and only one is settled without opening a browser.

The full procedure, the boundaries and where the rules came from are in [SKILL.md](SKILL.md).

## How it works

1. Start from the starter page
2. Map the sentence onto the starter's three parts
3. Keep the seven invariants
4. Spend boldness in one place
5. Use the status vocabulary as given
6. Be honest by construction
7. Keep the appearance contract
8. Check before you show it
9. Ship with the page

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
| Claude Code (CLI 2.1.173, macOS) | yes | In a fresh project with an isolated Claude config, inside a macOS sandbox that blocked reading the tester's ~/.claude folder (settings, session history, memory), Desktop, Documents and Downloads, SSH keys and git identity, a plain request that never names the skill triggered it and it ran its bundled script. The route 2 plugin commands were also run from a shell with an isolated config: marketplace add, install, list. Here the request was a bookkeeping review page, and the run produced an adapted product rather than a copy of the starter: its own product name and status words, a reconciliation equation computed from its own rows, and a provenance chip on every row. scripts/ui_check.py reports 0 findings on what it built, and the token file came through byte for byte. The run used up the 14-turn limit of the test harness while polishing, so it ended on that limit and the harness's final assertion never ran. |
| OpenAI Codex CLI (0.154.0-alpha.6.2, gpt-5.6-sol, low reasoning, macOS) | yes | Copied into `~/.agents/skills` of a temporary home (the folder route 4 clones into), in a fresh project, without the user's Codex config. From a plain request that never names the skill, Codex read SKILL.md, built the page from the starter and ran `scripts/ui_check.py` itself; the page it produced reports 0 findings. |
| Cursor, Gemini CLI | no | Not tested. Their documentation says both read `~/.agents/skills`, the folder route 4 clones into; Gemini CLI asks before it activates a skill. |

In this skill's Codex run, every call into the skill folder's scripts/ used that folder's absolute path. Route 4 was checked for this repository: cloned from GitHub into a temporary home's `~/.agents/skills`, it was listed by the step 3 command. This skill's frontmatter uses only name, description, license and metadata.

## Verify

```bash
python3 scripts/ui_check.py --selftest
```

Standard library only, Python 3.9+. Before publishing, the guarded lines of each script were
mutated one at a time in a sandbox copy and the self-test was confirmed to go red on the named
assertion, without a traceback; the unmutated control stayed green.

## Limits

- The checker reads the file; it cannot see layout, contrast on a rendered page, or a screenshot. `references/acceptance.md` lists the checks that need a browser and how to do them.
- The token file is a starting palette: change the brand names, and if you change colours keep the contrast pairs (every text colour in the file was solved against the hardest surface of its theme).
- No external component library and no CDN scripts. Web fonts are the only optional external request: the page must open offline with system fonts in their place and nothing else changed.
- The starter ships synthetic data only. Loading real records is the user's decision, and it changes what the footer has to say.

## License

MIT. Read a script before letting it run in your environment.
