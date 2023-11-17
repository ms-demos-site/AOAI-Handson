## アプリケーションコードのビルドとデプロイ
本アプリケーションは、フロントエンドがJavaScriptで書かれており、バックエンドがPythonで書かれています。まずは、フロントエンドのJavaScriptのコードビルドし、その後にPythonのコードと一緒にAzure Web Appにデプロイします。
### フロントエンドのビルド
`package.json`や`vite.config.ts`に従ってビルドをし、結果を`../backend/static`に出力します。
```PowerShell
cd jp-azureopenai-samples/5.internal-document-search/src/frontend
npm install
npm run build
```

### Pythonコードのデプロイ
ビルドされたフロントエンドのコードと一緒に、PythonのアプリケーションコードをAzure Web Appにデプロイします。
```PowerShell
$ENV:AZURE_WEBAPP_RESOURCE_GROUP = "rg-${ENV:AZURE_ENV_NAME}"
$ENV:WEBAPP_NAME = "xxx" # Azureリソースの作成時に作成されているリソースの名前

cd jp-azureopenai-samples/5.internal-document-search/src/backend
zip -r app.zip .
az webapp deployment source config-zip --resource-group $ENV:AZURE_WEBAPP_RESOURCE_GROUP --name $ENV:WEBAPP_NAME --src ./app.zip
```

コマンドの実行が終了すると、アプリケーションにアクセスする為の URL が表示されます。この URL をブラウザで開き、サンプルアプリケーションの利用を開始してください
> 注意: アプリケーションのデプロイ完了には数分かかることがあります。"Python Developer" のウェルカムスクリーンが表示される場合は、数分待ってアクセスし直してください。
