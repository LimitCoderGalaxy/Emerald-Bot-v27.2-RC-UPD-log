# Emerald Bot v27.2

**Codename: AURORA** | Premium local build for Discord communities

## Overview

Emerald is a local-first Discord bot built around premium command design, economy progression, moderation tooling, and a polished 2026-style command experience. The project keeps its deployment local while delivering strong UX across slash commands, profile flows, leaderboards, rewards, and welcome handling.

## Current Build

This build is tuned for a premium, modern feel without changing the core local-only architecture. The bot supports:
- local JSON storage by default
- optional PostgreSQL support when `DATABASE_URL` is configured
- economy commands in DMs and group DMs when user installs are enabled
- server-only XP progression with local tracking
- premiumized embeds, button-driven help, and clean command grouping
- welcome catch-up after restarts and offline announcements when configured

## Core Systems

### Economy and progression
- wallet, bank, inventory, shop, rewards, and trading flows
- daily, weekly, work, beg, and reward loops
- level progression and XP tracking with guild-safe checks
- premium and VIP status messaging

### Community and server tools
- moderation workflows, notes, warnings, role management, and channel actions
- profile and statistics views with premium card styling
- local welcome and reminder systems
- activity monitoring, badges, and rank tracking

### Diagnostics and runtime
- `/quantumcore` diagnostics and integrity checks
- runtime health reporting and uptime metadata
- theme-aware embed styling, polished command UX, and startup banner output

## Technology Stack
- **Runtime:** Node.js >= 22.12.0
- **Library:** discord.js 14.27.0
- **Environment:** dotenv 18.0.4
- **Status:** Local premium build

## Deploying and running locally

1. Install dependencies with `npm install`.
2. Configure `.env` values for your local bot token and any optional settings.
3. Run `npm run deploy` to register slash commands.
4. Start the bot with `node index.js` or your local run script.

Use `COMMAND_SCOPE=guild` with `GUILD_ID` only for a quick single-server test. For broad installation, prefer global registration.

## Economy in DMs

User-install support allows economy commands to work in bot DMs and group DMs. Supported flows share a DM wallet separate from each guild wallet. This keeps the bot usable in private contexts while preserving local guild-scope rules for server-based progression.

## Welcome flow

Use `/welcome set` to choose a channel for join announcements in a server, or `/welcome test` to preview the welcome card. The bot also attempts to send a direct message, and it checks for missed joins after a restart so new members are not lost during local downtime.

## Auto-moderation and monitoring

The local moderation engine includes risk scoring for spam bursts, repeated content, links, mentions, attachments, and account age context. It can warn, delete, timeout, kick, or ban depending on the configured actions.

## Commands

The bot includes a premium command suite with grouped flows and category-based discovery through `/help`.

Key command families include:
- Economy: `/economy`, `/balance`, `/daily`, `/weekly`, `/work`, `/shop`, `/inventory`, `/leaderboard`, `/trade`
- Moderation: `/moderation`, `/warn`, `/ban`, `/kick`, `/timeout`, `/report`
- Server management: `/roles`, `/channels`, `/rules`, `/welcome`
- Progression: `/level`, `/profile`, `/titles`, `/badges`, `/leaderboardlevel`
- Games: `/coinflip`, `/dice`, `/rps`, `/fun`
- Utility: `/help`, `/ping`, `/premium`, `/quantumcore`, `/updates`, `/remind`

## Final note

This bot is intentionally kept local-first, premium-looking, and stable for a personal or community environment. The broader command suite stays available while the UI and command grouping keep the experience feeling clean and modern.

**Emerald is powered locally and built to feel premium.**
