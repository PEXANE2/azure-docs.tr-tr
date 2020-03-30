---
title: Kaynak sağlayıcıları ve kaynak türleri
description: Kaynak Yöneticisi'ni destekleyen kaynak sağlayıcılar, şemaları ve kullanılabilir API sürümleri ve kaynakları barındırabilecek bölgeleri açıklar.
ms.topic: conceptual
ms.date: 08/29/2019
ms.openlocfilehash: 82b8251006a1a2d4edd198eca843489d3720f8d6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79273988"
---
# <a name="azure-resource-providers-and-types"></a>Azure kaynak sağlayıcıları ve türleri

Kaynakları dağıtırken, sık sık kaynak sağlayıcıları ve türleri hakkında bilgi almanız gerekir. Örneğin, anahtarları ve parolaları saklamak isterseniz Microsoft.KeyVault kaynak sağlayıcısı ile çalışırsınız. Bu kaynak sağlayıcısı, anahtar kasasını oluşturmak için vaults adlı bir kaynak türü sağlar.

Kaynak türü adı şu biçimdedir: **{kaynak-sağlayıcısı}/{kaynak-türü}**. Anahtar kasasının kaynak türü **Microsoft.KeyVault/vaults'dur.**

Bu makalede şunları öğreneceksiniz:

* Azure'daki tüm kaynak sağlayıcılarını görüntüleme
* Kaynak sağlayıcısının kayıt durumunu denetleme
* Kaynak sağlayıcısını kaydetme
* Kaynak sağlayıcısı için kaynak türlerini görüntüleme
* Kaynak türü için geçerli konumları görüntüleme
* Kaynak türü için geçerli API sürümlerini görüntüleme

Bu adımları Azure portalı, Azure PowerShell veya Azure CLI üzerinden yapabilirsiniz.

Kaynak sağlayıcılarını Azure hizmetleriyle eşleyen bir liste için Azure [hizmetleri için Kaynak sağlayıcılarına](azure-services-resource-providers.md)bakın.

## <a name="azure-portal"></a>Azure portalında

Tüm kaynak sağlayıcılarını ve aboneliğinizin kayıt durumunu görmek için:

1. [Azure portalında](https://portal.azure.com)oturum açın.
2. Azure portalı menüsünde **Tüm hizmetler**’i seçin.

    ![abonelikleri seçin](./media/resource-providers-and-types/select-all-services.png)

3. Tüm **hizmetler** kutusuna, **abonelik**girin ve ardından **Abonelikler'i**seçin.
4. Görüntülemek için abonelik listesinden aboneliği seçin.
5. **Kaynak sağlayıcılar** seçin ve kullanılabilir kaynak sağlayıcılarılistesini görüntüleyin.

    ![kaynak sağlayıcılarını göster](./media/resource-providers-and-types/show-resource-providers.png)

6. Bir kaynak sağlayıcısı nın kaydedilmesi, aboneliğinizi kaynak sağlayıcısıyla çalışacak şekilde yapılandırır. Kayıt kapsamı her zaman aboneliktir. Varsayılan olarak, birçok kaynak sağlayıcısı otomatik olarak kaydedilir. Ancak, bazı kaynak sağlayıcılarını el ile kaydetmeniz gerekebilir. Bir kaynak sağlayıcısı nı kaydetmek için kaynak `/register/action` sağlayıcısının çalışmasını yapmak için izniniz olması gerekir. Bu işlem, Katkıda Bulunan ve Sahip rolleriyle birlikte sunulur. Kaynak sağlayıcısını kaydetmek için **Kaydol'u**seçin. Önceki ekran görüntüsünde, **Kayıt** bağlantısı **Microsoft.Blueprint**için vurgulanır.

    Aboneliğinizde bu kaynak sağlayıcısından kaynak türleri hala varken bir kaynak sağlayıcısının kaydını kınıtamam.

Belirli bir kaynak sağlayıcısının bilgilerini görmek için:

1. [Azure portalında](https://portal.azure.com)oturum açın.
2. Azure portalı menüsünde **Tüm hizmetler**’i seçin.
3. Tüm **hizmetler** kutusuna **kaynak gezgini**girin ve ardından **Kaynak Gezgini'ni**seçin.

    ![tüm hizmetleri seçin](./media/resource-providers-and-types/select-resource-explorer.png)

4. Sağ oku seçerek **Sağlayıcıları** Genişletin.

    ![Sağlayıcıları seçin](./media/resource-providers-and-types/select-providers.png)

5. Görüntülemek istediğiniz bir kaynak sağlayıcısını ve kaynak türünü genişletin.

    ![Kaynak türünü seçin](./media/resource-providers-and-types/select-resource-type.png)

6. Kaynak Yöneticisi tüm bölgelerde desteklenir, ancak dağıttığınız kaynaklar tüm bölgelerde desteklenmeyebilir. Ayrıca, aboneliğinizde kaynağı destekleyen bazı bölgeleri kullanmanızı engelleyen sınırlamalar da olabilir. Kaynak gezgini kaynak türü için geçerli konumlar görüntüler.

    ![Konumları göster](./media/resource-providers-and-types/show-locations.png)

7. API sürümü, kaynak sağlayıcısı tarafından yayımlanan REST API işlemlerinin bir sürümüne karşılık gelir. Bir kaynak sağlayıcısı yeni özellikler etkinleştirir gibi, REST API yeni bir sürümünü yayımlar. Kaynak gezgini, kaynak türü için geçerli API sürümlerini görüntüler.

    ![API sürümlerini göster](./media/resource-providers-and-types/show-api-versions.png)

## <a name="azure-powershell"></a>Azure PowerShell

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Azure'daki tüm kaynak sağlayıcılarını ve aboneliğinizin kayıt durumunu görmek için şunları kullanın:

```azurepowershell-interactive
Get-AzResourceProvider -ListAvailable | Select-Object ProviderNamespace, RegistrationState
```

Hangi benzer sonuçlar döndürür:

```output
ProviderNamespace                RegistrationState
-------------------------------- ------------------
Microsoft.ClassicCompute         Registered
Microsoft.ClassicNetwork         Registered
Microsoft.ClassicStorage         Registered
Microsoft.CognitiveServices      Registered
...
```

Bir kaynak sağlayıcısı nın kaydedilmesi, aboneliğinizi kaynak sağlayıcısıyla çalışacak şekilde yapılandırır. Kayıt kapsamı her zaman aboneliktir. Varsayılan olarak, birçok kaynak sağlayıcısı otomatik olarak kaydedilir. Ancak, bazı kaynak sağlayıcılarını el ile kaydetmeniz gerekebilir. Bir kaynak sağlayıcısı nı kaydetmek için kaynak `/register/action` sağlayıcısının çalışmasını yapmak için izniniz olması gerekir. Bu işlem, Katkıda Bulunan ve Sahip rolleriyle birlikte sunulur.

```azurepowershell-interactive
Register-AzResourceProvider -ProviderNamespace Microsoft.Batch
```

Hangi benzer sonuçlar döndürür:

```output
ProviderNamespace : Microsoft.Batch
RegistrationState : Registering
ResourceTypes     : {batchAccounts, operations, locations, locations/quotas}
Locations         : {West Europe, East US, East US 2, West US...}
```

Aboneliğinizde bu kaynak sağlayıcısından kaynak türleri hala varken bir kaynak sağlayıcısının kaydını kınıtamam.

Belirli bir kaynak sağlayıcısının bilgilerini görmek için şunları kullanın:

```azurepowershell-interactive
Get-AzResourceProvider -ProviderNamespace Microsoft.Batch
```

Hangi benzer sonuçlar döndürür:

```output
{ProviderNamespace : Microsoft.Batch
RegistrationState : Registered
ResourceTypes     : {batchAccounts}
Locations         : {West Europe, East US, East US 2, West US...}

...
```

Kaynak sağlayıcısının kaynak türlerini görmek için şunları kullanın:

```azurepowershell-interactive
(Get-AzResourceProvider -ProviderNamespace Microsoft.Batch).ResourceTypes.ResourceTypeName
```

Hangi döner:

```output
batchAccounts
operations
locations
locations/quotas
```

API sürümü, kaynak sağlayıcısı tarafından yayımlanan REST API işlemlerinin bir sürümüne karşılık gelir. Bir kaynak sağlayıcısı yeni özellikler etkinleştirir gibi, REST API yeni bir sürümünü yayımlar.

Kaynak türü için kullanılabilir API sürümlerini almak için şunları kullanın:

```azurepowershell-interactive
((Get-AzResourceProvider -ProviderNamespace Microsoft.Batch).ResourceTypes | Where-Object ResourceTypeName -eq batchAccounts).ApiVersions
```

Hangi döner:

```output
2017-05-01
2017-01-01
2015-12-01
2015-09-01
2015-07-01
```

Kaynak Yöneticisi tüm bölgelerde desteklenir, ancak dağıttığınız kaynaklar tüm bölgelerde desteklenmeyebilir. Ayrıca, aboneliğinizde kaynağı destekleyen bazı bölgeleri kullanmanızı engelleyen sınırlamalar da olabilir.

Kaynak türü için desteklenen konumları almak için kullanın.

```azurepowershell-interactive
((Get-AzResourceProvider -ProviderNamespace Microsoft.Batch).ResourceTypes | Where-Object ResourceTypeName -eq batchAccounts).Locations
```

Hangi döner:

```output
West Europe
East US
East US 2
West US
...
```

## <a name="azure-cli"></a>Azure CLI

Azure'daki tüm kaynak sağlayıcılarını ve aboneliğinizin kayıt durumunu görmek için şunları kullanın:

```azurecli
az provider list --query "[].{Provider:namespace, Status:registrationState}" --out table
```

Hangi benzer sonuçlar döndürür:

```output
Provider                         Status
-------------------------------- ----------------
Microsoft.ClassicCompute         Registered
Microsoft.ClassicNetwork         Registered
Microsoft.ClassicStorage         Registered
Microsoft.CognitiveServices      Registered
...
```

Bir kaynak sağlayıcısı nın kaydedilmesi, aboneliğinizi kaynak sağlayıcısıyla çalışacak şekilde yapılandırır. Kayıt kapsamı her zaman aboneliktir. Varsayılan olarak, birçok kaynak sağlayıcısı otomatik olarak kaydedilir. Ancak, bazı kaynak sağlayıcılarını el ile kaydetmeniz gerekebilir. Bir kaynak sağlayıcısı nı kaydetmek için kaynak `/register/action` sağlayıcısının çalışmasını yapmak için izniniz olması gerekir. Bu işlem, Katkıda Bulunan ve Sahip rolleriyle birlikte sunulur.

```azurecli
az provider register --namespace Microsoft.Batch
```

Bu da kaydın devam ettiğimesajını döndürür.

Aboneliğinizde bu kaynak sağlayıcısından kaynak türleri hala varken bir kaynak sağlayıcısının kaydını kınıtamam.

Belirli bir kaynak sağlayıcısının bilgilerini görmek için şunları kullanın:

```azurecli
az provider show --namespace Microsoft.Batch
```

Hangi benzer sonuçlar döndürür:

```output
{
    "id": "/subscriptions/####-####/providers/Microsoft.Batch",
    "namespace": "Microsoft.Batch",
    "registrationsState": "Registering",
    "resourceTypes:" [
        ...
    ]
}
```

Kaynak sağlayıcısının kaynak türlerini görmek için şunları kullanın:

```azurecli
az provider show --namespace Microsoft.Batch --query "resourceTypes[*].resourceType" --out table
```

Hangi döner:

```output
Result
---------------
batchAccounts
operations
locations
locations/quotas
```

API sürümü, kaynak sağlayıcısı tarafından yayımlanan REST API işlemlerinin bir sürümüne karşılık gelir. Bir kaynak sağlayıcısı yeni özellikler etkinleştirir gibi, REST API yeni bir sürümünü yayımlar.

Kaynak türü için kullanılabilir API sürümlerini almak için şunları kullanın:

```azurecli
az provider show --namespace Microsoft.Batch --query "resourceTypes[?resourceType=='batchAccounts'].apiVersions | [0]" --out table
```

Hangi döner:

```output
Result
---------------
2017-05-01
2017-01-01
2015-12-01
2015-09-01
2015-07-01
```

Kaynak Yöneticisi tüm bölgelerde desteklenir, ancak dağıttığınız kaynaklar tüm bölgelerde desteklenmeyebilir. Ayrıca, aboneliğinizde kaynağı destekleyen bazı bölgeleri kullanmanızı engelleyen sınırlamalar da olabilir.

Kaynak türü için desteklenen konumları almak için kullanın.

```azurecli
az provider show --namespace Microsoft.Batch --query "resourceTypes[?resourceType=='batchAccounts'].locations | [0]" --out table
```

Hangi döner:

```output
Result
---------------
West Europe
East US
East US 2
West US
...
```

## <a name="next-steps"></a>Sonraki adımlar

* Kaynak Yöneticisi şablonları oluşturma hakkında bilgi edinmek için [bkz.](../templates/template-syntax.md) 
* Kaynak sağlayıcı şablon şemalarını görüntülemek için [Şablon başvurusuna](/azure/templates/)bakın.
* Kaynak sağlayıcılarını Azure hizmetleriyle eşleyen bir liste için Azure [hizmetleri için Kaynak sağlayıcılarına](azure-services-resource-providers.md)bakın.
* Bir kaynak sağlayıcısının işlemlerini görüntülemek için [Azure REST API'sine](/rest/api/)bakın.
