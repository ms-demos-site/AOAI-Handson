# Chat+社内文書検索
一般的なツールが事前にインストール済みであるAzure Cloud Shellからのセットアップする方法を記載します。2023年8月にはazdもAzure Cloud Shellに事前にインストールされるようになる可能性もありますが、2023年7月時点では事前にインストールされていないので、こちらの手順はazdを使わないものとします。

## デプロイの事前準備

### 環境変数設定
以下のように環境変数を設定します。
```PowerShell
$ENV:SUB = "your-subscription-id"
$ENV:USER_ID = "your-user-id" # 作業者のユーザープリンシパル名
$Env:AZURE_PRINCIPAL_ID = az ad user show --id $ENV:USER_ID -o tsv --query id # 作業者のObjectId

$ENV:LOC = "japaneast" # The location to store the deployment metadata and to deploy resources
$ENV:DEP_NAME = "deployment-name" # The deployment name 
$ENV:AZURE_ENV_NAME = "azureenvnameXXX" # XXX は作業者毎ユニークな文字列をセット。リソースグループ名のSuffix (rg-$ENV:AZURE_ENV_NAMEになる) 
```

?> 補足: YOUR-USER-IDの取得方法例: `Azure Portal > Azure AD > Users > 自分の名前で検索 > Object ID` をコピーしてください。


### 権限確認
実行するユーザの AAD アカウントは、`Microsoft.Authorization/roleAssignments/write` 権限を持っている必要があります。この権限は [ユーザーアクセス管理者](https://learn.microsoft.com/azure/role-based-access-control/built-in-roles#user-access-administrator) もしくは [所有者](https://learn.microsoft.com/azure/role-based-access-control/built-in-roles#owner)が保持しています。  
```
az role assignment list --assignee $ENV:USER_ID --subscription $ENV:SUB --output table
```

### コードの取得
```PowerShell
mkdir <作業用フォルダ名>
cd <作業用フォルダ名>
git clone https://github.com/ms-demos-site/aoai-handson-src.git
```