##1. Get column names from a table
```
SELECT [name] AS [Column Name]
FROM syscolumns
WHERE id = (SELECT id FROM sysobjects WHERE type = 'V' AND [Name] = 'Your table name')
```
