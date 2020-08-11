---
title: Blob 'lar için geçici silmeyi etkinleştirme ve yönetme
titleSuffix: Azure Storage
description: Yanlışlıkla değiştirildiğinde veya silindiğinde verilerinizi daha kolay kurtarmak için Blobların geçici silinmesini etkinleştirin.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 07/15/2020
ms.author: tamram
ms.subservice: blobs
ms.custom: devx-track-azurecli
ms.openlocfilehash: 213e4092719b274909cc764d94a72886bb4923c9
ms.sourcegitcommit: 269da970ef8d6fab1e0a5c1a781e4e550ffd2c55
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/10/2020
ms.locfileid: "88057416"
---
# <a name="enable-and-manage-soft-delete-for-blobs"></a>Blob 'lar için geçici silmeyi etkinleştirme ve yönetme

Blob geçici silme, verilerinizin yanlışlıkla veya yanlışlıkla değiştirilmesini veya silinmesini önler. Bir depolama hesabı için blob geçici silme etkinleştirildiğinde, blob 'lar, blob sürümleri (Önizleme) ve bu depolama hesabındaki anlık görüntüler, belirttiğiniz bir bekletme dönemi içinde silindikten sonra kurtarılabilir.

Verilerinizin yanlışlıkla bir uygulama veya başka bir depolama hesabı kullanıcısı tarafından değiştirilmesi veya silinmesi olasılığı varsa, Microsoft, blob geçici silme özelliğini etkinleştirmeyi önerir. Bu makalede, Bloblar için geçici silmenin nasıl etkinleştirileceği gösterilmektedir. Blob geçici silme hakkında daha fazla bilgi için bkz. [Bloblar Için geçici silme](soft-delete-blob-overview.md).

Kapsayıcılar için geçici silme özelliğini etkinleştirmeyi öğrenmek için bkz. [kapsayıcılar için geçici silmeyi etkinleştirme ve yönetme](soft-delete-container-enable.md).

## <a name="enable-blob-soft-delete"></a>Blob geçici silmeyi etkinleştir

# <a name="portal"></a>[Portal](#tab/azure-portal)

Azure portal kullanarak Depolama hesabınızdaki Bloblar için geçici silme özelliğini etkinleştirin:

1. [Azure portalında](https://portal.azure.com/) depolama hesabınıza gidin.
1. **BLOB hizmeti**altında **veri koruma** seçeneğini bulun.
1. **BLOB geçici silme** özelliğini *etkin*olarak ayarlayın.
1. **Bekletme ilkeleri**altında, geçici olarak silinen Blobların Azure depolama tarafından nasıl korunacağını belirtin.
1. Yaptığınız değişiklikleri kaydedin.

![Veri koruma blobu hizmeti 'nin seçili olduğu Azure portalının ekran görüntüsü.](media/soft-delete-blob-enable/storage-blob-soft-delete-portal-configuration.png)

Geçici silinen Blobları görüntülemek için **silinen Blobları göster** onay kutusunu seçin.

![Silinen Blobları göster seçeneğinin vurgulandığı veri koruma blobu hizmeti sayfasının ekran görüntüsü.](media/soft-delete-blob-enable/storage-blob-soft-delete-portal-view-soft-deleted.png)

Belirli bir blob için geçici olarak silinen anlık görüntüleri görüntülemek için, blobu seçin ve **anlık görüntüleri görüntüle**' ye tıklayın.

![Anlık görüntüleri görüntüle seçeneği vurgulanmış şekilde veri koruma blob hizmeti sayfasının ekran görüntüsü.](media/soft-delete-blob-enable/storage-blob-soft-delete-portal-view-soft-deleted-snapshots.png)

**Silinen anlık görüntüleri göster** onay kutusunun seçili olduğundan emin olun.

![Silinen Blobları göster seçeneğinin vurgulandığı anlık görüntüleri görüntüle sayfasının ekran görüntüsü.](media/soft-delete-blob-enable/storage-blob-soft-delete-portal-view-soft-deleted-snapshots-check.png)

Geçici olarak silinen bir blob veya anlık görüntüye tıkladığınızda, yeni blob özelliklerine dikkat edin. Bu, nesnenin ne zaman silindiğini ve BLOB ya da blob anlık görüntüsünün kalıcı olarak zaman aşımına erene kadar kaç gün kaldığını gösterir. Geçici olarak silinen nesne bir anlık görüntü değilse, geri alma seçeneğiniz de olur.

![Geçici olarak silinen bir nesnenin ayrıntılarının ekran görüntüsü.](media/soft-delete-blob-enable/storage-blob-soft-delete-portal-properties.png)

Bir Blobun silinmesinin, ilişkili tüm anlık görüntüleri de silmeyi unutmayın. Etkin bir blob için geçici olarak silinen anlık görüntüleri geri almak için bloba tıklayın ve **tüm anlık görüntüleri geri al**seçeneğini belirleyin.

![Geçici olarak silinen bir Blobun ayrıntılarının ekran görüntüsü.](media/soft-delete-blob-enable/storage-blob-soft-delete-portal-undelete-all-snapshots.png)

Bir Blobun anlık görüntülerini geri aldıktan sonra, bir anlık görüntüyü kök Blobun üzerine kopyalamak için **Yükselt** ' e tıklayabilir ve böylece blobu anlık görüntüye geri yükleyebilirsiniz.

![Yükselt seçeneği vurgulanmış şekilde anlık görüntüleri görüntüle sayfasının ekran görüntüsü.](media/soft-delete-blob-enable/storage-blob-soft-delete-portal-promote-snapshot.png)

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

Yanlışlıkla silinen Blobları kurtarmak için, bu bloblarda **silmeyi geri al blobu** çağırın. **Geri alma blobu**çağırma, hem etkin hem de geçici olarak silinen bloblarda, ilişkili tüm yazılımla silinen anlık görüntüleri etkin olarak geri yükleyeceğini unutmayın. Aşağıdaki örnek, bir kapsayıcıdaki tüm geçici silinen ve etkin bloblarda **geri alma blobu** çağırır:

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

# <a name="net-v12-sdk"></a>[.NET V12 SDK](#tab/dotnet)

Geçici silme özelliğini etkinleştirmek için bir blob istemcisinin hizmet özelliklerini güncelleştirin:

:::code language="csharp" source="~/azure-storage-snippets/blobs/howto/dotnet/dotnet-v12/DataProtection.cs" id="Snippet_EnableSoftDelete":::

Yanlışlıkla silinen Blobları kurtarmak için, bu bloblarda geri alma işlemini çağırın. Hem etkin hem de geçici olarak silinen bloblarda **geri alma işlemini çağırmanın,** tüm ilişkili geçici silinen anlık görüntüleri etkin olarak geri yükleyeceğini unutmayın. Aşağıdaki örnek, bir kapsayıcıdaki tüm geçici silinen ve etkin bloblarda geri alma işlemini çağırır:

:::code language="csharp" source="~/azure-storage-snippets/blobs/howto/dotnet/dotnet-v12/DataProtection.cs" id="Snippet_RecoverDeletedBlobs":::

Belirli bir blob sürümüne kurtarmak için, ilk olarak bir Blobun çağrısını geri alın ve ardından istenen anlık görüntüyü blob üzerinden kopyalayın. Aşağıdaki örnek, Blok Blobu en son oluşturulan anlık görüntüye kurtarır:

:::code language="csharp" source="~/azure-storage-snippets/blobs/howto/dotnet/dotnet-v12/DataProtection.cs" id="Snippet_RecoverSpecificBlobVersion":::

# <a name="net-v11-sdk"></a>[.NET v11 SDK](#tab/dotnet11)

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

Yanlışlıkla silinen Blobları kurtarmak için, bu bloblarda **silmeyi geri al blobu** çağırın. **Geri alma blobu**çağırma, hem etkin hem de geçici olarak silinen bloblarda, ilişkili tüm yazılımla silinen anlık görüntüleri etkin olarak geri yükleyeceğini unutmayın. Aşağıdaki örnek, bir kapsayıcıda geçici olarak silinen ve etkin bloblarda **geri alma blobu** çağırır:

```csharp
// Recover all blobs in a container
foreach (CloudBlob blob in container.ListBlobs(useFlatBlobListing: true, blobListingDetails: BlobListingDetails.Deleted))
{
       await blob.UndeleteAsync();
}
```

Belirli bir blob sürümüne kurtarmak için önce **geri alma blobu** işlemini çağırın, sonra istediğiniz anlık görüntüyü blob üzerinden kopyalayın. Aşağıdaki örnek, Blok Blobu en son oluşturulan anlık görüntüye kurtarır:

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

## <a name="next-steps"></a>Sonraki adımlar

- [BLOB depolama için geçici silme](soft-delete-overview.md)
- [Blob sürümü oluşturma (Önizleme)](versioning-overview.md)
