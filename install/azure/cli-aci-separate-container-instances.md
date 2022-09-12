
## Create Pre-requisite Azure Resources
### Create Storage Account, Container & File Share
```
STORAGE_ACCOUNT_NAME=squidexdatastore
STORAGE_FILE_SHARE_NAME=etc-squidex-mongodb
STORAGE_CONTAINER_NAME=etc-squidex-assets
RESOURCE_GROUP=squidex
STORAGE_LOCATION=eastus
STORAGE_SKU=Standard_LRS
```
```
az storage account create --name $STORAGE_ACCOUNT_NAME --resource-group $RESOURCE_GROUP --location $STORAGE_LOCATION --kind StorageV2 --sku $STORAGE_SKU
```
```
az storage container create --name $STORAGE_CONTAINER_NAME --account-name $STORAGE_ACCOUNT_NAME
```
```
az storage share-rm create --name $STORAGE_FILE_SHARE_NAME --storage-account $STORAGE_ACCOUNT_NAME
```
### Retrieve Storage Account Key
```
STORAGE_ACCOUNT_KEY=$(az storage account keys list --account-name $STORAGE_ACCOUNT_NAME --query [0].value -o tsv)
```
## Create MongoDB Azure Container Instance
The following command creates a MongoDB Azure Container Instance and sets a root username/password, the same is used with Squidex
```
MONGO_USERNAME=mongoadmin
MONGO_PASSWORD=Passw0rd
```
```
az container create --resource-group squidex --name squidex-mongodb --image mongo \
--azure-file-volume-account-name $STORAGE_ACCOUNT_NAME \
--azure-file-volume-account-key $STORAGE_ACCOUNT_KEY \
--azure-file-volume-share-name $STORAGE_FILE_SHARE_NAME \
--azure-file-volume-mount-path "/data/mongoaz" \
--cpu 2 --memory 2 --ports 27017 --ip-address public --protocol TCP --os-type Linux \
--environment-variables "MONGO_INITDB_ROOT_USERNAME"="$MONGO_USERNAME" "MONGO_INITDB_ROOT_PASSWORD"="$MONGO_PASSWORD"
```
