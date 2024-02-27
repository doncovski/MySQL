#Details about MySQL schema size
SELECT table_schema "DB Name", ROUND(SUM(data_length + index_length) / 1024 / 1024, 1) "DB Size in MB" FROM information_schema.tables GROUP BY table_schema;

#Details about MySQL storage engine size
SELECT ENGINE , round(sum(data_length)/1024/1024,2) "DATA_LENGTH(MB)", round(sum(index_length)/1024/1024,2) "INDEX_LENGTH(MB)",
round(sum( data_length + index_length )/1024/1024,2) "TOTAL(MB)" FROM information_schema.TABLES GROUP BY ENGINE ;

#Produce SQL script that renames all tables in a schema to its lower case form
select concat('rename table ', table_name, ' to ' , lower(table_name) , ';') from information_schema.tables where table_schema = 'your_schema_name';

#Extract Query output into CSV file
mysql -u user -p --host=hostname --database=db --batch -e "select * from yourtable" | sed 's/\t/","/g;s/^/"/;s/$/"/;s/\n//g' > yourlocalfilename

#Tips for MySQL prompt
mysql > pager command (filter output. Turn off with nopager. Example: pager grep -i 'sleep')
mysql > tee file (send output to file. Turn off with notee)
mysql > warnings (show any warnings. Turn off with nowarning )
mysql > prompt string (add metacharacters to prompt)
mysql > delimiter string (use other delimiter than ; Example: delimiter //)

#Select all InnoDB tables with Data free > 100MB
SELECT table_schema, table_name, data_free/1024/1024 AS data_free_MB FROM information_schema.tables WHERE engine LIKE 'InnoDB' AND data_free > 100*1024*1024;

#What’s inside the buffer pool and what's its size
SELECT page_type AS page_type, sum(data_size) / 1024 / 1024 AS size_in_mb FROM information_schema.innodb_buffer_page GROUP BY page_type ORDER BY size_in_mb DESC;

#Get the buffer pool usage by index
SELECT table_name AS table_name, index_name AS index_name, count(*) AS page_count, sum(data_size) / 1024 / 1024 AS size_in_mb
FROM information_schema.innodb_buffer_page GROUP BY table_name, index_name ORDER BY size_in_mb DESC;

#Query to find all Foreign keys that are referencing tableA from other tables
mysql> SELECT TABLE_NAME, COLUMN_NAME, CONSTRAINT_NAME, REFERENCED_TABLE_NAME, REFERENCED_COLUMN_NAME
FROM INFORMATION_SCHEMA.KEY_COLUMN_USAGE WHERE REFERENCED_TABLE_NAME = 'tableA';

#Query to find all Triggers in database 'schemaA'
mysql> SELECT * FROM INFORMATION_SCHEMA.TRIGGERS WHERE TRIGGER_SCHEMA='schemaA'\G
 If you want to find all triggers related to tableA, add additional condition in the WHERE clause ' AND EVENT_OBJECT_TABLE = tableA '

#Query to find out fill rate of AUTO_INCREMENT values
SELECT   TABLE_SCHEMA,   TABLE_NAME,   COLUMN_NAME,   DATA_TYPE,  COLUMN_TYPE,   IF(     LOCATE('unsigned', COLUMN_TYPE) > 0,     1,     0   ) AS IS_UNSIGNED,  
(     CASE DATA_TYPE       WHEN 'tinyint' THEN 255       WHEN 'smallint' THEN 65535       WHEN 'mediumint' THEN 16777215       WHEN 'int' THEN 4294967295      
WHEN 'bigint' THEN 18446744073709551615     END >> IF(LOCATE('unsigned', COLUMN_TYPE) > 0, 0, 1)   ) AS MAX_VALUE,   AUTO_INCREMENT,   AUTO_INCREMENT /
(     CASE DATA_TYPE       WHEN 'tinyint' THEN 255       WHEN 'smallint' THEN 65535       WHEN 'mediumint' THEN 16777215       WHEN 'int' THEN 4294967295      
WHEN 'bigint' THEN 18446744073709551615     END >> IF(LOCATE('unsigned', COLUMN_TYPE) > 0, 0, 1)   ) AS AUTO_INCREMENT_RATIO FROM   INFORMATION_SCHEMA.COLUMNS  
INNER JOIN INFORMATION_SCHEMA.TABLES USING (TABLE_SCHEMA, TABLE_NAME) WHERE   TABLE_SCHEMA NOT IN ('mysql', 'INFORMATION_SCHEMA', 'performance_schema')   AND EXTRA='auto_increment' ;

#Dump MySQL instance using mysql shell
Backup path = /data/mysql/dump
Schemas to exclude from backup in array --excludeSchemas
Threads to use for the backup --threads (don't use more than the available threads on the system)
mysqlsh root@localhost -S /var/run/mysqld/mysqld.sock -- util dump-instance /data/mysql/dump --excludeSchemas=["mysql","information_schema","performance_schema","sys"]  --threads=8 --showProgress
