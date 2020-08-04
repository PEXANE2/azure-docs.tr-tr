---
title: Azure Relay için ağ güvenliği
description: Bu makalede özel uç noktalardan erişimin nasıl yapılandırılacağı açıklanmaktadır
ms.topic: conceptual
ms.date: 06/23/2020
ms.openlocfilehash: bf49eff29385b5b72639420416df87b9187845e8
ms.sourcegitcommit: 3d56d25d9cf9d3d42600db3e9364a5730e80fa4a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2020
ms.locfileid: "87532994"
---
# <a name="network-security-for-azure-relay"></a>Azure Relay için ağ güvenliği 
Bu makalede, Azure Relay ile aşağıdaki güvenlik özelliklerinin nasıl kullanılacağı açıklanmaktadır: 

- IP güvenlik duvarı kuralları (Önizleme)
- Özel uç noktalar (Önizleme)

> [!NOTE]
> Azure Relay, ağ hizmeti uç noktalarını desteklemez. 


## <a name="ip-firewall"></a>IP güvenlik duvarı 
Varsayılan olarak, istek geçerli kimlik doğrulaması ve yetkilendirmeyle geldiği sürece, geçiş ad alanlarına internet 'ten erişilebilir. IP güvenlik duvarı ile, [CIDR (sınıfsız etki alanları arası yönlendirme)](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing) gösteriminde yalnızca bir dizi IPv4 adresi veya IPv4 adres aralığı ile sınırlayabilirsiniz.

Bu özellik, Azure Relay yalnızca belirli bilinen sitelerden erişilebilir olması gereken senaryolarda yararlıdır. Güvenlik duvarı kuralları, belirli IPv4 adreslerinden kaynaklanan trafiği kabul etmek için kuralları yapılandırmanızı sağlar. Örneğin, [Azure Express Route](../expressroute/expressroute-faqs.md#supported-services)ile geçiş kullanıyorsanız, yalnızca şirket ıçı altyapı IP adreslerinden gelen trafiğe izin vermek için bir **güvenlik duvarı kuralı** oluşturabilirsiniz. 

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
