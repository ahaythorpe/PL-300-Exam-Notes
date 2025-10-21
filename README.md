
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

### 🧠 Example
```text
Products (Category, ProductID)
   ↓
Sales (ProductID, SalesAmount)

# 🧮 Using VAR and RETURN in Complex Power BI Measures

The `VAR` keyword in DAX stores intermediate results (numbers, text, or tables) so you can reuse them later in the same measure.  
The `RETURN` block defines what expression to output after all `VAR` values are calculated.

---

## 🧠 Why Use VAR
- Simplifies complex logic.
- Improves readability and performance.
- Lets you reuse results without recalculating them.
- Works perfectly in Table / Matrix visuals where each cell has its own filter context.

---

## 💡 Syntax
```DAX
<Measure Name> =
VAR <var1> = <expression1>
VAR <var2> = <expression2>
RETURN
<final_expression>

# 🔢 Example 1 – Two VARs

Profit Margin % =
VAR TotalSales = SUM(Sales[Amount])
VAR TotalCost  = SUM(Sales[Cost])
RETURN
DIVIDE(TotalSales - TotalCost, TotalSales)

# Example 2 – Three VARs (KPI logic)

KPI Status =
VAR CurrentSales = SUM(Sales[Amount])
VAR TargetSales  = AVERAGE(Targets[Goal])
VAR Performance  = DIVIDE(CurrentSales, TargetSales)
RETURN
IF(
    Performance >= 1, "✅ Met Target",
    IF(Performance >= 0.9, "⚠️ Near Target", "❌ Below Target")
)

# 📊 Example 3 – VAR with a Table

Top 5 Products =
VAR Top5 = TOPN(5, Sales, Sales[Revenue], DESC)
RETURN
SUMX(Top5, Sales[Revenue])


---

# Running Final code

Use Return


