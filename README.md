# Exploratory Data Analysis (EDA): Orders & Transactions Performance

This repository features an end-to-end Exploratory Data Analysis (EDA) workflow focused on an e-commerce/retail **Orders Dataset**. Using Python, the project deep-dives into customer purchasing logs to calculate vital summary statistics, isolate order anomalies, track time-based purchasing trends, and deliver actionable operational insights.

## 🚀 5 Useful Insights Discovered
By analyzing the order metrics, five specific operational insights were uncovered:

1. **Peak Order Windows (Trend):** Time-series analysis shows order volume surges significantly during specific hours (e.g., mid-day lunch breaks and evening hours), highlighting optimal periods for deployment and marketing drops.
2. **Order Value Concentration (Pattern):** A Pareto analysis indicates that a minor percentage of high-value client accounts or specific product categories generate the vast majority of cumulative revenue.
3. **High-Ticket Anomalies (Outliers):** Interquartile Range (IQR) filtering isolated several massive order quantities far exceeding the 95th percentile, likely indicating bulk B2B wholesale buyers or data logging duplicates.
4. **Order Volume Velocity (Trend):** Clear cyclical velocity dips appear mid-week, signaling ideal windows to run promotional campaigns or flash discounts to stabilize revenue.
5. **Basket Size Correlation (Pattern):** Correlation matrices revealed a strong positive relationship between specific product pairings, providing data-backed foundation for recommendation algorithms and cross-selling.

## 📊 Statistical Analysis Performed
- **Descriptive Statistics:** Summarized average order value (AOV), total order count, and standard deviation distributions.
- **Data Distribution Mapping:** Assessed the skewness of order amounts to understand consumer spending limits.
- **Anomaly Detection:** Utilized statistical thresholds to isolate fraud patterns or out-of-bounds quantity inputs.

## 🛠️ Tech Stack & Dependencies
- **Language:** Python (`Eda analysis.py`)
- **Data Manipulation:** Pandas, NumPy
- **Visual Mapping:** Matplotlib, Seaborn

## 💻 How to Run the Project Locally
1. Clone this repository:
   ```bash
   git clone https://github.com
   ```
2. Install the necessary analysis libraries:
   ```bash
   pip install pandas numpy matplotlib seaborn
   ```
3. Run the analysis script:
   ```bash
   python "Eda analysis.py"
   ```
