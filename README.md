# Sales-Data-Analysis-and-Reporting-for-a-Retail-Chain

import pandas as pd
import matplotlib.pyplot as plt

# Load your dataset (update path to where your CSV is saved)
transactions_df = pd.read_csv("Retail_Data_Transactions.csv")

# Convert transaction date to datetime
transactions_df['trans_date'] = pd.to_datetime(transactions_df['trans_date'], errors='coerce')

# -----------------------------
# 1. Line Chart: Monthly Sales
# -----------------------------
monthly_sales = (
    transactions_df
    .groupby(transactions_df['trans_date'].dt.to_period("M"))['tran_amount']
    .sum()
    .reset_index()
)

# Convert Period to Timestamp for plotting
monthly_sales['trans_date'] = monthly_sales['trans_date'].dt.to_timestamp()

# Plot line chart
plt.figure(figsize=(8,4))
plt.plot(monthly_sales['trans_date'], monthly_sales['tran_amount'], marker='o', color='blue')
plt.title("Monthly Sales Trend")
plt.xlabel("Month")
plt.ylabel("Total Sales")
plt.xticks(rotation=45)
plt.grid(True)
plt.tight_layout()
plt.show()

# -----------------------------
# 2. Bar Chart: Top 5 Customers
# -----------------------------
top_customers = (
    transactions_df
    .groupby("customer_id")['tran_amount']
    .sum()
    .reset_index()
    .sort_values(by="tran_amount", ascending=False)
    .head(5)
)

# Plot bar chart
plt.figure(figsize=(7,4))
plt.bar(top_customers['customer_id'].astype(str), top_customers['tran_amount'], color='teal')
plt.title("Top 5 Customers by Sales")
plt.xlabel("Customer ID")
plt.ylabel("Total Sales")
plt.xticks(rotation=30)
plt.tight_layout()
plt.show()


