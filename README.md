# Customer Churn Analysis
![image](https://github.com/user-attachments/assets/fc056213-d479-4594-8caf-dee593cd6065)


## Title 
Customer Value and Churn Risk Segmentation for a California-based Telecommunications Provider

## 1. Background
Customer churn remains a critical issue in the telecommunications industry, directly impacting revenue and long-term sustainability. This study investigates a dataset of 7,043 customers from a fictional telecom operator based in California, comprising demographic, behavioral, service usage, and billing attributes. The business challenge addressed is the identification of high-value customers and early detection of churn risk in order to inform targeted retention strategies.

## 2. Objective
The primary objective is to develop a simplified, data-driven segmentation model that quantifies customer value and churn risk. The final deliverable is a one-page Power BI dashboard that provides actionable insights for executive decision-making, particularly for the Chief Marketing Officer (CMO). The goal is to enhance customer retention by identifying priority segments for engagement and risk mitigation.

## 3. Methodology

### 3.1 Data Preparation and Transformation
The dataset was processed using Power Query within Power BI. Initial steps included handling missing values, standardizing categorical variables, and filtering for active (staying) and recently churned customers. New normalized composite metrics were derived for both customer value and churn value, each ranging from 0 to 1 for interpretability and visual clarity.

### 3.2 Development of Customer Value Metric
**Purpose:**
The goal was to measure how valuable each customer is to the business. Instead of looking at one variable at a time, I created a single value score that combines multiple important factors.

**How I chose the variables:**
After exploring the data, I selected three variables that reflect customer value:

- **Monthly Charges:** How much the customer pays each month (shows how profitable they are).
- **Tenure:** How long the customer has stayed with the company (shows loyalty).
- **Referrals:** Whether they brought in new customers (shows engagement and satisfaction).

These three were combined into one score, with weights to reflect their importance:

- 60% Monthly Charges
- 30% Tenure
- 10% Referrals

I gave more weight to Monthly Charges because it reflects current revenue. Tenure was next, as loyal customers are more valuable over time. Referrals got a smaller share, but they still matter as a sign of trust.

**How the score was calculated:**
Each variable was first scaled to a range between 0 and 1 (a method called normalization). This made sure all values were on the same scale and could be fairly combined.

Then I used the formula:
  **Customer Value Score=(0.6×Monthly Charges)+(0.3×Tenure)+(0.1×Referrals)**
This created a score from 0 (lowest value) to 1 (highest value) for each customer.

### 3.3 Construction of Churn Risk Metric
**Purpose:**
The goal here was to identify which customers are most likely to leave (churn). I created a churn risk score that combines patterns found across multiple factors in the data.

**How I approached it:**
I started by finding the average churn rate in the company, which was about 28%. Then I looked at how this rate changed across different groups of customers—like contract types, marital status, and services used.

Example:

- Among all customers, 28% had churned, but for those with month-to-month contracts, the churn rate was 52%—much higher and for two-year contracts, it was only 3%.
--> This difference helped identify which factors were increasing churn risk.

**What I did next:**
I repeated this process for 16 variables, calculating the **difference between each group’s churn rate and the 28% average**. These differences showed whether a factor increased or decreased churn risk.

Then, I turned these differences into numbers between 0 and 1 using normalization (same method as above), and added them up to create a **Churn Risk Score** for each customer.

**Final step – Churn Classification:**
To make this score useful, I set a threshold:

- If a customer's score was **above 0.43**, they were labeled as **“Safe”** (less likely to churn).
- If it was below **0.43**, they were considered **“@ Risk”** and may need attention.

This helped the business quickly identify who to prioritize for retention efforts.

### 3.4 Segmentation and Classification
Based on their respective scores, customers were plotted on a 2D matrix with Value on one axis and Churn Risk on the other, yielding four strategic quadrants:

- High Value – Safe
- High Value – At Risk
- Low Value – Safe
- Low Value – At Risk

This classification enabled targeted interventions to protect high-value at-risk customers and minimize effort on low-value high-risk segments.

## 4. Visualization Strategy
To optimize interpretability for C-level stakeholders, a minimalist yet information-rich one-page dashboard was developed. It included:

- **Key Metric Cards (BANs)**: Total Monthly Charge, Customer Status Overall Information
- **Scatterplot Matrix**: Segment distribution across the Value vs. Churn Risk matrix, color-coded by customer status (Staying, Churned, Joined).
- **Bar Charts**: Top churn drivers visualized by their deviation from the baseline churn rate, such as contract type and marital status.

This approach provided both macro and micro perspectives on retention dynamics in a visually intuitive format.

## 5. Detailed Analysis

### 5.1 Churned vs. Stayed Customers
![image](https://github.com/user-attachments/assets/e6fb6839-6a98-4b92-8341-40208e251abe)

Churned customers make up 39% of the customer base but contribute 32% of monthly revenue. The high-value churned group alone accounts for $72K with the highest average monthly charge in the entire dataset—$94.1. This confirms that the business isn’t just losing volume, it’s losing revenue density.

Among stayed customers, a critical segment is those marked **“at risk”**: 1,889 accounts contributing $137K in monthly charges. These customers exhibit churn scores and behavioral patterns nearly identical to those who have already left. High-value stayed-at-risk customers pay an average of $89/month—only slightly less than the churned high-value group—indicating latent churn already embedded in the current base.

The scatterplot reinforces this. The high churn risk–high value quadrant is heavily populated by both churned (orange) and stayed (blue) customers. This visual overlap shows that the conditions leading to churn are not isolated—they persist, and they are repeating.

In contrast, safe customers, while contributing more stable revenue, show significantly lower average monthly charges, especially in the low-value segment ($23.8).

In short, the business is facing **quality churn**: the customers most worth keeping are the ones leaving—or about to. The challenge is not identifying them. The data already has. The challenge is acting on it.

### 5.2 Newly Joined Customers
![image](https://github.com/user-attachments/assets/e4bc1044-ac84-42a6-8693-d1146293e402)

Among 454 newly joined customers, 98% are low value, and 76% of total charges come from customers already classified as “at risk.” Their average monthly charge is modest—$41.6 overall, $56.6 for the at-risk group. The situation is even more concerning for high-value new joiners: all 10 accounts fall into the “at risk” category.

The scatterplot shows a tight clustering in the **high churn risk–low value quadrant**, overlapping with regions previously dominated by **churned customers**. Very few new accounts reach the low-risk zone, and none of the high-value new customers are considered safe.

This signals a structural weakness in acquisition. The company is bringing in customers who look behaviorally similar to those who previously churned—before they've even contributed meaningfully to revenue. Without a shift in onboarding or qualification strategy, most of these customers are unlikely to stay long or grow in value.

### 5.3 Key Trends and Common Patterns

**1. Churn Risk Is Present Across All Stages**
Whether customers are long-term (stayed), newly acquired, or already churned, churn risk consistently appears. It’s not isolated to any one phase—it **persists through the lifecycle** and is especially popular among high-value accounts.

**2. High Revenue Often Means High Risk**
Across both churned and stayed customers, high-value accounts show the **highest average monthly charges and the highest churn risk**. This contradicts the assumption that high spenders are stable. In fact, they are often the most vulnerable segment.

**3. Churn Is Predictable, Not Random**
The overlap in churn risk scores and value metrics between churned and stayed-at-risk customers proves that churn follows **a repeatable behavioral pattern**. Many stayed customers are simply earlier in the same journey churned customers have already taken.

**4. New Customers Look Like Churned Customers**
Most newly joined customers fall into the **high churn risk – low value quadrant**, the same region where churned accounts cluster. This suggests that **customer acquisition is bringing in accounts with low retention potential** from the start.

**5. High-Value Customers Are Rare and Fragile**
Among all customers, the highest-paying groups are also those **most likely to churn**, and their losses represent revenue decreases.

**6. Safe Customers Drive Volume, Not Margin**
The “safe” stayed group contributes significant revenue, but mainly from low-to-medium value accounts. These customers are valuable for stability but can not outweight the revenue loss caused by churn in the high-value segment

### 5.4. Churned Risk Factor
![image](https://github.com/user-attachments/assets/2317f5ba-f4d5-47ee-84fb-a20950ac2c96)
The profile that emerges from these churn risk factors is not simply one of dissatisfaction—it’s one of **disengagement** and **lack of commitment**.

Despite having core services like **Internet and Phone**, these customers are far more likely to churn when those services are paired with **month-to-month contracts, no added protections,** and no **behavioral signs of attachment**(like referrals, bundling, or optional features). This tells us that **service ownership alone doesn’t equal loyalty**.

**The Churn Mindset: “Nothing to Lose”**
Customers who churn typically:
- Use only essential services
- Avoid any long-term lock-in
- Decline support or protection features
- Make no referrals
- Accept no offers or perks
- Choose impersonal billing/payment methods (paperless, auto-drafted)

This is the behavior of a **highly commoditized customer** —one who sees the telecom provider as interchangeable and who is unlikely to feel any switching cost, emotional or practical.

What’s really happening?
The company is losing customers **not because they are unhappy** — **but because they were never truly onboarded, engaged, or bound to begin with**. They may have joined for price or convenience but found no reason to stay. No hooks, no habits, no value-added layers.

This means churn is not being caused at the end of the relationship—it’s seeded **at the very beginning**, during acquisition and onboarding.













