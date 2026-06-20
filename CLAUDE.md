# plan-2028

Personal trajectory tracker, June 2026 → June 2028. One data file, one static
renderer, one weekly nag. This repo tracks the plan; it must never become the
plan's competitor for attention.

## Files

- `plan.json` — single source of truth. Phases, objectives, 2028 audit items, log.
- `index.html` — static renderer for GitHub Pages. Reads `plan.json` at load. No build step.
- `.github/workflows/weekly-checkin.yml` — Friday check-in issue (cron, UTC).

## Data rules (non-negotiable)

1. `plan.json` is the only state. No databases, no localStorage, no external services.
2. `log` is append-only. Never edit or delete past entries. New entries go at the end: `{"date": "YYYY-MM-DD", "note": "..."}`.
3. Objectives are never deleted. If reality diverges, set `status` to `"dropped"` and add a `note` explaining why, plus a log entry. Rescoping = edit `title`, keep `id`, note the change in the log.
4. `status` enum: `not_started` | `in_progress` | `done` | `dropped`. Nothing else.
5. `id` values are stable forever. Dates are ISO `YYYY-MM-DD`.
6. Keep wording employer-anonymous. No company names, colleagues, or figures.

## Common operations

- "Log: X happened" → append to `log` with today's date.
- "Mark p1-o3 done" → set status, append a log entry recording it.
- "Rescope p1-o1 to ..." → edit title/note, append log entry with old → new.
- Always show the diff of `plan.json` before committing.

## Renderer spec (v0.1)

Single `index.html`, vanilla JS or CDN libraries only. Fetches `plan.json`
(same origin). Must render:

1. **Timeline** — horizontal, 2026-06-01 to 2028-06-30, phase bands labelled,
   a vertical today-line computed from the current date. Objective markers
   placed at their due dates, coloured by status.
2. **Audit scoreboard** — the five `audit_2028` items with status, top of page.
3. **Phase panels** — each phase's objectives with title, due date, status,
   note. Also show: objectives done vs % of phase window elapsed (two plain
   numbers side by side — this asymmetry is the point, do not editorialise it).
4. **Staleness gauge** — days since the last `log` entry, prominent. If > 14,
   make it visually loud. The tracker reports its own neglect.
5. **Log** — reverse-chronological, plain.

Design constraints: no gamification, no streaks, no badges, no confetti. Honest
and legible beats pretty. Must be readable on a phone. Uses the in-house "Tunnel"
aesthetic — its locked layer (palette, type, hard edges, the signature figures)
is linked from cuddly-lamp's `tokens.css`/`tunnel-figure.js` on the CDN, never
inlined; only the page-specific layout lives in `index.html`. The renderer keys
status colour off CSS variables (`--not_started` etc.) mapped onto the Tunnel
palette, and spends the single red `--route` on the timeline's "today" marker.
See `.claude/skills/tunnel-aesthetic/SKILL.md`.

## Build budget (instruction to Claude Code)

v0.1 ships in at most two sessions. If asked to add features before all five
renderer items above work end to end, push back and say so. Polish is session
two at most. After v0.1, changes to this repo should be measured in minutes,
not evenings — if a request would take longer, question whether it serves
tracking or has become procrastination.
