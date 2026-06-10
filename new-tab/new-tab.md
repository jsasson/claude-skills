Open a new terminal tab and start a Claude Code session in it. Paired with `remoteControlAtStartup`, this lets any running Claude session spin up additional remotely-accessible sessions.

**Arguments**: Optional — a target directory as a literal path OR a natural language description. Defaults to current working directory if omitted.

---

## Step 1: Resolve the target directory

**If no arguments**: run `pwd` and use that as the target. Skip to Step 2.

**If arguments look like a literal path** (starts with `/`, `~`, or `./`): expand `~` if present using `echo $ARGUMENTS`, verify it exists with `test -d`, then skip to Step 2.

**If arguments are natural language** (e.g., "my interview coach project", "the React app I was working on"):
- Search for likely matches:
  ```bash
  find ~ -maxdepth 4 -type d -iname "*KEYWORD*" 2>/dev/null | head -10
  ```
  Run multiple searches using different keywords extracted from the description.
- Also check common project roots: `~/`, `~/projects`, `~/Documents`, `~/Building`, `~/code`, `~/dev`, `~/work`
- Present the best 1-3 matches with their full paths and ask the user to confirm which one before proceeding.

## Step 2: Confirm the path

Always state the resolved full absolute path and ask: "Open a new Claude tab here: `PATH`?" Wait for confirmation before proceeding. This step is non-negotiable — never open a tab without explicit confirmation.

## Step 3: Detect the running terminal app

```bash
ps aux | grep -iE "iTerm2|Terminal\.app|Warp|Alacritty|Hyper" | grep -v grep | head -5
```

## Step 4: Open the tab

Substitute `TARGET_DIR` with the confirmed path.

**Terminal.app**: Use a shell variable for the path, then write to a temp file and execute:
```bash
TARGET_DIR="/the/confirmed/absolute/path"
printf 'tell application "Terminal" to do script "cd %s && claude"\n' "$TARGET_DIR" > /tmp/open_claude.scpt
osascript /tmp/open_claude.scpt
```

**iTerm2**: Use a shell variable for the path:
```bash
TARGET_DIR="/the/confirmed/absolute/path"
osascript << EOF
tell application "iTerm2"
  tell current window
    set newTab to (create tab with default profile)
    tell current session of newTab
      write text "cd $TARGET_DIR && claude"
    end tell
  end tell
end tell
EOF
```

**Warp**: AppleScript tab automation is unsupported. Fall back to Terminal.app and note the fallback to the user.

**Unknown terminal**: Report what was found, fall back to Terminal.app.

## Step 5: Confirm

Tell the user which terminal was used, the directory the tab opened in, and that Claude is starting there.
