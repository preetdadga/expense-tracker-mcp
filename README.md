# Expense Tracker MCP

A small FastMCP server for recording, listing, and summarizing expenses in a local SQLite database.

## Features

- Add expense entries with date, amount, category, subcategory, and note
- List expenses for an inclusive date range
- Summarize expenses by category for an inclusive date range
- Expose a JSON category list through an MCP resource

## Requirements

- Python 3.13 or newer
- `uv`

## Setup

Install dependencies:

```powershell
uv sync
```

Run the MCP server:

```powershell
uv run python main.py
```

The server creates a local `expenses.db` SQLite database automatically when it starts. This file is ignored by Git because it contains local/private expense data.

## Claude Desktop Example

Add an MCP server entry similar to this in your Claude Desktop config:

```json
{
  "mcpServers": {
    "expense-tracker": {
      "command": "uv",
      "args": [
        "--directory",
        "E:\\Work\\MCP\\Expense-Tracker-mcp",
        "run",
        "python",
        "main.py"
      ]
    }
  }
}
```

Adjust the path if you clone this repository somewhere else.

## Tools

### `add_expense`

Adds a new expense entry.

Arguments:

- `date`: Date string, preferably `YYYY-MM-DD`
- `amount`: Expense amount
- `category`: Main category
- `subcategory`: Optional subcategory
- `note`: Optional note

### `list_expenses`

Lists expenses between `start_date` and `end_date`, inclusive.

### `summarize`

Summarizes expenses by category between `start_date` and `end_date`, inclusive. Optionally filter by a single category.

## Resources

### `expense://categories`

Returns the contents of `categories.json` as JSON.
