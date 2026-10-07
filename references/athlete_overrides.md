# Athlete Overrides (fork-specific)

Single source of per-athlete deviations from the upstream skill. When this file conflicts with a workflow or reference, **this file wins**.

## Athlete
- intervals.icu id `i428455`, male, 38, ~88 kg, FTP 266 W (re-check against profile each session), LTHR 164, max HR 191.
- Timezone Europe/Moscow, metric.
- Response language: Russian. Keep cycling acronyms (FTP, TSS, IF, CTL/ATL/TSB, RPE) as-is.

## Goal and schedule
- Goal: raise FTP (no race date yet). Default block: FTP Builder (see `references/block_templates.md`); schedule an FTP test per Rule 4 when the last one is >8 weeks old.
- 3 rides/week: 2 indoor structured sessions + 1 outdoor ride (outdoor = endurance/tempo by feel, not ERG).
- Hard cap **90 min per session**. Fit progression via intensity density and interval quality, not duration. Weekly TSS targets must be sized for ≤ 3 × 90 min.
- With only 3 days, put the two key sessions (threshold / sweet spot / VO2) non-adjacent, outdoor ride as the long/endurance slot.

## No wellness data
- No wearable. **Skip** Readiness blocks, `--readiness-check`, `--wellness`, HRV/RHR/sleep gating, and menstrual-cycle references.
- Substitute: a one-line subjective check from the athlete before key sessions (legs, sleep, stress: ok / so-so / bad), plus TSB, ACWR and RPE-vs-IF trend from `workflows/plan.md` / `weekly_adaptation.md`.
- Collect RPE after every ride (needed for the RPE:IF mismatch rules, since there is no HRV safety net).
- Say explicitly in weekly reviews that readiness coaching is limited to PMC + RPE.

## Data access: MCP-first
Prefer the `intervals-icu` MCP tools over `scripts/intervals_icu_api.py`:
- Activity / history: `get_activity`, `list_activities`, `get_fitness_summary`, `get_athlete_curves`, `get_activity_streams`.
- Calendar: `list_events`, `create_events`, `update_event`.
- Python is still used for `pmc_calculator.py`, `generate_zwo.py`, `zwo_lint.py` (these need `.env` credentials only if they call the API).

## Publishing workouts (replaces Zwift-folder writes)
No Zwift folder exists on this machine. Instead of `batch_generate_zwo.py --output-dir <ZWIFT_WORKOUTS_DIR>`:
1. Design workouts as usual and lint any generated `.zwo` (`zwo_lint.py`).
2. Present the week as a table (date, name, duration, target TSS/IF, structure) and **wait for explicit approval**.
3. Create calendar events via the `create_events` MCP tool (or `create_workouts` for the library) with the structured workout text so power targets are % FTP. Check the tool schema before the first call.
4. Report the dates written. Never overwrite or delete existing events without confirmation.
The `.zwo` files may still be saved to a local folder of the athlete's choice if they ask for Zwift/other importers.

## Routine
- After each ride: "разбери последний заезд" → Activity Analysis + adaptation cascade.
- Sunday evening: Weekly Review (plan vs actual, PMC, next-week proposal). Proposal only — the athlete approves before any calendar change.
- Every 4–6 weeks: new block / FTP test.
