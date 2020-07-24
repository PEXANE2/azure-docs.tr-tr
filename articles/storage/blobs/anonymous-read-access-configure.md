---
title: Kapsayıcılar ve BLOB 'lar için anonim genel okuma erişimini yapılandırma
titleSuffix: Azure Storage
description: Depolama hesabı için blob verilerine anonim erişim izni verme veya bu erişimi engelleme hakkında bilgi edinin. Kapsayıcıları ve Blobları anonim erişim için kullanılabilir hale getirmek için kapsayıcı genel erişim ayarını ayarlayın.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 07/23/2020
ms.author: tamram
ms.reviewer: fryu
ms.openlocfilehash: a153a3735bbc46dbbce7e58374e1015ac1ec0bfb
ms.sourcegitcommit: 0e8a4671aa3f5a9a54231fea48bcfb432a1e528c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87133189"
---
# <a name="configure-anonymous-public-read-access-for-containers-and-blobs"></a>Kapsayıcılar ve BLOB 'lar için anonim genel okuma erişimini yapılandırma

Azure depolama, kapsayıcılar ve BLOB 'lar için isteğe bağlı anonim genel okuma erişimini destekler. Varsayılan olarak verilerinize anonim erişime izin verilmez. Anonim erişimi açıkça etkinleştirmediğiniz takdirde, bir kapsayıcıya ve bloblarına yapılan tüm isteklere Azure Active Directory (Azure AD) ya da paylaşılan anahtar yetkilendirmesi kullanılarak yetkilendirilmelidir. Anonim erişime izin vermek için bir kapsayıcının ortak erişim düzeyi ayarını yapılandırdığınızda, istemciler bu kapsayıcıdaki verileri, isteği yetkilendirmeden okuyabilir.

> [!WARNING]
> Bir kapsayıcı genel erişim için yapılandırıldığında, herhangi bir istemci o kapsayıcıdaki verileri okuyabilir. Genel erişim olası bir güvenlik riski sunar, bu nedenle senaryonuz bunu gerektirmiyorsa, Microsoft bu BT 'nin depolama hesabı için izin vermemenizi önerir. Daha fazla bilgi için bkz. [kapsayıcılar ve bloblara anonim genel okuma erişimini engelleme](anonymous-read-access-prevent.md).

Bu makalede bir kapsayıcı ve Blobları için anonim genel okuma erişiminin nasıl yapılandırılacağı açıklanır. Blob verilerine bir istemci uygulamasından anonim olarak erişme hakkında daha fazla bilgi için bkz. [.NET ile anonim kapsayıcıları ve Blobları anonim olarak erişim](anonymous-read-access-client.md).

## <a name="about-anonymous-public-read-access"></a>Anonim genel okuma erişimi hakkında

Verilerinize genel erişim her zaman varsayılan olarak yasaktır. Genel erişimi etkileyen iki ayrı ayar vardır:

1. **Depolama hesabı için genel erişime izin verin.** Varsayılan olarak, bir depolama hesabı bir kullanıcıya bir kapsayıcıya genel erişimi etkinleştirmek için uygun izinler sağlar. Blob verileri, kullanıcının kapsayıcının genel erişim ayarını açıkça yapılandırması için ek adım kullanmadığı takdirde genel erişim için kullanılamaz.
1. **Kapsayıcının ortak erişim ayarını yapılandırın.** Varsayılan olarak, kapsayıcının ortak erişim ayarı devre dışıdır, yani kapsayıcıya veya verilerine yönelik her istek için yetkilendirme gereklidir. Uygun izinlere sahip bir Kullanıcı, yalnızca depolama hesabı için anonim erişime izin verildiğinde anonim erişimi etkinleştirmek için bir kapsayıcının ortak erişim ayarını değiştirebilir.

Aşağıdaki tabloda her iki ayar da bir kapsayıcı için genel erişimi nasıl etkiler özetlenmektedir.

| Genel erişim ayarı | Bir kapsayıcı için genel erişim devre dışı bırakıldı (varsayılan ayar) | Bir kapsayıcı için genel erişim kapsayıcı olarak ayarlanmıştır | Ortak erişim bir kapsayıcının blob olarak ayarlandığı |
|--|--|--|--|
| Depolama hesabı için genel erişime izin verilmiyor | Depolama hesabındaki herhangi bir kapsayıcıya genel erişim yok. | Depolama hesabındaki herhangi bir kapsayıcıya genel erişim yok. Depolama hesabı ayarı kapsayıcı ayarını geçersiz kılar. | Depolama hesabındaki herhangi bir kapsayıcıya genel erişim yok. Depolama hesabı ayarı kapsayıcı ayarını geçersiz kılar. |
| Depolama hesabı (varsayılan ayar) için genel erişime izin verilir | Bu kapsayıcıya genel erişim yok (varsayılan yapılandırma). | Bu kapsayıcıya ve bloblarına genel erişime izin verilir. | Ortak erişime bu kapsayıcıdaki bloblara izin verilir, ancak kapsayıcının kendisini etkilemez. |

## <a name="allow-or-disallow-public-read-access-for-a-storage-account"></a>Depolama hesabı için genel okuma erişimine izin ver veya izin verme

Varsayılan olarak, bir depolama hesabındaki bağımsız kapsayıcılar için genel erişime izin verilir. Ortak erişime izin verildiğinde, uygun izinlere sahip bir Kullanıcı, bu kapsayıcıdaki verilere anonim genel erişimi etkinleştirmek için bir kapsayıcının ortak erişim ayarını değiştirebilir.

Bir kapsayıcıya genel erişimin her zaman varsayılan olarak kapalı olduğunu ve anonim isteklere izin verecek şekilde açıkça yapılandırılması gerektiğini aklınızda bulundurun. Depolama hesabındaki ayardan bağımsız olarak, uygun izinlere sahip bir Kullanıcı, kapsayıcıda genel erişimi etkinleştirmek için bu ek adımı alırsa, verileriniz hiçbir zaman ortak erişime açık olmayacaktır.

Depolama hesabı için genel erişime izin vermemek, bu hesaptaki tüm kapsayıcılara ve bloblara anonim erişimi engeller. Hesap için genel erişime izin verilse, bir kapsayıcı için genel erişim ayarını anonim erişime izin verecek şekilde yapılandırmak mümkün değildir. Microsoft, daha iyi güvenlik için, senaryolarınız kullanıcıların blob kaynaklarına anonim olarak erişmelerini gerektirmediği takdirde depolama hesaplarınız için genel erişime izin vermemeyi önerir.

> [!IMPORTANT]
> Bir depolama hesabı için genel erişime izin vermemek, bu depolama hesabındaki tüm kapsayıcılar için genel erişim ayarlarını geçersiz kılar. Depolama hesabı için genel erişime izin verilse, bu hesaba yönelik ileride yapılacak anonim istekler başarısız olur. Bu ayarı değiştirmeden önce, Depolama hesabınızdaki verilere anonim olarak erişebilecek istemci uygulamalarının etkilerini anladığınızdan emin olun. Daha fazla bilgi için bkz. [kapsayıcılar ve bloblara anonim genel okuma erişimini engelleme](anonymous-read-access-prevent.md).

Bir depolama hesabı için genel erişime izin vermek veya bu erişimi engellemek için, hesabın **Blobpublicaccess** özelliğini yapılandırmak üzere Azure Portal veya Azure CLI kullanın. Bu özellik, Azure Resource Manager dağıtım modeliyle oluşturulan tüm depolama hesapları için kullanılabilir. Daha fazla bilgi için bkz. [depolama hesabına genel bakış](../common/storage-account-overview.md).

# <a name="azure-portal"></a>[Azure Portal](#tab/portal)

Azure portal bir depolama hesabı için genel erişime izin vermek veya bu erişimi engellemek için şu adımları izleyin:

1. Azure portalda depolama hesabınıza gidin.
1. **Ayarlar**' ın altındaki **yapılandırma** ayarını bulun.
1. **BLOB ortak erişimini** **etkin** veya **devre dışı**olarak ayarlayın.

    :::image type="content" source="media/anonymous-read-access-configure/blob-public-access-portal.png" alt-text="Hesap için blob genel erişimine izin verme veya bu erişimi engelleme ile ilgili ekran görüntüsü":::

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Azure CLı ile bir depolama hesabı için genel erişime izin vermek veya erişimi engellemek için, önce [az Resource Show](/cli/azure/resource#az-resource-show) komutunu çağırarak depolama HESABıNıZıN kaynak kimliğini alın. Daha sonra, depolama hesabı için **Allowblobpublicaccess** özelliğini ayarlamak üzere [az Resource Update](/cli/azure/resource#az-resource-update) komutunu çağırın. Ortak erişime izin vermek için **Allowblobpublicaccess** özelliğini true olarak ayarlayın; izin vermemek için **false**olarak ayarlayın.

Aşağıdaki örnek, depolama hesabı için genel blob erişimine izin vermez. Köşeli ayraçlar içindeki yer tutucu değerlerini kendi değerlerinizle değiştirmeyi unutmayın:

```azurecli-interactive
storage_account_id=$(az resource show \
    --name anonpublicaccess \
    --resource-group storagesamples-rg \
    --resource-type Microsoft.Storage/storageAccounts \
    --query id \
    --output tsv)

az resource update \
    --ids $storage_account_id \
    --set properties.allowBlobPublicAccess=false
    ```
```

---

> [!NOTE]
> Bir depolama hesabı için genel erişime izin vermemek, bu depolama hesabında barındırılan herhangi bir statik Web sitesini etkilemez. **$Web** kapsayıcı her zaman herkese açık olarak erişilebilir.

## <a name="check-whether-public-access-is-allowed-for-a-storage-account"></a>Bir depolama hesabı için genel erişime izin verilip verilmediğini denetleyin

Bir depolama hesabı için genel erişime izin verilip verilmeyeceğini denetlemek için **Allowblobpublicaccess** özelliğinin değerini alın. Tek seferde büyük bir depolama hesabı için bu özelliği denetlemek üzere Azure Kaynak Grafiği gezginini kullanın.

> [!IMPORTANT]
> **Allowblobpublicaccess** özelliği varsayılan olarak ayarlı değildir ve açıkça ayarlanana kadar bir değer döndürmez. Depolama hesabı, özellik değeri **null** olduğunda veya **true**olduğunda ortak erişime izin verir.

### <a name="check-whether-public-access-is-allowed-for-a-single-storage-account"></a>Tek bir depolama hesabı için genel erişime izin verilip verilmediğini denetleyin

Azure CLı kullanarak tek bir depolama hesabı için genel erişime izin verilip verilmeyeceğini denetlemek için, **Allowblobpublicaccess** özelliği için [az Resource Show](/cli/azure/resource#az-resource-show) komutunu ve sorguyu çağırın:

```azurecli-interactive
az resource show \
    --name <storage-account> \
    --resource-group <resource-group> \
    --resource-type Microsoft.Storage/storageAccounts \
    --query properties.allowBlobPublicAccess \
    --output tsv
```

### <a name="check-whether-public-access-is-allowed-for-a-set-of-storage-accounts"></a>Bir depolama hesabı kümesi için genel erişime izin verilip verilmediğini denetleyin

En iyi performansa sahip bir depolama hesapları kümesi üzerinde genel erişime izin verilip verilmeyeceğini denetlemek için Azure portal Azure Kaynak Grafiği Gezginini kullanabilirsiniz. Kaynak Grafiği Gezginini kullanma hakkında daha fazla bilgi edinmek için bkz. [hızlı başlangıç: Azure Kaynak Grafiği gezginini kullanarak Ilk kaynak grafik sorgunuzu çalıştırma](/azure/governance/resource-graph/first-query-portal).

Kaynak Graph Explorer 'da aşağıdaki sorguyu çalıştırmak, depolama hesaplarının bir listesini döndürür ve her bir hesap için **Allowblobpublicaccess** özelliğinin değerini görüntüler:

```kusto
resources
| where type =~ 'Microsoft.Storage/storageAccounts'
| extend allowBlobPublicAccess = parse_json(properties).allowBlobPublicAccess
| project subscriptionId, resourceGroup, name, allowBlobPublicAccess
| order by subscriptionId, resourceGroup, name asc
```

## <a name="set-the-public-access-level-for-a-container"></a>Bir kapsayıcı için genel erişim düzeyini ayarlama

Anonim kullanıcılara bir kapsayıcıya ve bloblarına okuma erişimi sağlamak için, önce depolama hesabı için genel erişime izin verin, sonra kapsayıcının ortak erişim düzeyini ayarlayın. Depolama hesabı için genel erişim reddedilirse, bir kapsayıcı için genel erişimi yapılandırameyeceksiniz.

Bir depolama hesabı için genel erişime izin verildiğinde, bir kapsayıcıyı aşağıdaki izinlerle yapılandırabilirsiniz:

- **Genel okuma erişimi yok:** Kapsayıcıya ve bloblarına yalnızca yetkili bir istekle erişilebilir. Bu seçenek, tüm yeni kapsayıcılar için varsayılandır.
- **Yalnızca Bloblar Için genel okuma erişimi:** Kapsayıcı içindeki Bloblar anonim istek tarafından okunabilir, ancak kapsayıcı verileri anonim olarak kullanılamaz. Anonim istemciler kapsayıcı içindeki Blobları numaralandıramaz.
- **Kapsayıcı ve Blobları Için genel okuma erişimi:** Kapsayıcı ve blob verileri, kapsayıcı izin ayarları ve kapsayıcı meta verileri dışında anonim istek tarafından okunabilir. İstemciler kapsayıcı içindeki Blobları anonim istek ile numaralandırabilirler, ancak depolama hesabındaki kapsayıcıları numaralandıramaz.

Tek bir blob için genel erişim düzeyini değiştiremezsiniz. Genel erişim düzeyi yalnızca kapsayıcı düzeyinde ayarlanır.

Kapsayıcının ortak erişim düzeyini ayarlamak için Azure portal veya Azure CLı kullanın. Kapsayıcıyı oluştururken kapsayıcının ortak erişim düzeyini ayarlayabilir veya var olan bir kapsayıcıda bu ayarı güncelleştirebilirsiniz.

# <a name="azure-portal"></a>[Azure Portal](#tab/portal)

Azure portal bir veya daha fazla mevcut kapsayıcının genel erişim düzeyini güncelleştirmek için şu adımları izleyin:

1. Azure portal depolama hesabınıza genel bakış bölümüne gidin.
1. Menü dikey penceresinde **BLOB hizmeti** altında **kapsayıcılar**' ı seçin.
1. Ortak erişim düzeyini ayarlamak istediğiniz kapsayıcıları seçin.
1. Genel erişim ayarlarını göstermek için **erişim düzeyini Değiştir** düğmesini kullanın.
1. **Genel erişim düzeyi** açılan menüsünde istenen genel erişim düzeyini seçin ve değişikliği seçili kapsayıcılara uygulamak için Tamam düğmesine tıklayın.

    ![Portalda genel erişim düzeyinin nasıl ayarlanacağını gösteren ekran görüntüsü](./media/anonymous-read-access-configure/configure-public-access-container.png)

Depolama hesabı için genel erişime izin verilmedikçe, kapsayıcının ortak erişim düzeyi ayarlanamaz. Kapsayıcının ortak erişim düzeyini ayarlamaya çalışırsanız, hesap için genel erişime izin verilmediğinden ayarın devre dışı bırakıldığını görürsünüz.

:::image type="content" source="media/anonymous-read-access-configure/container-public-access-blocked.png" alt-text="Ortak erişime izin verilmediği zaman, Setting kapsayıcısının genel erişim düzeyinin engellendiğini gösteren ekran görüntüsü":::

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Azure CLı ile bir veya daha fazla kapsayıcının genel erişim düzeyini güncelleştirmek için [az Storage Container set Permission](/cli/azure/storage/container#az-storage-container-set-permission) komutunu çağırın. Hesap anahtarınızı, bir bağlantı dizesini veya paylaşılan erişim imzasını (SAS) geçirerek bu işlemi yetkilendirin. Kapsayıcının ortak erişim düzeyini ayarlayan kapsayıcı [ACL 'si](/rest/api/storageservices/set-container-acl) IŞLEMI Azure AD ile yetkilendirmeyi desteklemez. Daha fazla bilgi için bkz. [BLOB ve kuyruk verisi işlemlerini çağırma izinleri](/rest/api/storageservices/authorize-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations).

Aşağıdaki örnek, kapsayıcıya ve bloblarına anonim erişimi etkinleştirmek üzere bir kapsayıcı için genel erişim ayarını ayarlar. Köşeli ayraçlar içindeki yer tutucu değerlerini kendi değerlerinizle değiştirmeyi unutmayın:

```azurecli-interactive
az storage container set-permission \
    --name <container-name> \
    --account-name <account-name> \
    --public-access container \
    --account-key <account-key> \
    --auth-mode key
```

Depolama hesabı için genel erişime izin verilmedikçe, kapsayıcının ortak erişim düzeyi ayarlanamaz. Kapsayıcının ortak erişim düzeyini ayarlamaya çalışırsanız, depolama hesabında genel erişime izin verilmediğini belirten bir hata oluşur.

---

## <a name="check-the-container-public-access-setting"></a>Kapsayıcı genel erişim ayarını denetleyin

Bir veya daha fazla kapsayıcının genel erişim ayarını denetlemek için Azure portal, PowerShell, Azure CLı, Azure depolama istemci kitaplıklarından birini veya Azure depolama kaynak sağlayıcısı 'nı kullanabilirsiniz. Aşağıdaki bölümler birkaç örnek sunmaktadır.  

### <a name="check-the-public-access-setting-for-a-single-container"></a>Tek bir kapsayıcı için genel erişim ayarını denetleyin

Azure CLı ile bir veya daha fazla kapsayıcının genel erişim düzeyini almak için [az Storage Container Show Permission](/cli/azure/storage/container#az-storage-container-show-permission) komutunu çağırın. Hesap anahtarınızı, bir bağlantı dizesini veya paylaşılan erişim imzasını (SAS) geçirerek bu işlemi yetkilendirin. Kapsayıcının ortak erişim düzeyini döndüren [kapsayıcı al ACL 'si](/rest/api/storageservices/get-container-acl) , Azure AD ile yetkilendirmeyi desteklemez. Daha fazla bilgi için bkz. [BLOB ve kuyruk verisi işlemlerini çağırma izinleri](/rest/api/storageservices/authorize-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations).

Aşağıdaki örnek bir kapsayıcı için genel erişim ayarını okur. Köşeli ayraçlar içindeki yer tutucu değerlerini kendi değerlerinizle değiştirmeyi unutmayın:

```azurecli-interactive
az storage container show-permission \
    --name <container-name> \
    --account-name <account-name> \
    --account-key <account-key>
    --auth-mode key
```

### <a name="check-the-public-access-setting-for-a-set-of-containers"></a>Bir kapsayıcı kümesi için genel erişim ayarını denetleyin

Bir veya daha fazla depolama hesabında, kapsayıcıları listeleyerek ve genel erişim ayarını denetleyerek, genel erişim için hangi kapsayıcıların yapılandırıldığını denetlemek mümkündür. Bu yaklaşım, bir depolama hesabı çok sayıda kapsayıcı içermediği ya da ayarı az sayıda depolama hesabı üzerinde denetlerken pratik bir seçenektir. Ancak, çok sayıda kapsayıcıyı numaralandırmaya çalışırsanız performans düşebilir.

Aşağıdaki örnek, bir depolama hesabındaki tüm kapsayıcılar için genel erişim ayarını almak üzere PowerShell kullanır. Köşeli ayraçlar içindeki yer tutucu değerlerini kendi değerlerinizle değiştirmeyi unutmayın:

```powershell
$rgName = "<resource-group>"
$accountName = "<storage-account>"

$storageAccount = Get-AzStorageAccount -ResourceGroupName $rgName -Name $accountName
$ctx = $storageAccount.Context

Get-AzStorageContainer -Context $ctx | Select Name, PublicAccess
```

## <a name="next-steps"></a>Sonraki adımlar

- [Kapsayıcılara ve bloblara anonim genel okuma erişimini engelleyin](anonymous-read-access-prevent.md)
- [.NET ile ortak kapsayıcılara ve bloblara anonim erişim](anonymous-read-access-client.md)
- [Azure depolama 'ya erişimi yetkilendirme](../common/storage-auth.md)
