# insane-search (hoonsung-lang mirror)

This repo is a personal, self-hosted copy of [fivetaku/insane-search](https://github.com/fivetaku/insane-search),
kept here so the plugin can be pulled into **any** Claude Code project — including
`claude.ai/code` cloud/cowork sessions, which cannot reach files on a local machine's
`~/.claude` directory.

## What's different from upstream

- `skills/insane-search/engine/executor.py` and `phase0.py`: `subprocess.run(..., text=True)`
  calls now pin `encoding="utf-8", errors="replace"` instead of letting Python fall back to
  the OS locale encoding (this crashed with `UnicodeDecodeError` on Windows/Korean
  locale (cp949) when a subprocess — Node/Playwright, yt-dlp — wrote non-ASCII bytes).
- `skills/insane-search/engine/__main__.py`: reconfigures `stdout`/`stderr` to UTF-8 at
  startup so emoji in the human-readable summary output no longer crashes with
  `UnicodeEncodeError` on the same Windows locales.

Otherwise this mirrors upstream `insane-search` v0.16.3.

## How to enable this plugin in a project (including cloud/cowork sessions)

Add to that project's `.claude/settings.json`:

```json
{
  "extraKnownMarketplaces": {
    "insane-search-hoonsung": {
      "source": { "source": "git", "url": "https://github.com/hoonsung-lang/insane-search-plugin.git" }
    }
  },
  "enabledPlugins": {
    "insane-search@insane-search-hoonsung": true
  }
}
```

Because this is a normal git-hosted marketplace, it works the same whether Claude Code
is running locally or in a cloud/cowork sandbox that clones the project fresh.

## Updating from upstream

```bash
git clone https://github.com/fivetaku/insane-search.git /tmp/insane-search-upstream
# re-apply the encoding fix to executor.py / phase0.py / __main__.py, then copy
# the result over ./insane-search/ in this repo and commit.
```
