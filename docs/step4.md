### Create and assign Cosmos DB data actions roles
App ServiceがCosmos DBにアクセスするための権限付与が必要になります。付与しない場合、「企業向けChat」を利用しようとした場合に以下の様なエラーになります。
`Error: (Forbidden) Request blocked by Auth xxx : Request is blocked because principal [xxx] does not have required RBAC permissions to perform action`

```PowerShell
$env:AZURE_COSMOSDB_ACCOUNT = "xxx" # Azureリソースの作成時に作成されているリソースの名前
$env:AZURE_COSMOSDB_RESOURCE_GROUP = "rg-${ENV:AZURE_ENV_NAME}"
$env:BACKEND_IDENTITY_PRINCIPAL_ID = "app serviceのManaged IdentityのObject ID"

$env:BACKEND_IDENTITY_PRINCIPAL_ID = "app serviceのManaged IdentityのObject ID" # Azure PortalからApp ServiceのObejct IDを取得

cd jp-azureopenai-samples/5.internal-document-search

$roleId = az cosmosdb sql role definition create --account-name $env:AZURE_COSMOSDB_ACCOUNT --resource-group $env:AZURE_COSMOSDB_RESOURCE_GROUP --body ./scripts/cosmosreadwriterole.json --output tsv --query id

# So that the app service can access Cosmos DB
az cosmosdb sql role assignment create --account-name $env:AZURE_COSMOSDB_ACCOUNT --resource-group $env:AZURE_COSMOSDB_RESOURCE_GROUP --scope / --principal-id $env:BACKEND_IDENTITY_PRINCIPAL_ID --role-definition-id $roleId
```

ここまでの手順で、「企業向けChat」の機能が使えるようになりますが、社内文書データの投入ができていないので、「社内文書検索」機能を使おうとすると`Error: () The index 'gptkbindex' for service 'xxx' was not found`というエラーになります。
