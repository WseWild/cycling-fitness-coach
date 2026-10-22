# Local Overrides (fork-specific)

How this fork is wired on this machine. **No athlete data lives here** — profile, FTP, schedule, goals, communication style and history are in the athlete's exocortex. When this file conflicts with a workflow or reference, **this file wins**; when the athlete files conflict with a skill default, **the athlete files win**.

## Athlete context: exocortex `cycling/`
Path: `C:\Hit\exo_cortex\cycling\` (git repo `exo_cortex`). intervals.icu is the source of truth for numbers; the exocortex holds what intervals does not remember.
- **Session start:** read `README.md` (goal, schedule, how we talk) → `NOW.md` → `insights.md` → current file in `blocks/` → latest file in `log/`. Profile, vocabulary and test history are in `athlete.md`. If intervals has activities newer than the last `log/` entry, analyse those first. Do not re-ask for facts already in these files.
- **Language, voice, weekly schedule, working FTP, session caps** — take them from `README.md` / `NOW.md`, not from skill defaults. Re-check FTP against the intervals.icu profile each session; if they differ, say so.
- **After a ride analysis:** append 3–5 lines to `log/<YYYY-MM-DD>_<YYYY-MM-DD>.md` (Mon–Sun week): activity ID, plan → actual, RPE, one-line takeaway.
- **When a plan decision is made:** add a row at the top of `decisions.md` (date | decision | why | alternative | review).
- **When state changes** (FTP, block, next two weeks, open questions): rewrite `NOW.md`.
- **Claims about the athlete** go to `insights.md` with scope, n + activity IDs, status (`гипотеза` / `подтверждено` / `опровергнуто` / `устарело`) and review trigger. Never record a hypothesis as fact.
- **Every number carries its method** (FTP: ramp 75% / 20′ 95% / 12′ / eFTP; load: power-TSS / HRSS / sRPE-adjusted). Vocabulary in `athlete.md`.
- **Tests:** protocol + conditions in `tests/`, one summary row in `athlete.md`.
- Never copy streams, .fit, GPX or API keys into the exocortex. Do not commit or push; the owner reviews the diff.
- This replaces the Obsidian vault (`CYCLING_VAULT_PATH`) and `plans/active_plan.md`.

## RPE
- Read session RPE from the intervals.icu activity (`rpe` field). Ask for it only when the activity has none.

## No wellness data
- If intervals.icu has no wellness data for the athlete (check `README.md` / `athlete.md`), **skip** Readiness blocks, `--readiness-check`, `--wellness`, HRV/RHR/sleep gating.
- Substitute: an optional one-line subjective check before key sessions (legs, sleep, stress: ok / so-so / bad), plus TSB, ACWR and RPE-vs-IF trend from `workflows/plan.md` / `weekly_adaptation.md`. Say in weekly reviews that readiness coaching is limited to PMC + RPE.

## Data access: MCP-first
Prefer the `intervals-icu` MCP tools over `scripts/intervals_icu_api.py`:
- Activity / history: `get_activity`, `list_activities`, `get_fitness_summary`, `get_athlete_curves`, `get_activity_streams`.
- Calendar: `list_events`, `get_event`, `create_events`, `update_event`. There is no delete tool — ask the athlete to delete events by hand.
- Python is still used for `pmc_calculator.py`, `generate_zwo.py`, `zwo_lint.py` (these need `.env` credentials only if they call the API).

## Publishing workouts (replaces Zwift-folder writes)
No Zwift folder exists on this machine. Instead of `batch_generate_zwo.py --output-dir <ZWIFT_WORKOUTS_DIR>`:
1. Design workouts as usual; size weekly TSS to the athlete's weekly hours (TSS ≈ h × IF² × 100) — never promise more load than the time allows.
2. Present the week as a table (date, name, duration, target TSS/IF, structure) and **wait for explicit approval**.
3. Write via `create_events` / `update_event` with structured workout text (power as % FTP), names prefixed `AI:`. Check `workout_check` in the result.
4. Report the dates written. Never overwrite existing events without confirmation.
The `.zwo` files may still be saved to a local folder if the athlete asks for Zwift/other importers.
