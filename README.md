# new-tab

A Claude Code slash command that opens a new terminal tab and starts a Claude Code session in it.

## The idea

Claude Code has a setting called `remoteControlAtStartup` that automatically makes every session accessible via Remote Control as soon as it starts. When that's enabled, any running Claude session can be used as a launchpad: use `/new-tab` to spin up a new session in any directory, and that new session immediately becomes remotely accessible too.

This is useful when you're away from your machine — on your phone, a tablet, or another computer. As long as you have one Claude session reachable via Remote Control, you can use it to spin up more without ever touching the keyboard.

## Prerequisites

**1. Enable `remoteControlAtStartup` in your Claude settings**

Add this to `~/.claude/settings.json`:

```json
{
  "remoteControlAtStartup": true
}
```

With this enabled, every `claude` process that starts will automatically register itself for Remote Control. No manual setup required per session.

**2. Have at least one Claude session already running and accessible**

You need an existing session you can reach — via the Claude desktop app, claude.ai, or any other Remote Control entry point. That session is what you'll use to run `/new-tab`.

## Installation

Copy `new-tab.md` to your global Claude commands directory:

```bash
cp new-tab/new-tab.md ~/.claude/commands/new-tab.md
```

Create the directory first if it doesn't exist:

```bash
mkdir -p ~/.claude/commands
```

The command is available immediately in any Claude Code session — no restart needed.

## Usage

```
/new-tab
```
Opens a new tab in the current working directory.

```
/new-tab ~/projects/my-app
```
Opens a new tab in the specified path.

```
/new-tab my interview coach project
```
Natural language works too. Claude will search for matching directories, show you the best matches with their full paths, and ask you to confirm before opening anything.

## How the full loop works

1. You're away from your machine — on your phone, for example.
2. You open an existing Claude session via Remote Control (through the Claude app or claude.ai).
3. You type `/new-tab` and describe the project you want to work on.
4. Claude finds the directory, confirms the path, opens a new terminal tab on your machine, and starts Claude there.
5. That new session immediately appears in your Remote Control list because `remoteControlAtStartup` is on.
6. You switch to it and keep working.

## Supported terminals

| Terminal | Support |
|----------|---------|
| Terminal.app (macOS) | Full |
| iTerm2 (macOS) | Full |
| Warp | Fallback to Terminal.app |
| Others | Fallback to Terminal.app |

Linux support is not included yet. Contributions welcome.

## Notes

- The skill always confirms the resolved directory path before opening a tab. It will never take action without your explicit confirmation.
- If you pass a natural language description, Claude searches common project roots (`~`, `~/projects`, `~/Documents`, etc.) up to 4 levels deep.
- This is macOS-only due to the AppleScript dependency.
