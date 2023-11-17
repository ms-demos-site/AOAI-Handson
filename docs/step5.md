## データの投入（data配下のPDFを利用して Search Index を作成）
`data/`にあるPDFファイルをAzure Blob Storageにアプロードし、そのデータを使ってAzure Searchのインデックスを作成します。
### 環境変数の設定
```PowerShell
$env:AZURE_STORAGE_ACCOUNT = "xxx" # Azureリソースの作成時に作成されているリソースの名前
$env:AZURE_STORAGE_CONTAINER = "content" # 固定値
$env:AZURE_SEARCH_SERVICE = "xxx" # Azureリソースの作成時に作成されているリソースの名前
$env:AZURE_SEARCH_INDEX = "gptkbindex" # 固定値
$env:AZURE_SEARCH_KEY = "xxx" # Azure PortalからAzure Search ServiceのKeyを取得
$env:AZURE_FORMRECOGNIZER_SERVICE = "xxx" # Azureリソースの作成時に作成されているリソースの名前
$env:AZURE_FORMRECOGNIZER_KEY = "xxx" # Azure PortalからAzure Form RecognizerのKeyを取得
```

### スクリプト実行用にCosmosDB権限付与
```PowerShell
# So that the user can access Cosmos DB for script execution
az cosmosdb sql role assignment create --account-name $env:AZURE_COSMOSDB_ACCOUNT --resource-group $env:AZURE_COSMOSDB_RESOURCE_GROUP --scope / --principal-id $env:AZURE_PRINCIPAL_ID --role-definition-id $roleId
```

### Run prepdocs.py
```PowerShell
cd jp-azureopenai-samples/5.internal-document-search

$pythonCmd = Get-Command python -ErrorAction SilentlyContinue
if (-not $pythonCmd) {
  # fallback to python3 if python not found
  $pythonCmd = Get-Command python3 -ErrorAction SilentlyContinue
}

# Creating python virtual environment "scripts/.venv"
Start-Process -FilePath ($pythonCmd).Source -ArgumentList "-m venv ./scripts/.venv" -Wait -NoNewWindow

$venvPythonPath = "./scripts/.venv/scripts/python.exe"
if (Test-Path -Path "/usr") {
  # fallback to Linux venv path
  $venvPythonPath = "./scripts/.venv/bin/python"
}

# Installing dependencies from "requirements.txt" into virtual environment
Start-Process -FilePath $venvPythonPath -ArgumentList "-m pip install -r ./scripts/requirements.txt" -Wait -NoNewWindow

# Running "prepdocs.py"
$cwd = (Get-Location)
Start-Process -FilePath $venvPythonPath -ArgumentList "./scripts/prepdocs.py $cwd/data/* --storageaccount $env:AZURE_STORAGE_ACCOUNT --container $env:AZURE_STORAGE_CONTAINER --searchservice $env:AZURE_SEARCH_SERVICE --searchkey $env:AZURE_SEARCH_KEY --index $env:AZURE_SEARCH_INDEX --formrecognizerservice $env:AZURE_FORMRECOGNIZER_SERVICE --formrecognizerkey $env:AZURE_FORMRECOGNIZER_KEY -v --managedidentitycredential" -Wait -NoNewWindow
```

データの投入し、Search Indexを作成することで、社内文書検索の機能も使えるようになります。