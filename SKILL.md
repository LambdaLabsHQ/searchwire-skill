---
name: searchwire
description: Look up SEO data for a task - keyword search volume, Google results for a keyword, whether domains are registered, and whether a product name is free (domains, GitHub, npm). Use when choosing keywords, sizing demand, checking competitors in Google, or naming a product.
---

# Searchwire

Base URL: https://searchwire.dev. Full docs: https://searchwire.dev/llms.txt

Calls are POST with a JSON body. Send `Authorization: Bearer $SEARCHWIRE_KEY` when you have a key.
Without a key, domain and name checks and cached data work for 20 calls a day, 10 keywords a call.

- Search volume (batch up to 1,000 keywords in one call, it is cheaper):
  `curl -s https://searchwire.dev/v1/keywords/volume -H "Authorization: Bearer $SEARCHWIRE_KEY" -H "content-type: application/json" -d '{"keywords":["ai wiki","living docs"],"country":"us"}'`
- Google results: `{"keyword":"ai wiki","country":"us","depth":10}` to https://searchwire.dev/v1/serp
- Domains: `{"domains":["example.dev","example.com"]}` to https://searchwire.dev/v1/domains/check
- Product name: `{"name":"example","tlds":["com","dev","ai"]}` to https://searchwire.dev/v1/names/check

Work in this order: collect candidate keywords, get volume for all of them in one call,
narrow down, then check Google results for the few that matter (mind `observed_at`).

Prices: search volume $0.02 a call plus $0.0002 a keyword; Google results $0.002 collected, $0.005 live
(`"source":"live"`); domain, GitHub and npm checks free. Each key has a daily budget (`POST /v1/account`).

Every response has `cost` (USD) and each item has `source`, `origin`, `fetched_at`, `cached`.
Items that could not be answered carry `error`; do not invent values for them.

No key yet: `POST https://searchwire.dev/v1/billing/checkout` with `{"amount_usd":10}` returns `checkout_url` (ask a person to pay)
and `claim_url` (GET it after payment to receive the key). Save the key where it outlives this session, for
example as `SEARCHWIRE_KEY` in a secret store or an untracked `.env`, and tell the person where it is.
A person can see the key, the balance and the spend at https://searchwire.dev/account by signing in with the email address used at checkout.
