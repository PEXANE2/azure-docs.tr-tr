---
title: 'PowerShell betiği: Azure veri paylaşımında var olan paylaşımları listeleme | Microsoft Docs'
description: Bu PowerShell betiği, paylaşımların ayrıntılarını listeler ve görüntüler.
services: data-share
author: joannapea
ms.service: data-share
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 07/07/2019
ms.author: joanpo
ms.openlocfilehash: 53b44441397be6aaa690c75bf362fd16d5a34019
ms.sourcegitcommit: 6794fb51b58d2a7eb6475c9456d55eb1267f8d40
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70242898"
---
# <a name="use-powershell-to-view-the-details-of-a-sent-data-share"></a>Gönderilen veri paylaşımının ayrıntılarını görüntülemek için PowerShell 'i kullanma

Bu PowerShell betiği, mevcut bir hesaptan veri paylaşımlarını listeler ve belirli bir paylaşımın ayrıntılarını alır.


## <a name="sample-script"></a>Örnek betik

```powershell

# Set variables with your own values
$resourceGroupName = "<Resource group name>"
$dataShareAccountName = "<Data share account name>"
$dataShareName = "<Data share name>"

#Lists all data shares within an account
Get-AzDataShare -ResourceGroupName $resourceGroupName -AccountName $dataShareAccountName

#Gets details of a specific data share
Get-AzDataShare -ResourceGroupName $resourceGroupName -AccountName $dataShareAccountName -Name $dataShareName

```


## <a name="script-explanation"></a>Betik açıklaması

Bu betik şu komutları kullanır: 

| Komut | Notlar |
|---|---|
| [Get-AzDataShare](/powershell/module/az.resources/get-az) | Bir hesaptaki paylaşımların listesini alır. |
|||

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](https://docs.microsoft.com/powershell/).

Ek Azure veri paylaşımının PowerShell betiği örnekleri, [Azure veri paylaşımında PowerShell örnekleri](../../samples-powershell.md)bölümünde bulunabilir.
