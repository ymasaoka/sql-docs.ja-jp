---
title: srv_rpcparams (拡張ストアド プロシージャ API) | Microsoft Docs
description: Srv_rpcparams と、現在のリモートストアドプロシージャのパラメーター数を返す方法について説明します。
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: stored-procedures
ms.topic: reference
apiname:
- srv_rpcparams
apilocation:
- opends60.dll
apitype: DLLExport
dev_langs:
- C++
helpviewer_keywords:
- srv_rpcparams
ms.assetid: 96a5e6f6-d320-4623-b96e-0a856e3abebb
author: rothja
ms.author: jroth
ms.openlocfilehash: 47fa4b7a539f1491d539b73fdcc50a9b298db910
ms.sourcegitcommit: 216f377451e53874718ae1645a2611cdb198808a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/28/2020
ms.locfileid: "87248261"
---
# <a name="srv_rpcparams-extended-stored-procedure-api"></a>srv_rpcparams (拡張ストアド プロシージャ API)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
    
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureDontUse](../../includes/ssnotedepfuturedontuse-md.md)]代わりに CLR Integration をご使用ください。  
  
 現在のリモート ストアド プロシージャのパラメーター数を返します。  
  
## <a name="syntax"></a>構文  
  
```  
  
int srv_rpcparams ( SRV_PROC *  
srvproc   
);  
```  
  
## <a name="arguments"></a>引数  
 *srvproc*  
 特定のクライアント接続のためのハンドル (この場合は、リモート ストアド プロシージャを受け取るハンドル) である SRV_PROC 構造体を指すポインターです。 この構造体には、アプリケーションとクライアントの間の通信やデータを管理するために、拡張ストアド プロシージャ API ライブラリで使用する情報が格納されます。  
  
## <a name="returns"></a>戻り値  
 リモート ストアド プロシージャ内のパラメーター数を返します。 リモート ストアド プロシージャにパラメーターがない場合、または現在のリモート ストアド プロシージャがない場合は、-1 を返し、情報エラーを生成します。  
  
## <a name="remarks"></a>解説  
 この関数は現在のリモート ストアド プロシージャのパラメーター数を返します。 通常、この関数はリモート ストアド プロシージャから呼び出されます。  
  
 パラメーターを指定してリモート ストアド プロシージャを呼び出す場合、パラメーターは名前で指定することも、名前を使用せずにその位置を指定して渡すこともできます。 名前によるパラメーター指定と位置によるパラメーター指定を混合してリモート ストアド プロシージャを呼び出すと、エラーが発生します。 このエラーが発生してもリモート ストアド プロシージャ ハンドラーは呼び出されますが、ハンドラーはパラメーターを受け取らず、**srv_rpcparams** は 0 を返します。  
  
> [!IMPORTANT]  
>  拡張ストアド プロシージャのソース コードを十分に確認し、コンパイル済み DLL を、運用サーバーにインストールする前にテストする必要があります。 セキュリティの確認およびテストについて詳しくは、[Microsoft の Web サイト](https://go.microsoft.com/fwlink/?LinkID=54761&amp;clcid=0x409https://msdn.microsoft.com/security/)をご覧ください。  
  
  
