### Connect to sqlplus and open connection
````
sql>connect / as sysdba
sql> show con_name
````
  
### Oracle change password
````
sql> ALTER USER usuari IDENTIFIED BY currentPassword REPLACE newPassword;
````

### New oracle user
Open SQLPlus
  
````
SQL> connect system/admin@127.0.0.1:1521/XEPDB1
 
SQL> CREATE USER pepe IDENTIFIED BY pepe;
 
SQL> grant all privileges to pepe;
````
