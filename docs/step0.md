# このハンズオンラボを始めるまえに必要な環境設定

## 事前準備

## ハンズオン実施環境
本ハンズオンは、以下の OS で実施することが可能です。
- Windows 11 / 10
- MacOS X
  - BigSur 以上のバージョンでないと、XCode のバージョン非互換で CLI のインストールが出来ない場合があるようです。
- Linux

## Azure Subscription の準備
現状 Azure OpenAI Service へのアクセスは、限られたユーザー様に制限されております。
現在、アクセスが制限されているのは、高い需要、今後の製品改良、マイクロソフトの責任ある AIへのコミットメントを調整するためです。 現在のところ、Microsoft と既存のパートナーシップ関係があるお客様、リスクの低いユース ケース、軽減策の取り入れに取り組んでいるお客様を対象としています。

本ハンズオンの実施にあたり、上記の要件に従い、Azure Subscription を作っていただき、Azure OpenAI へのアクセス権を事前に申請いただく必要があります。

!> 可能であれば、みなさまの所属する会社から払い出しを受けた Azure Subscription をご利用いただくことで、最も早く利用開始可能になります。

会社から Azure Subscription の払い出しを受けるための手順については、所属する企業毎に異なる要件がありますので、まずは社内の IT 担当者やクラウドの担当者の方にご相談ください。
もし、会社から Azure Subscription の払い出しを受けられない場合には、次項にオプションとして、Azure の無償アカウントの作り方を紹介しますので、参考にしてください。

### Azure 無償評価版の作成方法
Azure 無料評価版は、下記のリンクを開いた先にある「無料で始める」というリンクから手続きを進めることで利用開始が可能です。

[Azure の無料アカウントを今すぐ作成する | Microsoft Azure](https://azure.microsoft.com/ja-jp/free/)

無料アカウントの作成には以下の情報が必要ですので、それぞれご用意ください。

- Microsoft アカウント
  - Outlook.com のメールアドレスを利用するか、もしくは任意のメールアドレスを Microsoft アカウントとして登録してご利用ください。
  - もし Microsoft アカウントをお持ちでなければ、[account.microsoft.com](https://account.microsoft.com/) にアクセスして、新規の Outlook.com のメールアドレスを取得ください。
- 有効なクレジットカード
  - 無料の範囲内で利用する場合でも、必ず登録が必要な項目です。
  - お手数ですが有効なクレジットカード情報の登録をよろしくお願いします。
  - 通常は、無償枠が完了した時点で一旦アカウントが停止されるようになっていますが、その制限を外すことも出来るので、アカウントの操作時にはご注意ください。
- 有効な電話番号
  - 個人認証のために有効な電話番号の登録も必要です。
  - SMS もしくは機械音声による通話にて、アカウント発行の途中で個人認証を実施します。

## Azure OpenAI Service へのアクセス申請
Azure の Subscription の準備が出来たら、[Azure OpenAI の利用申請](https://customervoice.microsoft.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR7en2Ais5pxKtso_Pz4b1_xUOFA5Qk1UWDRBMjg0WFhPMkIzTzhKQ1dWNyQlQCN0PWcu)を実施ください。

この申請の際には、ご利用中の Azure Subscription の情報が必要になります。
先述の通り、この手順にて、企業のアカウントに紐づく形で払い出しされた Azure Subscription の情報を入力いただくことで、最も迅速な利用開始が可能になります。
それ以外の Azure Subscription を登録いただいた場合、申請が迅速に受理されない場合がありますので、ご注意ください。

!> 正しく企業アカウントで申請をいただいた場合、承認に要する時間は凡そ 1 営業日以内になる場合が多いです（筆者個人の過去の経験則であり、公式情報ではありません。）

## セットアップガイド

#### クラウド実行環境
このデモをデプロイすると以下のリソースが Azure サブスクリプション上に作成されます。

| サービス名 | SKU | Note |
| --- | --- | --- |
|Azure App Service|S1||
|Azure OpenAI Service|S0|gpt-3.5-turbo gpt-3.5-turbo-16k|
|Azure Cognitive Search|S1||
|Azure Cosmos DB|プロビジョニング済みスループット||
|Azure Form Recgonizer|S0||
|Azure Blob Storage|汎用v2|ZRS|
|Azure Application Insights||ワークスペース　ベース|
|Azure Log Analytics|||

#### クラウド開発環境

<details><summary>詳細</summary>
<pre>
<code>

## Cloud Shell

### ツール
このデモをデプロイするためには、以下のツールが必要です。Azure Cloud Shellに、以下が事前にインストールされていることをご確認ください。PowerShellを前提としています。

| ツール名 | 確認コマンド | 推奨バージョン | 
| --- | --- | --- |
| [Azure CLI](https://learn.microsoft.com/cli/azure/install-azure-cli) | `az --version` | 2.50.0 以降 |
| [Python 3+](https://www.python.org/downloads/) | `python --version` | 3.9.14 以降 |
| pip ( Pythonと一緒にインストール ) | `pip --version` | 23.1.2 以降 |
| [Node.js](https://nodejs.org/en/download/) | `node --version` | 16.19.1 以降 |
| [Git](https://git-scm.com/downloads) | `git --version` | 2.33.8 以降 |

</code>
</pre>
</details>

#### ローカル開発環境（オプション）

<details><summary>詳細</summary>
<pre>
<code>

## ツール
このデモをデプロイするためには、ローカルに以下の開発環境が必要です。

| ツール名 | 確認コマンド | 推奨バージョン | 
| --- | --- | --- |
| [Azure CLI](https://learn.microsoft.com/cli/azure/install-azure-cli) | `az --version` | 2.50.0 以降 |
| [Python 3+](https://www.python.org/downloads/) | `python --version` | 3.9.14 以降 |
| pip ( Pythonと一緒にインストール ) | `pip --version` | 23.1.2 以降 |
| [Node.js](https://nodejs.org/en/download/) | `node --version` | 16.19.1 以降 |
| [Git](https://git-scm.com/downloads) | `git --version` | 2.33.8 以降 |



## Azure CLI の準備

### Azure CLI のインストール
Azure CLI のインストール方法は、OS によって様々です。
詳細については[Azure CLI をインストールする方法](https://learn.microsoft.com/ja-jp/cli/azure/install-azure-cli) のドキュメントを確認いただき、自身の利用されている OS に合った方法でセットアップをお願いします。

Azure CLI をインストールする方法<br>
https://learn.microsoft.com/ja-jp/cli/azure/install-azure-cli

インストールが完了したら、コマンドプロンプトや PowerShell や bash など、シェルを立ち上げて下記のコマンドを入力して、期待通りの応答が得られるか確認しましょう。

```cmd
> az -v
```

```出力結果
azure-cli                         2.51.0

core                              2.51.0
telemetry                          1.1.0

Dependencies:
msal                            1.24.0b1
azure-mgmt-resource             23.1.0b2

Python location 'C:\Program Files\Microsoft SDKs\Azure\CLI2\python.exe'
Extensions directory 'C:\Users\user\.azure\cliextensions'

Python (Windows) 3.10.10 (tags/v3.10.10:aad5f6a, Feb  7 2023, 17:20:36) [MSC v.1929 64 bit (AMD64)]

Legal docs and information: aka.ms/AzureCliLegal


Your CLI is up-to-date.
```

### Azure CLI にログイン
Azure CLI が利用可能になったら、操作対象の環境にログインします。
以下のコマンドを入力すると、ブラウザーでのログインを求められるので、有効な資格情報を入力すれば CLI へのログインが完了します。

```bat
> az login
```

すると、利用可能なサブスクリプションのリストが表示されますので、今回利用するサブスクリプションを見つけ "name" の項目と、"id" の項目をメモします。

```出力結果
]
  {
    "cloudName": "AzureCloud",
    "homeTenantId": "<GUID>",
    "id": "<GUID>",
    "isDefault": false,
    "managedByTenants": [
      {
        "tenantId": "<GUID>"
      }
    ],
    "name": "ここにサブスクリプション名が入っているはずです",
    "state": "Enabled",
    "tenantId": "<GUID>",
    "user": {
      "name": "tokawa@microsoft.com",
      "type": "user"
    }
  }
]
```

なお、サブスクリプションのリストを見逃してしまった場合には、以下のコマンドで表形式で表示も可能です。

```bat
>  az account list --output table --query "[].[name,id]"
```

利用するサブスクリプションを選択するためのコマンドは以下の通りです。

```bat
> az account set --subscription サブスクリプション名
```

これで、コマンドラインツールから Azure の操作が出来るようになりました！

## Azure Developer CLI の準備
### Azure Developer CLI のインストール
Azure Developer CLI (azd) のインストール方法も、OS によって様々です。
詳細については「[Azure Developer CLIをインストールまたは更新する](https://learn.microsoft.com/ja-jp/azure/developer/azure-developer-cli/install-azd?tabs=winget-windows%2Cbrew-mac%2Cscript-linux&pivots=os-windows)」のドキュメント確認して、お使いの OS にあわせた方法でインストールを行ってください。

Azure Developer CLIをインストールまたは更新する<br>
https://learn.microsoft.com/ja-jp/azure/developer/azure-developer-cli/install-azd?tabs=winget-windows%2Cbrew-mac%2Cscript-linux&pivots=os-windows

インストールが完了したら、設定反映のために一回ターミナルを再起動してからこの先の手順を進めてください。

### Azure Developer CLI へのログイン
Azure Developer CLI (azd) のセットアップが完了したら、ターミナルを開いて以下のコマンドを実行し、Azure 環境にログインします。

```bat
> azd auth login
```

Azure CLI のログイン時と同じように、ブラウザーが自動的に立ち上がり認証を促されますので、Azure へのアクセス権のあるユーザーにてログインを行ってください。

### Azure Developer CLI の初期設定
azd セットアップ後のターミナル再起動が完了したら、以下のコマンドを実行し、操作対象のサブスクリプションを指定しつつ動作確認します。

```bat
azd config set defaults.subscription サブスクリプションID
```
!> サブスクリプション ID は、先ほど Azure CLI のセットアップ時にメモした id の値 (GUID) を入力します

## Python のインストール
今回のハンズオンでは、Python 3.9 で書かれたサンプルコードを利用します。（最新版にまだアップデートできておらずすみません！）

[Python のダウンロードサイト](https://www.python.org/downloads/)から、お使いの OS に合った Python 3.9 系のインストーラーをダウンロードしてきて、インストールしてください。


Download Python | Python.org<br>
https://www.python.org/downloads/

## Node.js のインストール
今回のハンズオンでは、Node.js で書かれたサンプルコードも利用します。

[Node.js のダウンロードサイト](https://nodejs.org/en/download) からお使いの OS にあった Node.js 最新版のインストーラーをダウンロードしてインストールしてください。

Downloads | Node.js<br>
https://nodejs.org/en/download

## Git CLI のインストール
今回のハンズオンではサンプルソースコードのダウンロードに Azure Developer CLI の機能を利用しますが、その内部で Git CLI が利用されているためインストールが必要です。

[Git CLI のダウンロードサイト](https://git-scm.com/downloads) からお使いの OS にあわせた Git CLI のバージョンをダウンロードしてインストールしてください。

> インストール時に色々聞かれますが、基本的には全てデフォルト設定で問題ないはずです。

## PowerShell 7 のインストール
Windows 環境でハンズオンを実施中の方は、PowerShell 7 のセットアップも必要です、

[PowerShell 7 のダウンロードサイト](https://github.com/PowerShell/PowerShell/releases/tag/v7.3.6) よりインストーラーをダウンロードして、インストールしてください。

PowerShell<br>
https://github.com/PowerShell/PowerShell/releases/tag/v7.3.6

インストール途中に出てくる下記ダイアログの、下二つのチェックは入れておくとコンテキストメニューから PowerShell を開けるようになっておススメです。
![PowerShell のインストールオプション](./img/powershellinstall.png)

## .NET SDK のインストール
今回のハンズオンのサンプルソースコードに、.NET 7.0 ベースのソリューションが含まれているためインストールが必要です。

[.NET 7 のダウンロードサイト](https://dotnet.microsoft.com/ja-jp/download/dotnet/7.0) より最新の SDK をダウンロードしてインストールしてください。

.NET SDK<br>
https://dotnet.microsoft.com/ja-jp/download/dotnet/7.0

## Visual Studio Code の準備
### Visual Studio Code のインストール
まずは[Visual Studio Code のダウンロードサイト](https://code.visualstudio.com/download)から、ご利用されている OS にあわせた Visual Studio Code をダウンロードし、インストーラーを実行してインストールします。

Download Visual Studio Code<br>
https://code.visualstudio.com/download

> VS Code のセットアップ時に、以下の追加タスクのダイアログの、上二つのチェックボックス（[Code で開く] アクションを追加する）にチェックを入れておくと、あとから VS Code を開くときに楽になりますのでおススメです。
![VS Code Setup Dialog](./img/VSCodeSetup001.png)



### Visual Studio Code に Extensions をインストールする
VS Code への Extensions のインストールは、VS Code 起動後のサイドバーにある "Extensions" のアイコンから行います。
下記スクリーンショットの赤枠で囲んだアイコンです。

![VS Code Extensions](./img/VSCodeExt001.png)

"Search Extensions in Marketplace" のテキストボックスから、名前検索が出来ますので、以下にリストした Extensions を探してインストールしてください。

- Python
- Python Extension Pack
- Azure Tools
  - Azure Tools のセットアップ途中で、azd のセットアップや Azure へのログインなど求められますが、最低限 Azure へのログインさえ出来ていればあとは必要な時に必要な操作を求められるはずなので、一番下の "Mark Done" をクリックして完了させてしまって構いません。
- C#

完了したら、VS Code にフォーカスを当てた状態で <kbd>F1</kbd> キーを押して、"reload" と検索し "Developer: Reload Window" を実行し、VS Code に先ほどインストールしたプラグインが正しく読み込まれるようにしてください。

# 補足情報
## VS Code で Python を利用可能にするところまでのトレーニング
Microsoft Learn に VS Code に Python をセットアップする部分をまとめた技術トレーニングがありました。
こちらも是非参考にしてみてください。

Visual Studio Code で Python を使ってみる<br>
https://learn.microsoft.com/ja-jp/training/modules/python-install-vscode/

## VS Code で Azure にアクセスする部分の公式ドキュメント
Azure 開発用に VS Code をセットアップする方法はこちらにもまとまっていますのでご確認ください。

Azure 開発用に Visual Studio Code を構成する<br>
https://learn.microsoft.com/ja-jp/dotnet/azure/configure-vs-code

#### ローカル開発環境 ----
このデモをデプロイするためには、ローカルに以下の開発環境が必要です。
> **重要** このサンプルは Windows もしくは Linux 環境で動作します。ただし、WSL2 の環境では正常に動作しません。
- [Azure Developer CLI](https://aka.ms/azure-dev/install) （version 1.0.2以降推奨）
- [Azure CLI](https://learn.microsoft.com/cli/azure/install-azure-cli) （version 2.50.0以降推奨）
- [Python 3+](https://www.python.org/downloads/)（version 3.11以降推奨）
    - **重要**: Windows 環境では、python および pip を Path 環境変数に含める必要があります。
    - **重要**: `python --version` で現在インストールされている Python のバージョンを確認することができます。 Ubuntu を使用している場合、`sudo apt install python-is-python3` で `python` と `python3` をリンクさせることができます。    
- [Node.js](https://nodejs.org/en/download/)（version 14.18以降推奨）
- [Git](https://git-scm.com/downloads)
- [Powershell 7+ (pwsh)](https://github.com/powershell/powershell) - Windows で実行する場合のみ
   - **重要**: `pwsh.exe` が PowerShell コマンドとして実行できることを確認して下さい。

>注意: 実行するユーザの AAD アカウントは、`Microsoft.Authorization/roleAssignments/write` 権限を持っている必要があります。この権限は [ユーザーアクセス管理者](https://learn.microsoft.com/azure/role-based-access-control/built-in-roles#user-access-administrator) もしくは [所有者](https://learn.microsoft.com/azure/role-based-access-control/built-in-roles#owner)が保持しています。  
`az role assignment list --assignee <your-Azure-email-address> --subscription <subscription-id> --output table`

### インストール

#### プロジェクトの初期化

1. このリポジトリをクローンし、フォルダをターミナルで開きます。(Windows の場合は pwsh ターミナルで実行する例です)
1. `azd auth login` を実行します。
1. `azd init` を実行します。
    * 現在、このサンプルに必要な Azure Open AI のモデルは該当モデルをサポートしている**東日本**リージョンにデプロイすることが可能です。最新の情報は[こちら](https://learn.microsoft.com/en-us/azure/cognitive-services/openai/concepts/models)を参考にしてください。

#### スクラッチでの開始

新規に環境をデプロイする場合は、以下のコマンドを実行してください。

1. `az login`と`az account set -s YOUR_SUBSCRIPTION_ID`後に、`az ad user show --id your_account@your_tenant -o tsv --query id` を実行して、操作をするユーザの AAD アカウントのオブジェクトID を取得します。
1. 取得したオブジェクトID を環境変数`AZURE_PRINCIPAL_ID`にセットします。
    - Windows 環境で実行している場合は、`$Env:AZURE_PRINCIPAL_ID="Your Object ID"`を実行します。
    - Linux 環境で実行している場合は、`export AZURE_PRINCIPAL_ID="Your Object ID"`を実行します。
1. `azd up` を実行します。- このコマンドを実行すると、Azure上に必要なリソースをデプロイし、アプリケーションのビルドとデプロイが実行されます。また、`./data`配下の PDF を利用して Search Index を作成します。
    - Linux 環境で実行している場合は、`chmod +x scripts/prepdocs.sh`
1. コマンドの実行が終了すると、アプリケーションにアクセスする為の URL が表示されます。この URL をブラウザで開き、サンプルアプリケーションの利用を開始してください。  

コマンド実行結果の例：

!['Output from running azd up'](assets/endpoint.png)
    
> 注意: アプリケーションのデプロイ完了には数分かかることがあります。"Python Developer" のウェルカムスクリーンが表示される場合は、数分待ってアクセスし直してください。

#### アプリケーションのローカル実行 {#run_app_locally}
アプリケーションをローカルで実行する場合には、以下のコマンドを実行してください。`azd up`で既に Azure 上にリソースがデプロイされていることを前提にしています。

1. `azd login` を実行する。
2. `src` フォルダに移動する。
3. `./start.ps1` もしくは `./start.sh` を実行します。

##### VS Codeでのデバッグ実行
1. `src\backend`フォルダに異動する
2. `code .`でVS Codeを開く
3. Run>Start Debugging または F5

#### FrontendのJavaScriptのデバッグ
1. src/frontend/vite.config.tsのbuildに`minify: false`を追加
2. ブラウザのDeveloper tools > Sourceでブレイクポイントを設定して実行

### GPT-4モデルの利用
2023年6月現在、GPT-4 モデルは申請することで利用可能な状態です。このサンプルは GPT-4 モデルのデプロイに対応していますが、GPT-4 モデルを利用する場合には、[こちら](https://learn.microsoft.com/ja-jp/azure/cognitive-services/openai/how-to/create-resource?pivots=web-portal#deploy-a-model)を参考に、GPT-4 モデルをデプロイしてください。また、GPT-4 モデルの利用申請は[こちらのフォーム](https://aka.ms/oai/get-gpt4)から可能です。

GPT-4 モデルのデプロイ後、以下の操作を実行してください。

1. このサンプルをデプロイした際に、プロジェクトのディレクトリに `./${環境名}/.env` ファイルが作成されています。このファイルを任意のエディタで開きます。
1. 以下の行を探して、デプロイした GPT-4 モデルのデプロイ名を指定してください。
> AZURE_OPENAI_GPT_4_DEPLOYMENT="" # GPT-4モデルのデプロイ名
AZURE_OPENAI_GPT_4_32K_DEPLOYMENT="" # GPT-4-32Kモデルのデプロイ名

1. `azd up` を実行します。

GPT-4 モデルは、チャット機能、文書検索機能のオプションで利用することができます。

### Easy Authの設定（オプション）
必要に応じて、Azure AD に対応した Easy Auth を設定します。Easy Auth を設定した場合、UI の右上にログインユーザのアカウント名が表示され、チャットの履歴ログにもアカウント名が記録されます。
Easy Auth の設定は、[こちら](https://learn.microsoft.com/ja-jp/azure/app-service/scenario-secure-app-authentication-app-service)を参考にしてください。

</code>
</pre>
</details>
