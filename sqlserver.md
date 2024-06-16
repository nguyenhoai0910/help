##1. Show all table database.
```
SELECT [name] AS [Column Name]
FROM syscolumns
WHERE id = (SELECT id FROM sysobjects WHERE type = 'V' AND [Name] = 'Your table name')
```
