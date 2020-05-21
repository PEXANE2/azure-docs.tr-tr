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
ms.openlocfilehash: 84857315e4b6b4375ed5b78520b4c6ff0d66751a
ms.sourcegitcommit: 50673ecc5bf8b443491b763b5f287dde046fdd31
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2020
ms.locfileid: "83684989"
---
# <a name="azure-load-balancer-components"></a>Azure Load Balancer bileşenleri

Azure Load Balancer birkaç temel bileşenden oluşur. Bunlar aboneliğinizde Azure portal, Azure CLı, Azure PowerShell veya şablonlar aracılığıyla yapılandırılabilir.

## <a name="frontend-ip-configuration"></a>Ön uç IP yapılandırması<a name = "frontend-ip-configurations"></a>

Azure Load Balancer IP adresi. Bu, istemciler için iletişim noktasıdır. Bu IP adresleri şunlardan biri olabilir:

- **Genel IP adresi**
- **Özel IP adresi**

IP adresinin doğası, oluşturulan yük dengeleyicinin **türünü** belirler. Özel IP adresi seçimi bir iç yük dengeleyici oluşturur. Genel IP adresi seçimi bir genel yük dengeleyici oluşturur.

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

Bir sistem durumu araştırması, arka uç havuzundaki örneklerin sistem durumunu tespit etmek için kullanılır. Bir Load Balancer oluştururken, bir örneğin sağlıklı olup olmadığını ve trafiği akışa almak için Load Balancer kullanabileceği bir sistem durumu araştırması yapılandırmanız gerekir.

Sistem durumu araştırmalarının sağlıksız eşiğini tanımlayabilirsiniz. Bir araştırma yanıtlanamazsa Load Balancer sağlıksız örneklere yeni bağlantı göndermeyi durduruyor. Bir araştırma hatası varolan bağlantıları etkilemez. Bağlantı, uygulamaya kadar devam eder:

- Akışı sonlandırır
- Boşta kalma zaman aşımı oluştu
- VM kapanıyor

Load Balancer uç noktalar için farklı durum araştırma türleri sağlar: TCP, HTTP ve HTTPS.

Temel Load Balancer HTTPS araştırmaları desteklemez. Temel Load Balancer tüm TCP bağlantılarını (kurulan bağlantılar dahil) kapatır.

## <a name="load-balancing-rules"></a>Yük Dengeleme kuralları

Bir Load Balancer kuralı, gelen trafiğin arka uç havuzundaki **Tüm** örneklere nasıl dağıtıldığını tanımlamak için kullanılır. Yük Dengeleme kuralı, belirli bir ön uç IP yapılandırmasını ve bağlantı noktasını birden fazla arka uç IP adresine ve bağlantı noktasına eşler.

Örneğin, ön uç IP 'nizin bağlantı noktası 80 ' deki (veya başka bir bağlantı noktası) trafik, tüm arka uç örneklerinizin 80 numaralı bağlantı noktasına yönlendirilmek istiyorsanız, bunu başarmak için bir yük dengeleme kuralı kullanırsınız.

## <a name="inbound-nat-rules"></a>Gelen NAT kuralları

Gelen NAT kuralı, seçilen bir ön uç IP adresine ve bağlantı noktası birleşimine gönderilen gelen trafiği, arka uç havuzundaki **belirli** bir sanal makineye veya örneğe iletir. Bağlantı noktası iletme, Yük Dengeleme ile aynı karma tabanlı dağıtım tarafından yapılır.

Örneğin, bir arka uç havuzundaki sanal makine örneklerinin Uzak Masaüstü Protokolü (RDP) veya Secure Shell (SSH) oturumlarını istiyorsanız. Aynı ön uç IP adresindeki bağlantı noktalarıyla birden çok iç uç nokta eşlenebilir. Ön uç IP adresleri, ek bir sıçrama kutusu olmadan sanal makinelerinizi uzaktan yönetmek için kullanılabilir.

## <a name="outbound-rules"></a>Giden kuralları

Giden bir kural, arka uç havuzu tarafından tanımlanan tüm sanal makineler veya örnekler için giden ağ adresi çevirisi 'ni (NAT) yapılandırır. Bu, arka uçtaki örneklerin internet veya diğer uç noktalara iletişim kurmasını sağlar.

Temel yük dengeleyici giden kuralları desteklemez.

## <a name="next-steps"></a>Sonraki adımlar

- Load Balancer kullanmaya başlamak için bkz. [genel standart Load Balancer oluşturma](quickstart-load-balancer-standard-public-portal.md) .
- [Azure Load Balancer](load-balancer-overview.md)hakkında daha fazla bilgi edinin.
- [Genel IP adresi](https://docs.microsoft.com/azure/virtual-network/virtual-network-public-ip-address) hakkında bilgi edinin
- [Özel IP adresi](https://docs.microsoft.com/azure/virtual-network/virtual-network-ip-addresses-overview-arm#private-ip-addresses) hakkında bilgi edinin
- [Standart Load Balancer ve kullanılabilirlik alanları](load-balancer-standard-availability-zones.md)kullanma hakkında bilgi edinin.
- [Standart Load Balancer tanılama](load-balancer-standard-diagnostics.md)hakkında bilgi edinin.
- [Boşta durumunda TCP sıfırlaması](load-balancer-tcp-reset.md)hakkında bilgi edinin.
- [Ha bağlantı noktaları Yük Dengeleme kurallarıyla standart Load Balancer](load-balancer-ha-ports-overview.md)hakkında bilgi edinin.
- [Birden çok ön uç IP yapılandırmasıyla Load Balancer](load-balancer-multivip-overview.md)kullanma hakkında bilgi edinin.
- [Ağ güvenlik grupları](../virtual-network/security-overview.md)hakkında daha fazla bilgi edinin.
- [Araştırma türleri](load-balancer-custom-probe-overview.md#types)hakkında bilgi edinin.
- [Yük dengeleyici sınırları](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits#load-balancer)hakkında daha fazla bilgi edinin.
- [Bağlantı noktası iletmeyi](https://docs.microsoft.com/azure/load-balancer/tutorial-load-balancer-port-forwarding-portal)kullanma hakkında bilgi edinin.
- [Yük dengeleyici giden kuralları](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-rules-overview)hakkında daha fazla bilgi edinin.
