
## Create Pre-requisite Azure Resources
### Create Storage Account, Container & File Share

## Create MongoDB Azure Container Instance
The following command creates a MongoDB Azure Container Instance and sets a root username/password, the same is used with Squidex
```
az container create --resource-group squidex --name squidex-mongodb --image mongo \
--azure-file-volume-account-name [STORAGE_ACCOUNT_NAME] \
--azure-file-volume-account-key "[STORAGE_ACCOUNT_KEY]" \
--azure-file-volume-share-name etc-squidex-mongodb \
--azure-file-volume-mount-path "/data/mongoaz" \
--cpu 2 --memory 2 --ports 27017 --ip-address public --protocol TCP --os-type Linux \
--environment-variables "MONGO_INITDB_ROOT_USERNAME"="[USERNAME]" "MONGO_INITDB_ROOT_PASSWORD"="[PASSWORD]"
```
