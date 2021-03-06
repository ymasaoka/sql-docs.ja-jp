---
title: SharePoint サイトのレポート ビューアー Web パーツ - SSRS | Microsoft Docs
description: Report Viewer カスタム Web パーツを使用し、SharePoint サイトで SQL Server Reporting Services レポートを表示、移動、印刷、エクスポートします。
ms.date: 02/11/2020
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server-sharepoint
ms.topic: conceptual
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 86e6b09b6bfefaff1d7e15205e4cc5897d3a6ad5
ms.sourcegitcommit: 66a0672e47415dbd5cfd8d19075102c8c3973e70
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/21/2020
ms.locfileid: "83765020"
---
# <a name="report-viewer-web-part-on-a-sharepoint-site---reporting-services"></a>SharePoint サイトのレポート ビューアー Web パーツ - Reporting Services

[!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016-and-later](../../includes/ssrs-appliesto-2016-and-later.md)]  [!INCLUDE[ssrs-appliesto-pbirsi](../../includes/ssrs-appliesto-pbirs.md)] [!INCLUDE[ssrs-appliesto-sharepoint-2013-and-later](../../includes/ssrs-appliesto-sharepoint-2013-and-later.md)] [!INCLUDE[ssrs-appliesto-not-sharepoint-online](../../includes/ssrs-appliesto-not-sharepoint-online.md)]

レポート ビューアー Web パーツはカスタム Web パーツです。 Web パーツを使うと、SharePoint サイト内のレポート サーバーにあるレポートの表示、ナビゲーション、印刷、エクスポートを行うことができます。 レポート ビューアー Web パーツは、Microsoft SQL Server Reporting Services レポート サーバーによって処理されるレポート定義 (.rdl) ファイルに関連付けられています。 

最新のレポート ビューアー Web パーツは、Power BI Report Server に展開されたページ分割されたレポートを処理することもできます。 Web パーツは、Power BI レポートでは機能しません。

## <a name="why-the-report-viewer-web-part-is-re-introduced"></a>レポート ビューアー Web パーツが再度導入された理由

レポート ビューアー Web パーツは、SharePoint 製品用の Reporting Services アドインの一部として利用できました。 Web パーツは、SharePoint 統合モードのレポート サーバーに固有のものでした。 SQL Server 2016 の後で、SharePoint 統合モードは非推奨となりました。

SQL Server 2017 以降では、Reporting Services のインストール モードは**ネイティブ モード**だけです。 ページ ビューアー Web パーツを使うすべてのレポートの種類は、*rs:Embed=true* URL パラメーターを使って埋め込むことができます。 SharePoint ページへのレポートの埋め込みはユーザーから要望のあった統合方法であり、更新されたレポート ビューアー Web パーツはページ分割されたレポートでこのシナリオを可能にします。

ページ分割されたレポートを SharePoint ページに埋め込むにはページ ビューアー Web パーツで十分ですが、更新されたレポート ビューアー Web パーツはそれ以外の機能も提供します。

* 特定のツール バー ボタンの表示/非表示
* レポートのパラメーター値のオーバーライド
* レポート パラメーターへのフィルター Web パーツの接続

## <a name="download-the-report-viewer-web-part-solution-package"></a>レポート ビューアー Web パーツのソリューション パッケージのダウンロード

レポート ビューアー Web パーツは、Microsoft ダウンロード センターから入手できます。

[レポート ビューアー Web パーツのソリューション パッケージのダウンロード](https://www.microsoft.com/download/details.aspx?id=55949)

## <a name="considerations-and-limitations"></a>考慮事項と制限事項

以下の項目は、更新されたレポート ビューアー Web パーツに固有のものです。

* Web パーツは、"*クラシック*" SharePoint ページでのみ使うことができます。
* レポート ビューアー Web パーツでの埋め込みでサポートされているのは、ページ分割された (RDL) レポートだけです。 Power BI レポートまたはモバイル レポートを埋め込む場合は、*rs:Embed=true* URL パラメーターを使うことができます。

## <a name="next-steps"></a>次のステップ

更新されたレポート ビューアー Web パーツを使い始めるには、「[Deploy the Report Viewer web part on a SharePoint site](deploy-report-viewer-web-part.md)」(SharePoint サイトにレポート ビューアー Web パーツを展開する) を参照してください。
