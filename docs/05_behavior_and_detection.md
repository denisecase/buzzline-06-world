## Civic Transparency: Behavior and Detection

Real-world platform integrity teams analyze behavioral signals to detect coordination while preserving user privacy.
This notebook simulates an approach using synthetic civic discourse data structured as if it came from a proposed transparency API.

### What if platforms provided transparency data that protected user privacy?

Instead of raw posts and user data, imagine platforms shared **aggregated behavioral signals**:

**What we get:**

- How many posts per minute on a topic
- What percentage are reshares vs original content
- Age distribution of accounts (bucketed: "0-7 days old", "1-6 months", etc.)
- Posting method distribution ("manual", "scheduled", "API tools")
- Coordination indicators (burst patterns, synchronization, content duplication)

**What we DON'T get:**

- Individual usernames or IDs
- Actual post content
- Personal information
- Specific account details

**Privacy Considerations:**
- Aggregated data still carries re-identification risks
- Behavioral fingerprints can be unique with enough dimensions
- Platform teams have additional safeguards (differential privacy, k-anonymity)
- This exercise uses synthetic data with privacy protections built in

### Key Signals for Detection

1. **Burst Score (0-1)**: How "bursty" is the activity?

- Measures concentration of activity over time.
- Calculated as: variance in posting rate / mean posting rate
- 0.1 = steady rate (e.g., 10 posts/hour consistently)
- 0.9 = spike pattern (e.g., 200 posts in 5 min, then 2/hour)
- Based on burstiness detection from time series analysis

2. **Synchrony Index (0-1)**: How synchronized are the posts?

- Measures time-based, temporal clustering of posts
- 0.2 = posts distributed randomly across time
- 0.8 = 80%+ of posts occur in synchronized clusters
- Natural discussion shows 0.2-0.4; coordinated may show 0.7+

3. **Duplication Clusters**: How many groups of near-identical content?

- Organic: 2-3 clusters (people independently express similar thoughts)
- Suspicious: 15+ clusters (suggests copy-paste coordination)
- Threshold based on: population size, topic complexity, time window
- Note: Legitimate viral content can also create clusters

4. **Account Age Distribution**: Are these established users or new accounts?

- Groups accounts into buckets by age, using an enumerated type
- For example: ["0-7d", "8-30d", "1-6m", "6-24m", "24m+"]
- Tells what percent of the posters fall into each bucket
- Organic movements: bell curve across age ranges
- Coordinated campaigns: spike in "0-7 days" accounts
- Real campaigns also recruit new users, so age alone isn't diagnostic

5. **Automation Indicators**: Are posts coming from humans or automated tools?
- Groups accounts into buckets by account type, using an enumerated type
- For example: "enum": ["person","org","media","public_official","unverified","declared_automation"]
- Manual: Web/mobile app posting by humans
- Scheduled: Legitimate tools (Buffer, Hootsuite) used by real people
- API: Direct API access (can be legitimate developers or bots)
- High API use with other signals may be suspicious
- API use alone is not diagnostic (e.g., journalists, marketers use APIs)

### What Is Normal? Baseline Patterns in Organic Discourse

Typical ranges for legitimate civic discussions:
- Burst score: 0.10-0.35 (higher during breaking news)
- Synchrony: 0.15-0.40 (higher for time-sensitive events)
- New accounts: 5-15% of active accounts
- API posting: 10-20% (scheduled tools are common)

**Deviations** from these patterns warrant investigation, not automatic interpretation.
Context matters - for example, a genuine grassroots mobilization may show coordination indicators.

### Why These Signals?

These metrics derive from:
- **Time series analysis**: Burst detection algorithms from network monitoring
- **Social network theory**: Coordination requires temporal clustering
- **Information diffusion models**: Natural vs artificial spread patterns
- **Platform research**: Published studies on inauthentic behavior

References: Cha et al. (2010), Ferrara et al. (2016), Badawy et al. (2019)
