---
title: 'PowerShell betiği: yeni Azure veri paylaşma hesabı oluştur'
description: Bu PowerShell betiği yeni bir veri paylaşma hesabı oluşturur.
author: joannapea
ms.service: data-share
ms.topic: article
ms.date: 07/07/2019
ms.author: joanpo
ms.openlocfilehash: 4608afc16398a4cd1fa47ee1ae2cd4a4489a3f96
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "92221375"
---
# <a name="use-powershell-to-create-a-data-share-account-in-azure"></a>Azure 'da bir veri paylaşma hesabı oluşturmak için PowerShell 'i kullanma

Bu PowerShell betiği yeni bir veri paylaşma hesabı oluşturur. 

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
| [New-AzDataShareAccount](/powershell/module/az.datashare/new-azdatashareaccount) | Bir veri paylaşma hesabı oluşturur. |
|||

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/).

Ek Azure veri paylaşımının PowerShell betiği örnekleri, [Azure veri paylaşımında PowerShell örnekleri](../../samples-powershell.md)bölümünde bulunabilir.
