# MSSQL to PostgreSQL Migration with GitHub Copilot

Automated migration of SQL Server (MSSQL) schemas, views, and business logic to PostgreSQL using GitHub Copilot, Visual Studio Code, and the official PostgreSQL & SQL Server extensions.  
Includes code examples, validation reports, and workflow documentation to showcase efficient, AI-assisted database modernization.
![image](https://github.com/user-attachments/assets/e7ce6c6a-75ef-4578-9ef2-678458471e59)
---

## 🚀 Project Highlights

- **VS Code as a Unified Migration Environment:**
  - Used Visual Studio Code as the central IDE for all migration activities.
  - Leveraged the PostgreSQL and SQL Server extensions for direct database connectivity, query execution, and schema browsing within the editor.
- **Automated Schema Conversion:**  
  Refactored SQL Server-specific syntax (e.g., `DATEDIFF`, `ISDATE`, `[brackets]`) into PostgreSQL idioms like `generate_series`, `EXTRACT(ISODOW ...)`, and schema qualification (`dbo.`) with Copilot's help.
- **Validation & Testing:**  
  Automated validation reports comparing result sets between MSSQL and PostgreSQL, all from within VS Code.
- **Documentation & Reporting:**  
  Detailed migration reports with test queries, result sets, and screenshots.
- **Efficiency Gains:**  
  Reduced manual effort and error rates by integrating Copilot’s AI-powered suggestions and VS Code's extension ecosystem.

---

## 📋 Example: View Migration

### Original MSSQL View

```sql
CREATE VIEW [dbo].[BusinessDaysView] AS
SELECT
  PK__Visits AS FK_Visits,
  CASE
    WHEN DATEDIFF(dd, GETDATE(), ExpectedDate) < 0 THEN -1
    WHEN DATEDIFF(dd, GETDATE(), ExpectedDate) = 0 THEN 0
    ELSE DATEDIFF(day, GETDATE(), ExpectedDate)
  END AS BusinessDaysToExpectedDate
FROM Visits;
```

### Copilot-Assisted PostgreSQL Conversion

```sql
CREATE OR REPLACE VIEW dbo.businessdaysview AS
SELECT
  v.pk_visits AS fk_visits,
  CASE
    WHEN (v.expecteddate < CURRENT_DATE) THEN -1
    WHEN (v.expecteddate = CURRENT_DATE) THEN 0
    ELSE (
      SELECT COUNT(*)
      FROM generate_series(
        LEAST(CURRENT_DATE, v.expecteddate),
        GREATEST(CURRENT_DATE, v.expecteddate),
        INTERVAL '1 day'
      ) AS d(day)
      WHERE EXTRACT(ISODOW FROM d.day) < 6 -- Mon-Fri only
    )
  END AS businessdaystoexpecteddate
FROM dbo.visits v;
```

**Explanation:**  
- Replaced `DATEDIFF` with a `generate_series` and `COUNT(*)` to count business days.
- Used `EXTRACT(ISODOW ...)` for weekday logic.
- Added `dbo.` prefix for schema qualification.
- All code was written, tested, and validated inside VS Code using the relevant extensions.

---

## 🧪 Validation Report Example

```markdown
# Validation Report: dbo.businessdaysview

**Test Query:**
SELECT * FROM dbo.ontrac_businessdaysview LIMIT 10;

**Result Set:**
 fk_visits | businessdaystoexpecteddate
-----------------+--------------------------
               1 |                       -1
               2 |                        10
               3 |                         0
(3 rows)
```

---

## 💡 How Copilot, VS Code, and Extensions Helped

- **Unified Workflow:**
  - Used VS Code as a single environment for editing, running, and validating both MSSQL and PostgreSQL code.
  - Switched between SQL Server and PostgreSQL connections seamlessly using extensions.
- **Context-Aware Suggestions:**  
  Copilot recognized SQL Server-specific logic and suggested PostgreSQL equivalents directly in the editor.
- **Schema Qualification:**  
  Copilot and extensions helped ensure all objects were properly schema-qualified.
- **Error Reduction:**  
  Caught and fixed issues like unsupported functions and type mismatches with Copilot's suggestions and extension error highlighting.
- **Speed:**  
  Reduced migration time by generating ready-to-execute PostgreSQL code and validation scripts, all without leaving VS Code.
  
---

> “GitHub Copilot, combined with VS Code and the official database extensions, has been a game-changer in my database migration projects, enabling rapid, reliable, and well-documented transitions from legacy SQL Server environments to modern PostgreSQL platforms.”

---

# My Key Skills & Achievements in Database Migration

## 🚀 What I Did

- **Migrated complex SQL Server (MSSQL) views and business logic to PostgreSQL**
    - Analyzed legacy T-SQL code, including advanced business logic and custom functions.
    - Used Copilot and VS Code extensions to automate the conversion of SQL Server-specific syntax (e.g., `DATEDIFF`, `ISDATE`, `[brackets]`) to PostgreSQL equivalents (`generate_series`, `EXTRACT(ISODOW ...)`, `::date`, etc.).
    - Ensured all table and view references were schema-qualified (added `dbo.` prefix where needed).

- **Automated and validated the migration process**
    - Created and executed PostgreSQL scripts for each migrated view/function directly from VS Code.
    - Generated and compared result sets between SQL Server and PostgreSQL to ensure data consistency.
    - Automated the creation of validation reports, including test queries and actual result sets.

- **Documented the workflow and results**
    - Maintained detailed migration reports for each object, including:
        - Original MSSQL code
        - Copilot-assisted PostgreSQL code
        - Test queries and result sets
        - Explanations of key changes and logic refactoring
    - Included screenshots of:
        - Original code in SSMS/VS Code
        - Copilot suggestions and code completions
        - PostgreSQL execution and validation results

- **Leveraged Copilot and VS Code for efficiency and quality**
    - Reduced manual effort and error rates by using Copilot’s context-aware suggestions and extension error highlighting.
    - Quickly identified and fixed issues like unsupported functions, type mismatches, and schema qualification.
    - Enabled rapid, reliable, and well-documented transitions from legacy SQL Server environments to modern PostgreSQL platforms.

## 🧑‍💻 Example Workflow

1. **Analyze** the original MSSQL schema and business logic in VS Code.
2. **Convert** to PostgreSQL using Copilot’s code suggestions and extensions.
3. **Validate** by executing and comparing result sets across both platforms, all within VS Code.
4. **Document** the process with automated reports and screenshots.

## 📈 Impact

- Migrated dozens of complex views and functions with high accuracy.
- Ensured business logic and data integrity were preserved in the new platform.
- Created a repeatable, auditable migration process for future projects.

---

> “This project demonstrates my ability to combine deep SQL/database knowledge with modern AI tools and the VS Code ecosystem to deliver efficient, reliable, and well-documented database migrations.”
