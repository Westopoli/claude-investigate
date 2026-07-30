# claude-investigate

> Evidence-first bug diagnosis for Claude Code. Type `/investigate`, get a root cause. No guesses, no fixes — just proof.

![License](https://img.shields.io/badge/license-MIT-blue)
![Claude Code](https://img.shields.io/badge/Claude%20Code-skill-D97757)
![Status](https://img.shields.io/badge/status-v1.0-green)

[How it works](#how-it-works) • [Install](#install) • [Usage](#usage) • [Isolation techniques](#isolation-techniques)

`/investigate` is a Claude Code slash command that diagnoses bugs without guessing or fixing. You describe the broken behavior; Claude runs isolation rounds — one targeted step per round — until the root cause is backed by direct evidence. Then it hands you a clear HYPOTHESIS / EVIDENCE / SOLUTION and stops. You decide what to fix.

No speculation. No unsolicited rewrites. Just proof.

---

## How it works

1. Type `/investigate` (or describe a bug and Claude activates the skill automatically)
2. Claude states the current symptoms and lists 2–3 candidate causes in order of probability
3. Claude gives you one targeted isolation step: a debug print to add, a log to grep, a value to inspect, a boundary test to run — the single step most likely to discriminate between candidates
4. You run it and report back; Claude eliminates or confirms candidates and repeats
5. Once the root cause is established, Claude outputs **HYPOTHESIS / EVIDENCE / SOLUTION** and stops

Claude never asserts a cause without evidence. If evidence rules out a hypothesis, it says so explicitly and moves on. No implementation until you ask for it.

## Install

Copies the skill into `~/.claude/skills/`. Restart Claude Code, then invoke `/investigate`.

**macOS / Linux**
```bash
curl -fsSL https://raw.githubusercontent.com/Westopoli/claude-investigate/main/install.sh | bash
```

**Windows (PowerShell)**
```powershell
irm https://raw.githubusercontent.com/Westopoli/claude-investigate/main/install.ps1 | iex
```

### Manual install

```bash
# macOS / Linux
git clone https://github.com/Westopoli/claude-investigate
cd claude-investigate
./install.sh
```

```powershell
# Windows
git clone https://github.com/Westopoli/claude-investigate
cd claude-investigate
.\install.ps1
```

### Uninstall

```bash
# macOS / Linux
rm -rf ~/.claude/skills/investigate
```

```powershell
# Windows
Remove-Item -Recurse -Force $env:USERPROFILE\.claude\skills\investigate
```

## Usage

When you hit a bug, type:

```
/investigate
```

Also triggers on: "still broken", "debug this", "isolate the bug", "don't guess prove it", "what's causing this", "prove this hypothesis"

Describe the broken behavior and any error output you have. Claude asks for one piece of evidence at a time. Run each isolation step and paste the result back.

## Isolation techniques

Claude picks from these depending on context:

- **Debug print** — exact code snippet to add at a specific point
- **Narrow repro** — strip the failing case to the smallest input that still fails
- **Boundary test** — call one layer in isolation (direct fn call, raw API hit, single query)
- **Value inspection** — log the exact variable at the moment of failure
- **Diff test** — compare failing vs. passing case to find where they first diverge
- **Log grep** — exact string to grep for, exact log to search

"Add a print here" with a code snippet. Never "check the logs."

## License

MIT. Use it, fork it, ship it.
