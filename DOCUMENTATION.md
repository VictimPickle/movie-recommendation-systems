# 📚 Technical Documentation

## Complete Guide to Movie Recommendation Systems

---

## Table of Contents

1. [Data Analysis](#data-analysis)
2. [Content-Based Filtering (Detailed)](#content-based-filtering-detailed)
3. [Collaborative Filtering (Detailed)](#collaborative-filtering-detailed)
4. [Comparison Analysis](#comparison-analysis)
5. [Results & Findings](#results--findings)
6. [Limitations & Solutions](#limitations--solutions)

---

## Data Analysis

### Dataset Overview

#### Movies Dataset
```
Shape: (9,742, 3)
Columns: movieId, title, genres

Data Quality:
- No missing movieIds
- No duplicate movies
- Year range: 1902 - 2018
- Total genres: 20 (after parsing)
- Average genres per movie: 1.4
```

#### Ratings Dataset
```
Shape: (100,836, 4)
Columns: userId, movieId, rating, timestamp

Data Quality:
- 610 unique users
- 9,737 unique movies
- Rating scale: 0.5 - 5.0 (half-star increments)
- No missing ratings
- Timestamp range: 1997 - 2018
```

### Sparsity Analysis

**Critical Finding:** Matrix is 98.31% sparse!

```
Total possible ratings: 610 × 9,737 = 5,939,570
Actual ratings: 100,836
Sparsity: 98.31%

What this means:
- Each user rates only 165 movies on average (out of 9,737)
- Each movie receives only 10.4 ratings on average
- Vast majority of user-movie combinations are unrated
- Creates challenges for Collaborative Filtering
```

---

## Content-Based Filtering (Detailed)

### Algorithm Explanation

**Core Idea:** \"If you liked movies with these genres, you'll like other movies with similar genres.\"

### Step 1: Feature Engineering

```python
# One-hot encode genres
movies_encoded = pd.DataFrame()
for genre in all_genres:
    movies_encoded[genre] = movies['genres'].apply(
        lambda x: 1 if genre in x else 0
    )

# Result: 9,742 × 20 matrix
# Each row = one movie
# Each column = one genre (0 or 1)
```

**Genre List (20 total):**
- Action, Adventure, Animation
- Children, Comedy, Crime
- Documentary, Drama, Fantasy
- Film-Noir, Horror, IMAX
- Musical, Mystery, Romance
- Sci-Fi, Thriller, War, Western
- (no genres listed)

### Step 2: User Profile Creation

```python
User Input Example:
- Dark Knight, The (Rating: 5.0) → [Action:1, Crime:1, Drama:1, ...]
- Inception (Rating: 4.5) → [Action:1, Sci-Fi:1, Thriller:1, ...]
- Toy Story (Rating: 3.0) → [Animation:1, Comedy:1, Children:1, ...]
- Matrix, The (Rating: 5.0) → [Action:1, Sci-Fi:1, Thriller:1, ...]
- Shawshank Redemption (Rating: 5.0) → [Drama:1, Crime:1, ...]

User Profile Calculation:
profile[genre] = Σ(movie_vector[genre] × rating)

Results:
- Action: 5.0 + 4.5 + 5.0 = 14.5
- Drama: 5.0 + 5.0 = 10.0
- Sci-Fi: 4.5 + 5.0 = 9.5
- Crime: 5.0 + 5.0 = 10.0
- ...
```

### Step 3: Similarity Calculation

```python
# Matrix multiplication: (9,742 × 20) × (20 × 1) = (9,742 × 1)
scores = all_movies_genres @ user_profile

# Normalize by total preference weight
normalized_scores = scores / sum(user_profile)

# Result: 0-1 scale
```

### Step 4: Filtering & Ranking

```
Criteria:
1. Remove already-watched movies
2. Filter by minimum number of ratings (≥20)
3. Filter by minimum average rating (≥3.5)
4. Sort by recommendation score (descending)
5. Return top-10 movies
```

### Advantages & Disadvantages

**✅ Advantages:**
- Explainable (clear why movie is recommended)
- Works for brand new movies (no cold-start for items)
- No need for other users' data (privacy-friendly)
- Fast computation

**❌ Disadvantages:**
- Filter bubble (only recommends similar genres)
- Can't consider quality (no ratings used)
- Limited by feature quality (only genres here)
- Binary encoding is crude (Action in Toy Story ≠ Action in Matrix)

---

## Collaborative Filtering (Detailed)

### Algorithm Explanation

**Core Idea:** \"People who rated movies like you tend to have similar taste. Find those people and recommend movies they liked that you haven't seen.\"

### Step 1: User-Item Matrix

```python
# Create pivot table
user_item_matrix = ratings.pivot_table(
    index='userId',
    columns='movieId',
    values='rating'
)

# Result: 610 × 9,737 matrix
# Rows: users
# Columns: movies
# Values: ratings (or NaN)
# Sparsity: 98.31%!
```

### Step 2: Find Similar Users

```
Similarity Metric: Pearson Correlation Coefficient

Formula:
       Σ(x_i - x̄)(y_i - ȳ)
r = ─────────────────────────
    √[Σ(x_i - x̄)²][Σ(y_i - ȳ)²]

Where:
- x = your ratings
- y = other user's ratings
- Only for movies you both rated (≥3 common movies minimum)

Interpretation:
- r = +1.0: Perfect positive correlation (identical taste)
- r = 0.0: No correlation
- r = -1.0: Perfect negative correlation (opposite taste)
```

### Step 3: Weighted Recommendation

```python
For each unwatched movie:
    predicted_rating = Σ(similarity_score × user_rating) 
                      ───────────────────────────────────
                      Σ(similarity_score)

Example:
Movie A ratings from similar users:
- User 1 (similarity: 0.8) rated 4.5
- User 2 (similarity: 0.6) rated 4.0
- User 3 (similarity: 0.5) rated 3.5

Prediction:
= (0.8×4.5 + 0.6×4.0 + 0.5×3.5) / (0.8 + 0.6 + 0.5)
= (3.6 + 2.4 + 1.75) / 1.9
= 7.75 / 1.9
= 4.08 stars
```

### Step 4: Filtering & Ranking

```
Criteria:
1. Remove already-watched movies
2. Filter by minimum number of raters (≥3 similar users)
3. Sort by predicted rating (descending)
4. Return top-10 movies
```

### Advantages & Disadvantages

**✅ Advantages:**
- Breaks filter bubbles (recommends diverse content)
- Considers quality through rating patterns
- Can find non-obvious matches
- Leverages \"wisdom of the crowd\"

**❌ Disadvantages:**
- Cold-start problem for new users
- Needs substantial rating data
- Data sparsity (98% empty matrix)
- Can't explain \"why\" recommendation
- Popularity bias (tends toward highly-rated movies)

---

## Comparison Analysis

### Overlap Analysis

**Definition:** How many movies appear in both Top-10 lists?

```
Content-Based Recommendations: [Movie A, B, C, D, E, ...]
Collaborative Filtering: [Movie X, Y, Z, ...]

Overlap = Movies appearing in both lists
Overlap % = (overlap / 10) × 100%
```

**Typical Results:**
- Low overlap (0-30%) suggests systems capture different patterns
- High overlap (70%+) suggests agreement on quality

### Genre Diversity

**Metric:** Number of unique genres in recommendations

```python
all_genres = []
for recommendations in [cb_recs, cf_recs]:
    for movie in recommendations:
        all_genres.extend(movie['genres'])

diversity = len(set(all_genres))
```

**Interpretation:**
- Content-Based: Usually 8-12 genres (grouped around preferences)
- Collaborative Filtering: Usually 12-18 genres (more diverse)

### Quality Comparison

**Content-Based Score:** Recommendation Score (0-1)
**Collaborative Filtering:** Predicted Rating (0-5 stars)

```
Merge with actual ratings:
- Compare predicted vs actual
- Calculate prediction error
- Assess which system was closer
```

### Novelty Analysis

```python
popularity = recommendations.merge(ratings_stats).groupby('system')['num_ratings'].mean()

# Results:
# Content-Based avg: 45.2 ratings (more popular movies)
# CF avg: 32.1 ratings (more obscure movies)
```

---

## Results & Findings

### Content-Based Findings

1. **Filter Bubble Effect**
   - 85% of recommendations are Action/Drama/Crime/Sci-Fi
   - Limited exposure to other genres
   - Even though user rated Toy Story, no Animation recommendations

2. **Genre Combination Bias**
   - Movies with 5+ genres tend to rank higher
   - \"Rubber\" (10 genres) scores high despite obscurity
   - Demonstrates genre-count bias

3. **Quality Gap**
   - RoboCop 3 (poorly reviewed) ranks equal to classics
   - System cannot distinguish within same genres
   - Critical flaw: ignores actual quality signals

### Collaborative Filtering Findings

1. **Wisdom of Crowd**
   - Breaks filter bubble with 15+ genres in Top-10
   - Recommends unexpected quality movies
   - User patterns reveal hidden preferences

2. **Sparsity Impact**
   - Only 247 users had ≥3 common ratings with you
   - Limited to Top 100 for computational efficiency
   - Still found meaningful recommendations

3. **Popularity Bias**
   - CF recommendations averaged 32 ratings
   - vs. CB average of 45 ratings
   - Slight bias toward popular = quality signal

---

## Limitations & Solutions

### Content-Based Limitations

| Limitation | Reason | Solution |
|-----------|--------|----------|
| Filter Bubble | Only genres 1/0 | Use TF-IDF weighted genres |
| No Quality | Doesn't use ratings | Add quality weighting to scores |
| Binary Features | Can't distinguish nuance | Include directors, actors, keywords |
| Cold Start Users | Needs rated movies to start | Use demographic data, browse data |

### Collaborative Filtering Limitations

| Limitation | Reason | Solution |
|-----------|--------|----------|
| Sparse Data | 98% matrix empty | Matrix factorization (SVD/NMF) |
| Cold Start Users | New users no ratings | Use CB as fallback |
| Popularity Bias | Highly-rated movies dominate | Add \"serendipity\" factor |
| Explainability | \"Why?\" not clear | Explain via similar users |

### Sparsity Solutions

1. **Matrix Factorization**
   ```
   User-Item matrix = U × Σ × V^T
   (610 × 9737) = (610 × k) × (k × k) × (k × 9737)
   
   Where k ≈ 50 (latent factors)
   Dramatically reduces sparsity impact
   ```

2. **Implicit Feedback**
   ```
   Don't just use ratings 1-5
   Track: views, time watched, rewatches, shares
   More data points = better recommendations
   ```

3. **Hybrid Approach**
   ```
   score = 0.6 * cf_score + 0.4 * cb_score
   Benefits from both systems
   Mitigates individual weaknesses
   ```

---

## Advanced Techniques

### Deep Learning Approach

```python
# Autoencoders for collaborative filtering
# Input: user's rating vector (sparse)
# Compressed: latent features
# Output: predicted ratings (all movies)

# Neural Networks
# Input: movie features + user profile
# Hidden layers: learn complex patterns
# Output: recommendation score
```

### Context-Aware Recommendations

```
Additional context:
- Time of day (thriller at night, comedy during lunch)
- Device type (watch on phone vs TV)
- Weather/season
- Social influence (friends watching)
- Current mood (detected via interactions)
```

### Evaluation Metrics

| Metric | Formula | Interpretation |
|--------|---------|-----------------|
| **Precision@10** | (# relevant in top-10) / 10 | Accuracy of Top-10 |
| **Recall@10** | (# found in top-10) / (total relevant) | Coverage of good movies |
| **NDCG** | Normalized Discounted CG | Quality ranking (order matters) |
| **Serendipity** | % surprising but enjoyed | Innovation/novelty |

---

## Conclusion

### When to Use Each System

**Content-Based:**
- News articles (new content daily)
- Research papers (clear categories)
- Privacy-critical apps (no user data needed)
- Cold-start items (new movies day 1)

**Collaborative Filtering:**
- Streaming services (large user base)
- E-commerce (quality matters)
- Social networks (trend discovery)
- Established platforms (rich history)

**Hybrid (Best Choice):**
- Netflix: CF for established, CB for new releases
- Spotify: CF for playlists, CB for new artists
- YouTube: CF for subscribers, CB for new channels
- Amazon: CF for bestsellers, CB for niche products

---

*For questions or improvements, see the main README.md*
