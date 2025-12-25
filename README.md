# Ecommerce Product Recommendation System

This project focuses on building a **machine learning–driven recommendation system** for an e-commerce platform.  
The system analyzes historical user–product interactions (ratings) to suggest relevant products that a user is likely to prefer.

Instead of relying on a single technique, multiple recommendation strategies are implemented to handle different real-world scenarios such as:
- New users with no history
- Sparse interaction data
- Scalability for large datasets
- Personalized recommendations

The project is structured to demonstrate **progressive complexity**, starting from simple popularity-based recommendations and advancing toward latent factor models.

---

## Dataset Description

The dataset used in this project is an **Amazon Electronics product ratings dataset**, containing interactions between users and products in the form of ratings.

Key characteristics:
- The dataset does **not contain column headers**
- User and product identifiers are anonymized
- No personal or descriptive product information is used

To minimize bias and ensure fairness:
- Each user and product is represented using a **unique numeric identifier**
- No demographic or textual data is involved

Dataset source:
- https://www.kaggle.com/datasets/vibivij/amazon-electronics-rating-datasetrecommendation  
- Additional datasets: https://jmcauley.ucsd.edu/data/amazon/

---

## Recommendation Strategies

### 1) Rank-Based Product Recommendation

#### Conceptual Motivation

Rank-based recommendation is the **simplest form** of a recommender system.  
It does not rely on personalization and instead focuses on **overall product popularity**.

This approach is particularly useful when:
- A user is new to the platform
- No prior interaction data is available
- The system must immediately recommend something

This directly addresses the **Cold Start Problem**.

Cold start explanation:
[Cold Start Problem](ColdStartProblem.md)

---

#### Objective
- Identify products that are widely accepted by users
- Recommend items with strong collective feedback
- Provide default recommendations for new users

---

#### Methodology

1. Aggregate all ratings for each product
2. Compute:
   - Total number of ratings per product
   - Average rating per product
3. Filter products that do not meet a minimum interaction threshold
4. Sort remaining products based on average rating
5. Return the top *N* ranked products

This ensures that:
- Products with very few ratings do not dominate the recommendations
- Recommendations are statistically meaningful

---

#### Output
- Top 5 products with at least 50 or 100 user interactions

---

### 2) Similarity-Based Collaborative Filtering (User-Based)

#### Conceptual Motivation

Unlike rank-based methods, collaborative filtering focuses on **personalization**.  
The assumption behind this approach is:

> Users with similar past behavior are likely to prefer similar products in the future.

This method does **not require product metadata** and learns patterns purely from user interactions.

---

#### Objective
- Recommend products based on preferences of similar users
- Capture behavioral similarity between users

---

#### Methodology

1. Convert user identifiers into integer indices to simplify matrix operations
2. Create a **user–item interaction matrix**, where:
   - Rows represent users
   - Columns represent products
   - Values represent ratings
3. Compute **cosine similarity** between users
4. For a target user:
   - Identify the most similar users
   - Exclude the target user from the similarity list
5. Collect products interacted with by similar users
6. Remove products already interacted with by the target user
7. Rank remaining products and return top recommendations

---

#### Output
- Top 5 personalized product recommendations per user

---

### 3) Model-Based Collaborative Filtering

#### Conceptual Motivation

User-based collaborative filtering becomes inefficient when:
- The dataset grows large
- The interaction matrix becomes sparse

To overcome these limitations, **model-based collaborative filtering** is used.  
This approach learns **latent factors** that represent hidden relationships between users and products.

---

#### Objective
- Improve scalability and performance
- Capture underlying user–product preference patterns
- Handle sparse data efficiently

---

#### Methodology

1. Convert the user–item rating matrix into a **Compressed Sparse Row (CSR)** matrix to reduce memory usage
2. Apply **Singular Value Decomposition (SVD)** to factorize the matrix into:
   - User latent features
   - Product latent features
3. Reduce dimensionality to a fixed number of latent factors (e.g., 50)
4. Reconstruct the predicted rating matrix using the decomposed components
5. For a given user:
   - Compare predicted ratings with actual ratings
   - Filter out already-rated products
   - Rank remaining products by predicted rating
6. Recommend the top *N* products

---

#### Model Evaluation

To assess performance:
1. Compute average actual ratings
2. Compute average predicted ratings
3. Calculate **Root Mean Squared Error (RMSE)**

RMSE provides an interpretable measure of prediction accuracy, as it remains in the same scale as the ratings.

> The `mean_squared_error` function is configured with `squared=False` to directly return RMSE instead of MSE.

---

## ⚠️ Disclaimer

> ⚠️ This project is developed strictly for **learning and academic purposes**.  
> It is intended to demonstrate the working principles of recommendation systems and is **not production-ready**.

---

