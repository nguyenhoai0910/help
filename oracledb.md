

You can query the USER_TAB_COLUMNS table for table column metadata.
```sql
SELECT table_name, column_name, data_type, data_length
FROM USER_TAB_COLUMNS
WHERE table_name = 'MYTABLE'

```