MSSQL táblák méretének lekérdezése:

```sql
SELECT 
    t.NAME AS TableName,
    s.Name AS SchemaName,
    p.rows AS RowCounts,
    SUM(a.total_pages) * 8 AS TotalSpaceKB, 
    CAST(ROUND(((SUM(a.total_pages) * 8) / 1024.00), 2) AS NUMERIC(36, 2)) AS TotalSpaceMB,
	CAST(ROUND(((SUM(a.total_pages) * 8) / 1024.00 / 1024.00), 2) AS NUMERIC(36, 2)) AS TotalSpaceGB,
    SUM(a.used_pages) * 8 AS UsedSpaceKB, 
    CAST(ROUND(((SUM(a.used_pages) * 8) / 1024.00), 2) AS NUMERIC(36, 2)) AS UsedSpaceMB,
	CAST(ROUND(((SUM(a.used_pages) * 8) / 1024.00 / 1024.00), 2) AS NUMERIC(36, 2)) AS UsedSpaceGB,
    (SUM(a.total_pages) - SUM(a.used_pages)) * 8 AS UnusedSpaceKB,
    CAST(ROUND(((SUM(a.total_pages) - SUM(a.used_pages)) * 8) / 1024.00, 2) AS NUMERIC(36, 2)) AS UnusedSpaceMB,
	CAST(ROUND(((SUM(a.total_pages) - SUM(a.used_pages)) * 8) / 1024.00 / 1024.00, 2) AS NUMERIC(36, 2)) AS UnusedSpaceGB
FROM 
    sys.tables t
INNER JOIN      
    sys.indexes i ON t.OBJECT_ID = i.object_id
INNER JOIN 
    sys.partitions p ON i.object_id = p.OBJECT_ID AND i.index_id = p.index_id
INNER JOIN 
    sys.allocation_units a ON p.partition_id = a.container_id
LEFT OUTER JOIN 
    sys.schemas s ON t.schema_id = s.schema_id
WHERE 
    t.NAME NOT LIKE 'dt%' 
    AND t.is_ms_shipped = 0
    AND i.OBJECT_ID > 255 
GROUP BY 
    t.Name, s.Name, p.Rows
ORDER BY 
    t.Name
```

MySQL táblák méretének lekérdezése:
```sql
SELECT CONCAT(table_schema, '.', table_name),
       CONCAT(ROUND(table_rows / 1000000, 2), 'M')                                    rows,
       CONCAT(ROUND(data_length / ( 1024 * 1024 * 1024 ), 2), 'G')                    DATA,
       CONCAT(ROUND(index_length / ( 1024 * 1024 * 1024 ), 2), 'G')                   idx,
       CONCAT(ROUND(( data_length + index_length ) / ( 1024 * 1024 * 1024 ), 2), 'G') total_size,
       ROUND(index_length / data_length, 2)                                           idxfrac
FROM   information_schema.TABLES
ORDER  BY data_length + index_length DESC
LIMIT  10;
```

Idegen kulcsok műveleteinek ellenőrzése:

```sql
SELECT 
   OBJECT_NAME(f.parent_object_id) AS 'Table name',
   COL_NAME(fc.parent_object_id, fc.parent_column_id) AS 'Field name',
   OBJECT_NAME(f.referenced_object_id) AS 'Referenced table',
   COL_NAME(fc.referenced_object_id, fc.referenced_column_id) AS 'Referenced field name',
   update_referential_action_desc AS 'On Update',
   delete_referential_action_desc AS 'On Delete'
FROM sys.foreign_keys AS f,
     sys.foreign_key_columns AS fc
WHERE f.OBJECT_ID = fc.constraint_object_id
ORDER BY 1
```

Tranzakciós log ürítése és méretének beállítása:

```sql
ALTER DATABASE [your_db_name] 
SET RECOVERY SIMPLE;

CHECKPOINT;

-- shrink the log file to 200MB
DBCC SHRINKFILE (your_db_name_log,200);

-- define the initial size to 200MB as well.. CHANGE it as per your needs!
ALTER DATABASE [your_db_name]
MODIFY FILE (NAME=your_db_name_LOGICAL_log,SIZE=200MB,MAXSIZE=UNLIMITED,FILEGROWTH=100MB);

-- Optional if you want the database in full recovery mode 
-- for point in time recovery going forward
ALTER DATABASE [your_db_name] 
SET RECOVERY FULL;
-- Take full backup to establish log chain
-- take log backups to help truncate the log file and keep it manageable
```

Tábla létezésének lekérdezései:  
```sql
SELECT * FROM INFORMATION_SCHEMA.TABLES WHERE TABLE_SCHEMA = 'TheSchema'  AND  TABLE_NAME = 'TheTable'
```
  
```sql
SELECT OBJECT_ID (N'mytablename', N'U')
```
