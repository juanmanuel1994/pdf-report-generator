# PDF Report Generator

Generate professional, multi-page PDF reports from CSV or JSON data using Python and ReportLab.

## Features

- **Cover page** — title, subtitle, company name, author, date, optional logo
- **KPI cards row** — auto-detected numeric columns displayed as big-number cards
- **Formatted data table** — striped rows, header highlighting, pagination notice
- **Bar chart** — configurable numeric column vs. label column
- **Line chart** — up to 3 series on a single chart for trend analysis
- **Executive summary** — count / total / average / min / max per numeric column
- **Header & footer** — report title, page number, date on every page
- **Fully customizable** — colors, title, subtitle, logo, author, company via CLI flags

---

## Requirements

```
Python 3.10+
reportlab>=4.0
```

Install dependencies:

```bash
pip install reportlab
```

---

## Quick Start

### Run the built-in demo

```bash
python report_generator.py --demo -o demo_report.pdf
```

This generates `demo_report.pdf` using the included `sales_data.csv` (12-month sales dataset).

### Use your own CSV

```bash
python report_generator.py my_data.csv -o my_report.pdf -t "Q3 Report" -s "Regional Performance"
```

### Use your own JSON

```bash
python report_generator.py my_data.json -o my_report.pdf
```

---

## CLI Reference

```
usage: report_generator.py [-h] [-o OUTPUT] [-t TITLE] [-s SUBTITLE]
                            [--logo LOGO] [--author AUTHOR] [--company COMPANY]
                            [--primary-color PRIMARY_COLOR]
                            [--secondary-color SECONDARY_COLOR]
                            [--accent-color ACCENT_COLOR]
                            [--label-column LABEL_COLUMN]
                            [--bar-column BAR_COLUMN]
                            [--line-columns LINE_COLUMNS [LINE_COLUMNS ...]]
                            [--max-rows MAX_ROWS] [--demo]
                            data
```

| Argument | Default | Description |
|---|---|---|
| `data` | *(required)* | Path to `.csv` or `.json` data file |
| `-o / --output` | `report.pdf` | Output PDF file path |
| `-t / --title` | `Sales Report` | Report title |
| `-s / --subtitle` | *(empty)* | Report subtitle |
| `--logo` | *(none)* | Path to PNG/JPG logo image |
| `--author` | `Report Generator` | Author name shown on cover and footer |
| `--company` | *(empty)* | Company name shown on cover page |
| `--primary-color` | `#1a3c5e` | Primary hex color (headers, background) |
| `--secondary-color` | `#2e86c1` | Secondary hex color (charts, borders) |
| `--accent-color` | `#f39c12` | Accent hex color (dividers, highlights) |
| `--label-column` | *(auto)* | Column to use as X-axis / row labels |
| `--bar-column` | *(auto)* | Numeric column to plot as bar chart |
| `--line-columns` | *(auto)* | Numeric columns to plot as line chart (space-separated) |
| `--max-rows` | `50` | Maximum rows to display in the data table |
| `--demo` | `false` | Ignore `data` arg, use built-in demo dataset |

---

## Examples

### Custom colors and logo

```bash
python report_generator.py sales_data.csv \
  -o branded_report.pdf \
  -t "Acme Corp — 2024 Annual Report" \
  -s "Fiscal Year Summary" \
  --company "Acme Corporation" \
  --author "Finance Team" \
  --logo logo.png \
  --primary-color "#0d2137" \
  --secondary-color "#e74c3c" \
  --accent-color "#2ecc71"
```

### Choose specific columns for charts

```bash
python report_generator.py sales_data.csv \
  --label-column month \
  --bar-column revenue \
  --line-columns revenue expenses profit
```

### Limit table rows for large datasets

```bash
python report_generator.py big_data.csv --max-rows 100 -o big_report.pdf
```

---

## Data Format

### CSV

Any CSV file with a header row. Numeric columns are auto-detected.

```csv
month,revenue,expenses,profit
January,124500,78200,46300
February,138200,81500,56700
```

### JSON

Either a flat list:

```json
[
  {"month": "January", "revenue": 124500, "expenses": 78200},
  {"month": "February", "revenue": 138200, "expenses": 81500}
]
```

Or a wrapped object:

```json
{
  "data": [
    {"month": "January", "revenue": 124500}
  ]
}
```

Supported wrapper keys: `data`, `records`, `rows`, `items`.

---

## Project Structure

```
pdf-report-generator/
├── report_generator.py   # Main script
├── sales_data.csv        # Demo dataset (12-month sales)
├── sales_data.json       # Same dataset in JSON format
└── README.md             # This file
```

---

## Using as a Module

```python
from report_generator import generate_report

generate_report(
    data_path="sales_data.csv",
    output_path="report.pdf",
    title="My Custom Report",
    subtitle="Q4 2024",
    author="Data Team",
    company="My Company",
    primary_color="#1a3c5e",
    accent_color="#e74c3c",
    label_column="month",
    bar_column="revenue",
    line_columns=["revenue", "expenses", "profit"],
)
```

---

## License

MIT — free to use and modify.
