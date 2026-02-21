# StockMarket

![Stock Market](https://github.com/andresga11/StockMarket/blob/main/database_diagram.drawio.png)


# 📊 Stock Market Trading System

A backend trading system built with Spring Boot and JPA that models users, stocks, positions, and trades with proper transactional integrity and financial precision.

---

# 📌 Information Requirements

## 1️⃣ Users and Accounts

The system shall store users with a unique identifier.

### Attributes

- `id` (Primary Key)
- `username` (unique, required)
- `cashBalance` (BigDecimal, required)
- `accountStatus` (ACTIVE / DISABLED)
- `createdAt` (timestamp)

### Supported Functions

- Retrieve user by `id`
- Retrieve user by `username`

---

## 2️⃣ Stocks (Securities)

The system shall store stocks identified by a unique symbol (e.g., `"AAPL"`).

### Attributes

- `symbol` (Primary Key, unique, required)
- `companyName` (required)
- `currentPrice` (BigDecimal, required)
- `previousClose` (optional)
- `lastUpdated` (timestamp)

### Supported Functions

- List all stocks
- Retrieve stock by `symbol`

---

## 3️⃣ Positions (Current Holdings)

A position represents a user’s holdings in a specific stock.

Each position uniquely represents:

> One user + one stock

Uniqueness is enforced on:

