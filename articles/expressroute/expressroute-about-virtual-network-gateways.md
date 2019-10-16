---
title: ExpressRoute sanal ağ geçitleri hakkında-Azure | Microsoft Docs
description: ExpressRoute için sanal ağ geçitleri hakkında bilgi edinin. Bu makale, ağ geçidi SKU 'Ları ve türleri hakkında bilgi içerir.
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: conceptual
ms.date: 10/14/2019
ms.author: mialdrid
ms.custom: seodec18
ms.openlocfilehash: ba03d643c8d3770da60d4225d6c2b84d2a07766f
ms.sourcegitcommit: 1d0b37e2e32aad35cc012ba36200389e65b75c21
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/15/2019
ms.locfileid: "72325529"
---
# <a name="expressroute-virtual-network-gateway-and-fastpath"></a>ExpressRoute sanal ağ geçidi ve FastPath
Azure Sanal ağınızı ve şirket içi ağınızı ExpressRoute aracılığıyla bağlamak için önce bir sanal ağ geçidi oluşturmanız gerekir. Bir sanal ağ geçidi iki amaca hizmet eder: ağlar arasında Exchange IP yolları ve ağ trafiği yönlendirme. Bu makalede, ağ geçidi türleri, ağ geçidi SKU 'Ları ve SKU 'nun tahmini performansı açıklanmaktadır. Bu makalede ayrıca, şirket içi ağınızdan gelen ağ trafiğinin performansı artırmak için sanal ağ geçidini atlaması sağlayan bir özellik olan ExpressRoute [FastPath](#fastpath)açıklanır.

## <a name="gateway-types"></a>Ağ geçidi türleri

Bir sanal ağ geçidi oluşturduğunuzda, birkaç ayar belirtmeniz gerekir. Gerekli ayarlardan biri olan '-GatewayType ', ağ geçidinin ExpressRoute veya VPN trafiği için kullanılıp kullanılmayacağını belirtir. İki ağ geçidi türü şunlardır:

* **VPN** -genel Internet üzerinden şifrelenmiş trafik göndermek Için ' VPN ' ağ geçidi türünü kullanırsınız. Bu, VPN ağ geçidi olarak da adlandırılır. Siteden Siteye, Noktadan Siteye ve Sanal Ağdan Sanal Ağa bağlantıların tümü VPN ağ geçidi kullanır.

* **ExpressRoute** -özel bir bağlantıda ağ trafiği göndermek Için ' ExpressRoute ' ağ geçidi türünü kullanırsınız. Bu, ExpressRoute ağ geçidi olarak da adlandırılır ve ExpressRoute yapılandırılırken kullanılan ağ geçidi türüdür.

Bir sanal ağın her ağ geçidi türü için yalnızca bir sanal ağ geçidi olabilir. Örneğin, GatewayType Vpn kullanan bir sanal ağ geçidiniz ve GatewayType ExpressRoute kullanan bir sanal ağ geçidiniz olabilir.

## <a name="gwsku"></a>Ağ Geçidi SKU'ları
[!INCLUDE [expressroute-gwsku-include](../../includes/expressroute-gwsku-include.md)]

Ağ geçidinizin daha güçlü bir ağ geçidi SKU 'suna yükseltmek istiyorsanız, çoğu durumda ' Resize-AzVirtualNetworkGateway ' PowerShell cmdlet 'ini kullanabilirsiniz. Bu, standart ve HighPerformance SKU 'Larına yükseltmeler için çalışacaktır. Ancak, UltraPerformance SKU 'suna yükseltmek için, ağ geçidini yeniden oluşturmanız gerekecektir. Bir ağ geçidinin yeniden oluşturulması kapalı kalma süresini doğurur.

### <a name="aggthroughput"></a>Ağ Geçidi SKU 'SU tarafından tahmini performanons
Aşağıdaki tabloda ağ geçidi türleri ve tahmini performanons gösterilmektedir. Bu tablo hem Resource Manager, hem de klasik dağıtım modellerine uygulanır.

[!INCLUDE [expressroute-table-aggthroughput](../../includes/expressroute-table-aggtput-include.md)]

> [!IMPORTANT]
> Uygulama performansı, uçtan uca gecikme süresi ve uygulamanın açtığı trafik akışı sayısı gibi birden çok etkene bağlıdır. Tablodaki sayılar, uygulamanın teorik olarak ideal bir ortamda elde edilebileceği üst sınırı temsil eder.
>
>

### <a name="zrgw"></a>Bölge yedekli ağ geçidi SKU 'Ları

Ayrıca, ExpressRoute ağ geçitlerini Azure Kullanılabilirlik Alanları de dağıtabilirsiniz. Bu fiziksel ve mantıksal olarak farklı Kullanılabilirlik Alanları ayırır ve şirket içi ağ bağlantınızı bölge düzeyindeki hatalardan Azure ile koruyabilirsiniz.

![Bölge yedekli ExpressRoute ağ geçidi](./media/expressroute-about-virtual-network-gateways/zone-redundant.png)

Bölgesel olarak yedekli ağ geçitleri ExpressRoute ağ geçidi için özel yeni ağ geçidi SKU 'Ları kullanır.

* ErGw1AZ
* ErGw2AZ
* ErGw3AZ

Yeni ağ geçidi SKU 'Ları, gereksinimlerinize en iyi şekilde eşleşen diğer dağıtım seçeneklerini de destekler. Yeni ağ geçidi SKU 'Larını kullanarak bir sanal ağ geçidi oluştururken, ağ geçidini belirli bir bölgeye dağıtma seçeneğiniz de vardır. Bu, bir bölgesel ağ geçidi olarak adlandırılır. Bir bölgesel ağ geçidi dağıttığınızda, ağ geçidinin tüm örnekleri aynı Kullanılabilirlik alanına dağıtılır.

## <a name="fastpath"></a>FastPath
ExpressRoute sanal ağ geçidi, ağ yollarını Exchange ve ağ trafiğini yönlendirme için tasarlanmıştır. FastPath, şirket içi ağınız ve sanal ağınız arasındaki veri yolu performansını geliştirmek için tasarlanmıştır. Etkin olduğunda, FastPath ağ trafiğini, ağ geçidini atlayarak sanal ağdaki sanal makinelere doğrudan gönderir. 

FastPath tüm ExpressRoute devrelerinin üzerinde kullanılabilir. Yine de sanal ağ ile şirket içi ağ arasındaki yolların değişimi için bir sanal ağ geçidi oluşturulması gerekir. Sanal ağ geçidi, Ultra Performance veya ErGw3AZ olmalıdır.

FastPath aşağıdaki özellikleri desteklemez:
* UDR on Gateway alt ağı: sanal ağınızın ağ geçidi alt ağına UDR uygularsanız, şirket içi ağınızdan gelen ağ trafiği sanal ağ geçidine gönderilmeye devam edecektir.
* VNet eşlemesi: ExpressRoute 'a bağlı başka sanal ağlarınız varsa, şirket içi ağınızdan diğer sanal ağlara (yani "bağlı bileşen" sanal ağları) ağ trafiğini, sanal ağa gönderilmeye devam eder geçidinde. Geçici çözüm tüm sanal ağları ExpressRoute devresine doğrudan bağlamak olur.
* Temel Load Balancer: sanal ağınızda temel bir iç yük dengeleyici dağıtırsanız veya sanal ağınızda dağıttığınız Azure PaaS hizmeti temel bir iç yük dengeleyici kullanıyorsa, şirket içi ağınızdan gelen ağ trafiği, üzerinde barındırılan sanal IP 'lere Temel yük dengeleyici sanal ağ geçidine gönderilir. Çözüm, temel yük dengeleyiciyi [Standart yük dengeleyiciye](https://docs.microsoft.com/en-us/azure/load-balancer/load-balancer-overview)yükseltmeye yönelik bir çözümdür.
* Özel bağlantı: şirket içi ağınızdan sanal ağınızdaki [özel bir uç noktaya](../private-link/private-link-overview.md) bağlanırsanız bağlantı, sanal ağ geçidi üzerinden geçer.
 
## <a name="resources"></a>REST API 'leri ve PowerShell cmdlet 'leri
Sanal ağ geçidi yapılandırmalarına yönelik REST API 'Leri ve PowerShell cmdlet 'lerini kullanırken ek teknik kaynaklar ve özel sözdizimi gereksinimleri için aşağıdaki sayfalara bakın:

| **Klasik** | **Resource Manager** |
| --- | --- |
| [PowerShell](https://docs.microsoft.com/powershell/module/servicemanagement/azure/?view=azuresmps-4.0.0#azure) |[PowerShell](https://docs.microsoft.com/powershell/module/az.network#networking) |
| [REST API](https://msdn.microsoft.com/library/jj154113.aspx) |[REST API](https://msdn.microsoft.com/library/mt163859.aspx) |

## <a name="next-steps"></a>Sonraki adımlar
Kullanılabilir bağlantı yapılandırması hakkında daha fazla bilgi için bkz. [ExpressRoute 'A genel bakış](expressroute-introduction.md) .

ExpressRoute ağ geçitleri oluşturma hakkında daha fazla bilgi için bkz. [ExpressRoute için sanal ağ geçidi oluşturma](expressroute-howto-add-gateway-resource-manager.md) .

Bölge yedekli ağ geçitlerini yapılandırma hakkında daha fazla bilgi için bkz. bölgesel olarak [yedekli sanal ağ geçidi oluşturma](../../articles/vpn-gateway/create-zone-redundant-vnet-gateway.md) .

FastPath 'in nasıl etkinleştirileceği hakkında daha fazla bilgi için bkz. [sanal ağı ExpressRoute 'A bağlama](expressroute-howto-linkvnet-arm.md) . 
