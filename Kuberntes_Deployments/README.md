


    apidev@msftopenhack7009ops.onmicrosoft.com
    gC0qg2Ea4

    webdev@msftopenhack7009ops.onmicrosoft.com
    jM7fq6Mh7


az aks get-credentials -g rg-akscluster -n tripsinsight-cluster

az aks update -n tripsinsight-cluster -g rg-akscluster --attach-acr registryycv7004

az aks update -n tripsinsight-cluster -g rg-akscluster --enable-managed-identity

### Setting up Azure AAD
https://stacksimplify.com/azure-aks/kubernetes-rbac-role-and-rolebinding-with-azure-ad/


### Create Kubernetes Secret for SQL Server Secrets
kubectl create secret generic  sql-server-secret --from-literal SQL_USER=sqladminyCv7004 --from-literal SQL_PASSWORD=gB4gv6Hr2 --from-literal SQL_SERVER=sqlserverycv7004.database.windows.net --from-literal SQL_DBNAME=mydrivingDB -n api


### Deploying and Setting up Ingress controller Steps
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx

helm install ingress-nginx ingress-nginx/ingress-nginx --namespace ingress-basic 



kubectl create secret generic akv-creds -n api  --from-literal clientid=a5c08d01-0210-43fe-83ee-f89e187bdb23 --from-literal clientsecret=ab1e5887-2604-4743-959a-ef540bc93d05

[1:43 PM] Jarrett Long
clientId: a5c08d01-0210-43fe-83ee-f89e187bdb23

[1:43 PM] Jarrett Long
secretId: ab1e5887-2604-4743-959a-ef540bc93d05



nodePublishSecretRef:

name: akv-creds






SQL_USER	Yes	ENV or File	sqladmin	The username for the SQL Server database.
SQL_PASSWORD	Yes	ENV or File		The password for the SQL Server database.
SQL_SERVER	Yes	ENV or File		The server name for the SQL Server database.
SQL_DBNAME

Helpful data
ACR Login Server: registryycv7004.azurecr.io
ACR Username: registryycv7004
ACR Password: 96CSAoJvJGZDtYs1GjDFA4Db5W/tZhX8
SQL Server: sqlserverycv7004
SQL Server Username: sqladminyCv7004
SQL Server Password: gB4gv6Hr2
Simulator url: simulatorregistryycv7004.westus.azurecontainer.io
api-dev password: gC0qg2Ea4
web-dev password: jM7fq6Mh7