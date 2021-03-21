---
title: 'PowerShell betiği: Azure veri paylaşımında var olan paylaşımları listeleme'
description: Bu PowerShell betiği, paylaşımların ayrıntılarını listeler ve görüntüler.
services: data-share
author: joannapea
ms.author: joanpo
ms.service: data-share
ms.topic: article
ms.date: 07/07/2019
ms.openlocfilehash: 54b4b6cedc2dc09e4a9d5c4b7b937e80750e17c6
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "92220831"
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
| [Get-AzDataShare](/powershell/module/az.datashare/get-azdatashare) | Bir hesaptaki paylaşımların listesini alır. |
|||

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/).

Ek Azure veri paylaşımının PowerShell betiği örnekleri, [Azure veri paylaşımında PowerShell örnekleri](../../samples-powershell.md)bölümünde bulunabilir.
