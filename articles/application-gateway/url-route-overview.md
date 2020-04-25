---
title: Azure Application Gateway URL tabanlı içerik yönlendirmeye genel bakış
description: Bu makalede, Azure Application Gateway URL tabanlı içerik yönlendirme, UrlPathMap yapılandırması ve PathBasedRouting kuralına genel bakış sunulmaktadır.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.date: 09/10/2019
ms.author: victorh
ms.topic: conceptual
ms.openlocfilehash: 1d393055b0ac62198bd5a7239b2b92b7aeff62e5
ms.sourcegitcommit: f7fb9e7867798f46c80fe052b5ee73b9151b0e0b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/24/2020
ms.locfileid: "82145362"
---
# <a name="url-path-based-routing-overview"></a>URL Yolu Tabanlı Yönlendirmeye genel bakış

URL Yolu Tabanlı Yönlendirme, trafiği isteğin URL Yollarına göre arka uç sunucu havuzlarına yönlendirmenizi sağlar. 

Senaryolardan biri, farklı içerik türleri için istekleri farklı arka uç sunucu havuzlarına yönlendirmektir.

Aşağıdaki örnekte, Application Gateway contoso.com için VideoServerPool, ImageServerPool ve DefaultServerPool gibi üç arka uç sunucu havuzlarından trafik sunmaktadır.

![imageURLroute](./media/application-gateway-url-route-overview/figure1.png)

Http\://contoso.com/video/* Için Istekler VideoServerPool 'a yönlendirilir ve http\://contoso.com/images/*, ımageserverpool 'a yönlendirilir. Yol desenlerinden hiçbiri eşleşmiyorsa DefaultServerPool seçilir.

> [!IMPORTANT]
> V1 SKU 'SU için kurallar portalda listelendikleri sırada işlenir. Temel dinleyici listede ilk sıradaysa ve gelen bir istekle eşleşiyorsa, o dinleyici tarafından işlenir. V2 SKU 'SU için, tam eşleşmeler daha yüksek önceliğe sahiptir. Ancak, temel bir dinleyiciyi yapılandırmadan önce çok siteli dinleyicileri yapılandırmak kesinlikle önerilir. Bu işlem, trafiğin doğru arka uca yönlendirilmesini güvence altına alır.

## <a name="urlpathmap-configuration-element"></a>UrlPathMap yapılandırma öğesi

UrlPathMap öğesi, arka uç sunucu havuzu eşlemeleri için Yol desenleri belirtmek üzere kullanılır. Aşağıdaki kod örneği, şablon dosyasındaki urlPathMap öğesinin kod parçacığıdır.

```json
"urlPathMaps": [{
    "name": "{urlpathMapName}",
    "id": "/subscriptions/{subscriptionId}/../microsoft.network/applicationGateways/{gatewayName}/urlPathMaps/{urlpathMapName}",
    "properties": {
        "defaultBackendAddressPool": {
            "id": "/subscriptions/    {subscriptionId}/../microsoft.network/applicationGateways/{gatewayName}/backendAddressPools/{poolName1}"
        },
        "defaultBackendHttpSettings": {
            "id": "/subscriptions/{subscriptionId}/../microsoft.network/applicationGateways/{gatewayName}/backendHttpSettingsList/{settingname1}"
        },
        "pathRules": [{
            "name": "{pathRuleName}",
            "properties": {
                "paths": [
                    "{pathPattern}"
                ],
                "backendAddressPool": {
                    "id": "/subscriptions/{subscriptionId}/../microsoft.network/applicationGateways/{gatewayName}/backendAddressPools/{poolName2}"
                },
                "backendHttpsettings": {
                    "id": "/subscriptions/{subscriptionId}/../microsoft.network/applicationGateways/{gatewayName}/backendHttpsettingsList/{settingName2}"
                }
            }
        }]
    }
}]
```

### <a name="pathpattern"></a>Pathmodel

Pathmodel eşleştirilecek yol desenlerinin bir listesidir. Her biri / ile başlamalıdır. "*" işareti, yalnızca "/" işaretinin ardından en sona koyulabilir. Yol eşleştirici öğesine besleyici dize, ilk öğesinden sonra herhangi bir metin içermez mi? veya # ve bu karakter burada kullanılamaz. Aksi halde, bir URL 'de izin verilen karakterlere Pathmodel içinde izin verilir.

Desteklenen desenler Application Gateway v1 veya v2 dağıtdığınıza bağlıdır:

#### <a name="v1"></a>v1

Yol kuralları büyük/küçük harfe duyarlıdır.

|V1 yol kalıbı  |Destekleniyor mu?  |
|---------|---------|
|`/images/*`     |evet|
|`/images*`     |hayır|
|`/images/*.jpg`     |hayır|
|`/*.jpg`     |hayır|
|`/Repos/*/Comments/*`     |hayır|
|`/CurrentUser/Comments/*`     |evet|

#### <a name="v2"></a>v2

Yol kuralları büyük/küçük harfe duyarlıdır.

|v2 yol deseninin  |Destekleniyor mu?  |
|---------|---------|
|`/images/*`     |evet|
|`/images*`     |evet|
|`/images/*.jpg`     |hayır|
|`/*.jpg`     |hayır|
|`/Repos/*/Comments/*`     |hayır|
|`/CurrentUser/Comments/*`     |evet|

Daha fazla bilgi için [URL tabanlı yönlendirme kullanan bir Resource Manager şablonunu](https://azure.microsoft.com/documentation/templates/201-application-gateway-url-path-based-routing) inceleyebilirsiniz.

## <a name="pathbasedrouting-rule"></a>PathBasedRouting kuralı

PathBasedRouting türündeki RequestRoutingRule, bir dinleyiciyi urlPathMap’e bağlamak için kullanılır. Bu dinleyici için alınan tüm istekler, urlPathMap’te belirtilen ilkeye göre yönlendirilir.
PathBasedRouting kuralının kod parçacığı:

```json
"requestRoutingRules": [
    {

"name": "{ruleName}",
"id": "/subscriptions/{subscriptionId}/../microsoft.network/applicationGateways/{gatewayName}/requestRoutingRules/{ruleName}",
"properties": {
    "ruleType": "PathBasedRouting",
    "httpListener": {
        "id": "/subscriptions/{subscriptionId}/../microsoft.network/applicationGateways/{gatewayName}/httpListeners/<listenerName>"
    },
    "urlPathMap": {
        "id": "/subscriptions/{subscriptionId}/../microsoft.network/applicationGateways/{gatewayName}/urlPathMaps/{urlpathMapName}"
    }

}
    }
]
```

## <a name="next-steps"></a>Sonraki adımlar

URL tabanlı içerik yönlendirme hakkında bilgi edindikten sonra, URL yönlendirme kurallarıyla bir uygulama ağ geçidi oluşturmak için [URL tabanlı yönlendirme kullanan uygulama ağ geçidi oluşturma](create-url-route-portal.md) bölümüne gidin.
