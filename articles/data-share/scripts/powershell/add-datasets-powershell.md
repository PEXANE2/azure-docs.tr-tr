---
title: 'PowerShell betiği: Azure veri paylaşımında blob veri kümesi ekleme | Microsoft Docs'
description: Bu PowerShell betiği var olan bir paylaşıma blob veri kümesi ekler.
services: data-share
author: joannapea
ms.service: data-share
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 07/07/2019
ms.author: joanpo
ms.openlocfilehash: ef027387d6c8e119c03fef6e4e5d7fd7a4125e51
ms.sourcegitcommit: 6794fb51b58d2a7eb6475c9456d55eb1267f8d40
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70243041"
---
# <a name="use-powershell-to-create-a-data-share-in-azure"></a>Azure 'da veri paylaşma oluşturmak için PowerShell 'i kullanma

Bu PowerShell betiği, var olan bir veri paylaşımında bir blob veri kümesi ekler.

## <a name="sample-script"></a>Örnek betik

```powershell
# Set variables with your own values
$resourceGroupName = "<Resource group name>"
$dataShareAccountName = "<Data share account name>"
$dataShareName = "<Data share name>"
$blobDatasetName = "<Dataset name>"
$blobContainer = "<Blob container name>"
$blobFilePath = "<Blob file path>"
$storageAccountResourceId = "<Storage account resource id>"

#Add a blob dataset to the datashare
New-AzDataShareDataSet -ResourceGroupName $resourceGroupName -AccountName $dataShareAccountName -ShareName $dataShareName -Name $blobDataSetName -StorageAccountResourceId $storageAccountResourceId -FilePath $blobFilePath

```


## <a name="script-explanation"></a>Betik açıklaması

Bu betik şu komutları kullanır: 

| Komut | Notlar |
|---|---|
| [New-AzDataShareDataSet](/powershell/module/az.resources/new-azdatasharedataset) | Veri paylaşımında bir veri kümesi ekler. |
|||

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](https://docs.microsoft.com/powershell/).

Ek Azure veri paylaşımının PowerShell betiği örnekleri, [Azure veri paylaşımında PowerShell örnekleri](../../samples-powershell.md)bölümünde bulunabilir.
