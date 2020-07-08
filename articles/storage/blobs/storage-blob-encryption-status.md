---
title: Blob 'un şifreleme durumunu denetleme-Azure depolama
description: Belirli bir Blobun şifrelenip şifrelenmediğini denetlemek için Azure portal, PowerShell veya Azure CLı 'yı nasıl kullanacağınızı öğrenin. Blob şifrelenmemişse, blobu indirip yeniden karşıya yükleyerek şifrelemeyi zorlamak için AzCopy 'i nasıl kullanacağınızı öğrenin.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 11/26/2019
ms.author: tamram
ms.reviewer: ozgun
ms.subservice: common
ms.openlocfilehash: 6d0392cd8e94ba8a9026f557b90e740fbed7f50c
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84809085"
---
# <a name="check-the-encryption-status-of-a-blob"></a>Bir Blobun şifreleme durumunu denetleme

20 Ekim 2017 ' den sonra Azure depolama 'ya yazılan her Blok Blobu, ekleme Blobu veya Sayfa Blobu, Azure depolama şifrelemesi ile şifrelenir. Bu tarihten önce oluşturulan Bloblar, bir arka plan işlemi tarafından şifrelenmeye devam eder.

Bu makalede, belirli bir Blobun şifrelenip şifrelenmediğini belirleme işlemi gösterilmektedir.

## <a name="check-a-blobs-encryption-status"></a>Blobun şifreleme durumunu denetleme

Blob 'un kod olmadan şifrelenip şifrelenmediğini anlamak için Azure portal, PowerShell veya Azure CLı kullanın.

### <a name="azure-portal"></a>[Azure portalındaki](#tab/portal)

Bir Blobun şifrelenip şifrelenmediğini denetlemek için Azure portal kullanmak için şu adımları izleyin:

1. Azure portalında depolama hesabınıza gidin.
1. Hesaptaki kapsayıcıların listesine gitmek için **kapsayıcılar** ' ı seçin.
1. Blobu bulun ve **genel bakış** sekmesini görüntüleyin.
1. Sunucu tarafından **şifrelenen** özelliği görüntüleyin. **Doğru**ise, aşağıdaki görüntüde gösterildiği gibi BLOB şifrelenir. Blobun özelliklerinin, blob 'un oluşturulduğu tarih ve saati de içerdiğine dikkat edin.

    ![Azure portal 'da sunucu şifreli özelliğinin nasıl kontrol Leneceğini gösteren ekran görüntüsü](media/storage-blob-encryption-status/blob-encryption-property-portal.png)

### <a name="powershell"></a>[PowerShell](#tab/powershell)

Blob 'un şifrelenip şifrelenmediğini denetlemek için PowerShell 'i kullanmak için blob 'un **ısserverşifrelenen** özelliğini kontrol edin. Açılı ayraçlar içindeki yer tutucu değerlerini kendi değerlerinizle değiştirmeyi unutmayın:

```powershell
$account = Get-AzStorageAccount -ResourceGroupName <resource-group> `
    -Name <storage-account>
$blob = Get-AzStorageBlob -Context $account.Context `
    -Container <container> `
    -Blob <blob>
$blob.ICloudBlob.Properties.IsServerEncrypted
```

Blobun ne zaman oluşturulduğunu öğrenmek için **oluşturulan** özelliğin değerini kontrol edin:

```powershell
$blob.ICloudBlob.Properties.IsServerEncrypted
```

### <a name="azure-cli"></a>[Azure CLI](#tab/cli)

Blob 'un şifrelenip şifrelenmediğini denetlemek için Azure CLı 'yı kullanmak üzere blob 'un **ısserverşifrelenen** özelliğini denetleyin. Açılı ayraçlar içindeki yer tutucu değerlerini kendi değerlerinizle değiştirmeyi unutmayın:

```azurecli-interactive
az storage blob show \
    --account-name <storage-account> \
    --container-name <container> \
    --name <blob> \
    --query "properties.serverEncrypted"
```

Blobun ne zaman oluşturulduğunu öğrenmek için **oluşturulan** özelliğin değerini denetleyin.

---

## <a name="force-encryption-of-a-blob"></a>Blob şifrelemeyi zorlama

20 Ekim 2017 ' den önce oluşturulan bir blob, arka plan işlemi tarafından henüz şifrelenmemişse, blobu indirip yeniden karşıya yükleyerek şifrelemeyi hemen gerçekleştirebilirsiniz. Bunu yapmanın basit bir yolu AzCopy kullanmaktır.

AzCopy ile yerel dosya sisteminize bir blob indirmek için aşağıdaki sözdizimini kullanın:

```
azcopy copy 'https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-name>/<blob-path>' '<local-file-path>'

Example:
azcopy copy 'https://storagesamples.blob.core.windows.net/sample-container/blob1.txt' 'C:\temp\blob1.txt'
```

Blob 'u AzCopy ile Azure depolama 'ya yeniden yüklemek için aşağıdaki sözdizimini kullanın:

```
azcopy copy '<local-file-path>' 'https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-name>/<blob-name>'

Example:
azcopy copy 'C:\temp\blob1.txt' 'https://storagesamples.blob.core.windows.net/sample-container/blob1.txt'
```

Blob verilerini kopyalamak için AzCopy kullanma hakkında daha fazla bilgi için bkz. [AzCopy ve BLOB Storage ile veri aktarma](../common/storage-use-azcopy-blobs.md).

## <a name="next-steps"></a>Sonraki adımlar

[Bekleyen veri için Azure Depolama şifrelemesi](../common/storage-service-encryption.md)
