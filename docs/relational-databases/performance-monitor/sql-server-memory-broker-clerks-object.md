---
title: SQL Server、Memory Broker Clerks オブジェクト | Microsoft Docs
description: Memory Broker Clerk に関する統計情報のカウンターを提供する SQLServer:Memory Broker Clerks パフォーマンス オブジェクトについて説明します。
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- SQLServer:Memory Broker Clerks
ms.assetid: 47b9c236-66a3-4c42-97ee-da5555bdc046
author: julieMSFT
ms.author: jrasnick
ms.openlocfilehash: 728a660b3737410e9f235cb8632cbf948f7bcf82
ms.sourcegitcommit: 9470c4d1fc8d2d9d08525c4f811282999d765e6e
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/17/2020
ms.locfileid: "86458801"
---
# <a name="sql-server-memory-broker-clerks-object"></a>SQL Server、Memory Broker Clerks オブジェクト
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
**SQLServer:Memory Broker Clerks** パフォーマンス オブジェクトは、Memory Broker Clerk に関する統計情報のカウンターを提供します。

次の表では、SQL Server の **Memory Broker Clerks** パフォーマンス オブジェクトについて説明します。

|**SQL Server Memory Broker Clerks のカウンター**|説明|  
|-------------|-----------------|  
|**内部の利点**|エントリ数負荷に対するメモリの内部値 (ミリ秒/ページ/ミリ秒単位) に 100 億を掛けて小数点以下を切り捨てたもの。|
|**Memory Broker Clerk のサイズ**|Clerk のサイズ (ページ単位)。|
|**定期的な削除 (ページ)**|前回の定期的な削除によって Broker Clerk から削除されたページ数。|
|**メモリ不足による削除 (ページ/秒)**|メモリ不足によって Broker Clerk から削除された 1 秒あたりのページ数。|
|**シミュレーションの利点**|Clerk に対するメモリの値 (ミリ秒/ページ/ミリ秒単位) に 100 億を掛けて小数点以下を切り捨てたもの。|
|**シミュレーションのサイズ**|クラーク シミュレーションの現在のサイズ (ページ単位)。|

バッファー プールと列ストア オブジェクト プールのカウンターのインスタンスがあります。

## <a name="see-also"></a>参照  
[リソースの利用状況の監視 (システム モニター)](../../relational-databases/performance-monitor/monitor-resource-usage-system-monitor.md)
