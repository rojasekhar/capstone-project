Analytics:  Profiling, cleaning, and the data story

Workflow steps:

1. Profiling:
Dataset load using sns.load_dataset('titanic') and then read to csv for offline grading.
Profiling the data using df.info(), df.describe() and df.shape
Reported missing value percentages for each column.

2. Missing Value Handling:
Rule:  
  - <5% missing → drop rows  
  - 5–30% missing → impute (median for age)  
  - >30% missing → encode “Missing” category (deck)  
Exact percentages reported before applying strategy.

3. Univariate Analysis:
Histograms and boxplots for age and fare.
Outliers detected via IQR rule:
  - Age: ~32 passengers older than 58
  - Fare: ~120 passengers with fares above 60
Fare distribution is **right-skewed** (mean > median > mode).

4. Bivariate Analysis:
Survival rates broken down by sex, pclass, and sex & pclass together.
Correlation matrix restricted to 6 numeric columns:
  - survived, pclass, age, sibsp, parch, fare
Heatmap shown with `sns.heatmap`.
Strongest correlations:
  - pclass vs fare (negative)
  - sibsp vs parch (positive)

5. Multivariate Data Story:
At least 4 distinct charts (bar, box, scatter, heatmap, pairplot).

(i) Survival Rate by Class — Bar Chart
Interpretation: First‑class passengers had the highest survival rate, while second and third‑class passengers had the lowest. Wealth and cabin location likely gave them better access to lifeboats

(ii) Age vs Survival — Box Plot
Interpretation: Survivors seems to be younger, with many children among them. Older passengers were less likely to survive, suggesting age based evacuation priority.

(iii) Correlation Heatmap — Heatmap
Interpretation: The strongest negative correlation is between pclass and fare, showing that higher classes corresponded to higher ticket prices. A positive correlation between siblings/spouses (sibsp) and parents/children (parch) indicates families often traveled together. 

(iv) Fare vs Age by Survival — Scatter Plot
Interpretation: Survivors cluster among younger passengers with higher fares. This suggests survival was influenced by both wealth and age, with children and wealthier passengers having better chances. 

(v) Pair Plot of Key Features — Pair Plot
Interpretation: The pair plot shows survival patterns across multiple variables simultaneously. Clear separation appears by sex, class, and fare, with survivors concentrated among women, first‑class, and higher‑fare passengers. 

6. Exploratory/Standardization Check:
Applied z-score scaling to age and fare.
Before/after comparison confirmed mean ≈ 0 and std ≈ 1.

7. Preprocessing - train/test split with stratification:
split the data into train and test sets using a stratified split on the survived target. Stratification was necessary because survival is imbalanced (about 62% died vs 38% survived). Without stratification, the train/test sets could have skewed class distributions, leading to biased training and misleading evaluation. Stratification guarantees that both sets reflect the same class balance as the full dataset.

8. preprocessing fit only on the training data:
Implemented preprocessing with a ColumnTransformer inside a scikit‑learn Pipeline. Numeric features were imputed with the median and scaled with StandardScaler. Categorical features were imputed with the most frequent value and one‑hot encoded. All preprocessing steps were fit only on the training data and then applied in transform‑only mode to the test data, ensuring no leakage of test‑set information into training.

9. Train three classifiers on the same stratified train/test split:
Trained three classifiers — Logistic Regression, Decision Tree, and Random Forest — on the same stratified train/test split. Logistic Regression provided a linear baseline, the Decision Tree revealed interpretable rules (visualized with plot_tree), and the Random Forest improved accuracy through ensemble averaging. All models used the same preprocessing pipeline, fit only on the training data, ensuring no leakage of test‑set information.

10. Evaluate all three models with: a confusion matrix, accuracy, precision, recall, F1 score, and an ROC curve with AUC:
Evaluated three classifiers using confusion matrices, accuracy, precision, recall, F1 score, and ROC‑AUC. Logistic Regression provided a strong baseline, the Decision Tree offered interpretability at modest accuracy, and the Random Forest achieved the best overall performance. ROC curves confirmed that the Random Forest had the strongest ability to discriminate survivors from non‑survivors.

11. Imbalance handling comparison:
Trained Logistic Regression model and compared precision/recall/F1 across baseline, class weight and SMOTE variants.

12. Hyperparameter tuning:
Tuned the Random Forest over n_estimators, max_depth, and max_features using GridSearchCV. The best parameter set was reported, and refit the model with oob_score=True to obtain the out‑of‑bag score. The OOB score closely matched the test accuracy, confirming that the tuned Random Forest generalizes well without overfitting

13. Regression side-task:
Trained a multivariate linear regression to predict fare. The model achieved MAE ≈ 20, RMSE ≈ 30, R² ≈ 0.65, and Adjusted R² ≈ 0.63. The residual plot showed widening variance at higher predicted fares, indicating heteroscedasticity. This suggests linear regression captures general fare patterns but struggles with the extreme variability of high‑class fares.

14. Model comparison table:
Compared all the models with Accuracy, precision, Recall, F1 ROC-AUC scores. Among the classifiers, the Random Forest stands out as the best model.

15. complete pipeline:
Saved the complete fitted pipeline (preprocessing + Random Forest classifier) using joblib.dump. Reloading with joblib.load confirmed that the artifact works end‑to‑end on raw Titanic data, producing survival predictions without requiring manual preprocessing. This guarantees reproducibility and deployability of the model.