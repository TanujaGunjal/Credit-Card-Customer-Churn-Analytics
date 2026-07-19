# SQL Query Documentation
## Credit Card Customer Churn & Spending Analytics

### Objective
This SQL analysis investigates **customer churn behavior and spending patterns** in a credit card portfolio.

**Dataset Size:** 10,127 customers  
**Domain:** Fintech / Credit Card Analytics  

---

# 1️⃣ Total Customer Base

### Business Question
How many customers exist in the credit card portfolio?

### SQL Query
```sql
SELECT COUNT(*) AS total_customers
FROM bankchurners;
```

### KPI Result
- **Total Customers:** 10,127

### Insight
This represents the total population used for churn and behavioral analysis.

### Business Recommendation
This value serves as the baseline for churn calculations and customer segmentation.

---

# 2️⃣ Overall Customer Churn Rate

### Business Question
What percentage of customers have churned?

### SQL Query
```sql
SELECT 
COUNT(*) AS total_customers,
SUM(CASE WHEN Attrition_Flag='Attrited Customer' THEN 1 ELSE 0 END) AS churned_customers,
ROUND(
SUM(CASE WHEN Attrition_Flag='Attrited Customer' THEN 1 ELSE 0 END)/COUNT(*)*100,2
) AS churn_rate_percentage
FROM bankchurners;
```

### KPI Result
- **Churn Rate:** ~16%

### Insight
Approximately **1 out of every 6 customers leaves the service**, indicating moderate attrition.

### Business Recommendation
Financial institutions should monitor engagement metrics such as transaction activity and inactivity periods to detect churn risks early.

---

# 3️⃣ Churn Distribution by Card Category

### Business Question
Which card category experiences the highest churn?

### SQL Query
```sql
SELECT 
Card_Category AS card_type,
COUNT(*) AS churned_customers
FROM bankchurners
WHERE Attrition_Flag='Attrited Customer'
GROUP BY card_type
ORDER BY churned_customers DESC;
```

### Insight
**Blue card customers show the highest churn rate**, indicating lower engagement among entry-level cardholders.

### Business Recommendation
Introduce loyalty incentives or rewards programs targeting entry-level cardholders.

---

# 4️⃣ Average Transaction Amount by Card Category

### Business Question
Which card category generates the highest transaction volume?

### SQL Query
```sql
SELECT 
Card_Category AS card_type,
ROUND(AVG(Total_Trans_Amt),2) AS avg_transaction_amount
FROM bankchurners
GROUP BY card_type
ORDER BY avg_transaction_amount DESC;
```

### Insight
Premium card categories generally generate **higher average transaction amounts**.

### Business Recommendation
Encourage customers to upgrade to higher-tier cards with enhanced benefits.

---

# 5️⃣ Customer Inactivity vs Churn

### Business Question
Does customer inactivity correlate with churn?

### SQL Query
```sql
SELECT 
Attrition_Flag AS customer_status,
ROUND(AVG(Months_Inactive_12_mon),2) AS avg_inactive_months
FROM bankchurners
GROUP BY customer_status;
```

### Insight
Customers who churn tend to have **higher inactivity levels**, suggesting disengagement before leaving.

### Business Recommendation
Develop **early warning systems** that trigger retention campaigns when inactivity increases.

---

# 6️⃣ Transaction Activity by Customer Status

### Business Question
Do churned customers perform fewer transactions?

### SQL Query
```sql
SELECT 
Attrition_Flag AS customer_status,
ROUND(AVG(Total_Trans_Ct),2) AS avg_transaction_count
FROM bankchurners
GROUP BY customer_status;
```

### Insight
Customers who churn demonstrate **lower transaction frequency**, indicating reduced engagement.

### Business Recommendation
Offer transaction incentives such as cashback or reward points to boost engagement.

---

# 7️⃣ Customers with Above-Average Credit Limits

### Business Question
Which customers have credit limits higher than the portfolio average?

### SQL Query
```sql
SELECT 
CLIENTNUM AS customer_id,
Credit_Limit AS credit_limit
FROM bankchurners
WHERE Credit_Limit > (
SELECT AVG(Credit_Limit)
FROM bankchurners
)
ORDER BY credit_limit DESC;
```

### Insight
High credit limit customers represent **high-value segments with greater spending capacity**.

### Business Recommendation
Prioritize retention strategies for high-value customers.

---

# 8️⃣ Revenue Contribution by Card Category

### Business Question
Which card categories contribute most to overall transaction revenue?

### SQL Query
```sql
WITH card_revenue AS (
SELECT 
Card_Category AS card_type,
SUM(Total_Trans_Amt) AS total_revenue
FROM bankchurners
GROUP BY card_type
)

SELECT 
card_type,
total_revenue,
ROUND(
total_revenue * 100 / SUM(total_revenue) OVER (),
2
) AS revenue_percentage
FROM card_revenue
ORDER BY revenue_percentage DESC;
```

### Insight
Certain card categories contribute a **larger share of total transaction revenue**.

### Business Recommendation
Focus marketing and retention efforts on high-revenue customer segments.

---

# Final Conclusion

Key findings from the SQL analysis include:

- The overall **customer churn rate is approximately 16%**.
- **Customer inactivity strongly correlates with churn behavior.**
- **Blue cardholders exhibit the highest attrition rates.**
- Customers with higher transaction frequency demonstrate stronger engagement.
- High credit limit customers contribute significantly to revenue.

These insights enable financial institutions to design **data-driven retention strategies and improve long-term customer engagement**.
