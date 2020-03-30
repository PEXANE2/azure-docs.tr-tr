---
title: "PowerShell komut dosyası: Azure Veri Paylaşımı'ndaki varolan paylaşımları listele edin | Microsoft Dokümanlar"
description: Bu PowerShell komut dosyası, paylaşımların ayrıntılarını listeler ve görüntüler.
services: data-share
author: joannapea
ms.service: data-share
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 07/07/2019
ms.author: joanpo
ms.openlocfilehash: 6314bd348c22c901001b88eda6875181a2f69df4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "70307134"
---
# <a name="use-powershell-to-view-the-details-of-a-sent-data-share"></a>Gönderilen veri paylaşımının ayrıntılarını görüntülemek için PowerShell'i kullanma

Bu PowerShell komut dosyası, varolan bir hesaptaki veri paylaşımlarını listeler ve belirli bir paylaşımın ayrıntılarını alır.


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
| [Get-AzDataShare](/powershell/module/az.datashare/get-azdatashare?view=azps-2.6.0) | Bir hesaptaki hisselerin listesini alır ve listeler. |
|||

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](https://docs.microsoft.com/powershell/).

Ek Azure Veri Paylaşımı PowerShell komut dosyası örnekleri [Azure Veri Paylaşımı PowerShell örneklerinde](../../samples-powershell.md)bulunabilir.
