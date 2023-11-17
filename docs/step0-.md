# Chat+社内文書検索

## 概要
このデモは、ChatGPT ライクなインターフェースを使用して企業の社内文書を検索するアプリケーションの実装パターンです。デモアプリを利用するためには、Azure Open AI の ChatGPT(gpt-35-turbo) モデルと、Azure Cognitive Search、他にいくつかのリソースの作成が必要です。

このリポジトリでは、サンプルデータに[厚生労働省のモデル就業規則](https://www.mhlw.go.jp/stf/seisakunitsuite/bunya/koyou_roudou/roudoukijun/zigyonushi/model/index.html)を使用しています。

デモアプリは以下のように動作します。

## Architecture
![RAG Architecture](assets/appcomponents.png)

## UI
![Chat screen](assets/chatscreen.png)

## 本番稼働を視野にいれる場合の考慮事項
本番稼働（や本番に近い検証環境等）を視野にいれる場合、様々な考慮事項があります。考えられる考慮事項は[Cloud Adoption Framework](https://learn.microsoft.com/azure/cloud-adoption-framework/overview)や[Well-Architected Framework](https://learn.microsoft.com/azure/well-architected/)にまとめられていますが、考慮事項は多岐にわたるので、状況に応じて重要度や緊急度等をもとにした優先順位付けが必要になります。例えば、社内データと連携する場合にはプライベートネットワークを考慮した設計や企業のAzure基盤との連携が重要になること多くなるとことが推測されます。

### Azure共通基盤との連携
PoC/検証等の目的で小さく始めた後に、本番稼働を視野にいれる場合には、社内ガバナンス等を意識する必要があります。その際は、[Azure Cloud Adoption Framework](https://learn.microsoft.com/azure/cloud-adoption-framework/overview)で紹介されている [Azure Enterprise Scale Landing Zone](https://learn.microsoft.com/azure/cloud-adoption-framework/ready/landing-zone/)や[Azure CAF Landing Zones 設計・構築ハンズオン](https://github.com/nakamacchi/AzureCAF.LandingZones.Demo)等を参考にすることを推奨します。Azure Enterprise Scale Landing Zoneの概念において、こちらのサンプルアプリケーションは、[アプリケーションランディングゾーン](https://learn.microsoft.com/azure/cloud-adoption-framework/ready/landing-zone/#platform-landing-zones-vs-application-landing-zones)内にデプロイされるものに相当します。

### プライベートネットワーク構成
- [Bicepを活用した構築ガイダンス](https://github.com/Azure-Samples/jp-azureopenai-samples/blob/main/5.internal-document-search/deploy_private_endpoint_ennabled.md)
- [Azure CLIを活用した構築ガイダンス](https://github.com/nakamacchi/AzureCAF.LandingZones.Demo/blob/master/41.Spoke%20D%20(AOAI)%20%E7%A4%BE%E5%86%85%E6%96%87%E6%9B%B8%E6%A4%9C%E7%B4%A2%20%E3%82%A4%E3%83%B3%E3%83%95%E3%83%A9%E6%A7%8B%E7%AF%89/41_00_%E6%9C%AC%E3%82%B5%E3%83%B3%E3%83%97%E3%83%AB%E3%81%AB%E3%81%A4%E3%81%84%E3%81%A6.md)

### ナレッジベース設計
#### Azure Search Indexの作成・更新方法
本サンプルでは、`data/`ディレクトリにあるデータを、`scripts`にあるスクリプトでAzureにアップロードし、Azure Search Indexを作成する方法をとっています。Indexの更新のしやすさや検索結果の改善をする場合、サンプルのスクリプトを部分的に修正するのではなく、要件を整理した上で更新用のアプリケーション構築の用意等についての検討を推奨します。

#### 検索結果の「良さ」や「精度」改善するための設計
生成AIは確率的な性質を持つため、同じ入力に対しても異なる出力が生成される可能性があります。このような性質は、生成AIが多様な出力を生成できる利点でもありますが、一方で評価が難しくなる場合もあります。そのため、生成AIの「良さ」や「精度」を一様に評価するのは困難であり、用途や目的に応じて評価基準が設定されることが多いです。

生成AIの「良さ」や「精度」を一様に評価するのは困難ではありますが、以下の様なことを考慮すると、エンドユーザの検索結果に対する満足度を高めることができる可能性があります。
- **PDFファイルの分割方法**: 本サンプルでは与えられたPDFを機械的にページ単位で分割していますが。PDFファイルの分割方法の改善することにより検索結果の良さを高めることができる可能性があります。
- **画像・複雑な図表・PDF以外のファイルフォーマットの扱い**: 本サンプルはPDFファイルの扱いを前提としており、画像や複雑な図表を検索結果で活用することはできません。画像・複雑な図表・PDF以外のファイルフォーマットの扱いも重要なユースケースの場合、ナレッジベース設計を見直す必要がある可能性があります。
- **各モデルの最大トークン数による制約**: 本サンプルの文書検索の結果から返答を作成する過程において、最大トークン数の制約から文書の1024トークンのみを利用しています。この制約により、文書の内容を十分に活用できていない可能性があります。この制約を緩和することにより、検索結果の精度を高めることができる可能性があります。