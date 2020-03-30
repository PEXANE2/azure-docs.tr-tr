---
title: Azure Application Gateway'de WebSocket desteği
description: Application Gateway, tüm ağ geçidi boyutlarında WebSocket için yerel destek sağlar. Kullanıcı tarafından yapılandırılabilir ayar yok.
author: vhorne
ms.author: amsriva
ms.service: application-gateway
services: application-gateway
ms.topic: conceptual
ms.date: 11/16/2019
ms.openlocfilehash: baa02c4d946a121f26f421af99835ae2bea18847
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74130340"
---
# <a name="overview-of-websocket-support-in-application-gateway"></a>Uygulama Ağ Geçidi'nde WebSocket desteğine genel bakış

Application Gateway, tüm ağ geçidi boyutlarında WebSocket için yerel destek sağlar. WebSocket desteğini isteğe bağlı olarak etkinleştirmek veya devre dışı bırakmak için kullanıcı tarafından yapılandırılabilen bir ayar yoktur. 

[RFC6455'te](https://tools.ietf.org/html/rfc6455) standartlaştırılmış WebSocket protokolü, uzun süren bir TCP bağlantısı üzerinden sunucu ve istemci arasında tam çift yönlü iletişim sağlar. Bu özellik, http tabanlı uygulamalarda gerektiği gibi yoklama gerek kalmadan çift yönlü olabilir web sunucusu ve istemci arasında daha etkileşimli bir iletişim sağlar. WebSocket'in http'den farklı olarak düşük yükü vardır ve aynı TCP bağlantısını birden çok istek/yanıt için yeniden kullanabilir ve bu da kaynakların daha verimli kullanılmasıyla sonuçlanır. WebSocket protokolleri 80 ve 443 geleneksel HTTP bağlantı noktaları üzerinde çalışmak üzere tasarlanmıştır.

WebSocket trafiğini almak için bağlantı noktası 80 veya 443'te standart bir HTTP dinleyicisi kullanmaya devam edebilirsiniz. WebSocket trafiği daha sonra uygulama ağ geçidi kurallarında belirtildiği gibi uygun arka uç havuzunu kullanarak WebSocket etkin arka uç sunucusuna yönlendirilir. Arka uç sunucusu, [sistem durumu sondasına genel bakış](application-gateway-probe-overview.md) bölümünde açıklanan uygulama ağ geçidi sondalarına yanıt vermelidir. Uygulama ağ geçidi sistem sondaları yalnızca HTTP/HTTPS'dir. Her arka uç sunucusu, WebSocket trafiğini sunucuya yönlendirmek için uygulama ağ geçidi için HTTP sondalarına yanıt vermelidir.

Sohbet, pano ve oyun uygulamaları gibi hızlı ve gerçek zamanlı iletişimden yararlanan uygulamalarda kullanılır.

## <a name="how-does-websocket-work"></a>WebSocket nasıl çalışır?

Bir WebSocket bağlantısı kurmak için, istemci ve sunucu arasında belirli bir HTTP tabanlı el sıkışma sıcağına bağlanır. Başarılı olursa, uygulama katmanı iletişim kuralı önceden kurulmuş TCP bağlantısı kullanılarak HTTP'den WebSockets'e "yükseltilir". Bu gerçekleştiğinde, HTTP tamamen resmin dışındadır; veriler, WebSocket bağlantısı kapanana kadar her iki uç nokta yla WebSocket protokolü kullanılarak gönderilebilir veya alınabilir. 

![Websocket](./media/application-gateway-websocket/websocket.png)

### <a name="listener-configuration-element"></a>Dinleyici yapılandırma öğesi

Varolan bir HTTP dinleyicisi WebSocket trafiğini desteklemek için kullanılabilir. Aşağıda, örnek bir şablon dosyasından bir httpListeners öğesinin bir kesiti verilmiştir. WebSocket'i desteklemek ve WebSocket trafiğini güvence altına almak için hem HTTP hem de HTTPS dinleyicilerine ihtiyacınız olacaktır. Benzer şekilde, WebSocket trafiğini desteklemek için 80/443 bağlantı noktasındaki dinleyicilerle bir uygulama ağ geçidi oluşturmak için portalı veya Azure PowerShell'i kullanabilirsiniz.

```json
"httpListeners": [
        {
            "name": "appGatewayHttpsListener",
            "properties": {
                "FrontendIPConfiguration": {
                    "Id": "/subscriptions/{subscriptionId/resourceGroups/{resourceGroupName/providers/Microsoft.Network/applicationGateways/{applicationGatewayName/frontendIPConfigurations/DefaultFrontendPublicIP"
                },
                "FrontendPort": {
                    "Id": "/subscriptions/{subscriptionId/resourceGroups/{resourceGroupName/providers/Microsoft.Network/applicationGateways/{applicationGatewayName/frontendPorts/appGatewayFrontendPort443'"
                },
                "Protocol": "Https",
                "SslCertificate": {
                    "Id": "/subscriptions/{subscriptionId/resourceGroups/{resourceGroupName/providers/Microsoft.Network/applicationGateways/{applicationGatewayName/sslCertificates/appGatewaySslCert1'"
                },
            }
        },
        {
            "name": "appGatewayHttpListener",
            "properties": {
                "FrontendIPConfiguration": {
                    "Id": "/subscriptions/{subscriptionId/resourceGroups/{resourceGroupName/providers/Microsoft.Network/applicationGateways/{applicationGatewayName/frontendIPConfigurations/appGatewayFrontendIP'"
                },
                "FrontendPort": {
                    "Id": "/subscriptions/{subscriptionId/resourceGroups/{resourceGroupName/providers/Microsoft.Network/applicationGateways/{applicationGatewayName/frontendPorts/appGatewayFrontendPort80'"
                },
                "Protocol": "Http",
            }
        }
    ],
```

## <a name="backendaddresspool-backendhttpsetting-and-routing-rule-configuration"></a>BackendAddressPool, BackendHttpSetting ve Yönlendirme kuralı yapılandırma

BackendAddressPool, WebSocket etkin sunucularına sahip bir arka uç havuzu tanımlamak için kullanılır. BackendHttpSetting bir arka uç bağlantı noktası 80 ve 443 ile tanımlanır. HTTP Ayarları'ndaki istek zaman sonu değeri WebSocket oturumu için de geçerlidir. Uygun dinleyiciyi ilgili arka uç adresi havuzuna bağlamak için kullanılan yönlendirme kuralında değişiklik gerekmez. 

```json
"requestRoutingRules": [{
    "name": "<ruleName1>",
    "properties": {
        "RuleType": "Basic",
        "httpListener": {
            "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/applicationGateways/{applicationGatewayName}/httpListeners/appGatewayHttpsListener')]"
        },
        "backendAddressPool": {
            "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/applicationGateways/{applicationGatewayName}/backendAddressPools/ContosoServerPool')]"
        },
        "backendHttpSettings": {
            "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/applicationGateways/{applicationGatewayName}/backendHttpSettingsCollection/appGatewayBackendHttpSettings')]"
        }
    }

}, {
    "name": "<ruleName2>",
    "properties": {
        "RuleType": "Basic",
        "httpListener": {
            "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/applicationGateways/{applicationGatewayName}/httpListeners/appGatewayHttpListener')]"
        },
        "backendAddressPool": {
            "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/applicationGateways/{applicationGatewayName}/backendAddressPools/ContosoServerPool')]"
        },
        "backendHttpSettings": {
            "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/applicationGateways/{applicationGatewayName}/backendHttpSettingsCollection/appGatewayBackendHttpSettings')]"
        }

    }
}]
```

## <a name="websocket-enabled-backend"></a>WebSocket etkin arka uç

WebSocket'in çalışması için arka uçunuzda yapılandırılan bağlantı noktasında (genellikle 80/443) çalışan bir HTTP/HTTPS web sunucusu olmalıdır. Bu gereksinim, WebSocket protokolünün, websocket protokolüne üstbilgi alanı olarak yükseltilirken ilk el sıkışmanın HTTP olmasını gerektirmesidir. Aşağıdaki bir üstbilgi örneğidir:

```
    GET /chat HTTP/1.1
    Host: server.example.com
    Upgrade: websocket
    Connection: Upgrade
    Sec-WebSocket-Key: dGhlIHNhbXBsZSBub25jZQ==
    Origin: https://example.com
    Sec-WebSocket-Protocol: chat, superchat
    Sec-WebSocket-Version: 13
```

Bunun bir diğer nedeni de uygulama ağ geçidi arka uç sağlık sondası yalnızca HTTP ve HTTPS protokollerini destekler. Arka uç sunucusu HTTP veya HTTPS sondalarına yanıt vermiyorsa, arka uç havuzundan çıkarılır.

## <a name="next-steps"></a>Sonraki adımlar

WebSocket desteğini öğrendikten sonra, WebSocket etkinleştirilmiş bir web uygulamasıyla başlamak için [bir uygulama ağ geçidi oluşturmaya](quick-create-powershell.md) gidin.