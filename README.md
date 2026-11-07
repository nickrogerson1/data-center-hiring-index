# US Data Center Hiring Index — aggregate dataset

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.22261857.svg)](https://doi.org/10.5281/zenodo.22261857)

Monthly aggregate snapshot of live US data center job postings on Data Center
JobHub — methodology and current edition at
<https://datacenterjobhub.com/data-center-hiring-index>.

This repository holds the downloadable aggregates behind each published
edition. It contains counts and medians the site computed from its own
monitored listings — no job text, no employer descriptions, no personal data.

## Editions

<!-- editions:start -->
| Edition | Page | Cutoff (UTC) | Version DOI | Files |
|---|---|---|---|---|
| August 2026 | <https://datacenterjobhub.com/data-center-hiring-index/2026/08> | 2026-09-02 10:15:54 | [10.5281/zenodo.22261858](https://doi.org/10.5281/zenodo.22261858) | [`2026-08/data-center-hiring-snapshot-2026-08.csv`](2026-08/data-center-hiring-snapshot-2026-08.csv), [`2026-08/data-center-hiring-snapshot-2026-08-pay.csv`](2026-08/data-center-hiring-snapshot-2026-08-pay.csv) |
| October 2026 | <https://datacenterjobhub.com/data-center-hiring-index/2026/10> | 2026-11-01 10:39:10 | [10.5281/zenodo.24042965](https://doi.org/10.5281/zenodo.24042965) | [`2026-10/data-center-hiring-snapshot-2026-10.csv`](2026-10/data-center-hiring-snapshot-2026-10.csv), [`2026-10/data-center-hiring-snapshot-2026-10-pay.csv`](2026-10/data-center-hiring-snapshot-2026-10-pay.csv), [`2026-10/data-center-hiring-snapshot-2026-10-employers.csv`](2026-10/data-center-hiring-snapshot-2026-10-employers.csv), [`2026-10/data-center-hiring-snapshot-2026-10-panel.csv`](2026-10/data-center-hiring-snapshot-2026-10-panel.csv), [`2026-10/data-center-hiring-snapshot-2026-10-pay-transparency.csv`](2026-10/data-center-hiring-snapshot-2026-10-pay-transparency.csv) |
<!-- editions:end -->

Each edition is frozen at its cutoff and never revised. A later edition is a
new folder, not a change to an old one. There is no September 2026 edition:
its cutoff was not run, and the site keeps no history from which a past
board could be rebuilt faithfully.

## Files

### `data-center-hiring-snapshot-YYYY-MM.csv`

One row per aggregate. Columns:

| Column | Meaning |
|---|---|
| `dimension` | `summary`, `state`, `role` or `employer` |
| `key` | Machine key: a summary metric name, a USPS state code, a role-family key, or an employer slug on the site |
| `label` | Human-readable name |
| `value` | The count |
| `unit` | `count` for summary rows, `live_jobs` otherwise |
| `share_of_all_jobs_pct` | The value as a percentage of all live jobs, one decimal place; blank where a share is not meaningful |

Summary rows: live jobs, employers with live jobs, states represented,
distinct city–state combinations, listings with employer-stated pay, and
listings published in the 7 and 30 days before the cutoff. State rows list the
top 12 states; employer rows the top 12 employers; role rows every role family.

### `data-center-hiring-snapshot-YYYY-MM-pay.csv`

One row per role family and pay period. Columns:

| Column | Meaning |
|---|---|
| `role_key`, `role_label` | Role family |
| `pay_period` | `YEAR` or `HOUR` — never converted between |
| `sample_size` | Number of employer-stated ranges the row is computed from |
| `median_advertised_lower_bound` | Median of the ranges' minimums, USD |
| `median_advertised_upper_bound` | Median of the ranges' maximums, USD |
| `currency` | Always `USD` |

### `data-center-hiring-snapshot-YYYY-MM-employers.csv`

From October 2026. One row for every employer with a live job at the cutoff,
largest first — the aggregate file names only the top 12. It is what the next
edition's panel compares against.

| Column | Meaning |
|---|---|
| `captured_at` | The edition's cutoff, UTC, on every row |
| `employer_slug`, `employer_name` | The employer as the site renders it |
| `live_jobs` | Live jobs at the cutoff |
| `share_of_all_jobs_pct` | Share of all live jobs, one decimal place |

### `data-center-hiring-snapshot-YYYY-MM-panel.csv`

From October 2026. The only change between editions this dataset reports: the
employers in both editions whose boards were read the same way at both
cutoffs, counted under the earlier edition's title rules (see Method).

| Column | Meaning |
|---|---|
| `captured_at`, `base_captured_at` | This edition's cutoff and the previous edition's, UTC |
| `section` | `total` (the panel), `coverage` (employers added since the previous cutoff) or `employer` |
| `key`, `label` | Employer slug and name, or what the row counts |
| `adapter` | How the site reads the employer's board |
| `employers` | Employers the row covers |
| `base_live_jobs`, `live_jobs` | Live jobs at the previous cutoff and at this one |
| `newer_rules_only` | Of `live_jobs`, ads only the newer title rules admit |
| `like_for_like_live_jobs` | `live_jobs` less `newer_rules_only` |
| `change_pct` | Like-for-like against `base_live_jobs`, one decimal place |
| `in_panel` | `yes` or `no` |
| `note` | Why an employer is out of the panel, or what to know about one in it |

### `data-center-hiring-snapshot-YYYY-MM-pay-transparency.csv`

Stated-pay share by state, against whether state law requires a pay range in
the advertisement, with a control that compares employers advertising on both
sides of that line with themselves. It carries **its own cutoff** on every
row, later than the edition's, so its totals differ slightly from the
edition's; the mandate table is dated in `mandate_as_of`. States under 8 live
jobs count in the group totals and have no share of their own.

## Method

- **Population.** Listings live and approved on Data Center JobHub at the
  recorded cutoff. Every listing is in a US state. Closed, expired and
  duplicate listings are excluded; the site removes a listing when its source
  removes it and carries nothing older than 90 days by the source's own
  posting date.
- **Role families.** Job titles are grouped into broad data center role
  families by the site's title-matching rules. A family can include several
  seniority levels and specialties; a job belongs to exactly one family.
- **Pay.** Only employer-stated USD ranges with both a lower and an upper bound
  are used. Aggregator or feed estimates are excluded. Annual and hourly
  listings are reported separately and never converted. A role × period row is
  published only when it has at least 8 ranges, the same bar the site's salary
  pages use.
- **Medians of bounds, not of earnings.** The pay figures are statistics of
  advertised ranges, not of what anyone is paid.
- **Shares** are of all live jobs, to one decimal place.
- **Change between editions** is reported only for a panel: employers named
  in both editions whose boards the site read the same way at both cutoffs.
  An employer is out if the site reads it through a capped keyword search
  (the count is what fills the cap), if the code that reads its board changed
  between the cutoffs, or if its board was not read cleanly around either
  cutoff. The site's title rules also change, so the panel's ads are judged
  again by the rules as they stood at the earlier cutoff, rebuilt from the
  site's source history, and ads only the newer rules admit are counted out.
  Ads the newer rules turn away never reach the site, so the like-for-like
  count is a floor.

## Limits

This is a census of the monitored listings, not of the US labour market. The
monitored employer set grows over time, so totals move with coverage as well as
with hiring: never read the difference between two editions' totals as a
trend. The panel is the comparison, and it is only as wide as the earlier
edition allows — the August 2026 files name 12 employers, from October 2026
every employer is in the employers file. Monitoring began on 21 August 2026,
so earlier posting-date cohorts are incomplete.

## Licence and citation

The aggregates in this repository are released under
[Creative Commons Attribution 4.0 (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/).
Attribute to **Data Center JobHub** and link to
<https://datacenterjobhub.com/data-center-hiring-index>.

Suggested citation, naming the edition you used and its version DOI from the
table above:

> Data Center JobHub (2026). *US Data Center Hiring Snapshot: August 2026.*
> Zenodo. https://doi.org/10.5281/zenodo.22261858

To cite the series as a whole — always resolving to the most recent edition —
use the concept DOI <https://doi.org/10.5281/zenodo.22261857>.

The licence covers the aggregate tables here. It does not extend to the
underlying job listings, which belong to the employers who published them.
