---
title: Azure Load Balancer bileşenleri
description: Azure Load Balancer bileşenlere genel bakış
services: load-balancer
documentationcenter: na
author: asudbring
ms.service: load-balancer
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/04/2020
ms.author: allensu
ms.openlocfilehash: 6bf090cde7262fdae9c98ef55227bf2925937dbf
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2021
ms.locfileid: "101739840"
---
# <a name="azure-load-balancer-components"></a>Azure Load Balancer bileşenleri

Azure Load Balancer bazı önemli bileşenleri içerir. Bu bileşenler, aboneliğinizdeki aracılığıyla yapılandırılabilir:

* Azure portal
* Azure CLI
* Azure PowerShell
* Resource Manager Şablonları

## <a name="frontend-ip-configuration"></a>Ön uç IP yapılandırması <a name = "frontend-ip-configurations"></a>

Azure Load Balancer IP adresi. Bu, istemcilerle ilgili iletişim noktasıdır. Bu IP adresleri şunlardan biri olabilir:

- **Genel IP Adresi**
- **Özel IP adresi**

IP adresinin doğası, oluşturulan yük dengeleyicinin **türünü** belirler. Özel IP adresi seçimi bir iç yük dengeleyici oluşturur. Genel IP adresi seçimi bir genel yük dengeleyici oluşturur.

|  | Genel yük dengeleyici  | İç yük dengeleyici |
| ---------- | ---------- | ---------- |
| **Ön uç IP yapılandırması**| Genel IP adresi | Özel IP adresi|
| **Açıklama** | Ortak yük dengeleyici, gelen trafiğin genel IP ve bağlantı noktasını, sanal makinenin özel IP ve bağlantı noktasıyla eşleştirir. Yük dengeleyici trafiği VM 'den gelen yanıt trafiği için başka bir şekilde eşler. Yük Dengeleme kuralları uygulayarak, belirli trafik türlerini birden çok VM veya hizmet arasında dağıtabilirsiniz. Örneğin web isteği trafiğinin yükünü birden fazla web sunucusuna dağıtabilirsiniz.| İç yük dengeleyici, trafiği bir sanal ağ içindeki kaynaklara dağıtır. Azure, yük dengeli bir sanal ağın ön uç IP adreslerine erişimi kısıtlar. Ön uç IP adresleri ve sanal ağlar hiçbir şekilde doğrudan bir internet uç noktasına gösterilmez. İç iş kolu uygulamaları Azure'da çalışır ve Azure'dan veya şirket içi kaynaklardan erişim sağlanır. |
| **Desteklenen SKU 'Lar** | Temel, standart | Temel, standart |

![Katmanlı yük dengeleyici örneği](./media/load-balancer-overview/load-balancer.png)

Yük dengeleyicinin birden çok ön uç IP 'si olabilir. [Birden çok ön uçlar](load-balancer-multivip-overview.md)hakkında daha fazla bilgi edinin.

## <a name="backend-pool"></a>Arka uç havuzu

Gelen isteğe hizmet veren bir sanal makine ölçek kümesindeki sanal makine veya örnek grubu. Yüksek hacimli gelen trafiği karşılamak üzere maliyeti etkili bir şekilde ölçeklendirmek için, bilgi işlem kılavuzu genellikle arka uç havuzuna daha fazla örnek eklenmesini önerir.

Yük dengeleyici, örnekleri yukarı veya aşağı ölçeklendirirseniz otomatik yeniden yapılandırma yoluyla kendisini anında yeniden yapılandırır. Arka uç havuzundan VM ekleme veya kaldırma işlemi ek işlem yapılmadan yük dengeleyiciyi yeniden yapılandırır. Arka uç havuzunun kapsamı, sanal ağdaki herhangi bir sanal makinedir.

Arka uç havuzunuzu nasıl tasarlayacağınızı düşünürken, yönetim işlemlerinin uzunluğunu iyileştirmek için en az sayıda ayrı arka uç havuzu kaynağı tasarlayın. Veri düzlemi performansı veya ölçeği üzerinde farklılık yoktur.

## <a name="health-probes"></a>Durum araştırmaları

Bir sistem durumu araştırması, arka uç havuzundaki örneklerin sistem durumunu tespit etmek için kullanılır. Yük dengeleyici oluşturma sırasında, yük dengeleyicinin kullanması için bir sistem durumu araştırması yapılandırın.  Bu sistem durumu araştırması, bir örneğin sağlıklı olup olmadığını ve trafik alıp alamayacağını tespit eder.

Sistem durumu araştırmalarının sağlıksız eşiğini tanımlayabilirsiniz. Bir araştırma yanıtlanamazsa, yük dengeleyici sağlıksız örneklere yeni bağlantı göndermeyi durduruyor. Bir araştırma hatası varolan bağlantıları etkilemez. Bağlantı, uygulamaya kadar devam eder:

- Akışı sonlandırır
- Boşta kalma zaman aşımı oluştu
- VM kapanıyor

Yük dengeleyici uç noktalar için farklı durum araştırma türleri sağlar: TCP, HTTP ve HTTPS. [Load Balancer sistem durumu araştırmaları hakkında daha fazla bilgi edinin](load-balancer-custom-probe-overview.md).

Temel yük dengeleyici HTTPS araştırmalarını desteklemez. Temel yük dengeleyici tüm TCP bağlantılarını (kurulan bağlantılar dahil) kapatır.

## <a name="load-balancing-rules"></a>Yük Dengeleme kuralları

Gelen trafiğin arka uç havuzundaki **Tüm** örneklere nasıl dağıtıldığını tanımlamak için bir yük dengeleyici kuralı kullanılır. Yük Dengeleme kuralı, belirli bir ön uç IP yapılandırmasını ve bağlantı noktasını birden çok arka uç IP adresine ve bağlantı noktasına eşler.

Örneğin, ön uç IP 'nizden gelen trafiği arka uç örneklerinizin 80 numaralı bağlantı noktasına yönlendirmek için 80 numaralı bağlantı noktası için bir yük dengeleme kuralı kullanın.

:::image type="content" source="./media/load-balancer-components/lbrules.png" alt-text="Yük dengeleyici kuralı başvuru diyagramı" border="false":::

*Şekil: Load-Balancing kuralları*

## <a name="high-availability-ports"></a>Yüksek kullanılabilirlik bağlantı noktaları

**' Protocol-All ve port-0 '** ile yapılandırılmış bir yük dengeleyici kuralı. 

Bu kural, bir iç Standart Load Balancer tüm bağlantı noktalarına gelen tüm TCP ve UDP akışlarının yük dengelenmesi için tek bir kural sağlar. 

Yük Dengeleme kararı akış başına yapılır. Bu eylem aşağıdaki beş demet bağlantısına dayanır: 

1. 
    en yakın kullanılabilir uç noktayı arama
  
2. kaynak bağlantı noktası
3. hedef IP adresi
4. hedef bağlantı noktası
5. protokol

HA bağlantı noktaları Yük Dengeleme kuralları, sanal ağların içindeki ağ sanal gereçleri (NVA 'lar) için yüksek kullanılabilirlik ve ölçek gibi kritik senaryolarda size yardımcı olur. Bu özellik, çok sayıda bağlantı noktasının yük dengeli olması gerektiğinde yardımcı olabilir.

<p align="center">
  <img src="./media/load-balancer-components/harules.svg" alt="Figure depicts how Azure Load Balancer directs all frontend ports to three instances of all backend ports" width="512" title="HA bağlantı noktaları kuralları">
</p>

*Şekil: HA bağlantı noktası kuralları*

[Ha bağlantı noktaları](load-balancer-ha-ports-overview.md)hakkında daha fazla bilgi edinin.

## <a name="inbound-nat-rules"></a>Gelen NAT kuralları

Gelen NAT kuralı, ön uç IP adresine ve bağlantı noktası birleşimine gönderilen gelen trafiği iletir. Trafik, arka uç havuzundaki **belirli** bir sanal makineye veya örneğe gönderilir. Bağlantı noktası iletme, Yük Dengeleme ile aynı karma tabanlı dağıtım tarafından yapılır.

:::image type="content" source="./media/load-balancer-components/inboundnatrules.png" alt-text="Gelen NAT kuralı başvuru diyagramı" border="false":::

*Şekil: gelen NAT kuralları*

Sanal Makine Ölçek Kümeleri bağlamındaki gelen NAT kuralları gelen NAT havuzlarıdır. [Load Balancer bileşenleri ve sanal makine ölçek kümesi](../virtual-machine-scale-sets/virtual-machine-scale-sets-networking.md#azure-virtual-machine-scale-sets-with-azure-load-balancer)hakkında daha fazla bilgi edinin.

## <a name="outbound-rules"></a>Giden kuralları

Giden bir kural, arka uç havuzu tarafından tanımlanan tüm sanal makineler veya örnekler için giden ağ adresi çevirisi 'ni (NAT) yapılandırır. Bu kural, arka uçtaki örneklerin internet veya diğer uç noktalara iletişim kurmasını sağlar.

[Giden bağlantılar ve kurallar](load-balancer-outbound-connections.md)hakkında daha fazla bilgi edinin.

Temel yük dengeleyici giden kuralları desteklemez.

:::image type="content" source="./media/load-balancer-components/outbound-rules.png" alt-text="Giden kuralı başvuru diyagramı" border="false":::

*Şekil: giden kuralları*

## <a name="limitations"></a>Sınırlamalar

- Yük dengeleyici [sınırları](../azure-resource-manager/management/azure-subscription-service-limits.md) hakkında bilgi edinin 
- Yük dengeleyici, belirli TCP veya UDP protokolleri için yük dengeleme ve bağlantı noktası iletme sağlar. Yük Dengeleme kuralları ve gelen NAT kuralları TCP ve UDP 'yi destekler, ancak ıCMP dahil diğer IP protokollerini desteklemez.
- Bir arka uç VM 'den bir iç Load Balancer ön uca giden akış başarısız olur.
- Yük dengeleyici kuralı iki sanal ağı yayılamaz.  Ön uçların ve arka uç örneklerinin aynı sanal ağda bulunması gerekir.  
- IP parçalarını iletme, Yük Dengeleme kurallarında desteklenmez. UDP ve TCP paketlerinin IP parçalanması, Yük Dengeleme kurallarında desteklenmez. HA bağlantı noktaları Yük Dengeleme kuralları, var olan IP parçalarını iletmek için kullanılabilir. Daha fazla bilgi için bkz. [yüksek kullanılabilirlik bağlantı noktalarına genel bakış](load-balancer-ha-ports-overview.md).
- Kullanılabilirlik kümesi başına yalnızca 1 ortak Load Balancer ve 1 iç Load Balancer olabilir

## <a name="next-steps"></a>Sonraki adımlar

- Load Balancer kullanmaya başlamak için bkz. [Genel Standart yük dengeleyici oluşturma](quickstart-load-balancer-standard-public-portal.md) .
- [Azure Load Balancer](load-balancer-overview.md)hakkında daha fazla bilgi edinin.
- [Genel IP adresi](../virtual-network/virtual-network-public-ip-address.md) hakkında bilgi edinin
- [Özel IP adresi](../virtual-network/private-ip-addresses.md) hakkında bilgi edinin
- [Standart yük dengeleyici ve kullanılabilirlik alanları](load-balancer-standard-availability-zones.md)kullanma hakkında bilgi edinin.
- [Standart yük dengeleyici tanılama](load-balancer-standard-diagnostics.md)hakkında bilgi edinin.
- [Boşta durumunda TCP sıfırlaması](load-balancer-tcp-reset.md)hakkında bilgi edinin.
- [Ha bağlantı noktaları Yük Dengeleme kurallarıyla standart yük dengeleyici](load-balancer-ha-ports-overview.md)hakkında bilgi edinin.
- [Ağ güvenlik grupları](../virtual-network/network-security-groups-overview.md)hakkında daha fazla bilgi edinin.
- [Yük dengeleyici sınırları](../azure-resource-manager/management/azure-subscription-service-limits.md#load-balancer)hakkında daha fazla bilgi edinin.
- [Bağlantı noktası iletmeyi](./tutorial-load-balancer-port-forwarding-portal.md)kullanma hakkında bilgi edinin.
