# Chat+社内文書検索
一般的なツールが事前にインストール済みであるAzure Cloud Shellからのセットアップする方法を記載します。2023年8月にはazdもAzure Cloud Shellに事前にインストールされるようになる可能性もありますが、2023年7月時点では事前にインストールされていないので、こちらの手順はazdを使わないものとします。

# セットアップガイド
> **重要:** このサンプルをデプロイするには、**Azure Open AI サービスが有効になっているサブスクリプションが必要です**。Azure Open AI サービスへのアクセス申請は[こちら](https://aka.ms/oaiapply)から行ってください。

## 事前準備

### ツール
このデモをデプロイするためには、ローカルに以下の開発環境が必要です。Azure Cloud Shellに、以下が事前にインストールされていることをご確認ください。PowerShellを前提としています。

| ツール名 | 確認コマンド | 推奨バージョン | 
| --- | --- | --- |
| [Azure CLI](https://learn.microsoft.com/cli/azure/install-azure-cli) | `az --version` | 2.50.0 以降 |
| [Python 3+](https://www.python.org/downloads/) | `python --version` | 3.9.14 以降 |
| pip ( Pythonと一緒にインストール ) | `pip --version` | 23.1.2 以降 |
| [Node.js](https://nodejs.org/en/download/) | `node --version` | 16.19.1 以降 |
| [Git](https://git-scm.com/downloads) | `git --version` | 2.33.8 以降 |


### 環境変数設定
以下のように環境変数を設定します。
```PowerShell
$ENV:SUB = "your-subscription-id"
$ENV:USER_ID = "your-id" # 作業者のユーザープリンシパル名
$Env:AZURE_PRINCIPAL_ID = az ad user show --id $ENV:USER_ID -o tsv --query id # 作業者のObjectId

$ENV:LOC = "japaneast" # The location to store the deployment metadata and to deploy resources
$ENV:DEP_NAME = "deployment-name" # The deployment name 
$ENV:AZURE_ENV_NAME = "azureenvnametemp" # リソースグループ名のSuffix (rg-$ENV:AZURE_ENV_NAMEになる)
```

### 権限確認
実行するユーザの AAD アカウントは、`Microsoft.Authorization/roleAssignments/write` 権限を持っている必要があります。この権限は [ユーザーアクセス管理者](https://learn.microsoft.com/azure/role-based-access-control/built-in-roles#user-access-administrator) もしくは [所有者](https://learn.microsoft.com/azure/role-based-access-control/built-in-roles#owner)が保持しています。  
```
az role assignment list --assignee $ENV:USER_ID --subscription $ENV:SUB --output table
```

### コードの取得
```PowerShell
mkdir <作業用フォルダ名>
cd <作業用フォルダ名>
git clone https://github.com/Azure-Samples/jp-azureopenai-samples
```