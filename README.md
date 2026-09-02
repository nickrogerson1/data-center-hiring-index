# US Data Center Hiring Index — aggregate dataset

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.22261857.svg)](https://doi.org/10.5281/zenodo.22261857)

Monthly aggregate snapshot of live US data center job postings on Data Center
JobHub — methodology and current edition at
<https://datacenterjobhub.com/data-center-hiring-index>.

This repository holds the downloadable aggregates behind each published
edition. It contains counts and medians the site computed from its own
monitored listings — no job text, no employer descriptions, no personal data.

## Editions

| Edition | Page | Cutoff (UTC) | Files |
|---|---|---|---|
| August 2026 | <https://datacenterjobhub.com/data-center-hiring-index/2026/08> | 2026-09-02 10:15:54 | [`2026-08/data-center-hiring-snapshot-2026-08.csv`](2026-08/data-center-hiring-snapshot-2026-08.csv), [`2026-08/data-center-hiring-snapshot-2026-08-pay.csv`](2026-08/data-center-hiring-snapshot-2026-08-pay.csv) |

Each edition is frozen at its cutoff and never revised. A later edition is a
new folder, not a change to an old one.

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

## Limits

This is a census of the monitored listings, not of the US labour market. The
monitored employer set grows over time, so totals move with coverage as well as
with hiring; treat month-to-month change as indicative only until the employer
panel is stable. Monitoring began on 21 August 2026, so earlier posting-date
cohorts are incomplete.

## Licence and citation

The aggregates in this repository are released under
[Creative Commons Attribution 4.0 (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/).
Attribute to **Data Center JobHub** and link to
<https://datacenterjobhub.com/data-center-hiring-index>.

Suggested citation:

> Data Center JobHub (2026). *US Data Center Hiring Snapshot: August 2026.*
> Zenodo. https://doi.org/10.5281/zenodo.22261858

The DOI above is the August edition. To cite the series as a whole — always
resolving to the most recent edition — use the concept DOI
<https://doi.org/10.5281/zenodo.22261857>.

The licence covers the aggregate tables here. It does not extend to the
underlying job listings, which belong to the employers who published them.
