---
title: Kapsayıcılar ve BLOB 'lar için anonim genel okuma erişimini yapılandırma
titleSuffix: Azure Storage
description: Depolama hesabı için blob verilerine anonim erişimi etkinleştirmeyi veya devre dışı bırakmayı öğrenin. Kapsayıcıları ve Blobları anonim erişim için kullanılabilir hale getirmek için kapsayıcı genel erişim ayarını ayarlayın.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 06/29/2020
ms.author: tamram
ms.reviewer: fryu
ms.openlocfilehash: af589874021baaf04a423b7bbaa0e36528eda93c
ms.sourcegitcommit: 3541c9cae8a12bdf457f1383e3557eb85a9b3187
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2020
ms.locfileid: "86209940"
---
# <a name="configure-anonymous-public-read-access-for-containers-and-blobs"></a>Kapsayıcılar ve BLOB 'lar için anonim genel okuma erişimini yapılandırma

Azure depolama, kapsayıcılar ve BLOB 'lar için anonim genel okuma erişimini destekler. Varsayılan olarak, bir kapsayıcıya ve bloblarına yapılan tüm isteklere Azure Active Directory (Azure AD) ya da paylaşılan anahtar yetkilendirmesi kullanılarak yetki verilmelidir. Anonim erişime izin vermek için bir kapsayıcının ortak erişim düzeyi ayarını yapılandırdığınızda, istemciler bu kapsayıcıdaki verileri, isteği yetkilendirmeden okuyabilir.

> [!WARNING]
> Bir kapsayıcı genel erişim için yapılandırıldığında, herhangi bir istemci o kapsayıcıdaki verileri okuyabilir. Genel erişim olası bir güvenlik riski sunar, bu nedenle senaryonuz bunu gerektirmiyorsa, Microsoft bunu depolama hesabı için devre dışı bırakmanızı önerir. Daha fazla bilgi için bkz. [kapsayıcılar ve bloblara anonim genel okuma erişimini engelleme](anonymous-read-access-prevent.md).

Bir kapsayıcı için genel erişimi yapılandırmak üzere iki adım gerçekleştirmeniz gerekir:

1. Depolama hesabı için genel erişimi etkinleştir
1. Kapsayıcının ortak erişim ayarını yapılandırın

Bu makalede bir kapsayıcı ve Blobları için anonim genel okuma erişiminin nasıl yapılandırılacağı açıklanır. Blob verilerine bir istemci uygulamasından anonim olarak erişme hakkında daha fazla bilgi için bkz. [.NET ile anonim kapsayıcıları ve Blobları anonim olarak erişim](anonymous-read-access-client.md).

## <a name="enable-or-disable-public-read-access-for-a-storage-account"></a>Depolama hesabı için genel okuma erişimini etkinleştirme veya devre dışı bırakma

Varsayılan olarak, bir depolama hesabı için genel erişim etkindir. Genel erişimin devre dışı bırakılması, bu hesaptaki kapsayıcılara ve bloblara anonim erişimi engeller. Microsoft, daha iyi güvenlik için, senaryolarınız kullanıcıların blob kaynaklarına anonim olarak erişmelerini gerektirmediği takdirde depolama hesaplarınız için genel erişimi devre dışı bırakmanızı önerir.

> [!WARNING]
> Bir depolama hesabı için genel erişimin devre dışı bırakılması, bu depolama hesabındaki tüm kapsayıcılar için genel erişim ayarlarını geçersiz kılar. Depolama hesabı için genel erişim devre dışı bırakıldığında, bu hesaba yönelik ileride yapılacak anonim istekler başarısız olur.

Bir depolama hesabı için genel erişimi etkinleştirmek veya devre dışı bırakmak için, hesabın **Blobpublicaccess** özelliğini yapılandırmak üzere Azure Portal veya Azure CLI kullanın. Bu özellik, Azure Resource Manager dağıtım modeliyle oluşturulan tüm depolama hesapları için kullanılabilir. Daha fazla bilgi için bkz. [depolama hesabına genel bakış](../common/storage-account-overview.md).

# <a name="azure-portal"></a>[Azure Portal](#tab/portal)

Azure portal bir depolama hesabı için genel erişimi etkinleştirmek veya devre dışı bırakmak için şu adımları izleyin:

1. Azure portalda depolama hesabınıza gidin.
1. **Ayarlar**' ın altındaki **yapılandırma** ayarını bulun.
1. **BLOB genel erişimini** **devre dışı** veya **etkin**olarak ayarlayın.

    :::image type="content" source="media/anonymous-read-access-configure/blob-public-access-portal.png" alt-text="Hesap için blob genel erişiminin nasıl etkinleştirileceğini veya devre dışı bırakılacağını gösteren ekran görüntüsü":::

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Azure CLı ile bir depolama hesabı için genel erişimi etkinleştirmek veya devre dışı bırakmak için, önce [az Resource Show](/cli/azure/resource#az-resource-show) komutunu çağırarak depolama HESABıNıZıN kaynak kimliğini alın. Daha sonra, depolama hesabı için **Allowblobpublicaccess** özelliğini ayarlamak üzere [az Resource Update](/cli/azure/resource#az-resource-update) komutunu çağırın. Genel erişimi etkinleştirmek için **Allowblobpublicaccess** özelliğini true olarak ayarlayın; devre dışı bırakmak için **false**olarak ayarlayın.

Aşağıdaki örnek, depolama hesabı için genel blob erişimini devre dışı bırakır. Köşeli ayraçlar içindeki yer tutucu değerlerini kendi değerlerinizle değiştirmeyi unutmayın:

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

Azure CLı ile genel erişimin etkinleştirilip etkinleştirilmeyeceğini denetlemek için, **Allowblobpublicaccess** özelliği için [az Resource Show](/cli/azure/resource#az-resource-show) komutunu ve sorguyu çağırın:

```azurecli-interactive
az resource show \
    --name <storage-account> \
    --resource-group <resource-group> \
    --resource-type Microsoft.Storage/storageAccounts \
    --query properties.allowBlobPublicAccess \
    --output tsv
```

---

> [!NOTE]
> Bir depolama hesabı için genel erişimin devre dışı bırakılması, bu depolama hesabında barındırılan herhangi bir statik Web sitesini etkilemez. **$Web** kapsayıcı her zaman herkese açık olarak erişilebilir.

## <a name="check-the-public-access-setting-for-a-storage-account"></a>Bir depolama hesabı için genel erişim ayarını denetleyin

Bir depolama hesabının genel erişim ayarını denetlemek için **Allowblobpublicaccess** özelliğinin değerini alın. Tek seferde büyük bir depolama hesabı için bu özelliği denetlemek üzere Azure Kaynak Grafiği gezginini kullanın.

**Allowblobpublicaccess** özelliği varsayılan olarak ayarlı değildir ve açıkça ayarlanana kadar bir değer döndürmez. Depolama hesabı, özellik değeri null olduğunda genel erişime izin vermek için varsayılan değer olarak kullanılır.

### <a name="check-the-public-access-setting-for-a-single-storage-account"></a>Tek bir depolama hesabı için genel erişim ayarını denetleyin

Azure CLı kullanarak tek bir depolama hesabının genel erişim ayarını denetlemek için [az Resource Show](/cli/azure/resource#az-resource-show) komutunu ve **Allowblobpublicaccess** özelliği için sorguyu çağırın:

```azurecli-interactive
az resource show \
    --name <storage-account> \
    --resource-group <resource-group> \
    --resource-type Microsoft.Storage/storageAccounts \
    --query properties.allowBlobPublicAccess \
    --output tsv
```

### <a name="check-the-public-access-setting-for-a-set-of-storage-accounts"></a>Bir depolama hesapları kümesi için genel erişim ayarını denetleyin

En iyi performansa sahip bir dizi depolama hesabı genelinde genel erişim ayarını denetlemek için Azure portal Azure Kaynak Grafiği Gezginini kullanabilirsiniz. Kaynak Grafiği Gezginini kullanma hakkında daha fazla bilgi edinmek için bkz. [hızlı başlangıç: Azure Kaynak Grafiği gezginini kullanarak Ilk kaynak grafik sorgunuzu çalıştırma](/azure/governance/resource-graph/first-query-portal).

Kaynak Graph Explorer 'da aşağıdaki sorguyu çalıştırmak, depolama hesaplarının bir listesini döndürür ve her bir hesap için **Allowblobpublicaccess** özelliğinin değerini görüntüler:

```kusto
resources
| where type =~ 'Microsoft.Storage/storageAccounts'
| extend allowBlobPublicAccess = parse_json(properties).allowBlobPublicAccess
| project subscriptionId, resourceGroup, name, allowBlobPublicAccess
| order by subscriptionId, resourceGroup, name asc
```

## <a name="set-the-public-access-level-for-a-container"></a>Bir kapsayıcı için genel erişim düzeyini ayarlama

Anonim kullanıcılara bir kapsayıcıya ve bloblarına okuma erişimi sağlamak için, önce depolama hesabı için genel erişimi etkinleştirin, sonra kapsayıcının ortak erişim düzeyini ayarlayın. Depolama hesabı için genel erişim devre dışıysa, bir kapsayıcı için genel erişimi yapılandırameyeceksiniz.

Bir depolama hesabı için ortak erişim etkinleştirildiğinde, aşağıdaki izinlerle bir kapsayıcı yapılandırabilirsiniz:

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

Depolama hesabı için ortak erişim devre dışı bırakıldığında, kapsayıcının ortak erişim düzeyi ayarlanamaz. Kapsayıcının ortak erişim düzeyini ayarlamaya çalışırsanız, hesap için genel erişim yasaklanmış olduğundan ayarın devre dışı bırakıldığını görürsünüz.

:::image type="content" source="media/anonymous-read-access-configure/container-public-access-blocked.png" alt-text="Ortak erişim devre dışı bırakıldığında kapsayıcının genel erişim düzeyinin engellendiğini gösteren ekran görüntüsü":::

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

Depolama hesabı için ortak erişim devre dışı bırakıldığında, kapsayıcının ortak erişim düzeyi ayarlanamaz. Kapsayıcının ortak erişim düzeyini ayarlamaya çalışırsanız, depolama hesabında genel erişime izin verilmediğini belirten bir hata oluşur.

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