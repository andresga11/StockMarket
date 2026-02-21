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

`(user_id, stock_symbol)`


### Attributes

- `sharesOwned` (BigDecimal, required, >= 0)
- `averageCostPerShare` (optional but recommended)
- `updatedAt` (timestamp)

### Supported Functions

- Retrieve all positions for a user
- Retrieve position by `(user, stock)`
- Compute portfolio market value

---

## 4️⃣ Trades (Transaction History)

Trades are immutable records of buy/sell events.

Each trade belongs to exactly:

- One user
- One stock

### Attributes

- `type` (BUY / SELL)
- `shares` (BigDecimal, required, > 0)
- `executionPrice` (BigDecimal, required)
- `totalValue` (shares × price)
- `executedAt` (timestamp, required)
- `status` (FILLED / REJECTED)

### Supported Functions

- Retrieve trade history for a user
- Filter trades by stock symbol and date range

---

## 5️⃣ Balance Movements (Ledger) — Optional but Recommended

Tracks all cash movements for auditing and traceability.

Each ledger entry belongs to exactly one user.

### Attributes

- `type` (DEPOSIT, WITHDRAWAL, BUY_DEBIT, SELL_CREDIT, FEE)
- `amount` (BigDecimal, required)
- `createdAt` (timestamp)
- `referenceTradeId` (optional foreign key)

---

# 💼 Business Rules

## Buy Stock

The system shall allow a user to buy a stock using their available cash balance.

### Validations

- User exists
- Stock exists
- `shares > 0`
- User has sufficient balance for `(shares × currentPrice)`

### Atomic Operations

1. Create Trade (BUY)
2. Decrease user balance
3. Upsert Position:
   - Increase shares
   - Update average cost (if tracked)

---

## Sell Stock

The system shall allow a user to sell shares they own.

### Validations

- `shares > 0`
- User owns sufficient shares in Position

### Atomic Operations

1. Create Trade (SELL)
2. Increase user balance
3. Decrease Position shares  
   - Delete position if shares becomes 0

---

# 🔒 Integrity & Constraints

- `User.username` must be unique and not null
- `Stock.symbol` must be unique and not null
- `Position` must be unique on `(user_id, stock_symbol)`
- Position shares cannot be negative
- Monetary fields use `BigDecimal` with consistent scale
- Trades are append-only (no updates/deletes)
- Buy/sell operations must be transactional

---

# 📈 Reporting & Queries

The system must support:

- Dashboard view (user info + balance + portfolio)
- Holdings with market value and unrealized P/L
- Trade history per user
- Trade history per stock
- Top movers (optional)
- Admin stock management (optional)

---


