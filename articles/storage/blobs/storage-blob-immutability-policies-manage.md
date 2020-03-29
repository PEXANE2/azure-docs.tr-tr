---
title: Blob depolama için uygun olmayan ilkeler belirleyin ve yönetin - Azure Depolama
description: Belirli bir aralık için verileri eritilemez, değiştirilemez bir durumda depolamak için Blob (nesne) depolamaiçin WORM (Write Once, Read Many) desteğini nasıl kullanacağınızı öğrenin.
services: storage
author: tamram
ms.service: storage
ms.topic: conceptual
ms.date: 11/26/2019
ms.author: tamram
ms.subservice: blobs
ms.openlocfilehash: 05a155584f0cb69191883cb82b3db0af435ccc12
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78970109"
---
# <a name="set-and-manage-immutability-policies-for-blob-storage"></a>Blob depolama için uygun olmayan ilkeler belirleyin ve yönetin

Azure Blob depolama için değişmez depolama, kullanıcıların işle ilgili kritik veri nesnelerini WORM (Bir Kez Yaz, Çok Oku) durumunda depolamasına olanak tanır. Bu durum, verileri kullanıcı tarafından belirtilen bir aralık için eritilemez ve değiştirilemez hale getirir. Bekletme aralığı süresince, lekeler oluşturulabilir ve okunabilir, ancak değiştirilemez veya silinemez. Değişmez depolama, tüm Azure bölgelerindeki genel amaçlı v2 ve Blob depolama hesapları için kullanılabilir.

Bu makalede, Azure portalı, PowerShell veya Azure CLI kullanarak Blob depolamadaki veriler için uygun olmayan ilkeler ve yasal tutarlar nasıl ayarlanır ve yönetilir. Değişmez depolama hakkında daha fazla bilgi için, [değişmez depolama ile mağaza iş açısından kritik blob verileri](storage-blob-immutable-storage.md)bakın.

## <a name="set-retention-policies-and-legal-holds"></a>Bekletme politikalarını ve yasal dayanakları ayarlama

### <a name="portal"></a>[Portal](#tab/azure-portal)

1. Sabit durumda tutulması gereken blobların depolanması için yeni bir kapsayıcı oluşturun veya mevcut bir kapsayıcıyı seçin. Kapsayıcı genel amaçlı v2 veya Blob depolama hesabında olmalıdır.

2. Kapsayıcı ayarlarında **Access ilkesini** seçin. Daha sonra **Değişmez blob depolama**altında **ilke ekle'yi** seçin.

    ![Portaldaki konteyner ayarları](media/storage-blob-immutability-policies-manage/portal-image-1.png)

3. Zaman tabanlı bekletmeyi etkinleştirmek için açılır menüden **Zaman tabanlı bekletme'yi** seçin.

    !["İlke türü" altında seçilen "Zaman tabanlı bekletme"](media/storage-blob-immutability-policies-manage/portal-image-2.png)

4. Gün içinde bekletme aralığıgirin (kabul edilebilir değerler 1 ila 146000 gün arasındadır).

    !["Bekletme süresini güncelleştirme" kutusu](media/storage-blob-immutability-policies-manage/portal-image-5-retention-interval.png)

    İlkenin ilk durumu, özelliği sınamanızı ve kilitlemeden önce ilkede değişiklik yapmanızı sağlayan bir kilitlenir. Sec 17a-4 gibi düzenlemelere uygunluk için politikanın kilitlenmesi esastır.

5. İlkeyi kilitleyin. Elipsis 'e **(...**) sağ tıklanır ve aşağıdaki menü ek eylemlerle birlikte görünür:

    ![Menüde "Kilit lek", ilke](media/storage-blob-immutability-policies-manage/portal-image-4-lock-policy.png)

6. **Kilit Lek.** seçeneğini belirleyin ve kilidi onaylayın. İlke artık kilitli ve silinemez, yalnızca bekletme aralığının uzantılarına izin verilir. Blob siler ve geçersiz kılara izin verilmez. 

    ![Menüdeki "Kilitleme ilkesini" onaylama](media/storage-blob-immutability-policies-manage/portal-image-5-lock-policy.png)

7. Yasal dayanakları etkinleştirmek için **İlke Ekle'yi**seçin. Açılan menüden **Yasal tutun'u** seçin.

    ![Menüde "İlke türü" altında "Yasal tutma"](media/storage-blob-immutability-policies-manage/portal-image-legal-hold-selection-7.png)

8. Bir veya daha fazla etiketle yasal bir tutun oluşturun.

    ![İlke türü altında "etiket adı" kutusu](media/storage-blob-immutability-policies-manage/portal-image-set-legal-hold-tags.png)

9. Yasal bir beklemeyi temizlemek için, uygulanan yasal tutma tanımlayıcıetiketini kaldırın.

### <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Özellik aşağıdaki komut gruplarında yer `az storage container immutability-policy` aldı: ve `az storage container legal-hold`. Komutları görmek için üzerlerine koşun. `-h`

### <a name="powershell"></a>[Powershell](#tab/azure-powershell)

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Az.Storage modülü değişmez depolamayı destekler.  Özelliği etkinleştirmek için aşağıdaki adımları izleyin:

1. PowerShellGet'in en son sürümünün yüklü `Install-Module PowerShellGet –Repository PSGallery –Force`olduğundan emin olun: .
2. Azure PowerShell'in önceki yüklemelerini kaldırın.
3. Azure PowerShell'i yükleyin: `Install-Module Az –Repository PSGallery –AllowClobber`.

Aşağıdaki örnek PowerShell komut dosyası başvuru içindir. Bu komut dosyası yeni bir depolama hesabı ve kapsayıcı oluşturur. Daha sonra, yasal tutmaları nasıl ayarlayıp temize atabileceğinizi, zaman tabanlı bekletme ilkesini (immutability ilkesi olarak da bilinir) nasıl oluşturup kilitleyip kilitleyip bekletme aralığını nasıl uzatabileceğinizi gösterir.

İlk olarak, bir Azure Depolama hesabı oluşturun:

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
$storageAccount = New-AzStorageAccount -ResourceGroupName $resourceGroup -StorageAccountName `
    $storageAccount -SkuName Standard_ZRS -Location $location -Kind StorageV2

# Create a new container using the context
$container = New-AzStorageContainer -Name $container -Context $account.Context

# List the containers in the account
Get-AzStorageContainer -Context $account.Context

# Remove a container
Remove-AzStorageContainer -Name $container -Context $account.Context
```

Set ve net yasal tutarlar:

```powershell
# Set a legal hold
Add-AzRmStorageContainerLegalHold -ResourceGroupName $resourceGroup `
    -StorageAccountName $storageAccount -Name $container -Tag <tag1>,<tag2>,...

# Clear a legal hold
Remove-AzRmStorageContainerLegalHold -ResourceGroupName $resourceGroup `
    -StorageAccountName $storageAccount -Name $container -Tag <tag3>
```

Zaman adayalı zamana dayalı geçirimlilik ilkeleri oluşturun veya güncelleştirin:

```powershell
# Create a time-based immutablity policy
Set-AzRmStorageContainerImmutabilityPolicy -ResourceGroupName $resourceGroup `
    -StorageAccountName $storageAccount -ContainerName $container -ImmutabilityPeriod 10
```

Geri alınamazlık ilkelerini alın:

```powershell
# Get an immutability policy
Get-AzRmStorageContainerImmutabilityPolicy -ResourceGroupName $resourceGroup `
    -StorageAccountName $storageAccount -ContainerName $container
```

Değişmezlik ilkelerini kilitleyin `-Force` (komut istemini kapatmak için ekleyin):

```powershell
# Lock immutability policies
$policy = Get-AzRmStorageContainerImmutabilityPolicy -ResourceGroupName `
    $resourceGroup -StorageAccountName $storageAccount -ContainerName $container
Lock-AzRmStorageContainerImmutabilityPolicy -ResourceGroupName `
    $resourceGroup -StorageAccountName $storageAccount -ContainerName $container `
    -Etag $policy.Etag
```

İşgöremezlik ilkelerini genişletin:

```powershell
# Extend immutability policies
$policy = Get-AzRmStorageContainerImmutabilityPolicy -ResourceGroupName `
    $resourceGroup -StorageAccountName $storageAccount -ContainerName $container

Set-AzRmStorageContainerImmutabilityPolicy -ImmutabilityPolicy `
    $policy -ImmutabilityPeriod 11 -ExtendPolicy
```

Kilitsiz bir değişmezlik ilkesini `-Force` kaldırın (istemi kapatmak için ekleyin):

```powershell
# Remove an unlocked immutability policy
$policy = Get-AzRmStorageContainerImmutabilityPolicy -ResourceGroupName `
    $resourceGroup -StorageAccountName $storageAccount -ContainerName $container

Remove-AzRmStorageContainerImmutabilityPolicy -ImmutabilityPolicy $policy
```

---

## <a name="enabling-allow-protected-append-blobs-writes"></a>Etkinleştirme korumalı ek blobs yazmaizin

### <a name="portal"></a>[Portal](#tab/azure-portal)

![Ek ek yazmalar izin ver](media/storage-blob-immutability-policies-manage/immutable-allow-additional-append-writes.png)

### <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Özellik aşağıdaki komut gruplarında yer `az storage container immutability-policy` aldı: ve `az storage container legal-hold`. Komutları görmek için üzerlerine koşun. `-h`

### <a name="powershell"></a>[Powershell](#tab/azure-powershell)

```powershell
# Create an immutablity policy with appends allowed
Set-AzRmStorageContainerImmutabilityPolicy -ResourceGroupName $resourceGroup `
    -StorageAccountName $storageAccount -ContainerName $container -ImmutabilityPeriod 10 -AllowProtectedAppendWrite $true
```

---

## <a name="next-steps"></a>Sonraki adımlar

[İş açısından kritik blob verilerini değişmez depolama yla depolama](storage-blob-immutable-storage.md)
