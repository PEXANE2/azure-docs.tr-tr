---
title: Azure REST API özel eylemler ekleme
description: Azure REST API özel eylemlerin nasıl ekleneceğini öğrenin. Bu makale, özel eylemler uygulamak isteyen uç noktalar için gereksinimler ve en iyi uygulamalar hakkında yol gösterir.
ms.topic: conceptual
ms.author: jobreen
author: jjbfour
ms.date: 06/20/2019
ms.openlocfilehash: 6110a7952b7c29609d2b98e135b61032aec3fa52
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "75650402"
---
# <a name="adding-custom-actions-to-azure-rest-api"></a>Azure REST API özel eylemler ekleme

Bu makalede, özel eylemleri uygulayan Azure özel kaynak sağlayıcısı uç noktaları oluşturmak için gereksinimler ve en iyi yöntemler sunulacaktır. Azure özel kaynak sağlayıcıları hakkında bilginiz yoksa bkz. [özel kaynak sağlayıcılarına genel bakış](overview.md).

## <a name="how-to-define-an-action-endpoint"></a>Eylem uç noktası tanımlama

**Uç nokta** , BT ile Azure arasında temel alınan sözleşmeyi uygulayan bir hizmete işaret eden bir URL 'dir. Uç nokta özel kaynak sağlayıcısında tanımlanmıştır ve herkese açık olarak erişilebilen herhangi bir URL olabilir. Aşağıdaki örnekte, tarafından uygulanan adlı bir **eylem** vardır `myCustomAction` `endpointURL` .

Örnek **ResourceProvider**:

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

## <a name="building-an-action-endpoint"></a>Eylem uç noktası oluşturma

Bir **eylemi** uygulayan bir **uç noktanın** , Azure 'daki yeni API için isteği ve yanıtı işlemesi gerekir. Bir **eylem** içeren özel bir kaynak sağlayıcısı oluşturulduğunda, Azure 'da yeni bir API kümesi oluşturur. Bu durumda, eylem çağrılar için yeni bir Azure eylem API 'SI oluşturacaktır `POST` :

``` JSON
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/myCustomAction
```

Azure API gelen Istek:

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

Bu istek daha sonra şu biçimdeki **uç noktaya** iletilir:

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

Benzer şekilde, **uç noktadan** gelen yanıt daha sonra müşteriye iletilir. Uç noktadan gelen yanıt şunu döndürmelidir:

- Geçerli bir JSON nesne belgesi. Tüm diziler ve dizeler üst nesne altında iç içe olmalıdır.
- `Content-Type`Üst bilgi, "Application/JSON; olarak ayarlanmalıdır charset = UTF-8 ".

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

Azure özel kaynak sağlayıcısı yanıtı:

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

## <a name="calling-a-custom-action"></a>Özel bir eylem çağırma

Özel bir eylemi özel bir kaynak sağlayıcısında çağırmanın iki ana yöntemi vardır:

- Azure CLI
- Azure Resource Manager şablonları

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
action | *Yes* | **ResourceProvider**'da tanımlanan eylemin adı.
ayrılacak | *Yes* | **ResourceProvider**'ıN kaynak kimliği.
İstek gövdesi | *eşleşen* | **Uç noktaya**gönderilecek istek gövdesi.

### <a name="azure-resource-manager-template"></a>Azure Resource Manager Şablonu

> [!NOTE]
> Eylemlerin Azure Resource Manager şablonlarda sınırlı desteği vardır. Eylemin bir şablon içinde çağrılması için, [`list`](../templates/template-functions-resource.md#list) adının öneki içermesi gerekir.

Liste eylemiyle örnek **ResourceProvider** :

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

Örnek Azure Resource Manager şablonu:

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
ResourceIdentifier kimlikli | *Yes* | **ResourceProvider**'ıN kaynak kimliği.
apiVersion | *Yes* | Kaynak çalışma zamanının API sürümü. Bu her zaman "2018-09-01-Önizleme" olmalıdır.
functionValues | *eşleşen* | **Uç noktaya**gönderilecek istek gövdesi.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure özel kaynak sağlayıcılarına genel bakış](overview.md)
- [Hızlı başlangıç: Azure özel kaynak sağlayıcısı oluşturma ve özel kaynaklar dağıtma](./create-custom-provider.md)
- [Öğretici: Azure 'da özel eylemler ve kaynaklar oluşturma](./tutorial-get-started-with-custom-providers.md)
- [Nasıl yapılır: Azure REST API özel kaynak ekleme](./custom-providers-resources-endpoint-how-to.md)
