## Data Cleaning (Power Query)

The raw dataset was cleaned in Power Query before loading into Power BI.

| Step | Transformation Applied |
|---|---|
| Removed empty rows | Deleted rows where all fields were blank |
| Removed duplicates | Based on `employee_id` column |
| Replaced values | Fixed embedded spaces e.g. `E M P00015` → `EMP00015` |
| Filtered invalid records | Removed rows with inconsistent or corrupt data |
| Validated employee_id | Kept only records with exactly 8 characters |

**Original dataset:** 50,020 rows  
**Clean dataset:** 49,977 rows  
**Records removed:** 43
