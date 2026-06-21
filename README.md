# plan-2028

A personal trajectory tracker for June 2026 → June 2028: one data file, one
static renderer, and one weekly check-in. A single page that reports its own
progress — and its own neglect.

`plan.json` is the only source of truth: phases, objectives, a 2028 audit list,
and an append-only log. `index.html` renders it client-side for GitHub Pages with
no build step — a timeline with labelled phase bands and a computed "today"
marker, the audit scoreboard, per-phase panels, a prominent staleness gauge (days
since the last log entry, loud past two weeks), and the log in reverse order. A
scheduled GitHub Action opens a Friday check-in issue.

Each phase shows objectives-done beside percent-of-window-elapsed as two plain
numbers — the asymmetry is the point, not something to editorialise. No
gamification, no streaks, no badges.

## Design

Built in the in-house **Tunnel** aesthetic (`tunnel-aesthetic`, from
[`cuddly-lamp`](https://github.com/nihilisticiconoclast/cuddly-lamp)): the locked
chart-paper palette and Fraunces / Public Sans / IBM Plex Mono type, hard edges,
no shadows or gradients, **linked from the CDN rather than inlined**. Status
colours map onto the locked palette and the single red `route` is spent on the
timeline's "today" marker; the fixed house mark sits in the masthead with a
per-page doodle in the margin.

See [`CLAUDE.md`](CLAUDE.md) for the non-negotiable data rules (the log is
append-only, objectives are never deleted, wording stays employer-anonymous) and
the full renderer spec.
