---
name: searchwire
description: Look up SEO and ASO data for a task - keyword search volume, Google results for a keyword, the keywords an app ranks for in the App Store or Google Play with their search volume, who ranks for a keyword in the stores, an app's competitors, whether domains are registered, and whether a product name is free (domains, GitHub, npm). Use when choosing keywords, sizing demand, checking competitors in Google or the app stores, doing app store optimization, or naming a product.
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

Apps (ASO). `store` is `"apple"` (default) or `"google"`; `app_id` is the number in an App Store link
or the Google Play package name. Keyword data covers the United States in English.

- An app's keywords, with search volume and its rank: `{"app_id":"964220645","limit":100}` to https://searchwire.dev/v1/apps/keywords
  (`"max_rank":10` keeps the keywords where it ranks in the top 10; `"order":"rank"` lists best ranks first)
- Who ranks for a keyword: `{"keyword":"pixelate image","depth":10}` to https://searchwire.dev/v1/apps/search
  (apps in order with rating and number of ratings; any country; can take 5 to 20 seconds)
- An app's competitors: `{"app_id":"964220645","limit":20}` to https://searchwire.dev/v1/apps/competitors
- Store search volume for up to 25 keywords you name: `{"keywords":["pixelate image","blur photo"]}` to https://searchwire.dev/v1/apps/keywords/volume
  (a first lookup can take a minute)

For an app, start from apps: search one or two seed keywords, read the keywords of the apps that rank,
and ask for single keywords' volume only for what is still missing. Store volumes are estimates
(the stores publish none) and far lower than Google's for the same keyword; compare both.

Work in this order: collect candidate keywords, get volume for all of them in one call,
narrow down, then check Google results for the few that matter (mind `observed_at`).

Prices: search volume $0.02 a call plus $0.0002 a keyword; Google results $0.002 collected, $0.005 live
(`"source":"live"`); an app's keywords $0.02 a call plus $0.0002 a keyword; an app's competitors $0.02 a call plus $0.002 a
competitor; store search results $0.005 a keyword; store search volume $0.02 a keyword; domain, GitHub and npm checks free. Each key has a daily budget (`POST /v1/account`).

Every response has `cost` (USD) and each item has `source`, `origin`, `fetched_at`, `cached`.
Items that could not be answered carry `error`; do not invent values for them.

No key yet: `POST https://searchwire.dev/v1/billing/checkout` with `{"amount_usd":10}` returns `checkout_url` (ask a person to pay)
and `claim_url` (GET it after payment to receive the key). Save the key where it outlives this session, for
example as `SEARCHWIRE_KEY` in a secret store or an untracked `.env`, and tell the person where it is.
A person can see the key, the balance and the spend at https://searchwire.dev/account by signing in with the email address used at checkout.
