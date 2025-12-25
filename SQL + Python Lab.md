# Amazon Marketplace — MySQL DB + Python Lab 

A compact “Amazon-style” marketplace database with Python scripts for reporting, stored routines, and a simple GUI dashboard.

- **Tech**: MySQL 8, Python 3.11+, `mysql-connector-python`, Tkinter
- **Entities**: Customer, Address, Seller, Product, Category, ProductCategory (bridge), Listing, Order, OrderItem, Payment, Shipment
- **Used**: realistic M:N relationships (product⇄category, seller⇄product via listing), snapshot pricing on order lines, and parameterized SQL used from Python.

## Files
/sql
- marketplace_code.sql # schema + seed inserts for a small demo dataset
- amazon_mkt_dump.sql # optional: full dump you can import

/pythom
- report3_run.py # runs 6 parameterized queries (report screenshots)
- report3_routines.py # calls stored PROCEDURE + FUNCTION
- mkt_gui.py # Tkinter GUI: Daily Sales, Top Customers, In-Transit, Inventory

## Quick start
1) **Create DB & load data**
```sql
-- In MySQL Workbench
SOURCE sql/marketplace_code.sql;
-- (optional) to load a full dump
SOURCE sql/amazon_mkt_dump.sql;
```
2)
Install Python deps
```
pip install mysql-connector-python
```
3) Run the demos
```
python python/report3_run.py        # prompts for DB password + parameters
python python/report3_routines.py   # calls procedure + function
python python/mkt_gui.py            # GUI dashboards

Use host=localhost, user=root, database=amazon_mkt. Password = your local MySQL root password.
```

## Schema overview

- Product ⇄ Category: M:N via product_category
- Seller offers Product: 1:N via listing (tracks current price)
- Order 1:N OrderItem (each line stores price_at_sale, qty, item_total)
- Order has 1 Payment and 1 Shipment (MVP)
- Typical order flow: placed → paid → shipped → delivered


## Featured SQL (SIX parameterized queries)
These are the six queries executed by report3_run.py (each uses host variables):
- Orders by status & min total since date (status, min_total, start_date)
- Line items for one order (order_id)
- Subtotal vs. sum(lines) for an order (order_id)
- Revenue per seller since date (start_date, min_revenue)
- Avg listing price by category (min_avg_price)
- Orders with ≥ N lines since date (start_date, min_lines)

## Stored routines (used in report3_routines.py)
- Procedure sp_seller_revenue_since(p_start_date DATE, p_min_revenue DECIMAL)
- Returns each seller’s revenue (SUM of order_item.item_total) on/after the date, filtered by a minimum revenue.
- Function fn_order_lines_total(p_order_id INT) RETURNS DECIMAL
- Returns the sum of line totals for a single order.


## GUI

- A single-window Tkinter app with tabs:
- Daily Sales (by day & seller): orders, units, revenue in a date range
- Top Customers: top spenders in a date range
- In-Transit Aging: days since shipped_at for undelivered shipments
- Inventory: on-hand, restock threshold, low-stock flags

<img width="500" height="500" alt="image" src="https://github.com/user-attachments/assets/9eff9f83-8d4d-4ef7-a6e0-4904bf854c78" />


## Diagram
<img width="500" height="500" alt="Screenshot 2025-11-28 215208" src="https://github.com/user-attachments/assets/7223c34c-a50e-49d3-8d19-d6ca54202c7f" />

## ETC Screenshots
<img width="400" height="400" alt="Screenshot 2025-11-28 215817" src="https://github.com/user-attachments/assets/3a8e5c9d-3dd4-4787-9263-6bf93e17954d" />
<img width="400" height="400" alt="Screenshot 2025-11-28 215824" src="https://github.com/user-attachments/assets/cc15994e-3282-4951-9ebe-dbc12afb0288" />


## Sample results & Notes 
- Orders placed by five demo customers; two sellers (GadgetWorld, HomeHub)
- Categories include Electronics, Audio, Accessories, Home
- Typical outputs show:
- Seller revenue ≥ 0 since 2025-10-01
- Avg category prices (e.g., Electronics ~ 1074.00 in the seed dataset)
- Orders with ≥ 2 lines and line totals matching order subtotal

## Notes
- price_at_sale on order_item snapshots pricing (later listing changes don’t affect history)
- item_total = qty × price_at_sale, total = subtotal + tax
- One payment and one shipment per order (simplified MVP)






