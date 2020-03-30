---
title: Azure REST API'sine özel kaynak ekleme
description: Azure REST API'sine özel kaynakları nasıl ekleyeceğinizi öğrenin. Bu makalede, özel kaynakları uygulamak isteyen uç noktalar için gereksinimler ve en iyi uygulamalar üzerinden yürüyeceğiz.
ms.topic: conceptual
ms.author: jobreen
author: jjbfour
ms.date: 06/20/2019
ms.openlocfilehash: b6c5f5b8e437ad2dc2e8a3be3f3f2ed03a613b44
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75650532"
---
# <a name="adding-custom-resources-to-azure-rest-api"></a>Azure REST API'sine Özel Kaynak Ekleme

Bu makale, özel kaynakları uygulayan Azure Özel Kaynak Sağlayıcısı uç noktalarını oluşturmak için gereken gereksinimleri ve en iyi uygulamaları gözden geçirecektir. Azure Özel Kaynak Sağlayıcıları'nı bilmiyorsanız, [özel kaynak sağlayıcılarına genel bakışa](overview.md)bakın.

## <a name="how-to-define-a-resource-endpoint"></a>Kaynak bitiş noktası nasıl tanımlanır?

**Bitiş noktası,** bir hizmete işaret eden ve azure ile arasındaki temel sözleşmeyi uygulayan bir URL'dir. Bitiş noktası özel kaynak sağlayıcısında tanımlanır ve herkese açık herhangi bir URL olabilir. Aşağıdaki `myCustomResource` örnekte . **resourceType** `endpointURL`

Örnek **Kaynak Sağlayıcı:**

```JSON
{
  "properties": {
    "resourceTypes": [
      {
        "name": "myCustomResource",
        "routingType": "Proxy, Cache",
        "endpoint": "https://{endpointURL}/"
      }
    ]
  },
  "location": "eastus",
  "type": "Microsoft.CustomProviders/resourceProviders",
  "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}",
  "name": "{resourceProviderName}"
}
```

## <a name="building-a-resource-endpoint"></a>Kaynak bitiş noktası oluşturma

**Bir kaynak Türü** uygulayan bir bitiş **noktası,** Azure'daki yeni API isteği ve yanıtı işlemelidir. **Kaynak Türüne sahip** özel bir kaynak sağlayıcısı oluşturulduğunda, Azure'da yeni bir API kümesi oluşturur. Bu durumda, **kaynak Türü,** `PUT`tek `GET` `DELETE` bir kaynakta CRUD gerçekleştirmenin yanı sıra `GET` varolan tüm kaynakları almak için yeni bir Azure kaynak API'si oluşturur:

Tek Kaynak`PUT`(, `GET`, `DELETE`) ve:

``` JSON
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/myCustomResource/{myCustomResourceName}
```

Tüm Kaynakları`GET`Al ( ):

``` JSON
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/myCustomResource
```

Özel kaynaklar için, özel kaynak sağlayıcıları iki tür **yönlendirme Türleri**sunar: "`Proxy`ve "`Proxy, Cache`".

### <a name="proxy-routing-type"></a>proxy yönlendirme türü

"`Proxy`" **routingType,** tüm istek yöntemlerini özel kaynak sağlayıcısında belirtilen **bitiş noktasına** iliştirer. Ne zaman`Proxy`kullanılır " ":

- Yanıt üzerinde tam kontrol gereklidir.
- Sistemleri varolan kaynaklarla tümleştirme.

" " "`Proxy`kaynakları hakkında daha fazla bilgi edinmek için, [özel kaynak proxy başvurusuna](proxy-resource-endpoint-reference.md) bakın

### <a name="proxy-cache-routing-type"></a>proxy önbellek yönlendirme türü

"`Proxy, Cache`" **routingType** yalnızca `PUT` proxy'ler ve `DELETE` özel kaynak sağlayıcısında belirtilen bitiş **noktasına** yöntemleri isteyin. Özel kaynak sağlayıcısı, önbelleğinde depolayabildiği isteklere bağlı olarak istekleri otomatik olarak döndürecektir. `GET` Özel bir kaynak önbellekle işaretlenmişse, özel kaynak sağlayıcısı, API'leri Azure uyumlu hale getirmek için yanıta alan ekler/ üzerine yazar. Ne zaman`Proxy, Cache`kullanılır " ":

- Varolan kaynakları olmayan yeni bir sistem oluşturma.
- Mevcut Azure ekosistemiyle çalışın.

" " "`Proxy, Cache`kaynakları hakkında daha fazla bilgi edinmek için, [özel kaynak önbelleği başvurusuna](proxy-cache-resource-endpoint-reference.md) bakın

## <a name="creating-a-custom-resource"></a>Özel kaynak oluşturma

Özel bir kaynak sağlayıcısından özel kaynak oluşturmanın iki ana yolu vardır:

- Azure CLI
- Azure Kaynak Yöneticisi Şablonları

### <a name="azure-cli"></a>Azure CLI

Özel bir kaynak oluşturun:

```azurecli-interactive
az resource create --is-full-object \
                   --id /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/{resourceTypeName}/{customResourceName} \
                   --properties \
                    '{
                        "location": "eastus",
                        "properties": {
                            "myProperty1": "myPropertyValue1",
                            "myProperty2": {
                                "myProperty3": "myPropertyValue3"
                            }
                        }
                    }'
```

Parametre | Gerekli | Açıklama
---|---|---
tam nesne | *Evet* | Özellikleri nesne konum, etiketler, sku ve / veya plan gibi diğer seçenekleri içerdiğini gösterir.
id | *Evet* | Özel kaynağın kaynak kimliği. Bu **Kaynak Sağlayıcı** kapalı var olmalıdır
properties | *Evet* | **Bitiş noktasına**gönderilecek istek gövdesi.

Azure Özel Kaynağını Silme:

```azurecli-interactive
az resource delete --id /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/{resourceTypeName}/{customResourceName}
```

Parametre | Gerekli | Açıklama
---|---|---
id | *Evet* | Özel kaynağın kaynak kimliği. Bu **Kaynak Sağlayıcı**kapalı olmalıdır.

Azure Özel Kaynağı alın:

```azurecli-interactive
az resource show --id /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/{resourceTypeName}/{customResourceName}
```

Parametre | Gerekli | Açıklama
---|---|---
id | *Evet* | Özel kaynağın kaynak kimliği. Bu **Kaynak Sağlayıcı** kapalı var olmalıdır

### <a name="azure-resource-manager-template"></a>Azure Resource Manager Şablonu

> [!NOTE]
> Kaynaklar, yanıtın `id`uygun bir `name`, `type` ve **bitiş noktasından**içermesini gerektirir.

Azure Kaynak Yöneticisi `id`Şablonları, `name` `type` alt akış bitiş noktasından doğru bir şekilde döndürülmenizi gerektirir. Döndürülen kaynak yanıtı formda olmalıdır:

Örnek **uç nokta** yanıtı:

``` JSON
{
  "properties": {
    "myProperty1": "myPropertyValue1",
    "myProperty2": {
        "myProperty3": "myPropertyValue3"
    }
  },
  "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{customResourceName}",
  "name": "{customResourceName}",
  "type": "Microsoft.CustomProviders/resourceProviders/{resourceTypeName}"
}
```

Örnek Azure Kaynak Yöneticisi Şablonu:

```JSON
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
        {
            "type": "Microsoft.CustomProviders/resourceProviders/{resourceTypeName}",
            "name": "{resourceProviderName}/{customResourceName}",
            "apiVersion": "2018-09-01-preview",
            "location": "eastus",
            "properties": {
                "myProperty1": "myPropertyValue1",
                "myProperty2": {
                    "myProperty3": "myPropertyValue3"
                }
            }
        }
    ]
}
```

Parametre | Gerekli | Açıklama
---|---|---
kaynakTypeName | *Evet* | Özel sağlayıcıda tanımlanan **kaynak Türü'nün** **adı.**
kaynakProviderName | *Evet* | Özel kaynak sağlayıcı örnek adı.
özelResourceName | *Evet* | Özel kaynak adı.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Özel Kaynak Sağlayıcılarına Genel Bakış](overview.md)
- [Hızlı başlangıç: Azure Özel Kaynak Sağlayıcısı oluşturun ve özel kaynakları dağıtın](./create-custom-provider.md)
- [Öğretici: Azure'da özel eylemler ve kaynaklar oluşturun](./tutorial-get-started-with-custom-providers.md)
- [Nasıl Olunması: Azure REST API'sine Özel Eylemler Ekleme](./custom-providers-action-endpoint-how-to.md)
- [Başvuru: Özel Kaynak Proxy Başvurusu](proxy-resource-endpoint-reference.md)
- [Başvuru: Özel Kaynak Önbellek Başvurusu](proxy-cache-resource-endpoint-reference.md)
