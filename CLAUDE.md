# branchnew — instructions for Claude Code

`branchnew` is a small **macOS** tool: it splits the current terminal pane and
forks the current Claude Code session into the new pane. Optional iTerm2 add-on:
a **⌘F** hotkey that forks the exact session live in the current pane.

## If the user asks you to install branchnew

Do this, in order:

1. **Sanity check** — macOS only. If `uname` is not `Darwin`, tell the user it
   won't work (it needs iTerm2 / AppleScript / tmux) and stop.

2. **Run the installer.**
   - Inside a checkout of this repo: `./install.sh`
   - Only have the URL (no checkout):
     `curl -fsSL https://raw.githubusercontent.com/limin112/branchnew/main/install.sh | bash`

   This installs `~/.local/bin/branchnew` + the `/branchnew` slash command, and
   adds `~/.local/bin` to PATH if it isn't already.

3. **Ask about the hotkey** — ask: *"Also set up the iTerm2 ⌘F hotkey fork? (iTerm2 only)"*
   - **No** → done. Tell them: type `/branchnew` inside Claude Code to fork (start
     a new session if it doesn't show up yet), or run `branchnew` in a terminal.
   - **Yes** → re-run with `--hotkey` (`./install.sh --hotkey`, or the curl
     one-liner with `| bash -s -- --hotkey`). That also installs the iTerm2 daemon
     and **auto-wires the Claude hooks**. Then give the user the two manual steps
     you CANNOT do yourself:
       1. iTerm2 → Settings → General → Magic → enable **"Enable Python API"**.
       2. **Restart iTerm2**, and click **Allow** when it asks about `claude_fork.py`.
     After that, **⌘F** in a Claude pane forks it.
     ⚠️ Warn them: ⌘F then overrides iTerm2's **Find** (globally). Rebind: see `HOTKEY-FORK.md`.

4. **Verify & report** — run `command -v branchnew` and check
   `~/.claude/commands/branchnew.md` exists; tell the user what got installed and
   how to use it.

## Don't
- Don't try to restart iTerm2, click GUI settings, or toggle the Python API
  yourself — you can't. Instruct the user to do those.
- Don't hand-edit `~/.claude/settings.json` for the hooks — `./install.sh --hotkey`
  already wires them idempotently (and backs the file up to `.bak`).

## More
- Usage & design: `README.md`
- Hotkey system internals & the pane↔session data contract: `HOTKEY-FORK.md`
