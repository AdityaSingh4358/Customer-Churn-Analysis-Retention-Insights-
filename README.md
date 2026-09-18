# Customer-Churn-Analysis-Retention-Insights
# 📉 Customer Churn Analysis for an OTT Streaming Platform

> Which customers are leaving, why are they leaving, and what can we do to keep them?

---

## What is this project about?

**Churn** simply means a customer cancelling their subscription.

Imagine a Netflix-style streaming service. If 3 out of every 10 subscribers cancel, the company loses a lot of money. It is also much cheaper to keep an existing customer than to find a new one.

In this project, I acted as a data analyst for such a company. I looked at customer, subscription and support data to find out:

- **Who** is most likely to cancel?
- **Why** do they cancel?
- **What** can the company do to stop it?

---

## 🔑 Key Results at a Glance

| Metric | Result |
|---|---|
| Overall churn rate | **28.6%** (about 3 in 10 customers left) |
| Retention rate | **71.4%** (customers who stayed) |
| Average customer tenure | **1,451 days** |
| Revenue lost to churn | **~18%** |

### 💡 Main Findings

1. **Contract type matters most.**
   Customers on **monthly** plans churn at **55.6%**, while those on **annual** plans churn at only **8.3%**. Monthly customers leave almost **7 times more often**.
2. **The Basic plan has the most cancellations,** but since these customers pay less, the money lost is smaller than the numbers suggest.
3. **Karnataka has the highest number of churned customers,** with a spike in **September**. This is worth investigating.
4. **Unhappy support experiences lead to cancellations.** Customers whose complaints were escalated are more likely to leave.

### ✅ What the company should do

- Encourage monthly subscribers to move to **annual plans** (discounts or offers).
- Look into **why Karnataka's cancellations spiked in September** (pricing, outages, complaints?).
- Contact customers with **escalated complaints** early, before they cancel.
- Build a simple **High / Medium / Low churn-risk list** so the support team can focus on the customers most likely to leave.
- Track competitors when customers mention switching as their reason for leaving.

---

## 🗂️ About the Data

The data comes from a **SQLite database** (`customer_churn.db`) with three linked tables. They are connected using the `customerid` column.

| Table | What it contains |
|---|---|
| **Customer** | Name, gender, date of birth, country, state |
| **Subscription** | Start date, renewal date, plan type (Basic / Standard / Premium), contract type (Monthly / Annual), cancellation date and reason, monthly charges, CLTV, churn score |
| **Support** | Complaint date, whether the complaint was escalated, CSAT score |

---

##  Analysis Process (Step by Step)

The whole project follows a simple flow:

```
Understand problem → Load data → Clean data → Create new columns → Combine tables
                  → Calculate KPIs → Make charts → Find insights → Recommend actions
```

### Step 1: Understand the business problem
Before touching any data, I defined the goal: find out **who churns, why they churn, and how to reduce it**.

### Step 2: Load the data
- Connected to the SQLite database using `sqlite3`.
- Listed all tables in the database and loaded each one into its own pandas DataFrame (Customer, Subscription, Support).
- Checked the column names and data types of every table.

### Step 3: Clean the data
Each table had its own small problems, so I fixed them one by one.

| Table | What I fixed |
|---|---|
| **Customer** | Renamed `name` to `customer_name` · Dropped `interests` and `pincode` (not needed) · Converted date of birth to a proper date · Made gender labels consistent ("Men" → "Male", "Women" → "Female") · Filled missing countries by matching each customer's state to its country |
| **Subscription** | Converted start, renewal and cancellation dates to proper date format |
| **Support** | Dropped unused columns (`col_1`, `comment`) · Converted complaint date to a proper date |

### Step 4: Create new columns (feature engineering)
- **Churn flag:** `1` if a customer has a cancellation date, otherwise `0`.
- **Complaint count:** how many complaints each customer raised.
- **Tenure (days):** days from subscription start to cancellation (or to today, if the customer is still active).
- **Churn risk:** based on the churn score: below 50 = **Low**, 50 to 69 = **Medium**, 70 and above = **High**.

### Step 5: Combine the three tables
- A customer can have many complaints, which would create duplicate rows after joining. So I first counted complaints per customer, then kept only the **latest complaint** for each one.
- Then I joined Subscription, Customer and Support using `customerid`.
- I checked the number of rows **before and after** joining to make sure nothing was duplicated or lost.
- Saved the clean, combined data as `exported_churn_data.csv`.

### Step 6: Calculate the KPIs
Worked out the key business numbers: churn rate, retention rate, churn by plan, ARPU, average tenure, revenue at risk, escalation rate, average complaints per customer, and the link between escalations and churn (see the KPI table below).

### Step 7: Visualize the patterns
Worked on a **copy** of the data so the clean dataset stays untouched, then created:
- A line chart of **monthly churn** to spot spikes
- Bar charts of **churn by plan type** and **churn by state**
- A **correlation heatmap** to see which factors move together with churn. Before this, I converted categories into ordered numbers (Basic < Standard < Premium, Monthly < Annual, Low < Medium < High) so the comparison makes sense.

### Step 8: Find insights and recommend actions
Compared the results across groups (contract, plan, state, support history) to find the biggest churn drivers, then turned them into clear, practical recommendations for the business (see **Key Results** above).

---

##  KPIs Calculated

| KPI | How it is calculated |
|---|---|
| Churn rate | Churned customers ÷ total customers |
| Retention rate | 100% − churn rate |
| Churn by plan type | Churn rate for Basic, Standard and Premium |
| Churn by state | Churn rate for each state |
| ARPU (average revenue per user) | Average of monthly charges |
| Average tenure | Days between start date and cancellation (or today, if still active) |
| Revenue at risk | Monthly charges linked to churned or high-risk customers |
| Escalation rate | Share of complaints that were escalated |
| Complaints per customer | Total complaints ÷ number of customers |
| Escalation vs churn | Do customers with escalated complaints leave more often? |

---

## 📊 Charts in the Notebook

- Monthly churn trend over time
- Churn rate by plan type
- Churn rate by state
- Correlation heatmap (which factors move together with churn)

---

## 🧰 Tools Used

- **Python**: pandas, NumPy
- **SQL / SQLite**: for storing and querying the data
- **Matplotlib & Seaborn**: for charts
- **Jupyter Notebook**: for the analysis

---

## 📁 Project Files

```
├── Churn_Analysis_of_Customer.ipynb      # Full analysis code
├── Customer_Churn_Analysis_Report.pdf    # Business report with findings
├── customer_churn.db                     # Source database (SQLite)
└── README.md                             # You are here
```

---

## ▶️ How to Run It Yourself

1. **Clone or download** this repository.
2. **Install the libraries:**
   ```bash
   pip install pandas numpy matplotlib seaborn jupyter
   ```
3. Make sure `customer_churn.db` is in the **same folder** as the notebook.
4. **Open the notebook:**
   ```bash
   jupyter notebook Churn_Analysis_of_Customer.ipynb
   ```
5. Run the cells from top to bottom.

---

##  What's Next?

- Build a **prediction model** (logistic regression or gradient boosting) that gives every active customer a probability of leaving.
- Create an **interactive dashboard with Streamlit**, so anyone can filter churn by plan, region and support history.

---

## 👤 About Me

**Aditya Mohan Singh**: Civil Engineering student at NIT Calicut, exploring data analytics.
This is an independent portfolio project.

Feel free to reach out or leave feedback! 😊
