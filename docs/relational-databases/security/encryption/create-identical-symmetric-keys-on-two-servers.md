---
title: 2 台のサーバーでの同じ対称キーの作成 | Microsoft Docs
description: Transact-SQL を使用して、SQL Server の 2 台のサーバーに同じ対称キーを作成する方法について説明します。 これにより、別のデータベースまたはサーバーでの暗号化がサポートされます。
ms.custom: ''
ms.date: 05/30/2019
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- symmetric keys [SQL Server], creating
ms.assetid: a13d0b21-a43b-43c0-9c22-7ba8f3d15e80
author: jaszymas
ms.author: jaszymas
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 6a65944d2ee5e4a3d0844ebb50cdd3d4b6dad135
ms.sourcegitcommit: da88320c474c1c9124574f90d549c50ee3387b4c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/01/2020
ms.locfileid: "85765040"
---
# <a name="create-identical-symmetric-keys-on-two-servers"></a>2 台のサーバーでの同じ対称キーの作成
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  このトピックでは、 [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)] を使用して [!INCLUDE[tsql](../../../includes/tsql-md.md)]で 2 台のサーバーに同じ対称キーを作成する方法について説明します。 暗号化テキストの暗号化を解除するには、暗号化に使用したキーが必要です。 1 つのデータベースで暗号化と暗号化解除の両方が行われる場合、データベースに格納されているキーを、権限に応じて暗号化と暗号化解除の両方に使用できます。 一方、暗号化と暗号化解除が別々のデータベースまたは別々のサーバーで行われる場合、一方のデータベースに格納されているキーを他方のデータベースで使用することはできません。
  
## <a name="before-you-begin"></a>はじめに  
  
### <a name="limitations-and-restrictions"></a>制限事項と制約事項  
  
- 対称キーを作成するときには、証明書、パスワード、対称キー、非対称キー、PROVIDER のうち少なくとも 1 つを使用して対称キーを暗号化する必要があります。 キーには種類ごとの暗号化を複数指定できます。 つまり、1 つの対称キーを、複数の証明書、パスワード、対称キー、および非対称キーを使用して同時に暗号化できます。  
  
- データベースのマスター キーの公開キーではなく、パスワードを使用して対称キーを暗号化する場合は、TRIPLE DES 暗号化アルゴリズムが使用されます。 このため、AES など、強力な暗号化アルゴリズムで作成されたキーでも、キー自身はそれより弱いアルゴリズムで保護されます。  
  
## <a name="security"></a>セキュリティ  
  
### <a name="permissions"></a>アクセス許可  
 データベースに対する ALTER ANY SYMMETRIC KEY 権限が必要です。 AUTHORIZATION を指定する場合は、データベース ユーザーに対する IMPERSONATE 権限、またはアプリケーション ロールに対する ALTER 権限が必要です。 証明書または非対称キーを使用して暗号化する場合は、証明書または非対称キーに対する VIEW DEFINITION 権限が必要です。 対称キーを所有できるのは、Windows ログイン、 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ログイン、およびアプリケーション ロールだけです。 グループとロールは対称キーを所有できません。  
  
## <a name="using-transact-sql"></a>Transact-SQL の使用  
  
### <a name="to-create-identical-symmetric-keys-on-two-different-servers"></a>2 台のサーバーに同じ対称キーを作成するには  
  
1. **オブジェクト エクスプローラー**で、 [!INCLUDE[ssDE](../../../includes/ssde-md.md)]のインスタンスに接続します。  
  
2. [標準] ツール バーの **[新しいクエリ]** をクリックします。  
  
3. 次の CREATE MASTER KEY、CREATE CERTIFICATE、および CREATE SYMMETRIC KEY ステートメントを実行して、キーを作成します。  
  
    ```sql
    CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'My p@55w0Rd';  
    GO  
    CREATE CERTIFICATE [cert_keyProtection] WITH SUBJECT = 'Key Protection';  
    GO  
    CREATE SYMMETRIC KEY [key_DataShare] WITH  
        KEY_SOURCE = 'My key generation bits. This is a shared secret!',  
        ALGORITHM = AES_256,   
        IDENTITY_VALUE = 'Key Identity generation bits. Also a shared secret'  
        ENCRYPTION BY CERTIFICATE [cert_keyProtection];  
    GO  
    ```  
  
4. 別のサーバー インスタンスに接続し、別のクエリ ウィンドウを開き、前述の SQL ステートメントを実行して、そのサーバーに同じキーを作成します。  
  
5. 最初のサーバーで次の OPEN SYMMETRIC KEY ステートメントと SELECT ステートメントを実行して、キーをテストします。  
  
    ```sql
    OPEN SYMMETRIC KEY [key_DataShare]   
        DECRYPTION BY CERTIFICATE cert_keyProtection;  
    GO  
    SELECT encryptbykey(key_guid('key_DataShare'), 'MyData' )  
    GO  
    -- For example, the output might look like this: 0x2152F8DA8A500A9EDC2FAE26D15C302DA70D25563DAE7D5D1102E3056CE9EF95CA3E7289F7F4D0523ED0376B155FE9C3  
    ```  
  
6. 他方のサーバーで、上の SELECT ステートメントの結果を次のコードの `@blob` 値として貼り付け、次のコードを実行して、このキーで暗号化テキストの暗号化を解除できることを確認します。  
  
    ```sql
    OPEN SYMMETRIC KEY [key_DataShare]   
        DECRYPTION BY CERTIFICATE cert_keyProtection;  
    GO  
    DECLARE @blob varbinary(8000);  
    SET @blob = SELECT CONVERT(varchar(8000), decryptbykey(@blob));  
    GO  
    ```  
  
7. 両方のサーバーで対称キーを終了します。  
  
    ```sql
    CLOSE SYMMETRIC KEY [key_DataShare];  
    GO  
    ```  

### <a name="encryption-changes-in-sql-server-2017-cu2"></a>SQL Server 2017 CU2 における暗号化の変更

SQL Server 2016 では、その暗号化処理に SHA1 ハッシュ アルゴリズムが使用されます。 SQL Server 2017 以降は、代わりに SHA2 が使用されます。 つまり、SQL Server 2016 で暗号化されたアイテムを SQL Server 2017 インストールで復号化するには、追加の手順が必要な場合があります。 追加の手順を次に示します。

- SQL Server 2017 が累積的な更新プログラム 2 (CU2) 以上に更新されていることを確認します。
  - 重要な詳細情報については、[SQL Server 2017 用の累積的な更新プログラム 2 (CU2)](https://support.microsoft.com/help/4052574) に関するページを参照してください。
- CU2 をインストールした後、SQL Server 2017 でトレース フラグ 4631 を有効にします: `DBCC TRACEON(4631, -1);`
  - トレース フラグ 4631 は SQL Server 2017 の新機能です。 SQL Server 2017 でマスター キー、証明書、または対称キーを作成するには、まずトレース フラグ 4631 をグローバルに `ON` にする必要があります。 これにより、作成したこれらのアイテムが、SQL Server 2016 以前のバージョンと相互運用できるようになります。

詳細なガイドについては、次を参照してください。

- [修正: SQL Server 2017 は同じ対称キーを使用して SQL Server の以前のバージョンで暗号化されたデータを解読できません。](https://support.microsoft.com/help/4053407/sql-server-2017-cannot-decrypt-data-encrypted-by-earlier-versions)
- [SQL Server 2017 と他のバージョンの SQL Server の間で、同一の対称キーが機能しない](https://feedback.azure.com/forums/908035-sql-server/suggestions/33116269-identical-symmetric-keys-do-not-work-between-sql-s) <!-- Issue 2225. Thank you Stephen W and Sam Rueby. -->

## <a name="for-more-information"></a>詳細情報

-   [CREATE MASTER KEY &#40;Transact-SQL&#41;](../../../t-sql/statements/create-master-key-transact-sql.md)  
  
-   [CREATE CERTIFICATE &#40;Transact-SQL&#41;](../../../t-sql/statements/create-certificate-transact-sql.md)  
  
-   [CREATE SYMMETRIC KEY &#40;Transact-SQL&#41;](../../../t-sql/statements/create-symmetric-key-transact-sql.md)  
  
-   [ENCRYPTBYKEY &#40;Transact-SQL&#41;](../../../t-sql/functions/encryptbykey-transact-sql.md)  
  
-   [DECRYPTBYKEY &#40;Transact-SQL&#41;](../../../t-sql/functions/decryptbykey-transact-sql.md)  
  
-   [OPEN SYMMETRIC KEY &#40;Transact-SQL&#41;](../../../t-sql/statements/open-symmetric-key-transact-sql.md)  
