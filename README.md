# Searchwire skill

Gives a coding agent (Claude Code, Codex, Cursor and others) real SEO numbers instead of guesses:

- **Keyword search volume**, ad price per click and competition, for up to 1,000 keywords in one call
- **Google results** for a keyword: who ranks, with URL and title
- **Name checks**: whether a product name is free as a domain, on GitHub and on npm

It is one `SKILL.md` that teaches the agent to call the [Searchwire](https://searchwire.dev) HTTP API. There is nothing to run and no MCP server.

## Install

With the [skills CLI](https://skills.sh):

```
npx skills add LambdaLabsHQ/searchwire-skill
```

Or by hand, for Claude Code:

```
mkdir -p .claude/skills/searchwire
curl -s https://searchwire.dev/skill.md -o .claude/skills/searchwire/SKILL.md
```

Other agents can read the full API docs at <https://searchwire.dev/llms.txt>.

## Try it

Domain and name checks work without a key:

> Is "pagenta" free as a .com, .dev and .ai domain, on GitHub and on npm?

Search volume and Google results need a key:

> List 200 keywords our docs could rank for, get their US search volume from Searchwire in one call, keep the 20 best, check who ranks for the top 5, and write a content plan.

That run costs about $0.07.

## What an answer looks like

Asked on 10 October 2026, US:

| Keyword | Searches a month | Ad price a click |
|---|---|---|
| seo agent | 27,100 | $39.88 |
| serp api | 12,100 | $8.99 |
| keyword research tool | 4,400 | $15.75 |
| seo api | 390 | $24.38 |

## Key and price

A key comes with prepaid credit, from $10. There is no sign-up and no subscription, credit does not expire, and unused credit is refunded on request within 30 days. Buy one at <https://searchwire.dev>, or let the agent start the purchase: it gets a payment link for you and picks up the key once you have paid.

| Call | Price |
|---|---|
| Search volume | $0.02 a call, plus $0.0002 a keyword |
| Google results, collected earlier | $0.002 |
| Google results, fetched live | $0.005 |
| Domain, GitHub and npm checks | free |

Every key has a daily spending cap ($5 unless you change it), so an agent in a loop cannot run up the bill, and every response says what it cost.

## Licence

MIT for this repository. Use of the API is under the [Searchwire terms](https://searchwire.dev/terms).
