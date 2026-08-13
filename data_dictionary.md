# Data Dictionary

## Dataset Overview

The analytical dataset contains cleaned public Twitter/X post records about TikTok regulation in the United States between 2020 and 2024. The final Power BI analytical table contains 567K cleaned records.

Field names are documented from the available Power BI model. Types and definitions should be interpreted according to the supplied dataset rather than as a live Twitter/X API specification.

## Core Tables

| Table | Description |
|---|---|
| `all` | Original imported source records before final analytical cleaning |
| `TikTok_Clean` | Cleaned post-level table used for measures and visual analysis |
| `Dim_Date` | Date dimension used for monthly and quarterly reporting |
| `Regulatory Events` | Selected US TikTok regulatory milestones and their dates |
| `Measure` | Numeric DAX measures |
| `KPI Display` | Formatted measures used in KPI cards |

## `TikTok_Clean` Fields

| Field | Expected type | Description | Analytical use |
|---|---|---|---|
| `id` | Text / identifier | Post identifier supplied in the dataset | Post-level reference and possible uniqueness checks |
| `username` | Text | Username or account label associated with the post | Distinct-user counts and account ranking |
| `created_at` | Date/DateTime | Date or timestamp associated with post creation | Time-series analysis |
| `Collection Date` | Date | Date associated with dataset collection or source-file period | Collection-period exploration; not a verified publisher/source |
| `text` | Text | Post text where retained | Topic context; no validated sentiment measure is published |
| `follower_count` | Whole number | Recorded follower count associated with the account record | Reach comparison and average follower calculation |
| `like_count` | Whole number | Recorded likes | Engagement calculation |
| `retweet_count` | Whole number | Recorded retweets | Engagement calculation |
| `reply_count` | Whole number | Recorded replies | Engagement calculation |
| `location` | Text | Self-reported or supplied location field where available | Context only; not treated as verified geolocation |
| `Source.Name` | Text | Source-file or collection-batch metadata | Data lineage and exploratory grouping; not a publisher or platform source |

## `Dim_Date` Fields

| Field | Type | Description |
|---|---|---|
| `Date` | Date | Unique calendar date used for relationships and filtering |
| `Month` | Text | Calendar month label |
| `Month Number` | Whole number | Numeric month used for chronological sorting |
| `Quarter` | Text | Calendar quarter label |
| `Quarter-Year` | Text | Combined quarter and year used in trend and peak calculations |

## `Regulatory Events` Fields

| Field | Type | Description |
|---|---|---|
| `Date` | Date | Date assigned to the selected regulatory milestone |
| `Event` | Text | Short description of the selected milestone |

The event table supports timeline comparison. It is not presented as a complete legal database.

## Published Measures

| Measure | Definition |
|---|---|
| `Total Tweets` | Count of rows in `TikTok_Clean` |
| `Unique Users` | Distinct count of `TikTok_Clean[username]` |
| `Total Actors` | Legacy duplicate concept based on distinct usernames; interpreted externally as unique users/accounts |
| `Total Likes` | Sum of recorded likes |
| `Total Retweets` | Sum of recorded retweets |
| `Total Replies` | Sum of recorded replies |
| `Total Engagement` | Total Likes + Total Retweets + Total Replies |
| `Average Engagement per Tweet` | Total Engagement / Total Tweets |
| `Average Engagement per Actor` | Legacy label for engagement divided by distinct accounts in the account analysis |
| `Average Followers` | Average `follower_count` across post records |
| `Peak Engagement` | Maximum quarterly Total Engagement across `Quarter-Year` values |

## Published KPI Results

| KPI | Result |
|---|---:|
| Cleaned posts | 567K |
| Unique users | 314K |
| Total recorded engagement | 9.1M |
| Average engagement per post | 16.06 |
| Peak engagement period | Q1 2024 |
| Peak quarterly engagement | 2.4M |

## Metric Caveats

- `Total Engagement` includes only likes, retweets, and replies available in the dataset.
- `Average Followers` is calculated across post records and is not necessarily an unweighted average of unique accounts.
- `Total Actors` and `Unique Users` use the same distinct-username concept in the reviewed model.
- The legacy `Average Engagement per Source` measure is not documented as a valid source KPI because its reviewed formula divides total engagement by total retweets.
- `Source.Name` and `Collection Date` are treated as collection metadata, not verified media sources.
- No sentiment, stance, bot, network-centrality, demographic, or causal metric is included.

[Return to the project README](README.md)
