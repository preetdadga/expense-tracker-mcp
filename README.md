# Expense Tracker MCP

A small FastMCP server for recording, updating, deleting, listing, and summarizing expenses in a local SQLite database.

## Features

- Add expense entries with date, amount, category, subcategory, and note
- Update or delete existing expense entries by id
- List expenses for an inclusive date range
- Summarize expenses by category for an inclusive date range
- Expose an editable JSON category catalog through an MCP resource

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

The database table is created with these fields:

- `id`: Auto-incrementing expense id
- `date`: Expense date as text, preferably `YYYY-MM-DD`
- `amount`: Expense amount as a number
- `category`: Main category
- `subcategory`: Optional subcategory
- `note`: Optional note

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

Returns:

```json
{
  "status": "ok",
  "id": 1
}
```

### `update_expense`

Updates an existing expense entry by id.

Arguments:

- `id`: Existing expense id
- `date`: Date string, preferably `YYYY-MM-DD`
- `amount`: Expense amount
- `category`: Main category
- `subcategory`: Optional subcategory
- `note`: Optional note

Returns `{"status": "ok", "id": ...}` when the row is updated, or `{"status": "not_found", "id": ...}` when no expense exists with that id.

### `delete_expense`

Deletes an existing expense entry by id.

Arguments:

- `id`: Existing expense id

Returns `{"status": "ok", "id": ...}` when the row is deleted, or `{"status": "not_found", "id": ...}` when no expense exists with that id.

### `list_expenses`

Lists expenses between `start_date` and `end_date`, inclusive.

Arguments:

- `start_date`: Start date, inclusive
- `end_date`: End date, inclusive

Returns expense entries ordered by `id` ascending.

### `summarize`

Summarizes expenses by category between `start_date` and `end_date`, inclusive. Optionally filter by a single category.

Arguments:

- `start_date`: Start date, inclusive
- `end_date`: End date, inclusive
- `category`: Optional category filter

Returns rows with `category` and `total_amount`.

## Resources

### `expense://categories`

Returns the contents of `categories.json` as JSON.

The file is read fresh on each resource request, so you can edit `categories.json` without restarting the MCP server. The current catalog includes categories such as `food`, `transport`, `housing`, `utilities`, `health`, `education`, `business`, `travel`, `investments`, `insurance`, `loans_debt`, and `misc`, each with a list of subcategories.
