---
title: Azure Application Gateway URL tabanlı içerik yönlendirmeye genel bakış
description: Bu makalede, Azure Uygulama Ağ Geçidi URL tabanlı içerik yönlendirme, UrlPathMap yapılandırması ve PathBasedRouting kuralına genel bir bakış sağlanmaktadır.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.date: 09/10/2019
ms.author: victorh
ms.topic: conceptual
ms.openlocfilehash: e20acb131b1a091fef858dab34705f4a8d3b4c4a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77251847"
---
# <a name="url-path-based-routing-overview"></a>URL Yolu Tabanlı Yönlendirmeye genel bakış

URL Yolu Tabanlı Yönlendirme, trafiği isteğin URL Yollarına göre arka uç sunucu havuzlarına yönlendirmenizi sağlar. 

Senaryolardan biri, farklı içerik türleri için istekleri farklı arka uç sunucu havuzlarına yönlendirmektir.

Aşağıdaki örnekte, Application Gateway contoso.com için VideoServerPool, ImageServerPool ve DefaultServerPool gibi üç arka uç sunucu havuzlarından trafik sunmaktadır.

![imageURLroute](./media/application-gateway-url-route-overview/figure1.png)

http\://contoso.com/video/* talepleri VideoServerPool'a yönlendirilir\:ve http //contoso.com/images/* ImageServerPool'a yönlendirilir. Yol desenlerinden hiçbiri eşleşmiyorsa DefaultServerPool seçilir.

> [!IMPORTANT]
> v1 SKU için kurallar portalda listelenen sırayla işlenir. Temel dinleyici listede ilk sıradaysa ve gelen bir istekle eşleşiyorsa, o dinleyici tarafından işlenir. v2 SKU için, tam eşleşmeler daha yüksek önceliğe sahiptir. Ancak, temel bir dinleyiciyi yapılandırmadan önce çok siteli dinleyicileri yapılandırmanız önerilir. Bu işlem, trafiğin doğru arka uca yönlendirilmesini güvence altına alır.

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

### <a name="pathpattern"></a>Yol Deseni

PathPattern eşleşecek yol desenleri listesidir. Her biri / ile başlamalıdır. "*" işareti, yalnızca "/" işaretinin ardından en sona koyulabilir. Yol eşleştiriciye beslenen dize ilkinden sonra herhangi bir metin içermez? ya da #, ve bu chars burada izin verilmez. Aksi takdirde, bir URL'de izin verilen karakterlere PathPattern'de izin verilir.

Desteklenen desenler, Uygulama Ağ Geçidi v1 veya v2'yi dağıtıp dağıtmadığınıza bağlıdır:

#### <a name="v1"></a>v1

Yol kuralları büyük/küçük harf duyarsızdır.

|v1 yol deseni  |Desteklenir mi?  |
|---------|---------|
|`/images/*`     |evet|
|`/images*`     |hayır|
|`/images/*.jpg`     |hayır|
|`/*.jpg`     |hayır|
|`/Repos/*/Comments/*`     |hayır|
|`/CurrentUser/Comments/*`     |evet|

#### <a name="v2"></a>v2

Yol kuralları büyük/küçük harf duyarsızdır.

|v2 yol deseni  |Desteklenir mi?  |
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
        "id": "/subscriptions/{subscriptionId}/../microsoft.network/applicationGateways/{gatewayName}/ urlPathMaps/{urlpathMapName}"
    },

}
    }
]
```

## <a name="next-steps"></a>Sonraki adımlar

URL tabanlı içerik yönlendirme hakkında bilgi edindikten sonra, URL yönlendirme kurallarıyla bir uygulama ağ geçidi oluşturmak için [URL tabanlı yönlendirme kullanan uygulama ağ geçidi oluşturma](create-url-route-portal.md) bölümüne gidin.
