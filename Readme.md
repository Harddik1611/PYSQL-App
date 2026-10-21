# PYSQL App: Inventory & Supply Chain Dashboard

A Streamlit web app that lets non-technical users view and manage a MySQL inventory database without writing any SQL Queries.

---

## Overview

Managers and team leads often need to check stock levels, add products, or confirm that a supplier shipment has arrived, but the data lives in a MySQL database that only people who know SQL can use. This project puts a simple web interface on top of that database so those tasks take a few clicks.

The app connects to a MySQL database called `supply_chain` and calls its tables and stored procedures from Python.

## Features

**Dashboard (Basic Information)**
- KPI cards: total suppliers, total products, product categories, sale value, restock value, and the number of products below their reorder level with no reorder pending
- Supplier contact details
- Products with their supplier and current stock, sorted from lowest stock up
- Products that need reordering (stock at or below reorder level)

**Operational tasks**
- **Add New Product:** form-based entry; the product is created through the `AddNewProdcutManualId` stored procedure
- **Product History:** pick a product and view its inventory history, most recent first
- **Place Reorder:** choose a product and quantity; creates a reorder with status `Ordered`
- **Receive Reorder:** mark a pending reorder as received through the `MarkReorderAsReceived` stored procedure

## Tech Stack

| Layer | Technology |
|-------|------------|
| Frontend | Streamlit |
| Backend | Python, `mysql-connector-python` |
| Data handling | pandas |
| Database | MySQL (tables + stored procedures) |

## Project Structure

```
PYSQL-App/
├── app.py                # Streamlit UI (dashboard + operational screens)
├── db_functions.py       # Database connection and query/procedure wrappers
├── sql_connector.ipynb   # Notebook for testing the MySQL connection and queries
├── requirements.txt      # Python dependencies
├── data/                 # Project data
└── .devcontainer/        # Dev container configuration
```

## Database

The app expects a MySQL database named **`supply_chain`** containing:

| Table | Purpose |
|-------|---------|
| `products` | Product catalog: name, category, price, stock quantity, reorder level, supplier |
| `suppliers` | Supplier name, contact person, email, phone |
| `reorders` | Reorder records: product, quantity, date, status (`Ordered` / `Received`) |
| `stock_entries` | Stock movements (e.g. `Sale`, `Restock`) with quantity change and date |
| `product_inventory_history` | Historical stock records per product |

**Stored procedures used by the app**

- `AddNewProdcutManualId(name, category, price, stock_quantity, reorder_level, supplier_id)`
- `MarkReorderAsReceived(reorder_id)`

> Add your schema and sample data here (for example `data/schema.sql`) so others can recreate the database, then update the setup step below to point to it.

## Getting Started

### Prerequisites

- Python 3.8 or higher
- MySQL server (running locally or reachable over the network)

### Installation

1. **Clone the repository**
   ```bash
   git clone https://github.com/Harddik1611/PYSQL-App.git
   cd PYSQL-App
   ```

2. **Install dependencies**
   ```bash
   pip install -r requirements.txt
   ```

3. **Set up the database**
   Create the `supply_chain` database, its tables, and the two stored procedures listed above, then load your data.

4. **Configure the connection**
   Open `db_functions.py` and set your own MySQL host, user, password and database name in `connect_to_db()`.
   Do not commit real passwords to a public repository.

5. **Run the app**
   ```bash
   streamlit run app.py
   ```
   Then open <http://localhost:8501> in your browser.

## Usage

1. Choose **Basic Information** in the sidebar to see the dashboard.
2. Choose **Operational Task**, then pick a task from the dropdown:
   - *Add New Product*: fill in the form and submit
   - *Product History*: select a product to see its history
   - *Place Reorder*: select a product, enter a quantity, and place the order
   - *Receive Reorder*: select a pending order and mark it as received

## Roadmap

- [ ] Move database credentials to environment variables (`.env`)
- [ ] Include the schema and sample data in the repository
- [ ] User authentication and role-based access
- [ ] Search and filtering
- [ ] Export to CSV/Excel
- [ ] Charts for stock and sales trends
- [ ] Low-stock email notifications

## Contributing

Issues and pull requests are welcome. For larger changes, please open an issue first to discuss what you would like to change.

## License

No license has been added yet. Add a `LICENSE` file (for example MIT) to make the terms of use clear.

## Author

**Harddik** ([@Harddik1611](https://github.com/Harddik1611))
