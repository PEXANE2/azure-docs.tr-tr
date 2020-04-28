---
title: Özel kaynak ara sunucu başvurusu
description: Azure özel kaynak sağlayıcıları için özel kaynak proxy başvurusu. Bu makale, proxy özel kaynaklarını uygulayan uç noktalara yönelik gereksinimlere geçer.
ms.topic: conceptual
ms.author: jobreen
author: jjbfour
ms.date: 06/20/2019
ms.openlocfilehash: 46b38686b39836f3d4bfb80686d514f932a79bf3
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "75650467"
---
# <a name="custom-resource-proxy-reference"></a>Özel kaynak proxy başvurusu

Bu makale, proxy özel kaynaklarını uygulayan uç noktalara yönelik gereksinimlere geçer. Azure özel kaynak sağlayıcıları hakkında bilginiz yoksa bkz. [özel kaynak sağlayıcılarına genel bakış](overview.md).

## <a name="how-to-define-a-proxy-resource-endpoint"></a>Proxy kaynağı uç noktası tanımlama

"Proxy" olarak **Routingtype** belirtilerek bir proxy kaynağı oluşturulabilir.

Örnek özel kaynak sağlayıcısı:

```JSON
{
  "properties": {
    "resourceTypes": [
      {
        "name": "myCustomResources",
        "routingType": "Proxy",
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

## <a name="building-proxy-resource-endpoint"></a>Proxy kaynak uç noktası derleniyor

"Proxy" kaynak **uç noktası** uygulayan bir **uç nokta** , Azure 'daki yeni API için isteği ve yanıtı işlemelidir. Bu durumda, **ResourceType** , `PUT`, ve için `GET`yeni bir Azure Kaynak API 'si oluşturur ve tek `DELETE` bir kaynakta `GET` CRUD gerçekleştirir ve tüm mevcut kaynakları alır.

> [!NOTE]
> `id`, `name`Ve `type` alanları gerekli değildir, ancak özel kaynağı mevcut Azure ekosistemiyle tümleştirmeniz gerekir.

Örnek kaynak:

``` JSON
{
    "name": "{myCustomResourceName}",
    "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/myCustomResources/{myCustomResourceName}",
    "type": "Microsoft.CustomProviders/resourceProviders/myCustomResources",
    "properties": {
        "myProperty1": "myPropertyValue1",
        "myProperty2": {
            "myProperty3" : "myPropertyValue3"
        }
    }
}
```

Parametre başvurusu:

Özellik | Örnek | Açıklama
---|---|---
ad | ' {myCustomResourceName} ' | Özel kaynağın adı.
type | ' Microsoft. CustomProviders/resourceProviders/{resourceTypeName} ' | Kaynak türü ad alanı.
id | '/Subscriptions/{SubscriptionID}/ResourceGroups/{resourcegroupname}/<br>sağlayıcılar/Microsoft. CustomProviders/resourceProviders/{resourceProviderName}/<br>myCustomResources/{myCustomResourceName} ' | Kaynak KIMLIĞI.

### <a name="create-a-custom-resource"></a>Özel bir kaynak oluşturun

Azure API gelen Istek:

``` HTTP
PUT https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resource-provider-name}/myCustomResources/{myCustomResourceName}?api-version=2018-09-01-preview
Authorization: Bearer eyJ0e...
Content-Type: application/json

{
    "properties": {
        "myProperty1": "myPropertyValue1",
        "myProperty2": {
            "myProperty3" : "myPropertyValue3"
        }
    }
}
```

Bu istek daha sonra şu biçimdeki **uç noktaya** iletilir:

``` HTTP
PUT https://{endpointURL}/?api-version=2018-09-01-preview
Content-Type: application/json
X-MS-CustomProviders-RequestPath: /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/myCustomResources/{myCustomResourceName}

{
    "properties": {
        "myProperty1": "myPropertyValue1",
        "myProperty2": {
            "myProperty3" : "myPropertyValue3"
        }
    }
}
```

Benzer şekilde, **uç noktadan** gelen yanıt daha sonra müşteriye iletilir. Uç noktadan gelen yanıt şunu döndürmelidir:

- Geçerli bir JSON nesne belgesi. Tüm diziler ve dizeler üst nesne altında iç içe olmalıdır.
- Üst `Content-Type` bilgi, "Application/JSON; olarak ayarlanmalıdır charset = UTF-8 ".

**Uç nokta** Yanıtıyla

``` HTTP
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8

{
    "name": "{myCustomResourceName}",
    "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/myCustomResources/{myCustomResourceName}",
    "type": "Microsoft.CustomProviders/resourceProviders/myCustomResources",
    "properties": {
        "myProperty1": "myPropertyValue1",
        "myProperty2": {
            "myProperty3" : "myPropertyValue3"
        }
    }
}
```

Azure özel kaynak sağlayıcısı yanıtı:

``` HTTP
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8

{
    "name": "{myCustomResourceName}",
    "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/myCustomResources/{myCustomResourceName}",
    "type": "Microsoft.CustomProviders/resourceProviders/myCustomResources",
    "properties": {
        "myProperty1": "myPropertyValue1",
        "myProperty2": {
            "myProperty3" : "myPropertyValue3"
        }
    }
}
```

### <a name="remove-a-custom-resource"></a>Özel bir kaynağı kaldırma

Azure API gelen Istek:

``` HTTP
Delete https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/myCustomResources/{myCustomResourceName}?api-version=2018-09-01-preview
Authorization: Bearer eyJ0e...
Content-Type: application/json
```

Bu istek daha sonra şu biçimdeki **uç noktaya** iletilir:

``` HTTP
Delete https://{endpointURL}/?api-version=2018-09-01-preview
Content-Type: application/json
X-MS-CustomProviders-RequestPath: /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/myCustomResources/{myCustomResourceName}
```

Benzer şekilde, **bitiş noktasından** gelen yanıt müşteriye geri iletilir. Uç noktadan gelen yanıt şunu döndürmelidir:

- Geçerli JSON nesne belgesi. Tüm diziler ve dizeler üst nesne altında iç içe olmalıdır.
- Üst `Content-Type` bilgi, "Application/JSON; olarak ayarlanmalıdır charset = UTF-8 ".

**Uç nokta** Yanıtıyla

``` HTTP
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
```

Azure özel kaynak sağlayıcısı yanıtı:

``` HTTP
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
```

### <a name="retrieve-a-custom-resource"></a>Özel bir kaynak alma

Azure API gelen Istek:

``` HTTP
GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/myCustomResources/{myCustomResourceName}?api-version=2018-09-01-preview
Authorization: Bearer eyJ0e...
Content-Type: application/json
```

Bu istek daha sonra şu biçimdeki **uç noktaya** iletilir:

``` HTTP
GET https://{endpointURL}/?api-version=2018-09-01-preview
Content-Type: application/json
X-MS-CustomProviders-RequestPath: /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/myCustomResources/{myCustomResourceName}
```

Benzer şekilde, **uç noktadan** gelen yanıt daha sonra müşteriye iletilir. Uç noktadan gelen yanıt şunu döndürmelidir:

- Geçerli bir JSON nesne belgesi. Tüm diziler ve dizeler üst nesne altında iç içe olmalıdır.
- Üst `Content-Type` bilgi, "Application/JSON; olarak ayarlanmalıdır charset = UTF-8 ".

**Uç nokta** Yanıtıyla

``` HTTP
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8

{
    "name": "{myCustomResourceName}",
    "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/myCustomResources/{myCustomResourceName}",
    "type": "Microsoft.CustomProviders/resourceProviders/myCustomResources",
    "properties": {
        "myProperty1": "myPropertyValue1",
        "myProperty2": {
            "myProperty3" : "myPropertyValue3"
        }
    }
}
```

Azure özel kaynak sağlayıcısı yanıtı:

``` HTTP
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8

{
    "name": "{myCustomResourceName}",
    "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/myCustomResources/{myCustomResourceName}",
    "type": "Microsoft.CustomProviders/resourceProviders/myCustomResources",
    "properties": {
        "myProperty1": "myPropertyValue1",
        "myProperty2": {
            "myProperty3" : "myPropertyValue3"
        }
    }
}
```

### <a name="enumerate-all-custom-resources"></a>Tüm özel kaynakları listeleme

Azure API gelen Istek:

``` HTTP
GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/myCustomResources?api-version=2018-09-01-preview
Authorization: Bearer eyJ0e...
Content-Type: application/json
```

Bu istek daha sonra şu biçimdeki **uç noktaya** iletilir:

``` HTTP
GET https://{endpointURL}/?api-version=2018-09-01-preview
Content-Type: application/json
X-MS-CustomProviders-RequestPath: /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/myCustomResources
```

Benzer şekilde, **uç noktadan** gelen yanıt daha sonra müşteriye iletilir. Uç noktadan gelen yanıt şunu döndürmelidir:

- Geçerli bir JSON nesne belgesi. Tüm diziler ve dizeler üst nesne altında iç içe olmalıdır.
- Üst `Content-Type` bilgi, "Application/JSON; olarak ayarlanmalıdır charset = UTF-8 ".
- Kaynak listesi, en üst düzey `value` özelliğin altına yerleştirilmelidir.

**Uç nokta** Yanıtıyla

``` HTTP
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8

{
    "value" : [
        {
            "name": "{myCustomResourceName}",
            "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/myCustomResources/{myCustomResourceName}",
            "type": "Microsoft.CustomProviders/resourceProviders/myCustomResources",
            "properties": {
                "myProperty1": "myPropertyValue1",
                "myProperty2": {
                    "myProperty3" : "myPropertyValue3"
                }
            }
        }
    ]
}
```

Azure özel kaynak sağlayıcısı yanıtı:

``` HTTP
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8

{
    "value" : [
        {
            "name": "{myCustomResourceName}",
            "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/myCustomResources/{myCustomResourceName}",
            "type": "Microsoft.CustomProviders/resourceProviders/myCustomResources",
            "properties": {
                "myProperty1": "myPropertyValue1",
                "myProperty2": {
                    "myProperty3" : "myPropertyValue3"
                }
            }
        }
    ]
}
```

## <a name="next-steps"></a>Sonraki adımlar

- [Azure özel kaynak sağlayıcılarına genel bakış](overview.md)
- [Hızlı başlangıç: Azure özel kaynak sağlayıcısı oluşturma ve özel kaynaklar dağıtma](./create-custom-provider.md)
- [Öğretici: Azure 'da özel eylemler ve kaynaklar oluşturma](./tutorial-get-started-with-custom-providers.md)
- [Nasıl yapılır: Azure REST API özel eylemler ekleme](./custom-providers-action-endpoint-how-to.md)
- [Başvuru: özel kaynak önbellek başvurusu](proxy-cache-resource-endpoint-reference.md)
