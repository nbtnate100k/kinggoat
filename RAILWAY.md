# Railway deploy

This app reads secrets from **environment variables** at runtime. A `.env` on your PC is **not** included in the Docker build (`.env` must stay gitignored).

## Required

1. In **Railway** → your **service** → **Variables**, add:

   | Name | Value |
   |------|--------|
   | `TELEGRAM_BOT_TOKEN` | Full token from [@BotFather](https://t.me/BotFather) |

   Name must match **exactly** (no stray spaces before/after).

2. **Replicas:** Use **one** replica. Telegram polling allows only one `getUpdates` client per bot token (`railway.toml` notes this).

3. Redeploy after saving variables.

## Optional

Mirror anything you use locally from `.env.example`, for example:

- `ADMIN_USER_IDS`
- `BTC_WALLET`, `LTC_WALLET`

## Troubleshooting

- Log still says token is missing → variable is absent, misspelled, or not on **this** service (check you edited the worker that runs `python bot.py`, not another service).
- `409 Conflict` in logs → two processes share the same token (second Railway service, extra replica, or bot running locally at the same time).
