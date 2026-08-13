# Methodology

## 1. Analytical Objective

This project examines public Twitter/X discussion about TikTok regulation in the United States between 2020 and 2024. Its purpose is to identify engagement patterns, high-visibility accounts, concentrated collection periods, and temporal alignment with selected regulatory milestones.

The analysis is exploratory. It supports regulatory monitoring and hypothesis generation; it does not establish causal effects or measure representative public opinion.

## 2. Data Scope

| Item | Scope |
|---|---|
| Unit of analysis | One cleaned social-media post record |
| Platform represented | Twitter/X |
| Topic | Public discussion about TikTok regulation |
| Geographic policy context | United States |
| Time period | 2020-2024 |
| Final cleaned records | 567K |
| Unique usernames | 314K |
| Engagement fields | Likes, retweets, replies |

The project does not use TikTok platform data or TikTok internal metrics.

## 3. Data Preparation

Power Query was used to transform the original imported table (`all`) into the cleaned analytical table (`TikTok_Clean`). The available model evidence shows the cleaned table retaining fields required for:

- Post date and collection date
- Username and post identifier
- Text
- Follower count
- Like count
- Retweet count
- Reply count
- Location where available
- Source-file metadata

The final Dashboard record count of 567K is based on `COUNTROWS(TikTok_Clean)` and therefore represents the cleaned analytical table.

Specific transformation steps should be interpreted conservatively where the original Power Query script is not published. The repository does not claim undocumented procedures such as language classification, bot detection, sentiment labeling, or geolocation inference.

## 4. Analytical Model

The Power BI model uses the following structure:

| Table | Role |
|---|---|
| `all` | Original imported data |
| `TikTok_Clean` | Cleaned post-level analytical table |
| `Dim_Date` | Date dimension containing month, quarter, and quarter-year fields |
| `Regulatory Events` | Selected regulatory milestones with event dates |
| `Measure` | Reusable numeric DAX measures |
| `KPI Display` | Formatted measures used for presentation |

`TikTok_Clean` is connected to `Dim_Date`. The `Regulatory Events` table uses the same date structure, allowing engagement and selected milestones to be compared using a consistent time dimension.

## 5. KPI Definitions

### Total Posts

Count of cleaned records in `TikTok_Clean`.

```DAX
Total Tweets = COUNTROWS(TikTok_Clean)
```

### Unique Users

Distinct count of usernames appearing in the cleaned table.

```DAX
Unique Users = DISTINCTCOUNT(TikTok_Clean[username])
```

### Total Recorded Engagement

Sum of likes, retweets, and replies available in the dataset.

```DAX
Total Engagement =
[Total Likes] + [Total Retweets] + [Total Replies]
```

This definition does not include views, clicks, saves, quote posts, or interactions not present in the model.

### Average Engagement per Post

```DAX
Average Engagement per Tweet =
DIVIDE([Total Engagement], [Total Tweets])
```

### Average Engagement per User

The legacy model labels this measure `Average Engagement per Actor`. It divides engagement assigned to the account analysis by the number of distinct usernames.

```DAX
Average Engagement per Actor =
DIVIDE([Total Actor Engagement], [Total Actors])
```

For external interpretation, the result is described as average engagement per user/account rather than proof of formal stakeholder status.

### Average Followers

```DAX
Average Followers =
AVERAGE(TikTok_Clean[follower_count])
```

This is an average across post records and may weight accounts with multiple posts more heavily.

### Peak Quarterly Engagement

```DAX
Peak Engagement =
MAXX(
    VALUES(Dim_Date[Quarter-Year]),
    [Total Engagement]
)
```

This calculates the maximum total engagement observed among quarter-year periods.

## 6. Analytical Approaches

### Time-Series Analysis

Post volume and engagement are aggregated by quarter-year to identify peaks and changes across the 2020-2024 period.

### Account-Level Influence Exploration

Accounts are compared using recorded engagement and follower count. High engagement is treated as a visibility signal, not a complete measure of political or social influence.

### Collection-Period Exploration

Collection dates and source-file metadata are used to examine concentration across data batches and high-activity dates. They are not interpreted as verified content publishers or causal sources of discussion.

### Regulatory-Milestone Comparison

Selected regulatory events are mapped to the date dimension and viewed alongside quarterly engagement. The method identifies temporal coincidence and does not estimate causal impact.

## 7. Interpretation Rules

- Use **coincided with**, **was associated with**, or **occurred during** when describing regulatory events and engagement peaks.
- Do not use **caused**, **triggered**, or **drove** unless supported by a separate causal design.
- Refer to high-engagement usernames as **accounts** or **users**; use **stakeholder** only as an exploratory interpretation.
- Do not describe the analysis as sentiment analysis because no validated sentiment measure is included.
- Do not treat the dataset as representative of the US public or all TikTok users.

## 8. Limitations

- The dataset is a supplied portfolio dataset and may reflect collection choices that are not fully documented.
- Public social-media activity is not representative of the general population.
- Engagement counts measure recorded interactions, not agreement, persuasion, or offline impact.
- Repeated posts by the same account affect record-weighted averages such as average follower count.
- Account identity, follower count, and activity may change over time.
- The selected event list is not a comprehensive legal chronology.
- No validated sentiment, stance, network, bot, demographic, or causal analysis is included.
- Collection metadata does not identify verified information sources.

## 9. Reproducibility Notes

To reproduce the published analytical logic:

1. Load the source records into Power Query.
2. Produce a cleaned post-level table containing the documented fields.
3. Create a date dimension covering the analysis period.
4. Map the cleaned post dates and selected event dates to the date dimension.
5. Create the documented DAX measures.
6. Validate overall totals before building visual pages.
7. Interpret event alignment as temporal association rather than causal effect.

[Return to the project README](README.md)
