---
title: Nesne çoğaltmayı yapılandırma
titleSuffix: Azure Storage
description: Blok bloblarını bir depolama hesabındaki bir kapsayıcıdan diğerine zaman uyumsuz olarak kopyalamak için nesne çoğaltmasını nasıl yapılandıracağınızı öğrenin.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 09/15/2020
ms.author: tamram
ms.subservice: blobs
ms.custom: devx-track-azurecli, devx-track-azurepowershell
ms.openlocfilehash: e6e6c802da212294594f45d0545c6cf07694760b
ms.sourcegitcommit: 7374b41bb1469f2e3ef119ffaf735f03f5fad484
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/16/2020
ms.locfileid: "90707926"
---
# <a name="configure-object-replication-for-block-blobs"></a>Blok Blobları için nesne çoğaltmasını yapılandırma

Nesne çoğaltma, blok bloblarını bir kaynak depolama hesabı ve hedef hesap arasında zaman uyumsuz olarak kopyalar. Nesne çoğaltma hakkında daha fazla bilgi için bkz. [nesne çoğaltma](object-replication-overview.md).

Nesne çoğaltmasını yapılandırdığınızda, kaynak depolama hesabı ve hedef hesap belirten bir çoğaltma ilkesi oluşturursunuz. Çoğaltma İlkesi, kaynak kapsayıcısını ve hedef kapsayıcıyı belirten bir veya daha fazla kural içerir ve kaynak kapsayıcısındaki hangi blok bloblarının çoğaltılacağı anlamına gelir.

Bu makalede, Azure portal, PowerShell veya Azure CLı kullanarak depolama hesabınız için nesne çoğaltmanın nasıl yapılandırılacağı açıklanır. Ayrıca, nesne çoğaltmasını yapılandırmak için Azure depolama kaynak sağlayıcısı istemci kitaplıklarından birini de kullanabilirsiniz.

[!INCLUDE [storage-data-lake-gen2-support](../../../includes/storage-data-lake-gen2-support.md)]

## <a name="create-a-replication-policy-and-rules"></a>Çoğaltma İlkesi ve kuralları oluşturma

Nesne çoğaltmasını yapılandırmadan önce, zaten mevcut değilse kaynak ve hedef depolama hesapları oluşturun. Her iki hesap da genel amaçlı v2 depolama hesabı olmalıdır. Daha fazla bilgi için bkz. [Azure depolama hesabı oluşturma](../common/storage-account-create.md).

Nesne çoğaltma, hem kaynak hem de hedef hesap için blob sürümü oluşturma özelliğinin etkinleştirildiğinden ve kaynak hesap için blob değişiklik akışı 'nın etkin olmasını gerektirir. Blob sürümü oluşturma hakkında daha fazla bilgi için bkz. [BLOB sürümü oluşturma](versioning-overview.md). Değişiklik akışı hakkında daha fazla bilgi için bkz. [Azure Blob depolamada akış desteğini değiştirme](storage-blob-change-feed.md). Bu özelliklerin etkinleştirilmesi, ek maliyetlere neden olabileceğini aklınızda bulundurun.

Bir depolama hesabı, en fazla iki hedef hesap için kaynak hesap olarak görev yapabilir. Kaynak ve hedef hesaplar aynı bölgede veya farklı bölgelerde olabilir. Farklı aboneliklerde ve farklı Azure Active Directory (Azure AD) kiracılarında da bulunabilir. Her hesap çifti için yalnızca bir çoğaltma ilkesi oluşturulabilir.

Nesne çoğaltmasını yapılandırırken, Azure depolama kaynak sağlayıcısı aracılığıyla hedef hesapta bir çoğaltma ilkesi oluşturursunuz. Çoğaltma ilkesi oluşturulduktan sonra, Azure Storage buna bir ilke KIMLIĞI atar. Daha sonra bu çoğaltma ilkesini, ilke KIMLIĞINI kullanarak kaynak hesapla ilişkilendirmeniz gerekir. Çoğaltmanın gerçekleşmesi için kaynak ve hedef hesapların ilke KIMLIĞI aynı olmalıdır.

Bir depolama hesabı için bir nesne çoğaltma ilkesi yapılandırmak için, depolama hesabı düzeyine veya daha yüksek olan Azure Resource Manager **katkıda** bulunan rolü atanmalıdır. Daha fazla bilgi için bkz. Azure rol tabanlı Access Control (RBAC) belgelerindeki [Azure yerleşik rolleri](../../role-based-access-control/built-in-roles.md) .

### <a name="configure-object-replication-when-you-have-access-to-both-storage-accounts"></a>Her iki depolama hesabına erişiminiz olduğunda nesne çoğaltmasını yapılandırma

Hem kaynak hem de hedef depolama hesaplarına erişiminiz varsa, her iki hesap üzerinde de nesne çoğaltma ilkesini yapılandırabilirsiniz.

Azure portal nesne çoğaltmasını yapılandırmadan önce, zaten mevcut değilse kaynak ve hedef kapsayıcıları ilgili depolama hesaplarında oluşturun. Ayrıca, blob sürüm oluşturmayı etkinleştirin ve kaynak hesapta akışı değiştirin ve hedef hesapta blob sürümü oluşturmayı etkinleştirin.

# <a name="azure-portal"></a>[Azure Portal](#tab/portal)

Azure portal, ilkeyi hedef hesap için yapılandırdıktan sonra kaynak hesapta otomatik olarak oluşturur.

Azure portal bir çoğaltma ilkesi oluşturmak için şu adımları izleyin:

1. Azure portal kaynak depolama hesabına gidin.
1. **BLOB hizmeti**altında, **nesne çoğaltma**' yı seçin.
1. **Çoğaltma kurallarını ayarla**' yı seçin.
1. Hedef aboneliği ve depolama hesabını seçin.
1. **Kapsayıcı çiftleri** bölümünde, kaynak hesaptan kaynak kapsayıcısını ve hedef hesaptan bir hedef kapsayıcıyı seçin. Çoğaltma İlkesi başına en fazla 10 kapsayıcı çifti oluşturabilirsiniz.

    Aşağıdaki görüntüde bir çoğaltma kuralları kümesi gösterilmektedir.

    :::image type="content" source="media/object-replication-configure/configure-replication-policy.png" alt-text="Azure portal 'de çoğaltma kurallarını gösteren ekran görüntüsü":::

1. İsterseniz yalnızca önek düzeniyle eşleşen Blobları kopyalamak için bir veya daha fazla filtre belirtin. Örneğin, bir ön ek belirtirseniz `b` , yalnızca adı Bu harfle başlayan Bloblar çoğaltılır. Önekinin bir parçası olarak bir sanal dizin belirtebilirsiniz. Ön ek dizesi joker karakterleri desteklemiyor.

    Aşağıdaki görüntüde, bir çoğaltma kuralının bir parçası olarak hangi Blobların kopyalanacağını kısıtlayan filtreler gösterilmektedir.

    :::image type="content" source="media/object-replication-configure/configure-replication-copy-prefix.png" alt-text="Bir çoğaltma kuralı için filtreleri gösteren ekran görüntüsü":::

1. Varsayılan olarak, kopyalama kapsamı yalnızca yeni nesneleri kopyalamak üzere ayarlanır. Kapsayıcıdaki tüm nesneleri kopyalamak veya özel bir tarih ve saatten başlayarak nesneleri kopyalamak için, **değişiklik** bağlantısını seçin ve kapsayıcı çiftinin kopya kapsamını yapılandırın.

    Aşağıdaki görüntüde nesneleri belirtilen bir tarih ve saatten kopyalayan özel bir kopya kapsamı gösterilmektedir.

    :::image type="content" source="media/object-replication-configure/configure-replication-copy-scope.png" alt-text="Nesne çoğaltma için özel kopya kapsamını gösteren ekran görüntüsü":::

1. Çoğaltma ilkesini oluşturmak ve verileri çoğaltmaya başlamak için **Kaydet ve Uygula '** yı seçin.

Nesne çoğaltmasını yapılandırdıktan sonra, aşağıdaki görüntüde gösterildiği gibi, Azure portal çoğaltma ilkesini ve kurallarını görüntüler.

:::image type="content" source="media/object-replication-configure/object-replication-policies-portal.png" alt-text="Azure portal nesne çoğaltma ilkesini gösteren ekran görüntüsü":::

# <a name="powershell"></a>[PowerShell](#tab/powershell)

PowerShell ile bir çoğaltma ilkesi oluşturmak için, önce az. Storage PowerShell modülünün sürüm [2.5.0](https://www.powershellgallery.com/packages/Az.Storage/2.5.0) veya üstünü yüklemeniz gerekir. Azure PowerShell yükleme hakkında daha fazla bilgi için bkz. [PowerShellGet Ile yükleme Azure PowerShell](/powershell/azure/install-az-ps).

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

Azure CLı ile bir çoğaltma ilkesi oluşturmak için önce Azure CLı sürüm 2.11.1 veya üstünü yüklemeniz gerekir. Daha fazla bilgi için bkz. [Azure CLI kullanmaya başlama](/cli/azure/get-started-with-azure-cli).

Ardından, kaynak ve hedef depolama hesaplarında blob sürüm oluşturmayı etkinleştirin ve [az Storage Account blob-Service-Properties Update](/cli/azure/storage/account/blob-service-properties#az_storage_account_blob_service_properties_update) komutunu çağırarak kaynak hesapta değişiklik akışını etkinleştirin. Açılı ayraçlar içindeki değerleri kendi değerlerinizle değiştirmeyi unutmayın:

```azurecli
az login

az storage account blob-service-properties update \
    --resource-group <resource-group> \
    --account-name <source-storage-account> \
    --enable-versioning

az storage account blob-service-properties update \
    --resource-group <resource-group> \
    --account-name <source-storage-account> \
    --enable-change-feed

az storage account blob-service-properties update \
    --resource-group <resource-group> \
    --account-name <dest-storage-account> \
    --enable-versioning
```

Kaynak ve hedef kapsayıcıları ilgili depolama hesaplarında oluşturun.

```azurecli
az storage container create \
    --account-name <source-storage-account> \
    --name source-container-1 \
    --auth-mode login
az storage container create \
    --account-name <source-storage-account> \
    --name source-container-2 \
    --auth-mode login

az storage container create \
    --account-name <dest-storage-account> \
    --name dest-container-1 \
    --auth-mode login
az storage container create \
    --account-name <dest-storage-account> \
    --name dest-container-1 \
    --auth-mode login
```

[Az Storage Account veya-Policy Create](/cli/azure/storage/account/or-policy#az_storage_account_or_policy_create)yöntemini çağırarak hedef hesapta yeni bir çoğaltma ilkesi ve ilişkili bir kural oluşturun.

```azurecli
az storage account or-policy create \
    --account-name <dest-storage-account> \
    --resource-group <resource-group> \
    --source-account <source-storage-account> \
    --destination-account <dest-storage-account> \
    --source-container source-container-1 \
    --destination-container dest-container-1 \
    --min-creation-time '2020-09-10T00:00:00Z' \
    --prefix-match a

```

Azure depolama, yeni ilkenin oluşturulduğu sırada ilke KIMLIĞINI ayarlar. İlkeye ek kurallar eklemek için [az Storage Account veya-Policy kuralını](/cli/azure/storage/account/or-policy/rule#az_storage_account_or_policy_rule_add) çağırın ve ilke kimliğini sağlayın.

```azurecli
az storage account or-policy rule add \
    --account-name <dest-storage-account> \
    --resource-group <resource-group> \
    --source-container source-container-2 \
    --destination-container dest-container-2 \
    --policy-id <policy-id> \
    --prefix-match b
```

Sonra, ilke KIMLIĞINI kullanarak ilkeyi kaynak hesapta oluşturun.

```azurecli
az storage account or-policy show \
    --resource-group <resource-group> \
    --account-name <dest-storage-account> \
    --policy-id <policy-id> |
    az storage account or-policy create --resource-group <resource-group> \
    --account-name <source-storage-account> \
    --policy "@-"
```

---

### <a name="configure-object-replication-when-you-have-access-only-to-the-destination-account"></a>Yalnızca hedef hesaba erişiminiz olduğunda nesne çoğaltmasını yapılandırma

Kaynak depolama hesabı için izinleriniz yoksa, hedef hesapta nesne çoğaltmasını yapılandırabilir ve kaynak hesapta aynı ilkeyi oluşturmak için başka bir kullanıcıya ilke tanımını içeren bir JSON dosyası sağlayabilirsiniz. Örneğin, kaynak hesap hedef hesaptan farklı bir Azure AD kiracısında ise, nesne çoğaltmasını yapılandırmak için bu yaklaşımı kullanabilirsiniz.

İlkeyi oluşturmak için, hedef depolama hesabı düzeyine veya daha yüksek Azure Resource Manager **katkıda bulunan** rolün atanması gerektiğini unutmayın. Daha fazla bilgi için bkz. Azure rol tabanlı Access Control (RBAC) belgelerindeki [Azure yerleşik rolleri](../../role-based-access-control/built-in-roles.md) .

Aşağıdaki tabloda, her senaryodaki JSON dosyasındaki ilke KIMLIĞI ve kural kimlikleri için hangi değerlerin kullanılacağı özetlenmektedir.

| Bu hesap için JSON dosyasını oluştururken... | İlke KIMLIĞI ve kural kimliklerini bu değere ayarlayın... |
|-|-|
| Hedef hesap | *Varsayılan*dize değeri. Azure depolama, ilke KIMLIĞI ve kural kimliklerini sizin için oluşturur. |
| Kaynak hesap | Hedef hesapta tanımlı ilkeyi JSON dosyası olarak indirdiğinizde döndürülen ilke KIMLIĞI ve kural kimliklerinin değerleri. |

Aşağıdaki örnek, *b* önekiyle eşleşen tek bir kuralla hedef hesapta bir çoğaltma ilkesi tanımlar ve çoğaltılacak Bloblar için en düşük oluşturma süresini ayarlar. Açılı ayraçlar içindeki değerleri kendi değerlerinizle değiştirmeyi unutmayın:

```json
{
  "properties": {
    "policyId": "default",
    "sourceAccount": "<source-account>",
    "destinationAccount": "<dest-account>",
    "rules": [
      {
        "ruleId": "default",
        "sourceContainer": "<source-container>",
        "destinationContainer": "<destination-container>",
        "filters": {
          "prefixMatch": [
            "b"
          ],
          "minCreationTime": "2020-08-028T00:00:00Z"
        }
      }
    ]
  }
}
```

# <a name="azure-portal"></a>[Azure Portal](#tab/portal)

Hedef hesapta nesne çoğaltmasını Azure portal bir JSON dosyası ile yapılandırmak için aşağıdaki adımları izleyin:

1. Hedef hesapta çoğaltma ilkesini tanımlayan yerel bir JSON dosyası oluşturun. Azure Storage 'ın ilke KIMLIĞINI tanımlayabilmesi için **PolicyId** alanını **Default** olarak ayarlayın.

    Çoğaltma ilkesini tanımlayan bir JSON dosyası oluşturmanın kolay bir yolu, öncelikle Azure portal iki depolama hesabı arasında bir test çoğaltma ilkesi oluşturmaktır. Daha sonra çoğaltma kurallarını indirebilir ve JSON dosyasını gerektiği gibi değiştirebilirsiniz.

1. Azure portal hedef hesabın **nesne çoğaltma** ayarları ' na gidin.
1. **Çoğaltma kurallarını karşıya yükle**' yi seçin.
1. JSON dosyasını karşıya yükleyin. Azure portal, aşağıdaki görüntüde gösterildiği gibi, oluşturulacak ilke ve kuralları görüntüler.

    :::image type="content" source="media/object-replication-configure/replication-rules-upload-portal.png" alt-text="Çoğaltma İlkesi tanımlamak için bir JSON dosyasının nasıl karşıya yükleneceğini gösteren ekran görüntüsü":::

1. Hedef hesapta çoğaltma ilkesini oluşturmak için **karşıya yükle** ' yi seçin.

Daha sonra kaynak hesabı yapılandırmak için başka bir kullanıcıya sağlayabileceğiniz ilke tanımını içeren bir JSON dosyası indirebilirsiniz. Bu JSON dosyasını indirmek için şu adımları izleyin:

1. Azure portal hedef hesabın **nesne çoğaltma** ayarları ' na gidin.
1. İndirmek istediğiniz ilkenin yanındaki **daha fazla** düğmesini seçin, ardından aşağıdaki görüntüde gösterildiği gibi, **kuralları indir**' i seçin.

    :::image type="content" source="media/object-replication-configure/replication-rules-download-portal.png" alt-text="Çoğaltma kurallarının bir JSON dosyasına nasıl indirileceği gösteren ekran görüntüsü":::

1. Kaynak hesapta ilkeyi yapılandırmak üzere başka bir kullanıcıyla paylaşmak için JSON dosyasını yerel bilgisayarınıza kaydedin.

İndirilen JSON dosyası, hedef hesapta ilke için Azure Storage 'ın oluşturduğu ilke KIMLIĞINI içerir. Kaynak hesapta nesne çoğaltmasını yapılandırmak için aynı ilke KIMLIĞINI kullanmanız gerekir.

Kaynak hesapta aynı ilkeyi otomatik olarak oluşturmaz Azure portal aracılığıyla hedef hesap için bir çoğaltma ilkesi oluşturmak üzere bir JSON dosyası yüklemeyi unutmayın. Azure depolama nesneleri çoğaltmaya başlamadan önce başka bir kullanıcının ilkeyi kaynak hesapta oluşturması gerekir.

# <a name="powershell"></a>[PowerShell](#tab/powershell)

PowerShell 'den hedef hesabın çoğaltma ilkesi tanımını içeren bir JSON dosyasını indirmek için, ilkeyi döndürmek üzere [Get-AzStorageObjectReplicationPolicy](/powershell/module/az.storage/get-azstorageobjectreplicationpolicy) komutunu çağırın. Ardından ilkeyi JSON 'a dönüştürün ve aşağıdaki örnekte gösterildiği gibi yerel bir dosya olarak kaydedin. Açılı ayraçlar ve dosya yolundaki değerleri kendi değerlerinizle değiştirmeyi unutmayın:

```powershell
$rgName = "<resource-group>"
$destAccountName = "<destination-storage-account>"

$destPolicy = Get-AzStorageObjectReplicationPolicy -ResourceGroupName $rgname `
    -StorageAccountName $destAccountName
$destPolicy | ConvertTo-Json -Depth 5 > c:\temp\json.txt
```

Kaynak hesapta PowerShell ile çoğaltma ilkesini yapılandırmak üzere JSON dosyasını kullanmak için yerel dosyayı alın ve JSON 'dan bir nesneye dönüştürün. Ardından, aşağıdaki örnekte gösterildiği gibi, ilkeyi kaynak hesapta yapılandırmak için [set-AzStorageObjectReplicationPolicy](/powershell/module/az.storage/set-azstorageobjectreplicationpolicy) komutunu çağırın. Açılı ayraçlar ve dosya yolundaki değerleri kendi değerlerinizle değiştirmeyi unutmayın:

```powershell
$object = Get-Content -Path C:\temp\json.txt | ConvertFrom-Json
Set-AzStorageObjectReplicationPolicy -ResourceGroupName $rgname `
    -StorageAccountName $srcAccountName `
    -PolicyId $object.PolicyId `
    -SourceAccount $object.SourceAccount `
    -DestinationAccount $object.DestinationAccount `
    -Rule $object.Rules
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Hedef hesabın çoğaltma ilkesi tanımını Azure CLı 'dan bir JSON dosyasına yazmak için [az Storage Account veya-Policy komutunu ya](/cli/azure/storage/account/or-policy#az_storage_account_or_policy_show) da bir dosyaya çıktıyı göster komutunu çağırın.

Aşağıdaki örnek, ilke tanımını *policy.js*ADLı bir JSON dosyasına yazar. Açılı ayraçlar ve dosya yolundaki değerleri kendi değerlerinizle değiştirmeyi unutmayın:

```azurecli
az storage account or-policy show \
    --account-name <dest-account-name> \
    --policy-id  <policy-id> > policy.json
```

Azure CLı ile kaynak hesapta çoğaltma ilkesini yapılandırmak üzere JSON dosyasını kullanmak için, [az Storage Account veya-Policy Create](/cli/azure/storage/account/or-policy#az_storage_account_or_policy_create) komutunu çağırın ve dosyadaki *policy.js* başvurun. Açılı ayraçlar ve dosya yolundaki değerleri kendi değerlerinizle değiştirmeyi unutmayın:

```azurecli
az storage account or-policy create \
    -resource-group <resource-group> \
    --source-account <source-account-name> \
    --policy @policy.json
```

---

## <a name="remove-a-replication-policy"></a>Çoğaltma ilkesini kaldırma

Bir çoğaltma ilkesini ve ilişkili kurallarını kaldırmak için Azure portal, PowerShell veya CLı kullanın.

# <a name="azure-portal"></a>[Azure Portal](#tab/portal)

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
    --policy-id <policy-id> \
    --account-name <source-storage-account> \
    --resource-group <resource-group>

az storage account or-policy delete \
    --policy-id <policy-id> \
    --account-name <dest-storage-account> \
    --resource-group <resource-group>
```

---

## <a name="next-steps"></a>Sonraki adımlar

- [Nesne çoğaltmasına genel bakış](object-replication-overview.md)
- [Blob sürüm oluşturmayı etkinleştirme ve yönetme](versioning-enable.md)
- [Azure Blob depolamada işlem değişiklik akışı](storage-blob-change-feed-how-to.md)
