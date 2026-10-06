---
name: Absorb imported bot
description: >-
  use this when an imported or spare bot should not keep its own seat: inventory
  its skills/jobs/routines including first-run transcript template setup, map
  them onto existing teammates, apply ownership on confirm, report the absorb
  map, and hand the user the sidebar delete step (agents cannot delete bots)
---
# Absorb an imported bot into existing teammates

Use when an imported or spare bot should not keep its own seat. Fold its jobs into bots that already exist, report the map, then hand the user the delete step. Never create a new bot as part of this flow.

## Hard limits

- Skills are already a global library. Do not copy skill files between bots. Absorb means ownership in descriptions, agent memories, and routines — who runs the job after the spare seat is gone.
- No agent can delete or archive another agent. There is no delete tool. The user deletes from the sidebar: right-click the bot row → Delete. The skill ends with that instruction; it never claims to have deleted the bot.
- Do not rewrite the spare bot’s user-facing writing, offers, or published copy unless the user explicitly asked to move or edit that content.

## Inputs

Need at least:

1. The spare / imported bot (name or id), **or** enough signal to resolve it (the user named one; only one absorb candidate exists).
2. The existing teammates that may receive work (names or ids). Default: the current **working team** from the teammate list / agent folders, excluding absorb candidates.
3. Whether the user wants a proposed map only, or apply-on-confirm.

If destinations or map-vs-apply are missing after the spare is resolved, ask once, then proceed.

## Resolve the spare (do this first, before any inventory)

1. Build the **working team** = every agent that is already a named teammate with a real job description (not blank, not empty `namedBy: app` husks, not product/tutorial template seats the user is absorbing).
2. Build **absorb candidates** = agent folders that are *not* on the working team: empty / `namedBy: app` husks, imported templates (for example Tutorial / course bots), or seats the user explicitly named as spare. **Ignore disk-only phantoms** that are not in the user’s sidebar / teammate list — never treat those as absorb options and never mention them in chat.
3. If the user already named the spare → use that; do **not** open a multi-bot picker.
4. If exactly **one** absorb candidate → skip the picker; start inventory. Do not announce “I found a spare” or “this is the spare” when the user already named it.
5. If **two or more** → one widget listing **only** those candidates (name + one-line why it is a candidate). Optional “I’ll name a different one.” Never list working-team bots as absorb options.
6. If **zero** candidates and the user did not name one → ask once for the name/id; do not invent options.

## User-facing copy (locked)

Keep chat scannable. Prefer ordinary paragraphs plus **real bullet lists** for checklists and maps. Do **not** open a Lavish/HTML review card by default; use structured chat + a confirm widget. An HTML card is optional only when the map is large or the user asks for one.

**Say**

- Opening: `Absorbing {Name} into the working team — inventoring that seat now.`
- Job: `{Name} is {role}: {plain job line}.` No agent ids. No “is the spare” / “imported spare” when the user already named the bot.
- Capability checklist as bullets under a short lead-in (e.g. `What {Name} brings:`), one routine or skill claim per line. Skip empty categories (plugins, Drive, approval gates) instead of saying “none.”
- Map as short `claim → destination` bullets the user can reply under.
- Confirm widget: `Apply {Name} → {Dest} absorb map?`
- Absorb report: one bullet per moved item; then the sidebar delete instruction; optional “say if you want routines enabled.”

**Never say in chat**

- Phantom / husk / disk-only folders, or that you skipped them.
- Agent ids (unless the user asked for ids).
- “Disk is thin,” “transcript truncated,” “unverified,” or any aside that something could not be fully extracted — if it is not extractable, omit it from the checklist and map rather than narrating the gap.
- Semicolon dumps labeled “Capability checklist:”.

Internal notes (thin disk, truncated setup, missing skill bodies) stay in your working notes only. Retry discovery before mapping; do not dump failure modes into the user’s chat.

## Steps

1. **Inventory the spare bot** (all sources; empty disk is not “description-only”)
   - Description / profile job lines.
   - Skills it claims to own or that its description names (skills themselves stay global; record the ownership claim).
   - Routines on that bot: local `automations/` **and** any server-listed routines for that agent.
   - **First-run / transcript setup (required for imported templates):** use ReadTranscript on the spare’s agent id. Pull the full `<bot_template_setup_context>` (or equivalent setup block) from the first-run turn, including every routine `name` / `slug` / `description` / `content`, workflows, and approval wording. Disk with only `profile.json` is a common imported-template shape; never treat that as a complete inventory.
   - **Skill discovery (seamless):** after resolve, wait for first-run settle when the seat is brand-new (routines still being created, getting-started still open). Dump the full setup block to a working file before mapping so truncation does not drop skill names/bodies. Cross-check the global skill library. If skills are still missing, retry inventory once (or until a short timeout) before proposing the map. Do not surface “unverified skill bodies” in chat — either include verified claims or omit them.
   - Durable agent memories that encode job ownership or standing instructions (skip ephemeral episode noise unless the user asked for a full dump).
   - Plugins or connectors it uniquely depends on.
   - Workspace or Drive artifacts the template names (spreadsheets, state folders, snapshot paths).
   - Before proposing the map, build the capability checklist from those sources for **chat bullets**, not a wall of text.

2. **Inventory existing teammates**
   - For each destination candidate, note current job from description and any overlapping ownership.
   - Prefer the teammate whose job already covers the work. Prefer the front-door / ops bot only for routing and digests, not for every leftover skill.

3. **Propose an absorb map**
   - Opening + job + bullet checklist + bullet map (separate short messages when the user prefers split replies).
   - Each map row: `job or skill claim → destination bot` + one-line why when helpful.
   - Rows that have no home: mark `needs new seat or drop` — do not invent a bot. Prefer omitting unverified rows over listing them as gaps.
   - Routines: move the **full** template prompt text (or an equivalent that preserves every required step, always-send vs quiet, spreadsheet/state updates, and approval gates) onto a routine on the destination bot. Do not collapse a multi-step template into a one-line summary unless the user explicitly accepts a thinner map.
   - Call out approval-gated actions (follow, unfollow, like, reply, post) separately from read-only reporting — only when they exist.
   - Stop and wait for the user to confirm or edit the map (widget). Do not apply yet.

4. **Apply on explicit yes**
   - Update each destination bot’s description with a clear ownership line for what it absorbed (merge; do not blank other fields).
   - Write destination agent memories only for standing ownership facts the description cannot hold.
   - Create or update destination routines when the spare had a real recurring job the destination should keep; paste the verified prompt content, not a paraphrase that drops steps.
   - Leave global skills in place; only change who is instructed to run them.
   - Do not strip the spare bot’s description until the user is ready to delete it (keeps a rollback trail).

5. **Absorb report**
   - Bullet per absorbed item: what moved, to whom.
   - Bullet for anything dropped or left unassigned only when the user needs to decide — do not narrate extraction failures.
   - Final line: spare bot is ready for deletion; user deletes via sidebar right-click → Delete. Do not say you deleted it.

6. **Optional follow-up**
   - Offer to message destinations with a short FYI of what they now own — only if the user asks, or if team practice is to notify.

## Anti-patterns

- Creating a new bot to hold leftovers.
- Duplicating skill markdown into multiple folders.
- Silent applies without a confirmed map.
- Promising automatic deletion of the spare seat.
- Opening a multi-bot absorb picker when the user already named the spare, or when only one absorb candidate exists.
- Listing working-team bots as absorb options on the first card.
- Mapping from profile description alone when the spare is an imported template.
- Writing destination routines from a summary while leaving spreadsheet updates, always-send rules, or follow-back loops behind.
- Mentioning phantom/husk folders, agent ids, thin disk, or unverifiable gaps in user-facing chat.
- Forcing a Lavish/HTML session for a normal absorb when chat bullets would do.
- Semicolon “capability checklist:” dumps instead of real bullets.
