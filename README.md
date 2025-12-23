# Expense Tracker MCP Server

A local Expense Tracker tool built using FastMCP, Python, and SQLite, fully integrated with Claude Desktop MCP.
It allows Claude to add, list, and summarize your expenses using simple MCP tool calls.
---
# 🚀 Features

Add expenses with date, amount, category, subcategory, and notes

List expenses between two dates

Summarize total spending by category

Read categories/subcategories from a JSON resource

Local data storage using SQLite (expenses.db)

Works directly inside Claude Desktop using MCP
---
# 🧩 Tech Stack

Python 3.10+

FastMCP

SQLite

UV (optional)

Claude Desktop MCP Protocol

# Project Structure
```markdowm
ExpenseTracker/
│
├── main.py
├── categories.json
├── expenses.db        # auto-created at runtime
├── README.md
└── requirements.txt (optional)
```

# 📝 MCP Tools Implemented
### 🔹 add_expense()

Adds a new expense to the database.
```python
@mcp.tool()
def add_expense(date, amount, category, subcategory="", note=""):
```
### 🔹 list_expenses()

Returns all expenses between start_date and end_date.
```python
@mcp.tool()
def list_expenses(start_date, end_date):
```

### 🔹 summarize()

Returns category-wise total spending.
```python
@mcp.tool()
def summarize(start_date, end_date, category=None):
```
### 🔹 MCP Resource: expense://categories

Reads categories from a JSON file.
```python
@mcp.resource("expense://categories", mime_type="application/json")
def categories():
    return open(CATEGORIES_PATH).read()
```
⚙️ Installation

You can install dependencies using UV or pip.

### 🟦 Option 1 — Using UV (Recommended)
```bash
uv init .
uv add fastmcp
```

Run development server:
```bash
uv run fastmcp dev main.py
```

Run normal server:
```bash
uv run fastmcp main.py
```
### 🟧 Option 2 — Using pip
```terminal
pip install fastmcp
python main.py
```
🔌 Claude Desktop Integration

To connect this MCP server with Claude Desktop, edit:

claude_desktop_config.json


Add:
```
{
  "mcpServers": {
    "ExpenseTracker": {
      "command": "uv",
      "args": [
        "run",
        "--with",
        "fastmcp",
        "fastmcp",
        "run",
        "path/to/main.py"
      ],
      "transport": "stdio"
    }
  }
}
```


Then install using:
```bash
uv run fastmcp install claude-desktop main.py
````

▶️ Usage Examples with Claude Desktop
### Add an Expense
use: ExpenseTracker
add_expense(
  date="2025-11-23",
  amount=500,
  category="Food & Dining",
  subcategory="Cafe",
  note="Starbucks"
)

### List Expenses
list_expenses("2025-10-01", "2025-11-23")

### Summarize Spending
summarize("2025-10-01", "2025-11-23")


Example output:
```
Shopping          ₹6300
Food & Dining     ₹3450
Healthcare        ₹3450
Utilities         ₹3300
Housing           ₹5000
```

# 🧠 How It Works (Short Explanation)

The server starts using FastMCP.

SQLite DB (expenses.db) is created if missing.

Claude Desktop loads the MCP server through STDIO transport.

Claude can now call:

add_expense

list_expenses

summarize

expense://categories resource

All expenses are stored locally and returned as JSON.


# 🏁 Final Notes

This project demonstrates integration between:

Python eco-system

Local databases

Claude Desktop MCP tools

Agent-like expense tracking

A great real-world, developer-focused MCP demo project.
