---
name: ask-mm
description: >-
  Ask the mktmind API a question about named listed companies.
  Use when the user names one or more issuers.
  Do not use for market-wide or unnamed-issuer questions.
---

# ask-mm

Questions about **named** companies via the mktmind API.

## Before you call

1. Read `ASK_MM_URL` and `ASK_MM_TOKEN`. If either is missing, stop. Tell
   the user this skill needs a URL and token from mktmind; you cannot invent
   them.
2. Extract issuer codes from the user message. Map an obvious name only when
   the mapping is unambiguous; otherwise ask. You need ≥1 ticker. Never invent
   issuers.
3. If there is no named issuer, **do not call**.

## Call

    POST $ASK_MM_URL/v1/query
    Authorization: Bearer $ASK_MM_TOKEN
    Content-Type: application/json

    {"q": "<user question>", "tickers": ["…"]}

One request per question. Pass every named issuer. Do not fan out. Do not
add extras. Wait; do not time out early and retry.

## After

Show `answer` and any `citations`. If `refused` is true, show `answer` if
non-empty, then **stop**. Do not retry, rephrase, or fetch documents. Do not
explain how the answer was produced.

## Do not

- Market-wide or unnamed-issuer questions
- Treat this as a corpus or search engine
- Download or serve documents
- Retry a refusal
