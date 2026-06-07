## Garmin Ecosystem (via Garmin MCP)

Use the Garmin MCP tools to pull real-time physiological data. Always prefer compact/summary endpoints to avoid token bloat.

### Daily Check-In Data Pull

When performing a daily or weekly check-in, retrieve the following data using the Garmin MCP.

**Priority 1 (Always Pull):**

| Data Need | MCP Tool | Key Params |
|-----------|----------|------------|
| Training Readiness | `get_training_readiness` | `date` (YYYY-MM-DD) |
| Morning Readiness | `get_morning_training_readiness` | `date` (YYYY-MM-DD) |
| HRV (today) | `get_hrv_data` | `date`, `return_timeseries=false` |
| HRV Trend (7-14 days) | `get_hrv_trend` | `start_date`, `end_date` (max 30 days) |
| Resting Heart Rate | `get_rhr_day` | `date` |
| Sleep Quality | `get_sleep_summary` | `date` (compact, ~350 bytes) |
| Body Battery | `get_body_battery` | `start_date`, `end_date` |
| Daily Stats | `get_stats` | `date` (steps, calories, HR, stress, sleep) |
| Lifestyle Logs | `get_lifestyle_logging_data` | `date` (retrieve yesterday and today's logs) |

**Priority 2 (Weekly/Deep Dive):**

| Data Need | MCP Tool | Key Params |
|-----------|----------|------------|
| Training Load Trend (ACWR) | `get_training_load_trend` | `start_date`, `end_date` (4-8 weeks recommended, max 90 days). Returns CTL, ATL, TSB, ACWR per day |
| VO2 Max Trend | `get_vo2max_trend` | `start_date`, `end_date` (4-12 weeks recommended, max 90 days) |
| Training Status | `get_training_status` | `date` (load, VO2 max, recovery indicators) |
| Race Predictions | `get_race_predictions` | No params. Returns predicted 5K, 10K, HM, Marathon times |
| Endurance Score | `get_endurance_score` | `start_date`, `end_date` |
| Hill Score | `get_hill_score` | `start_date`, `end_date` |
| Lactate Threshold | `get_lactate_threshold` | Optional `start_date`, `end_date`. Omit for latest |
| Fitness Age | `get_fitnessage_data` | `date`, `details=true` for breakdown |
| Weekly Intensity Minutes | `get_weekly_intensity_minutes` | `end_date`, `weeks` (default 4, max 52) |

**Priority 3 (Activity-Specific):**

| Data Need | MCP Tool | Key Params |
|-----------|----------|------------|
| Recent Activities | `get_activities_by_date` | `start_date`, `end_date`, optional `activity_type` (e.g. "running") |
| Activity Detail | `get_activity` | `activity_id` (integer) |
| Activity Splits | `get_activity_splits` | `activity_id` |
| HR Zone Distribution | `get_activity_hr_in_timezones` | `activity_id` |
| Training Effect | `get_training_effect` | `activity_id` (aerobic/anaerobic effect) |
| Advanced FIT Data | `get_activity_fit_data` | `activity_id`, `include_records=false` (set true only when deep-diving a specific workout) |

**Priority 4 (Supporting/Contextual):**

| Data Need | MCP Tool | Key Params |
|-----------|----------|------------|
| Stress Data | `get_stress_data` | `date` (full time-series, ~35KB) |
| Body Battery Events | `get_body_battery_events` | `date` (drain/charge events) |
| Garmin Body Composition | `get_body_composition` | `start_date`, optional `end_date` |
| Respiration Data | `get_respiration_data` | `date` (~20KB time-series) |
| Progress Summary | `get_progress_summary_between_dates` | `start_date`, `end_date`, `metric` (e.g. "elevationGain", "distance", "duration") |

### Data Size Warnings

- **Prefer** `get_sleep_summary` over `get_sleep_data` (~350B vs ~50KB)
- **Prefer** `get_stats` over individual daily metric pulls (one call, curated data)
- **Avoid** `get_heart_rates` unless specifically analyzing intra-day HR patterns (~25KB)
- **Avoid** `get_activity_fit_data` with `include_records=true` unless deep-diving a single key workout
- **Avoid** `get_hrv_data` with `return_timeseries=true` unless diagnosing overnight HRV anomalies
