# DA_Capstone-Project_AditiPravinDhuri09

# Mamaearth Regional Sales & Returns Analytics Pipeline

## Project Overview

This project analyzes Mamaearth sales, returns, and customer behavior using SQL and Python. The workflow consists of:

1. Creating and populating a relational database.
2. Running analytical SQL reports.
3. Cleaning and analyzing transactional data using Python.
4. Generating visualizations.
5. Producing verified business metrics in JSON format.
6. Creating a Situation–Complication–Resolution (SCR) business narrative using Gemini AI or an offline fallback.

The repository is structured so that every result can be reproduced from the provided source files.

---

# Repository Structure

```text
<repo>/
├── README.md
├── sql/
│   ├── schema.sql
│   ├── seed_data.sql
│   └── reports.sql
├── data/
│   ├── customers.csv
│   ├── products.csv
│   └── orders.csv
├── analysis/
│   ├── clean_and_eda.py
│   └── visualize.py
├── visualizations/
│   ├── return_rate_by_payment.png
│   └── monthly_revenue_trend.png
└── narrator/
    ├── findings.json
    └── generate_narrative.py
```

---

# Prerequisites

Install the required Python libraries before running the analysis.

```bash
pip install pandas numpy matplotlib
```

For Gemini narrative generation:

```bash
pip install google-genai
```

A MySQL-compatible database is required for executing the SQL scripts.

---

# Step 1 – Create and Populate the Database

Navigate to the `sql/` folder and execute the scripts in the following order.

## 1. Create Tables

Run:

```sql
SOURCE sql/schema.sql;
```

This script creates all required tables and relationships.

## 2. Load Sample Data

Run:

```sql
SOURCE sql/seed_data.sql;
```

This script inserts the customer, product, and order records used throughout the project.

## 3. Run Analytical Reports

Run:

```sql
SOURCE sql/reports.sql;
```

The report script generates analytical outputs including:

* Revenue analysis
* Customer spending analysis
* Product category performance
* Return analysis
* Loyalty segmentation
* Business reporting metrics

All SQL results should execute successfully before moving to the Python analysis stage.

---

# Step 2 – Run Data Cleaning and EDA

Navigate to the project root directory and execute:

```bash
python analysis/clean_and_eda.py
```

This script:

* Loads customer, product, and order datasets
* Cleans missing and inconsistent values
* Calculates cleaned revenue
* Calculates return rates
* Identifies high-risk customer segments
* Produces all metrics required for the business narrative

## Output

The script writes:

```text
narrator/findings.json
```

This file is the primary output of Part 2 Task 5 and contains the verified metrics used later by the narrative generator.

---

# Step 3 – Generate Visualizations

Execute:

```bash
python analysis/visualize.py
```

This script reads the cleaned analytical results and generates visual outputs.

## Outputs

```text
visualizations/return_rate_by_payment.png
visualizations/monthly_revenue_trend.png
```

These charts summarize:

* Return rates across payment methods
* Monthly revenue trends

---

# Step 4 – Generate the Business Narrative

The narrative generator converts the verified findings into a business summary using the Situation–Complication–Resolution (SCR) framework.

Execute:

```bash
python narrator/generate_narrative.py
```

The script automatically reads:

```text
narrator/findings.json
```

and generates a business narrative.

---

# Gemini API Mode (Online)

To use Gemini, create an API key from Google AI Studio and set it as an environment variable.

## Windows PowerShell

```powershell
$env:GEMINI_API_KEY="YOUR_API_KEY"
python narrator/generate_narrative.py
```

## Windows Command Prompt

```cmd
set GEMINI_API_KEY=YOUR_API_KEY
python narrator/generate_narrative.py
```

## macOS / Linux

```bash
export GEMINI_API_KEY="YOUR_API_KEY"
python narrator/generate_narrative.py
```

When a valid key is available, the script sends the findings to Gemini and generates an AI-written SCR narrative.

---

# Offline Mode (No API Key Required)

The project includes a fully offline fallback path.

Simply run:

```bash
python narrator/generate_narrative.py
```

without defining the `GEMINI_API_KEY` environment variable.

In this mode:

* No network access is required.
* No API key is required.
* No paid service is required.
* The script generates a deterministic SCR narrative using the verified findings in `findings.json`.

This ensures the repository remains fully reproducible and gradable even when internet access or Gemini API access is unavailable.

---

# Reproducibility Checklist

Follow these steps in order:

1. Run `sql/schema.sql`
2. Run `sql/seed_data.sql`
3. Run `sql/reports.sql`
4. Run `analysis/clean_and_eda.py`
5. Run `analysis/visualize.py`
6. Verify  `narrator/findings.json` 
7. Run `narrator/generate_narrative.py`
8. Generate the SCR narrative using either:

   * Gemini API mode, or
   * Offline fallback mode

Following the above sequence allows a new user to reproduce all project outputs, metrics, visualizations, and narrative results from the repository contents alone.

