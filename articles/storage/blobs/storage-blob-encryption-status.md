---
title: Bir lekenin şifreleme durumunu kontrol edin - Azure Depolama
description: Belirli bir lekenin şifrelenip şifrelenmediğini kontrol etmek için Azure portalı, PowerShell veya Azure CLI'yi nasıl kullanacağınızı öğrenin. Bir blob şifrelenmemişse, blob'u indirip yeniden yükleyerek şifrelemeyi zorlamak için AzCopy'yi nasıl kullanacağınızı öğrenin.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 11/26/2019
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: common
ms.openlocfilehash: 5cef0e94a43b3ef16d45f7f43658f962e07b5345
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74707589"
---
# <a name="check-the-encryption-status-of-a-blob"></a>Bir lekenin şifreleme durumunu denetleme

20 Ekim 2017'den sonra Azure Depolama'ya yazılan her blok blob, ek blob veya sayfa blob'u Azure Depolama şifrelemesi ile şifrelenir. Bu tarihten önce oluşturulan blobs bir arka plan işlemi tarafından şifrelenmiş olmaya devam ediyor.

Bu makalede, belirli bir blob şifrelenmiş olup olmadığını belirlemek için nasıl gösterilmektedir.

## <a name="check-a-blobs-encryption-status"></a>Blob'un şifreleme durumunu denetleme

Bir blob'un kodsuz şifrelenip şifrelenmediğini belirlemek için Azure portalını, PowerShell'i veya Azure CLI'yi kullanın.

### <a name="azure-portal"></a>[Azure portalında](#tab/portal)

Bir blob'un şifrelenip şifrelenmediğini kontrol etmek için Azure portalını kullanmak için aşağıdaki adımları izleyin:

1. Azure portalında depolama hesabınıza gidin.
1. Hesaptaki kapsayıcılar listesine gitmek için **Kapsayıcılar'ı** seçin.
1. Blob'u bulun ve **Genel Bakış** sekmesini görüntüleyin.
1. Sunucu **Şifreli** özelliğini görüntüleyin. **True**ise , aşağıdaki resimde gösterildiği gibi, o zaman blob şifrelenir. Blob'un özelliklerinin, blob'un oluşturulduğu tarih ve saati de içerdiğine dikkat edin.

    ![Azure portalında Sunucu Şifreli özelliğinin nasıl denetlenir olduğunu gösteren ekran görüntüsü](media/storage-blob-encryption-status/blob-encryption-property-portal.png)

### <a name="powershell"></a>[Powershell](#tab/powershell)

Bir blob şifrelenmiş olup olmadığını kontrol etmek için PowerShell kullanmak için, blob **IsServerEncrypted** özelliğini kontrol edin. Açı parantezindeki yer tutucu değerlerini kendi değerlerinizle değiştirmeyi unutmayın:

```powershell
$account = Get-AzStorageAccount -ResourceGroupName <resource-group> `
    -Name <storage-account>
$blob = Get-AzStorageBlob -Context $account.Context `
    -Container <container> `
    -Blob <blob>
$blob.ICloudBlob.Properties.IsServerEncrypted
```

Blob'un ne zaman oluşturulduğunu belirlemek **için, Oluşturulan** özelliğin değerini denetleyin:

```powershell
$blob.ICloudBlob.Properties.IsServerEncrypted
```

### <a name="azure-cli"></a>[Azure CLI](#tab/cli)

Bir blob'un şifrelenip şifrelenmediğini kontrol etmek için Azure CLI'yi kullanmak için blob'un **IsServerEncrypted** özelliğini denetleyin. Açı parantezindeki yer tutucu değerlerini kendi değerlerinizle değiştirmeyi unutmayın:

```azurecli-interactive
az storage blob show \
    --account-name <storage-account> \
    --container-name <container> \
    --name <blob> \
    --query "properties.serverEncrypted"
```

Blob'un ne zaman oluşturulduğunu belirlemek **için, oluşturulan** özelliğin değerini denetleyin.

---

## <a name="force-encryption-of-a-blob"></a>Bir lekenin kuvvet şifrelemesi

20 Ekim 2017 tarihinden önce oluşturulmuş bir leke henüz arka plan işlemi tarafından şifrelenmemişse, blob'u indirip yeniden yükleyerek şifrelemeyi hemen oluşmaya zorlayabilirsiniz. Bunu yapmanın basit bir yolu AzCopy ile.

AzCopy ile yerel dosya sisteminize bir blob indirmek için aşağıdaki sözdizimini kullanın:

```
azcopy copy 'https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-name>/<blob-path>' '<local-file-path>'

Example:
azcopy copy 'https://storagesamples.blob.core.windows.net/sample-container/blob1.txt' 'C:\temp\blob1.txt'
```

AzCopy ile blob'u Azure Depolama alanına yeniden yüklemek için aşağıdaki sözdizimini kullanın:

```
azcopy copy '<local-file-path>' 'https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-name>/<blob-name>'

Example:
azcopy copy 'C:\temp\blob1.txt' 'https://storagesamples.blob.core.windows.net/sample-container/blob1.txt'
```

Blob verilerini kopyalamak için AzCopy kullanma hakkında daha fazla bilgi için [Bkz. AzCopy ve Blob depolama ile veri aktarımı.](../common/storage-use-azcopy-blobs.md)

## <a name="next-steps"></a>Sonraki adımlar

[Veriler için Azure Depolama şifrelemesi](../common/storage-service-encryption.md)
