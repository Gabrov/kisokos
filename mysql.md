Táblaellenőrzés:
```
CHECK TABLE tbl_name [, tbl_name] ... [option] ...

option: {
    FOR UPGRADE
  | QUICK
  | FAST
  | MEDIUM
  | EXTENDED
  | CHANGED
}
```

|Type|Meaning|
|---|---|
|QUICK|Do not scan the rows to check for incorrect links. Applies to InnoDB and MyISAM tables and views.|
|FAST|Check only tables that have not been closed properly. Ignored for InnoDB; applies only to MyISAM tables and views.|
|CHANGED|Check only tables that have been changed since the last check or that have not been closed properly. Ignored for InnoDB; applies only to MyISAM tables and views.|
|MEDIUM|Scan rows to verify that deleted links are valid. This also calculates a key checksum for the rows and verifies this with a calculated checksum for the keys. Ignored for InnoDB; applies only to MyISAM tables and views.|
|EXTENDED|Do a full key lookup for all keys for each row. This ensures that the table is 100% consistent, but takes a long time. Ignored for InnoDB; applies only to MyISAM tables and views.|

Össze is lehet vonni őket:
```
CHECK TABLE test_table FAST QUICK;
```

DB shrinkelés:
```
OPTIMIZE [NO_WRITE_TO_BINLOG | LOCAL]
    TABLE tbl_name [, tbl_name] ...
```
By default, the server writes OPTIMIZE TABLE statements to the binary log so that they replicate to replication slaves. To suppress logging, specify the optional NO_WRITE_TO_BINLOG keyword or its alias LOCAL.

MyISAM Tábla javítása:
```
sudo service mysql stop
cd /var/lib/mysql/$DATABASE_NAME

myisamchk -r $TABLE_NAME
```

vagy a fenti nem működik:

```
myisamchk -r -v -f $TABLE_NAME

sudo service mysql start
```

Lekérdezés naplózás bekapcsolása:
```
SET global general_log_file='/tmp/mysql.log';
SET global general_log = on;
SET global log_output = 'file';
```

Ugyanez táblába:
```
CREATE TABLE `slow_log` (
   `start_time` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP 
                          ON UPDATE CURRENT_TIMESTAMP,
   `user_host` mediumtext NOT NULL,
   `query_time` time NOT NULL,
   `lock_time` time NOT NULL,
   `rows_sent` int(11) NOT NULL,
   `rows_examined` int(11) NOT NULL,
   `db` varchar(512) NOT NULL,
   `last_insert_id` int(11) NOT NULL,
   `insert_id` int(11) NOT NULL,
   `server_id` int(10) unsigned NOT NULL,
   `sql_text` mediumtext NOT NULL,
   `thread_id` bigint(21) unsigned NOT NULL
  ) ENGINE=CSV DEFAULT CHARSET=utf8 COMMENT='Slow log'

CREATE TABLE `general_log` (
   `event_time` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP
                          ON UPDATE CURRENT_TIMESTAMP,
   `user_host` mediumtext NOT NULL,
   `thread_id` bigint(21) unsigned NOT NULL,
   `server_id` int(10) unsigned NOT NULL,
   `command_type` varchar(64) NOT NULL,
   `argument` mediumtext NOT NULL
  ) ENGINE=CSV DEFAULT CHARSET=utf8 COMMENT='General log'

SET global general_log = 1;
SET global log_output = 'table';

select * from mysql.general_log

SET global general_log = 0;
```
