---
title: 'PowerShell komut dosyası: Yeni Azure Veri Paylaşımı hesabı oluşturma | Microsoft Dokümanlar'
description: Bu PowerShell komut dosyası yeni bir Veri Paylaşımı hesabı oluşturur.
services: data-share
author: joannapea
ms.service: data-share
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 07/07/2019
ms.author: joanpo
ms.openlocfilehash: c3852dd5f1d3d3df8a982716ce5dab9426782869
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "70307263"
---
# <a name="use-powershell-to-create-a-data-share-account-in-azure"></a>Azure'da veri paylaşımı hesabı oluşturmak için PowerShell'i kullanma

Bu PowerShell komut dosyası yeni bir Veri Paylaşımı hesabı oluşturur. 

## <a name="sample-script"></a>Örnek betik

```powershell
# Set variables with your own values
$resourceGroupName = "<Resource group name"
$location = "<Location>"
$dataShareAccountName = "<Data share account name>"

# Create a new Azure Data Share account
New-AzDataShareAccount -ResourceGroupName $resourceGroupName -Name $dataShareAccountName -Location $location
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik şu komutları kullanır: 

| Komut | Notlar |
|---|---|
| [Yeni-AzDataShareAccount](/powershell/module/az.datashare/new-azdatashareaccount?view=azps-2.6.0) | Bir veri paylaşım hesabı oluşturur. |
|||

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](https://docs.microsoft.com/powershell/).

Ek Azure Veri Paylaşımı PowerShell komut dosyası örnekleri [Azure Veri Paylaşımı PowerShell örneklerinde](../../samples-powershell.md)bulunabilir.
