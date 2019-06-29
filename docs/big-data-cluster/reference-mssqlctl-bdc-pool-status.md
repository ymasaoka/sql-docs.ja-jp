---
title: mssqlctl bdc pool status reference
titleSuffix: SQL Server big data clusters
description: Mssqlctl bdc プール状態コマンドに関する参照記事です。
author: rothja
ms.author: jroth
manager: jroth
ms.date: 06/26/2019
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: b6eba925adeb7f18adff133ba8110c6766bfed79
ms.sourcegitcommit: ce5770d8b91c18ba5ad031e1a96a657bde4cae55
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/25/2019
ms.locfileid: "67394314"
---
# <a name="mssqlctl-bdc-pool-status"></a>mssqlctl bdc プールの状態

[!INCLUDE[tsql-appliesto-ssver15-xxxx-xxxx-xxx](../includes/tsql-appliesto-ssver15-xxxx-xxxx-xxx.md)]

次の記事への参照を提供する、 **bdc プールの状態**コマンド、 **mssqlctl**ツール。 その他の詳細については**mssqlctl**コマンドを参照してください[mssqlctl 参照](reference-mssqlctl.md)します。

## <a name="commands"></a>コマンド
|     |     |
| --- | --- |
[mssqlctl bdc プールの状態の表示](#mssqlctl-bdc-pool-status-show) | プールの状態。
## <a name="mssqlctl-bdc-pool-status-show"></a>mssqlctl bdc プールの状態の表示
プールの状態。
```bash
mssqlctl bdc pool status show --kind -k 
                              [--name -n]
```
### <a name="examples"></a>使用例
記憶域プールの状態を取得します。
```bash
mssqlctl bdc pool status show --kind storage --name default
```
データのプールの状態を取得します。
```bash
mssqlctl bdc pool status show --kind data --name default
```
コンピューティング プールの状態を取得します。
```bash
mssqlctl bdc pool status show --kind compute --name default
```
マスターのプールの状態を取得します。
```bash
mssqlctl bdc pool status show --kind master --name default
```
Spark のプールの状態を取得します。
```bash
mssqlctl bdc pool status show --kind spark --name default
```
### <a name="required-parameters"></a>必要なパラメーター
#### `--kind -k`
BDC プールの種類。
### <a name="optional-parameters"></a>省略可能なパラメーター
#### `--name -n`
BDC プールの名前。
`default`
### <a name="global-arguments"></a>グローバル引数
#### `--debug`
すべてのデバッグ ログを表示するログの詳細度を向上します。
#### `--help -h`
このヘルプ メッセージと終了を示します。
#### `--output -o`
出力形式。  使用できる値: json、jsonc、table、tsv です。  既定: json。
#### `--query -q`
JMESPath クエリ文字列。 参照してください[ http://jmespath.org/ ](http://jmespath.org/])詳細と例。
#### `--verbose`
ログ記録を上げます。 完全なデバッグ ログのデバッグ - 使用します。

## <a name="next-steps"></a>次のステップ

インストールする方法について、 **mssqlctl**ツールを参照してください[インストールの SQL Server 2019 ビッグ データ クラスターを管理する mssqlctl](deploy-install-mssqlctl.md)します。