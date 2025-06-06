# sale-data-analysis-of-any-commercial-store
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
from datetime import datetime, timedelta

# ------------------------------
# 1. Generate Simulated Sales Data
# ------------------------------

np.random.seed(42)  # Reproducibility

# Sample products
products = ['Rice', 'Sugar', 'Milk', 'Bread', 'Detergent', 'Shampoo', 'Toothpaste', 'Eggs', 'Water', 'Snacks']

# Generate 1000 transactions
n = 1000
data = {
    'Date': [datetime(2025, 1, 1) + timedelta(days=np.random.randint(0, 90)) for _ in range(n)],
    'Product': np.random.choice(products, n),
    'Units_Sold': np.random.randint(1, 10, n),
    'Unit_Price': np.random.uniform(1.5, 20.0, n)
}

df = pd.DataFrame(data)
df['Total_Sale'] = df['Units_Sold'] * df['Unit_Price']
df['Date'] = pd.to_datetime(df['Date'])

# ------------------------------
# 2. Summary Metrics
# ------------------------------

total_sales = df['Total_Sale'].sum()
total_orders = len(df)
average_order_value = df['Total_Sale'].mean()
unique_products = df['Product'].nunique()

print("=== SALES SUMMARY ===")
print(f"Total Revenue: ${total_sales:,.2f}")
print(f"Total Orders: {total_orders}")
print(f"Average Order Value: ${average_order_value:.2f}")
print(f"Number of Products Sold: {unique_products}\n")

# ------------------------------
# 3. Monthly Sales Trend
# ------------------------------

df['Month'] = df['Date'].dt.to_period('M')
monthly_sales = df.groupby('Month')['Total_Sale'].sum()

print("=== MONTHLY SALES ===")
print(monthly_sales)

monthly_sales.plot(kind='bar', title='Monthly Sales Revenue', color='skyblue')
plt.ylabel("Revenue ($)")
plt.tight_layout()
plt.show()

# ------------------------------
# 4. Top-Selling Products
# ------------------------------

top_products = df.groupby('Product')['Total_Sale'].sum().sort_values(ascending=False).head(5)
print("\n=== TOP 5 BEST-SELLING PRODUCTS ===")
print(top_products)

top_products.plot(kind='barh', title='Top 5 Best-Selling Products', color='orange')
plt.xlabel("Revenue ($)")
plt.gca().invert_yaxis()
plt.tight_layout()
plt.show()

# ------------------------------
# 5. Units Sold per Product
# ------------------------------

units_sold = df.groupby('Product')['Units_Sold'].sum().sort_values(ascending=False)
print("\n=== TOTAL UNITS SOLD PER PRODUCT ===")
print(units_sold)

units_sold.plot(kind='bar', title='Units Sold per Product', color='green')
plt.ylabel("Units")
plt.tight_layout()
plt.show()
