# mktmind-api-skill

Client skill for the mktmind API: cited answers about ASX-listed companies,
used inside agent tools. Not an open service — you need a token from
mktmind.

## Install

Copy SKILL.md into your agent's skill path (Claude Code:
~/.claude/skills/ask-mm/SKILL.md). Same file works in Codex, OpenCode, and
other skill-reading agents. No local server. No npm.

    export ASK_MM_URL="https://api.mktmind.ai"   # no trailing slash
    export ASK_MM_TOKEN="…"                       # given by mktmind

If those are unset, the skill cannot run.

Tokens are personal: each user gets their own, and it can be revoked
independently. Never log it; never commit it; never share it.
