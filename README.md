
# 📘 PL-300 Power BI Data Analyst — Illustrated Study Guide (2025 Edition)

# Install packages 

This command is a terminal prompt

# Publishing to PDF with Pandadoc and formatting after setting directory in terminal 

# Changing the PATH cd
cd ~/Documents && FILE=$(find . -name "PL300_Study_Guide_2025.md" -print -quit) && python3 -c "import pypandoc,os; f='$FILE'; pdf=f[:-3]+'.pdf'; pypandoc.convert_file(f,'pdf',outputfile=pdf,extra_args=['--standalone']); print('✅ Exported to', os.path.abspath(pdf))"

# Publishing the code PDF
{
  "key": "cmd + Shift + e",
  "command": "markdown-pdf.export"
}


---
### 🎨 Color Code Legend
🔵 Prepare & Model Data  
🟢 DAX & Context  
🟠 Visualize & Analyze  
🟣 Deploy & Govern  
🧩 Reference & Exam Tips

---

## 🔵 1. Exam Overview & Strategy

**Exam Structure:** 60 questions in 90 minutes.  
**Domains:**  
- Prepare the Data (15–20%)  
- Model the Data (30–35%)  
- Visualize & Analyze (25–30%)  
- Deploy & Maintain Assets (20–25%)  

💡 *Tip:* No penalty for guessing—always answer every question.

⚠️ *Exam Trap:* Watch for wording differences like “best meets the requirement” vs “most efficient solution.”

---

## 🔵 2. Prepare & Model Data

**Power Query Workflow:** Connect → Transform → Clean → Load  

| Task | Function | Description |
|------|-----------|--------------|
| Combine Files | Append Queries | Add rows (stack datasets) |
| Join Tables | Merge Queries | Add columns based on matching key |
| Reorder Columns | Table.ReorderColumns | Organize data for readability |

💡 *Tip:* Always use *Transform Data* before loading to ensure data types match.

**Star Schema Example:**  
Fact Table: `Sales`  
Dimension Tables: `Product`, `Customer`, `Date`  
Relationship: `Product[ProductID] → Sales[ProductID]`

⚠️ *Exam Trap:* Avoid many-to-many relationships—prefer one-to-many (1:*).

---

## 🔵 3. Relationships & Cardinality

- **One-to-Many (1:*)**: Most common (e.g., One Product → Many Sales).  
- **Both-Direction Filter:** Use only for role-playing dimensions.  
- **Inactive Relationships:** Reactivate using `USERELATIONSHIP()` in DAX.

💡 *Tip:* Always check cross-filter direction when debugging unexpected results.

📝 **Mini Exam Q:**  
> The Finance and Sales reports share a Date table. You need a single slicer for both.  
✅ Enable **bi-directional filtering** between Date and Sales.

---

## 🟢 4. Core DAX & Summary Stats

### Summary Statistics Functions

| Purpose | Function | Example |
|----------|-----------|----------|
| Sum | `SUM()` | `SUM(Sales[Amount])` |
| Average | `AVERAGE()` | `AVERAGE(Sales[Amount])` |
| Count Rows | `COUNTROWS()` | `COUNTROWS(Sales)` |
| Distinct Count | `DISTINCTCOUNT()` | `DISTINCTCOUNT(Customer[ID])` |
| Minimum/Maximum | `MIN()`, `MAX()` | `MIN(Product[Price])` |

### Iterators (Row-by-Row Calculations)
```DAX
Total Profit = SUMX(Sales, Sales[Qty] * (Sales[Price] - Sales[Cost]))
```
💡 *Use X-functions (SUMX, AVERAGEX) when calculations involve multiple columns.*

### Context & CALCULATE
```DAX
Profit 2024 =
CALCULATE(SUM(Sales[Profit]), YEAR(Sales[Date]) = 2024)
```
⚠️ *Exam Trap:* CALCULATE changes the filter context—it’s the most tested DAX function.

📝 **Mini Exam Q:**  
> Return total revenue ignoring slicers.  
✅ `CALCULATE(SUM(Sales[Amount]), ALL('Region'))`

---

## 🟢 5. Intermediate DAX & Time Intelligence

```DAX
Sales YTD = TOTALYTD([Total Sales], 'Date'[Date])
Sales LY = CALCULATE([Total Sales], SAMEPERIODLASTYEAR('Date'[Date]))
YoY Change = [Sales YTD] - [Sales LY]
```
💡 *Ensure your Date table is marked as a Date Table for these to work.*

📝 **Mini Exam Q:**  
> Show year-over-year growth trend.  
✅ Use `TOTALYTD` and `SAMEPERIODLASTYEAR`.

---

## 🟠 6. Visualize & Analyze

### Slicers vs Visuals
| Feature | Visual | Slicer |
|----------|--------|--------|
| Purpose | Display data | Filter visuals |
| Interaction | Passive | Interactive |
| Scope | Single visual | Page or synced visuals |

💡 *Page filters act like hidden slicers set by the author.*

📝 **Mini Exam Q:**  
> Analysts must toggle between profit and revenue metrics.  
✅ Use **Field Parameters**.

---

## 🟣 7. Deploy & Govern

### Workspace Roles
| Role | Permissions |
|------|--------------|
| Viewer | View content only |
| Contributor | Edit reports but not add users |
| Member | Add/edit users and content |
| Admin | Full access |

💡 *Dataset → Report → Dashboard → App* is the Power BI publishing flow.

⚠️ *Exam Trap:* Only Admins and Members can manage workspace access.

📝 **Mini Exam Q:**  
> A Contributor cannot share reports. Why?  
✅ Sharing requires Member or Admin privileges.

---

## 🧩 8. DAX Quick Reference Table

| Category | Function | Example |
|-----------|-----------|----------|
| Aggregation | SUM, AVERAGE | `SUM(Sales[Amount])` |
| Iteration | SUMX, AVERAGEX | `SUMX(Sales, Sales[Qty]*Sales[Price])` |
| Filter Context | CALCULATE, FILTER, ALL | `CALCULATE([Sales], ALL(Sales))` |
| Time | TOTALYTD, SAMEPERIODLASTYEAR | `TOTALYTD([Sales],'Date'[Date])` |
| Logic | IF, SWITCH | `SWITCH(TRUE(), [Val]>1000, "High", "Low")` |

💡 *Use `SWITCH(TRUE())` instead of nested IF for clarity.*

---

## 🟠 9. Slicer as Visual

- Slicers can act like visuals when styled as **tiles**, **buttons**, or **hierarchies**.  
- Example: Display available regions and allow filtering simultaneously.  
💡 *Use “Tile Layout” slicers for dashboards.*

📝 **Mini Exam Q:**  
> You need to show regions and let users select them interactively.  
✅ Use a **Tile-style slicer**.

---

## 🟠 10. Slicer & Filter Combinations

| Feature | Page Filter | Slicer |
|----------|--------------|---------|
| User Interaction | Hidden | Visible |
| Scope | Entire Page | Multiple visuals |
| Typical Use | Fixed context | User-driven filtering |

💡 *“Filters fix, slicers flex.”*

📝 **Mini Exam Q:**  
> Restrict report to Q1 2024 only, without user control.  
✅ Use **Page Filter**.

---

## 🧩 11. Exam Traps & Memory Tips

💡 **Memory Aids**
- “Visuals display, slicers slice, filters fix.”  
- “CALCULATE controls context.”  
- “ALL removes filters; FILTER refines them.”

⚠️ **Top Traps**
1. Forgetting to mark Date tables.  
2. Using both-direction filters unnecessarily.  
3. Mixing calculated columns with measures in DAX questions.  
4. Confusing Import vs DirectQuery features.

📝 **Final Practice Question:**  
> A report must show global sales totals unaffected by slicers. Which function clears filters?  
✅ **ALL()**

# 🎯 Power BI – Cross Filtering, Cross Highlighting & Relationship Types

> **PL-300 Quick Reference – Report Interactions & Data Model Directionality**

---

## 🧭 1️⃣ Cross Filtering vs Cross Highlighting

| Concept | Behavior | Visual Example | Use Case |
|----------|-----------|----------------|-----------|
| **Cross Filtering** | Selecting one visual **filters** another visual so only matching data remains visible. | Selecting **Victoria** on a map filters a bar chart to show **only Victoria’s** sales. | You want visuals to **fully update** to the selected context. |
| **Cross Highlighting** | Selecting one visual **highlights** matching portions in another visual — the rest stays visible but dimmed. | Selecting **Victoria** on a map highlights Victoria’s portion in each bar, other states remain faded. | You want to **compare** the selected values to the total. |

### 🧩 How to Configure
1. Click any visual.  
2. Ribbon → **Format** → **Edit interactions**.  
3. On the target visuals:  
   - 🪣 **Funnel icon** → Cross Filter  
   - 🎯 **Circle icon** → Cross Highlight  
   - 🚫 **No icon** → No interaction  

💡 *Slicers always apply filters — they never “highlight.”*

---

## 🔄 2️⃣ Relationship Direction (Filter Flow in Data Model)

| Relationship Type | Filter Flow | Default Use | Typical Example |
|--------------------|-------------|--------------|-----------------|
| **Single-direction (one-way)** | From lookup → fact | Default | `Products → Sales` |
| **Bi-directional (both)** | Filters flow both ways | Many-to-many or complex models | `Customers ↔ Regions` via `Sales` bridge |

# 🧮 VAR and RETURN in Complex Power BI Measures

The `VAR` keyword in DAX stores intermediate results (numbers, text, or tables) so you can reuse them later in the same measure.  
The `RETURN` block defines what expression to output after all `VAR` values are calculated.

---

## 🧠 Why Use VAR

- Simplifies complex logic  
- Improves readability and performance  
- Reuses results without recalculating them  
- Works perfectly in Table / Matrix visuals (each cell = its own filter context)

---

## 🧩 Syntax

DAX
<Measure Name> =
VAR <var1> = <expression1>
VAR <var2> = <expression2>
RETURN
<final_expression>

## 💡 Example 1 — Two VARs

```DAX
Profit Margin % =
VAR TotalSales = SUM(Sales[Amount])
VAR TotalCost  = SUM(Sales[Cost])
RETURN
DIVIDE(TotalSales - TotalCost, TotalSales)
```

## ⚙️ Example 2 — KPI Logic
```DAX
KPI Status =
VAR Sales = SUM(Sales[Amount])
VAR Target = AVERAGE(Targets[Goal])
RETURN
IF(Sales >= Target, "✅ Met", "❌ Missed")

## 🎯 PL-300 Tricky Power BI Exam Questions — Explanations, Comparisons & Common Mistakes

A complete revision sheet of the most commonly misunderstood Power BI exam concepts.
Includes detailed **explanations**, **DAX formulas**, **Power Query logic**, **visualization reasoning**, and **exam memory hacks**.

---

## 🧩 1. DAX — SAMEPERIODLASTYEAR and CALCULATE

## ✅ Example
```DAX
CALCULATE(SUM(Enrollments[Enrollments_amount]), SAMEPERIODLASTYEAR('Date'[Date]))
🧠 Explanation
SAMEPERIODLASTYEAR() shifts the date context one year back while preserving the same range (e.g., same month/week from last year).

CALCULATE() changes the context of a calculation, enabling you to aggregate using a new time period.

## ⚙️ How It Works
Function	Role
CALCULATE()	Re-evaluates an expression in a modified filter context.
SAMEPERIODLASTYEAR()	Returns a set of dates exactly one year before the current filter context.

## 🧠 Memory Trick
"CALCULATE" applies the logic,
"SAMEPERIODLASTYEAR" tells when to apply it.

## 🧩 2. DAX — PREVIOUSYEAR with COUNT
✅ Example
DAX
Copy code
CALCULATE(COUNT(Sales[SalesID]), PREVIOUSYEAR('Date'[Date]))

##🧠 Explanation
PREVIOUSYEAR() returns all dates from the previous calendar year, making it ideal for YoY goal or comparison measures.

## 📊 Use Case
If you want to create a goal measure that’s 10% higher than last year’s sales:

DAX
Copy code
Goal = CALCULATE(COUNT(Sales[SalesID]), PREVIOUSYEAR('Date'[Date])) * 1.1
⚠️ Common Mistake
Confusing PREVIOUSYEAR() with SAMEPERIODLASTYEAR().
The former fetches the entire previous year; the latter fetches the same range from the previous year.

## 🧩 3. FILTER + ALL — Isolating Context

✅ Example
DAX
Copy code
2019 Enrollments = CALCULATE([Total_Enrollments], FILTER(ALL('Date'), 'Date'[Year] = 2019))
🧠 Explanation
ALL() removes all filters applied to the 'Date' table, giving a clean slate.

The FILTER() function then reapplies only the year 2019.

## 🧩 Summary Table

Function	Description	Typical Use
ALL()	Ignores any filter context	Compare across entire dataset
FILTER()	Applies conditional logic	Select subsets dynamically

⚠️ Mistake
Using VALUES() instead of ALL() would limit filtering only to visible data, not the full dataset.

## 🧩 4. DATEADD — Comparing Current vs Previous Periods

✅ Example
DAX
Copy code
CALCULATE(SUM(Sales[Amount]), DATEADD('Date'[Date], -1, YEAR))

## 🧠 Explanation

DATEADD() shifts dates backward or forward by a specified interval (month, quarter, year).
Used with CALCULATE() to compare current and prior periods.

## 📘 Quick Comparison

Function	Shift	Context Type
DATEADD()	Flexible (± N intervals)	Continuous date column required
SAMEPERIODLASTYEAR()	Fixed one-year shift	Retains same shape of data
PARALLELPERIOD()	Similar to DATEADD	Often slower, but broader

## 🧩 5. LASTNONBLANK — Handling Sparse Data

✅ Example
DAX
Copy code
12MonthsEnrollments =
VAR last12Months =
    CALCULATE(
        SUM(Enrollments[EnrollmentsAmount]),
        DATEADD(LASTNONBLANK(Enrollments[EnrollmentDate], SUM(Enrollments[EnrollmentsAmount])), -12, MONTH)
    )
RETURN last12Months
🧠 Explanation
LASTNONBLANK() returns the last date in a column where the expression is not blank — perfect for trailing metrics (e.g., 12-month rolling totals).

## ⚠️ Mistake
Using LASTDATE() will return blanks if data has gaps — LASTNONBLANK() ensures you always get the last actual data point.

## 🧩 6. COUNTROWS + FILTER — Conditional Row Counting

✅ Example
DAX
Copy code
Larger Enrollments =
COUNTROWS(FILTER(FactCourseEnrollments, FactCourseEnrollments[EnrollmentsAmount] > 1000))
🧠 Explanation
COUNTROWS() counts rows in a filtered table.

FILTER() defines which rows qualify (like WHERE clause in SQL).

## ⚙️ Why FILTER Here?
The question specifies "Enrollments > 1000", which requires row-level evaluation.
Using COUNT() would not work since it only counts scalar values, not row context.

## 🧩 7. UNICHAR — Displaying Checkboxes in Matrix Visuals
✅ Example
DAX
Copy code
Schedule Checkbox =
IF(
    COUNTROWS(Appointments) > 0,
    UNICHAR(9635),
    ""
)

## 🧠 Explanation
UNICHAR(9635) produces a checkbox (☑️).

Combined with IF() logic, it visually marks rows with data (appointments).

## 🧠 Why This Works
In matrix visuals, this formula displays dynamic icons per cell.
COUNTROWS() checks data presence; UNICHAR() turns it into a visual cue.

⚠️ Mistake
Confusing UNICHAR() (symbol rendering) with CHAR() (ASCII-specific).

## 🧩 8. Power Query — Table.ReplaceValue()
✅ Example
powerquery
Copy code
Table.ReplaceValue(SalesLT_Address, "1318", "1319", Replacer.ReplaceText, {"AddressLine1"})
🧠 Explanation
Replaces text values within a column.

In this example, 21318 Lasalle Street becomes 21319 Lasalle Street because ReplaceValue acts on any text containing 1318.

## ⚠️ Common Mistake
Expecting it to replace only exact matches.
It replaces substrings within text — so 21318 → 21319 happens automatically.

🧩 9. Decomposition Tree — What It Can’t Do
✅ Question
What does the Decomposition Tree not enable you to do?

Answer: Conduct what-if analysis with built-in parameters.

## 🧠 Why
The Decomposition Tree performs root cause analysis, breaking down measures by attributes.

It can automatically find highest/lowest contributing dimensions, but not simulate “what-if” scenarios.

## 🧠 Summary
Feature	Supported?
Root cause analysis	✅
Auto-analyze dimension contribution	✅
Built-in what-if simulation	❌

## 🧩 10. Data Alerts — KPI, Gauges, and Cards
✅ Question
Where can you configure and set data alerts?

Answer: Only in Power BI Service, on visuals like KPI cards, gauges, and cards.

## 🧠 Why
Data alerts require Power BI Service + Dashboard tiles, not desktop visuals.

## ⚙️ Supported Visuals
Visual	Alerts Supported?
KPI Card	✅
Gauge	✅
Card	✅
Table / Matrix	❌

## 🧩 11. Reference Lines — Median, Percentile, and Constant
📘 Example
Create a reference line to show which employees earn above the median salary.

Correct Solution:
Add a Percentile Line (50%) using Salary measure.
The 50th percentile = median.

⚙️ Difference Table

Type	Description	Suitable for Median?
Percentile Line	Draws at a chosen percentile (e.g. 50%)	✅
Median Line	Automatic median	✅
Constant Line	Fixed value	❌

⚠️ Common Mistake

Setting a Constant Line = 0.5 does not represent 50th percentile — it’s a literal value, not a statistical measure.

🧩 12. Azure Analysis Services — Connect Live
✅ Question
You’re creating a Power BI report with data from an Azure Analysis Services Cube. You want data to update immediately.

Correct Answer: Connect Live

# ⚙️ Comparison

Mode	Data Storage	Refresh	Editable?	Ideal Use
Import	In Power BI	Manual/Scheduled	✅ Yes	Static reports
DirectQuery	In DB	Real-time SQL queries	✅ Yes	SQL DBs
Connect Live	In Cube (AAS/SSAS)	Immediate	❌ No	Semantic models

# ⚠️ Common Mistake

Choosing DirectQuery for AAS — it’s not used for cubes, only for relational DBs.

## 🧩 13. Power Query Parameters — Switching Between Databases

✅ Correct Answer
Create a parameter and update the queries to use it.

# 🧠 Why

Power Query Parameters let you dynamically switch between Dev, Test, and Prod databases without rewriting queries.

# ⚙️ Example

powerquery
Copy code
Source = Sql.Database(ParameterServer, ParameterDatabase)
# ✅ Benefits
Advantage	Description
Flexibility	Switch environments in Power BI Service
Consistency	One dataset, multiple environments
Automation	Used with deployment pipelines

# ⚠️ Common Mistakes

Mistake	Why Wrong
Creating separate queries	Redundant and error-prone
Using ReplaceValue	Static replacement only
Using JSON file	Not supported for dynamic sources

# 🧠 Quick Memory Recap Grid

Concept	Core Function	Quick Recall
Compare YoY	CALCULATE + SAMEPERIODLASTYEAR	"Shift 1 year, keep shape"
Rolling totals	DATEADD + LASTNONBLANK	"Move back in time with data gaps handled"
Conditional count	COUNTROWS + FILTER	"Row-based WHERE logic"
Checkboxes	IF + UNICHAR	"Text-based visual symbol"
Replace values	Table.ReplaceValue	"Substring not exact match"
Root cause analysis	Decomposition Tree	"Why not what-if"
Alerts	Power BI Service only	"Dashboard, not Desktop"
Median	Percentile line (50%)	"50th percentile = median"
Azure Cubes	Connect Live	"Live = immediate refresh"
Multi-environments	Power Query Parameters	"Dynamic connection strings"

✅ Final Takeaway

Most tricky PL-300 questions test whether you understand how Power BI behaves behind the scenes — context transitions, model relationships, environment configurations, and visualization properties.

Focus on why each function exists, not just what it does.

## 🧩 20. Node.js, Python, and JSON in Power BI

### 🧠 Context

Although Power BI itself doesn’t rely on Node.js or Python internally for its core data engine, both appear in the **exam** because they are supported for **extensibility** — creating custom visuals, running analytics scripts, or integrating external tools.

---

### 🟩 Node.js — Custom Visual Development

- Power BI’s **Custom Visuals SDK** (`pbiviz`) is built on **Node.js + npm**.  
- Developers install it using:


## Key Role

Node.js provides the runtime for packaging and building **custom visuals** using JavaScript or TypeScript.  
It is **not** used for data modeling, importing, or transformation inside Power BI Desktop.

| Task | Uses Node.js? | Explanation |
|------|----------------|-------------|
| Creating visuals with custom JS libraries | ✅ | Node.js compiles and packages custom visuals. |
| Importing or modeling data | ❌ | Handled by Power Query (M) and VertiPaq. |
| Running visuals inside reports | ❌ | Visuals render in browser via Power BI framework. |

## Memory Tip: 

> Node.js = Developer tool for building visuals, not part of the data engine.



### 🟦 Python — Data Transformation and Analytics

Power BI can **run Python scripts** directly for data import, transformation, and visualization.

**Use Cases:**
- Connect to and shape data:
```python
import pandas as pd
df = dataset.groupby("Region")["Sales"].sum().reset_index()

Here’s the **final section to append to your Tricky Power BI Questions Markdown** (you can paste this block at the end of your existing `.md` file).
It includes the full explanation of **Node.js, Python, and JSON in Power BI** — complete with context, comparison tables, and key takeaways formatted consistently with the rest of your notes.

---

```markdown
## 🧩 20. Node.js, Python, and JSON in Power BI

### 🧠 Context

Although Power BI itself doesn’t rely on Node.js or Python internally for its core data engine, both appear in the **exam** because they are supported for **extensibility** — creating custom visuals, running analytics scripts, or integrating external tools.

---

### 🟩 Node.js — Custom Visual Development

- Power BI’s **Custom Visuals SDK** (`pbiviz`) is built on **Node.js + npm**.  
- Developers install it using:
```

npm install -g powerbi-visuals-tools

```
- Then create and package visuals with:
```

pbiviz new MyVisual
pbiviz start
pbiviz package

````

**Key Role:**  

Node.js provides the runtime for packaging and building **custom visuals** using JavaScript or TypeScript.  
It is **not** used for data modeling, importing, or transformation inside Power BI Desktop.

| Task | Uses Node.js? | Explanation |
|------|----------------|-------------|
| Creating visuals with custom JS libraries | ✅ | Node.js compiles and packages custom visuals. |
| Importing or modeling data | ❌ | Handled by Power Query (M) and VertiPaq. |
| Running visuals inside reports | ❌ | Visuals render in browser via Power BI framework. |

**Memory Tip:**  

> Node.js = Developer tool for building visuals, not part of the data engine.

---

### 🟦 Python — Data Transformation and Analytics
Power BI can **run Python scripts** directly for data import, transformation, and visualization.

**Use Cases:**

- Connect to and shape data:
```python
import pandas as pd
df = dataset.groupby("Region")["Sales"].sum().reset_index()
````

* Build advanced analytics (Machine Learning, forecasting).
* Create Python visuals (e.g., Matplotlib, Seaborn, Plotly).

**Setup:**

Enable Python scripting via:
`File → Options → Python scripting → Set Python home directory`.

| Task                      | Uses Python? | Engine                          |
| ------------------------- | ------------ | ------------------------------- |
| Data shaping (via Pandas) | ✅            | Local Python runtime            |
| Visualization             | ✅            | Matplotlib/Seaborn              |
| Model refresh in Service  | ⚙️ Limited   | Needs gateway and local runtime |
| Core Power Query (M)      | ❌            | M engine only                   |

**Memory Tip:**

> Python = Analytics and visualization inside Power BI Desktop.

---

### 🟨 JSON — Configuration and Data Format

JSON is **widely used** in Power BI as a data interchange and configuration format.

| Usage              | Description                                                                                                      |
| ------------------ | ---------------------------------------------------------------------------------------------------------------- |
| **Themes**         | Report styles, colors, fonts stored as JSON files (`.json`).                                                     |
| **Custom Visuals** | Visual manifest and settings stored in JSON within `.pbiviz` packages.                                           |
| **Web API Data**   | Power Query retrieves API data in JSON and converts it using M functions (`Json.Document`, `Table.FromRecords`). |
| **REST API**       | Power BI REST API returns and accepts JSON payloads for automation.                                              |

**Example in Power Query:**

```powerquery
let
    Source = Json.Document(Web.Contents("https://api.example.com/data")),
    Records = Source[records],
    Table = Table.FromRecords(Records)
in
    Table



### 🔄 Combined Comparison Table

| Technology  | Role                     | Used Inside Power BI Engine? | Typical Use                            | Example Exam Context                                                 |
| ----------- | ------------------------ | ---------------------------- | -------------------------------------- | -------------------------------------------------------------------- |
| **Node.js** | Custom Visual SDK        | ❌                            | Build and package visuals              | “Which environment is used to develop custom Power BI visuals?”      |
| **Python**  | Analytics and scripting  | ✅ (locally)                  | Data transformation, ML, visualization | “Which scripting language allows you to create statistical visuals?” |
| **JSON**    | Format and configuration | ✅ (as metadata)              | Themes, API data, visual manifests     | “Which format defines report theme settings in Power BI?”            |


### 💡 Key Takeaways

* **Node.js**: Powers **external SDK** (for developers) — not used in Power BI’s core engine.
* **Python**: Used **inside Power BI Desktop** for data transformation and advanced visuals.
* **JSON**: The **formatting and data-exchange language** across APIs, visuals, and themes.

> 🧭 **Exam Hint:**

> * If a question mentions **building custom visuals** → Node.js / pbiviz CLI.
> * If it mentions **statistical or ML analysis** → Python.
> * If it mentions **report themes or web data** → JSON.

