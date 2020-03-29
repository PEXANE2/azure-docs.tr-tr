---
title: 'PowerShell komut dosyası: yeni bir Azure Veri Paylaşımı oluşturma | Microsoft Dokümanlar'
description: Bu PowerShell komut dosyası, varolan bir Veri Paylaşımı hesabında yeni bir veri paylaşımı oluşturur.
services: data-share
author: joannapea
ms.service: data-share
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 07/07/2019
ms.author: joanpo
ms.openlocfilehash: 54d5dc35cf230b62cbe5f8c3cb4a1dc81eccf1e3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "70307254"
---
# <a name="use-powershell-to-create-a-data-share-in-azure"></a>Azure'da Veri Paylaşımı oluşturmak için PowerShell'i kullanın

Bu PowerShell komut dosyası, varolan bir Veri Paylaşımı hesabında yeni bir Veri Paylaşımı oluşturur.

## <a name="sample-script"></a>Örnek betik

```powershell

# Set variables with your own values
$resourceGroupName = "<Resource group name>"
$dataShareAccountName = "<Data share account name>"
$dataShareName = "<Data share name>"

# Create a new Azure Data Share
New-AzDataShare -ResourceGroupName $resourceGroupName -AccountName $dataShareAccountName -Name $dataShareName

```


## <a name="script-explanation"></a>Betik açıklaması

Bu betik şu komutları kullanır: 

| Komut | Notlar |
|---|---|
| [Yeni-AzDataShare](/powershell/module/az.datashare/new-azdatashare?view=azps-2.6.0) | Bir veri paylaşımı oluşturur. |
|||

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](https://docs.microsoft.com/powershell/).

Ek Azure Veri Paylaşımı PowerShell komut dosyası örnekleri [Azure Veri Paylaşımı PowerShell örneklerinde](../../samples-powershell.md)bulunabilir.
