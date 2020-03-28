---
title: Azure PowerShell Script Örneği - Ekine göre kapsayıcıları silme | Microsoft Dokümanlar
description: Azure Depolama blob’u kapsayıcılarını, bir kapsayıcı adı ön ekine göre silin.
services: storage
author: tamram
ms.service: storage
ms.subservice: blobs
ms.devlang: powershell
ms.topic: sample
ms.date: 06/13/2017
ms.author: tamram
ms.openlocfilehash: 73c2f221d2ac0b3ceb76e3db0fdc6379a6dd330a
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "80060873"
---
# <a name="delete-containers-based-on-container-name-prefix"></a>Kapsayıcıları, kapsayıcı adı ön ekine göre silme

Bu komut dosyası, Azure Blob depolama sındaki kapsayıcıları kapsayıcı adındaki bir önek temel alından siler.

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh-az.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek betik

[!code-powershell[main](../../../powershell_scripts/storage/delete-containers-by-prefix/delete-containers-by-prefix.ps1 "Delete containers by prefix")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme

Kaynak grubunu, kalan kapsayıcıları ve ilgili tüm kaynakları kaldırmak için aşağıdaki komutu çalıştırın.

```powershell
Remove-AzResourceGroup -Name containerdeletetestrg
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik, kapsayıcıları, kapsayıcı adı ön ekine göre silmek için aşağıdaki komutları kullanır. Tablodaki her öğe, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [Get-AzStorageAccount](/powershell/module/az.storage/get-azstorageaccount) | Belirli bir Depolama hesabını veya kaynak grubundaki veya abonelikteki Tüm Depolama hesaplarını alır. |
| [Get-AzStorageContainer](/powershell/module/az.storage/Get-AzStorageContainer) | Depolama hesabıyla ilişkili depolama kapsayıcılarını listeler. |
| [Kaldır-AzStorageContainer](/powershell/module/az.storage/Remove-AzStorageContainer) | Belirtilen depolama kapsayıcısını kaldırır. |

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell modülü hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/overview).

Ek depolama PowerShell betiği örnekleri, [Azure Blob depolama için Azure PowerShell örneklerinde](../blobs/storage-samples-blobs-powershell.md) bulunabilir.
