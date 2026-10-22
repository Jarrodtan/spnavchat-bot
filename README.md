# SP Navigators Chat (@spnavchat_bot)

A 24/7 Telegram bot that helps SP Navigators members find Bible verses.

> 🚧 **Status:** In development. Phase 1 (verse lookup, leader escalation, crisis safety) and members-only approval are done, and improvements are ongoing.

- 🙋 Personal/life questions → gentle reply + alert to the leaders' group
- 🆘 Crisis words → fixed helpline message + urgent alert (checked before the AI)
- 🔑 Members only: new people get an Approve / Decline request in the leaders' group (crisis help still goes to everyone)
- Never speaks for God or gives personal advice

Runs on a Cloudflare Worker with Workers AI (`@cf/meta/llama-3.1-8b-instruct-fast`).

## How it works

- **Serverless webhook:** Telegram pushes each update to a Cloudflare Worker, which is verified with a secret header, so there's no server to keep running.
- **Safety first:** crisis keywords are checked *before* any AI call, so a fixed helpline reply is guaranteed and never depends on the model.
- **Human in the loop:** personal questions and new member requests go to a leaders' group with inline Approve / Decline buttons.
- **Member storage:** approved, pending and declined users are stored in Cloudflare KV.
- **Secrets** (bot token, webhook secret, chat IDs) live in Worker secrets and are never committed to the repo.

**Tech:** JavaScript · Cloudflare Workers · Workers AI (Llama 3.1) · Cloudflare KV · Telegram Bot API

I designed and built the bot end-to-end for SP Navigators.

## Setup

1. `npm install`
2. Set the secrets:
   - `npx wrangler secret put BOT_TOKEN` — token from @BotFather
   - `npx wrangler secret put WEBHOOK_SECRET` — any long random string (letters, numbers, `_` `-`)
   - `npx wrangler secret put LEADER_CHAT_ID` — the leaders' group chat id (starts with `-100…`)
3. `npx wrangler deploy` and note the `*.workers.dev` URL
4. Point Telegram at it (run once):
   `https://api.telegram.org/bot<BOT_TOKEN>/setWebhook?url=<WORKER_URL>&secret_token=<WEBHOOK_SECRET>`
