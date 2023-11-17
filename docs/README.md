# Azure OpenAI 入門ハンズオンラボ

このハンズオンラボでは、以下のことをカバーします。

- このハンズオンラボを始めるまえに必要な環境設定
- Azure Conatiner Apps を用いたアプリケーションの実装
- Azure Cosmos DB を用いたアプリケーションの実装
- ChatGPT ライクなインターフェースを使用して企業の社内文書を検索するアプリケーションの実装


### 前提知識
- **Azureの基礎**: Azure Portalの使い方、Azure CLIの使い方、Azureリソースの概念、RBAC等のAzureの基礎が前提知識になります。自信がない場合は、[Microsoft Azure Virtual Training Day: Azureの基礎](https://www.microsoft.com/ja-jp/events/top/training-days/azure?activetab=pivot:azure%E3%81%AE%E5%9F%BA%E7%A4%8Etab)等の活用を推奨します。
- **Azure OpenAI Serviceの基礎**: Azure OpenAI Serviceとは何かを理解している必要があります。[Azure OpenAI Developers セミナー](https://www.youtube.com/watch?v=ek3YWrHD76g)をご覧いただければ、最低限の基礎は身に付きます。
- **PowerShellやBash等のコマンドラインツールの使い方の基礎**: 自信がない場合は、[Introduction to PowerShell](https://learn.microsoft.com/training/modules/introduction-to-powershell/)や[Introduction to Bash](https://learn.microsoft.com/training/modules/bash-introduction/)をご活用ください
- **VS Code等のコードエディタの使い方の基礎**: 自信がない場合は、[Introduction to Visual Studio Code](https://learn.microsoft.com/training/modules/introduction-to-visual-studio-code/)をご活用ください
 

## 本番稼働を視野にいれる場合の考慮事項
本番稼働（や本番に近い検証環境等）を視野にいれる場合、様々な考慮事項があります。考えられる考慮事項は[Cloud Adoption Framework](https://learn.microsoft.com/azure/cloud-adoption-framework/overview)や[Well-Architected Framework](https://learn.microsoft.com/azure/well-architected/)にまとめられていますが、考慮事項は多岐にわたるので、状況に応じて重要度や緊急度等をもとにした優先順位付けが必要になります。例えば、社内データと連携する場合にはプライベートネットワークを考慮した設計や企業のAzure基盤との連携が重要になること多くなるとことが推測されます。

## 制限事項

本ドキュメントの使用においては、次の制限、制約をご理解の上、活用ください。

+ 目的外利用の禁止  
本ドキュメントは Microsoft Azure 上において、システムやソリューションの円滑かつ安全な構築に資することを目的に作成されています。この目的に反する利用はお断りいたします。
+ フィードバック  
本ドキュメントの記載内容へのコメントやフィードバックをいただけます場合は、[コンテンツ原盤管理用のリポジトリ](https://github.com/tokawa-ms/AOAI_101_Handson) への Pull Request もしくは Issue にてお知らせください。なお、個別質問への回答やフィードバックへの対応はお約束いたしかねますこと、ご承知くださいますようお願い申し上げます。
+ 公式情報の確認  
本ドキュメントは有志のエンジニアにより、Microsoft が提供する OpenAI の参考実装の分かりやすいデプロイ手順を共有する為に作成されたものです。そのため、本ドキュメントの記載内容については Microsoft として公式に表明されたものではなく、日本マイクロソフトおよび米国 Microsoft Corporation は一切の責任を負いません。また、本ドキュメントの記載内容について Azure サポートへお問い合わせいただいても、回答することはできません。  
Microsoft Azure の公式情報については、Azure のドキュメントをご確認ください。
+ 免責  
本ドキュメントの記載内容によって発生したいかなる損害についても、日本マイクロソフトおよび米国 Microsoft Corporation は一切の責任を負いません。

## Trademarks

This project may contain trademarks or logos for projects, products, or services. Authorized use of Microsoft 
trademarks or logos is subject to and must follow 
[Microsoft's Trademark & Brand Guidelines](https://www.microsoft.com/en-us/legal/intellectualproperty/trademarks/usage/general).
Use of Microsoft trademarks or logos in modified versions of this project must not cause confusion or imply Microsoft sponsorship.
Any use of third-party trademarks or logos are subject to those third-party's policies.