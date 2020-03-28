---
title: PowerShell ile depolama hesabı erişim anahtarlarını döndürme
titleSuffix: Azure Storage
description: Bir Azure Depolama hesabı oluşturun ve ardından hesap erişim anahtarlarından birini alın ve döndürün.
services: storage
author: tamram
ms.service: storage
ms.subservice: blobs
ms.devlang: powershell
ms.topic: sample
ms.date: 12/04/2019
ms.author: tamram
ms.openlocfilehash: 52ebed3de093f15d8188ee5fec49d75d5a4a206d
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "80060821"
---
# <a name="rotate-storage-account-access-keys-with-powershell"></a>PowerShell ile depolama hesabı erişim anahtarlarını döndürme

Bu komut dosyası bir Azure Depolama hesabı oluşturur, yeni depolama hesabının birincil erişim anahtarını görüntüler ve ardından anahtarı yeniler (döndürür).

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

Bu komut dosyası, depolama hesabı oluşturmak ve erişim anahtarlarından birini almak ve döndürmek için aşağıdaki komutları kullanır. Tablodaki her öğe, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [Al-Azlocation](/powershell/module/az.resources/get-azlocation) | Her konum için tüm konumları ve desteklenen kaynak sağlayıcılarını alır. |
| [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) | Bir Azure kaynak grubu oluşturur. |
| [New-AzStorageAccount](/powershell/module/az.storage/new-azstorageaccount) | Bir Depolama hesabı oluşturur. |
| [Get-AzStorageAccountKey](/powershell/module/az.storage/get-azstorageaccountkey) | Azure Depolama hesabının erişim anahtarlarını alır. |
| [Yeni-AzStorageAccountKey](/powershell/module/az.storage/new-azstorageaccountkey) | Azure Depolama hesabının erişim anahtarını yeniden oluşturur. |

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell modülü hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/overview).

Ek depolama PowerShell betiği örnekleri, [Azure Blob depolama için Azure PowerShell örneklerinde](../blobs/storage-samples-blobs-powershell.md) bulunabilir.
