---
title: Azure Load Balancer için yüksek kullanılabilirlik bağlantı noktalarını yapılandırma
titleSuffix: Azure Load Balancer
description: Tüm bağlantı noktalarında iç trafik yükünü dengelemek için yüksek kullanılabilirlik bağlantı noktalarını nasıl kullanacağınızı öğrenin
services: load-balancer
documentationcenter: na
author: rdhillon
manager: narayan
ms.service: load-balancer
ms.devlang: na
ms.topic: how-to
ms.custom: seodec18
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/21/2018
ms.author: allensu
ms.openlocfilehash: a71a01629f334534d8eb26847a8f3400efbbeafe
ms.sourcegitcommit: ad66392df535c370ba22d36a71e1bbc8b0eedbe3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/16/2020
ms.locfileid: "84807833"
---
# <a name="configure-high-availability-ports-for-an-internal-load-balancer"></a>İç yük dengeleyici için yüksek kullanılabilirlik bağlantı noktaları yapılandırma

Bu makalede, iç yük dengeleyicide yüksek kullanılabilirlik bağlantı noktalarının örnek bir dağıtımı sağlanmıştır. Ağ sanal gereçlerine (NVA 'lar) özgü yapılandırma hakkında daha fazla bilgi için, ilgili sağlayıcı web sitelerine bakın.

>[!NOTE]
>Azure Load Balancer iki farklı türü destekler: Temel ve Standart. Bu makalede Standart Load Balancer açıklanmaktadır. Temel Load Balancer hakkında daha fazla bilgi için bkz. [Load Balancer genel bakış](load-balancer-overview.md).

Çizimde, bu makalede açıklanan dağıtım örneği için aşağıdaki yapılandırma gösterilmektedir:

- NVA 'lar, yüksek kullanılabilirlik bağlantı noktaları yapılandırmasının arkasındaki bir iç yük dengeleyicinin arka uç havuzunda dağıtılır. 
- DMZ alt ağında uygulanan Kullanıcı tanımlı yol (UDR), sonraki atlamayı iç yük dengeleyici sanal IP 'si olarak yaparak tüm trafiği NVA 'lar 'e yönlendirir. 
- İç yük dengeleyici, trafiği yük dengeleyici algoritmasına göre etkin NVA 'lar birine dağıtır.
- NVA trafiği işler ve arka uç alt ağındaki özgün hedefe iletir.
- Arka uç alt ağında karşılık gelen bir UDR yapılandırılmışsa, dönüş yolu aynı rotayı alabilir. 

![Yüksek kullanılabilirlik bağlantı noktaları örnek dağıtımı](./media/load-balancer-configure-ha-ports/haports.png)

## <a name="configure-high-availability-ports"></a>Yüksek kullanılabilirlik bağlantı noktalarını yapılandırma

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Yüksek kullanılabilirlik bağlantı noktalarını yapılandırmak için arka uç havuzundaki NVA 'lar ile iç yük dengeleyici ayarlayın. NVA durumunu ve yük dengeleyici kuralını yüksek kullanılabilirlik bağlantı noktalarıyla algılamak için karşılık gelen yük dengeleyici durum araştırma yapılandırmasını ayarlayın. Genel yük dengeleyici ile ilgili yapılandırma, [kullanmaya başlama](load-balancer-get-started-ilb-arm-portal.md)bölümünde ele alınmıştır. Bu makalede, yüksek kullanılabilirlik bağlantı noktaları yapılandırması vurgulanmaktadır.

Yapılandırma temelde ön uç bağlantı noktası ve arka uç bağlantı noktası değerini **0**olarak ayarlamayı içerir. Protokol değerini **All**olarak ayarlayın. Bu makalede Azure portal, PowerShell ve Azure CLı kullanarak yüksek kullanılabilirlik bağlantı noktalarının nasıl yapılandırılacağı açıklanır.

### <a name="configure-a-high-availability-ports-load-balancer-rule-with-the-azure-portal"></a>Azure portal ile yüksek kullanılabilirlik bağlantı noktaları yük dengeleyici kuralı yapılandırma

Azure portal kullanarak yüksek kullanılabilirlik bağlantı noktalarını yapılandırmak için, **ha bağlantı noktaları** onay kutusunu seçin. Seçildiğinde, ilişkili bağlantı noktası ve protokol yapılandırması otomatik olarak doldurulur. 

![Azure portal aracılığıyla yüksek kullanılabilirlik bağlantı noktaları yapılandırması](./media/load-balancer-configure-ha-ports/haports-portal.png)

### <a name="configure-a-high-availability-ports-load-balancing-rule-via-the-resource-manager-template"></a>Kaynak Yöneticisi şablonu aracılığıyla yüksek kullanılabilirlik bağlantı noktaları Yük Dengeleme kuralı yapılandırma

Load Balancer kaynağında Microsoft. Network/loadBalancers için 2017-08-01 API sürümünü kullanarak yüksek kullanılabilirlik bağlantı noktaları yapılandırabilirsiniz. Aşağıdaki JSON kod parçacığı, REST API aracılığıyla yüksek kullanılabilirlik bağlantı noktaları için yük dengeleyici yapılandırmasındaki değişiklikleri gösterir:

```json
    {
        "apiVersion": "2017-08-01",
        "type": "Microsoft.Network/loadBalancers",
        ...
        "sku":
        {
            "name": "Standard"
        },
        ...
        "properties": {
            "frontendIpConfigurations": [...],
            "backendAddressPools": [...],
            "probes": [...],
            "loadBalancingRules": [
             {
                "properties": {
                    ...
                    "protocol": "All",
                    "frontendPort": 0,
                    "backendPort": 0
                }
             }
            ],
       ...
       }
    }
```

### <a name="configure-a-high-availability-ports-load-balancer-rule-with-powershell"></a>PowerShell ile yüksek kullanılabilirlik bağlantı noktaları yük dengeleyici kuralı yapılandırma

PowerShell ile iç yük dengeleyici oluştururken yüksek kullanılabilirlik bağlantı noktaları yük dengeleyici kuralını oluşturmak için aşağıdaki komutu kullanın:

```powershell
lbrule = New-AzLoadBalancerRuleConfig -Name "HAPortsRule" -FrontendIpConfiguration $frontendIP -BackendAddressPool $beAddressPool -Probe $healthProbe -Protocol "All" -FrontendPort 0 -BackendPort 0
```

### <a name="configure-a-high-availability-ports-load-balancer-rule-with-azure-cli"></a>Azure CLı ile yüksek kullanılabilirlik bağlantı noktaları yük dengeleyici kuralı yapılandırma

[İç yük dengeleyici kümesi oluşturma](load-balancer-get-started-ilb-arm-cli.md)adım 4 ' te, yüksek kullanılabilirlik bağlantı noktaları yük dengeleyici kuralını oluşturmak için aşağıdaki komutu kullanın:

```azurecli
az network lb rule create --resource-group contoso-rg --lb-name contoso-ilb --name haportsrule --protocol all --frontend-port 0 --backend-port 0 --frontend-ip-name feilb --backend-address-pool-name beilb
```

## <a name="next-steps"></a>Sonraki adımlar

[Yüksek kullanılabilirlik bağlantı noktaları](load-balancer-ha-ports-overview.md)hakkında daha fazla bilgi edinin.