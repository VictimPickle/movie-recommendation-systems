# 🎬 Movie Recommendation Systems

> A comprehensive implementation of **Content-Based** and **Collaborative Filtering** recommendation algorithms using the MovieLens dataset

[![Python 3.8+](https://img.shields.io/badge/Python-3.8%2B-blue)](https://www.python.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-orange)](https://jupyter.org/)

---

## 📋 Table of Contents

- [Overview](#overview)
- [Project Structure](#project-structure)
- [Datasets](#datasets)
- [Features](#features)
- [Installation](#installation)
- [Usage](#usage)
- [Methodology](#methodology)
- [Results](#results)
- [Key Insights](#key-insights)
- [Improvements & Future Work](#improvements--future-work)
- [Contributing](#contributing)
- [Author](#author)

---

## 🎯 Overview

This project implements two fundamental recommendation system algorithms and compares their effectiveness:

### **Content-Based Filtering**
- Recommends movies based on **genre similarity**
- Analyzes movie features and user preferences
- Explains recommendations transparently
- Works well for new items (no cold-start for movies)

### **Collaborative Filtering**
- Recommends movies based on **user behavior patterns**
- Finds similar users and their preferences
- Discovers non-obvious connections
- Leverages "wisdom of the crowd"

---

## 📁 Project Structure

```
movie-recommendation-systems/
├── README.md                          # This file
├── DOCUMENTATION.md                   # Detailed technical documentation
├── Project.ipynb                      # Main Jupyter notebook with full implementation
├── data/
│   ├── movies.csv                    # Movie database (9,742 movies with genres)
│   └── ratings.csv                   # User ratings (100,836 ratings from 610 users)
├── results/                          # Analysis outputs and visualizations
│   ├── content_based_recommendations.csv
│   ├── collaborative_filtering_recommendations.csv
│   └── comparison_analysis.txt
└── images/                           # Visualizations and charts
```

---

## 📊 Datasets

### Movies Dataset (`movies.csv`)
- **Records:** 9,742 movies
- **Columns:** `movieId`, `title`, `genres`
- **Year Range:** 1902 - 2018
- **Genre Count:** 20 unique genres
- **Sample:**
  ```
  movieId,title,genres
  1,Toy Story (1995),Adventure|Animation|Children|Comedy|Fantasy
  2,Jumanji (1995),Adventure|Children|Fantasy
  ```

### Ratings Dataset (`ratings.csv`)
- **Records:** 100,836 ratings
- **Columns:** `userId`, `movieId`, `rating`, `timestamp`
- **Users:** 610 unique users
- **Rating Scale:** 0.5 - 5.0 (half-star increments)
- **Sparsity:** 98.31% (very sparse matrix!)
- **Sample:**
  ```
  userId,movieId,rating,timestamp
  1,1,4.0,964982703
  1,3,4.0,964981247
  ```

---

## ✨ Features

### Data Processing & Analysis
- ✅ Data cleaning and validation
- ✅ Missing value handling
- ✅ Genre extraction and one-hot encoding
- ✅ Rating distribution analysis
- ✅ Data sparsity metrics
- ✅ Year extraction and normalization

### Content-Based System
- ✅ Genre-based feature vectors
- ✅ Weighted user preference profiles
- ✅ Cosine similarity calculation
- ✅ Top-N recommendation generation
- ✅ Quality filtering (minimum ratings & rating threshold)
- ✅ Genre diversity analysis

### Collaborative Filtering System
- ✅ User-Item matrix construction
- ✅ Similar user identification
- ✅ Pearson correlation calculation
- ✅ Weighted rating predictions
- ✅ Recommendation ranking
- ✅ Novelty vs. popularity analysis

### Comparative Analysis
- ✅ Recommendation overlap detection
- ✅ Genre diversity comparison
- ✅ Popularity bias measurement
- ✅ System strengths/weaknesses evaluation
- ✅ Radar chart visualization
- ✅ Side-by-side summary table

---

## 🚀 Installation

### Prerequisites
- Python 3.8+
- Jupyter Notebook or JupyterLab

### Setup

1. **Clone the repository**
   ```bash
   git clone https://github.com/VictimPickle/movie-recommendation-systems.git
   cd movie-recommendation-systems
   ```

2. **Create virtual environment** (optional but recommended)
   ```bash
   python -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   ```

3. **Install dependencies**
   ```bash
   pip install pandas numpy matplotlib scipy jupyter
   ```

4. **Launch Jupyter Notebook**
   ```bash
   jupyter notebook Project.ipynb
   ```

---

## 📖 Usage

### Running the Complete Pipeline

The `Project.ipynb` notebook contains the complete implementation organized in 5 phases:

#### **Phase 1: Data Understanding & Preprocessing**
```python
# Load datasets
movies = pd.read_csv("data/movies.csv")
ratings = pd.read_csv("data/ratings.csv")

# Analyze structure and quality
print(f"Movies: {len(movies)}, Users: {ratings['userId'].nunique()}")
print(f"Sparsity: {sparsity:.2f}%")
```

#### **Phase 2: Content-Based Filtering**
```python
# Create user profile based on rated movies
user_input = [
    {'title': 'Dark Knight, The', 'rating': 5.0},
    {'title': 'Inception', 'rating': 4.5},
    {'title': 'Matrix, The', 'rating': 5.0}
]

# Get recommendations
recommendations = get_content_based_recommendations(user_input)
```

#### **Phase 3: Collaborative Filtering**
```python
# Find similar users
similar_users = find_similar_users(user_id, min_common_movies=3)

# Get weighted recommendations from similar users
cf_recommendations = get_cf_recommendations(similar_users)
```

#### **Phase 4: Evaluation & Comparison**
```python
# Compare both systems
compare_systems(content_based, collaborative_filtering)
# Output: overlap analysis, diversity metrics, quality comparison
```

#### **Phase 5: Documentation & Insights**
- Technical lessons learned
- Real-world applications
- Possible improvements
- Conclusions

---

## 🔬 Methodology

### Content-Based Filtering Algorithm

```
1. Create genre vectors for all movies
   movie_vector = [Action: 1, Drama: 1, Comedy: 0, ...]

2. Build user profile from rated movies
   user_profile = Σ(genre_vector × rating) for all rated movies

3. Calculate similarity scores
   score(movie) = dot_product(movie_vector, user_profile) / total_preference_weight

4. Rank and filter recommendations
   - Remove already-watched movies
   - Filter by minimum ratings and average rating
   - Return top-N movies
```

### Collaborative Filtering Algorithm

```
1. Build user-item matrix
   Matrix[user, movie] = rating (or NaN if not rated)

2. Find similar users using Pearson Correlation
   r = Σ(x_i - x̄)(y_i - ȳ) / √[Σ(x_i - x̄)²][Σ(y_i - ȳ)²]
   
   Where x = your ratings, y = other user's ratings

3. Predict ratings for unseen movies
   predicted_rating = Σ(similarity × rating) / Σ(similarity)

4. Rank and filter recommendations
   - Remove already-watched movies
   - Filter by minimum number of raters
   - Return top-N predicted highest-rated movies
```

---

## 📈 Results

### Content-Based Recommendations (Sample)

| Title | Genres | Year | Score |
|-------|--------|------|-------|
| Patlabor: The Movie | Action, Drama, Sci-Fi, Thriller | 1989 | 0.765 |
| Strange Days | Action, Crime, Drama, Sci-Fi | 1995 | 0.732 |
| Watchmen | Action, Drama, Mystery, Sci-Fi | 2009 | 0.678 |

### Collaborative Filtering Recommendations (Sample)

| Title | Predicted Rating | Similar Users |
|-------|------------------|----------------|
| Movie A | 4.2 | 5 |
| Movie B | 4.0 | 4 |
| Movie C | 3.9 | 3 |

### Key Metrics

- **Matrix Sparsity:** 98.31% (very sparse)
- **Avg Ratings/User:** 165
- **Avg Ratings/Movie:** 10.4
- **Unique Genres:** 20
- **Date Range:** 1902 - 2018

---

## 💡 Key Insights

### Content-Based Insights

1. **Filter Bubble Effect**
   - System creates "bubbles" around favorite genres
   - Limits discovery of diverse content
   - Example: Only Action/Drama recommendations despite rating diverse movies

2. **Genre Combination Bias**
   - Movies with more genres score higher (more matching opportunities)
   - Obscure movies can rank highly if genres match
   - Quality is NOT considered in scoring

3. **Rating Weight Impact**
   - Lower-rated movies have minimal impact on profile
   - Example: Toy Story (3.0 rating) has minimal Comedy/Animation weight
   - Result: No Comedy/Animation recommendations

### Collaborative Filtering Insights

1. **Wisdom of the Crowd**
   - Breaks filter bubbles by leveraging user patterns
   - Can recommend unexpected but quality movies
   - Considers actual user ratings, not just features

2. **Data Sparsity Challenge**
   - 98.31% of matrix is empty (users rate few movies)
   - Limits similar user finding
   - Cold-start problem for new users

3. **Popularity Bias**
   - System tends to recommend popular movies
   - Makes sense (many users rated them = quality signal)
   - Can miss niche but high-quality recommendations

---

## 🔧 Improvements & Future Work

### Short-term Improvements

**Content-Based:**
- Use TF-IDF weighting instead of binary genre encoding
- Include additional features: directors, actors, keywords
- Add temporal factors (movie age, release trends)

**Collaborative Filtering:**
- Implement matrix factorization (SVD, NMF)
- Try cosine similarity alongside Pearson correlation
- Use implicit feedback (views, time spent watching)

### Long-term Enhancements

**Hybrid Approach:**
```python
final_score = 0.6 * cf_score + 0.4 * cb_score
```

**Advanced Techniques:**
- Deep learning models (autoencoders, neural networks)
- Context-aware recommendations (time, device, social)
- Real-time learning from user feedback
- A/B testing framework for algorithm evaluation

---

## 🤝 Contributing

Contributions are welcome! Here's how:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/improvement`)
3. Commit changes (`git commit -m 'Add improvement'`)
4. Push to branch (`git push origin feature/improvement`)
5. Open a Pull Request

---

## 📚 Resources

- [Recommendation Systems - Stanford](https://web.stanford.edu/class/cs224w/)
- [Collaborative Filtering Guide](https://developers.google.com/machine-learning/recommendation/collaborative/basics)
- [MovieLens Dataset](https://grouplens.org/datasets/movielens/)
- [Content-Based Filtering Tutorial](https://developers.google.com/machine-learning/recommendation/content-based/basics)

---

## 📝 License

This project is licensed under the MIT License - see [LICENSE](LICENSE) file for details.

---

## 👤 Author

**Mobin Ghorbani**
- 📍 Location: Tehran, Iran
- 🎓 CS Student at University of Tehran
- 🔗 GitHub: [@VictimPickle](https://github.com/VictimPickle)
- 📧 Email: mobinghorbanihokmabad@gmail.com

---

## 🎓 Academic Context

This project was developed as part of **ML (Machine Learning)** course exploring recommendation systems.

Key learning outcomes:
- ✅ Understanding collaborative vs. content-based filtering
- ✅ Matrix operations for similarity calculations
- ✅ Handling sparse data
- ✅ Comparative algorithm evaluation
- ✅ Data preprocessing and feature engineering

---

## 🙏 Acknowledgments

- MovieLens dataset provided by [GroupLens Research](https://grouplens.org/)
- Inspiration from Netflix, Spotify, and YouTube recommendation systems
- Educational resources from Andrew Ng's ML course

---

## ⭐ Show Your Support

If this project helped you, please consider:
- ⭐ Starring the repository
- 🐛 Reporting issues
- 💡 Suggesting improvements
- 📤 Sharing with others

---

<div align="center">

**Made with ❤️ by a CS Student**

*Exploring the art and science of recommendations* 🎬

</div>
