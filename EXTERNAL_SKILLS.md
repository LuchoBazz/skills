# External Skills Catalog

A curated list of external skills compatible with Claude Code and Agent SDK-compatible hosts.

---

## Code Quality

### [ponytail](https://github.com/DietrichGebert/ponytail)
Enforces a lazy-coding ruleset — YAGNI, stdlib-first, shortest diff — as an always-on mode with slash commands (`/ponytail-review`, `/ponytail-audit`, `/ponytail-debt`).

**Install:** Add the repo to your agent's skill config (no CLI installer).

---

### [no-mistakes](https://github.com/kunchenguid/no-mistakes)
Gates coding tasks through an AI-driven validation pipeline. Auto-applies safe fixes and escalates judgment calls. Usable as `/no-mistakes <task>` or as a pre-commit gate.

**Install:**

```bash
no-mistakes init
```

---

## Writing & Communication

### [caveman](https://github.com/juliusbrussee/caveman)
Compresses AI agent output to ~65% fewer tokens using fragment-based responses while preserving full technical accuracy. Compatible with 30+ agents.

**Install:**
```bash
# macOS/Linux
curl -fsSL https://raw.githubusercontent.com/JuliusBrussee/caveman/main/install.sh | bash

# Windows
irm https://raw.githubusercontent.com/JuliusBrussee/caveman/main/install.ps1 | iex
```

---

### [humanizer](https://github.com/blader/humanizer/blob/main/SKILL.md)
Strips 33+ patterns of AI-generated writing — inflated language, filler, em-dash overuse, vague attributions — and optionally calibrates to a provided human writing sample.

**Install:** Load as an agent skill in any Claude Agent SDK-compatible host (no CLI installer).

---

## AI / Browser Automation

### [video-use](https://github.com/browser-use/video-use)
Edits video via Claude Code automation — filler removal, color grading, subtitle burning, and animation generation — using `ffmpeg` and `uv` under the hood.

**Install:**
```bash
git clone https://github.com/browser-use/video-use ~/Developer/video-use
ln -sfn ~/Developer/video-use ~/.claude/skills/video-use
cd ~/Developer/video-use && uv sync && brew install ffmpeg
```

---

## Developer Experience

### [agent-skills](https://github.com/addyosmani/agent-skills)
A collection of 24 opinionated Claude Code skills covering the full dev lifecycle — define, plan, build, verify, review, ship — each enforcing a specific engineering discipline.

**Install:**
```bash
# All skills
npx skills add addyosmani/agent-skills

# Single skill
npx skills add addyosmani/agent-skills --skill <skill-name>
```
