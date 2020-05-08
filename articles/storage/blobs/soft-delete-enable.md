---
title: Bloblar için geçici silmeyi etkinleştir
titleSuffix: Azure Storage
description: Yanlışlıkla değiştirildiğinde veya silindiğinde verilerinizi daha kolay kurtarmak için blob nesneleri için geçici silmeyi etkinleştirin.
services: storage
author: tamram
ms.service: storage
ms.topic: conceptual
ms.date: 04/02/2020
ms.author: tamram
ms.subservice: blobs
ms.openlocfilehash: 26a16d0eeb81c12faede1c00bdf5a0d724f7a6c6
ms.sourcegitcommit: d815163a1359f0df6ebfbfe985566d4951e38135
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2020
ms.locfileid: "82884689"
---
# <a name="enable-soft-delete-for-blobs"></a>Bloblar için geçici silmeyi etkinleştir

Aşağıdaki adımlarda, geçici silme ile çalışmaya başlama gösterilmektedir.

# <a name="portal"></a>[Portal](#tab/azure-portal)

Azure portal kullanarak Depolama hesabınızdaki Bloblar için geçici silme özelliğini etkinleştirin:

1. [Azure Portal](https://portal.azure.com/)depolama hesabınızı seçin. 

2. **BLOB hizmeti**altındaki **veri koruma** seçeneğine gidin.

3. **BLOB geçici silme** altında **etkin** ' e tıklayın

4. **Bekletme ilkeleri** altında *saklamak* istediğiniz gün sayısını girin

5. Veri koruma ayarlarınızı onaylamak için **Kaydet** düğmesini seçin

![](media/soft-delete-enable/storage-blob-soft-delete-portal-configuration.png)

Geçici silinen Blobları görüntülemek için **silinen Blobları göster** onay kutusunu seçin.

![](media/soft-delete-enable/storage-blob-soft-delete-portal-view-soft-deleted.png)

Belirli bir blob için geçici olarak silinen anlık görüntüleri görüntülemek için, blobu seçin ve **anlık görüntüleri görüntüle**' ye tıklayın.

![](media/soft-delete-enable/storage-blob-soft-delete-portal-view-soft-deleted-snapshots.png)

**Silinen anlık görüntüleri göster** onay kutusunun seçili olduğundan emin olun.

![](media/soft-delete-enable/storage-blob-soft-delete-portal-view-soft-deleted-snapshots-check.png)

Geçici olarak silinen bir blob veya anlık görüntüye tıkladığınızda, yeni blob özelliklerine dikkat edin. Bu, nesnenin ne zaman silindiğini ve BLOB ya da blob anlık görüntüsünün kalıcı olarak zaman aşımına erene kadar kaç gün kaldığını gösterir. Geçici olarak silinen nesne bir anlık görüntü değilse, geri alma seçeneğiniz de olur.

![](media/soft-delete-enable/storage-blob-soft-delete-portal-properties.png)

Bir Blobun silinmesinin, ilişkili tüm anlık görüntüleri de silmeyi unutmayın. Etkin bir blob için geçici olarak silinen anlık görüntüleri geri almak için bloba tıklayın ve **tüm anlık görüntüleri geri al**seçeneğini belirleyin.

![](media/soft-delete-enable/storage-blob-soft-delete-portal-undelete-all-snapshots.png)

Bir Blobun anlık görüntülerini geri aldıktan sonra, bir anlık görüntüyü kök Blobun üzerine kopyalamak için **Yükselt** ' e tıklayabilir ve böylece blobu anlık görüntüye geri yükleyebilirsiniz.

![](media/soft-delete-enable/storage-blob-soft-delete-portal-promote-snapshot.png)

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Geçici silme özelliğini etkinleştirmek için bir blob istemcisinin hizmet özelliklerini güncelleştirin. Aşağıdaki örnek, bir abonelikteki hesapların bir alt kümesi için geçici silme imkanı sunar:

```powershell
Set-AzContext -Subscription "<subscription-name>"
$MatchingAccounts = Get-AzStorageAccount | where-object{$_.StorageAccountName -match "<matching-regex>"}
$MatchingAccounts | Enable-AzStorageDeleteRetentionPolicy -RetentionDays 7
```
Aşağıdaki komutu kullanarak geçici silmenin açık olduğunu doğrulayabilirsiniz:

```powershell
$MatchingAccounts | $account = Get-AzStorageAccount -ResourceGroupName myresourcegroup -Name storageaccount
   Get-AzStorageServiceProperty -ServiceType Blob -Context $account.Context | Select-Object -ExpandProperty DeleteRetentionPolicy
```

Yanlışlıkla silinen Blobları kurtarmak için, bu bloblarda geri alma işlemini çağırın. **Geri alma blobu**çağırma, hem etkin hem de geçici olarak silinen bloblarda, ilişkili tüm yazılımla silinen anlık görüntüleri etkin olarak geri yükleyeceğini unutmayın. Aşağıdaki örnek, bir kapsayıcıdaki tüm geçici silinen ve etkin bloblarda geri alma işlemini çağırır:

```powershell
# Create a context by specifying storage account name and key
$ctx = New-AzStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

# Get the blobs in a given container and show their properties
$Blobs = Get-AzStorageBlob -Container $StorageContainerName -Context $ctx -IncludeDeleted
$Blobs.ICloudBlob.Properties

# Undelete the blobs
$Blobs.ICloudBlob.Undelete()
```
Geçerli geçici silme bekletme ilkesini bulmak için aşağıdaki komutu kullanın:

```azurepowershell-interactive
   $account = Get-AzStorageAccount -ResourceGroupName myresourcegroup -Name storageaccount
   Get-AzStorageServiceProperty -ServiceType Blob -Context $account.Context
```

# <a name="cli"></a>[CLI](#tab/azure-CLI)

Geçici silme özelliğini etkinleştirmek için bir blob istemcisinin hizmet özelliklerini güncelleştirin:

```azurecli-interactive
az storage blob service-properties delete-policy update --days-retained 7  --account-name mystorageaccount --enable true
```

Geçici silme özelliğinin açık olduğunu doğrulamak için aşağıdaki komutu kullanın: 

```azurecli-interactive
az storage blob service-properties delete-policy show --account-name mystorageaccount 
```

# <a name="python"></a>[Python](#tab/python)

Geçici silme özelliğini etkinleştirmek için bir blob istemcisinin hizmet özelliklerini güncelleştirin:

```python
# Make the requisite imports
from azure.storage.blob import BlockBlobService
from azure.storage.common.models import DeleteRetentionPolicy

# Initialize a block blob service
block_blob_service = BlockBlobService(
    account_name='<enter your storage account name>', account_key='<enter your storage account key>')

# Set the blob client's service property settings to enable soft delete
block_blob_service.set_blob_service_properties(
    delete_retention_policy=DeleteRetentionPolicy(enabled=True, days=7))
```

# <a name="net"></a>[.NET](#tab/net)

Geçici silme özelliğini etkinleştirmek için bir blob istemcisinin hizmet özelliklerini güncelleştirin:

```csharp
// Get the blob client's service property settings
ServiceProperties serviceProperties = blobClient.GetServiceProperties();

// Configure soft delete
serviceProperties.DeleteRetentionPolicy.Enabled = true;
serviceProperties.DeleteRetentionPolicy.RetentionDays = RetentionDays;

// Set the blob client's service property settings
blobClient.SetServiceProperties(serviceProperties);
```

Yanlışlıkla silinen Blobları kurtarmak için, bu bloblarda geri alma işlemini çağırın. **Geri alma blobu**çağırma, hem etkin hem de geçici olarak silinen bloblarda, ilişkili tüm yazılımla silinen anlık görüntüleri etkin olarak geri yükleyeceğini unutmayın. Aşağıdaki örnek, bir kapsayıcıdaki tüm geçici silinen ve etkin bloblarda geri alma işlemini çağırır:

```csharp
// Recover all blobs in a container
foreach (CloudBlob blob in container.ListBlobs(useFlatBlobListing: true, blobListingDetails: BlobListingDetails.Deleted))
{
       await blob.UndeleteAsync();
}
```

Belirli bir blob sürümüne kurtarmak için, ilk olarak bir Blobun çağrısını geri alın ve ardından istenen anlık görüntüyü blob üzerinden kopyalayın. Aşağıdaki örnek, Blok Blobu en son oluşturulan anlık görüntüye kurtarır:

```csharp
// Undelete
await blockBlob.UndeleteAsync();

// List all blobs and snapshots in the container prefixed by the blob name
IEnumerable<IListBlobItem> allBlobVersions = container.ListBlobs(
    prefix: blockBlob.Name, useFlatBlobListing: true, blobListingDetails: BlobListingDetails.Snapshots);

// Restore the most recently generated snapshot to the active blob    
CloudBlockBlob copySource = allBlobVersions.First(version => ((CloudBlockBlob)version).IsSnapshot &&
    ((CloudBlockBlob)version).Name == blockBlob.Name) as CloudBlockBlob;
blockBlob.StartCopy(copySource);
```

---

