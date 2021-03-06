---
title: profiler ユーティリティ
description: profiler ユーティリティによって、SQL Server Profiler ツールが起動されます。 省略可能な引数を使用して、アプリケーションの起動方法を制御できます。
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
helpviewer_keywords:
- command prompt utilities [SQL Server], profiler90 utility
- profiler90 utility
- Profiler [SQL Server Profiler], starting
- SQL Server Profiler, starting
- starting SQL Server Profiler
ms.assetid: e91c30a9-0d29-4f84-bcb8-e8fb62afadda
author: markingmyname
ms.author: maghan
ms.openlocfilehash: a6a51b6949d3b387c1284bdd5cbaa9a64b3cfa37
ms.sourcegitcommit: c8e1553ff3fdf295e8dc6ce30d1c454d6fde8088
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/22/2020
ms.locfileid: "86917072"
---
# <a name="profiler-utility"></a>profiler ユーティリティ
[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]
  **profiler** ユーティリティにより [!INCLUDE[ssSqlProfiler](../includes/sssqlprofiler-md.md)] ツールが起動されます。 このトピックの後半で説明する省略可能な引数を使用して、アプリケーションの起動を制御できます。  
  
> [!NOTE]  
>  **profiler** ユーティリティは、トレースのスクリプティングを目的とするものではありません。 詳細については、「 [SQL Server Profiler](../tools/sql-server-profiler/sql-server-profiler.md)」を参照してください。  
  
## <a name="syntax"></a>構文  
  
```  
  
profiler  
    [ /? ] |  
[  
{  
{ /U login_id [ /P password ] }  
| /E  
}  
{[ /S sql_server_name ] | [ /A analysis_services_server_name ] }  
[ /D database ]  
[ /T "template_name" ]  
[ /B { "trace_table_name" } ]  
{ [/F "filename" ] | [ /O "filename" ] }  
[ /L locale_ID ]  
[ /M "MM-DD-YY hh:mm:ss" ]  
[ /R ]  
[ /Z file_size ]  
]  
```  
  
## <a name="arguments"></a>引数  
 **/?**  
 **profiler** の引数に関する構文の概要を表示します。  
  
 **/U** *login_id*  
 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 認証に使用するユーザー ログイン ID を指定します。 ログイン ID では大文字と小文字は区別されます。  
  
> [!NOTE]  
>  [!INCLUDE[ssNoteWinAuthentication](../includes/ssnotewinauthentication-md.md)]  
  
 **/P** *password*  
 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 認証で必要なユーザーのパスワードを指定します。  
  
 **/E**  
 現在のユーザーの資格情報に基づいて、Windows 認証による接続を行います。  
  
 **/S**  *sql_server_name*  
 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]のインスタンスを指定します。 **/U** スイッチと **/P** スイッチ、または **/E** スイッチで指定した認証情報を使用して、Profiler は指定されたサーバーに自動的に接続します。 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] の名前付きインスタンスに接続するには、 **/S** *sql_server_name*\\*instance_name* を使用します。  
  
 **/A**  *analysis_services_server_name*  
 Analysis Services のインスタンスを指定します。 **/U** スイッチと **/P** スイッチ、または **/E** スイッチで指定した認証情報を使用して、Profiler は指定されたサーバーに自動的に接続します。 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] の名前付きインスタンスに接続するには、 **/A** *analysis_services_server_name\instance_name* を使用します。  
  
 **/D** *database*  
 接続に使用するデータベースの名前を指定します。 データベースを指定しないと、指定したユーザーに対して既定のデータベースが選択されます。  
  
 **/B "** *trace_table_name* **"**  
 Profiler を起動するときに読み込むトレース テーブルを指定します。 データベース、ユーザーやスキーマ、およびテーブルを指定する必要があります。  
  
 **/T"** *template_name* **"**  
 トレースを構成するために読み込まれるテンプレートを指定します。 テンプレート名は引用符で囲む必要があります。 テンプレートは、システム テンプレート ディレクトリまたはユーザー テンプレート ディレクトリに格納されている必要があります。 同じ名前のテンプレートが両方のディレクトリにある場合は、システム ディレクトリのテンプレートが読み込まれます。 指定した名前のテンプレートが存在しない場合は、標準テンプレートが読み込まれます。 テンプレートのファイル拡張子 (.tdf) は、 *template_name*の一部として指定しないでください。 次に例を示します。  
  
```  
/T "standard"  
```  
  
 **/F"** *filename* **"**  
 Profiler を起動するときに読み込むトレース ファイルのパスとファイル名を指定します。 パスとファイル名はすべて引用符で囲む必要があります。 このオプションを **/O**と同時に使用することはできません。  
  
 **/O "** *filename*  **"**  
 トレース結果を書き込むファイルのパスとファイル名を指定します。 パスとファイル名はすべて引用符で囲む必要があります。 このオプションを **/F**と同時に使用することはできません。  
  
 **/L** *locale_ID*  
 使用できません。  
  
 **/M "** *MM-DD-YY hh:mm:ss* **"**  
 トレースが停止する日と時刻を指定します。 停止時刻は引用符で囲む必要があります。 次の表に示すパラメーターに従って停止時刻を指定します。  
  
|パラメーター|定義|  
|---------------|----------------|  
|mm|月を表す 2 桁の数字です。|  
|DD|日を表す 2 桁の数字です。|  
|YY|年を表す 2 桁の数字です。|  
|hh|時刻を表す 24 時間形式の 2 桁の数字です。|  
|mm|分を表す 2 桁の数字です。|  
|ss|秒を表す 2 桁の数字です。|  
  
> [!NOTE]  
>  "MM-DD-YY hh:mm:ss" 形式は、 **で** [日時の値の表示に地域別設定を使用する] [!INCLUDE[ssSqlProfiler](../includes/sssqlprofiler-md.md)]オプションが有効になっている場合にのみ使用できます。 このオプションが無効になっている場合は、"YYYY-MM-DD hh:mm:ss" の日付/時刻形式を使用する必要があります。  
  
 **/R**  
 トレース ファイルのロールオーバーを可能にします。  
  
 **/Z**  *file_size*  
 トレース ファイルのサイズをメガバイト (MB) 単位で指定します。 既定値は 5 MB です。 ロールオーバーが可能な場合は、すべてのロールオーバー ファイルはこの引数で指定した値に制限されます。  
  
## <a name="remarks"></a>解説  
 特定のテンプレートでトレースを開始する場合は、 **/S** オプションと **/T** オプションを同時に使用します。 たとえば、MyServer\MyInstance で Standard テンプレートを使用してトレースを開始する場合は、コマンド プロンプトで次のように入力します。  
  
```  
profiler /S MyServer\MyInstance /T "Standard"  
```  
  
## <a name="see-also"></a>参照  
 [コマンド プロンプト ユーティリティ リファレンス &#40;データベース エンジン&#41;](../tools/command-prompt-utility-reference-database-engine.md)  
  
  
