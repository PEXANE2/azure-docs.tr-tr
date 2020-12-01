---
title: Kaynak sağlayıcıları ve kaynak türleri
description: Azure Resource Manager destekleyen kaynak sağlayıcılarını açıklar. Şemaları, kullanılabilir API sürümlerini ve kaynakları barındırabildiğiniz bölgeleri açıklar.
ms.topic: conceptual
ms.date: 11/30/2020
ms.custom: devx-track-azurecli
ms.openlocfilehash: a8adbce80d5e8f9ee9df2050d8f43363cbf57dc3
ms.sourcegitcommit: 9eda79ea41c60d58a4ceab63d424d6866b38b82d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/30/2020
ms.locfileid: "96352108"
---
# <a name="azure-resource-providers-and-types"></a>Azure kaynak sağlayıcıları ve türleri

Kaynakları dağıttığınızda, genellikle kaynak sağlayıcıları ve türleri hakkında bilgi almanız gerekir. Örneğin, anahtarları ve parolaları saklamak isterseniz Microsoft.KeyVault kaynak sağlayıcısı ile çalışırsınız. Bu kaynak sağlayıcısı, anahtar kasasını oluşturmak için vaults adlı bir kaynak türü sağlar.

Kaynak türü adı şu biçimdedir: **{kaynak-sağlayıcısı}/{kaynak-türü}**. Bir Anahtar Kasası için kaynak türü **Microsoft. keykasası/kasadır**.

Bu makalede şunları öğreneceksiniz:

* Azure 'daki tüm kaynak sağlayıcılarını görüntüleme
* Kaynak sağlayıcısının kayıt durumunu denetle
* Kaynak sağlayıcısını kaydetme
* Kaynak sağlayıcısı için kaynak türlerini görüntüleme
* Kaynak türü için geçerli konumları görüntüleme
* Kaynak türü için geçerli API sürümlerini görüntüleme

Bu adımları Azure portal, Azure PowerShell veya Azure CLı aracılığıyla yapabilirsiniz.

Kaynak sağlayıcılarını Azure hizmetleriyle eşleyen bir liste için bkz. [Azure hizmetleri Için kaynak sağlayıcıları](azure-services-resource-providers.md).

## <a name="register-resource-provider"></a>Kaynak sağlayıcısını kaydetme

Kaynak sağlayıcısını kullanmadan önce, Azure aboneliğinizin kaynak sağlayıcısı için kayıtlı olması gerekir. Kayıt, aboneliğinizi kaynak sağlayıcısıyla çalışacak şekilde yapılandırır. Bazı kaynak sağlayıcıları varsayılan olarak kaydedilir. Diğer kaynak sağlayıcıları, belirli eylemleri gerçekleştirdiğinizde otomatik olarak kaydedilir. Örneğin, Portal üzerinden bir kaynak oluşturduğunuzda, kaynak sağlayıcısı genellikle sizin için kaydedilir. Diğer senaryolarda, bir kaynak sağlayıcısını el ile kaydetmeniz gerekebilir.

Bu makalede, bir kaynak sağlayıcısının kayıt durumunu denetleme ve gerektiğinde kaydetme işlemlerinin nasıl yapılacağı gösterilir. Kaynak sağlayıcısı için işlem yapmak için izninizin olması gerekir `/register/action` . İzin katkıda bulunan ve sahip rollerine dahildir.

> [!IMPORTANT]
> Yalnızca bir kaynak sağlayıcısını kullanmaya hazırsanız kaydolun. Kayıt adımı, aboneliğinizde en az ayrıcalıkları korumanıza olanak sağlar. Kötü amaçlı bir Kullanıcı kayıtlı olmayan kaynak sağlayıcılarını kullanamaz.

Uygulama kodunuz, **kayıt** durumunda olan bir kaynak sağlayıcısı için kaynak oluşturulmasını engellemez. Kaynak sağlayıcısını kaydettiğinizde, her desteklenen bölge için işlem ayrı ayrı yapılır. Bir bölgede kaynak oluşturmak için kaydın yalnızca o bölgede tamamlanması gerekir. Kayıt durumundaki kaynak sağlayıcısını engellemeden, uygulamanız tüm bölgelerin tamamlanmasını beklerken çok daha erken devam edebilir.

Aboneliğinizde kaynak sağlayıcınızdan hala kaynak türleri varsa, bir kaynak sağlayıcısının kaydını silemezsiniz.

## <a name="azure-portal"></a>Azure portal

Tüm kaynak sağlayıcılarını ve aboneliğiniz için kayıt durumunu görmek için:

1. [Azure portalında](https://portal.azure.com) oturum açın.
2. Azure portalı menüsünde **Tüm hizmetler**’i seçin.

    ![abonelikleri Seç](./media/resource-providers-and-types/select-all-services.png)

3. **Tüm hizmetler** kutusunda, **abonelik** girin ve ardından **abonelikler**' i seçin.
4. Görüntülenecek abonelik listesinden aboneliği seçin.
5. **Kaynak sağlayıcıları** ' nı seçin ve kullanılabilir kaynak sağlayıcılarının listesini görüntüleyin.

    ![Kaynak sağlayıcılarını göster](./media/resource-providers-and-types/show-resource-providers.png)

6. Bir kaynak sağlayıcısını kaydetmek için **Kaydet**' i seçin. Önceki ekran görüntüsünde, **kayıt** bağlantısı **Microsoft. Blueprint** için vurgulanır. Aboneliğinizde en düşük ayrıcalıklara sahip olmak için, yalnızca kullanmaya hazırsınız kaynak sağlayıcılarını kaydedin.

Belirli bir kaynak sağlayıcısına ilişkin bilgileri görmek için:

1. [Azure portalında](https://portal.azure.com) oturum açın.
2. Azure portalı menüsünde **Tüm hizmetler**’i seçin.
3. **Tüm hizmetler** kutusunda, **Kaynak Gezgini**' ni girin ve ardından **Kaynak Gezgini**' yi seçin.

    ![Tüm hizmetleri seçin](./media/resource-providers-and-types/select-resource-explorer.png)

4. Sağ oku seçerek **sağlayıcılar** ' ı genişletin.

    ![Sağlayıcıları seçin](./media/resource-providers-and-types/select-providers.png)

5. Görüntülemek istediğiniz kaynak sağlayıcısını ve kaynak türünü genişletin.

    ![Kaynak türünü seçin](./media/resource-providers-and-types/select-resource-type.png)

6. Tüm bölgelerde Kaynak Yöneticisi desteklenir, ancak dağıttığınız kaynaklar tüm bölgelerde desteklenmeyebilir. Ayrıca, aboneliğiniz üzerinde kaynağı destekleyen bazı bölgeler kullanmanızı önleyen sınırlamalar olabilir. Kaynak Gezgini, kaynak türü için geçerli konumlar görüntüler.

    ![Konumları göster](./media/resource-providers-and-types/show-locations.png)

7. API sürümü, kaynak sağlayıcısı tarafından yayınlanan REST API işlemlerinin bir sürümüne karşılık gelir. Bir kaynak sağlayıcısı yeni özellikleri sağladığından, REST API yeni bir sürümünü yayınlar. Kaynak Gezgini, kaynak türü için geçerli API sürümlerini görüntüler.

    ![API sürümlerini göster](./media/resource-providers-and-types/show-api-versions.png)

## <a name="azure-powershell"></a>Azure PowerShell

Azure 'daki tüm kaynak sağlayıcılarını ve aboneliğinizin kayıt durumunu görmek için şunu kullanın:

```azurepowershell-interactive
Get-AzResourceProvider -ListAvailable | Select-Object ProviderNamespace, RegistrationState
```

Şuna benzer sonuçlar döndüren:

```output
ProviderNamespace                RegistrationState
-------------------------------- ------------------
Microsoft.ClassicCompute         Registered
Microsoft.ClassicNetwork         Registered
Microsoft.ClassicStorage         Registered
Microsoft.CognitiveServices      Registered
...
```

Aboneliğiniz için tüm kayıtlı kaynak sağlayıcılarını görmek için şunu kullanın:

```azurepowershell-interactive
 Get-AzResourceProvider -ListAvailable | Where-Object RegistrationState -eq "Registered" | Select-Object ProviderNamespace, RegistrationState | Sort-Object ProviderNamespace
```

Aboneliğinizde en düşük ayrıcalıklara sahip olmak için, yalnızca kullanmaya hazırsınız kaynak sağlayıcılarını kaydedin. Bir kaynak sağlayıcısını kaydetmek için şunu kullanın:

```azurepowershell-interactive
Register-AzResourceProvider -ProviderNamespace Microsoft.Batch
```

Şuna benzer sonuçlar döndüren:

```output
ProviderNamespace : Microsoft.Batch
RegistrationState : Registering
ResourceTypes     : {batchAccounts, operations, locations, locations/quotas}
Locations         : {West Europe, East US, East US 2, West US...}
```

Belirli bir kaynak sağlayıcısına ilişkin bilgileri görmek için şunu kullanın:

```azurepowershell-interactive
Get-AzResourceProvider -ProviderNamespace Microsoft.Batch
```

Şuna benzer sonuçlar döndüren:

```output
{ProviderNamespace : Microsoft.Batch
RegistrationState : Registered
ResourceTypes     : {batchAccounts}
Locations         : {West Europe, East US, East US 2, West US...}

...
```

Bir kaynak sağlayıcısının kaynak türlerini görmek için şunu kullanın:

```azurepowershell-interactive
(Get-AzResourceProvider -ProviderNamespace Microsoft.Batch).ResourceTypes.ResourceTypeName
```

Şunu döndürür:

```output
batchAccounts
operations
locations
locations/quotas
```

API sürümü, kaynak sağlayıcısı tarafından yayınlanan REST API işlemlerinin bir sürümüne karşılık gelir. Bir kaynak sağlayıcısı yeni özellikleri sağladığından, REST API yeni bir sürümünü yayınlar.

Bir kaynak türü için kullanılabilir API sürümlerini almak için şunu kullanın:

```azurepowershell-interactive
((Get-AzResourceProvider -ProviderNamespace Microsoft.Batch).ResourceTypes | Where-Object ResourceTypeName -eq batchAccounts).ApiVersions
```

Şunu döndürür:

```output
2017-05-01
2017-01-01
2015-12-01
2015-09-01
2015-07-01
```

Tüm bölgelerde Kaynak Yöneticisi desteklenir, ancak dağıttığınız kaynaklar tüm bölgelerde desteklenmeyebilir. Ayrıca, aboneliğiniz üzerinde kaynağı destekleyen bazı bölgeler kullanmanızı önleyen sınırlamalar olabilir.

Kaynak türü için desteklenen konumları almak için kullanın.

```azurepowershell-interactive
((Get-AzResourceProvider -ProviderNamespace Microsoft.Batch).ResourceTypes | Where-Object ResourceTypeName -eq batchAccounts).Locations
```

Şunu döndürür:

```output
West Europe
East US
East US 2
West US
...
```

## <a name="azure-cli"></a>Azure CLI

Azure 'daki tüm kaynak sağlayıcılarını ve aboneliğinizin kayıt durumunu görmek için şunu kullanın:

```azurecli-interactive
az provider list --query "[].{Provider:namespace, Status:registrationState}" --out table
```

Şuna benzer sonuçlar döndüren:

```output
Provider                         Status
-------------------------------- ----------------
Microsoft.ClassicCompute         Registered
Microsoft.ClassicNetwork         Registered
Microsoft.ClassicStorage         Registered
Microsoft.CognitiveServices      Registered
...
```

Aboneliğiniz için tüm kayıtlı kaynak sağlayıcılarını görmek için şunu kullanın:

```azurecli-interactive
az provider list --query "sort_by([?registrationState=='Registered'].{Provider:namespace, Status:registrationState}, &Provider)" --out table
```

Aboneliğinizde en düşük ayrıcalıklara sahip olmak için, yalnızca kullanmaya hazırsınız kaynak sağlayıcılarını kaydedin. Bir kaynak sağlayıcısını kaydetmek için şunu kullanın:

```azurecli-interactive
az provider register --namespace Microsoft.Batch
```

Bu, kaydın devam eden bir ileti döndürür.

Belirli bir kaynak sağlayıcısına ilişkin bilgileri görmek için şunu kullanın:

```azurecli-interactive
az provider show --namespace Microsoft.Batch
```

Şuna benzer sonuçlar döndüren:

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

Bir kaynak sağlayıcısının kaynak türlerini görmek için şunu kullanın:

```azurecli-interactive
az provider show --namespace Microsoft.Batch --query "resourceTypes[*].resourceType" --out table
```

Şunu döndürür:

```output
Result
---------------
batchAccounts
operations
locations
locations/quotas
```

API sürümü, kaynak sağlayıcısı tarafından yayınlanan REST API işlemlerinin bir sürümüne karşılık gelir. Bir kaynak sağlayıcısı yeni özellikleri sağladığından, REST API yeni bir sürümünü yayınlar.

Bir kaynak türü için kullanılabilir API sürümlerini almak için şunu kullanın:

```azurecli-interactive
az provider show --namespace Microsoft.Batch --query "resourceTypes[?resourceType=='batchAccounts'].apiVersions | [0]" --out table
```

Şunu döndürür:

```output
Result
---------------
2017-05-01
2017-01-01
2015-12-01
2015-09-01
2015-07-01
```

Tüm bölgelerde Kaynak Yöneticisi desteklenir, ancak dağıttığınız kaynaklar tüm bölgelerde desteklenmeyebilir. Ayrıca, aboneliğiniz üzerinde kaynağı destekleyen bazı bölgeler kullanmanızı önleyen sınırlamalar olabilir.

Kaynak türü için desteklenen konumları almak için kullanın.

```azurecli-interactive
az provider show --namespace Microsoft.Batch --query "resourceTypes[?resourceType=='batchAccounts'].locations | [0]" --out table
```

Şunu döndürür:

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

* Kaynak Yöneticisi şablonları oluşturma hakkında bilgi edinmek için bkz. [yazma Azure Resource Manager şablonları](../templates/template-syntax.md). 
* Kaynak sağlayıcısı şablon şemalarını görüntülemek için bkz. [şablon başvurusu](/azure/templates/).
* Kaynak sağlayıcılarını Azure hizmetleriyle eşleyen bir liste için bkz. [Azure hizmetleri Için kaynak sağlayıcıları](azure-services-resource-providers.md).
* Bir kaynak sağlayıcısına yönelik işlemleri görüntülemek için bkz. [Azure REST API](/rest/api/).
