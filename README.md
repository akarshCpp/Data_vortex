# DATA VORTEX – Rebuilding the Social Engine

## Round 1 – Phase 1

This project focuses on recovering and restoring a deliberately corrupted social-media dataset as part of the DATA VORTEX Round 1 challenge.

The objective was to reconstruct a reliable analytical dataset by identifying corruption, handling missing and invalid values, standardizing formats, validating relationships, and performing exploratory data analysis (EDA).

---

## Dataset

The recovered Social Engine data consists of two datasets:

### Users
Contains information about 1,500 users.

| Column | Description |
|---|---|
| user_id | Unique user identifier |
| location | User location |
| language | User language |
| account_created | Account creation date |
| follower_count | Number of followers |

### Posts
The original corrupted dataset contained 12,360 records.

After cleaning and duplicate removal, the final dataset contains:

- 12,000 unique posts
- 8 columns
- May 1, 2024 – April 30, 2025

| Column | Description |
|---|---|
| post_id | Unique post identifier |
| user_id | User who created the post |
| platform | Social media platform |
| text_content | Post content |
| timestamp | Post timestamp |
| likes | Number of likes |
| shares | Number of shares |
| comments | Number of comments |

---

## Data Recovery & Cleaning

The corrupted dataset contained several data-quality issues, including:

- Duplicate post records
- Missing platform values
- Missing and corrupted text values
- Missing likes
- Negative likes
- HTML tags and HTML entities in text
- Character encoding corruption
- Literal and corrupted NULL values
- Multiple timestamp formats

### Cleaning approach

The preprocessing pipeline:

1. Removed duplicate posts using `post_id`.
2. Standardized missing platform values as `Unknown`.
3. Cleaned HTML tags and HTML entities from text.
4. Repaired detectable character encoding corruption.
5. Converted NULL-like text values into `[MISSING_TEXT]`.
6. Standardized timestamps into a consistent datetime format.
7. Replaced invalid/missing likes using platform-level median values where possible.
8. Used the overall valid median for records where a platform-level median was unavailable.
9. Preserved valid shares and comments.
10. Validated user-post relationships.
11. Removed temporary EDA columns before exporting the final dataset.

All transformations were documented and justified in the notebook.

---

## Exploratory Data Analysis

The EDA covers:

### Engagement Analysis
- Likes, shares and comments distribution
- Platform-level engagement
- Engagement correlations
- Highly engaged posts

### Temporal Analysis
- Monthly posting activity
- Day-of-week posting patterns
- Monthly average engagement

Hourly analysis was not used for behavioral interpretation because some source timestamps were date-only values that normalize to midnight.

### Content Analysis
- Post text length distribution
- Text length vs engagement
- Hashtag usage
- Mention usage
- Common words

### User & Follower Analysis
- Posts per user
- Follower-count distribution
- Follower count vs engagement
- Posting frequency vs follower count

### Anomaly Analysis
- Statistical engagement outliers
- Zero-engagement values
- Negative engagement validation
- Duplicate validation
- Timestamp validation
- Referential integrity

---

## Key Findings

### Data Quality
The corrupted source contained multiple inconsistencies, but the final dataset contains:

- 12,000 unique posts
- 1,500 users
- 0 missing values
- 0 duplicate post IDs
- 0 negative engagement values
- 0 invalid user references

### Engagement
No IQR-based statistical outliers were detected in likes, shares, or comments.

Zero engagement values were extremely rare and were retained because they represent valid observations.

### Temporal Behaviour
Posting activity remained relatively stable throughout the one-year period.

- Highest monthly post count: May 2024 – 1,038 posts
- Lowest monthly post count: February 2025 – 914 posts

Average monthly engagement showed moderate fluctuations but no sustained upward or downward trend.

### Content
Valid post text averaged approximately 118 characters.

83.1% of valid-text posts contained between 101 and 150 characters.

Text length had almost no linear relationship with likes, shares or comments.

Approximately 13.5% of valid-text posts contained mentions.

### Users & Followers
Users published an average of 8 posts, with a median of 8.

Follower count showed almost no linear relationship with likes, shares or comments.

Posting frequency and follower count also showed almost no relationship, with a correlation of -0.008.

---

## Repository Structure

```text
DATA-VORTEX/
│
├── README.md
├── ROUND1.ipynb
│
├── data/
│   ├── Social_Engine_Users_Cleaned.csv
│   └── Social_Engine_Posts_Cleaned.csv
│
└── report/
    └── Phase1_EDA_Report.pdf
