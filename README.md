# Emerald Bot v27.2 RC (Release Candidate 1)

**Codename: GOLDEN GATE** | Premium local build for Discord communities

## Overview

Emerald is a local-first Discord bot built around premium command design, community progression, moderation tooling, and a polished 2026-style command experience. The project keeps its deployment local while delivering strong UX across slash commands, profile flows, and welcome handling.

## Current Build

This build is tuned for a premium, modern feel without changing the core local-only architecture. The bot supports:
- local JSON storage by default
- optional PostgreSQL support when `DATABASE_URL` is configured
- server-only XP progression with local tracking
- premiumized embeds, button-driven help, and clean command grouping
- welcome catch-up after restarts and offline announcements when configured

## Core Systems

### Progression
- server-only level progression, with 2x XP on Wednesdays, Fridays, and occasional bonus weeks
- premium and VIP status messaging

### Community and server tools
- moderation workflows, notes, warnings, role management, and channel actions
- profile and statistics views with premium card styling
- local welcome and reminder systems
- level progression, badges, and rank tracking

### Diagnostics and runtime
- `/quantumos` diagnostics and integrity checks
- runtime health reporting and uptime metadata
- theme-aware embed styling, polished command UX, and startup banner output

### Discord Store subscriptions
- Personal Store entitlements can be checked with `/premium`.
- Set `DISCORD_SKU_STARTER`, `DISCORD_SKU_SUPPORTER`, `DISCORD_SKU_PREMIUM`, and `DISCORD_SKU_ULTIMATE` to the matching product SKU IDs from the Discord Developer Portal.
- Active Premium and Ultimate subscribers receive the existing VIP message XP multiplier; higher active tiers take precedence if multiple entitlements exist.
- Store SKU IDs are product identifiers, not bot tokens. Keep `DISCORD_TOKEN` private as usual.

## Technology Stack
- **Runtime:** Node.js >= 22.12.0
- **Library:** discord.js 14.27.0
- **Environment:** dotenv 18.0.4
- **Status:** Local premium build

## Deploying and running locally

1. Install dependencies with `npm install`.
2. Configure `.env` values for your local bot token and any optional settings.
   To enable Store checks, configure the four `DISCORD_SKU_*` values above.
3. Run `npm run deploy` to register slash commands.
4. Start the bot with `node index.js` or your local run script.

Use `COMMAND_SCOPE=guild` with `GUILD_ID` only for a quick single-server test. For broad installation, prefer global registration.

## Server setup

Use `/setup` for one private dashboard with server rules, Welcome, Logs, AutoMod, Join security, staff applications, member reports, Premium feature guidance, Ultimate scheduled announcements, economy settings, and a refreshable health check. Choose a section from the dashboard menu. Edit or restore default rules from the Rules page. The health check verifies configured channel references, the bot's channel permissions, enabled join-security permissions, and whether the saved application panel still exists. Configure channels and messages, choose which log categories to record, post or close the application panel, customize Premium application questions, test configured channels, and return to the dashboard from each page. Editing rules, logs, AutoMod, Join security, applications, reports, and the health check require Administrator; Welcome settings require Manage Server. Personal and one-off moderation commands remain available through their own slash commands.

The Welcome page lets admins control channel posts and direct welcome DMs separately; DMs are enabled by default. Premium/Ultimate custom message editing is available only to the actual server owner. Use `{mention}`, `{user}`, `{server}`, and `{memberCount}` for dynamic text. The Logs page provides separate controls for member, message, channel/thread, role, moderation, voice/stage, and other server-change events; all categories remain enabled unless an admin turns them off. The Announcements page lets an Ultimate server owner schedule up to 10 plain-text posts per server, one-time, daily, or weekly, with a delay of up to seven days. Announcements do not parse mentions; schedules automatically pause if the server owner loses Ultimate. Owners can still pause or delete schedules. The AutoMod page includes advanced rule controls and word filters, which also require Premium or Ultimate and the actual server-owner account. Premium owners can also review application/report queues. AutoMod remains disabled by default.

The bot also attempts to send a direct welcome message, and it checks for missed joins after a restart so new members are not lost during local downtime. Ultimate users receive 10-minute and one-hour snooze buttons on reminder DMs.

## Auto-moderation and monitoring

AutoMod is disabled by default and configured separately in each server. Use its page in `/setup` for basic and advanced controls. Advanced rules and word filters require Emerald Premium or Ultimate and the actual server-owner account; Discord permissions are still required. Enforced message actions can warn, timeout, kick, or ban; alert-only is the default, so review rules carefully before selecting destructive actions. Enable the Moderation log category to receive AutoMod action logs. Image-content checks require the optional `IMAGE_MODERATION_API` and `IMAGE_MODERATION_API_KEY` service settings; without them, the per-server image rule can still flag large image attachments.

Join security is configured separately in each server from `/setup` → Join security and is off until an Administrator enables it. Bot protection can kick or ban bots not on the trusted-bot ID list; add legitimate bot integrations to that list before enabling protection. Raid detection defaults to the Medium / Active profile (9 joins in 10 seconds), quarantines the burst with Discord timeouts for 10 minutes, and sends alerts to the configured server log channel. Small / Private (4 joins / 10 seconds), Large / Creator (18 joins / 10 seconds), and custom thresholds are available. The Large profile temporarily raises Discord verification to Very High and restores the previous level after the protection window; it requires Manage Server. New-account age is a configurable 2-5 day signal and defaults to logging only, not removal. A young account is not proof of abuse, and Discord verification is not a CAPTCHA. Kick, ban, timeout, and verification responses require the corresponding bot permissions and a suitable role hierarchy; `/setup` → Health check reports missing permissions.

## Personal subscription commands

Subscription benefits are checked against the invoking user's active Discord entitlement and follow that user across servers. Free users keep the core commands and basic moderation. Supporter adds an Emerald Supporter badge shown by `/profile` and `/badges`, plus a second title showcase slot. Premium adds a third title slot, 2x message XP, up to three recurring reminders, customizable staff-application questions, and owner-only application/report queues. `/remind` accepts either a delay in minutes or a local clock time with an IANA time zone; recurring local-time reminders keep their wall-clock time across daylight-saving changes. Ultimate raises the recurring-reminder limit to 10, adds 10-minute and 1-hour snooze controls, and lets server owners schedule announcements from `/setup`. Server-wide Premium/Ultimate features still require the actual server owner and the relevant Discord permissions. A subscription never grants Discord roles or permissions.

## Commands

The Glimmer economy uses one shared wallet and bank across servers and DMs. `/work` earns 25-100 Glimmers once per server cooldown, and `/daily` earns 250-500 Glimmers once per server cooldown. `/balance` privately shows wallet, savings, and total wealth; `/pay` sends Glimmers from the wallet; and `/leaderboard` ranks total wallet-plus-bank wealth. Work and daily cooldowns are configured per server in `/setup` → Economy by that server's owner or an Administrator. Claims in DMs have no cooldown and receive no subscriber bonus. Emerald Store subscribers receive server-only reward bonuses: Starter 5%, Supporter 10%, Premium 15%, and Ultimate 20%.

Use `/bank` to deposit or withdraw Glimmers and set a savings goal. Banked Glimmers earn no interest and must be withdrawn before spending or sending. Your regular `/profile` card shows savings-goal progress without exposing the goal amount or bank balance. `/shop` sells 16 cosmetic economy titles, with exclusive titles for Supporter, Premium, and Ultimate subscribers; purchases do not affect rewards or balances. `/achievements` tracks seven lifetime economy milestones and unlocks profile badges. `/transactions` privately shows up to 10 recent rewards, payments, bank transfers, and purchases. Successful earning and payment activity is visible in server channels; DM replies remain private. Legacy economy balances and shop data were reset. Browse all active commands with `/help`.
Use `/bot info` for live server count, combined member reach, command count, runtime, language, and uptime. `/bot ping` checks latency.

Key command families include:
- Moderation: `/moderation ban`, `/moderation warn`, `/moderation report`, `/reports`, `/setup`
- Server management: `/roles`, `/channels`, `/rules`, `/quantumos`
- Economy: `/work`, `/daily`, `/balance`, `/bank`, `/pay`, `/leaderboard`, `/shop`, `/achievements`, `/transactions`
- Progression: `/member profile`, `/member titles`, `/badges`, `/leaderboardlevel`
- Utility: `/help`, `/bot info`, `/bot ping`, `/premium`, `/remind`, `/remind-cancel`
- Profile features: `/profile`, `/member title`

Premium server owners can review application and report queues from their `/setup` pages or the `/reports` command. Staff can take, resolve, or dismiss reports in the configured review channel; these controls still require Administrator permission.

## Final note

This bot is intentionally kept local-first, premium-looking, and stable for a personal or community environment. The broader command suite stays available while the UI and command grouping keep the experience feeling clean and modern.

**Emerald is powered locally and built to feel premium.**
