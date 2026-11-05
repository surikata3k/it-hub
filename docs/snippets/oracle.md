---
description: "Oracle Database snippets: connect with SQL*Plus, create users and change passwords."
---

# Oracle

## Connect with SQL\*Plus

Inside the [Oracle Free container](https://github.com/surikata3k/docker):

```bash
docker exec -it oracle sqlplus / as sysdba
```

```text
SQL> show con_name
SQL> show pdbs
```

## Create a user

Users are created in the pluggable database (`FREEPDB1` in Oracle Free, `XEPDB1` in Oracle XE):

```text
SQL> connect system/123456@localhost:1521/FREEPDB1
SQL> CREATE USER pepe IDENTIFIED BY pepe;
SQL> GRANT CONNECT, RESOURCE TO pepe;
SQL> ALTER USER pepe QUOTA UNLIMITED ON USERS;
```

`CONNECT` lets the user log in and `RESOURCE` lets them create tables, sequences and procedures. The quota lets them store data in the `USERS` tablespace.

Minimum permission to let a user log in:

```sql
GRANT CREATE SESSION TO user_name;
```

## Change a password

A user changing their own password:

```sql
ALTER USER user_name IDENTIFIED BY new_password REPLACE old_password;
```

An administrator changing someone else's password:

```sql
ALTER USER user_name IDENTIFIED BY new_password;
```
