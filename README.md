# SP Navigators Chat (@spnavchat_bot)

A 24/7 Telegram bot that helps SP Navigators members find Bible verses.

- 📖 Topic/doctrine questions → real verse text (WEB translation, looked up from bible-api.com, never written by the AI)
- 🙋 Personal/life questions → gentle reply + alert to the leaders' group
- 🆘 Crisis words → fixed helpline message + urgent alert (checked before the AI)
- Never speaks for God or gives personal advice

Runs on a Cloudflare Worker with Workers AI (`@cf/zai-org/glm-4.7-flash`).

## Setup

1. `npm install`
2. Set the secrets:
   - `npx wrangler secret put BOT_TOKEN` — token from @BotFather
   - `npx wrangler secret put WEBHOOK_SECRET` — any long random string (letters, numbers, `_` `-`)
   - `npx wrangler secret put LEADER_CHAT_ID` — the leaders' group chat id (starts with `-100…`)
3. `npx wrangler deploy` and note the `*.workers.dev` URL
4. Point Telegram at it (run once):
   `https://api.telegram.org/bot<BOT_TOKEN>/setWebhook?url=<WORKER_URL>&secret_token=<WEBHOOK_SECRET>`
