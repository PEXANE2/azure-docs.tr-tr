---
title: BLOB depolama-Azure depolama için dengesde kullanılabilirlik ilkelerini ayarlama ve yönetme
description: Verileri, belirli bir Aralık için silinebilir olmayan, değiştirilemeyen bir durumda depolamak üzere blob (nesne) depolama için PARAZITI (bir kez yaz, çok oku) desteğini nasıl kullanacağınızı öğrenin.
services: storage
author: tamram
ms.service: storage
ms.topic: conceptual
ms.date: 11/26/2019
ms.author: tamram
ms.subservice: blobs
ms.openlocfilehash: 539154135c35e034c889294d911fb53b3d45daa4
ms.sourcegitcommit: 318d1bafa70510ea6cdcfa1c3d698b843385c0f6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2020
ms.locfileid: "83771018"
---
# <a name="set-and-manage-immutability-policies-for-blob-storage"></a>BLOB depolama için dengesde kullanılabilirlik ilkelerini ayarlama ve yönetme

Azure Blob depolama için sabit depolama, kullanıcıların iş açısından kritik veri nesnelerini bir solucan içinde depolamasına olanak sağlar (bir kez yaz, çok oku) durumu. Bu durum, verileri silinebilir olmayan ve Kullanıcı tarafından belirtilen bir Aralık için değiştirilemez hale getirir. Saklama aralığı süresince, Bloblar oluşturulup okunabilir, ancak değiştirilemez veya silinemez. Tüm Azure bölgelerindeki genel amaçlı v2 ve BLOB depolama hesapları için sabit depolama kullanılabilir.

Bu makalede, Azure portal, PowerShell veya Azure CLı kullanarak blob depolamada bulunan veriler için imlik kullanılabilirliği ilkelerinin ve yasal tutmalar ayarlama ve yönetme işlemlerinin nasıl yapılacağı gösterilir. Sabit depolama hakkında daha fazla bilgi için bkz. [Sabit depolama ile iş açısından kritik blob verilerini depolama](storage-blob-immutable-storage.md).

## <a name="set-retention-policies-and-legal-holds"></a>Bekletme ilkelerini ve yasal tutmaları ayarlama

### <a name="portal"></a>[Portal](#tab/azure-portal)

1. Sabit durumda tutulması gereken blobların depolanması için yeni bir kapsayıcı oluşturun veya mevcut bir kapsayıcıyı seçin. Kapsayıcı, genel amaçlı bir v2 veya blob depolama hesabında olmalıdır.

2. Kapsayıcı ayarları ' nda **erişim ilkesi** ' ni seçin. Ardından, **sabit blob depolaması**altında **ilke Ekle** ' yi seçin.

    ![Portalda kapsayıcı ayarları](media/storage-blob-immutability-policies-manage/portal-image-1.png)

3. Zamana dayalı saklama süresini etkinleştirmek için, açılan menüden **zamana dayalı saklama** ' yı seçin.

    !["Ilke türü" altında "zaman tabanlı bekletme" seçildi](media/storage-blob-immutability-policies-manage/portal-image-2.png)

4. Bekletme aralığını gün cinsinden girin (kabul edilebilir değerler 1-146000 gündür).

    !["Saklama süresini güncelleştirme" kutusu](media/storage-blob-immutability-policies-manage/portal-image-5-retention-interval.png)

    İlkenin ilk durumu kilidi açmadan önce özelliği test etmeniz ve ilkede değişiklikler yapmanız sağlanır. İlkeyi kilitlemek, SEC 17A-4 gibi yönetmeliklerle uyumluluk açısından önemlidir.

5. İlkeyi kilitleyin. Üç nokta (**...**) simgesine sağ tıklayın ve aşağıdaki menü ek eylemlerle görüntülenir:

    ![Menüdeki "ilkeyi kilitle"](media/storage-blob-immutability-policies-manage/portal-image-4-lock-policy.png)

6. **Kilit ilkesini** seçin ve kilidi onaylayın. İlke artık kilitli ve silinemez, yalnızca bekletme aralığının uzantılarına izin verilir. Blob silme ve geçersiz kılmalara izin verilmez. 

    ![Menüde "kilit ilkesini" onaylayın](media/storage-blob-immutability-policies-manage/portal-image-5-lock-policy.png)

7. Yasal tutmaları etkinleştirmek için **Ilke Ekle**' yi seçin. Açılan menüden **yasal tut** ' u seçin.

    !["Ilke türü" altındaki menüdeki "Legal Hold"](media/storage-blob-immutability-policies-manage/portal-image-legal-hold-selection-7.png)

8. Bir veya daha fazla etiketle geçerli bir saklama alanı oluşturun.

    ![İlke türü altındaki "etiket adı" kutusu](media/storage-blob-immutability-policies-manage/portal-image-set-legal-hold-tags.png)

9. Yasal bir tutmayı temizlemek için uygulanan geçerli saklama tanımlayıcı etiketini kaldırın.

### <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Bu özellik aşağıdaki komut gruplarına dahildir: `az storage container immutability-policy` ve `az storage container legal-hold` . `-h`Komutları görmek için üzerinde çalıştırın.

### <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Az. Storage modülü, sabit depolamayı destekler.  Özelliği etkinleştirmek için şu adımları izleyin:

1. PowerShellGet 'in en son sürümünün yüklü olduğundan emin olun: `Install-Module PowerShellGet –Repository PSGallery –Force` .
2. Önceki Azure PowerShell yüklemesini kaldırın.
3. Azure PowerShell yüklensin: `Install-Module Az –Repository PSGallery –AllowClobber` .

Aşağıdaki örnek PowerShell betiği başvuru içindir. Bu betik yeni bir depolama hesabı ve kapsayıcısı oluşturur. Daha sonra yasal tutmaları ayarlama ve Temizleme, zaman tabanlı bir bekletme ilkesi oluşturma ve kilitleme (aynı zamanda, imlebilirlik ilkesi olarak da bilinir) ve bekletme aralığını genişletme işlemlerinin nasıl yapılacağını gösterir.

İlk olarak, bir Azure depolama hesabı oluşturun:

```powershell
$resourceGroup = "<Enter your resource group>"
$storageAccount = "<Enter your storage account name>"
$container = "<Enter your container name>"
$location = "<Enter the storage account location>"

# Log in to Azure
Connect-AzAccount
Register-AzResourceProvider -ProviderNamespace "Microsoft.Storage"

# Create your Azure resource group
New-AzResourceGroup -Name $resourceGroup -Location $location

# Create your Azure storage account
$account = New-AzStorageAccount -ResourceGroupName $resourceGroup -StorageAccountName `
    $storageAccount -SkuName Standard_ZRS -Location $location -Kind StorageV2

# Create a new container using the context
$container = New-AzStorageContainer -Name $container -Context $account.Context

# List the containers in the account
Get-AzStorageContainer -Context $account.Context

# Remove a container
Remove-AzStorageContainer -Name $container -Context $account.Context
```

Yasal tutmaları ayarla ve temizle:

```powershell
# Set a legal hold
Add-AzRmStorageContainerLegalHold -ResourceGroupName $resourceGroup `
    -StorageAccountName $storageAccount -Name $container -Tag <tag1>,<tag2>,...

# Clear a legal hold
Remove-AzRmStorageContainerLegalHold -ResourceGroupName $resourceGroup `
    -StorageAccountName $storageAccount -Name $container -Tag <tag3>
```

Zamana dayalı ve kullanılabilirlik ilkeleri oluşturun veya güncelleştirin:

```powershell
# Create a time-based immutability policy
Set-AzRmStorageContainerImmutabilityPolicy -ResourceGroupName $resourceGroup `
    -StorageAccountName $storageAccount -ContainerName $container -ImmutabilityPeriod 10
```

İmlebilirlik kuralları alma:

```powershell
# Get an immutability policy
Get-AzRmStorageContainerImmutabilityPolicy -ResourceGroupName $resourceGroup `
    -StorageAccountName $storageAccount -ContainerName $container
```

Dengesizin kullanılabilirliği ilkelerini kilitle ( `-Force` istemi kapatmak için Ekle):

```powershell
# Lock immutability policies
$policy = Get-AzRmStorageContainerImmutabilityPolicy -ResourceGroupName `
    $resourceGroup -StorageAccountName $storageAccount -ContainerName $container
Lock-AzRmStorageContainerImmutabilityPolicy -ResourceGroupName `
    $resourceGroup -StorageAccountName $storageAccount -ContainerName $container `
    -Etag $policy.Etag
```

Dengesde kullanılabilirlik ilkelerini uzat:

```powershell
# Extend immutability policies
$policy = Get-AzRmStorageContainerImmutabilityPolicy -ResourceGroupName `
    $resourceGroup -StorageAccountName $storageAccount -ContainerName $container

Set-AzRmStorageContainerImmutabilityPolicy -ImmutabilityPolicy `
    $policy -ImmutabilityPeriod 11 -ExtendPolicy
```

Kilitsiz bir imlebilirlik ilkesini kaldırma ( `-Force` istemi kapatmak için Ekle):

```powershell
# Remove an unlocked immutability policy
$policy = Get-AzRmStorageContainerImmutabilityPolicy -ResourceGroupName `
    $resourceGroup -StorageAccountName $storageAccount -ContainerName $container

Remove-AzRmStorageContainerImmutabilityPolicy -ImmutabilityPolicy $policy
```

---

## <a name="enabling-allow-protected-append-blobs-writes"></a>Korumalı ekleme bloblarına izin vermeyi etkinleştirme

### <a name="portal"></a>[Portal](#tab/azure-portal)

![Ek ekleme yazmaları sağlar](media/storage-blob-immutability-policies-manage/immutable-allow-additional-append-writes.png)

### <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Bu özellik aşağıdaki komut gruplarına dahildir: `az storage container immutability-policy` ve `az storage container legal-hold` . `-h`Komutları görmek için üzerinde çalıştırın.

### <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```powershell
# Create an immutability policy with appends allowed
Set-AzRmStorageContainerImmutabilityPolicy -ResourceGroupName $resourceGroup `
    -StorageAccountName $storageAccount -ContainerName $container -ImmutabilityPeriod 10 -AllowProtectedAppendWrite $true
```

---

## <a name="next-steps"></a>Sonraki adımlar

[Sabit depolamayla iş açısından kritik blob verilerini depolayın](storage-blob-immutable-storage.md)
