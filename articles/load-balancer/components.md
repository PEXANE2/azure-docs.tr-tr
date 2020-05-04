---
title: Azure Load Balancer bileşenleri
description: Azure Load Balancer bileşenlere genel bakış
services: load-balancer
documentationcenter: na
author: asudbring
ms.service: load-balancer
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/30/2020
ms.author: allensu
ms.openlocfilehash: 4a84c43b57ec4f632a2bfabb10d112e4975249bf
ms.sourcegitcommit: 4499035f03e7a8fb40f5cff616eb01753b986278
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/03/2020
ms.locfileid: "82733116"
---
# <a name="azure-load-balancer-components"></a>Azure Load Balancer bileşenleri

Azure Load Balancer, işlemi için birkaç temel bileşen içerir. Bu bileşenler aboneliğinizde Azure portal, Azure CLı, Azure PowerShell veya şablonları aracılığıyla yapılandırılabilir.

## <a name="frontend-ip-configurations"></a>Ön uç IP yapılandırması

Yük dengeleyicinin IP adresi. Bu, istemcilerle ilgili iletişim noktasıdır. Bu adresler şunlardan biri olabilir:

- **Genel IP adresi**
- **Özel IP adresi**

IP adresi seçimi, oluşturulan yük dengeleyicinin **türünü** belirler. Özel IP adresi seçimi bir iç yük dengeleyici oluşturur. Genel IP adresi seçimi bir genel yük dengeleyici oluşturur.

|  | Genel Load Balancer  | İç Yük Dengeleyici |
| ---------- | ---------- | ---------- |
| Ön uç IP yapılandırması| Genel IP adresi | Özel IP adresi|
| Açıklama | Ortak yük dengeleyici, gelen trafiğin genel IP ve bağlantı noktasını, sanal makinenin özel IP ve bağlantı noktasıyla eşleştirir. Yük dengeleyici trafiği VM 'den gelen yanıt trafiği için başka bir şekilde eşler. Yük Dengeleme kuralları uygulayarak, belirli trafik türlerini birden çok VM veya hizmet arasında dağıtabilirsiniz. Örneğin web isteği trafiğinin yükünü birden fazla web sunucusuna dağıtabilirsiniz.| İç yük dengeleyici, trafiği bir sanal ağ içindeki kaynaklara dağıtır. Azure, yük dengeli bir sanal ağın ön uç IP adreslerine erişimi kısıtlar. Ön uç IP adresleri ve sanal ağlar hiçbir şekilde doğrudan bir internet uç noktasına gösterilmez. İç iş kolu uygulamaları Azure'da çalışır ve Azure'dan veya şirket içi kaynaklardan erişim sağlanır. |
| Desteklenen SKU 'Lar | Temel, standart | Temel, standart |

![Katmanlı yük dengeleyici örneği](./media/load-balancer-overview/load-balancer.png)

## <a name="backend-pool"></a>Arka uç havuzu

Gelen isteğe hizmet veren bir sanal makine ölçek kümesindeki sanal makine veya örnek grubu. Yüksek hacimli gelen trafiği karşılamak üzere maliyeti etkili bir şekilde ölçeklendirmek için, bilgi işlem kılavuzu genellikle arka uç havuzuna daha fazla örnek eklenmesini önerir. 

Yük dengeleyici, örnekleri yukarı veya aşağı ölçeklendirirseniz otomatik yeniden yapılandırma yoluyla kendisini anında yeniden yapılandırır. Arka uç havuzundan VM ekleme veya kaldırma işlemi ek işlem yapılmadan yük dengeleyiciyi yeniden yapılandırır. Arka uç havuzunun kapsamı, sanal ağdaki herhangi bir sanal makinedir. 

Arka uç havuzunuzu nasıl tasarlayacağınızı düşünürken, yönetim işlemlerinin uzunluğunu iyileştirmek için en az sayıda ayrı arka uç havuzu kaynağı tasarlayın. Veri düzlemi performansı veya ölçeği üzerinde farklılık yoktur.

## <a name="health-probes"></a>Sistem durumu araştırmaları

Bir sistem durumu araştırması, arka uç havuzundaki örneklerin sistem durumunu tespit etmek için kullanılır. Sistem durumu araştırmalarının sağlıksız eşiğini tanımlayabilirsiniz. Bir araştırma yanıtlanamazsa, yük dengeleyici sağlıksız örneklere yeni bağlantı göndermeyi durduruyor. Bir araştırma hatası varolan bağlantıları etkilemez. Bağlantı, uygulamaya kadar devam eder:

- Akışı sonlandırır
- Boşta kalma zaman aşımı oluştu
- VM kapanıyor

Load Balancer uç noktalar için farklı durum araştırma türleri sağlar:

- TCP
- HTTP
- HTTPS

Temel yük dengeleyici HTTPS araştırmalarını desteklemez. Temel yük dengeleyici tüm TCP bağlantılarını (kurulan bağlantılar dahil) kapatır.

## <a name="load-balancing-rules"></a>Yük Dengeleme kuralları

Yük Dengeleme kuralları yük dengeleyicisine ne yapılacağını söyler. Yük Dengeleme kuralı, belirli bir ön uç IP yapılandırmasını ve bağlantı noktasını birden fazla arka uç IP adresine ve bağlantı noktasına eşler.

## <a name="inbound-nat-rules"></a>Gelen NAT kuralları

Gelen NAT kuralları, trafiği ön uç IP adresinden sanal ağın içindeki bir arka uç örneğine iletir. Bağlantı noktası iletme, Yük Dengeleme ile aynı karma tabanlı dağıtım tarafından yapılır. 

Kullanım örneği, bir sanal ağ içindeki sanal makıne örneklerini ayırmak için Uzak Masaüstü Protokolü (RDP) veya Secure Shell (SSH) oturumlarından oluşur. Aynı ön uç IP adresindeki bağlantı noktalarıyla birden çok iç uç nokta eşlenebilir. Ön uç IP adresleri, ek bir sıçrama kutusu olmadan sanal makinelerinizi uzaktan yönetmek için kullanılabilir.

## <a name="outbound-rules"></a>Giden kuralları

Giden bir kural, arka uç havuzu tarafından tanımlanan tüm sanal makineler veya örnekler için giden ağ adresi çevirisi 'ni (NAT) yapılandırır.

Temel yük dengeleyici giden kuralları desteklemez.
![Azure Load Balancer](./media/load-balancer-overview/load-balancer-overview.png)

## <a name="next-steps"></a>Sonraki adımlar

- Load Balancer kullanmaya başlamak için bkz. [genel standart Load Balancer oluşturma](quickstart-load-balancer-standard-public-portal.md) .
- [Azure Load Balancer](load-balancer-overview.md)hakkında daha fazla bilgi edinin.
- [Genel IP adresi](https://docs.microsoft.com/azure/virtual-network/virtual-network-public-ip-address) hakkında bilgi edinin
- [Özel IP adresi](https://docs.microsoft.com/azure/virtual-network/virtual-network-ip-addresses-overview-arm#private-ip-addresses) hakkında bilgi edinin
- [Standart Load Balancer ve kullanılabilirlik alanları](load-balancer-standard-availability-zones.md)kullanma hakkında bilgi edinin.
- [Standart Load Balancer tanılama](load-balancer-standard-diagnostics.md)hakkında bilgi edinin.
- [Boşta durumunda TCP sıfırlaması](load-balancer-tcp-reset.md)hakkında bilgi edinin.
- [Ha bağlantı noktaları Yük Dengeleme kurallarıyla standart Load Balancer](load-balancer-ha-ports-overview.md)hakkında bilgi edinin.
- [Birden çok ön uç ile Load Balancer](load-balancer-multivip-overview.md)kullanma hakkında bilgi edinin.
- [Ağ güvenlik grupları](../virtual-network/security-overview.md)hakkında daha fazla bilgi edinin.
- [Araştırma türleri](load-balancer-custom-probe-overview.md#types)hakkında bilgi edinin.
- [Yük dengeleyici sınırları](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits#load-balancer)hakkında daha fazla bilgi edinin.
- [Bağlantı noktası iletmeyi](https://docs.microsoft.com/azure/load-balancer/tutorial-load-balancer-port-forwarding-portal)kullanma hakkında bilgi edinin.
- [Yük dengeleyici giden kuralları](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-rules-overview)hakkında daha fazla bilgi edinin.
