# Data Definitions

This document includes:

1. Core Data Fields
2. Aggregated Metrics
3. Account Behavior Metrics
4. Suggested Detection Thresholds

## 1. Core Data Fields

### 1.1 Individual API Data Points (and Example Interpretations)

**ts (timestamp)**
- **Type**: Timestamp
- **Description**: UTC timestamp for when this data point was observed (ISO 8601 format)
- **Example**: `2026-02-01T09:45:00Z`
- **Usage**: Temporal analysis of coordination patterns and event correlation

**volume**
- **Type**: Integer
- **Description**: Total number of posts observed in this time interval (typically 1 minute)
- **Range**: 50-800+ posts per minute
- **Interpretation**: 
  - 50-200: Normal organic activity
  - 300-800: Potential coordination burst

**burst_score**
- **Type**: Float (0-1)
- **Description**: Burstiness indicator measuring deviation from normal posting patterns
- **Range**: 0.05-0.95
- **Interpretation**:
  - 0.05-0.25: Organic, steady posting
  - 0.35-0.60: Moderate coordination
  - 0.75-0.95: High coordination burst

**synchrony_index**
- **Type**: Float (0-1)
- **Description**: Temporal synchronization measure indicating coordinated timing
- **Range**: 0.10-0.90
- **Interpretation**:
  - 0.10-0.35: Natural human posting rhythms
  - 0.40-0.65: Some coordination
  - 0.70-0.90: High synchronization (suspicious)

**duplication_clusters**
- **Type**: Integer
- **Description**: Count of near-duplicate content clusters detected
- **Range**: 1-15 clusters per minute
- **Interpretation**:
  - 1-4: Normal content diversity
  - 4-8: Moderate recycling
  - 8-15: Mass content duplication (coordination indicator)

**topic (content hash)**
- **Type**: String (8 characters)
- **Description**: Privacy-preserving hash identifier for content cluster
- **Format**: Lowercase hexadecimal (e.g., `a7f3d9e2`)
- **Purpose**: Enables analysis without revealing actual content

## 2. Aggregated Metrics

### 2.1 Coordination Summary Fields (Suggested Signals That Might Assist With Analysis)

**total_points**
- **Type**: Integer
- **Description**: Total number of data points (minutes) analyzed for this content hash
- **Typical Value**: 900 (15 topics × 5 time windows × 12 hours of data)

**avg_burst_score**
- **Type**: Float (0-1)
- **Description**: Average burst score across all time intervals for this content hash
- **Interpretation**:
  - ~0.15: Organic discourse baseline
  - ~0.60: Coordinated manipulation

**max_burst_score**
- **Type**: Float (0-1)
- **Description**: Highest burst score observed for this content hash
- **Purpose**: Identifies peak coordination moments

**high_burst_count**
- **Type**: Integer
- **Description**: Number of intervals with burst_score > 0.7 (coordination threshold)
- **Interpretation**:
  - 0: No coordination events
  - 250-300: Systematic coordination campaign

**avg_synchrony_index**
- **Type**: Float (0-1)
- **Description**: Average temporal synchronization across all intervals
- **Purpose**: Measures overall coordination consistency

**avg_duplication_clusters**
- **Type**: Float
- **Description**: Average number of content duplication clusters per interval
- **Interpretation**:
  - 2-4: Natural content sharing
  - 7-8: Coordinated content recycling

**suspicion_score**
- **Type**: Float (0-1)
- **Description**: Composite coordination indicator combining multiple signals
- **Formula**: `(burst_score * 0.4) + (synchrony_index * 0.4) + (duplication_factor * 0.2)`
- **Interpretation**:
  - 0.18-0.19: Organic civic discourse
  - 0.59-0.60: Coordinated manipulation

## 3. Account Behavior Metrics and Example Interpretations

**avg_new_accounts**
- **Type**: Float (0-1)
- **Description**: Average proportion of posts from accounts 0-7 days old
- **Interpretation**:
  - ~0.10: Normal community growth
  - ~0.27: New account surge (manipulation indicator)

**avg_api_posting**
- **Type**: Float (0-1)
- **Description**: Average proportion of posts made via API clients
- **Interpretation**:
  - ~0.10: Mostly manual posting
  - ~0.28: High automation (coordination indicator)

**new_account_automation_correlation**
- **Type**: Float (0-1)
- **Description**: Correlation between new account activity and API client usage
- **Purpose**: Detects coordinated account creation + automation

## 4. Example Detection Thresholds

### 4.1 Example Coordination Indicators (what would you use?)
- **High burst events**: burst_score > 0.7
- **High synchrony events**: synchrony_index > 0.7
- **High duplication events**: duplication_clusters > 8
- **Coordinated burst flag**: burst_score > 0.7 AND synchrony_index > 0.7
- **New account surge**: avg_new_accounts > 0.3
- **High automation**: avg_api_posting > 0.4

### 4.2 Example Manipulation Score Calculation (what would you use?)
```
manipulation_score = (burst_score * 0.3) + 
                     (synchrony_index * 0.3) + 
                     (min(duplication_clusters/15, 1.0) * 0.2) +
                     (automation_ratio * 0.1) +
                     (new_account_ratio * 0.1)
```

