# Promotion Effectiveness at a Fashion Retail Chain

---

# **B1. Problem Formulation**  

## **(a) ML Problem Definition**  

**Target Variable:**  
- Number of *items sold* per store per month (sales volume).  

**Input Features (examples):**  
- **Store-level:** location type (urban/semi-urban/rural), store size, footfall, competition density  
- **Promotion-level:** type of promotion (Flat Discount, BOGO, etc.), discount depth/value  
- **Customer-level (aggregated):** demographics, purchase patterns  
- **Time-level:** month, seasonality, weekends, festivals  
- **Historical performance:** past sales under similar promotions  

**Type of ML Problem:**  
- Supervised learning — Regression problem  

**Justification:**  
- The target is a continuous numeric variable (items sold).  
- We predict sales under different promotions and choose the best → *predict-then-optimize approach*.  

---

## **(b) Why Items Sold > Revenue**  

**Issues with Revenue:**  
- Affected by price discounts  
- Varies by product price  
- Promotions (e.g., BOGO) distort revenue  

**Why Items Sold is Better:**  
- Direct measure of customer response  
- Not biased by pricing  
- Aligns with goal: increase inventory movement  

**Broader Principle:**  
> The target variable must align with the true business objective and avoid external distortions.  

---

## **(c) Alternative Modelling Strategy (2 marks)**  

**Problem with Single Global Model:**  
- Ignores differences across store locations  

**Proposed Approach:**  
- Hierarchical / segmented models  
  - Separate models for urban, semi-urban, rural stores  
  - Or multi-level (mixed-effects) models  

**Justification:**  
- Captures local variation  
- Improves prediction accuracy  

---

# **B2. Data and EDA Strategy (10 marks)**  

## **(a) Data Joining & Dataset Design (4 marks)**  

**Tables:**  
- Transactions  
- Store attributes  
- Promotion details  
- Calendar  

**Join Strategy:**  
- Transactions + Store → `store_id`  
- Transactions + Promotion → `promotion_id`  
- Transactions + Calendar → `date`  

**Final Grain:**  
- One row = one store × one month  

**Aggregations:**  
- Total items sold  
- Total transactions  
- Average basket size  
- Promotion usage rate  
- Revenue (optional)  

**Engineered Features:**  
- % transactions under promotion  
- Lag features (previous month sales)  
- Rolling averages  

---

## **(b) EDA Plan (4 marks)**  

### 1. Promotion vs Sales  
- Chart: Boxplot / Bar chart  
- Goal: Compare performance across promotions  
- Impact: Identify strong promotions  

---

### 2. Time Series Trend  
- Chart: Line chart  
- Goal: Identify seasonality (festivals, holidays)  
- Impact: Add time-based features  

---

### 3. Store Segmentation  
- Chart: Scatter plots  
- Goal: Analyze sales vs footfall, size, competition  
- Impact: Support clustering or segmentation  

---

### 4. Promotion Effect by Store Type  
- Chart: Grouped bar chart  
- Goal: Understand interaction effects  
- Impact: Create interaction features  

---

### 5. Target Distribution  
- Chart: Histogram  
- Goal: Check skewness  
- Impact: Apply transformations (e.g., log) if needed  

---

## **(c) Handling Promotion Imbalance (2 marks)**  

**Problem:**  
- 80% transactions without promotions  
- Model may ignore promotion effects  

**Solutions:**  
- Oversample promotion cases  
- Use balanced sampling  
- Add promotion indicator features  

---

# **B3. Model Evaluation and Deployment (12 marks)**  

## **(a) Train-Test Split & Metrics (4 marks)**  

**Train-Test Strategy:**  
- Time-based split  
  - Train: First ~2–2.5 years  
  - Test: Last ~6–12 months  

**Why Not Random Split?**  
- Causes data leakage  
- Breaks time dependency  

---

**Evaluation Metrics:**  

- **MAE (Mean Absolute Error):**  
  - Average error in units sold  

- **RMSE (Root Mean Squared Error):**  
  - Penalizes large errors  

- **MAPE (Mean Absolute Percentage Error):**  
  - Percentage-based error  

**Interpretation:**  
- Low MAE → Accurate predictions  
- Low RMSE → Fewer large mistakes  
- Low MAPE → Consistency across stores  

---

## **(b) Explaining Different Recommendations (4 marks)**  

**Approach:**  
- Use feature importance and local explanations (e.g., SHAP)  

**Example:**  
- **December (Loyalty Bonus):**  
  - High repeat purchases  
  - Holiday season  

- **March (Flat Discount):**  
  - Lower demand  
  - Higher price sensitivity  

**Communication:**  
- Translate insights into business language  
- Focus on *why* the recommendation makes sense  

---

## **(c) Deployment & Monitoring (4 marks)**  

### 1. Model Saving  
- Save using Pickle / Joblib  
- Version control (v1, v2, etc.)  

---

### 2. Monthly Prediction Pipeline  
1. Collect latest data  
2. Aggregate to store-month level  
3. Generate features  
4. Predict sales for each promotion  
5. Select best promotion  

---

### 3. Output  
- Table:  
  - Store ID → Recommended Promotion  

---

### 4. Monitoring & Retraining  

**Performance Monitoring:**  
- Track predicted vs actual sales  
- Monitor MAE / MAPE  

**Data Drift Detection:**  
- Changes in customer behavior  
- Changes in footfall or promotion response  

**Retraining Trigger:**  
- Performance drops beyond threshold  
- Major seasonal or business changes  

---

**Conclusion:**  
A successful system combines prediction, monitoring, and periodic retraining to ensure long-term effectiveness.