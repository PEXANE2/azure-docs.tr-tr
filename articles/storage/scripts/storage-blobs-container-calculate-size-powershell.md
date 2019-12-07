---
title: PowerShell ile bir blob kapsayıcısının boyutunu hesaplama
titleSuffix: Azure Storage
description: Blob 'ların her birinin boyutunu toplayarak Azure Blob depolama alanındaki bir kapsayıcının boyutunu hesaplayın.
services: storage
author: tamram
ms.custom: mvc
ms.service: storage
ms.topic: sample
ms.date: 12/04/2019
ms.author: tamram
ms.openlocfilehash: 890da96ac1d97a2eb84f5296c74a02824d657273
ms.sourcegitcommit: 8bd85510aee664d40614655d0ff714f61e6cd328
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2019
ms.locfileid: "74895052"
---
# <a name="calculate-the-size-of-a-blob-container-with-powershell"></a>PowerShell ile bir blob kapsayıcısının boyutunu hesaplama

Bu betik, Azure Blob depolama alanındaki bir kapsayıcının boyutunu, kapsayıcıdaki blob’ların boyutunu toplayarak hesaplar.

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh-az.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

> [!IMPORTANT]
> Bu PowerShell betiği, kapsayıcı için tahmini bir boyut sağlar ve faturalandırma hesaplamaları için kullanılmamalıdır. Faturalama amacıyla kapsayıcı boyutunu hesaplayan bir betik için, bkz. [Fatura amaçları için bir BLOB depolama kapsayıcısının boyutunu hesaplama](../scripts/storage-blobs-container-calculate-billing-size-powershell.md). 

## <a name="sample-script"></a>Örnek betik

[!code-powershell[main](../../../powershell_scripts/storage/calculate-container-size/calculate-container-size.ps1 "Calculate container size")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme 

Kaynak grubunu, kapsayıcıyı ve ilgili tüm kaynakları kaldırmak için aşağıdaki komutu çalıştırın.

```powershell
Remove-AzResourceGroup -Name bloblisttestrg
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik, Blob depolama kapsayıcısının boyutunu hesaplamak için aşağıdaki komutları kullanır. Tablodaki her öğe, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [Get-AzStorageAccount](/powershell/module/az.storage/get-azstorageaccount) | Bir kaynak grubunda veya abonelikte belirtilen depolama hesabını veya tüm depolama hesaplarını alır. |
| [Get-AzStorageBlob](/powershell/module/az.storage/Get-AzStorageBlob) | Bir kapsayıcıdaki Blobları listeler. |

## <a name="next-steps"></a>Sonraki adımlar

Faturalama amacıyla kapsayıcı boyutunu hesaplayan bir betik için, bkz. [Fatura amaçları için bir BLOB depolama kapsayıcısının boyutunu hesaplama](../scripts/storage-blobs-container-calculate-billing-size-powershell.md).

Azure PowerShell modülü hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/overview).

Ek depolama PowerShell betiği örnekleri, [Azure depolama Için PowerShell örneklerinde](../blobs/storage-samples-blobs-powershell.md)bulunabilir.
