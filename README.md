# Emerald Bot v26.52

**Codename: PRISM** | Quantum Core 2.8.0

## Overview

Quantum Core 2.8 provides Emerald's runtime diagnostics and integrity checks. The `/quantumcore` command reports bot health, runtime details, cooldown activity, environment overrides, and Quantum Intelligence's live session statistics.

## Quantum Intelligence 2.5.7

Quantum Intelligence 2.5.7 is Emerald's local, explainable moderation scorer. It combines recent message bursts, repeated content, link/mention/attachment volume, and account age into a 0-100 risk score. Account age is only one signal and does not trigger action by itself. High-confidence repeated spam, message floods, or combined risk use the existing AutoMod alert/delete/action settings. Session counters in `/quantumcore` reset when the bot restarts. Quantum Intelligence and Quantum Core versions are configured independently with `QUANTUM_INTELLIGENCE_VERSION` and `QUANTUM_CORE_VERSION`.

## Core Features

### Core Identity & Monitoring
- ✓ Core identity and versioning
- ✓ Live health diagnostics (latency, memory, uptime)
- ✓ Runtime monitoring and analytics

### Diagnostics & Integrity
- ✓ Gateway/API latency and uptime reporting
- ✓ Runtime details for Node.js and discord.js
- ✓ Process and heap memory reporting
- ✓ Cooldown activity snapshots
- ✓ Configuration and environment integrity warnings
- ✓ Explainable risk scoring for repeated messages, rapid bursts, and combined signals

### Economy & Engagement
- ✓ Full economy system with currency management
- ✓ User levels and experience system
- ✓ Badge and achievement system
- ✓ Role management and permissions
- ✓ Moderation tools (warn, ban, timeout)
- ✓ Auto-moderation for spam, bot accounts, and configured image checks

## Technology Stack
- **Runtime:** Node.js >= 22.12.0
- **Library:** discord.js 14.27.0
- **Environment:** dotenv 18.0.4
- **Codename:** PRISM
- **Status:** Stable


## Auto-Moderation Configuration

Use the `.env` file to tune Emerald Auto-Mod:
- `AUTO_MOD_ENABLED` — enable or disable automod
- `AUTO_MOD_ACCOUNT_ACTION` — action for detected account/message risk (`timeout`, `kick`, `ban`)
- `AUTO_MOD_IMAGE_ENABLED` — enable image scanning on attachments
- `AUTO_MOD_IMAGE_ACTION` — action for unsafe image detection (`timeout`, `kick`, `ban`)
- `AUTO_MOD_DELETE_MESSAGE` — remove flagged messages automatically
- `AUTO_MOD_BOT_ACCOUNT_ACTION` — action for detected bot accounts (`kick`, `ban`, `none`)
- `AUTO_MOD_NEW_ACCOUNT_DAYS` — account age context in risk scoring; age alone does not trigger moderation
- `AUTO_MOD_SUSPICIOUS_IMAGE_SIZE_KB` — image size threshold for suspicious attachment checks
- `AUTO_MOD_ALERT_ONLY` — report only without taking action
- `AUTO_MOD_TIMEOUT_DURATION_MINUTES` — timeout length for `timeout` actions
- `AUTO_MOD_WHITELIST_ROLE_IDS` — comma-separated role IDs excluded from automod
- `AUTO_MOD_WHITELIST_CHANNEL_IDS` — comma-separated channel IDs excluded from automod
- `AUTO_MOD_MAX_MENTIONS` — maximum allowed mentions per message
- `AUTO_MOD_MAX_LINKS` — maximum allowed links per message
- `AUTO_MOD_MAX_ATTACHMENTS` — maximum allowed attachments per message
- `AUTO_MOD_SPAM_WINDOW_SECONDS` — time window used to detect message bursts
- `AUTO_MOD_MAX_MESSAGES_PER_WINDOW` — message count allowed per user within that window
- `AUTO_MOD_DUPLICATE_MESSAGE_THRESHOLD` — repeated matching messages that trigger spam detection
- `QUANTUM_INTELLIGENCE_RISK_THRESHOLD` — combined risk score required to trigger moderation (1-100; default 70)
- `AUTO_MOD_FORBIDDEN_WORDS` — comma-separated forbidden words/phrases
- `AUTO_MOD_AUTO_WARN` — send a warning DM when a rule is triggered
- `AUTO_MOD_LOG_CHANNEL_ID` — optional channel ID for logging automod actions

## Deploying to Servers

Invite the application with the `bot` and `applications.commands` OAuth scopes. Run `npm run deploy` to register slash commands globally; global commands become available in every server where the bot is installed after Discord finishes propagating them. Set `COMMAND_SCOPE=guild` with `GUILD_ID` only when testing commands in one development server.

### Economy in DMs

To use economy commands in bot DMs or group DMs, enable **User Install** in the Discord Developer Portal under **Installation**, keep **Guild Install** enabled, and deploy commands globally with `COMMAND_SCOPE=global`. Supported DM actions use one shared DM wallet, separate from each server's wallet. Balance, inventory, and profile responses are private in DMs; shop, rewards, trades, and leaderboards use the shared DM economy.

### Economy Reliability

Economy updates are serialized per storage scope within the single bot process. Payments, full-balance transfers, and accepted trades update affected accounts together, while concurrent robbery attempts cannot take more than the target's available balance. This does not make multi-process or multi-worker operation safe; run one bot instance.

### Welcome DMs

The bot sends welcome DMs to new human members and checks for missed joins after restarting. It saves a per-server checkpoint; the first startup with this feature sets a baseline and does not message existing members. Enable **Server Members Intent** under **Bot → Privileged Gateway Intents**. If a member's DMs are closed, `WELCOME_CHANNEL_ID` can be set as a fallback channel.

### Runtime Reliability

On `SIGINT` or `SIGTERM`, the bot clears its recurring and reminder timers, disconnects from Discord, and closes the configured storage pool. Reminder scheduling also handles dates beyond Node.js's single-timer limit by scheduling in safe intervals.

## PostgreSQL Storage

Set `DATABASE_URL` in `.env` to enable PostgreSQL storage. For the local database and role created during setup, the connection format is `postgresql://emerald_bot:URL_ENCODED_PASSWORD@localhost:5432/Emerald_Bot`. URL-encode reserved characters in the password and never commit `.env`. The `emerald_bot` role must own the database or have permission to create tables in its schema; the bot creates its storage table automatically. `PG_POOL_MAX` optionally sets the connection pool size (default 10).

With `DATABASE_URL` unset, the bot uses per-guild JSON files. When PostgreSQL is enabled, the bot imports existing JSON data into that guild's database scope on first access; legacy root data is assigned to the configured `GUILD_ID`, while other guilds start separately. Reminders remain global.

Global command registration makes the bot installable across servers. PostgreSQL and per-guild scopes provide a better starting point for hundreds of servers, but this setup has not been load-tested at that scale and still runs as a single process. Keep one bot instance until storage and deployment are designed for concurrent workers.

## Commands

The bot provides **40 top-level slash commands**, with related actions grouped into subcommands:
- Economy and rewards (`/rewards`, `/balance`, `/pay`, `/shop`)
- Moderation and staff notes (`/moderation`)
- Server management (`/roles`, `/channels`, `/rules`)
- Games and trading (`/roulette`, `/rps`, `/coinflip`, `/trade`)
- Member profiles and progression (`/member`, `/badges`, `/titles`)
- Utility and diagnostics (`/ping`, `/help`, `/updates`, `/quantumcore`)

## Update Command

The `/updates` command fetches the latest release log from GitHub and highlights the current v26.52 release. It includes:
- Changelog preview or recent commits when logs are unavailable
- Static release summary for the current 26.52 update
- Current bot version and Quantum Core status
- Stable release indicator


**It's The Newly Powered Engine**

Thank You All For Your Support!
