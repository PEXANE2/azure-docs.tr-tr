---
title: PowerShell ile bir blob konteynerboyutunu hesaplama
titleSuffix: Azure Storage
description: Azure Blob depolama alanında bir kapsayıcının boyutunu, her bir blob'unun boyutunu toplamolarak hesaplayın.
services: storage
author: tamram
ms.service: storage
ms.subservice: blobs
ms.devlang: powershell
ms.topic: sample
ms.date: 12/04/2019
ms.author: tamram
ms.openlocfilehash: de51ed7d91ba1102f5a9cd376ab95f49dd54d9f3
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "80067066"
---
# <a name="calculate-the-size-of-a-blob-container-with-powershell"></a>PowerShell ile bir blob konteyner boyutunu hesaplama

Bu betik, Azure Blob depolama alanındaki bir kapsayıcının boyutunu, kapsayıcıdaki blob’ların boyutunu toplayarak hesaplar.

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh-az.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

> [!IMPORTANT]
> Bu PowerShell komut dosyası kapsayıcı için tahmini bir boyut sağlar ve fatura hesaplamaları için kullanılmamalıdır. Faturalandırma amacıyla kapsayıcı boyutunu hesaplayan bir komut dosyası [için](../scripts/storage-blobs-container-calculate-billing-size-powershell.md)bkz.

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
| [Get-AzStorageAccount](/powershell/module/az.storage/get-azstorageaccount) | Belirli bir Depolama hesabını veya kaynak grubundaki veya abonelikteki Tüm Depolama hesaplarını alır. |
| [Al-AzStorageBlob](/powershell/module/az.storage/Get-AzStorageBlob) | Bir kaptaki lekeleri listeler. |

## <a name="next-steps"></a>Sonraki adımlar

Faturalandırma amacıyla kapsayıcı boyutunu hesaplayan bir komut dosyası [için](../scripts/storage-blobs-container-calculate-billing-size-powershell.md)bkz.

Azure PowerShell modülü hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/overview).

Ek depolama PowerShell komut dosyası örnekleri [Azure Depolama için PowerShell örnekleri](../blobs/storage-samples-blobs-powershell.md)bulunabilir.
