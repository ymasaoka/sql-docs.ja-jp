---
title: レポートのキャッシュ | Microsoft Docs
description: キャッシュに残っていれば処理済みレポートの表示速度を上げる、レポート マネージャーのレポートのキャッシュについて説明します。
ms.date: 05/14/2019
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server
ms.topic: conceptual
helpviewer_keywords:
- report execution properties [Reporting Services]
- cache [Reporting Services]
- report processing [Reporting Services], caching
- cached instances [Reporting Services]
- refreshing cache
- cached reports [Reporting Services]
- preloading cache
- invalid cached reports [Reporting Services]
- performance [Reporting Services]
- expiration [Reporting Services]
- snapshots [Reporting Services], caching
ms.assetid: 146542c3-8efd-4b89-a8d8-77d22896630e
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 43a03b47b1d18569f56dae2652144af8c7e11adf
ms.sourcegitcommit: f0772f614482e0b3cde3609e178689ce62ca3a19
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/09/2020
ms.locfileid: "84548064"
---
# <a name="caching-reports-ssrs"></a>複数のレポートのキャッシュ (SSRS)
  レポート サーバーでは、処理済みレポートのコピーをキャッシュして、ユーザーがレポートを開いたときにそのコピーを返すことができます。 レポートがキャッシュされたコピーかどうかをユーザーが判断できる唯一の方法は、レポートが実行された日時を確認することです。 その日時が現在の日時ではなく、レポートがスナップショットでない場合、そのレポートはキャッシュから取得されたレポートです。  
  
 レポートのサイズが大きい場合やレポートへのアクセスが頻繁に行われる場合、キャッシュを行うことでレポートの取得にかかる時間を短縮できます。 サーバーを再起動した場合、レポート サーバー Web サービスをオンラインに戻すと、キャッシュされたすべてのインスタンスが修復されます。  
  
 キャッシュは、パフォーマンスを向上させる技法です。 キャッシュのコンテンツは変動するので、レポートを追加、置換、または削除すると変更できます。 より予測可能なキャッシュ方法が必要な場合、レポート スナップショットを作成する必要があります。 詳細については、「 [レポート処理プロパティの設定](../../reporting-services/report-server/set-report-processing-properties.md)」を参照してください。  
  
> [!NOTE]  
>  [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] では、データベースに一時ファイルが保持され、ユーザー セッションやレポート処理のサポートに使用されます。 これらのファイルは内部使用を目的にキャッシュされ、単一のブラウザー セッションの間、表示状態の整合性を保つために使用されます。 内部使用を目的とした一時ファイルのキャッシュ方法の詳細については、「[レポート サーバー データベース &#40;SSRS ネイティブ モード&#41;](../../reporting-services/report-server/report-server-database-ssrs-native-mode.md)」を参照してください。  
  
## <a name="cached-instances"></a>キャッシュされたインスタンス  
 キャッシュされたレポートのインスタンスは、レポートの中間形式に基づいています。 通常、レポート サーバーでは、レポート名に基づいてレポートのインスタンスが 1 つキャッシュされます。 ただし、クエリ パラメーターに基づいてレポートに別のデータを含めることができる場合、任意の指定された時間に複数のバージョンのレポートがキャッシュされることがあります。 たとえば、パラメーター値として地域コードを取得するパラメーター化されたレポートがあるとします。 4 人の異なるユーザーが、4 つの一意の地域コードを指定した場合、キャッシュされたコピーが 4 つ作成されます。  
  
 一意の地域コードを含んだレポートを実行する最初のユーザーは、その地域のデータを含むキャッシュされたレポートを作成します。 同じ地域コードを使用してレポートを要求する後続のユーザーは、キャッシュされたコピーを取得します。  
  
 すべてのレポートをキャッシュできるわけではありません。 レポートにユーザー依存データが含まれる場合、レポートで資格情報の入力が必要となる場合、またはレポートで Windows 認証が使用される場合は、レポートをキャッシュできません。  
  
## <a name="refreshing-the-cache"></a>キャッシュの更新  
 以前にキャッシュされたコピーの有効期限が切れた後でユーザーがレポートを選択すると、キャッシュされたレポートが新しいバージョンに置き換えられます。 キャッシュされたインスタンスとして実行するように構成されたレポートは、有効期限の設定に基づいてキャッシュから定期的に削除されます。 レポートの有効期限は分単位で設定することも、スケジュールされた時間に設定することもできます。どちらの設定を使用するかは、データに要求される即時性に基づいて決定します。 SOAP API を使用する以外の方法で、キャッシュからレポートを直接削除することはできません。  
  
 キャッシュの有効期限を構成するには、共有スケジュールまたはレポート固有スケジュールを使用します。 共有スケジュールを使用し、その後に続けて共有スケジュールを一時停止した場合、スケジュールが停止している間はキャッシュ期限が切れることはありません。 続けて共有スケジュールを削除した場合、スケジュール設定のコピーがレポート固有スケジュールとして保存されます。  
  
 スケジュールの有効期限が切れた場合、またはキャッシュの有効期限日にスケジュール エンジンを利用できない場合は、スケジュールを延長するか、スケジュールされているサービスを開始することで、スケジュールされた操作を再開できるまでレポート サーバーでアクティブなレポートが実行されます。  
  
## <a name="preloading-the-cache"></a>キャッシュを事前に読み込む  
 キャッシュの事前読み込みを使用すると、サーバーのパフォーマンスを向上できます。 パラメーター化されたレポートのインスタンスのコレクションをキャッシュに事前に読み込むには、次の 2 つの方法があります。  
  
1.  キャッシュの更新プランを作成します。 更新プランを作成すると、1 つのレポートのスケジュールや共有スケジュールを指定できます。  
  
2.  NULL 配信プロバイダーを使用するデータ ドリブン サブスクリプションを作成します。 NULL 配信プロバイダーを配信方法としてサブスクリプションに指定すると、レポート サーバーはレポート サーバー データベースを配信先に設定し、NULL 表示拡張機能と呼ばれる特殊な表示拡張機能を使用します。 NULL 配信プロバイダーでは、他の配信拡張機能とは異なり、サブスクリプションの定義から構成できる配信設定がありません。  
  
 レポートのキャッシュは、パラメーター化されたレポートの複数のインスタンスをキャッシュする場合に特に役立ちます。この場合、さまざまなレポートのインスタンスを生成するために個別のパラメーター値が使用されます。 レポートには、クエリ ベースのパラメーターのみを指定できることに注意してください。  
  
 スケジュールを指定する場合や、データ ドリブン サブスクリプションを作成する場合は、キャッシュに対するレポートの配信頻度をスケジュールする必要があります。 古いコピーの有効期限が切れていない場合、新しいコピーはキャッシュに配信されません。 したがって、レポートの実行プロパティの構成には、キャッシュの有効期限の設定を含める必要があります。 この有効期限の設定には、定義するサブスクリプションのスケジュールとの一貫性が必要です。 たとえば、毎晩実行されるサブスクリプションを作成する場合、サブスクリプションが実行される前に、キャッシュも毎晩、有効期限切れになる必要があります。 実行プロパティに有効期限の日時が含まれていない場合、新しい配信が無視されます。 キャッシュの更新の詳細については、「 [スケジュール](../../reporting-services/subscriptions/schedules.md)」を参照してください。 プロパティの設定方法については、「 [レポート処理プロパティの設定](../../reporting-services/report-server/set-report-processing-properties.md)」を参照してください。 データ ドリブン サブスクリプションの使用に関する詳細については、「 [データ ドリブン サブスクリプション](../../reporting-services/subscriptions/data-driven-subscriptions.md)」を参照してください。  
  
## <a name="conditions-that-cause-cache-expiration"></a>キャッシュが有効期限切れとなる条件  
 レポート定義の変更、レポート パラメーターの変更、データ ソースの資格情報の変更、レポート実行オプションの変更などのイベントが発生すると、キャッシュされたレポートが無効になります。 キャッシュに格納されたレポートを削除する場合、キャッシュされたバージョンのレポートも削除されます。  
  
 何らかの理由で、キャッシュされたインスタンスでレポートを実行できない場合 (たとえば、ユーザーが指定したパラメーター値が、キャッシュされたレポートの生成に使用するパラメーター値と異なる場合)、レポート サーバーでレポートが再実行されます。  
  
## <a name="see-also"></a>関連項目  
 [処理オプションの設定 &#40;Reporting Services の SharePoint 統合モード&#41;](../../reporting-services/report-server-sharepoint/set-processing-options-reporting-services-in-sharepoint-integrated-mode.md)   
 [レポート処理プロパティの設定](../../reporting-services/report-server/set-report-processing-properties.md)   
 [Reporting Services の概念 &#40;SSRS&#41;](../../reporting-services/reporting-services-concepts-ssrs.md)   
 [キャッシュを事前に読み込む](../../reporting-services/report-server/preload-the-cache-report-manager.md)   
 [[スケジュール]](../../reporting-services/subscriptions/schedules.md)   
 [共有データセットのキャッシュ &#40;SSRS&#41;](../../reporting-services/report-server/cache-shared-datasets-ssrs.md)   
  
  
