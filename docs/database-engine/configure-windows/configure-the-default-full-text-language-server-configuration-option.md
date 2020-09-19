---
title: default full-text language サーバー構成オプションの構成 | Microsoft Docs
description: default full-text language オプションについて説明します。 これを構成して、SQL Server が使用するフルテキスト インデックスの既定の言語を指定する方法について説明します。
ms.custom: ''
ms.date: 03/02/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- languages [full-text search]
- default full-text language option
ms.assetid: 0fa8785b-0830-4a52-aff5-fcf8268b72fc
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 51e7b8c99fbf6c6cf7bf70a52b220ae4cd0ea3bc
ms.sourcegitcommit: da88320c474c1c9124574f90d549c50ee3387b4c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/01/2020
ms.locfileid: "85697724"
---
# <a name="configure-the-default-full-text-language-server-configuration-option"></a>default full-text language サーバー構成オプションの構成
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  このトピックでは、 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]または [!INCLUDE[tsql](../../includes/tsql-md.md)]を使用して、 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]で、 **default full-text language [!INCLUDE[tsql](../../includes/tsql-md.md)]** サーバー構成オプションを構成する方法について説明します。 **default full-text language** オプションでは、フルテキスト インデックスの既定の言語を指定します。 言語分析は、フルテキスト インデックスが付与され、データの言語に依存するすべてのデータに対して実行されます。 このオプションの既定値は、サーバーの言語です。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]のローカライズされたバージョンでは、適切な言語が存在する場合、 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]セットアップは既定の **default full-text language** オプションをサーバーの言語に設定します。 ローカライズされていないバージョンの [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]では、 **default full-text language** オプションは英語になります。  
  
 **このトピックの内容**  
  
-   **作業を開始する準備:**  
  
     [制限事項と制約事項](#Restrictions)  
  
     [Recommendations (推奨事項)](#Recommendations)  
  
     [Security](#Security)  
  
-   **以下を使用して default full-text language オプションを構成するには:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
-   **補足情報:** [default full-text language オプションを構成した後](#FollowUp)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> はじめに  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> 制限事項と制約事項  
  
-   CREATE FULLTEXT INDEX または ALTER FULLTEXT INDEX ステートメントで LANGUAGE **language_term** オプションを使用して列に言語が指定されていない場合、 **default full-text language** オプションの値がフルテキスト インデックスで使用されます。 既定のフルテキスト言語がサポートされていない場合や、言語分析パッケージがない場合は、CREATE または ALTER 操作は失敗し、[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]は指定した言語が無効であることを示すエラー メッセージを返します。  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> 推奨事項  
  
-   このオプションは詳細設定オプションであるため、熟練したデータベース管理者または認定された [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] プロフェッショナルだけが変更するようにしてください。  
  
-   **default full-text language** オプションでは、LCID 値を指定する必要があります。 サポートされている LCID とその関連言語の一覧については、 [sys.fulltext_languages &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-fulltext-languages-transact-sql.md)を参照してください。 たとえば、他の言語も独立系ソフトウェア ベンダーから入手できる場合があります。 特定の言語の方言が見つからない場合は、Full-Text Engine によって自動的にプライマリ言語に切り替えられます。  
  
###  <a name="security"></a><a name="Security"></a> セキュリティ  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permissions  
 パラメーターなし、または最初のパラメーターだけを指定して **sp_configure** を実行する権限は、既定ですべてのユーザーに付与されます。 両方のパラメーターを指定して **sp_configure** を実行し構成オプションを変更したり RECONFIGURE ステートメントを実行したりするには、ALTER SETTINGS サーバーレベル権限がユーザーに付与されている必要があります。 ALTER SETTINGS 権限は、 **sysadmin** 固定サーバー ロールと **serveradmin** 固定サーバー ロールでは暗黙のうちに付与されています。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> SQL Server Management Studio の使用  
  
#### <a name="to-configure-the-default-full-text-language-option"></a>default full-text language オプションを構成するには  
  
1.  オブジェクト エクスプローラーで、サーバーを右クリックし、 **[プロパティ]** をクリックします。  
  
2.  **[詳細設定]** ノードをクリックします。  
  
3.  [その他] の **[既定のフルテキスト言語]** を使用して、フルテキスト インデックスが作成される列の既定の言語の値を指定します。  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Transact-SQL の使用  
  
#### <a name="to-configure-the-default-full-text-language-option"></a>default full-text language オプションを構成するには  
  
1.  [!INCLUDE[ssDE](../../includes/ssde-md.md)]に接続します。  
  
2.  [標準] ツール バーの **[新しいクエリ]** をクリックします。  
  
3.  次の例をコピーしてクエリ ウィンドウに貼り付け、 **[実行]** をクリックします。 この例では、 [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) を使用して `default full-text` オプションの値をオランダ語 (`1043`) に設定する方法を示します。  
  
```sql  
USE AdventureWorks2012 ;  
GO  
EXEC sp_configure 'show advanced options', 1 ;  
GO  
RECONFIGURE  
GO  
EXEC sp_configure 'default full-text language', 1043 ;  
GO  
RECONFIGURE  
GO  
  
```  
  
 詳細については、「 [サーバー構成オプション &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)」を参照してください。  
  
##  <a name="follow-up-after-you-configure-the-default-full-text-language-option"></a><a name="FollowUp"></a>補足情報: default full-text language オプションを構成した後  
 新しい設定は、サーバーを再起動しなくてもすぐに有効になります。  
  
## <a name="see-also"></a>参照  
 [sys.fulltext_languages &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-fulltext-languages-transact-sql.md)   
 [RECONFIGURE &#40;Transact-SQL&#41;](../../t-sql/language-elements/reconfigure-transact-sql.md)   
 [サーバー構成オプション &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)   
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)   
 [CREATE FULLTEXT INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-fulltext-index-transact-sql.md)   
 [ALTER FULLTEXT INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-fulltext-index-transact-sql.md)  
  
  
