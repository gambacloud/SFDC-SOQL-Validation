# SOQL → CSV Validator (v3)

A single-file **HTML/JS app** that runs entirely in the browser. Paste a SOQL query and a CSV dataset, and it will validate each row against the SOQL `WHERE` clause. No backend required – works locally or on GitHub Pages.

## Features
- **Logical operators**: `AND`, `OR`, `NOT`, with full parentheses support
- **Comparisons**: `=`, `!=`, `>`, `>=`, `<`, `<=`
- **Text functions**: `LIKE`, `NOT LIKE` (with `%` and `_` wildcards)
- **Sets**: `IN`, `NOT IN`
- **Null checks**: `IS NULL`, `IS NOT NULL`
- **Ranges**: `BETWEEN a AND b`
- **Case functions**: `LOWER(field)`, `UPPER(field)`
- **Date literals** (Salesforce-style):
  - `TODAY`
  - `YESTERDAY`
  - `TOMORROW`
  - `LAST_N_DAYS:n` (e.g. `LAST_N_DAYS:30`)
- **Related fields**: supports dotted field names like `CustomOBJ__r.DefaultField` or `StandardOBJ.CustomField__c` as long as the CSV header matches.

All date comparisons are evaluated at **date-only precision** (00:00 local time).

## Usage
1. Open `index.html` directly in your browser (double-click the file) – no server needed.
2. Or host it on **GitHub Pages**:
   - Push this file to a repo.
   - In *Settings → Pages*, enable Pages from branch `main`, folder `/ (root)`.
   - Visit the generated URL.

## Example
### SOQL
```sql
SELECT Id, Name FROM Account
WHERE CreatedDate = TODAY
   OR CreatedDate = YESTERDAY
   OR CloseDate = LAST_N_DAYS:7
   OR NOT Type IN ('Customer')
   OR CustomOBJ__r.DefaultField = 'X'
```

### CSV
```csv
Name,Status,Type,CreatedDate,CloseDate,CustomOBJ__r.DefaultField
Account 1,Active,Customer,2025-08-23,2025-08-20,X
Account 2,Inactive,Prospect,2025-08-22,2025-08-15,Y
Account 3,Active,Partner,2025-08-16,2025-07-30,X
```

### Output
- Each row gets `VALID` = TRUE/FALSE
- `Explanation` shows which condition(s) matched
- KPI summary: Total / Valid / Invalid / Success %
- Download results as CSV

## Development
- Pure frontend: only depends on [PapaParse](https://www.papaparse.com/) for CSV parsing.
- Easy to extend: edit `<script>` section inside `index.html`.
- Contributions welcome: fork, edit, open PR.

---
Enjoy validating your SOQL queries against sample CSVs without Salesforce! 🚀
