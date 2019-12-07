---
title: Depolama hesabı erişim anahtarlarını PowerShell ile döndürme
titleSuffix: Azure Storage
description: Bir Azure depolama hesabı oluşturun, ardından hesap erişim anahtarlarından birini alın ve döndürün.
services: storage
author: tamram
ms.custom: mvc
ms.service: storage
ms.topic: sample
ms.date: 12/04/2019
ms.author: tamram
ms.openlocfilehash: 54a7a6cb89c88726853a39ab5b2a61e1f0cde0b5
ms.sourcegitcommit: 8bd85510aee664d40614655d0ff714f61e6cd328
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2019
ms.locfileid: "74894937"
---
# <a name="rotate-storage-account-access-keys-with-powershell"></a>Depolama hesabı erişim anahtarlarını PowerShell ile döndürme

Bu betik bir Azure depolama hesabı oluşturur, yeni depolama hesabının birincil erişim anahtarını görüntüler, ardından anahtarı yeniler (döndürür).

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh-az.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek betik

[!code-powershell[main](../../../powershell_scripts/storage/rotate-storage-account-keys/rotate-storage-account-keys.ps1 "Rotate storage account keys")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme

Kaynak grubunu, depolama hesabını ve ilgili tüm kaynakları kaldırmak için aşağıdaki komutu çalıştırın.

```powershell
Remove-AzResourceGroup -Name rotatekeystestrg
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik, depolama hesabı oluşturmak için aşağıdaki komutları kullanır ve erişim anahtarlarından birini alır ve döndürür. Tablodaki her öğe, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [Get-AzLocation](/powershell/module/az.resources/get-azlocation) | Her konum için tüm konumları ve desteklenen kaynak sağlayıcılarını alır. |
| [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) | Bir Azure Kaynak grubu oluşturur. |
| [New-AzStorageAccount](/powershell/module/az.storage/new-azstorageaccount) | Bir Depolama hesabı oluşturur. |
| [Get-AzStorageAccountKey](/powershell/module/az.storage/get-azstorageaccountkey) | Azure Depolama hesabının erişim anahtarlarını alır. |
| [New-AzStorageAccountKey](/powershell/module/az.storage/new-azstorageaccountkey) | Bir Azure depolama hesabı için erişim anahtarını yeniden oluşturur. |

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell modülü hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/overview).

Ek depolama PowerShell betiği örnekleri, [Azure Blob depolama için Azure PowerShell örneklerinde](../blobs/storage-samples-blobs-powershell.md) bulunabilir.
