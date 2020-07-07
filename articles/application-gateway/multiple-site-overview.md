---
title: Azure Application Gateway 'de birden çok site barındırma
description: Bu makalede Azure Application Gateway çoklu site desteğine genel bir bakış sunulmaktadır.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.date: 03/11/2020
ms.author: amsriva
ms.topic: conceptual
ms.openlocfilehash: 4d945a255dacd35c61c3c80574b7d46b56de4aab
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "80257419"
---
# <a name="application-gateway-multiple-site-hosting"></a>Application Gateway birden çok site barındırma

Birden çok site barındırma, bir uygulama ağ geçidinin aynı bağlantı noktasında birden fazla Web uygulaması yapılandırmanızı sağlar. Bu özellik, bir Application Gateway 'e en çok 100 web sitesi ekleyerek dağıtımlarınız için daha verimli bir topoloji yapılandırmanıza olanak tanır. Her web sitesi, kendi arka uç havuzuna yönlendirilebilir. Aşağıdaki örnekte, Application Gateway `contoso.com` `fabrikam.com` ContosoServerPool ve FabrikamServerPool adlı iki arka uç sunucu havuzu için ve üzerinden trafiğe hizmet verir.

![imageURLroute](./media/multiple-site-overview/multisite.png)

> [!IMPORTANT]
> Kurallar, v1 SKU 'SU için portalda listelendikleri sırada işlenir. V2 SKU 'SU için, tam eşleşmeler daha yüksek önceliğe sahiptir. Temel dinleyiciyi yapılandırmadan önce çok siteli dinleyicileri yapılandırmanız önerilir.  Bu işlem, trafiğin doğru arka uca yönlendirilmesini güvence altına alır. Temel dinleyici listede ilk sıradaysa ve gelen bir istekle eşleşiyorsa, o dinleyici tarafından işlenir.

`http://contoso.com` için istekler ContosoServerPool’a ve `http://fabrikam.com` için istekler FabrikamServerPool’a yönlendirilir.

Benzer şekilde, aynı uygulama ağ geçidi dağıtımında aynı üst etki alanının birden çok alt etki alanını barındırabilirsiniz. Örneğin, `http://blog.contoso.com` `http://app.contoso.com` tek bir uygulama ağ geçidi dağıtımını barındırabilirsiniz.

## <a name="host-headers-and-server-name-indication-sni"></a>Barındırma üstbilgileri ve Sunucu Adı Belirtme (SNI)

Aynı altyapıda birden çok site barındırmayı etkinleştirmek için üç yaygın mekanizma bulunur.

1. Birden çok web uygulamasının her birini benzersiz bir IP adresinde barındırın.
2. Birden çok web uygulamasını aynı IP adresinde barındırmak için ana bilgisayar adını kullanın.
3. Birden çok web uygulamasını aynı IP adresinde barındırmak için farklı bağlantı noktalarını kullanın.

Şu anda Application Gateway trafiği dinlediği tek bir genel IP adresini destekler. Bu nedenle, her biri kendi IP adresine sahip olan birden çok uygulama desteklenmemektedir. 

Application Gateway, her biri farklı bağlantı noktalarında dinleyen birden çok uygulamayı destekler, ancak bu senaryo uygulamaların standart olmayan bağlantı noktalarında trafiği kabul etmesini gerektirir. Bu, genellikle istediğiniz bir yapılandırma değildir.

Application Gateway, aynı genel IP adresinde ve bağlantı noktasında birden çok web sitesini barındırmak için HTTP 1.1 barındırma bilgilerini kullanır. Uygulama ağ geçidinde barındırılan siteler Sunucu Adı Belirtme (SNı) TLS uzantılı TLS yük boşaltma 'yı da destekleyebilir. Bu senaryo, istemci tarayıcısının ve arka uç web grubunun RFC 6066’da belirtildiği gibi HTTP/1.1 ve TLS uzantısını desteklemesi gerektiği anlamına gelir.

## <a name="listener-configuration-element"></a>Dinleyici yapılandırma öğesi

Mevcut HTTPListener yapılandırma öğeleri, ana bilgisayar adı ve sunucu adı belirtme öğelerini destekleyecek şekilde geliştirilmiştir. Trafiği uygun arka uç havuzuna yönlendirmek için Application Gateway tarafından kullanılır. 

Aşağıdaki kod örneği, bir şablon dosyasındaki HttpListeners öğesinin kod parçacıbir örneğidir:

```json
"httpListeners": [
    {
        "name": "appGatewayHttpsListener1",
        "properties": {
            "FrontendIPConfiguration": {
                "Id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/frontendIPConfigurations/DefaultFrontendPublicIP"
            },
            "FrontendPort": {
                "Id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/frontendPorts/appGatewayFrontendPort443'"
            },
            "Protocol": "Https",
            "SslCertificate": {
                "Id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/sslCertificates/appGatewaySslCert1'"
            },
            "HostName": "contoso.com",
            "RequireServerNameIndication": "true"
        }
    },
    {
        "name": "appGatewayHttpListener2",
        "properties": {
            "FrontendIPConfiguration": {
                "Id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/frontendIPConfigurations/appGatewayFrontendIP'"
            },
            "FrontendPort": {
                "Id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/frontendPorts/appGatewayFrontendPort80'"
            },
            "Protocol": "Http",
            "HostName": "fabrikam.com",
            "RequireServerNameIndication": "false"
        }
    }
],
```

Uçtan uca şablon tabanlı dağıtım için [birden çok site barındırma kullanan Resource Manager şablonunu](https://github.com/Azure/azure-quickstart-templates/blob/master/201-application-gateway-multihosting) ziyaret edebilirsiniz.

## <a name="routing-rule"></a>Yönlendirme kuralı

Yönlendirme kuralında hiçbir değişiklik yapılması gerekmez. Uygun site dinleyicisini ilgili arka uç adres havuzuna bağlamak için 'Temel' yönlendirme kuralını kullanmaya devam etmeniz gerekir.

```json
"requestRoutingRules": [
{
    "name": "<ruleName1>",
    "properties": {
        "RuleType": "Basic",
        "httpListener": {
            "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/httpListeners/appGatewayHttpsListener1')]"
        },
        "backendAddressPool": {
            "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/backendAddressPools/ContosoServerPool')]"
        },
        "backendHttpSettings": {
            "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/backendHttpSettingsCollection/appGatewayBackendHttpSettings')]"
        }
    }

},
{
    "name": "<ruleName2>",
    "properties": {
        "RuleType": "Basic",
        "httpListener": {
            "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/httpListeners/appGatewayHttpListener2')]"
        },
        "backendAddressPool": {
            "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/backendAddressPools/FabrikamServerPool')]"
        },
        "backendHttpSettings": {
            "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/backendHttpSettingsCollection/appGatewayBackendHttpSettings')]"
        }
    }

}
]
```

## <a name="next-steps"></a>Sonraki adımlar

Birden çok site barındırma hakkında bilgi aldıktan sonra birden fazla web uygulamasını destekleyebilen uygulama ağ geçidi oluşturmak için [birden çok site barındırma kullanan uygulama ağ geçidi oluşturma](tutorial-multiple-sites-powershell.md) bölümüne gidin.

