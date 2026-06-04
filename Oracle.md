Újraindítás:
```
sqlplus / as sysdba;
shutdown immediate;
startup nomount;
alter database mount;
alter database open;
```

archív log kikapcsolása:
```
sqlplus / as sysdba
SHUTDOWN IMMEDIATE;
STARTUP MOUNT;
ALTER DATABASE NOARCHIVELOG;
ALTER DATABASE OPEN;
ARCHIVE LOG LIST;
```

Listener vezérlő:
```
lsnrctl start
```

konténer: CDB$ROOT

Táblaterület létrehozása:
```
CREATE TABLESPACE my_new_tablespace
DATAFILE 'my_new_tablespace.dbf'
SIZE 50M
AUTOEXTEND ON
NEXT 10M
MAXSIZE 500M
EXTENT MANAGEMENT LOCAL;
```

Kapcsolat váltás:
```
show con_name;
show pdbs;
alter session set container = XEPDB1;
show con_name;
```

user (sqlplus sys/oracle@localhost:1521/FREEPDB1 as sysda):
```
ALTER SESSION SET CONTAINER = freepdb1;
CREATE USER new_username IDENTIFIED BY "StrongP4ssword123!" ACCOUNT UNLOCK;
-- Allow the user to log in
GRANT CREATE SESSION TO new_username;

-- (Optional) Grant resource permissions to create tables, indexes, and sequences
GRANT RESOURCE TO new_username;
```
