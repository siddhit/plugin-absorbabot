# Absorb a Bot

Cursor plugin that ships the **Absorb imported bot** skill. Use it when an imported or spare Grok Bot should not keep its own seat.

## What it does

The skill folds that spare seat into teammates that already exist:

1. Inventory the spare bot’s skills, jobs, and routines, including first-run transcript template setup.
2. Map each claim onto an existing teammate.
3. Apply ownership only after you confirm the map.
4. Report what moved, what was left unassigned, and anything that could not be verified.

It does not create a new bot, and it does not copy skill files between bots. Skills stay a global library; absorb means who is instructed to run the job after the spare seat is gone.

**Agents cannot delete bots.** There is no delete tool. When the spare seat is ready, you remove it yourself: in the sidebar, right-click the bot row → **Delete**.

## Install

This repository is one plugin at the root (manifest: `.cursor-plugin/plugin.json`). Submit the repo to the Cursor Marketplace, then install **Absorb a Bot**.

## Use

Ask the agent to absorb an imported or spare bot into the current team. Name the spare seat, or let the skill take the only absorb candidate. It proposes a map and waits. After you confirm, it updates destination descriptions, memories, and routines, then tells you the spare bot is ready for the sidebar delete step.

## Layout

- `.cursor-plugin/plugin.json` — plugin manifest
- `.cursor-plugin/marketplace.json` — single marketplace entry (the template validator requires this file)
- `skills/absorb-imported-bot/SKILL.md` — Absorb imported bot skill
- `assets/logo.svg` — plugin logo

## Validate

```bash
node scripts/validate-template.mjs
```
