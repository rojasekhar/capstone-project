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

6. Exploratory/Standardization Check
Applied z-score scaling to age and fare.
Before/after comparison confirmed mean ≈ 0 and std ≈ 1.

