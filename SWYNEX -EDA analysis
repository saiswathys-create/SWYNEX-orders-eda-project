import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import matplotlib.dates as mdates

plt.rcParams.update({"figure.dpi": 130, "font.size": 10})

df = pd.read_csv("/mnt/user-data/uploads/orders_cleaned.csv", parse_dates=["SignupDate","OrderDate"])
df["Revenue"] = df["Quantity"] * df["Price"]

# ---------------- Summary stats ----------------
summary = df[["Quantity","Price","Revenue"]].describe()
print(summary)
print("\nTotal revenue (all orders):", df["Revenue"].sum())
print("Total revenue (Delivered only):", df[df.Status=="Delivered"]["Revenue"].sum())
print("Avg order value:", df["Revenue"].mean())
print("Median order value:", df["Revenue"].median())

# ---------------- 1. Orders/Revenue by status ----------------
status_counts = df["Status"].value_counts()
status_rev = df.groupby("Status")["Revenue"].sum().reindex(status_counts.index)
fig, ax1 = plt.subplots(figsize=(7,4.5))
bars = ax1.bar(status_counts.index, status_counts.values, color="#2563eb", alpha=0.85)
ax1.set_ylabel("Number of Orders", color="#2563eb")
ax1.set_title("Order Status: Count vs Revenue")
for b in bars:
    ax1.text(b.get_x()+b.get_width()/2, b.get_height(), f"{int(b.get_height())}",
              ha="center", va="bottom", fontsize=9)
ax2 = ax1.twinx()
ax2.plot(status_counts.index, status_rev.values, color="#dc2626", marker="o", linewidth=2)
ax2.set_ylabel("Total Revenue ($)", color="#dc2626")
plt.tight_layout()
plt.savefig("charts2/01_status.png")
plt.close()

# ---------------- 2. Revenue by product category ----------------
cat_rev = df.groupby("ProductCategory")["Revenue"].sum().sort_values(ascending=False)
fig, ax = plt.subplots(figsize=(7,4.5))
bars = ax.bar(cat_rev.index, cat_rev.values, color="#0ea5e9")
ax.set_title("Total Revenue by Product Category")
ax.set_ylabel("Revenue ($)")
plt.xticks(rotation=20)
for b in bars:
    ax.text(b.get_x()+b.get_width()/2, b.get_height(), f"${b.get_height():,.0f}",
            ha="center", va="bottom", fontsize=8)
plt.tight_layout()
plt.savefig("charts2/02_category_revenue.png")
plt.close()

# ---------------- 3. Monthly order trend ----------------
monthly = df.set_index("OrderDate").resample("ME").size()
monthly_rev = df.set_index("OrderDate").resample("ME")["Revenue"].sum()
fig, ax = plt.subplots(figsize=(8,4.5))
ax.plot(monthly.index, monthly.values, marker="o", color="#2563eb", linewidth=1.8, label="Order count")
ax.set_title("Monthly Order Volume (2021-2024)")
ax.set_ylabel("Number of Orders")
ax.xaxis.set_major_formatter(mdates.DateFormatter("%b %Y"))
fig.autofmt_xdate()
ax.grid(alpha=0.3)
plt.tight_layout()
plt.savefig("charts2/03_monthly_orders.png")
plt.close()

# ---------------- 4. Revenue by country ----------------
country_rev = df.groupby("Country")["Revenue"].sum().sort_values(ascending=False)
fig, ax = plt.subplots(figsize=(7,4.5))
colors = ["#94a3b8" if c=="Unknown" else "#16a34a" for c in country_rev.index]
bars = ax.bar(country_rev.index, country_rev.values, color=colors)
ax.set_title("Total Revenue by Country")
ax.set_ylabel("Revenue ($)")
plt.xticks(rotation=20)
for b in bars:
    ax.text(b.get_x()+b.get_width()/2, b.get_height(), f"${b.get_height():,.0f}",
            ha="center", va="bottom", fontsize=8)
plt.tight_layout()
plt.savefig("charts2/04_country_revenue.png")
plt.close()

# ---------------- 5. Price distribution / outliers by category ----------------
fig, ax = plt.subplots(figsize=(7,4.5))
df.boxplot(column="Price", by="ProductCategory", ax=ax, grid=False,
           boxprops=dict(color="#334155"), medianprops=dict(color="#dc2626"))
ax.set_title("Price Distribution by Category (Outlier Check)")
plt.suptitle("")
ax.set_ylabel("Price ($)")
plt.xticks(rotation=20)
plt.tight_layout()
plt.savefig("charts2/05_price_boxplot.png")
plt.close()

# ---------------- 6. Cancelled/Returned rate by category ----------------
prob = df.groupby("ProductCategory")["Status"].apply(
    lambda s: (s.isin(["Cancelled","Returned"])).mean()*100
).sort_values(ascending=False)
fig, ax = plt.subplots(figsize=(7,4.5))
bars = ax.bar(prob.index, prob.values, color="#dc2626")
ax.set_title("Cancelled + Returned Rate by Category")
ax.set_ylabel("% of Orders")
plt.xticks(rotation=20)
for b in bars:
    ax.text(b.get_x()+b.get_width()/2, b.get_height(), f"{b.get_height():.1f}%",
            ha="center", va="bottom", fontsize=8)
plt.tight_layout()
plt.savefig("charts2/06_cancel_return_rate.png")
plt.close()

# ---------------- Extra numbers for insights ----------------
print("\nOrders per Gender:\n", df["Gender"].value_counts())
print("\nRevenue per Gender:\n", df.groupby("Gender")["Revenue"].sum())
print("\nAvg order value by Gender:\n", df.groupby("Gender")["Revenue"].mean())

print("\nCancel+Return rate overall:", (df["Status"].isin(["Cancelled","Returned"])).mean()*100)
print("\nCancel+Return by category:\n", prob)

print("\nUnknown country orders:", (df["Country"]=="Unknown").sum())
print("Unknown country revenue:", df[df.Country=="Unknown"]["Revenue"].sum())

q99 = df["Revenue"].quantile(0.95)
print("\n95th pct revenue:", q99)
outliers = df[df["Revenue"]>q99]
print(outliers[["OrderID","ProductCategory","Quantity","Price","Revenue","Status"]])

corr = df[["Quantity","Price","Revenue"]].corr()
print("\nCorrelation:\n", corr)

df["SignupYear"] = df["SignupDate"].dt.year
print("\nMissing SignupDate count:", df["SignupDate"].isna().sum())

print("\nAvg days between signup and order (where signup known):")
delta = (df["OrderDate"] - df["SignupDate"]).dt.days
print(delta.describe())
