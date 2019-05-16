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
