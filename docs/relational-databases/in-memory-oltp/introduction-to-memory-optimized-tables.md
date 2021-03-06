---
title: メモリ最適化テーブルの概要 | Microsoft Docs
description: メモリ最適化テーブルについて説明します。メモリ最適化テーブルは持続性があり、アトミック、一貫性、分離性、持続性を備えたトランザクションをサポートします。
ms.custom: ''
ms.date: 12/02/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: ef1cc7de-63be-4fa3-a622-6d93b440e3ac
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 9677adb821528d7188a64415c344548a7ea400ed
ms.sourcegitcommit: 4d370399f6f142e25075b3714e5c2ce056b1bfd0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/09/2020
ms.locfileid: "91868676"
---
# <a name="introduction-to-memory-optimized-tables"></a>メモリ最適化テーブルの概要
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  メモリ最適化テーブルは、[CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md) を使用して作成されます。  
  
 メモリ最適化テーブルは既定では完全に持続性があり、従来のディスクベース テーブルのトランザクションと同様に、メモリ最適化テーブルのトランザクションは完全に原子性、一貫性、分離性、持続性 (ACID) を備えています。 メモリ最適化テーブルおよびネイティブ コンパイル ストアド プロシージャは、[!INCLUDE[tsql](../../includes/tsql-md.md)] 機能のサブセットのみをサポートします。
 
SQL Server 2016 以降、および Azure SQL Database では、インメモリ OLTP に固有の [照合順序またはコード ページ](../../relational-databases/collations/collation-and-unicode-support.md) に関する制限はありません。
  
 メモリ最適化テーブルのプライマリ ストレージは、メイン メモリです。 テーブルの行は、メモリから読み込まれ、メモリに書き込まれます。 ディスク上にはテーブル データの 2 番目のコピーが保持されますが、持続性を実現するためのものにすぎません。 持続性のあるテーブルの詳細については、「 [メモリ最適化オブジェクト用ストレージの作成と管理](../../relational-databases/in-memory-oltp/creating-and-managing-storage-for-memory-optimized-objects.md) 」を参照してください。 データベース復旧中は、メモリ最適化テーブルのデータはディスクからの読み取りのみ行われます (たとえば、 サーバー再起動後などの場合)。  
  
 パフォーマンスをさらに向上するために、インメモリ OLTP ではトランザクションの持続性が遅延している状態の持続性テーブルがサポートされます。 遅延持続性トランザクションは、トランザクションがコミットし、クライアントに制御を返した後すぐにディスクに保存されます。 ディスクに保存せずにコミットされたトランザクションは、パフォーマンスの向上と引き換えに、サーバー クラッシュまたはフェールオーバー時に失われます。  
  
 既定の持続性を持つメモリ最適化テーブルだけでなく、 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] は、記録やディスクへのデータ保存が行われない持続性のないメモリ最適化テーブルをサポートします。 つまり、これらのテーブルのトランザクションはディスク IO を必要としませんが、サーバーのクラッシュまたはフェールオーバーが発生した場合、データを復旧できません。  
  
 インメモリ OLTP は、開発、配置、管理、サポートなど、すべての領域においてシームレスな使用環境を提供するために、 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] と統合されています。 データベースには、ディスク ベースのオブジェクトと同様にインメモリのオブジェクトを含めることができます。  
  
 メモリ最適化テーブル内の行には、バージョンが付いています。 これは、テーブルの各行に複数のバージョンが存在する可能性があることを意味します。 行バージョンはいずれも、同じテーブル データ構造で保持されます。 行のバージョン管理は、同じ行に対して読み取りと書き込みを同時に実行できるようにするために使用します。 同じ行に対して読み取りおよび書き込みを同時に実行する方法の詳細については、「 [Transactions with Memory-Optimized Tables (メモリ最適化テーブルでのトランザクション)](../../relational-databases/in-memory-oltp/transactions-with-memory-optimized-tables.md)」を参照してください。  
  
 次の図は、複数バージョン管理について説明したものです。 この図では、行が 3 つあるテーブルを示しています。行のそれぞれに、バージョンが複数存在します。  
  
![複数バージョン管理](../../relational-databases/in-memory-oltp/media/hekaton-tables-1.gif "複数バージョン管理")  
  
 テーブルには、r1、r2、および r3 の 3 行があります。 r1 には 3 つ、r2 には 2 つ、r3 には 4 つのバージョンがあります。 同じ行に複数のバージョンがある場合にも、メモリ上の場所が連続しているとは限らないことに注意してください。 バージョンの異なる行がテーブル データ構造のさまざまな場所に分散して配置されていることもあります。  
  
 メモリ最適化テーブルのデータ構造は、行バージョンのコレクションと考えることができます。 ディスク ベース テーブルの行がページとエクステントによって整理され、個々の行がページ番号とページ オフセットでアドレス指定されているのに対して、メモリ最適化テーブルの行バージョンは、8 バイトのメモリ ポインターを使用してアドレスが指定されています。  
  
 メモリ最適化テーブルのデータには、次の 2 つの方法でアクセスできます。  
  
-   ネイティブ コンパイル ストアド プロシージャを経由してアクセスできます。  
  
-   ネイティブ コンパイル ストアド プロシージャの外で、解釈された [!INCLUDE[tsql](../../includes/tsql-md.md)]を経由してアクセスできます。 これらの [!INCLUDE[tsql](../../includes/tsql-md.md)] ステートメントは、解釈されたストアド プロシージャ内にあっても、アドホック [!INCLUDE[tsql](../../includes/tsql-md.md)] ステートメントであってもかまいません。  
  
## <a name="accessing-data-in-memory-optimized-tables"></a>メモリ最適化テーブルのデータへのアクセス  

メモリ最適化テーブルには、ネイティブ コンパイル ストアド プロシージャ (「[ネイティブ コンパイル ストアド プロシージャ](./a-guide-to-query-processing-for-memory-optimized-tables.md)」) からアクセスするのが最も効率的です。 メモリ最適化テーブルのデータには、(従来の) 解釈された [!INCLUDE[tsql](../../includes/tsql-md.md)]でもアクセスできます。 解釈された [!INCLUDE[tsql](../../includes/tsql-md.md)] とは、ネイティブ コンパイル ストアド プロシージャを使用せずにメモリ最適化テーブルにアクセスすることを表します。 解釈された [!INCLUDE[tsql](../../includes/tsql-md.md)] アクセスの例として、DML トリガー、アドホック [!INCLUDE[tsql](../../includes/tsql-md.md)] バッチ、ビュー、およびテーブル値関数からのメモリ最適化テーブルへのアクセスがあります。  
  
 次の表に、さまざまなオブジェクトへのネイティブ アクセスおよび解釈された [!INCLUDE[tsql](../../includes/tsql-md.md)] アクセスを示します。  
  
|特徴量|ネイティブ コンパイル ストアド プロシージャを使用したアクセス|解釈された [!INCLUDE[tsql](../../includes/tsql-md.md)] アクセス|CLR アクセス|  
|-------------|-------------------------------------------------------|-------------------------------------------|----------------|  
|メモリ最適化テーブル|はい|はい|いいえ*|  
|メモリ最適化テーブル型|はい|はい|いいえ|  
|ネイティブ コンパイル ストアド プロシージャ|ネイティブ コンパイル ストアド プロシージャの入れ子がサポートされるようになりました。 参照先のプロシージャがネイティブにコンパイルされている場合は、ストアド プロシージャ内で EXECUTE 構文を使用できます。|はい|いいえ*|  
  
 *コンテキスト接続 (CLR モジュールを実行するときの [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] からの接続) からメモリ最適化テーブルまたはネイティブ コンパイル ストアド プロシージャにアクセスすることはできません。 ただし、別の接続を作成して開き、そこからメモリ最適化テーブルおよびネイティブ コンパイル ストアド プロシージャにアクセスできます。  
  
## <a name="performance-and-scalability"></a>パフォーマンスとスケーラビリティ  

インメモリ OLTP を使用することで実現できるパフォーマンスの向上には、次の要素が影響します。  
  
*通信:* 短いストアド プロシージャに対する多数の呼び出しを実行するアプリケーションは、各ストアド プロシージャにより多くの機能が実装されて呼び出しが少なくて済むアプリケーションと比較した場合、パフォーマンス向上の点で劣る場合があります。  
  
*[!INCLUDE[tsql](../../includes/tsql-md.md)] 実行:* インメモリ OLTP は、解釈されたストアド プロシージャやクエリ実行ではなく、ネイティブ コンパイル ストアド プロシージャを使用する場合に、最高のパフォーマンスを発揮します。 こうしたストアド プロシージャからメモリ最適化テーブルにアクセスすると有利な場合があります。  
  
*範囲スキャンとポイント参照:* メモリ最適化された非クラスター化インデックスでは、範囲スキャンと並べ替えられたスキャンがサポートされています。 ポイント参照については、メモリ最適化された非クラスター化インデックスよりもメモリ最適化されたハッシュ インデックスの方がパフォーマンスが優れています。 メモリ最適化された非クラスター化インデックスはディスク ベース インデックスよりもパフォーマンスが優れています。

- SQL Server 2016 以降、メモリ最適化テーブルのクエリ プランでは並列でテーブルをスキャンできます。 これにより、分析クエリのパフォーマンスが向上します。
  - ハッシュ インデックスも、SQL Server 2016 で並列スキャン可能になりました。
  - 非クラスター化インデックスも、SQL Server 2016 で並列スキャン可能になりました。
  - 列ストア インデックスは、SQL Server 2014 での採用時から並列スキャン可能です。
  
*インデックス操作:* インデックス操作はログに記録されず、メモリ内にのみ存在します。  
  
*同時実行:* ラッチの競合やブロックなど、エンジンレベルのコンカレンシーがパフォーマンスに影響するアプリケーションの場合、アプリケーションをインメモリ OLTP に移行するとパフォーマンスが大幅に向上します。  
  
次の表は、リレーショナル データベースで一般的に生じるパフォーマンスとスケーラビリティの問題、およびインメモリ OLTP によってどのようにパフォーマンスが向上するかを示しています。  
  
|問題|インメモリ OLTP の影響|  
|-----------|----------------------------|  
|パフォーマンス<br /><br /> リソース (CPU、I/O、ネットワーク、またはメモリ) の使用率が高い。|CPU<br /> ネイティブ コンパイル ストアド プロシージャは、解釈されたストアド プロシージャに比較すると [!INCLUDE[tsql](../../includes/tsql-md.md)] ステートメントを実行するために必要な命令数が少ないため、CPU の使用率を大幅に下げることができます。<br /><br /> インメモリ OLTP では、1 台のサーバーで 5 ～ 10 台のサーバーのスループットに対応できる可能性があるため、スケールアウトしたワークロード環境でのハードウェア投資を削減できます。<br /><br /> I/O<br /> データやインデックス ページに対する処理に起因する I/O のボトルネックが発生する場合、インメモリ OLTP ではボトルネックを低減できます。 また、インメモリ OLTP オブジェクトのチェックポイント処理は連続的であり、I/O 操作の突然の増加を招くことはありません。 ただし、パフォーマンス クリティカルなテーブルのワーキング セットがメモリに収まらない場合、インメモリ OLTP では、データがメモリ常駐型であることが必要であるため、パフォーマンスは向上しません。 ログ記録で I/O のボトルネックが発生する場合、インメモリ OLTP はログ記録が少ないためボトルネックを軽減できます。 1 つ以上のメモリ最適化テーブルが持続性のないテーブルとして構成されている場合は、データのログ記録を無効にできます。<br /><br /> メモリ<br /> インメモリ OLTP では、パフォーマンス上の利点は得られません。 インメモリ OLTP では、オブジェクトがメモリ常駐型であることが必要になるため、メモリの負荷が増加する可能性があります。<br /><br /> ネットワーク<br /> インメモリ OLTP では、パフォーマンス上の利点は得られません。 データは、データ層からアプリケーション層に渡す必要があります。|  
|スケーラビリティ<br /><br /> [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] アプリケーションでのスケーリングの問題のほとんどは、ロック、ラッチ、スピンロックの競合など、同時実行の問題が原因です。|ラッチの競合<br /> 一般的なシナリオとして、キー順に行を同時に挿入した場合の、インデックスの最終ページでの競合を挙げることができます。 インメモリ OLTP では、データへのアクセス時にラッチを使用しないため、ラッチの競合に関連するスケーラビリティの問題は完全になくなります。<br /><br /> スピンロックの競合<br /> インメモリ OLTP では、データへのアクセス時にラッチを使用しないため、スピンロックの競合に関連するスケーラビリティの問題は完全になくなります。<br /><br /> ロックに関連する競合<br /> データベース アプリケーションで読み取り操作と書き込み操作の間にブロックの問題が発生した場合、インメモリ OLTP では、新しい形式のオプティミスティック コンカレンシーを使用してすべてのトランザクション分離レベルを実装するため、ブロックの問題は排除されます。 インメモリ OLTP では行バージョンを格納するために TempDB は使用されません。<br /><br /> 2 つの同時実行トランザクションで同じ行を更新しようとした場合など、2 つの書き込み操作間の競合によってスケーリングの問題が発生した場合、インメモリ OLTP では、一方のトランザクションが成功し、他方のトランザクションは失敗するようになります。 失敗したトランザクションは明示的または暗黙的に再送信して、トランザクションを再試行する必要があります。 いずれの場合も、アプリケーションに変更を加える必要があります。<br /><br /> アプリケーションで 2 つの書き込み操作間の競合が頻繁に発生する場合、オプティミスティック ロックの価値は減少します。 このアプリケーションはインメモリ OLTP に適していません。 ほとんどの OLTP アプリケーションでは、競合がロックのエスカレーションによって引き起こされない限り、書き込みの競合はありません。|  
  
##  <a name="row-level-security-in-memory-optimized-tables"></a><a name="rls"></a> メモリ最適化テーブルの行レベルのセキュリティ  

[行レベルのセキュリティ](../../relational-databases/security/row-level-security.md) はメモリ最適化テーブルでサポートされます。 行レベルのセキュリティ ポリシーは、基本的には、ディスク ベース テーブルと同じように、メモリ最適化テーブルに適用できますが、セキュリティ述語として使用されるインライン テーブル値関数をネイティブにコンパイル (WITH NATIVE_COMPILATION オプションを使用して作成) しなければならない場合を除きます。 詳細については、「 [行レベルのセキュリティ](../../relational-databases/security/row-level-security.md#Limitations) 」トピックの「 [Cross-Feature Compatibility (機能間の互換性)](../../relational-databases/security/row-level-security.md) 」セクションを参照してください。  
  
 行レベルのセキュリティに不可欠なさまざまな組み込みのセキュリティ関数が、インメモリ テーブルに対して有効になっています。 詳細については、「 [ネイティブ コンパイル モジュールの組み込み関数](../../relational-databases/in-memory-oltp/supported-features-for-natively-compiled-t-sql-modules.md#bfncsp)」を参照してください。  
  
 **EXECUTE AS CALLER** - ヒントが指定されていなくても、すべてのネイティブ モジュールが EXECUTE AS CALLER を既定でサポートおよび使用しています。 これは、すべての行レベルのセキュリティ述語関数が EXECUTE AS CALLER を使用するものと見なされ、その関数 (およびその内部で使用される組み込み関数) は、呼び出しユーザーのコンテキストで評価されるためです。   
EXECUTE AS CALLER には、呼び出し元のアクセス許可チェックによるパフォーマンス ヒットがそれほどありません (約 10%)。 モジュールで EXECUTE AS OWNER または EXECUTE AS SELF が明示的に指定されている場合、これらのアクセス許可のチェックと関連するパフォーマンス コストは回避されますが、 このオプションのいずれかを、上記の組み込み関数と共に使用すると、コンテキスト切り替えが必要になるため、パフォーマンス ヒットがかなり大きくなります。  
  
## <a name="scenarios"></a>シナリオ

[!INCLUDE[hek_1](../../includes/hek-1-md.md)] によってパフォーマンスを向上できる典型的なシナリオの概要については、「 [インメモリ OLTP](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md)」を参照してください。  
  
## <a name="see-also"></a>参照

[インメモリ OLTP &#40;インメモリ最適化&#41;](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md)  
  
