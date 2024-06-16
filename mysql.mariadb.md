## 1. Get column names from a table

```sql
SELECT column_name 
FROM information_schema.columns 
WHERE 
table_schema = 'Schema' AND table_name = 'Table_Name'

```