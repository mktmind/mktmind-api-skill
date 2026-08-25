# mktmind-api-skill

Skill for the mktmind API. Not an open service — you need a URL and token
from mktmind.

## Install

Copy SKILL.md into your agent's skill path (Claude Code:
~/.claude/skills/ask-mm/SKILL.md). Same file works in Codex, OpenCode, and
other skill-reading agents. No local server. No npm.

    export ASK_MM_URL="https://…"     # origin given by mktmind; no trailing slash
    export ASK_MM_TOKEN="…"           # given by mktmind

If those are unset, the skill cannot run.

Token lives in env; never log it; never commit it.
