---
title: 'PowerShell betiği: Azure veri paylaşımının anlık görüntü Tetikleyicileri oluşturma ve görüntüleme | Microsoft Docs'
description: Bu PowerShell betiği, paylaşılan anlık görüntü Tetikleyicileri oluşturur ve alır.
services: data-share
author: joannapea
ms.service: data-share
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 07/07/2019
ms.author: joanpo
ms.openlocfilehash: f3d2439c4456c34e5c554a5399a77f19a954a9e3
ms.sourcegitcommit: 6794fb51b58d2a7eb6475c9456d55eb1267f8d40
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70243093"
---
# <a name="use-powershell-to-monitor-the-usage-of-a-sent-data-share"></a>Gönderilen veri paylaşımının kullanımını izlemek için PowerShell 'i kullanma

Bu PowerShell betiği, paylaşılan anlık görüntü Tetikleyicileri oluşturur ve alır.

## <a name="sample-script"></a>Örnek betik

```powershell
# Set variables with your own values
$resourceGroupName = "<Resource group name>"
$dataShareAccountName = "<Data share account name>"
$dataShareName = "<Data share name>"
$subscriptionName = "<Share subscription name>"
$recurrenceInterval = "<Synchronization recurrence interval>"
$synchronizationTime = "<Synchronization time>"

# Create a new snapshot trigger
New-AzDataShareTrigger -ResourceGroupName $resourceGroupName -AccountName $dataShareAccountName -ShareSubscriptionName $subscriptionName -Name $dataShareName  -RecurrenceInterval $recurrenceInterval -SynchronizationTime $synchronizationTime

#List snapshot triggers
Get-AzDataShareTrigger -ResourceGroupName $resourceGroupName -AccountName $dataShareAccountName -ShareSubscriptionName $subscriptionName  -Name $dataShareName

#Get a specific share snapshot trigger
Get-AzDataShareTrigger -ResourceGroupName $resourceGroupName -AccountName $dataShareAccountName -ShareSubscriptionName -Name $dataShareName
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik şu komutları kullanır: 

| Komut | Notlar |
|---|---|
| [New-AzDataShareTrigger](/powershell/module/az.resources/new-azdatasharetrigger) | Bir paylaşma anlık görüntü tetikleyicisi oluşturun. |
| [Get-AzDataShareTrigger](/powershell/module/az.resources/get-azdatasharetrigger) | Bir paylaşma eşitlemesinin eşitleme ayarlarını alır. |
|||

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](https://docs.microsoft.com/powershell/).

Ek Azure veri paylaşımının PowerShell betiği örnekleri, [Azure veri paylaşımında PowerShell örnekleri](../../samples-powershell.md)bölümünde bulunabilir.
