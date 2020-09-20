---
title: nested triggers サーバー構成オプションの構成 | Microsoft Docs
description: nested triggers オプションについて説明します。 これを使用して、SQL Server でカスケードできる AFTER トリガーのレベル数を設定する方法を説明します。
ms.custom: ''
ms.date: 03/02/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- nested triggers option
ms.assetid: 29d7372b-d406-4a5b-80c6-a2d231d25211
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 9b236dc0cb810a6ba8d63ef7c68367eab2252b8b
ms.sourcegitcommit: da88320c474c1c9124574f90d549c50ee3387b4c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/01/2020
ms.locfileid: "85758257"
---
# <a name="configure-the-nested-triggers-server-configuration-option"></a>nested triggers サーバー構成オプションの構成
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  このトピックでは、 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]または [!INCLUDE[tsql](../../includes/tsql-md.md)]を使用して、 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] を使用して、 **nested triggers** サーバー構成オプションを構成する方法について説明します。 **nested triggers** オプションは、AFTER トリガーを連鎖できるかどうかを制御します。 つまり、1 つの操作が別のトリガーを開始し、開始されたトリガーからさらに別のトリガーを開始するなどの動作ができるかどうかを制御します。 **nested triggers** を 0 に設定すると、AFTER トリガーは連鎖できません。 **nested triggers** を 1 (既定値) に設定すると、AFTER トリガーは 32 レベルまで連鎖できます。 INSTEAD OF トリガーは、このオプションの設定に関係なく入れ子にできます。  
  
 **このトピックの内容**  
  
-   **作業を開始する準備:**  
  
     [Security](#Security)  
  
-   **以下を使用して nested triggers オプションを構成するには:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
-   **補足情報:** [nested triggers オプションを構成した後](#FollowUp)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> はじめに  
  
###  <a name="security"></a><a name="Security"></a> セキュリティ  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permissions  
 パラメーターなし、または最初のパラメーターのみを指定して **sp_configure** を実行する権限は、既定ですべてのユーザーに付与されます。 両方のパラメーターを指定して **sp_configure** を実行し構成オプションを変更したり RECONFIGURE ステートメントを実行したりするには、ALTER SETTINGS サーバーレベル権限がユーザーに付与されている必要があります。 ALTER SETTINGS 権限は、 **sysadmin** 固定サーバー ロールと **serveradmin** 固定サーバー ロールでは暗黙のうちに付与されています。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> SQL Server Management Studio の使用  
  
#### <a name="to-configure-the-nested-triggers-option"></a>nested triggers オプションを構成するには  
  
1.  **オブジェクト エクスプローラー**で、サーバーを右クリックし、 **[プロパティ]** を選択します。  
  
2.  **[詳細設定]** ページで、 **[トリガーから他のトリガーの起動を許可する]** オプションを **[True]** (既定) または **[False]** に設定します。  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Transact-SQL の使用  
  
#### <a name="to-configure-the-nested-triggers-option"></a>nested triggers オプションを構成するには  
  
1.  [!INCLUDE[ssDE](../../includes/ssde-md.md)]に接続します。  
  
2.  [標準] ツール バーの **[新しいクエリ]** をクリックします。  
  
3.  次の例をコピーしてクエリ ウィンドウに貼り付け、 **[実行]** をクリックします。 この例では、 [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) を使用して、 `nested triggers` オプションの値を `0`に設定する方法を示します。  
  
```wmimof  
USE AdventureWorks2012 ;  
GO  
EXEC sp_configure 'show advanced options', 1;  
GO  
RECONFIGURE ;  
GO  
EXEC sp_configure 'nested triggers', 0 ;  
GO  
RECONFIGURE ;  
GO  
  
```  
  
 詳細については、「 [サーバー構成オプション &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)」を参照してください。  
  
##  <a name="follow-up-after-you-configure-the-nested-triggers-option"></a><a name="FollowUp"></a>補足情報: nested triggers オプションを構成した後  
 新しい設定は、サーバーを再起動しなくてもすぐに有効になります。  
  
## <a name="see-also"></a>参照  
 [入れ子になったトリガーの作成](../../relational-databases/triggers/create-nested-triggers.md)   
 [RECONFIGURE &#40;Transact-SQL&#41;](../../t-sql/language-elements/reconfigure-transact-sql.md)   
 [サーバー構成オプション &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)   
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)  
  
  
