---
title: Azure Yük Dengeleyicisi için Yüksek Kullanılabilirlik Bağlantı Noktalarını Yapılandırma
titleSuffix: Azure Load Balancer
description: Tüm bağlantı noktalarındaki iç trafiği dengelemek için Yüksek Kullanılabilirlik Bağlantı Noktalarını nasıl kullanacağınızı öğrenin
services: load-balancer
documentationcenter: na
author: rdhillon
manager: narayan
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.custom: seodec18
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/21/2018
ms.author: allensu
ms.openlocfilehash: c6b8ecb443408f23ae604bd9c8139cb0a2afcd12
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79477791"
---
# <a name="configure-high-availability-ports-for-an-internal-load-balancer"></a>İç yük dengeleyicisi için Yüksek Kullanılabilirlik Bağlantı Noktalarını Yapılandırın

Bu makalede, bir iç yük dengeleyici yüksek kullanılabilirlik bağlantı noktaları bir örnek dağıtım sağlar. Ağ sanal cihazlarına (NVA) özgü yapılandırmalar hakkında daha fazla bilgi için ilgili sağlayıcı web sitelerine bakın.

>[!NOTE]
>Azure Load Balancer iki farklı türü destekler: Temel ve Standart. Bu makalede Standart Yük Dengeleyicisi anlatılmaktadır. Temel Yük Dengeleyicisi hakkında daha fazla bilgi için [Bkz. Yük Dengeleyicisi genel bakışı.](load-balancer-overview.md)

Resimde, bu makalede açıklanan dağıtım örneğinin aşağıdaki yapılandırması gösterilmektedir:

- NVA'lar, Yüksek Kullanılabilirlik Bağlantı Noktaları yapılandırmasının arkasındaki dahili yük dengeleyicisinin arka uç havuzunda dağıtılır. 
- DMZ alt ağına uygulanan kullanıcı tanımlı rota (UDR), iç yük dengeleyici sanal IP olarak bir sonraki atlamayı yaparak tüm trafiği NVA'lara yönlendirir. 
- İç yük dengeleyici, yük dengeleyici algoritmasına göre trafiği etkin NV'lerden birine dağıtır.
- NVA trafiği işler ve arka uç alt netindeki orijinal hedefe iletir.
- İlgili udr arka uç alt netinde yapılandırılırsa, dönüş yolu aynı rotayı alabilir. 

![Yüksek Kullanılabilirlik Bağlantı Noktaları örnek dağıtım](./media/load-balancer-configure-ha-ports/haports.png)

## <a name="configure-high-availability-ports"></a>Yüksek Kullanılabilirlik Bağlantı Noktalarını Yapılandırma

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Yüksek Kullanılabilirlik Bağlantı Noktalarını yapılandırmak için, arka uç havuzunda NVA'larla bir dahili yük dengeleyiciayarlayın. NVA durumunu ve Yüksek Kullanılabilirlik Bağlantı Noktaları ile yük dengeleyici kuralını algılamak için karşılık gelen bir yük dengeleyici sistem sondası yapılandırması ayarlayın. Genel yük dengeleyicisi ile ilgili yapılandırma Başlat'ta ele [alınmıştır.](load-balancer-get-started-ilb-arm-portal.md) Bu makalede, Yüksek Kullanılabilirlik Bağlantı Noktaları yapılandırması vurgulamaktadır.

Yapılandırma aslında ön uç bağlantı noktası ve arka uç bağlantı noktası değerini **0**olarak ayarlama içerir. Protokol değerini **Tümü'ne**ayarlayın. Bu makalede, Azure portalı, PowerShell ve Azure CLI'yi kullanarak Yüksek Kullanılabilirlik Bağlantı Noktaları'nın nasıl yapılandırılabildiğini açıklanmaktadır.

### <a name="configure-a-high-availability-ports-load-balancer-rule-with-the-azure-portal"></a>Azure portalı ile Yüksek Kullanılabilirlik Bağlantı Noktaları yük dengeleyici kuralını yapılandırma

Azure portalını kullanarak Yüksek Kullanılabilirlik Bağlantı Noktalarını yapılandırmak için **HA Bağlantı Noktaları** onay kutusunu seçin. Seçildiğinde, ilgili bağlantı noktası ve protokol yapılandırması otomatik olarak doldurulur. 

![Azure portalı üzerinden Yüksek Kullanılabilirlik Bağlantı Noktaları yapılandırması](./media/load-balancer-configure-ha-ports/haports-portal.png)

### <a name="configure-a-high-availability-ports-load-balancing-rule-via-the-resource-manager-template"></a>Kaynak Yöneticisi şablonu aracılığıyla Yüksek Kullanılabilirlik Bağlantı Noktaları yük dengeleme kuralını yapılandırma

Load Balancer kaynağında Microsoft.Network/loadBalancers için 2017-08-01 API sürümünü kullanarak Yüksek Kullanılabilirlik Bağlantı Noktalarını yapılandırabilirsiniz. Aşağıdaki JSON snippet REST API üzerinden Yüksek Kullanılabilirlik Bağlantı Noktaları için yük dengeleyici yapılandırmadeğişiklikleri göstermektedir:

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

### <a name="configure-a-high-availability-ports-load-balancer-rule-with-powershell"></a>PowerShell ile Yüksek Kullanılabilirlik Bağlantı Noktaları yük dengeleyici kuralını yapılandırın

PowerShell ile dahili yük dengeleyicisini oluştururken Yüksek Kullanılabilirlik Bağlantı Noktaları yük dengeleyici kuralını oluşturmak için aşağıdaki komutu kullanın:

```powershell
lbrule = New-AzLoadBalancerRuleConfig -Name "HAPortsRule" -FrontendIpConfiguration $frontendIP -BackendAddressPool $beAddressPool -Probe $healthProbe -Protocol "All" -FrontendPort 0 -BackendPort 0
```

### <a name="configure-a-high-availability-ports-load-balancer-rule-with-azure-cli"></a>Azure CLI ile Yüksek Kullanılabilirlik Bağlantı Noktaları yük dengeleyici kuralını yapılandırma

[Dahili yük dengeleyici kümesi oluşturma](load-balancer-get-started-ilb-arm-cli.md)nın 4.

```azurecli
az network lb rule create --resource-group contoso-rg --lb-name contoso-ilb --name haportsrule --protocol all --frontend-port 0 --backend-port 0 --frontend-ip-name feilb --backend-address-pool-name beilb
```

## <a name="next-steps"></a>Sonraki adımlar

[Yüksek Kullanılabilirlik Bağlantı Noktaları](load-balancer-ha-ports-overview.md)hakkında daha fazla bilgi edinin.