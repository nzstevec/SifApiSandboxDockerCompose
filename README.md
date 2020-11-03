
**Create mocksif server in local machine**

Assuming you have Docker for Windows installed. You can enter docker commands from a Windows command prompt (or bash if configured?)

To start as a daemon in the background:
  docker-compose up -d

To start in foreground:
  docker-compose up

To stop:
  docker-compose down

To list docker containers:
  docker container ls

To list docker images:
  docker image ls

To logon to dockerhub:
  docker login

To pull an updated docker image:
  docker pull sierracsd/mocksifdb

**Create mocksif server in Azure**

```
az group create --name mocksifResourceGroup --location "Australia East"
az appservice plan create --name mocksifServicePlan --resource-group mocksifResourceGroup --sku B1 --is-linux
az webapp create --resource-group mocksifResourceGroup --plan mocksifServicePlan --name mocksif --multicontainer-config-type compose --multicontainer-config-file docker-compose.yml

```

**Example Azure Cloud session:**
```
Requesting a Cloud Shell.Succeeded.
Connecting terminal...

Welcome to Azure Cloud Shell

Type "az" to use Azure CLI
Type "help" to learn about Cloud Shell

steve@Azure:~$ cd data/git/SifApiSandboxDockerCompose/
steve@Azure:~/data/git/SifApiSandboxDockerCompose$ ls
docker-compose.yml  README.md
steve@Azure:~/data/git/SifApiSandboxDockerCompose$ cat docker-compose.yml
version: '3.3'

services:
   sqlserver:
     image: sierracsd/mocksifdb:latest
     container_name: "sqlserver"
     ports:
      - "1433:1433"
     environment:
       ACCEPT_EULA: "Y"
       SA_PASSWORD: "Qwerty2020"

   sif:
     depends_on:
       - sqlserver
     image: sierracsd/mocksif:1.0
     container_name: "sif"
     ports:
       - "80:80"
       - "443:443"
steve@Azure:~/data/git/SifApiSandboxDockerCompose$ az group create --name mocksifResourceGroup --location "Australia East"
{
  "id": "/subscriptions/54917460-2378-4ece-b77c-ae155bca5055/resourceGroups/mocksifResourceGroup",
  "location": "australiaeast",
  "managedBy": null,
  "name": "mocksifResourceGroup",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "tags": null,
  "type": "Microsoft.Resources/resourceGroups"
}
steve@Azure:~/data/git/SifApiSandboxDockerCompose$ az appservice plan create --name mocksifServicePlan --resource-group mocksifResourceGroup --sku B1 --is-linux
{- Finished ..
  "freeOfferExpirationTime": "2020-11-13T18:17:51.356666",
  "geoRegion": "Australia East",
  "hostingEnvironmentProfile": null,
  "hyperV": false,
  "id": "/subscriptions/54917460-2378-4ece-b77c-ae155bca5055/resourceGroups/mocksifResourceGroup/providers/Microsoft.Web/serverfarms/mocksifServicePlan",
  "isSpot": false,
  "isXenon": false,
  "kind": "linux",
  "location": "Australia East",
  "maximumElasticWorkerCount": 1,
  "maximumNumberOfWorkers": 3,
  "name": "mocksifServicePlan",
  "numberOfSites": 0,
  "perSiteScaling": false,
  "provisioningState": "Succeeded",
  "reserved": true,
  "resourceGroup": "mocksifResourceGroup",
  "sku": {
    "capabilities": null,
    "capacity": 1,
    "family": "B",
    "locations": null,
    "name": "B1",
    "size": "B1",
    "skuCapacity": null,
    "tier": "Basic"
  },
  "spotExpirationTime": null,
  "status": "Ready",
  "subscription": "54917460-2378-4ece-b77c-ae155bca5055",
  "tags": null,
  "targetWorkerCount": 0,
  "targetWorkerSizeId": 0,
  "type": "Microsoft.Web/serverfarms",
  "workerTierName": null
}
steve@Azure:~/data/git/SifApiSandboxDockerCompose$ az webapp create --resource-group mocksifResourceGroup --plan mocksifServicePlan --name mocksif --multicontainer-config-type compose --multicontainer-config-file docker-compose.yml
{- Finished ..
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "clientCertExclusionPaths": null,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "mocksif.azurewebsites.net",
  "enabled": true,
  "enabledHostNames": [
    "mocksif.azurewebsites.net",
    "mocksif.scm.azurewebsites.net"
  ],
  "ftpPublishingUrl": "ftp://waws-prod-sy3-051.ftp.azurewebsites.windows.net/site/wwwroot",
  "hostNameSslStates": [
    {
      "hostType": "Standard",
      "ipBasedSslResult": null,
      "ipBasedSslState": "NotConfigured",
      "name": "mocksif.azurewebsites.net",
      "sslState": "Disabled",
      "thumbprint": null,
      "toUpdate": null,
      "toUpdateIpBasedSsl": null,
      "virtualIp": null
    },
    {
      "hostType": "Repository",
      "ipBasedSslResult": null,
      "ipBasedSslState": "NotConfigured",
      "name": "mocksif.scm.azurewebsites.net",
      "sslState": "Disabled",
      "thumbprint": null,
      "toUpdate": null,
      "toUpdateIpBasedSsl": null,
      "virtualIp": null
    }
  ],
  "hostNames": [
    "mocksif.azurewebsites.net"
  ],
  "hostNamesDisabled": false,
  "hostingEnvironmentProfile": null,
  "httpsOnly": false,
  "hyperV": false,
  "id": "/subscriptions/54917460-2378-4ece-b77c-ae155bca5055/resourceGroups/mocksifResourceGroup/providers/Microsoft.Web/sites/mocksif",
  "identity": null,
  "inProgressOperationId": null,
  "isDefaultContainer": null,
  "isXenon": false,
  "kind": "app,linux,container",
  "lastModifiedTimeUtc": "2020-10-14T18:18:21.806666",
  "location": "Australia East",
  "maxNumberOfWorkers": null,
  "name": "mocksif",
  "outboundIpAddresses": "20.53.107.10,20.53.111.51,20.53.110.74,20.53.111.68,20.53.111.72,20.53.109.240",
  "possibleOutboundIpAddresses": "20.53.107.10,20.53.111.51,20.53.110.74,20.53.111.68,20.53.111.72,20.53.109.240,20.53.110.94,20.53.110.101,20.53.111.122,20.53.111.125,20.53.111.191,20.53.111.213,20.53.82.83,20.53.82.138,20.53.82.178,20.53.82.235,20.53.83.93,20.53.84.28",
  "redundancyMode": "None",
  "repositorySiteName": "mocksif",
  "reserved": true,
  "resourceGroup": "mocksifResourceGroup",
  "scmSiteAlsoStopped": false,
  "serverFarmId": "/subscriptions/54917460-2378-4ece-b77c-ae155bca5055/resourceGroups/mocksifResourceGroup/providers/Microsoft.Web/serverfarms/mocksifServicePlan",
  "siteConfig": {
    "acrUseManagedIdentityCreds": false,
    "acrUserManagedIdentityID": null,
    "alwaysOn": null,
    "apiDefinition": null,
    "apiManagementConfig": null,
    "appCommandLine": null,
    "appSettings": null,
    "autoHealEnabled": null,
    "autoHealRules": null,
    "autoSwapSlotName": null,
    "azureMonitorLogCategories": null,
    "azureStorageAccounts": null,
    "connectionStrings": null,
    "cors": null,
    "customAppPoolIdentityAdminState": null,
    "customAppPoolIdentityTenantState": null,
    "defaultDocuments": null,
    "detailedErrorLoggingEnabled": null,
    "documentRoot": null,
    "experiments": null,
    "fileChangeAuditEnabled": null,
    "ftpsState": null,
    "functionAppScaleLimit": null,
    "functionsRuntimeScaleMonitoringEnabled": null,
    "handlerMappings": null,
    "healthCheckPath": null,
    "http20Enabled": null,
    "httpLoggingEnabled": null,
    "ipSecurityRestrictions": [
      {
        "action": "Allow",
        "description": "Allow all access",
        "ipAddress": "Any",
        "name": "Allow all",
        "priority": 1,
        "subnetMask": null,
        "subnetTrafficTag": null,
        "tag": null,
        "vnetSubnetResourceId": null,
        "vnetTrafficTag": null
      }
    ],
    "javaContainer": null,
    "javaContainerVersion": null,
    "javaVersion": null,
    "limits": null,
    "linuxFxVersion": null,
    "loadBalancing": null,
    "localMySqlEnabled": null,
    "logsDirectorySizeLimit": null,
    "machineKey": null,
    "managedPipelineMode": null,
    "managedServiceIdentityId": null,
    "metadata": null,
    "minTlsVersion": null,
    "minimumElasticInstanceCount": 0,
    "netFrameworkVersion": null,
    "nodeVersion": null,
    "numberOfWorkers": null,
    "phpVersion": null,
    "powerShellVersion": null,
    "preWarmedInstanceCount": null,
    "publishingPassword": null,
    "publishingUsername": null,
    "push": null,
    "pythonVersion": null,
    "remoteDebuggingEnabled": null,
    "remoteDebuggingVersion": null,
    "requestTracingEnabled": null,
    "requestTracingExpirationTime": null,
    "routingRules": null,
    "runtimeADUser": null,
    "runtimeADUserPassword": null,
    "scmIpSecurityRestrictions": [
      {
        "action": "Allow",
        "description": "Allow all access",
        "ipAddress": "Any",
        "name": "Allow all",
        "priority": 1,
        "subnetMask": null,
        "subnetTrafficTag": null,
        "tag": null,
        "vnetSubnetResourceId": null,
        "vnetTrafficTag": null
      }
    ],
    "scmIpSecurityRestrictionsUseMain": null,
    "scmMinTlsVersion": null,
    "scmType": null,
    "tracingOptions": null,
    "use32BitWorkerProcess": null,
    "virtualApplications": null,
    "vnetName": null,
    "vnetRouteAllEnabled": null,
    "webSocketsEnabled": null,
    "websiteTimeZone": null,
    "winAuthAdminState": null,
    "winAuthTenantState": null,
    "windowsFxVersion": null,
    "xManagedServiceIdentityId": null
  },
  "slotSwapStatus": null,
  "state": "Running",
  "suspendedTill": null,
  "tags": null,
  "targetSwapSlot": null,
  "trafficManagerHostNames": null,
  "type": "Microsoft.Web/sites",
  "usageState": "Normal"
}
```