# 🤖 Basics of Machine Learning — Cheatsheet

> A comprehensive reference covering ML fundamentals, algorithms, evaluation, and workflows.

---

## Table of Contents

1. [What is Machine Learning?](#what-is-machine-learning)
2. [Types of ML](#types-of-machine-learning)
3. [ML Workflow](#ml-workflow)
4. [Data Preprocessing](#data-preprocessing)
5. [Feature Engineering](#feature-engineering)
6. [Supervised Learning Algorithms](#supervised-learning-algorithms)
7. [Unsupervised Learning Algorithms](#unsupervised-learning-algorithms)
8. [Model Evaluation Metrics](#model-evaluation-metrics)
9. [Bias-Variance Tradeoff](#bias-variance-tradeoff)
10. [Regularization](#regularization)
11. [Hyperparameter Tuning](#hyperparameter-tuning)
12. [Ensemble Methods](#ensemble-methods)
13. [Neural Networks Basics](#neural-networks-basics)
14. [Common Libraries](#common-python-libraries)
15. [Key Formulas](#key-formulas)

---

## What is Machine Learning?

Machine Learning is a subset of Artificial Intelligence where systems **learn patterns from data** to make predictions or decisions **without being explicitly programmed**.

```
Traditional Programming:  Data + Rules     → Answers
Machine Learning:         Data + Answers   → Rules (Model)
```

### AI vs ML vs DL

```
┌─────────────────────────────────────────┐
│  Artificial Intelligence (AI)           │
│  ┌───────────────────────────────────┐  │
│  │  Machine Learning (ML)            │  │
│  │  ┌─────────────────────────────┐  │  │
│  │  │  Deep Learning (DL)         │  │  │
│  │  │  (Neural Networks)          │  │  │
│  │  └─────────────────────────────┘  │  │
│  └───────────────────────────────────┘  │
└─────────────────────────────────────────┘
```

---

## Types of Machine Learning

### 1. Supervised Learning

Learns from **labeled data** (input → known output).

| Task               | Output Type        | Examples                          |
|--------------------|--------------------|-----------------------------------|
| **Classification** | Discrete (category)| Spam detection, image recognition |
| **Regression**     | Continuous (number)| House price prediction, forecasting|

### 2. Unsupervised Learning

Discovers patterns in **unlabeled data**.

| Task                  | Purpose                        | Examples                     |
|-----------------------|--------------------------------|------------------------------|
| **Clustering**        | Group similar data             | Customer segmentation        |
| **Dimensionality Reduction** | Reduce features         | PCA, t-SNE                   |
| **Association**       | Find rules/relationships       | Market basket analysis       |
| **Anomaly Detection** | Find outliers                  | Fraud detection              |

### 3. Semi-Supervised Learning

Uses a **small amount of labeled** + **large amount of unlabeled** data.

### 4. Reinforcement Learning

Agent learns by **interacting with an environment** and receiving rewards/penalties.

- **Agent** takes **actions** in an **environment**
- Receives **reward/penalty** signals
- Goal: maximize cumulative reward
- Examples: Game AI, robotics, autonomous driving

### 5. Self-Supervised Learning

Model generates its own labels from the data structure (e.g., predicting masked words in BERT).

---

## ML Workflow

```
1. Define Problem → 2. Collect Data → 3. Explore (EDA) → 4. Preprocess
→ 5. Feature Engineering → 6. Split Data → 7. Select Model
→ 8. Train → 9. Evaluate → 10. Tune Hyperparameters
→ 11. Final Evaluation → 12. Deploy → 13. Monitor
```

### Data Splitting

```python
from sklearn.model_selection import train_test_split

X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42, stratify=y
)
```

| Split         | Typical %  | Purpose                          |
|---------------|------------|----------------------------------|
| Training      | 60-80%     | Learn patterns                   |
| Validation    | 10-20%     | Tune hyperparameters             |
| Test          | 10-20%     | Final unbiased evaluation        |

---

## Data Preprocessing

### Handling Missing Values

```python
import pandas as pd
from sklearn.impute import SimpleImputer

df.isnull().sum()                          # Count missing per column
df.dropna()                                 # Drop rows with NaN
df.fillna(df.mean())                        # Fill with mean

imputer = SimpleImputer(strategy='median')  # mean, median, most_frequent
X = imputer.fit_transform(X)
```

### Feature Scaling

| Method              | Formula                            | When to Use                    |
|---------------------|------------------------------------|--------------------------------|
| **Standardization** | `(x - μ) / σ`                     | Normal distribution, SVM, NN   |
| **Min-Max Scaling** | `(x - min) / (max - min)`         | Bounded range [0, 1]           |
| **Robust Scaling**  | `(x - median) / IQR`              | Data with outliers             |

```python
from sklearn.preprocessing import StandardScaler, MinMaxScaler

scaler = StandardScaler()
X_scaled = scaler.fit_transform(X_train)
X_test_scaled = scaler.transform(X_test)   # Don't fit on test!
```

### Encoding Categorical Variables

```python
from sklearn.preprocessing import LabelEncoder, OneHotEncoder

# Label Encoding (ordinal: low < medium < high)
le = LabelEncoder()
df['size_encoded'] = le.fit_transform(df['size'])

# One-Hot Encoding (nominal: red, blue, green)
df_encoded = pd.get_dummies(df, columns=['color'], drop_first=True)
```

---

## Feature Engineering

### Common Techniques

| Technique              | Description                                      |
|------------------------|--------------------------------------------------|
| **Polynomial Features**| Create interaction & power terms (x₁², x₁·x₂)   |
| **Binning**            | Convert continuous → categorical (age groups)     |
| **Log Transform**      | Reduce skewness in right-skewed data              |
| **Date Decomposition** | Extract year, month, day, day-of-week from dates  |
| **Text Vectorization** | TF-IDF, Bag of Words, embeddings                  |
| **Target Encoding**    | Replace category with mean of target variable     |

### Feature Selection

```python
# Correlation analysis
df.corr()['target'].sort_values(ascending=False)

# Variance Threshold — remove low-variance features
from sklearn.feature_selection import VarianceThreshold
selector = VarianceThreshold(threshold=0.01)

# SelectKBest — statistical tests
from sklearn.feature_selection import SelectKBest, f_classif
selector = SelectKBest(f_classif, k=10)
X_selected = selector.fit_transform(X, y)
```

---

## Supervised Learning Algorithms

### Linear Regression

Predicts a continuous output using a linear relationship.

```
ŷ = w₁x₁ + w₂x₂ + ... + wₙxₙ + b
```

```python
from sklearn.linear_model import LinearRegression

model = LinearRegression()
model.fit(X_train, y_train)
predictions = model.predict(X_test)
model.coef_        # Weights
model.intercept_   # Bias
```

**Assumptions:** Linear relationship, independence, homoscedasticity, normality of residuals.

### Logistic Regression

Binary/multiclass **classification** using sigmoid function.

```
P(y=1|x) = 1 / (1 + e^(-z))    where z = wᵀx + b
```

```python
from sklearn.linear_model import LogisticRegression
model = LogisticRegression()
model.fit(X_train, y_train)
model.predict(X_test)          # Class labels
model.predict_proba(X_test)    # Probabilities
```

### Decision Trees

Tree-based model that splits data on feature thresholds.

```python
from sklearn.tree import DecisionTreeClassifier
model = DecisionTreeClassifier(max_depth=5, min_samples_split=10)
model.fit(X_train, y_train)
model.feature_importances_   # Feature importance scores
```

**Pros:** Interpretable, no scaling needed, handles non-linear data
**Cons:** Prone to overfitting, unstable (small data changes → different tree)

### K-Nearest Neighbors (KNN)

Classifies based on k closest training points.

```python
from sklearn.neighbors import KNeighborsClassifier
model = KNeighborsClassifier(n_neighbors=5, metric='euclidean')
model.fit(X_train, y_train)
```

**Key:** Choose k wisely (odd for binary), feature scaling is crucial.

### Support Vector Machines (SVM)

Finds the optimal hyperplane that maximizes margin between classes.

```python
from sklearn.svm import SVC
model = SVC(kernel='rbf', C=1.0, gamma='scale')
model.fit(X_train, y_train)
```

| Kernel   | Use Case                                 |
|----------|------------------------------------------|
| `linear` | Linearly separable data                  |
| `rbf`    | Non-linear data (default, most common)   |
| `poly`   | Polynomial decision boundary             |

### Naive Bayes

Probabilistic classifier based on Bayes' theorem with independence assumption.

```
P(class|features) ∝ P(features|class) × P(class)
```

```python
from sklearn.naive_bayes import GaussianNB, MultinomialNB
model = GaussianNB()             # Continuous features
model = MultinomialNB()          # Text/count data
```

### Algorithm Selection Guide

| Scenario                     | Try First                              |
|------------------------------|----------------------------------------|
| Small dataset, linear        | Logistic Regression, SVM (linear)      |
| Large dataset                | Random Forest, Gradient Boosting       |
| Text classification          | Naive Bayes, Logistic Regression       |
| Interpretability needed      | Decision Tree, Logistic Regression     |
| High-dimensional data        | SVM (rbf), Random Forest              |
| Regression                   | Linear Regression, Random Forest       |

---

## Unsupervised Learning Algorithms

### K-Means Clustering

Partitions data into k clusters by minimizing within-cluster variance.

```python
from sklearn.cluster import KMeans
kmeans = KMeans(n_clusters=3, random_state=42, n_init=10)
kmeans.fit(X)
labels = kmeans.labels_
centers = kmeans.cluster_centers_
```

**Choosing k:** Elbow method (plot inertia vs k), Silhouette score.

### Principal Component Analysis (PCA)

Reduces dimensionality while preserving maximum variance.

```python
from sklearn.decomposition import PCA
pca = PCA(n_components=2)           # or n_components=0.95 (95% variance)
X_reduced = pca.fit_transform(X)
pca.explained_variance_ratio_       # Variance explained per component
```

### Hierarchical Clustering

Builds a tree of clusters (dendrogram).

```python
from sklearn.cluster import AgglomerativeClustering
model = AgglomerativeClustering(n_clusters=3, linkage='ward')
labels = model.fit_predict(X)
```

### DBSCAN

Density-based clustering — finds clusters of varying shape, detects outliers.

```python
from sklearn.cluster import DBSCAN
model = DBSCAN(eps=0.5, min_samples=5)
labels = model.fit_predict(X)   # -1 = noise/outlier
```

---

## Model Evaluation Metrics

### Classification Metrics

```python
from sklearn.metrics import (
    accuracy_score, precision_score, recall_score,
    f1_score, confusion_matrix, classification_report,
    roc_auc_score
)

accuracy_score(y_true, y_pred)
precision_score(y_true, y_pred, average='weighted')
recall_score(y_true, y_pred, average='weighted')
f1_score(y_true, y_pred, average='weighted')
confusion_matrix(y_true, y_pred)
print(classification_report(y_true, y_pred))
roc_auc_score(y_true, y_prob)
```

| Metric        | Formula                        | Meaning                           |
|---------------|--------------------------------|-----------------------------------|
| **Accuracy**  | (TP+TN) / Total               | Overall correctness               |
| **Precision** | TP / (TP+FP)                   | Of predicted +, how many correct? |
| **Recall**    | TP / (TP+FN)                   | Of actual +, how many found?      |
| **F1 Score**  | 2·(P·R)/(P+R)                 | Harmonic mean of P and R          |
| **ROC-AUC**   | Area under ROC curve           | Discrimination ability            |

**Confusion Matrix:**

```
                  Predicted
              │  Pos  │  Neg  │
    ──────────┼───────┼───────┤
Actual  Pos   │  TP   │  FN   │
        Neg   │  FP   │  TN   │
```

### Regression Metrics

```python
from sklearn.metrics import mean_squared_error, mean_absolute_error, r2_score

mse  = mean_squared_error(y_true, y_pred)
rmse = mean_squared_error(y_true, y_pred, squared=False)
mae  = mean_absolute_error(y_true, y_pred)
r2   = r2_score(y_true, y_pred)
```

| Metric  | Meaning                                   | Best Value |
|---------|-------------------------------------------|------------|
| **MSE** | Mean of squared errors                    | 0          |
| **RMSE**| Root of MSE (same unit as y)              | 0          |
| **MAE** | Mean of absolute errors                   | 0          |
| **R²**  | Proportion of variance explained          | 1.0        |

### Cross-Validation

```python
from sklearn.model_selection import cross_val_score

scores = cross_val_score(model, X, y, cv=5, scoring='accuracy')
print(f"Mean: {scores.mean():.3f}, Std: {scores.std():.3f}")
```

---

## Bias-Variance Tradeoff

```
Total Error = Bias² + Variance + Irreducible Error
```

| Problem         | Bias   | Variance | Symptom                          | Fix                            |
|-----------------|--------|----------|----------------------------------|--------------------------------|
| **Underfitting**| High   | Low      | Poor on train AND test           | More features, complex model   |
| **Good Fit**    | Low    | Low      | Good on both                     | 🎯 Target                     |
| **Overfitting** | Low    | High     | Great on train, poor on test     | More data, regularization      |

```
    Error
    │
    │  \  Total Error
    │   \    ___________
    │    \ /            ── Variance
    │     X
    │    / \____________ ── Bias²
    │   /
    └──────────────────── Model Complexity
       Simple → Complex
```

---

## Regularization

Prevents overfitting by adding a penalty term to the loss function.

| Method      | Penalty          | Effect                              |
|-------------|------------------|-------------------------------------|
| **L1 (Lasso)** | `λ Σ|wᵢ|`    | Sparse weights (feature selection)  |
| **L2 (Ridge)** | `λ Σwᵢ²`     | Small weights (weight decay)        |
| **Elastic Net**| L1 + L2       | Combination of both                 |

```python
from sklearn.linear_model import Ridge, Lasso, ElasticNet

Ridge(alpha=1.0)        # L2
Lasso(alpha=1.0)        # L1
ElasticNet(alpha=1.0, l1_ratio=0.5)
```

### Other Regularization Techniques

- **Dropout** (neural networks): Randomly disable neurons during training
- **Early Stopping**: Stop training when validation error increases
- **Data Augmentation**: Generate more training data
- **Max Depth / Min Samples** (trees): Limit tree complexity

---

## Hyperparameter Tuning

```python
from sklearn.model_selection import GridSearchCV, RandomizedSearchCV

# Grid Search — exhaustive
param_grid = {'C': [0.1, 1, 10], 'kernel': ['linear', 'rbf']}
grid = GridSearchCV(SVC(), param_grid, cv=5, scoring='accuracy')
grid.fit(X_train, y_train)
grid.best_params_       # {'C': 1, 'kernel': 'rbf'}
grid.best_score_        # 0.95

# Randomized Search — faster, samples from distributions
from scipy.stats import uniform
param_dist = {'C': uniform(0.1, 10), 'kernel': ['linear', 'rbf']}
random_search = RandomizedSearchCV(SVC(), param_dist, n_iter=50, cv=5)
```

| Method            | Pros                      | Cons                      |
|-------------------|---------------------------|---------------------------|
| **Grid Search**   | Exhaustive, thorough      | Slow for large spaces     |
| **Random Search** | Faster, covers wide range | May miss optimal          |
| **Bayesian Opt**  | Smart, efficient          | Complex setup             |

---

## Ensemble Methods

Combine multiple models for better performance.

### Bagging (Bootstrap Aggregating)

Train models on random subsets → **average** (regression) or **vote** (classification).

```python
from sklearn.ensemble import RandomForestClassifier
rf = RandomForestClassifier(n_estimators=100, max_depth=10, random_state=42)
rf.fit(X_train, y_train)
rf.feature_importances_
```

### Boosting

Train models **sequentially**, each fixing errors of the previous.

```python
from sklearn.ensemble import GradientBoostingClassifier
# XGBoost (install: pip install xgboost)
import xgboost as xgb
model = xgb.XGBClassifier(n_estimators=100, learning_rate=0.1, max_depth=5)
```

| Method              | Key Idea                                    |
|---------------------|---------------------------------------------|
| **Random Forest**   | Bagging + random feature subsets            |
| **AdaBoost**        | Boost misclassified samples' weights        |
| **Gradient Boosting**| Fit residuals sequentially                 |
| **XGBoost**         | Optimized gradient boosting (fast, popular) |
| **LightGBM**        | Leaf-wise growth (very fast)               |
| **CatBoost**        | Handles categoricals natively              |
| **Stacking**        | Use a meta-model on top of base models      |

---

## Neural Networks Basics

### Perceptron (Single Neuron)

```
Input → Weighted Sum → Activation Function → Output
x₁ ──w₁──┐
x₂ ──w₂──┤→ Σ(wᵢxᵢ + b) → f(z) → ŷ
x₃ ──w₃──┘
```

### Common Activation Functions

| Function    | Formula                    | Range       | Use Case                |
|-------------|----------------------------|-------------|-------------------------|
| **Sigmoid** | 1/(1+e⁻ˣ)                 | (0, 1)      | Binary output           |
| **Tanh**    | (eˣ-e⁻ˣ)/(eˣ+e⁻ˣ)       | (-1, 1)     | Hidden layers           |
| **ReLU**    | max(0, x)                  | [0, ∞)      | Hidden layers (default) |
| **Softmax** | eˣⁱ / Σeˣʲ                | (0, 1)      | Multi-class output      |

### Loss Functions

| Task                  | Loss Function          |
|-----------------------|------------------------|
| Binary classification | Binary Cross-Entropy   |
| Multi-class           | Categorical Cross-Entropy |
| Regression            | MSE / MAE              |

### Training Process

```
1. Forward Pass    → Compute predictions
2. Compute Loss    → Compare predictions vs actual
3. Backward Pass   → Compute gradients (backpropagation)
4. Update Weights  → Optimizer adjusts weights
5. Repeat          → Until convergence
```

### Optimizers

| Optimizer | Description                                       |
|-----------|---------------------------------------------------|
| **SGD**   | Basic stochastic gradient descent                 |
| **Adam**  | Adaptive learning rate (most popular)             |
| **RMSprop**| Adaptive, good for RNNs                          |
| **AdaGrad**| Adaptive, good for sparse data                   |

---

## Common Python Libraries

| Library          | Purpose                                        |
|------------------|------------------------------------------------|
| **NumPy**        | Numerical computing, arrays                    |
| **Pandas**       | Data manipulation, DataFrames                  |
| **Matplotlib**   | Basic plotting                                 |
| **Seaborn**      | Statistical visualization                      |
| **Scikit-learn** | ML algorithms, preprocessing, evaluation       |
| **XGBoost**      | Gradient boosting                              |
| **LightGBM**     | Fast gradient boosting                         |
| **TensorFlow**   | Deep learning framework (Google)               |
| **PyTorch**      | Deep learning framework (Meta)                 |
| **Keras**        | High-level DL API (part of TensorFlow)         |
| **Statsmodels**  | Statistical modeling                           |
| **NLTK/spaCy**   | Natural Language Processing                    |
| **OpenCV**       | Computer Vision                                |

---

## Key Formulas

### Distances

```
Euclidean:     d = √(Σ(xᵢ - yᵢ)²)
Manhattan:     d = Σ|xᵢ - yᵢ|
Cosine Sim:    cos(θ) = (x·y) / (||x|| × ||y||)
```

### Information Theory (Decision Trees)

```
Entropy:       H = -Σ pᵢ log₂(pᵢ)
Gini Impurity: G = 1 - Σ pᵢ²
Info Gain:     IG = H(parent) - Σ(weighted H(child))
```

### Gradient Descent

```
w_new = w_old - α × ∂Loss/∂w

α = learning rate
- Too large → diverges
- Too small → slow convergence
```

---

## ML Pipeline Example (End-to-End)

```python
import pandas as pd
from sklearn.model_selection import train_test_split, cross_val_score
from sklearn.preprocessing import StandardScaler
from sklearn.ensemble import RandomForestClassifier
from sklearn.metrics import classification_report
from sklearn.pipeline import Pipeline

# 1. Load data
df = pd.read_csv("data.csv")

# 2. Separate features and target
X = df.drop("target", axis=1)
y = df["target"]

# 3. Split
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42
)

# 4. Build pipeline
pipe = Pipeline([
    ('scaler', StandardScaler()),
    ('clf', RandomForestClassifier(n_estimators=100, random_state=42))
])

# 5. Cross-validate
scores = cross_val_score(pipe, X_train, y_train, cv=5)
print(f"CV Accuracy: {scores.mean():.3f} ± {scores.std():.3f}")

# 6. Train & evaluate
pipe.fit(X_train, y_train)
y_pred = pipe.predict(X_test)
print(classification_report(y_test, y_pred))
```

---

## Glossary

| Term              | Definition                                              |
|-------------------|---------------------------------------------------------|
| **Feature**       | Input variable (column) used for prediction             |
| **Label/Target**  | Output variable to predict                              |
| **Instance**      | Single data point (row)                                 |
| **Epoch**         | One complete pass through the training data             |
| **Batch Size**    | Number of samples per gradient update                   |
| **Learning Rate** | Step size for weight updates                            |
| **Overfitting**   | Model memorizes training data, fails on new data        |
| **Underfitting**  | Model is too simple to capture patterns                 |
| **Hyperparameter**| Setting configured before training (not learned)        |
| **Gradient**      | Direction & magnitude of steepest loss increase         |
| **Inference**     | Using a trained model to make predictions               |

---

> **Last Updated:** April 2026
