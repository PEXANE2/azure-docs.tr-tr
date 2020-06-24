---
title: Nesne çoğaltmasını yapılandırma (Önizleme)
titleSuffix: Azure Storage
description: Blok bloblarını bir depolama hesabındaki bir kapsayıcıdan diğerine zaman uyumsuz olarak kopyalamak için nesne çoğaltmasını nasıl yapılandıracağınızı öğrenin.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 06/16/2020
ms.author: tamram
ms.subservice: blobs
ms.openlocfilehash: 9cb9f1a33c37487f4bfb1419d45d4e42a862d815
ms.sourcegitcommit: e3c28affcee2423dc94f3f8daceb7d54f8ac36fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/17/2020
ms.locfileid: "84888109"
---
# <a name="configure-object-replication-for-block-blobs-preview"></a>Blok Blobları için nesne çoğaltmasını yapılandırma (Önizleme)

Nesne çoğaltma (Önizleme), blok bloblarını bir kaynak depolama hesabı ve hedef hesap arasında zaman uyumsuz olarak kopyalar. Nesne çoğaltma hakkında daha fazla bilgi için bkz. [nesne çoğaltma (Önizleme)](object-replication-overview.md).

Nesne çoğaltmasını yapılandırdığınızda, kaynak depolama hesabı ve hedef hesap belirten bir çoğaltma ilkesi oluşturursunuz. Çoğaltma İlkesi, kaynak kapsayıcısını ve hedef kapsayıcıyı belirten bir veya daha fazla kural içerir ve kaynak kapsayıcısındaki hangi blok bloblarının çoğaltılacağı anlamına gelir.

Bu makalede, Azure portal, PowerShell veya Azure CLı kullanarak depolama hesabınız için nesne çoğaltmanın nasıl yapılandırılacağı açıklanır. Ayrıca, nesne çoğaltmasını yapılandırmak için Azure depolama kaynak sağlayıcısı istemci kitaplıklarından birini de kullanabilirsiniz.

## <a name="create-a-replication-policy-and-rules"></a>Çoğaltma İlkesi ve kuralları oluşturma

Nesne çoğaltmasını yapılandırmadan önce, zaten mevcut değilse kaynak ve hedef depolama hesapları oluşturun. Her iki hesap da genel amaçlı v2 depolama hesabı olmalıdır. Daha fazla bilgi için bkz. [Azure depolama hesabı oluşturma](../common/storage-account-create.md).

Bir depolama hesabı, en fazla iki hedef hesap için kaynak hesap olarak görev yapabilir. Ve bir hedef hesabın ikiden fazla kaynak hesabı olamaz. Kaynak ve hedef hesaplar farklı bölgelerde bulunabilir. Hedef hesapların her birine veri çoğaltmak için ayrı çoğaltma ilkeleri yapılandırabilirsiniz.

Başlamadan önce, aşağıdaki özellik önizlemeleri için kaydolduğunuzdan emin olun:

- [Nesne çoğaltma (Önizleme)](object-replication-overview.md)
- [Blob sürümü oluşturma (Önizleme)](versioning-overview.md)
- [Azure Blob depolamada akış desteğini değiştirme (Önizleme)](storage-blob-change-feed.md)

# <a name="azure-portal"></a>[Azure portal](#tab/portal)

Azure portal nesne çoğaltmasını yapılandırmadan önce, zaten mevcut değilse kaynak ve hedef kapsayıcıları ilgili depolama hesaplarında oluşturun. Ayrıca, blob sürümü oluşturma ve kaynak hesapta akışı değiştirme ve hedef hesapta blob sürümü oluşturma 'yı etkinleştirme.

Azure portal bir çoğaltma ilkesi oluşturmak için şu adımları izleyin:

1. Azure portal kaynak depolama hesabına gidin.
1. **BLOB hizmeti**altında, **nesne çoğaltma**' yı seçin.
1. **Çoğaltmayı ayarla**' yı seçin.
1. Hedef aboneliği ve depolama hesabını seçin.
1. **Kapsayıcı çiftleri** bölümünde, kaynak hesaptan kaynak kapsayıcısını ve hedef hesaptan bir hedef kapsayıcıyı seçin. Çoğaltma İlkesi başına en fazla 10 kapsayıcı çifti oluşturabilirsiniz.

    Aşağıdaki görüntüde bir çoğaltma kuralları kümesi gösterilmektedir.

    :::image type="content" source="media/object-replication-configure/configure-replication-policy.png" alt-text="Azure portal 'de çoğaltma kurallarını gösteren ekran görüntüsü":::

1. İsterseniz yalnızca önek düzeniyle eşleşen Blobları kopyalamak için bir veya daha fazla filtre belirtin. Örneğin, bir ön ek belirtirseniz `b` , yalnızca adı Bu harfle başlayan Bloblar çoğaltılır. Önekinin bir parçası olarak bir sanal dizin belirtebilirsiniz.

    Aşağıdaki görüntüde, bir çoğaltma kuralının bir parçası olarak hangi Blobların kopyalanacağını kısıtlayan filtreler gösterilmektedir.

    :::image type="content" source="media/object-replication-configure/configure-replication-copy-prefix.png" alt-text="Bir çoğaltma kuralı için filtreleri gösteren ekran görüntüsü":::

1. Varsayılan olarak, kopyalama kapsamı yalnızca yeni nesneleri kopyalamak üzere ayarlanır. Kapsayıcıdaki tüm nesneleri kopyalamak veya özel bir tarih ve saatten başlayarak nesneleri kopyalamak için, **değişiklik** bağlantısını seçin ve kapsayıcı çiftinin kopya kapsamını yapılandırın.

    Aşağıdaki görüntüde özel bir kopya kapsamı gösterilmektedir.

    :::image type="content" source="media/object-replication-configure/configure-replication-copy-scope.png" alt-text="Nesne çoğaltma için özel kopya kapsamını gösteren ekran görüntüsü":::

1. Çoğaltma ilkesini oluşturmak ve verileri çoğaltmaya başlamak için **Kaydet ve Uygula '** yı seçin.

# <a name="powershell"></a>[PowerShell](#tab/powershell)

PowerShell ile bir çoğaltma ilkesi oluşturmak için, önce sürüm 2.0.1 'yi yükler-az. Storage PowerShell modülünün [önizlemesi](https://www.powershellgallery.com/packages/Az.Storage/2.0.1-preview) . Önizleme modülünü yüklemek için aşağıdaki adımları izleyin:

1. **Ayarlar**altındaki **uygulamalar & Özellikler** ayarını kullanarak Windows 'un önceki Azure PowerShell yüklemelerini kaldırın.

1. PowerShellGet 'in en son sürümüne sahip olduğunuzdan emin olun. Bir Windows PowerShell penceresi açın ve en son sürümü yüklemek için aşağıdaki komutu çalıştırın:

    ```powershell
    Install-Module PowerShellGet –Repository PSGallery –Force
    ```

    PowerShellGet yükledikten sonra PowerShell penceresini kapatın ve yeniden açın.

1. Azure PowerShell en son sürümünü yükler:

    ```powershell
    Install-Module Az –Repository PSGallery –AllowClobber
    ```

1. Az. Storage Preview modülünü Install:

    ```powershell
    Install-Module Az.Storage -Repository PSGallery -RequiredVersion 2.0.1-preview -AllowPrerelease -AllowClobber -Force
    ```

Azure PowerShell yükleme hakkında daha fazla bilgi için bkz. [PowerShellGet Ile yükleme Azure PowerShell](/powershell/azure/install-az-ps).

Aşağıdaki örnek, kaynak ve hedef hesaplarda bir çoğaltma ilkesinin nasıl oluşturulacağını göstermektedir. Açılı ayraçlar içindeki değerleri kendi değerlerinizle değiştirmeyi unutmayın:

```powershell
# Sign in to your Azure account.
Connect-AzAccount

# Set variables.
$rgName = "<resource-group>"
$srcAccountName = "<source-storage-account>"
$destAccountName = "<destination-storage-account>"
$srcContainerName1 = "source-container1"
$destContainerName1 = "dest-container1"
$srcContainerName2 = "source-container2"
$destContainerName2 = "dest-container2"

# Enable blob versioning and change feed on the source account.
Update-AzStorageBlobServiceProperty -ResourceGroupName $rgname `
    -StorageAccountName $srcAccountName `
    -EnableChangeFeed $true `
    -IsVersioningEnabled $true

# Enable blob versioning on the destination account.
Update-AzStorageBlobServiceProperty -ResourceGroupName $rgname `
    -StorageAccountName $destAccountName `
    -IsVersioningEnabled $true

# List the service properties for both accounts.
Get-AzStorageBlobServiceProperty -ResourceGroupName $rgname `
    -StorageAccountName $srcAccountName
Get-AzStorageBlobServiceProperty -ResourceGroupName $rgname `
    -StorageAccountName $destAccountName

# Create containers in the source and destination accounts.
Get-AzStorageAccount -ResourceGroupName $rgname -StorageAccountName $srcAccountName |
    New-AzStorageContainer $srcContainerName1
Get-AzStorageAccount -ResourceGroupName $rgname -StorageAccountName $destAccountName |
    New-AzStorageContainer $destContainerName1
Get-AzStorageAccount -ResourceGroupName $rgname -StorageAccountName $srcAccountName |
    New-AzStorageContainer $srcContainerName2
Get-AzStorageAccount -ResourceGroupName $rgname -StorageAccountName $destAccountName |
    New-AzStorageContainer $destContainerName2

# Define replication rules for each container.
$rule1 = New-AzStorageObjectReplicationPolicyRule -SourceContainer $srcContainerName1 `
    -DestinationContainer $destContainerName1 `
    -PrefixMatch b
$rule2 = New-AzStorageObjectReplicationPolicyRule -SourceContainer $srcContainerName2 `
    -DestinationContainer $destContainerName2  `
    -MinCreationTime 2020-05-10T00:00:00Z

# Create the replication policy on the destination account.
$destPolicy = Set-AzStorageObjectReplicationPolicy -ResourceGroupName $rgname `
    -StorageAccountName $destAccountName `
    -PolicyId default `
    -SourceAccount $srcAccountName `
    -Rule $rule1,$rule2

# Create the same policy on the source account.
Set-AzStorageObjectReplicationPolicy -ResourceGroupName $rgname `
    -StorageAccountName $srcAccountName `
    -InputObject $destPolicy
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Azure CLı ile bir çoğaltma ilkesi oluşturmak için önce Azure depolama için Önizleme uzantısını yüklemeniz gerekir.:

```azurecli
az extension add -n storage-or-preview
```

Ardından, Azure kimlik bilgilerinizle oturum açın:

```azurecli
az login
```

Kaynak ve hedef depolama hesaplarında blob sürüm oluşturmayı etkinleştirin ve kaynak hesapta değişiklik akışını etkinleştirin. Açılı ayraçlar içindeki değerleri kendi değerlerinizle değiştirmeyi unutmayın:

```azurecli
az storage blob service-properties update --resource-group <resource-group> \
    --account-name <source-storage-account> \
    --enable-versioning

az storage blob service-properties update --resource-group <resource-group> \
    --account-name <source-storage-account> \
    --enable-change-feed

az storage blob service-properties update --resource-group <resource-group> \
    --account-name <dest-storage-account> \
    --enable-versioning
```

Kaynak ve hedef kapsayıcıları ilgili depolama hesaplarında oluşturun.

```azurecli
az storage container create --account-name <source-storage-account> --name source-container3 --auth-mode login
az storage container create --account-name <source-storage-account> --name source-container4 --auth-mode login

az storage container create --account-name <dest-storage-account> --name source-container3 --auth-mode login
az storage container create --account-name <dest-storage-account> --name source-container4 --auth-mode login
```

Hedef hesapta yeni bir çoğaltma ilkesi ve ilişkili kurallar oluşturun.

```azurecli
az storage account or-policy create --account-name <dest-storage-account> \
    --resource-group <resource-group> \
    --source-account <source-storage-account> \
    --destination-account <dest-storage-account> \
    --source-container source-container3 \
    --destination-container dest-container3 \
    --min-creation-time '2020-05-10T00:00:00Z' \
    --prefix-match a

az storage account or-policy rule add --account-name <dest-storage-account> \
    --destination-container dest-container4 \
    --policy-id <policy-id> \
    --resource-group <resource-group> \
    --source-container source-container4 \
    --prefix-match b
```

İlkeyi, ilke KIMLIĞINI kullanarak kaynak hesapta oluşturun.

```azurecli
az storage account or-policy show --resource-group <resource-group> \
    --name <dest-storage-account> \
    --policy-id <policy-id> |
    --az storage account or-policy create --resource-group <resource-group> \
    --name <source-storage-account> \
    --policy "@-"
```

---

## <a name="remove-a-replication-policy"></a>Çoğaltma ilkesini kaldırma

Bir çoğaltma ilkesini ve ilişkili kurallarını kaldırmak için Azure portal, PowerShell veya CLı kullanın.

# <a name="azure-portal"></a>[Azure portal](#tab/portal)

Azure portal bir çoğaltma ilkesini kaldırmak için şu adımları izleyin:

1. Azure portal kaynak depolama hesabına gidin.
1. **Ayarlar**altında, **nesne çoğaltma**' yı seçin.
1. İlke adının yanındaki **diğer** düğmesine tıklayın.
1. **Kuralları Sil**' i seçin.

# <a name="powershell"></a>[PowerShell](#tab/powershell)

Bir çoğaltma ilkesini kaldırmak için, ilkeyi kaynak hesaptan ve hedef hesaptan silin. İlke silindiğinde, onunla ilişkili kurallar da silinir.

```powershell
# Remove the policy from the destination account.
Remove-AzStorageObjectReplicationPolicy -ResourceGroupName $rgname `
    -StorageAccountName $destAccountName `
    -PolicyId $destPolicy.PolicyId

# Remove the policy from the source account.
Remove-AzStorageObjectReplicationPolicy -ResourceGroupName $rgname `
    -StorageAccountName $srcAccountName `
    -PolicyId $destPolicy.PolicyId
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Bir çoğaltma ilkesini kaldırmak için, ilkeyi kaynak hesaptan ve hedef hesaptan silin. İlke silindiğinde, onunla ilişkili kurallar da silinir.

```azurecli
az storage account or-policy delete \
    --policy-id $policyid \
    --account-name <source-storage-account> \
    --resource-group <resource-group>

az storage account or-policy delete \
    --policy-id $policyid \
    --account-name <dest-storage-account> \
    --resource-group <resource-group>
```

---

## <a name="next-steps"></a>Sonraki adımlar

- [Nesne çoğaltmasına genel bakış (Önizleme)](object-replication-overview.md)
