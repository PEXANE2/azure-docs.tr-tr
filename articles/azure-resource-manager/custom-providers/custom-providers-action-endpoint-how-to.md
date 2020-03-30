---
title: Azure REST API'sine özel eylemler ekleme
description: Azure REST API'sine nasıl özel eylemler ekleyeceğinizi öğrenin. Bu makalede, özel eylemleri uygulamak isteyen uç noktalar için gereksinimler ve en iyi uygulamalar üzerinden yürüyeceğiz.
ms.topic: conceptual
ms.author: jobreen
author: jjbfour
ms.date: 06/20/2019
ms.openlocfilehash: 6110a7952b7c29609d2b98e135b61032aec3fa52
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75650402"
---
# <a name="adding-custom-actions-to-azure-rest-api"></a>Azure REST API'sine Özel Eylemler Ekleme

Bu makale, özel eylemler uygulayan Azure Özel Kaynak Sağlayıcısı uç noktalarını oluşturmak için gereken gereksinimleri ve en iyi uygulamaları gözden geçirir. Azure Özel Kaynak Sağlayıcıları'nı bilmiyorsanız, [özel kaynak sağlayıcılarına genel bakışa](overview.md)bakın.

## <a name="how-to-define-an-action-endpoint"></a>Eylem Bitiş Noktası nasıl tanımlanır?

**Bitiş noktası,** bir hizmete işaret eden ve azure ile arasındaki temel sözleşmeyi uygulayan bir URL'dir. Bitiş noktası özel kaynak sağlayıcısında tanımlanır ve herkese açık herhangi bir URL olabilir. Aşağıdaki `myCustomAction` örnekte . **action** `endpointURL`

Örnek **Kaynak Sağlayıcı:**

```JSON
{
  "properties": {
    "actions": [
      {
        "name": "myCustomAction",
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

## <a name="building-an-action-endpoint"></a>Eylem bitiş noktası oluşturma

Bir **eylemi** uygulayan bir **bitiş noktası,** Azure'daki yeni API isteği ve yanıtı işlemelidir. **Eylemi** olan özel bir kaynak sağlayıcısı oluşturulduğunda, Azure'da yeni bir API kümesi oluşturur. Bu durumda, eylem aramalar için `POST` yeni bir Azure eylem API'si oluşturur:

``` JSON
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/myCustomAction
```

Azure API Gelen İstek:

``` HTTP
POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/myCustomAction?api-version=2018-09-01-preview
Authorization: Bearer eyJ0e...
Content-Type: application/json

{
    "myProperty1": "myPropertyValue1",
    "myProperty2": {
        "myProperty3" : "myPropertyValue3"
    }
}
```

Bu istek daha sonra formdaki **bitiş noktasına** iletilir:

``` HTTP
POST https://{endpointURL}/?api-version=2018-09-01-preview
Content-Type: application/json
X-MS-CustomProviders-RequestPath: /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/myCustomAction

{
    "myProperty1": "myPropertyValue1",
    "myProperty2": {
        "myProperty3" : "myPropertyValue3"
    }
}
```

Benzer şekilde, bitiş **noktasından** gelen yanıt daha sonra müşteriye geri iletilir. Bitiş noktasından gelen yanıt döndürülmelidir:

- Geçerli bir JSON nesne belgesi. Tüm diziler ve dizeleri bir üst nesnenin altında iç içe olmalıdır.
- Üstbilgi `Content-Type` "uygulama/json; charset=utf-8".

``` HTTP
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8

{
    "myProperty1": "myPropertyValue1",
    "myProperty2": {
        "myProperty3" : "myPropertyValue3"
    }
}
```

Azure Özel Kaynak Sağlayıcısı Yanıtı:

``` HTTP
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8

{
    "myProperty1": "myPropertyValue1",
    "myProperty2": {
        "myProperty3" : "myPropertyValue3"
    }
}
```

## <a name="calling-a-custom-action"></a>Özel Eylem Arama

Özel bir kaynak sağlayıcısının özel eylemini çağırmanın iki ana yolu vardır:

- Azure CLI
- Azure Kaynak Yöneticisi Şablonları

### <a name="azure-cli"></a>Azure CLI

```azurecli-interactive
az resource invoke-action --action {actionName} \
                          --ids /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName} \
                          --request-body \
                            '{
                                "myProperty1": "myPropertyValue1",
                                "myProperty2": {
                                    "myProperty3": "myPropertyValue3"
                                }
                            }'
```

Parametre | Gerekli | Açıklama
---|---|---
action | *Evet* | **Kaynak Sağlayıcısı'nda**tanımlanan eylemin adı.
Kimlik | *Evet* | **Kaynak Sağlayıcısı'nın**kaynak kimliği.
istek-gövde | *hayır* | **Bitiş noktasına**gönderilecek istek gövdesi.

### <a name="azure-resource-manager-template"></a>Azure Resource Manager Şablonu

> [!NOTE]
> Eylemler, Azure Kaynak Yöneticisi Şablonları'nda sınırlı destek gösterir. Eylemin bir şablon içinde çağrılabilmesi için, kendi [`list`](../templates/template-functions-resource.md#list) adında önek içermesi gerekir.

Liste Eylemi ile Örnek **Kaynak Sağlayıcı:**

```JSON
{
  "properties": {
    "actions": [
      {
        "name": "listMyCustomAction",
        "routingType": "Proxy",
        "endpoint": "https://{endpointURL}/"
      }
    ]
  },
  "location": "eastus"
}
```

Örnek Azure Kaynak Yöneticisi Şablonu:

``` JSON
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "variables": {
        "resourceIdentifier": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}",
        "apiVersion": "2018-09-01-preview",
        "functionValues": {
            "myProperty1": "myPropertyValue1",
            "myProperty2": {
                "myProperty3": "myPropertyValue3"
            }
        }
    },
    "resources": [],
    "outputs": {
        "myCustomActionOutput": {
            "type": "object",
            "value": "[listMyCustomAction(variables('resourceIdentifier'), variables('apiVersion'), variables('functionValues'))]"
        }
    }
}
```

Parametre | Gerekli | Açıklama
---|---|---
resourceIdentifier | *Evet* | **Kaynak Sağlayıcısı'nın**kaynak kimliği.
apiVersion | *Evet* | Kaynak çalışma zamanının API sürümü. Bu her zaman "2018-09-01-önizleme" olmalıdır.
fonksiyonDeğerleri | *hayır* | **Bitiş noktasına**gönderilecek istek gövdesi.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Özel Kaynak Sağlayıcılarına Genel Bakış](overview.md)
- [Hızlı başlangıç: Azure Özel Kaynak Sağlayıcısı oluşturun ve özel kaynakları dağıtın](./create-custom-provider.md)
- [Öğretici: Azure'da özel eylemler ve kaynaklar oluşturun](./tutorial-get-started-with-custom-providers.md)
- [Nasıl Yapilir: Azure REST API'sine Özel Kaynak Ekleme](./custom-providers-resources-endpoint-how-to.md)
