# claude-watch

![my-minimal-claude-code-statusline-config-v0-bw0th9wf90mg1](https://github.com/user-attachments/assets/05edca4f-749a-433b-b4da-262f840e0a1c)

## Installation

**1. Copy the scripts**

```sh
cp fetch-usage.sh ~/.claude/fetch-usage.sh
cp statusline-command.sh ~/.claude/statusline-command.sh
chmod +x ~/.claude/fetch-usage.sh ~/.claude/statusline-command.sh
```

**2. Merge `settings.json` into `~/.claude/settings.json`**

Add the `statusLine` and `hooks` blocks from `settings.json` into your existing `~/.claude/settings.json`. If you don't have one yet, copy it directly:

```sh
cp settings.json ~/.claude/settings.json
```

**3. Trigger an initial fetch (optional)**

```sh
bash ~/.claude/fetch-usage.sh
```

The usage cache will otherwise populate automatically on the next tool call or Claude response.

## What it shows

Everything on a single line:

```
Opus 4.6 (1M) | my-project • main | 5h: 3h 12m • 7d: 4d 22h | ctx 45% (430k/1000k)
```

| Segment | Description |
|---------|-------------|
| Model | Active model name (shortened, e.g. "Opus 4.6 (1M)") |
| Folder | Current working directory basename |
| Branch | Git branch (if in a repo) |
| 5h / 7d | Time remaining until rate limit resets |
| ctx | Context window usage (percentage + token counts) |

## How it works

- **`statusline-command.sh`** — reads the JSON piped by Claude Code and renders a single-line status bar with model, folder, branch, rate limit reset countdowns, and context window usage.
- **`fetch-usage.sh`** — reads the OAuth token from the macOS keychain, caches it in `/tmp/.claude_token_cache` for 15 minutes, hits the `/oauth/usage` endpoint (3s timeout), and writes results to `/tmp/.claude_usage_cache`. On failure the stale cache is preserved.
- **`settings.json`** — wires up the statusline command and triggers `fetch-usage.sh` in the background on `PreToolUse` and `Stop` hooks.

## Dependencies

- `jq`
- `curl`
- `git` (optional, for branch display)
