Project README — Zepto Analytics & Data Pipeline
This repository contains three modules:

Module 1 — Data Pipeline (/data_pipeline)

Module 2 — Analytics Pipeline (/analytics)

Module 3 — Support Assistant (/support_assistant)

Module 1 — Data Pipeline (/data_pipeline):

Purpose: Implements a raw-to-relational pipeline for catalog-style product data. Scrapes, cleans, enriches, and loads into a normalized SQLite database, then queries with SQL and pandas.

Steps
Scraping:

Used requests + BeautifulSoup to scrape ≥ 60 books across ≥ 3 categories.

Captured: title, price (GBP), star_rating (text), availability, category.

Cleaning:

Converted price → float (price_gbp).

Converted star_rating → integer (1–5).

Parsed availability → boolean (in_stock).

Applied median imputation or row drop for parsing errors.

Added price_inr using fixed baseline conversion: 1 GBP = 105.50 INR.

Database schema:

Two-table normalized schema:

categories(category_id PK, category_name UNIQUE)

books(book_id PK, title, price_gbp, price_inr, rating, in_stock, category_id FK)

Queries:

≥ 5 queries demonstrating: SELECT/WHERE, ORDER BY, LIMIT, DISTINCT, IN/BETWEEN, and JOIN.

Example: Top 10 highest-rated books per category.

Verification:

Read query results via pd.read_sql.

Reproduced JOIN results via pd.merge to confirm equivalence.

Deliverables:

End-to-end scraping + cleaning .

SQLite database file or recreation script.

Query outputs logged.

Git workflow: feature branch created, committed, merged back to main.

Module 2 — Analytics Pipeline (/analytics):

Purpose: Analyst-to-data-scientist workflow: profile Titanic dataset, clean it, tell a clear visual story, and build predictive models.

Steps
Part A — EDA & Data Story

Dataset load:

Loaded once via sns.load_dataset('titanic').

Saved offline fallback: titanic.csv.

Profiling:

Reported df.info(), df.describe(), df.shape.

Calculated missing-value percentages per column.

Missing-value handling:

<5% → drop rows.

5–30% → impute.

30% → drop column or encode “missing” category (with justification).

Univariate analysis:

Histograms + box plots for age and fare.

Outlier counts via IQR rule.

Mean/median/mode for fare → skewness conclusion.

Bivariate analysis:

Survival rates by sex, pclass, and sex+pclass.

Correlation matrix (survived, pclass, age, sibsp, parch, fare).

Heatmap + interpretation of top 2 strongest correlations.

Multivariate story:

≥ 4 charts (bar/box/scatter/heatmap/pairplot).

Each with 2–4 sentence interpretation.

Standardization check:

Applied z-score to age and fare.

Verified mean ≈ 0, std ≈ 1.

Part B — Predictive Modeling

Train/test split:

Stratified split justified by class imbalance.

Preprocessing:

Imputation, encoding (sex, embarked), scaling.

Fit only on train, transform-only on test.

Implemented via ColumnTransformer + Pipeline.

Models:

Logistic Regression, Decision Tree (visualized with plot_tree), Random Forest.

Evaluation:

Confusion matrix, accuracy, precision, recall, F1, ROC/AUC.

Comparison table across classifiers.

Imbalance handling:

Baseline vs. class_weight='balanced' vs. SMOTE (train-only).

Compared metrics, concluded best strategy.

Hyperparameter tuning:

GridSearchCV over n_estimators, max_depth, max_features.

Reported best params + OOB score.

Regression side-task:

Predicted fare via linear regression.

Reported MAE, RMSE, R², Adjusted R².

Residual plot → heteroscedasticity conclusion.

Final comparison:

Classification metrics table (accuracy, precision, recall, F1, AUC).

Regression metrics table (MAE, RMSE, R², Adjusted R²).

3–5 sentence recommendation of best classifier.

Pipeline saving:

Saved complete pipeline (preprocessing + estimator) via joblib.dump.

Reloaded with joblib.load to confirm predictions on raw input.

Deliverables:

Committed titanic.csv offline fallback.

≥ 4 multivariate charts with interpretations.

Full metric suite for classifiers + regression.

Saved joblib pipeline.