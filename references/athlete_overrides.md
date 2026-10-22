# Athlete Overrides (fork-specific)

Single source of per-athlete deviations from the upstream skill. When this file conflicts with a workflow or reference, **this file wins**.

## Athlete
- intervals.icu id `i428455`, male, 38, ~88 kg, FTP 266 W (re-check against profile each session), LTHR 164, max HR 191.
- Timezone Europe/Moscow, metric.
- Response language: Russian. Keep cycling acronyms (FTP, TSS, IF, CTL/ATL/TSB, RPE) as-is.

## Memory: exocortex `cycling/` (source of context between chats)
Path: `C:\Hit\exo_cortex\cycling\` (git repo `exo_cortex`, full protocol in `cycling/README.md`). intervals.icu stays the source of truth for numbers; the exocortex holds what intervals does not remember.
- **Session start:** read `NOW.md` → `insights.md` → current file in `blocks/` → latest file in `log/`. If intervals has activities newer than the last `log/` entry, analyse those first. Do not re-ask the athlete for facts already in these files.
- **After a ride analysis:** append 3–5 lines to `log/<YYYY-MM-DD>_<YYYY-MM-DD>.md` (Mon–Sun week): activity ID, plan → actual, RPE, one-line takeaway.
- **When a plan decision is made:** add a row at the top of `decisions.md` (date | decision | why | alternative | review).
- **When state changes** (FTP, block, next two weeks, open questions): rewrite `NOW.md`.
- **Claims about the athlete** go to `insights.md` with scope, n + activity IDs, status (`гипотеза` / `подтверждено` / `опровергнуто` / `устарело`) and review trigger. Never record a hypothesis there as fact.
- **Every number carries its method** (FTP: ramp 75% / 20′ 95% / 12′ / eFTP; load: power-TSS / HRSS / sRPE-adjusted). Vocabulary in `athlete.md`.
- **Tests:** protocol + conditions in `tests/`, one summary row in `athlete.md`.
- Never copy streams, .fit, GPX or API keys into the exocortex. Do not commit or push; the owner reviews the diff.
- This replaces the Obsidian vault (`CYCLING_VAULT_PATH`) and `plans/active_plan.md` for this athlete.

## Goal and schedule
- Goal: raise FTP over winter 2026/27, mostly indoor (MyWhoosh). Target number and date are set after the test series 26–31.10.2026 — see `NOW.md`.
- **Winter mode until New Year: 3 rides/week, 6–8 h/week, all indoor.** The old 90-min hard cap is lifted. Default split: two key sessions (threshold / sweet spot / VO2) of 75–120 min, non-adjacent, plus one long Z2 endurance session (2.5–3.5 h) to carry the volume. Confirm the split with the athlete when building a block.
- Default block: FTP Builder (see `references/block_templates.md`) with week-to-week progression of time-in-zone; VO2 mix per `decisions.md` (2 of 3 sessions 30/15, 1 of 3 long 4–5′ intervals). Schedule an FTP test per Rule 4 when the last one is >8 weeks old.
- Summer (from spring 2027): long outdoor rides and brevets for durability and fun — do not "optimise" them away; place key sessions ≥2 days from a long ride.

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
