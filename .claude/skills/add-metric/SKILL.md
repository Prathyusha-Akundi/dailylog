---
name: add-metric
description: Add a new trackable metric or daily module to lifelog end to end — model, migration, Today form, Metric class for goals, dashboard widget, tests. Use when asked to track something new.
---

# Add a metric / module to lifelog

Follow these steps in order. Stop and ask if the request belongs to a later phase in docs/PLAN.md.

1. **Clarify the shape**: what is logged per day, its unit, input style (tap / number / text), and which aggregations make sense (sum, count of days, average, latest).
2. **Model** in `app/models/<domain>.py`: include `date` (indexed), `source`, nullable `external_id`. Typed SQLAlchemy 2.x.
3. **Migration**: autogenerate, read it, fix anything wrong, apply locally.
4. **Today screen**: add an HTMX partial in `app/templates/partials/`. Prefer taps. State in the summary how many seconds this adds to daily logging.
5. **Metric class** in `app/metrics/<domain>.py`: subclass `Metric`, implement `aggregate(agg, start, end, filters)` with one SQL query; register in `registry.py`.
6. **Dashboard**: add the widget to week and month views (same component, different range).
7. **Tests**: model round-trip, each supported aggregation, a goal using this metric across a week boundary, and a "stay at most" goal if it applies.
8. Run `ruff` and `pytest`; both must pass. Summarise what changed in 3 lines.
