---
title: "PowerShell komut dosyası: Azure Veri Paylaşımı'nın kullanımını izleyin | Microsoft Dokümanlar"
description: Bu PowerShell komut dosyası, gönderilen bir veri paylaşımının kullanım ölçümlerini alır.
services: data-share
author: joannapea
ms.service: data-share
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 07/07/2019
ms.author: joanpo
ms.openlocfilehash: 0a4084d309dd0160970f1c03540705b310eb8e75
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "70307200"
---
# <a name="use-powershell-to-monitor-the-usage-of-a-sent-data-share"></a>Gönderilen veri paylaşımının kullanımını izlemek için PowerShell'i kullanın

Bu PowerShell komut dosyası, gönderilen bir veri paylaşımının eşitlemelerini listeleyerek ve belirli bir eşitlemenin ayrıntılarını alarak veri kullanımını izler.

## <a name="sample-script"></a>Örnek betik


```powershell
# Set variables with your own values
$resourceGroupName = "<Resource group name>"
$dataShareAccountName = "<Data share account name>"
$dataShareName = "<Data share name>"
$synchronizationId = "<Azure synchronization id>"

# List synchronizations on a share
Get-AzDataShareSynchronization -ResourceGroupName $resourceGroupName -AccountName $dataShareAccountName -ShareName $dataShareName

#Get details of a specific synchronization
Get-AzDataShareSynchronizationDetails -ResourceGroupName $resourceGroupName -AccountName $dataShareAccountName -ShareName $dataShareName -SynchronizationId $synchronizationId
```


## <a name="script-explanation"></a>Betik açıklaması

Bu betik şu komutları kullanır: 

| Komut | Notlar |
|---|---|
| [Get-AzDataShareSynchronization](/powershell/module/az.datashare/get-azdatasharesynchronization?view=azps-2.6.0) | Bir hissedeki eşitlemeleri listele. |
| [Get-AzDataShareSynchronizationAyrıntılar](/powershell/module/az.datashare/get-azdatasharesynchronizationdetail?view=azps-2.6.0) | Hisse eşitlemesi eşitleme ayrıntılarını alır. |
|||

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](https://docs.microsoft.com/powershell/).

Ek Azure Veri Paylaşımı PowerShell komut dosyası örnekleri [Azure Veri Paylaşımı PowerShell örneklerinde](../../samples-powershell.md)bulunabilir.
