# mktmind-api-skill

Client skill for the mktmind API: cited answers about ASX-listed companies,
used inside agent tools. Not an open service — you need a token from
mktmind.

## What the API covers

The API answers from extracted ASX announcement-form facts. Each answer is
cite-or-refuse: every number must be traceable to a cited announcement
page, or the API refuses instead of guessing.

| Form | Kind | What you can ask |
|---|---|---|
| Appendix 3B | raise | Proposed issues behind capital raises: classes, amounts, dates |
| Appendix 3G | issue | Actual issues incl. unquoted/incentive: option series, performance rights, exercise prices, expiries, class totals |
| Appendix 3H | cessation | Securities ceasing: reason family (lapse, expiry, buy-back cancellation, cancellation by agreement), counts, remaining on issue |

Example questions (validated against the ECM workflow suite):

- "What capital raises happened in the week 17–21 August 2026?"
- "What did MetalsTech issue under its incentive scheme in August 2026?"
- "Which companies reported buy-back-driven cessations on Appendix 3H in
  August 2026?"
- "ETM's expired options: how many ceased and at what exercise price?"

Cross-form questions (3G issues minus 3H cessations = net issued-capital
movement for one issuer) are answerable but slower.

## Install

Copy SKILL.md into your agent's skill path (Claude Code:
~/.claude/skills/ask-mm/SKILL.md). Same file works in Codex, OpenCode, and
other skill-reading agents. No local server. No npm.

    export ASK_MM_URL="https://api.mktmind.ai"   # no trailing slash
    export ASK_MM_TOKEN="…"                       # given by mktmind

If those are unset, the skill cannot run.

Tokens are personal: each user gets their own, and it can be revoked
independently. Never log it; never commit it; never share it.

## Current limitations

- **Coverage window.** Facts currently cover August 2026 (ASX appendix
  3B/3G/3H announcements); the lake grows daily but older months are not
  backfilled yet.
- **Buy-back forms (Appendix 3C) are not extracted yet.** Buy-back
  visibility today comes from the 3H cessation side ("cancellation
  pursuant to an on-market/selective buy-back"), not the 3C filings
  themselves.
- **Quotations (Appendix 2A) are not enabled** — new listings/quotation
  questions will refuse.
- **Aggregate tapes cite samples.** Market-wide window answers (e.g. a
  28-name buy-back tape) can carry a single representative citation rather
  than one per name; ask a follow-up per-issuer question to pin a specific
  company.
- **Refusals can oscillate.** The same question may be answered in one
  session and refused in another; a refusal means "could not verify", not
  "no data". Do not hammer retries.
- **Query discipline.** At most two tickers per question; window questions
  need explicit dates at most 14 days apart; calls are slow by design
  (up to two minutes per issuer question, ten for windows).

## Future work

- Appendix 3C (buy-back) extraction and buy-back routing audit
- Appendix 2A (quotation) enablement
- Full historical backfill beyond the current month
- Serving determinism: retry-on-refusal and denser citations for
  aggregate tapes
- Cross-form analyst suites (raise → issue → cessation lifecycle per
  instrument class)
