---
title: Azure REST API özel kaynakları ekleme
description: Azure REST API özel kaynakları nasıl ekleyeceğinizi öğrenin. Bu makalede, özel kaynakları uygulamak isteyen uç noktalar için gereksinimler ve en iyi uygulamalar açıklanmaktadır.
ms.topic: conceptual
ms.author: jobreen
author: jjbfour
ms.date: 06/20/2019
ms.openlocfilehash: b6c5f5b8e437ad2dc2e8a3be3f3f2ed03a613b44
ms.sourcegitcommit: f788bc6bc524516f186386376ca6651ce80f334d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/03/2020
ms.locfileid: "75650532"
---
# <a name="adding-custom-resources-to-azure-rest-api"></a>Azure REST API özel kaynakları ekleme

Bu makale, özel kaynakları uygulayan Azure özel kaynak sağlayıcısı uç noktaları oluşturmaya yönelik gereksinimlere ve en iyi uygulamalara gider. Azure özel kaynak sağlayıcıları hakkında bilginiz yoksa bkz. [özel kaynak sağlayıcılarına genel bakış](overview.md).

## <a name="how-to-define-a-resource-endpoint"></a>Kaynak uç noktası tanımlama

**Uç nokta** , BT ile Azure arasında temel alınan sözleşmeyi uygulayan bir hizmete işaret eden bir URL 'dir. Uç nokta özel kaynak sağlayıcısında tanımlanmıştır ve herkese açık olarak erişilebilen herhangi bir URL olabilir. Aşağıdaki örnekte, `endpointURL`tarafından uygulanan `myCustomResource` adlı bir **ResourceType** vardır.

Örnek **ResourceProvider**:

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

## <a name="building-a-resource-endpoint"></a>Kaynak uç noktası oluşturma

**ResourceType** uygulayan bir **uç nokta** , Azure 'daki yeni API için isteği ve yanıtı işlemelidir. Bir **ResourceType** ile özel bir kaynak sağlayıcısı oluşturulduğunda, Azure 'da yeni bir API kümesi oluşturur. Bu durumda **ResourceType** , `PUT`, `GET`ve `DELETE` için yeni bir Azure Kaynak API 'si oluşturacak ve tek bir kaynakta CRUD 'yi gerçekleştirecek ve tüm mevcut kaynakları almak için `GET`.

Tek kaynak (`PUT`, `GET`ve `DELETE`) işleme:

``` JSON
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/myCustomResource/{myCustomResourceName}
```

Tüm kaynakları al (`GET`):

``` JSON
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/myCustomResource
```

Özel kaynaklar için özel kaynak sağlayıcıları iki tür **routingtypes**sunar: "`Proxy`" ve "`Proxy, Cache`".

### <a name="proxy-routing-type"></a>Proxy yönlendirme türü

"`Proxy`" **Routingtype** , tüm istek yöntemlerini özel kaynak sağlayıcısında belirtilen **uç noktaya** göre proxy 'leri. "`Proxy`" ne zaman kullanılır:

- Yanıt üzerinde tam denetim gereklidir.
- Sistemleri mevcut kaynaklarla tümleştirme.

"`Proxy`" kaynakları hakkında daha fazla bilgi edinmek için bkz [. özel kaynak proxy başvurusu](proxy-resource-endpoint-reference.md)

### <a name="proxy-cache-routing-type"></a>Proxy önbelleği yönlendirme türü

"`Proxy, Cache`" **Routingtype** proxy 'leri yalnızca özel kaynak sağlayıcısında belirtilen **uç noktaya** `PUT` ve `DELETE` istek yöntemleri. Özel kaynak sağlayıcısı, önbelleğinde ne depolandığını temel alarak `GET` isteklerini otomatik olarak döndürür. Özel bir kaynak önbellek ile işaretlenmişse, özel kaynak sağlayıcısı, API 'Leri Azure ile uyumlu hale getirmek için yanıttaki alanları da ekler/üzerine yazar. "`Proxy, Cache`" ne zaman kullanılır:

- Mevcut kaynağı olmayan yeni bir sistem oluşturuluyor.
- Mevcut Azure ekosistemiyle çalışın.

"`Proxy, Cache`" kaynakları hakkında daha fazla bilgi edinmek için bkz [. özel kaynak önbelleği başvurusu](proxy-cache-resource-endpoint-reference.md)

## <a name="creating-a-custom-resource"></a>Özel kaynak oluşturma

Özel kaynak sağlayıcısından özel bir kaynak oluşturmanın iki ana yolu vardır:

- Azure CLI
- Azure Resource Manager şablonları

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

Parametre | Gereklidir | Açıklama
---|---|---
-Full-Object | *Yes* | Özellikler nesnesinin location, tags, SKU ve/veya plan gibi diğer seçenekleri içerdiğini belirtir.
id | *Yes* | Özel kaynağın kaynak KIMLIĞI. Bu, **ResourceProvider** 'ın kapalı olması gerekir
properties | *Yes* | **Uç noktaya**gönderilecek istek gövdesi.

Azure özel kaynağını silme:

```azurecli-interactive
az resource delete --id /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/{resourceTypeName}/{customResourceName}
```

Parametre | Gereklidir | Açıklama
---|---|---
id | *Yes* | Özel kaynağın kaynak KIMLIĞI. Bu, **ResourceProvider**'ın kapalı olması gerekir.

Azure özel kaynağını alma:

```azurecli-interactive
az resource show --id /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/{resourceTypeName}/{customResourceName}
```

Parametre | Gereklidir | Açıklama
---|---|---
id | *Yes* | Özel kaynağın kaynak KIMLIĞI. Bu, **ResourceProvider** 'ın kapalı olması gerekir

### <a name="azure-resource-manager-template"></a>Azure Resource Manager Şablonu

> [!NOTE]
> Kaynaklar, yanıtın **uç noktadan**uygun bir `id`, `name`ve `type` içermesini gerektirir.

Azure Resource Manager şablonlar `id`, `name`ve `type` aşağı akış uç noktasından doğru bir şekilde verilmesini gerektirir. Döndürülen bir kaynak yanıtı şu biçimde olmalıdır:

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

Örnek Azure Resource Manager şablonu:

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

Parametre | Gereklidir | Açıklama
---|---|---
resourceTypeName | *Yes* | Özel sağlayıcıda tanımlanan **ResourceType** öğesinin **adı** .
resourceProviderName | *Yes* | Özel kaynak sağlayıcısı örnek adı.
customResourceName | *Yes* | Özel kaynak adı.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure özel kaynak sağlayıcılarına genel bakış](overview.md)
- [Hızlı başlangıç: Azure özel kaynak sağlayıcısı oluşturma ve özel kaynaklar dağıtma](./create-custom-provider.md)
- [Öğretici: Azure 'da özel eylemler ve kaynaklar oluşturma](./tutorial-get-started-with-custom-providers.md)
- [Nasıl yapılır: Azure REST API özel eylemler ekleme](./custom-providers-action-endpoint-how-to.md)
- [Başvuru: özel kaynak proxy başvurusu](proxy-resource-endpoint-reference.md)
- [Başvuru: özel kaynak önbellek başvurusu](proxy-cache-resource-endpoint-reference.md)
