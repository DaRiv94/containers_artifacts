
# ACR Login, Pull Dataoad image

### ACR Login
```
docker login registryycv7004.azurecr.io`
```
username: registryycv7004
password: 96CSAoJvJGZDtYs1GjDFA4Db5W/tZhX8

```
docker pull registryycv7004.azurecr.io/dataload:1.0 
```

# Docker Network

### Create network
```
docker network create tripinsightsnetwork
```

# SQL Server

## Run SQL Server locally 
(Connection String for something like SQLelectrion *mssql://sa:gB4gv6Hr2@0.0.0.0:1433*)

```
docker run --network tripinsightsnetwork -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=gB4gv6Hr2" --name sql1 -p 1433:1433 -d mcr.microsoft.com/mssql/server:2017-latest
```

## 2 Options to create database (Go inside, or create from outside)

### Enter SQL Server (Option 1 step 1/2)
```
docker exec -it sql1  /opt/mssql-tools/bin/sqlcmd -S localhost -U sa -P gB4gv6Hr2
```

### create database (Option 1 step 2/2)
```
CREATE DATABASE mydrivingDB;
GO
```

### Double check database creation (Option 1 step 2.5/2) (type *exit* and enter to get out of container)
```
SELECT Name from sys.Databases;
GO
```

### Create Database for SQL Server (Option 2 step 1/1)
```
docker exec sql1 /opt/mssql-tools/bin/sqlcmd -S localhost -U SA -P 'gB4gv6Hr2' -Q "CREATE DATABASE mydrivingDB"
```

### Double Check Database mydrivingDB is created in SQL Server (Option 2 step 1.5/1)
```
docker exec sql1 /opt/mssql-tools/bin/sqlcmd -S localhost -U SA -P 'gB4gv6Hr2' -Q "SELECT Name from sys.Databases"
```

### Load data into SQL Server
```
docker run --network tripinsightsnetwork -e SQLFQDN=sql1 -e SQLUSER=sa -e SQLPASS=gB4gv6Hr2 -e SQLDB=mydrivingDB registryycv7004.azurecr.io/dataload:1.0
```

---
# POI Container

### Build POI Image (NOTE: I did rename docker files)

```
docker build -f .\POI_Dockerfile_3 -t "tripinsights/poi:1.0" ..\src\poi\
```



### Start up POI container with proper configuration
```
docker run --network tripinsightsnetwork -d -p 8080:80 -e "SQL_DBNAME=mydrivingDB" -e "SQL_USER=sa" -e "SQL_PASSWORD=gB4gv6Hr2" -e "SQL_SERVER=sql1" -e "ASPNETCORE_ENVIRONMENT=Local" tripinsights/poi:1.0
```

### Health Check POI
```
Invoke-WebRequest http://localhost:8080/api/poi/healthcheck
```

### Get Data for POI to double check SQL Server connection
```
Invoke-WebRequest http://localhost:8080/api/poi
```

### Build POI Image to publish (NOTE: I did rename docker files)

```
docker build --no-cache --build-arg IMAGE_VERSION="1.0" --build-arg IMAGE_CREATE_DATE="$(Get-Date((Get-Date).ToUniversalTime()) -UFormat '%Y-%m-%dT%H:%M:%SZ')" --build-arg IMAGE_SOURCE_REVISION="$(git rev-parse HEAD)" -f .\POI_Dockerfile_3 -t "registryycv7004.azurecr.io/tripinsights/poi:1.0" ..\src\poi\
```

### List acr repos
```
az acr repository list --name registryycv7004
```

### Push POI Image to publish (NOTE: I did rename docker files)

```
docker push registryycv7004.azurecr.io/tripinsights/poi:1.0
```

### List acr repo and then list POI repo info
```
az acr repository show --name registryycv7004 --repository tripinsights/poi
```

### List acr repo and then list POI repo tags
```
az acr repository show-tags --name registryycv7004 --repository tripinsights/poi
```





---
# trips

### Build Local image
```
docker build -f .\Trips_Dockerfile_4 -t "tripinsights/trips:1.0" ..\src\trips\
```


### Run Trips Image
```
docker run --network tripinsightsnetwork -d -p 8081:80 -e "SQL_DBNAME=mydrivingDB" -e "SQL_USER=sa" -e "SQL_PASSWORD=gB4gv6Hr2" -e "SQL_SERVER=sql1" -e "OPENAPI_DOCS_URI=http://localhost:80" tripinsights/trips:1.0
```

### Health Check Trips Container
```
Invoke-WebRequest http://localhost:8081/api/trips/healthcheck
```

### Get Data  Trips Container to double check SQL Server connection
```
Invoke-WebRequest http://localhost:8081/api/trips
```

### Build POI Image to publish (NOTE: I did rename docker files)

```
docker build --no-cache --build-arg IMAGE_VERSION="1.0" --build-arg IMAGE_CREATE_DATE="$(Get-Date((Get-Date).ToUniversalTime()) -UFormat '%Y-%m-%dT%H:%M:%SZ')" --build-arg IMAGE_SOURCE_REVISION="$(git rev-parse HEAD)" -f .\Trips_Dockerfile_4 -t "registryycv7004.azurecr.io/tripinsights/trips:1.1" ..\src\trips\
```

### List acr repos
```
az acr repository list --name registryycv7004
```

### Push Trips Image to publish 

```
docker push registryycv7004.azurecr.io/tripinsights/trips:1.1
```

### List acr repo and then list Trips repo info
```
az acr repository show --name registryycv7004 --repository tripinsights/trips
```

### List acr repo and then list Trips repo tags
```
az acr repository show-tags --name registryycv7004 --repository tripinsights/trips
```

---
# Below are MY Notes
# WORK IN PROGRESS BELOW

---
# tripviewer

### Build Local image
```
docker build -f .\Trips_Dockerfile_4 -t "tripinsights/trips:1.0" ..\src\trips\
```


### Run Trips Image
```
docker run --network tripinsightsnetwork -d -p 8081:80 -e "SQL_DBNAME=mydrivingDB" -e "SQL_USER=sa" -e "SQL_PASSWORD=gB4gv6Hr2" -e "SQL_SERVER=sql1" -e "OPENAPI_DOCS_URI=http://localhost:80" tripinsights/trips:1.0
```

### Health Check Trips Container
```
Invoke-WebRequest http://localhost:8081/api/trips/healthcheck
```

### Get Data  Trips Container to double check SQL Server connection
```
Invoke-WebRequest http://localhost:8081/api/trips
```

### Build POI Image to publish (NOTE: I did rename docker files)

```
docker build --no-cache --build-arg IMAGE_VERSION="1.0" --build-arg IMAGE_CREATE_DATE="$(Get-Date((Get-Date).ToUniversalTime()) -UFormat '%Y-%m-%dT%H:%M:%SZ')" --build-arg IMAGE_SOURCE_REVISION="$(git rev-parse HEAD)" -f .\Trips_Dockerfile_4 -t "registryycv7004.azurecr.io/tripinsights/trips:1.1" ..\src\trips\
```

### List acr repos
```
az acr repository list --name registryycv7004
```

### Push Trips Image to publish 

```
docker push registryycv7004.azurecr.io/tripinsights/trips:1.1
```

### List acr repo and then list Trips repo info
```
az acr repository show --name registryycv7004 --repository tripinsights/trips
```

### List acr repo and then list Trips repo tags
```
az acr repository show-tags --name registryycv7004 --repository tripinsights/trips
```




docker run --network tripinsightsnetwork -d -p 8081:80 -e "SQL_DBNAME=mydrivingDB" -e "SQL_USER=sa" -e "SQL_PASSWORD=gB4gv6Hr2" -e "SQL_SERVER=sql1" -e "OPENAPI_DOCS_URI=http://localhost:80" tripinsights/trips:1.0


docker build --no-cache --build-arg IMAGE_VERSION="1.0" --build-arg IMAGE_CREATE_DATE="$(Get-Date((Get-Date).ToUniversalTime()) -UFormat '%Y-%m-%dT%H:%M:%SZ')" --build-arg IMAGE_SOURCE_REVISION="$(git rev-parse HEAD)" -f Dockerfile -t "tripinsights/poi:1.0" .


az acr repository list --name registryycv7004



From [Openhack lab page](https://openhack.skillmeup.com/labengine/modules/microsoft-open-hack-containers-v2_2d85a605ca87_3_0)
```
ACR Login Server: registryycv7004.azurecr.io
ACR Username: registryycv7004
ACR Password: 96CSAoJvJGZDtYs1GjDFA4Db5W/tZhX8
SQL Server: sqlserverycv7004
SQL Server Username: sqladminyCv7004
SQL Server Password: gB4gv6Hr2
Simulator url: simulatorregistryycv7004.westus.azurecontainer.io
api-dev password: gC0qg2Ea4
web-dev password: jM7fq6Mh7
```