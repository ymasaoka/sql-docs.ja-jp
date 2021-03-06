---
title: 可用性グループの圧縮の調整 | Microsoft Docs
description: SQL Server で可用性グループのデータ ストリームが圧縮されるしくみと、その結果、ネットワーク トラフィックの削減、CPU 負荷の増加、待機時間が生じる可能性について説明します。
ms.custom: ''
ms.date: 06/22/2016
ms.prod: sql
ms.technology: high-availability
ms.topic: conceptual
ms.assetid: 7632769c-b246-4766-886f-7c60ec540be8
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 635a9534f12d8ac48eaa0c331371ac2875577a91
ms.sourcegitcommit: 2f868a77903c1f1c4cecf4ea1c181deee12d5b15
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/02/2020
ms.locfileid: "91670405"
---
# <a name="tune-compression-for-availability-group"></a>可用性グループの圧縮の調整
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
既定では、SQL Server は、可用性グループにとって適切な場合にデータ ストリームを圧縮します。 圧縮によって、ネットワーク トラフィックが削減され、CPU の負荷が増加し、遅延が引き起こされる可能性があります。 圧縮を有効にするには、sysadmin 固定サーバー ロールのメンバーである必要があります。 次の表は、SQL Server が可用性グループのログ ストリームに対して圧縮を使用するタイミングを示します。

| シナリオ | 圧縮の設定
| ---- | ----
| 同期コミット レプリカ | 非圧縮
| 非同期コミット レプリカ | Compressed
| 自動シードの処理中 | 非圧縮
| データベースでの TDE の有効化  | 非圧縮

## <a name="trace-flags-for-availability-group-compression"></a>可用性グループ圧縮のトレース フラグ 

ほとんどのシナリオでは、これらの設定を変更しないことをお勧めします。 グローバル トレース フラグを使用して、これらの設定の変更をテストできます。 SQL Server は、グローバル トレース フラグをインスタンス全体に適用します。 インスタンスのすべての可用性グループがこれらの設定の影響を受けます。  

次の表は、SQL Server の既定の圧縮動作を変更するトレース フラグを示します。 

トレース フラグ | 説明
------------- | -------------
1462          | 同期レプリカを使用した可用性グループのログ ストリーム圧縮を無効にします。 この機能は、非同期レプリカでは既定で有効にされており、ネットワーク帯域幅が最適化されます。
9567          | 自動シード処理時の可用性グループのデータ ストリーム圧縮を有効にします。 自動シードの処理中、圧縮によって転送時間が大幅に短縮され、プロセッサの負荷が増大します。
9592          | 同期レプリカを使用した可用性グループのログ ストリーム圧縮を有効にします。 この機能は、圧縮によって待機時間が追加されるため、同期レプリカでは既定で無効にされています。 非同期レプリカのログ ストリーム圧縮は同期レプリカに対しては既定で有効にされています。


## <a name="resources"></a>リソース


[データベース エンジンのスタートアップ オプション](../../../database-engine/configure-windows/database-engine-service-startup-options.md)

[自動シード処理](./automatically-initialize-always-on-availability-group.md)

[Always On の前提条件](prereqs-restrictions-recommendations-always-on-availability.md)