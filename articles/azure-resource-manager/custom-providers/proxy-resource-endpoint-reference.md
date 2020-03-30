---
title: Özel kaynak ara sunucu başvurusu
description: Azure Özel Kaynak Sağlayıcıları için özel kaynak proxy başvurusu. Bu makalede, proxy özel kaynakları uygulayan uç noktalar için gereksinimleri üzerinden gidecek.
ms.topic: conceptual
ms.author: jobreen
author: jjbfour
ms.date: 06/20/2019
ms.openlocfilehash: 46b38686b39836f3d4bfb80686d514f932a79bf3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75650467"
---
# <a name="custom-resource-proxy-reference"></a>Özel Kaynak Proxy Başvurusu

Bu makalede, proxy özel kaynakları uygulayan uç noktalar için gereksinimleri üzerinden gidecek. Azure Özel Kaynak Sağlayıcıları'nı bilmiyorsanız, [özel kaynak sağlayıcılarına genel bakışa](overview.md)bakın.

## <a name="how-to-define-a-proxy-resource-endpoint"></a>Proxy kaynak bitiş noktası nasıl tanımlanır?

"Proxy" için **yönlendirme Türü** belirterek bir proxy kaynağı oluşturulabilir.

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

## <a name="building-proxy-resource-endpoint"></a>Proxy kaynak bitiş noktası oluşturma

"Proxy" kaynak **bitiş noktası** nı uygulayan bir bitiş **noktası,** Azure'daki yeni API isteği ve yanıtı işlemelidir. Bu durumda, **kaynak Türü,** `PUT`tek `GET` `DELETE` bir kaynakta CRUD gerçekleştirmenin yanı sıra `GET` varolan tüm kaynakları almak için yeni bir Azure kaynak API'si oluşturur.

> [!NOTE]
> , `id` `name`ve `type` alanlar gerekli değildir, ancak özel kaynağı varolan Azure ekosistemiyle tümleştirmek için gereklidir.

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
ad | '{myCustomResourceName}' | Özel kaynağın adı.
type | 'Microsoft.CustomProviders/resourceProviders/{resourceTypeName}' | Kaynak türü ad alanı.
id | '/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/<br>sağlayıcılar/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/<br>myCustomResources/{myCustomResourceName}' | Kaynak kimliği.

### <a name="create-a-custom-resource"></a>Özel kaynak oluşturma

Azure API Gelen İstek:

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

Bu istek daha sonra formdaki **bitiş noktasına** iletilir:

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

Benzer şekilde, bitiş **noktasından** gelen yanıt daha sonra müşteriye geri iletilir. Bitiş noktasından gelen yanıt döndürülmelidir:

- Geçerli bir JSON nesne belgesi. Tüm diziler ve dizeleri bir üst nesnenin altında iç içe olmalıdır.
- Üstbilgi `Content-Type` "uygulama/json; charset=utf-8".

**Bitiş Noktası** Yanıt:

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

Azure Özel Kaynak Sağlayıcısı Yanıtı:

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

Azure API Gelen İstek:

``` HTTP
Delete https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/myCustomResources/{myCustomResourceName}?api-version=2018-09-01-preview
Authorization: Bearer eyJ0e...
Content-Type: application/json
```

Bu istek daha sonra formdaki **bitiş noktasına** iletilir:

``` HTTP
Delete https://{endpointURL}/?api-version=2018-09-01-preview
Content-Type: application/json
X-MS-CustomProviders-RequestPath: /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/myCustomResources/{myCustomResourceName}
```

Benzer şekilde bitiş **noktasından** gelen yanıt daha sonra müşteriye geri iletilir. Bitiş noktasından gelen yanıt döndürülmelidir:

- Geçerli JSON nesne belgesi. Tüm diziler ve dizeleri bir üst nesnenin altında iç içe olmalıdır.
- Üstbilgi `Content-Type` "uygulama/json; charset=utf-8".

**Bitiş Noktası** Yanıt:

``` HTTP
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
```

Azure Özel Kaynak Sağlayıcısı Yanıtı:

``` HTTP
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
```

### <a name="retrieve-a-custom-resource"></a>Özel bir kaynak alma

Azure API Gelen İstek:

``` HTTP
GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/myCustomResources/{myCustomResourceName}?api-version=2018-09-01-preview
Authorization: Bearer eyJ0e...
Content-Type: application/json
```

Bu istek daha sonra formdaki **bitiş noktasına** iletilir:

``` HTTP
GET https://{endpointURL}/?api-version=2018-09-01-preview
Content-Type: application/json
X-MS-CustomProviders-RequestPath: /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/myCustomResources/{myCustomResourceName}
```

Benzer şekilde, bitiş **noktasından** gelen yanıt daha sonra müşteriye geri iletilir. Bitiş noktasından gelen yanıt döndürülmelidir:

- Geçerli bir JSON nesne belgesi. Tüm diziler ve dizeleri bir üst nesnenin altında iç içe olmalıdır.
- Üstbilgi `Content-Type` "uygulama/json; charset=utf-8".

**Bitiş Noktası** Yanıt:

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

Azure Özel Kaynak Sağlayıcısı Yanıtı:

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

### <a name="enumerate-all-custom-resources"></a>Tüm özel kaynakları sayısallandırma

Azure API Gelen İstek:

``` HTTP
GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/myCustomResources?api-version=2018-09-01-preview
Authorization: Bearer eyJ0e...
Content-Type: application/json
```

Bu istek daha sonra formdaki **bitiş noktasına** iletilir:

``` HTTP
GET https://{endpointURL}/?api-version=2018-09-01-preview
Content-Type: application/json
X-MS-CustomProviders-RequestPath: /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/myCustomResources
```

Benzer şekilde, bitiş **noktasından** gelen yanıt daha sonra müşteriye geri iletilir. Bitiş noktasından gelen yanıt döndürülmelidir:

- Geçerli bir JSON nesne belgesi. Tüm diziler ve dizeleri bir üst nesnenin altında iç içe olmalıdır.
- Üstbilgi `Content-Type` "uygulama/json; charset=utf-8".
- Kaynak listesi üst düzey `value` özelliğin altına yerleştirilmelidir.

**Bitiş Noktası** Yanıt:

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

Azure Özel Kaynak Sağlayıcısı Yanıtı:

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

- [Azure Özel Kaynak Sağlayıcılarına Genel Bakış](overview.md)
- [Hızlı başlangıç: Azure Özel Kaynak Sağlayıcısı oluşturun ve özel kaynakları dağıtın](./create-custom-provider.md)
- [Öğretici: Azure'da özel eylemler ve kaynaklar oluşturun](./tutorial-get-started-with-custom-providers.md)
- [Nasıl Olunması: Azure REST API'sine Özel Eylemler Ekleme](./custom-providers-action-endpoint-how-to.md)
- [Başvuru: Özel Kaynak Önbellek Başvurusu](proxy-cache-resource-endpoint-reference.md)
