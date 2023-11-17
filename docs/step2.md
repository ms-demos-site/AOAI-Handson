## Azureのリソース作成
以下のコマンドを用いて、Azureのリソースを作成します。

```PowerShell
cd jp-azureopenai-samples/5.internal-document-search/infra

az deployment sub create --name $ENV:DEP_NAME --location $ENV:LOC --template-file main.bicep --parameters principalId=$ENV:AZURE_PRINCIPAL_ID environmentName=$ENV:AZURE_ENV_NAME location=$ENV:LOC
```

> 注意: 実行環境の bicep のバージョンが古いため、実行エラーが発生する場合は、こちらのコマンド `az bicep upgrade` を実行してください。

`InsufficientQuota`等のエラーについては[トラブルシューティングガイド](assets/troubleshooting.md)をご参照ください。
