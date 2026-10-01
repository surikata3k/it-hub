# Run docker image

### Mysql
```
 docker run -d -p 3306:3306 --name mysql-db  -e MYSQL_ROOT_PASSWORD=123456 -v mysqldata:/var/lib/mysql mysql
```

### Oracle
(username: sys)
```
docker run --name oracle -p 1521:1521 -e ORACLE_PWD=123456 -v oracledb:/opt/oracle/oradata container-registry.oracle.com/database/free:latest
```
```
GRANT CREATE SESSION TO user_name;
```

### Postgres
```
docker run -p 5432:5432 -e POSTGRES_USER=postgres -e POSTGRES_PASSWORD=123456 -e POSTGRES_DB=postgres -v pgdata:/var/lib/postgresql/data -d postgres
```

### Sql Server
```
docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=Password_123#" -p 1433:1433 -v sqlvolume:/var/opt/mssql --name sql_server_container -d mcr.microsoft.com/mssql/server
```

### Mongodb
```
docker run --name mongodb -d -p 27017:27017 -v mongodbdata:/data/db mongodb/mongodb-community-server:latest
```

### Jenkins
```
docker run -p 8080:8080 -p 50000:50000 --restart=on-failure -v jenkins_home:/var/jenkins_home jenkins/jenkins:lts-jdk17
```

### Ubuntu
```
docker run -i -t --rm ubuntu:latest /bin/bash
```

### MeTube
```
docker run -d -p 8081:8081 -v .:/downloads ghcr.io/alexta69/metube
```

### Portainer
```
docker run -d -p 8000:8000 -p 9443:9443 --name portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce:lts
```

