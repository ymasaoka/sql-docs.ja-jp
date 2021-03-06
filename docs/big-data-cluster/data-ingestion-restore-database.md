---
title: データベースを復元する
titleSuffix: SQL Server big data clusters
description: この記事では、SQL Server 2019 ビッグ データ クラスターのマスター インスタンスにデータベースを復元する方法について説明します。
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mihaelab
ms.date: 08/21/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: a4f4f5651d14fde272de66506aca7abed51cc7de
ms.sourcegitcommit: da88320c474c1c9124574f90d549c50ee3387b4c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/01/2020
ms.locfileid: "85784319"
---
# <a name="restore-a-database-into-the-sql-server-big-data-cluster-master-instance"></a>SQL Server ビッグ データ クラスターのマスター インスタンスにデータベースを復元する

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

この記事では、[!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]のマスター インスタンスに既存のデータベースを復元する方法について説明します。 バックアップ、コピー、復元の方法を使用することをお勧めします。

## <a name="backup-your-existing-database"></a>既存のデータベースをバックアップする

最初に、Windows または Linux のいずれかの SQL Server から既存の SQL Server データベースをバックアップします。 Transact-SQL または SQL Server Management Studio (SSMS) などのツールと共に、標準的なバックアップ手法を使用します。

この記事では、AdventureWorks データベースを復元する方法について説明しますが、どのようなデータベース バックアップでも使用できます。 

> [!TIP]
> [AdventureWorks のバックアップ](../samples/adventureworks-install-configure.md)をダウンロードします。

## <a name="copy-the-backup-file"></a>バックアップ ファイルをコピーする

Kubernetes クラスターのマスター インスタンス ポッド内にある SQL Server コンテナーにバックアップ ファイルをコピーします。

```bash
kubectl cp <path to .bak file> master-0:/var/tmp/<.bak filename> -c mssql-server -n <name of your big data cluster>
```

例:

```bash
kubectl cp ~/Downloads/AdventureWorks2016CTP3.bak master-0:/var/tmp/AdventureWorks2016CTP3.bak -c mssql-server -n clustertest
```

次に、バックアップ ファイルがポッド コンテナーにコピーされたことを確認します。

```bash
kubectl exec -it master-0 -n <name of your big data cluster> -c mssql-server -- bin/bash
cd /var/
ls /tmp
exit
```

例:

```bash
kubectl exec -it master-0 -n clustertest -c mssql-server -- bin/bash
cd /var/
ls /tmp
exit
```

## <a name="restore-the-backup-file"></a>バックアップ ファイルを復元する

次に、データベース バックアップをマスター インスタンス SQL Server に復元します。  Windows で作成されたデータベース バックアップを復元する場合は、ファイルの名前を取得する必要があります。  Azure Data Studio では、マスター インスタンスを接続し、この SQL スクリプトを実行します。

```sql
RESTORE FILELISTONLY FROM DISK='/tmp/<db file name>.bak'
```

例:

```sql
RESTORE FILELISTONLY FROM DISK='/tmp/AdventureWorks2016CTP3.bak'
```

![バックアップ ファイル リスト](media/restore-database/database-restore-file-list.png)

ここで、データベースを復元します。 次のスクリプトは一例です。 必要に応じて、データベースのバックアップに合わせて名前とパスを置き換えます。

```sql
RESTORE DATABASE AdventureWorks2016CTP3
FROM DISK='/tmp/AdventureWorks2016CTP3.bak'
WITH MOVE 'AdventureWorks2016CTP3_Data' TO '/var/opt/mssql/data/AdventureWorks2016CTP3_Data.mdf',
        MOVE 'AdventureWorks2016CTP3_Log' TO '/var/opt/mssql/data/AdventureWorks2016CTP3_Log.ldf',
        MOVE 'AdventureWorks2016CTP3_mod' TO '/var/opt/mssql/data/AdventureWorks2016CTP3_mod'
```

## <a name="configure-data-pool-and-hdfs-access"></a>データ プールと HDFS アクセスを構成する

SQL Server マスター インスタンスがデータ プールと HDFS にアクセスできるよう、データ プールと記憶域プールのストアド プロシージャを実行します。 新しく復元したデータベースに対して、次の Transact-SQL スクリプトを実行します。

```sql
USE AdventureWorks2016CTP3
GO
-- Create the SqlDataPool data source:
IF NOT EXISTS(SELECT * FROM sys.external_data_sources WHERE name = 'SqlDataPool')
  CREATE EXTERNAL DATA SOURCE SqlDataPool
  WITH (LOCATION = 'sqldatapool://controller-svc/default');

-- Create the SqlStoragePool data source:
IF NOT EXISTS(SELECT * FROM sys.external_data_sources WHERE name = 'SqlStoragePool')
   CREATE EXTERNAL DATA SOURCE SqlStoragePool
   WITH (LOCATION = 'sqlhdfs://controller-svc/default');
GO
```

> [!NOTE]
> 以前のバージョンの SQL Server から復元されたデータベースに対してのみ、これらのセットアップ スクリプトを実行する必要があります。 SQL Server マスター インスタンスに新しいデータベースを作成した場合、データ プールと記憶域プールのストアド プロシージャは既に構成されています。

## <a name="next-steps"></a>次のステップ

[!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]の詳細については、次の概要を参照してください。

- [[!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]とは](big-data-cluster-overview.md)
