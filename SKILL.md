---
name: ask-mm
description: >-
  Ask the mktmind API about ASX-listed companies. Use when the user names
  one or two issuers, or asks a market question scoped to explicit dates
  (for example, "capital raises last week"). Do not use for market-wide
  questions without a date range.
---

# ask-mm

Questions about **named** ASX issuers — or the market within a **date
range** — via the mktmind API.

## Before you call

1. Read `ASK_MM_URL` and `ASK_MM_TOKEN`. If either is missing, stop. Tell
   the user this skill needs a token from mktmind; you cannot invent them.
2. Pick one question shape:
   - **Named issuers.** Extract ticker codes from the user message. Map an
     obvious company name only when the mapping is unambiguous; otherwise
     ask. At most **two** tickers — if the user names more, ask them to
     pick the two that matter. Never invent issuers.
   - **Date-scoped market question** (for example, "what capital raises
     happened last week?"). Turn it into explicit `start` and `end` dates
     in `YYYY-MM-DD` form, at most 14 days apart. If the user gives no
     dates, do not call; ask for a range.
3. If there is no named issuer and no date range, **do not call**.

## Call

    POST $ASK_MM_URL/v1/query
    Authorization: Bearer $ASK_MM_TOKEN
    Content-Type: application/json

    {"q": "<user question>", "tickers": ["BHP"]}

    {"q": "<user question>", "window": {"start": "2026-08-17", "end": "2026-08-21"}}

One request per question, in one shape — `tickers` or `window`, never both.
Do not fan out, do not add issuers the user did not name. These calls are
slow by design: allow at least two minutes (ten for window questions); do
not time out early and retry.

## After

Show `answer` and any `citations` (each carries ticker, date, headline,
page, and a short snippet). If `refused` is true, show `answer` if
non-empty, then **stop**. Do not retry, rephrase, or fetch documents. Do
not explain how the answer was produced.

## Do not

- Market-wide or unnamed-issuer questions without an explicit date range
- More than two tickers
- Treat this as a corpus or search engine
- Download or serve documents
- Retry a refusal
