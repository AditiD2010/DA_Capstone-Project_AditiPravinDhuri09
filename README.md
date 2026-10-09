# DA_Capstone-Project_AditiPravinDhuri09

# Mamaearth Regional Sales & Returns Analytics Pipeline

## Project Overview
This project analyzes a sample e-commerce dataset inspired by Mamaearth to understand sales performance, customer spending, payment-method return patterns, and unusual transactions. It combines SQL-based reporting with Python data cleaning and exploratory analysis, then organizes key findings into a business narrative using the **Situation--Complication--Resolution (SCR)** framework. The workflow consists of:

1. Creating and populating a relational database.
2. Running analytical SQL reports.
3. Cleaning and analyzing transactional data using Python.
4. Generating visualizations.
5. Producing verified business metrics in JSON format.
6. Creating a Situation–Complication–Resolution (SCR) business narrative using Gemini AI or an offline fallback.


## Project objectives

The main objective is to demonstrate an end-to-end analytical approach to a retail sales and returns problem.

1.  Explore customer, product, and order data using SQL.
2.  Calculate revenue and customer-spending metrics.
3. Identify missing, inconsistent, and potentially duplicated records.
4.  Standardize payment-method labels and investigate unusual order
    quantities.
5.  Compare return rates by payment method and customer segment.
6.  Examine monthly revenue and investigate the effect of unusual
    transactions.
7.  Package verified findings into a concise, decision-oriented SCR
    narrative.
8.  Provide a deterministic offline narrative option when a generative.


 # Business questions
 How are product returns impacting our bottom-line profitability across different regions and order categories, and what is our true net revenue once return-   related data anomalies and leakage are fully accounted for? Specifically, this analysis aims to identify the root causes and hidden patterns behind return rates—pinpointing high-risk regions, product lines, or customer segments—and translate these findings into actionable, data-backed operational and financial strategies to protect overall profit margins.


 ## Tools and technologies
 -----------------------------------------------------------------------
  Tool                                Use in this project
  ----------------------------------- -----------------------------------
  SQL / MySQL-compatible database     Relational tables, joins,
                                      aggregations, segmentation, and
                                      reports

  Python                              Data preparation, calculations,
                                      quality checks, and narrative logic

  Pandas                              Data loading, cleaning,
                                      transformation, and exploratory
                                      analysis

  NumPy                               Numerical operations where required

  Matplotlib                          Visualization

  Jupyter Notebook / Google Colab     Interactive analysis and
                                      experimentation

  Google Gemini API (`google-genai`)  Optional AI-generated SCR narrative

  JSON                                Structured findings and narrative
                                      output


# Repository Structure
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

   
## Workflow
The intended analytical workflow has five stages:

1.  **Database setup and SQL reporting** --- define the customer,
    product, and order tables; load data; run queries for revenue,
    customers, categories, missing ratings, and returns.
2.  **Data inspection and cleaning** --- inspect data types and missing
    values, standardize text fields such as payment method, and
    investigate duplicate-looking records.
3.  **Exploratory analysis** --- calculate revenue, return rates,
    customer-segment metrics, and monthly trends.
4.  **Findings preparation** --- organize key metrics into a consistent
    JSON structure for downstream reporting.
5.  **Business narrative** --- generate a
    Situation--Complication--Resolution summary using Gemini when
    configured, or use a deterministic offline fallback.


## Key findings
The following values are recorded in the project's findings notebook. They should be treated as **reported project outputs** and revalidated against the final cleaned dataset and code before being used for business decisions.

  Metric                                        Reported result
  ------------------------------------------ ------------------
  Raw revenue                                        ₹99,860.20
  Cleaned revenue                                    ₹97,358.30
  Duplicate-reconciliation difference                 ₹2,501.90
  COD return rate                                         44.4%
  Card return rate                                        14.7%
  UPI return rate                                         18.9%
  Highest-risk segment identified              COD, city tier 2
  Return rate for the identified segment                  54.5%
  Reported peak month after outlier review           March 2026
  Reported March revenue                             ₹20,318.90
  January 2026 apparent revenue                      ₹29,582.10
  January 2026 corrected revenue                     ₹11,637.10


  ### Interpretation

-   The reported return rate is highest for **Cash on Delivery (COD)**
    among the payment methods examined.
-   The analysis flags **COD orders from city tier 2** as a segment for
    further investigation.
-   The monthly revenue review identifies January as a month where
    unusual bulk-order quantities may materially affect the apparent
    result.
-   Cleaning changes reported revenue, so raw and cleaned figures should
    be shown separately and the reconciliation rule should be
    documented.

These findings describe patterns in this dataset; they do **not** prove that a payment method or city tier causes returns. Sample sizes, order-level context, and the duplicate/outlier rules should be reviewed before operational decisions are made.


## Business recommendations

Based on the reported results, the following are reasonable next steps
for a business team:

1.  **Investigate COD returns:** review return reasons, delivery
    failures, cancellations, and customer confirmation processes for COD
    orders.
2.  **Segment the investigation:** examine COD orders from city tier 2
    separately, while checking the number of orders in that segment so
    that rates are not interpreted without context.
3.  **Validate unusual transactions:** verify high-quantity orders
    against source records before excluding or adjusting them. A large
    order may be legitimate rather than an error.
4.  **Make data-quality rules auditable:** record how duplicates are
    identified, which rows are removed, and how the revenue
    reconciliation difference is calculated.
5.  **Monitor monthly performance:** compare cleaned revenue across
    months and retain a clear record of any adjustments.
6.  **Use validated metrics in reporting:** generate business narratives
    from a single verified findings file rather than manually retyping
    figures in multiple places.


## Data dictionary
The relational model contains three logical entities:

### Customers

  Field                  Description
  ---------------------- --------------------------------------------------
  `customer_id`          Unique customer identifier
  `name`                 Customer name in the sample dataset
  `city`                 Customer city
  `city_tier`            City-tier grouping used for segmentation
  `signup_date`          Customer registration date
  `acquisition_source`   Recorded customer acquisition channel
  `loyalty_tier`         Derived segment in the SQL exercise, where added

### Products

  Field            Description
  ---------------- ---------------------------
  `product_id`     Unique product identifier
  `product_name`   Product name
  `category`       Product category
  `price`          Unit price in INR

### Orders

  -----------------------------------------------------------------------
  Field                               Description
  ----------------------------------- -----------------------------------
  `order_id`                          Unique order identifier

  `customer_id`                       Customer reference

  `product_id`                        Product reference

  `order_date`                        Order date

  `quantity`                          Quantity ordered

  `discount_pct`                      Discount percentage; missing values
                                      are treated as zero for the stated
                                      revenue formula

  `payment_method`                    Payment method, standardized for
                                      analysis

  `rating`                            Customer rating; may be missing

  `returned`                          Return indicator used by the
                                      analysis
  -----------------------------------------------------------------------



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


## Limitations

-   The project uses a sample dataset and should not be interpreted as
    an official statement about Mamaearth's actual business performance.
-   Return-rate differences are descriptive and do not establish
    causation.
-   Revenue figures depend on the treatment of discounts, duplicates,
    unusual quantities, and returns.
-   The duplicate-reconciliation difference should not be interpreted as
    confirmed lost revenue unless the underlying records and
    deduplication rule have been independently validated.
-   Gemini-generated text can omit or misstate information; generated
    narratives must be checked against the source findings.
-   API-based narrative generation requires a valid key and may depend
    on quota, connectivity, and service availability.


## Author
**Aditi Pravin Dhuri**\
MBA FinTech Forensics student \| Aspiring Data Analyst

This capstone demonstrates applied learning in SQL, Python-based data analysis, data quality, visualization, and AI-assisted business reporting.

