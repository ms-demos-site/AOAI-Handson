## Azureのリソース作成
以下のコマンドを用いて、Azureのリソースを作成します。
```PowerShell
cd jp-azureopenai-samples/5.internal-document-search/infra

az deployment sub create --name $ENV:DEP_NAME --location $ENV:LOC --template-file main.bicep --parameters principalId=$ENV:AZURE_PRINCIPAL_ID environmentName=$ENV:AZURE_ENV_NAME location=$ENV:LOC
```

`InsufficientQuota`等のエラーについては[トラブルシューティングガイド](../troubleshooting.md)をご参照ください。
