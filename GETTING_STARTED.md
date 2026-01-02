# 🚀 Getting Started

Quick guide to run the Movie Recommendation Systems project locally.

---

## Prerequisites

- **Python 3.8+** (check with `python --version`)
- **pip** (Python package manager, usually comes with Python)
- **Git** (to clone the repository)
- **About 500MB free disk space** (for datasets and dependencies)

---

## Installation (5 minutes)

### Step 1: Clone the Repository

```bash
# Clone the repo
git clone https://github.com/VictimPickle/movie-recommendation-systems.git

# Navigate to the folder
cd movie-recommendation-systems
```

### Step 2: Create Virtual Environment (Recommended)

**Windows:**
```bash
python -m venv venv
venv\Scripts\activate
```

**macOS/Linux:**
```bash
python3 -m venv venv
source venv/bin/activate
```

### Step 3: Install Dependencies

```bash
# Install all required packages
pip install -r requirements.txt
```

This installs:
- pandas (data processing)
- numpy (numerical computing)
- matplotlib (visualization)
- jupyter (notebook environment)
- scipy (scientific computing)

### Step 4: Launch Jupyter

```bash
jupyter notebook
```

This opens your browser at `http://localhost:8888`

---

## Running the Notebook

### Option A: Run Everything (5-10 minutes)

1. Open `Project.ipynb` in Jupyter
2. Click **Kernel → Restart & Run All**
3. Wait for execution to complete
4. View all outputs and visualizations

### Option B: Run Cells Sequentially

1. Open `Project.ipynb`
2. Start with **Phase 1** cells
3. Run cells one-by-one with **Shift + Enter**
4. Read explanations between cells
5. Analyze outputs as you go

### Option C: Run Specific Phases

**Phase 1: Data Understanding** (2 min)
```
Load data, explore structure, check quality
```

**Phase 2: Content-Based System** (2 min)
```
Create user profile, calculate recommendations
```

**Phase 3: Collaborative Filtering** (1 min)
```
Find similar users, predict ratings
```

**Phase 4: Comparison & Evaluation** (1 min)
```
Compare both systems, analyze differences
```

**Phase 5: Documentation & Insights** (1 min)
```
Review findings and conclusions
```

---

## Customization

### Change User Input

Find this cell in Phase 2:

```python
user_input = [
    {'title': 'Dark Knight, The', 'rating': 5.0},
    {'title': 'Inception', 'rating': 4.5},
    {'title': 'Toy Story', 'rating': 3.0},
    {'title': 'Matrix, The', 'rating': 5.0},
    {'title': 'Shawshank Redemption, The', 'rating': 5.0}
]
```

**Modify to:**
- Change movie titles (must match dataset)
- Change ratings (0.5 - 5.0)
- Add/remove movies
- Re-run all following cells

**Common movie titles:**
- 'Avatar (2009)'
- 'Forrest Gump (1994)'
- 'The Dark Knight (2008)'
- 'Pulp Fiction (1994)'
- 'Fight Club (1999)'
- 'Inception (2010)'
- 'The Matrix (1999)'
- 'Titanic (1997)'
- 'Saving Private Ryan (1998)'

### Adjust Parameters

**Content-Based Filtering:**
```python
# Minimum ratings for quality filter
MIN_RATINGS = 20  # Change to 10 or 30

# Minimum average rating
MIN_AVG_RATING = 3.5  # Change to 3.0 or 4.0
```

**Collaborative Filtering:**
```python
# Minimum common movies
MIN_COMMON_MOVIES = 3  # Change to 2 or 5

# Number of similar users to compare
TOP_K = 20  # Change to 10 or 50

# Minimum raters for a recommendation
MIN_RATERS = 3  # Change to 2 or 5
```

---

## Understanding Outputs

### Recommendation Table

```
title | genres | year | score/rating
------|--------|------|-------------
Movie A | Action, Drama | 2009 | 0.765
Movie B | Action, Sci-Fi | 1995 | 0.732
```

**Score Meaning:**
- Content-Based: 0.0 - 1.0 (normalized similarity)
- CF: 0.0 - 5.0 (predicted rating stars)

### Similarity Analysis

```
Pearson Correlation: -1.0 to +1.0

+1.0: Identical taste (perfect match)
 0.0: No correlation (unrelated taste)
-1.0: Opposite taste (disagree on everything)
```

### Genre Distribution

Lists all genres in top recommendations:
- Content-Based: Usually 5-10 genres (narrow)
- Collaborative Filtering: Usually 10-18 genres (broad)

---

## Troubleshooting

### Issue: "No module named 'pandas'"

**Solution:**
```bash
pip install pandas numpy matplotlib jupyter
```

### Issue: "Movie title not found in dataset"

**Cause:** Title doesn't match exactly in CSV

**Solution:**
1. Check exact title in `movies.csv`
2. Use exact capitalization
3. Include year in parentheses
4. Example: `'Avatar (2009)'` not `'avatar'`

### Issue: Jupyter won't open in browser

**Solution:**
```bash
# Manually copy the token URL shown in terminal
# Example: http://localhost:8888/?token=abc123...
# Paste into browser address bar
```

### Issue: Notebook runs very slowly

**Cause:** Computing similarities for all users

**Solution:**
```python
# In Phase 3, reduce number of users
MAX_USERS_TO_COMPARE = 50  # Instead of 100
TOP_K = 10  # Instead of 20
```

### Issue: Out of Memory error

**Solution:**
```bash
# Restart Jupyter
# Kernel → Restart & Clear Output
# Then run specific phases only
```

---

## Next Steps

### After Running Once:

1. **Experiment** with different user inputs
2. **Adjust parameters** to see impact
3. **Read DOCUMENTATION.md** for technical details
4. **Compare results** between systems
5. **Implement improvements** from the suggestions

### Advanced Learning:

1. **Modify algorithms:**
   - Try cosine similarity instead of Pearson
   - Use TF-IDF instead of binary genre encoding
   - Implement matrix factorization

2. **Add features:**
   - Include director/actor information
   - Add release year weighting
   - Implement temporal dynamics

3. **Evaluate deeply:**
   - Implement precision/recall metrics
   - Create train/test split
   - Validate on held-out users

4. **Visualize more:**
   - Create user similarity networks
   - Plot recommendation confidence distributions
   - Show genre evolution over time

---

## Resources

**Learning Materials:**
- [Recommendation Systems - Andrew Ng](https://www.coursera.org/learn/machine-learning)
- [MovieLens Documentation](https://grouplens.org/datasets/movielens/)
- [Pandas Tutorial](https://pandas.pydata.org/docs/getting_started/)
- [NumPy Guide](https://numpy.org/doc/stable/user/guide.html)

**Code References:**
- See `DOCUMENTATION.md` for detailed explanations
- See comments in `Project.ipynb` for inline help
- Check README.md for methodology overview

---

## Getting Help

**If something doesn't work:**

1. Check error message carefully
2. Google the error message
3. Check Troubleshooting section above
4. Review the cell explanations
5. Post on Stack Overflow with full error trace

---

## Fun Experiments to Try

### Experiment 1: Filter Bubble
```python
# Rate only Action movies
user_input = [
    {'title': 'The Matrix (1999)', 'rating': 5.0},
    {'title': 'Inception (2010)', 'rating': 5.0},
    {'title': 'The Dark Knight (2008)', 'rating': 5.0},
    {'title': 'Avatar (2009)', 'rating': 5.0},
    {'title': 'Terminator 2 (1991)', 'rating': 5.0}
]
# Result: Only action movies recommended!
```

### Experiment 2: Diverse Taste
```python
# Rate diverse genres
user_input = [
    {'title': 'Forrest Gump (1994)', 'rating': 5.0},  # Drama
    {'title': 'The Princess Bride (1987)', 'rating': 5.0},  # Fantasy
    {'title': 'Dumb and Dumber (1994)', 'rating': 5.0},  # Comedy
    {'title': 'Goodfellas (1990)', 'rating': 5.0},  # Crime
    {'title': 'Schindler's List (1993)', 'rating': 5.0}  # Historical
]
# Result: Much more diverse recommendations!
```

### Experiment 3: Low Ratings
```python
# Rate movies low
user_input = [
    {'title': 'The Matrix (1999)', 'rating': 1.0},  # Disliked
    {'title': 'Inception (2010)', 'rating': 1.5},  # Disliked
    # ... etc
]
# Result: Recommendations avoid those genres
```

---

## You're Ready! 🎉

Now open Jupyter and run `Project.ipynb`:

```bash
jupyter notebook
```

Happy exploring! 🎬
