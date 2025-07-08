# Customer Churn Analysis
![image](https://github.com/user-attachments/assets/fc056213-d479-4594-8caf-dee593cd6065)

## 1. Background
Customer churn remains a critical issue in the telecommunications industry, directly impacting revenue and long-term sustainability. This project investigates a dataset of 7,043 customers from a fictional telecom operator based in California, comprising demographic, behavioral, service usage, and billing attributes. The business challenge addressed is the identification of high-value customers and early detection of churn risk in order to inform targeted retention strategies.

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

Each variable carries different scales and units (e.g., tenure might range from 1 to 70 months, monthly charges from $20 to $120), making them unsuitable for direct aggregation.

These three were combined into one score, with weights to reflect their importance:

- 60% Monthly Charges
- 30% Tenure
- 10% Referrals

I gave more weight to Monthly Charges because it reflects current revenue. Tenure was next, as loyal customers are more valuable over time. Referrals got a smaller share, but they still matter as a sign of trust.
Then I used the formula:

**Customer Value Score=(0.6×Monthly Charges) / MAX(Monthly Charges) +(0.3×Tenure) / MAX(Tenure) +(0.1×Referrals) / MAX(Referrals)**
  
![image](https://github.com/user-attachments/assets/7e089707-584a-4615-9693-07c92584ace3)


**Why Normalize?**
After weighting the variables, we applied min-max normalization, which rescales each value to a standard range from 0 to 1. This preserves the relative differences across customers while aligning all variables on the same scale.

![image](https://github.com/user-attachments/assets/867c87ea-55b4-4547-98eb-e9fb7446b34a)

Where:
- 𝑋 is the original value
- 𝑋min and 𝑋max are the minimum and maximum observed values for that variable

Each variable was normalized separately using this formula in Power Query. This created a score from 0 (lowest value) to 1 (highest value) for each customer.

![image](https://github.com/user-attachments/assets/7e28a7b1-9ba5-435d-9a3d-271ce59e5548)


**Result**

Each customer is assigned a value score between 0 and 1:

- Closer to 1 → high financial and behavioral value
- Closer to 0 → lower strategic importance

Therefore, we set a threshold by taking the Average of the Normalised Value Rating column by:

- greater than 0.52, we will set as **"High Value"**,  less than 0.52, we will set as **"Low Value"** 

This normalized and weighted score allows for easy segmentation (e.g., high vs. low value) and integration into visual tools like scatterplots and dashboards.

### 3.3 Construction of Churn Risk Metric
**Purpose:**
The goal here was to identify which customers are most likely to leave (churn). I created a churn risk score that combines patterns found across multiple factors in the data.

**How I approached it:**
Instead of using machine learning to predict churn, we looked at each variable (like contract type or payment method) and checked:
**"Are people with this characteristic more or less likely to churn than average?"**

For example:
- 28% of all customers churn.
- But among month-to-month contract holders, 52% churn.
→ That’s a +24% churn risk compared to the average.

We did this for 16 variables, then combined and normalized those deviations into a single score that reflects how much a customer's **profile matches the behavior of past churners.**

**Here is the step-by-step:**

**Step 1: Baseline Churn Rate**

- The overall churn rate in the dataset is 28%.
- For each categorical variable (e.g., contract type, marital status, online security), the churn rate of each subgroup was calculated.
- Then, the **deviation** from the baseline was computed.

Example:
- Customers on month-to-month contracts have a 52% churn rate
- Deviation = 28% (baseline) – 52% = –24%
→ This subgroup is **more likely to churn**

![image](https://github.com/user-attachments/assets/b6d4f7bd-609d-43a8-8803-8a81736f8b17)


**Step 2: Deviation Scoring Across Multiple Variables**
This calculation was repeated for **16 churn-related variables**, including:

- Contract type
- Payment method
- Tech support and protection plan status
- Referral activity
- Streaming usage
- Offer acceptance, etc.

Each subgroup’s deviation was treated as a **churn risk signal**, where **greater negative deviation indicates higher risk**.

![image](https://github.com/user-attachments/assets/2954bcec-3f8c-43dd-89d8-234e9e329680)


**Step 3: Normalization**
To combine all variables fairly, each deviation value was normalized using **min-max scaling:**

![image](https://github.com/user-attachments/assets/799f99a5-d82e-4fa4-874e-0db21aa881b8)

D stands for Deviation. This scaled all deviation scores to a standard range from **0 to 1**, making them comparable across variables.

![image](https://github.com/user-attachments/assets/9b77b604-eb81-4967-bf36-d4551c78d2e7)


**Step 4: Aggregation into a Composite Score**

Once normalized, all 16 variables were aggregated (summed or averaged) to generate a Churn Risk Score for each customer.

This final score ranges from 0 (very high risk) to 1 (very low risk).

**Step 5: Threshold for Classification**
A cutoff point of 0.43 was applied:

- Customers with a churn score **above 0.43** were considered **“Safe”**
- Customers with a score **below 0.43** were labeled **“At Risk”** (@Risk)

This threshold was selected based on taking average of the Normalised Churn Risk column.


### 3.4 Segmentation and Classification
Based on their respective scores, here is the customer segmentation and classification, and this was plot on a scatterplot chart yielding four strategic quadrants:

**Final Segmentation**

![image](https://github.com/user-attachments/assets/0b6ae213-f0ef-47c7-9ea2-f92e4f4b00ae)

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

Churned customers make up 29% of the customer base but contribute 32% of monthly revenue. The high-value churned group alone accounts for $72K with the highest average monthly charge in the entire dataset—$94.1. This confirms that the business isn’t just losing volume, it’s losing revenue density.

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

**1. High-Value Churn Is a Major Revenue Loss**
Although high-value customers make up a smaller portion of the churned group, they account for a large share of the lost revenue. Their average monthly charges are significantly higher than others, meaning **losing just a few of them has the same financial impact as losing hundreds of low-value accounts.**

This suggests the company isn’t just bleeding volume—it’s bleeding margin. These customers were not only profitable, but already engaged in premium plans. Yet, **they still left.**

**2. Stayed Customers at Risk Outperform Safe Ones in Revenue**
Among customers who haven’t churned yet, those marked as “at risk” (based on churn score) have **higher average monthly charges** than those considered safe. This creates a quiet vulnerability: revenue is currently being held up by customers who resemble churners in behavior.

This means the business is not in a stable position—it’s in a **delayed-loss position**. Without intervention, the next major churn wave will come from within the most valuable accounts still active today.

**3. Newly Joined Customers Offer Little Long-Term Value**
The vast majority of new customers are both **low in value** and **high in churn risk**. Many of them already fall into the same segmentation quadrants as previously churned accounts. This means the **customer acquisition funnel is pulling in** the wrong profiles—those who are likely to leave before generating meaningful revenue.

This isn’t just a retention problem—it’s an acquisition design problem. The company is not attracting loyal, high-value customers. It’s onboarding churn-prone users and hoping they stay—despite no signs that they will. Their **structing growth and revenue around unstable customer segments**


### 5.4. Churned Risk Factor
![image](https://github.com/user-attachments/assets/2317f5ba-f4d5-47ee-84fb-a20950ac2c96)

The most common characteristics among churned customers reveal a clear behavioral pattern:

1. They have services, but no attachment

Most churned customers had Internet and Phone services, but lacked any deeper engagement. Having the service wasn’t enough to keep them.

2. They’re on flexible, easy-to-leave contracts

Month-to-month plans appear frequently, highlighting a structural weakness—these customers can leave at any time, and many do.

3. They avoid ddd-ons and optional features

From security to tech support and streaming, churned customers tend to opt out. This shows minimal product investment and limited perceived value.

4. They interact passively

With paperless billing and bank withdrawals, there’s little to no contact. Automation makes the relationship feel transactional—and easy to walk away from.

5. They don’t engage beyond the basics

No referrals, no offers accepted, no bundled services used. These customers stay isolated from loyalty mechanisms and product ecosystems.

6. High-performance plans don’t guarantee retention

Even fiber optic users churn, suggesting that strong infrastructure isn’t enough when expectations aren’t matched.

--> Churned customers are not dissatisfied—they’re disengaged. They use the bare minimum, stay anonymous, and leave without friction. The business doesn’t lose them—it never truly had them.

### 5.5. Final Synthesis: What the Data Really Tell Us
All in all, the data shows that churned customers consistently had active services, but low engagement. Most of them were on month-to-month contracts, opted out of add-on features, automated their payments, and did not participate in any loyalty programs or offers. Their usage was limited to basic services, with no behavioral connection to the provider.

This pattern also appears among many **stayed customers**, particularly those flagged as **“at risk”**. These accounts are still active but share the same behavioral traits as churned customers. Many of them are also high value in terms of monthly charges, meaning they represent both a revenue source and a churn risk.

**Newly joined customers** follow a similar profile. The majority are low-value and already fall into the high churn risk category. Their behavior matches that of churned customers more than it does retained ones. This suggests that the current acquisition process brings in customers who are unlikely to stay long term.

Across all segments, the key issue is consistent: many customers use the services but do not engage with the brand in a meaningful way. They do not adopt value-added features, do not commit to long-term contracts, and show no loyalty behavior.

This is not only a sign of retention challenges—it also points to issues at the acquisition and onboarding stages. Customers are not being guided toward behaviors that improve loyalty or reduce churn risk.

## 6. Actions and Recommendations

**1. Retain High-Value Customers Showing Churn Risk**

**What to do:**
Use churn risk scores to identify high-value customers who are still active but showing early signs of churn (e.g., score ≤ 0.43). Focus retention efforts on this group through contract renewal offers, personalized outreach, or added-value benefits.

**Why it matters:**
These customers contribute a significant share of current revenue, yet their behavior closely mirrors churned profiles. If ignored, this segment is likely to churn next—resulting in substantial financial loss.

**What this solves:**
Helps the business avoid losing its highest-paying customers by addressing churn risk early—before they decide to leave.

**2. Improve the Quality of New Customer Acquisition**

**What to do:**
Apply basic churn filters during acquisition (e.g., contract preference, feature opt-in) and implement onboarding that includes guided product introductions, early offers, and prompts for adopting features tied to long-term retention.

**Why it matters:**
Most newly joined customers are low-value and already high-risk. They reflect the same behavioral patterns as churned customers. Without intervention, they are unlikely to stay or grow in value.

**What this solves:**
Reduces early-stage churn, raises the baseline quality of the customer base, and aligns acquisition with long-term business value—not just volume.

**3. Reduce Churn by Promoting Commitment and Engagement Early**

**What to do:**
Encourage contract upgrades (e.g., from month-to-month to annual plans) and embed value-added features (like tech support or streaming bundles) as part of default plans. Actively promote engagement tools such as referral programs or content services.

**Why it matters:**
Churned customers rarely engaged with optional features or loyalty programs. They remained on flexible contracts and interacted minimally with the service. These conditions made it easy to leave.

**What this solves:**
Creates early behavioral anchors that increase switching cost, deepen product use, and lower churn probability—especially among high-risk, low-engagement users.















