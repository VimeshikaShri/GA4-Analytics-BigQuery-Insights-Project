# GoDebug — GA4 Analytics & BigQuery Insights Project

**Live app:** [go-debug-assets--vimeshika.replit.app](https://go-debug-assets--vimeshika.replit.app/)
**Analytics tool:** Google Analytics 4 (GA4) → BigQuery Export → SQL analysis
**Author:** Vimeshika Shri

This repository documents an end-to-end web analytics implementation I built and ran on **GoDebug**, a web app I deployed on Replit. It covers everything from instrumenting the app with GA4, to configuring the BigQuery export, to querying the raw event data and turning it into product decisions.

I'm publishing it as a working sample of the kind of analysis, debugging, and cross-functional communication expected in a **[Product Solution Engineer, Apps – gTech Ads Customer Experience](https://www.google.com/about/careers/applications/u/0/jobs/results/91302025064522438-product-solution-engineer-apps-gtech-ads-customer-experience-lcs)** role: reading product telemetry, spotting where tagging or funnels are broken, and translating data into a clear action plan.

---

## Table of contents

1. [Why this project](#why-this-project)
2. [Architecture](#architecture)
3. [Process & procedure](#process--procedure)
4. [GA4 reports built](#ga4-reports-built)
5. [Headline numbers (Jul 2 – Jul 29, 2026)](#headline-numbers-jul-2--jul-29-2026)
6. [Insights & recommendations](#insights--recommendations)
7. [BigQuery export](#bigquery-export)
8. [Repository structure](#repository-structure)
9. [Tools & skills demonstrated](#tools--skills-demonstrated)

---

## Why this project

A big part of a Product Solutions Engineer's job is turning raw product/ads telemetry into a diagnosis: *is this a tagging bug, a funnel problem, or a genuine product issue — and what's the fix?* I wanted a small, real, end-to-end example of doing that rather than just a resume bullet, so I:

- Shipped a real web app (**GoDebug**) to Replit.
- Instrumented it with **GA4** (Google's current analytics platform, the same measurement stack that underpins Google Ads conversion tracking).
- Linked the property to **BigQuery** so I could go past the GA4 UI and query event-level data directly with SQL.
- Used the data to find real issues in the product's funnel and tagging — the same debugging muscle the role calls for.

## Architecture

```
GoDebug (web app, hosted on Replit)
        │  gtag.js / Google tag
        ▼
GA4 Property (ID: 547243036)
        │  standard + custom reports (GA4 UI)
        │  BigQuery Linking (Admin → Product Links → BigQuery Links)
        ▼
BigQuery dataset: analytics_547243036
  (project: project-f3310b3f-c096-49d1-a35)
        │  SQL queries on events_* tables
        ▼
Insights → README (this doc)
```

## Process & procedure

**1. Instrument the app**
Added the Google tag (`gtag.js`) snippet to GoDebug's pages and connected it to a new GA4 **Web** data stream so page views, scrolls, and app events (`signup`, `form_start`, `user_engagement`, etc.) started flowing in.

**2. Validate the tag with DebugView**
Before trusting any report, I used GA4's **DebugView** to confirm events were firing correctly and with the right parameters — the same first step I'd take when troubleshooting a customer's tagging issue: confirm the data is even arriving before analyzing it.

**3. Build out the reporting hub**
Inside [Reports Snapshot](https://analytics.google.com/analytics/web/#/a402431492p547243036/reports/reportinghub), I worked through GA4's standard report collections — **Acquisition, Engagement, Retention, Demographics, Tech, and Life Cycle (Generate leads)** — to get a full picture of who's visiting, where from, on what device, and what they do once they land.

**4. Link the property to BigQuery**
Under **Admin → Product Links → BigQuery Links**, I linked the GA4 property to a Google Cloud project so every event gets exported daily as a row in BigQuery, giving me raw access beyond what the GA4 UI can slice.

- **Project ID:** `project-f3310b3f-c096-49d1-a35`
- **Dataset ID:** `analytics_547243036`
- **Console:** [console.cloud.google.com/bigquery?project=project-f3310b3f-c096-49d1-a35](https://console.cloud.google.com/bigquery?project=project-f3310b3f-c096-49d1-a35)

**5. Query the raw export**
Used standard SQL in BigQuery against the `events_*` tables to reproduce and cross-check what the GA4 UI reports were showing (see [BigQuery export](#bigquery-export) below), and to answer questions the UI can't (e.g., event-level joins, custom cohorts).

**6. Turn findings into recommendations**
Compiled the numbers into the [insights](#insights--recommendations) section below — the part of the workflow that matters most: not just "here's a chart" but "here's what's broken and here's the fix."

## GA4 reports built

All reports below are for the **Last 28 days: Jul 2 – Jul 29, 2026**, scoped to the `All Users` audience. Screenshots are in [`assets/screenshots`](assets/screenshots).

| Report | What it shows |
|---|---|
| [Reports snapshot](assets/screenshots/01-reports-snapshot.png) | Top-line KPIs: users, sessions, engagement time, traffic sources |
| [Traffic overview](assets/screenshots/02-traffic-overview.png) | Country/city breakdown, event counts, stickiness, language |
| [Traffic acquisition — channel group](assets/screenshots/03-traffic-acquisition-channel-group.png) | Sessions by Direct / Paid Search / Organic Social / Cross-network / Paid Video |
| [User acquisition — channel group](assets/screenshots/04-user-acquisition-channel-group.png) | New vs. returning users by first-touch channel |
| [User acquisition — source/medium](assets/screenshots/05-user-acquisition-source-medium.png) | `google / cpc`, `(direct) / (none)`, `linkedin.com / referral` breakdown |
| [Demographics — Country](assets/screenshots/06-demographics-country.png) | Active users, engagement rate, and event count by country |
| [User attributes overview](assets/screenshots/07-user-attributes-overview.png) | Country, city, language (gender/age/interests not yet signalled) |
| [Landing pages](assets/screenshots/08-landing-pages.png) | Sessions and engagement by entry page |
| [Pages & screens](assets/screenshots/09-pages-and-screens.png) | Views and per-page engagement across the app |
| [Tech overview](assets/screenshots/10-tech-overview.png) | Platform, OS, device category, screen resolution |
| [Tech — Browser](assets/screenshots/11-tech-browser.png) | Engagement rate broken out by browser |
| [Audiences](assets/screenshots/12-audiences.png) | `All Users` audience trend |
| [Generate leads overview (Life Cycle)](assets/screenshots/13-generate-leads-overview.png) | Leads-focused view: new/returning users, key events, cohorts |
| [Engagement & retention overview](assets/screenshots/14-engagement-retention-overview.png) | Retention/engagement by cohort, user stickiness |

## Headline numbers (Jul 2 – Jul 29, 2026)

| Metric | Value |
|---|---|
| Active users | 58 |
| New users | 59 |
| Sessions | 70 |
| Avg. engagement time / active user | 30s |
| Engaged sessions / active user | 0.43 |
| Total events | 367 |
| Key events (conversions) configured | 0 |
| Total revenue | ₹0.00 |

**Sessions by default channel group**

| Channel | Sessions | Engagement rate |
|---|---|---|
| Direct | 34 (48.6%) | 32.4% |
| Paid Search | 27 (38.6%) | 40.7% |
| Organic Social | 4 (5.7%) | 50.0% |
| Cross-network | 3 (4.3%) | 33.3% |
| Paid Video | 2 (2.9%) | 0.0% |

**Top countries by active users**

| Country | Active users |
|---|---|
| India | 24 (41.4%) |
| Venezuela | 9 (15.5%) |
| (not set) | 7 (12.1%) |
| Pakistan | 6 (10.3%) |
| Bangladesh / Iraq / UK / USA / Tanzania | 2 each |

**Device & browser mix**

| Dimension | Split |
|---|---|
| Device category | Mobile 84.5% · Desktop 13.8% · Tablet 1.7% |
| Top OS | Android 40 · iOS 10 · Macintosh 5 · Windows 2 |
| Top browsers (active users, engagement rate) | Chrome 41 (37.7%) · Safari 11 (9.1%) · Android Webview 4 (75%) · Samsung Internet 2 (50%) |

**Top event names**

| Event | Count |
|---|---|
| page_view | 153 |
| session_start | 71 |
| first_visit | 59 |
| scroll | 28 |
| Signups | 26 |
| user_engagement | 18 |
| form_start | 12 |

## Insights & recommendations

Reading across these reports side by side (rather than one dashboard in isolation) surfaced a few things worth acting on — this cross-report reasoning is the actual diagnostic step, not just the charts:

- **No conversions are configured.** Every report shows `Key events: 0.00` and `Total revenue: ₹0.00`, even though 26 `Signups` events and 12 `form_start` events fired. The events are present — they're just not marked as **Key events** in GA4 Admin. This is a classic "the data exists, the config doesn't" bug: the fix is a config change (mark `Signups` as a key event), not a re-instrumentation.
- **Safari is under-tracking engagement relative to Chrome.** Chrome active users show a 37.7% engagement rate; Safari active users (11, or ~19% of users) show only 9.1%, despite similar traffic volume. That gap is large enough to suspect a Safari-specific tagging/consent or ITP (Intelligent Tracking Prevention) issue rather than a genuine behavioral difference — worth a DebugView session on Safari specifically.
- **Strong top-of-funnel, softer signup completion.** The homepage (`/`) drives 90% of sessions and 98% of new users, but only 23 views on `/signup` and 26 `Signups` events follow from 59 new users — signalling a homepage → signup drop-off worth funnel-exploring in GA4's Explore reports.
- **Paid Search converts attention better than Direct.** Paid Search sessions show a 40.7% engagement rate vs. 32.4% for Direct, despite Direct carrying slightly more volume — the paid channel is currently the more efficient one on quality, not just quantity.
- **Low return rate (3 of 59 users).** Retention-by-cohort peaks at ~4% and stickiness (DAU/MAU) sits at 6.9%, both consistent with a new/single-burst launch rather than an established retained base — expected at this stage, but the first metric I'd track post-launch.
- **Traffic and users are concentrated in a short burst** (most of the last 28 days show ~0 activity followed by a spike near the end) rather than a steady drip — consistent with a single promotional push (LinkedIn, YouTube, Google Ads/CPC) rather than organic, ongoing discovery.

## BigQuery export

The GA4 property streams its raw event data into BigQuery, which lets me validate the UI reports and answer questions the UI alone can't (event-level joins, custom time windows, funnel math). All queries below run against:

```
project-f3310b3f-c096-49d1-a35.analytics_547243036.events_*
```

**Daily active users**
```sql
SELECT
  event_date,
  COUNT(DISTINCT user_pseudo_id) AS active_users
FROM `project-f3310b3f-c096-49d1-a35.analytics_547243036.events_*`
WHERE _TABLE_SUFFIX BETWEEN '20260702' AND '20260729'
GROUP BY event_date
ORDER BY event_date;
```

**Sessions by traffic source / medium** (cross-checks the User acquisition report)
```sql
SELECT
  traffic_source.source AS source,
  traffic_source.medium AS medium,
  COUNT(DISTINCT user_pseudo_id) AS users
FROM `project-f3310b3f-c096-49d1-a35.analytics_547243036.events_*`
WHERE _TABLE_SUFFIX BETWEEN '20260702' AND '20260729'
  AND event_name = 'first_visit'
GROUP BY source, medium
ORDER BY users DESC;
```

**Event volume by event name** (cross-checks the "Event count by Event name" widget)
```sql
SELECT
  event_name,
  COUNT(*) AS event_count
FROM `project-f3310b3f-c096-49d1-a35.analytics_547243036.events_*`
WHERE _TABLE_SUFFIX BETWEEN '20260702' AND '20260729'
GROUP BY event_name
ORDER BY event_count DESC;
```

**Active users by country**
```sql
SELECT
  geo.country AS country,
  COUNT(DISTINCT user_pseudo_id) AS active_users
FROM `project-f3310b3f-c096-49d1-a35.analytics_547243036.events_*`
WHERE _TABLE_SUFFIX BETWEEN '20260702' AND '20260729'
GROUP BY country
ORDER BY active_users DESC;
```

**Homepage → signup drop-off** (the funnel question the GA4 UI table alone doesn't answer directly)
```sql
WITH homepage_users AS (
  SELECT DISTINCT user_pseudo_id
  FROM `project-f3310b3f-c096-49d1-a35.analytics_547243036.events_*`
  WHERE _TABLE_SUFFIX BETWEEN '20260702' AND '20260729'
    AND event_name = 'page_view'
    AND (SELECT value.string_value FROM UNNEST(event_params) WHERE key = 'page_location') LIKE '%/'
),
signup_users AS (
  SELECT DISTINCT user_pseudo_id
  FROM `project-f3310b3f-c096-49d1-a35.analytics_547243036.events_*`
  WHERE _TABLE_SUFFIX BETWEEN '20260702' AND '20260729'
    AND event_name = 'Signups'
)
SELECT
  (SELECT COUNT(*) FROM homepage_users) AS homepage_users,
  (SELECT COUNT(*) FROM signup_users)   AS signed_up_users,
  ROUND(SAFE_DIVIDE((SELECT COUNT(*) FROM signup_users), (SELECT COUNT(*) FROM homepage_users)) * 100, 1) AS conversion_pct;
```

> These queries are illustrative of the analysis approach; field names (`traffic_source`, `geo.country`, `event_params`) follow the [standard GA4 BigQuery export schema](https://support.google.com/analytics/answer/7029846).

## Repository structure

```
.
├── README.md
└── assets/
    └── screenshots/
        ├── 01-reports-snapshot.png
        ├── 02-traffic-overview.png
        ├── 03-traffic-acquisition-channel-group.png
        ├── 04-user-acquisition-channel-group.png
        ├── 05-user-acquisition-source-medium.png
        ├── 06-demographics-country.png
        ├── 07-user-attributes-overview.png
        ├── 08-landing-pages.png
        ├── 09-pages-and-screens.png
        ├── 10-tech-overview.png
        ├── 11-tech-browser.png
        ├── 12-audiences.png
        ├── 13-generate-leads-overview.png
        └── 14-engagement-retention-overview.png
```

## Tools & skills demonstrated

- **Google Analytics 4** — property/stream setup, DebugView validation, standard + custom reports across Acquisition, Engagement, Retention, Tech, and Life Cycle collections
- **Google Cloud / BigQuery** — linking a GA4 property to BigQuery, querying the raw `events_*` export with SQL
- **Debugging & root-cause analysis** — spotting a missing Key-event configuration, a Safari-specific engagement gap, and a homepage→signup drop-off by cross-referencing multiple reports rather than reading one in isolation
- **Communicating data to a non-technical audience** — translating raw metrics into a short list of concrete, actionable recommendations

---

*This project is a self-directed case study built to demonstrate analytics instrumentation, SQL/BigQuery fluency, and product-debugging thinking relevant to Google's gTech Ads Customer Experience team.*
