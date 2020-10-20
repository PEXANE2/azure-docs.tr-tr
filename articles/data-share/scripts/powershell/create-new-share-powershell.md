---
title: 'PowerShell betiği: yeni bir Azure veri paylaşma oluşturma'
description: Bu PowerShell betiği, var olan bir veri paylaşma hesabı içinde yeni bir veri paylaşma oluşturur.
author: joannapea
ms.service: data-share
ms.topic: article
ms.date: 07/07/2019
ms.author: joanpo
ms.openlocfilehash: 36c1bdfef2afe4c34796a804784317a5e7fe12ff
ms.sourcegitcommit: 8d8deb9a406165de5050522681b782fb2917762d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/20/2020
ms.locfileid: "92221358"
---
# <a name="use-powershell-to-create-a-data-share-in-azure"></a>Azure 'da veri paylaşma oluşturmak için PowerShell 'i kullanma

Bu PowerShell betiği, var olan bir veri paylaşma hesabı içinde yeni bir veri paylaşma oluşturur.

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
| [New-AzDataShare](/powershell/module/az.datashare/new-azdatashare) | Bir veri paylaşma oluşturur. |
|||

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/).

Ek Azure veri paylaşımının PowerShell betiği örnekleri, [Azure veri paylaşımında PowerShell örnekleri](../../samples-powershell.md)bölümünde bulunabilir.
