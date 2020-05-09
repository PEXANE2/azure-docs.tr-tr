---
title: Azure Relay için ağ güvenliği
description: Bu makalede özel uç noktalardan erişimin nasıl yapılandırılacağı açıklanmaktadır
services: service-bus-relay
author: spelluru
ms.service: service-bus-relay
ms.topic: conceptual
ms.date: 05/07/2020
ms.author: spelluru
ms.openlocfilehash: d0a0d03680877ae78f5af939f8d47e4e426abadb
ms.sourcegitcommit: 999ccaf74347605e32505cbcfd6121163560a4ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2020
ms.locfileid: "82984778"
---
# <a name="network-security-for-azure-relay"></a>Azure Relay için ağ güvenliği 
Bu makalede, Azure Relay ile aşağıdaki güvenlik özelliklerinin nasıl kullanılacağı açıklanmaktadır: 

- IP güvenlik duvarı kuralları (Önizleme)
- Özel uç noktalar (Önizleme)

> [!NOTE]
> Azure Relay, ağ hizmeti uç noktalarını desteklemez. 


## <a name="ip-firewall"></a>IP güvenlik duvarı 
Varsayılan olarak, istek geçerli kimlik doğrulaması ve yetkilendirmeyle geldiği sürece, geçiş ad alanlarına internet 'ten erişilebilir. IP güvenlik duvarı ile, [CIDR (sınıfsız etki alanları arası yönlendirme)](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing) gösteriminde yalnızca bir dizi IPv4 adresi veya IPv4 adres aralığı ile sınırlayabilirsiniz.

Bu özellik, Azure Relay yalnızca belirli bilinen sitelerden erişilebilir olması gereken senaryolarda yararlıdır. Güvenlik duvarı kuralları, belirli IPv4 adreslerinden kaynaklanan trafiği kabul etmek için kuralları yapılandırmanızı sağlar. Örneğin, [Azure Express Route](/azure/expressroute/expressroute-faqs#supported-services)ile geçiş kullanıyorsanız, yalnızca şirket ıçı altyapı IP adreslerinden gelen trafiğe izin vermek için bir **güvenlik duvarı kuralı** oluşturabilirsiniz. 

IP güvenlik duvarı kuralları, geçiş ad alanı düzeyinde uygulanır. Bu nedenle, kurallar desteklenen herhangi bir protokolü kullanarak istemcilerden gelen tüm bağlantılara uygulanır. Geçiş ad alanındaki izin verilen bir IP kuralıyla eşleşmeyen bir IP adresinden gelen bağlantı girişimleri yetkisiz olarak reddedilir. Yanıt, IP kuralından bahsetmiyor. IP filtresi kuralları sırasıyla uygulanır ve IP adresiyle eşleşen ilk kural kabul etme veya reddetme eylemini belirler.

Daha fazla bilgi için bkz. [bir geçiş ad alanı IÇIN IP güvenlik duvarını yapılandırma](ip-firewall-virtual-networks.md)

## <a name="private-endpoints"></a>Özel uç noktalar

Azure **özel bağlantı hizmeti** , Azure hizmetlerine (örneğin, Azure Relay, Azure Service Bus, Azure Event Hubs, Azure depolama ve Azure Cosmos DB) ve Azure 'da barındırılan müşteri/iş ortağı hizmetlerine sanal ağınızdaki özel bir uç nokta üzerinden erişmenizi sağlar. Daha fazla bilgi için bkz. [Azure özel bağlantısı (Önizleme) nedir?](../private-link/private-link-overview.md)

**Özel uç nokta** , bir sanal ağda çalışan iş yüklerinizin **özel bir bağlantı kaynağına** (örneğin, bir geçiş ad alanı) sahip olan bir hizmete özel ve güvenli bir şekilde bağlanmasına olanak tanıyan bir ağ arabirimidir. Özel uç nokta, sanal ağınızdan bir özel IP adresi kullanarak hizmeti sanal ağınıza etkin bir şekilde getiriyor. Hizmete giden tüm trafik özel uç nokta aracılığıyla yönlendirilebilir, bu nedenle ağ geçitleri, NAT cihazları, ExpressRoute, VPN bağlantıları veya genel IP adresleri gerekmez. Sanal ağınız ve hizmet arasındaki trafik, Microsoft omurga ağı üzerinden, genel Internet 'ten etkilenme olasılığını ortadan kaldırır. Belirli Azure Relay ad alanlarına bağlantılara izin vererek, erişim denetiminde ayrıntı düzeyi düzeyi sağlayabilirsiniz.

> [!NOTE]
> Bu özellik şu anda **Önizleme**aşamasındadır. 

Daha fazla bilgi için bkz. [Özel uç noktaları yapılandırma](private-link-service.md)


## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makalelere bakın:

- [IP güvenlik duvarını yapılandırma](ip-firewall-virtual-networks.md)
- [Özel uç noktaları yapılandırma](private-link-service.md)