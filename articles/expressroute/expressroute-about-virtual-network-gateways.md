---
title: ExpressRoute sanal ağ geçitleri - Azure hakkında | Microsoft Docs
description: ExpressRoute için sanal ağ geçitleri hakkında öğrenin. Bu makalede, ağ geçidi SKU'ları ve türler hakkında bilgi içerir.
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: conceptual
ms.date: 09/04/2019
ms.author: mialdrid
ms.custom: seodec18
ms.openlocfilehash: 5b74e387c6bee58acbbb7bae320a9bc72a4dda1c
ms.sourcegitcommit: aebe5a10fa828733bbfb95296d400f4bc579533c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/05/2019
ms.locfileid: "70376287"
---
# <a name="expressroute-virtual-network-gateway-and-fastpath"></a>ExpressRoute sanal ağ geçidi ve FastPath
Azure Sanal ağınızı ve şirket içi ağınızı ExpressRoute aracılığıyla bağlamak için önce bir sanal ağ geçidi oluşturmanız gerekir. Bir sanal ağ geçidi iki amaca hizmet eder: ağlar arasında Exchange IP yolları ve ağ trafiği yönlendirme. Bu makalede, ağ geçidi türleri, ağ geçidi SKU 'Ları ve SKU 'nun tahmini performansı açıklanmaktadır. Bu makalede ayrıca, şirket içi ağınızdan gelen ağ trafiğinin performansı artırmak için sanal ağ geçidini atlaması sağlayan bir özellik olan ExpressRoute [FastPath](#fastpath)açıklanır.

## <a name="gateway-types"></a>Ağ geçidi türleri

Bir sanal ağ geçidi oluştururken birkaç ayar yapılandırırsınız gerekir. Gerekli ayarlardan biri '-GatewayType', ağ geçidini ExpressRoute için kullanılıp kullanılmayacağını belirtir ya da VPN trafiği. İki ağ geçidi türleri şunlardır:

* **VPN** - genel Internet şifrelenmiş trafik göndermek için 'Vpn' ağ geçidi türünü kullanın. Bu VPN ağ geçidi olarak da adlandırılır. Siteden Siteye, Noktadan Siteye ve Sanal Ağdan Sanal Ağa bağlantıların tümü VPN ağ geçidi kullanır.

* **ExpressRoute** - özel bir bağlantı üzerinde ağ trafiği göndermek için 'ExpressRoute' ağ geçidi türünü kullanın. Bu, bir ExpressRoute ağ geçidi olarak da adlandırılır ve ExpressRoute yapılandırma sırasında kullanılan ağ geçidi türüdür.

Bir sanal ağın her ağ geçidi türü için yalnızca bir sanal ağ geçidi olabilir. Örneğin, GatewayType Vpn kullanan bir sanal ağ geçidiniz ve GatewayType ExpressRoute kullanan bir sanal ağ geçidiniz olabilir.

## <a name="gwsku"></a>Ağ Geçidi SKU'ları
[!INCLUDE [expressroute-gwsku-include](../../includes/expressroute-gwsku-include.md)]

Ağ geçidinizin daha güçlü bir ağ geçidi SKU 'suna yükseltmek istiyorsanız, çoğu durumda ' Resize-AzVirtualNetworkGateway ' PowerShell cmdlet 'ini kullanabilirsiniz. Bu, standart ve yüksek performanslı SKU'lar yükseltmeleri için çalışır. Ancak, UltraPerformance SKU'su için yükseltmek için ağ geçidini yeniden oluşturmanız gerekecektir. Bir ağ geçidi yeniden kapalı kalma süresi artmasına neden olur.

### <a name="aggthroughput"></a>Ağ geçidi SKU'suna göre tahmini performans
Aşağıdaki tabloda, ağ geçidi türleri ve tahmini performanslarını gösterir. Bu tablo hem Resource Manager, hem de klasik dağıtım modellerine uygulanır.

[!INCLUDE [expressroute-table-aggthroughput](../../includes/expressroute-table-aggtput-include.md)]

> [!IMPORTANT]
> Uygulama performansını uçtan uca gecikme süresi ve trafik akışları uygulama açılır sayısı gibi birden çok etkene bağlıdır. Tablo sayılar uygulama teorik olarak ideal bir ortam elde edebilirsiniz üst sınırını ifade eder.
>
>

### <a name="zrgw"></a>Bölgesel olarak yedekli ağ geçidi SKU'ları

Azure kullanılabilirlik alanları, ExpressRoute ağ geçitleri de dağıtabilirsiniz. Bu fiziksel ve mantıksal olarak bunları farklı kullanılabilirlik alanları, şirket içi ağ bağlantınızı bölge düzeyinde hatalardan Azure'a koruma halinde ayırır.

![Bölgesel olarak yedekli ExpressRoute ağ geçidi](./media/expressroute-about-virtual-network-gateways/zone-redundant.png)

Bölgesel olarak yedekli ağ geçitleri, ExpressRoute ağ geçidi için belirli yeni ağ geçidi SKU'ları kullanın.

* ErGw1AZ
* ErGw2AZ
* ErGw3AZ

Yeni ağ geçidi SKU'ları, en iyi sonucu gereksinimlerinize uyacak şekilde diğer dağıtım seçenekleri de destekler. Yeni ağ geçidi SKU'ları kullanarak bir sanal ağ geçidi oluştururken, aynı zamanda belirli bir bölgenin ağ geçidi dağıtmak için seçeneğiniz vardır. Bu, bölgesel bir ağ geçidi olarak adlandırılır. Bölgesel bir ağ geçidi dağıttığınızda, ağ geçidinin tüm örnekleri aynı kullanılabilirlik alanında dağıtılır.

## <a name="fastpath"></a>FastPath
ExpressRoute sanal ağ geçidi, ağ yollarını Exchange ve ağ trafiğini yönlendirme için tasarlanmıştır. FastPath, şirket içi ağınız ve sanal ağınız arasındaki veri yolu performansını geliştirmek için tasarlanmıştır. Etkin olduğunda, FastPath ağ trafiğini, ağ geçidini atlayarak sanal ağdaki sanal makinelere doğrudan gönderir. 

FastPath yalnızca [ExpressRoute doğrudan](expressroute-erdirect-about.md) üzerinden kullanılabilir. Diğer bir deyişle, bu özelliği yalnızca [Sanal ağınızı](expressroute-howto-linkvnet-arm.md) ExpressRoute doğrudan bağlantı noktasında oluşturulmuş bir ExpressRoute devresine bağladığınızda etkinleştirebilirsiniz. FastPath, sanal ağ ile şirket içi ağ arasındaki yolların değişimi için bir sanal ağ geçidinin oluşturulmasını gerektirir. Sanal ağ geçidi, Ultra Performance veya ErGw3AZ olmalıdır.

FastPath aşağıdaki özellikleri desteklemez:
* UDR on Gateway alt ağı: sanal ağınızın ağ geçidi alt ağına UDR uygularsanız, şirket içi ağınızdan gelen ağ trafiği sanal ağ geçidine gönderilmeye devam edecektir.
* VNet eşlemesi: ExpressRoute 'a bağlı başka sanal ağlarınız varsa, şirket içi ağınızdan diğer sanal ağlara (yani "bağlı bileşen" sanal ağları) ağ trafiğini, sanal ağa gönderilmeye devam eder geçidinde. Geçici çözüm tüm sanal ağları ExpressRoute devresine doğrudan bağlamak olur.
* Temel Load Balander: sanal ağınıza temel bir iç yük dengeleyici dağıtırsanız veya sanal ağınızda dağıttığınız Azure PaaS hizmeti temel bir iç yük dengeleyici kullanıyorsa, şirket içi ağınızdan gelen ağ trafiği, üzerinde barındırılan sanal IP 'lere Temel yük dengeleyici sanal ağ geçidine gönderilir. Çözüm, temel yük dengeleyiciyi [Standart yük dengeleyiciye](https://docs.microsoft.com/en-us/azure/load-balancer/load-balancer-overview)yükseltmeye yönelik bir çözümdür. 
 
## <a name="resources"></a>REST API ve PowerShell cmdlet'leri
Ek teknik kaynaklar ve sanal ağ geçidi yapılandırması için REST API'ler ve PowerShell cmdlet'lerini kullanırken, belirli bir söz dizimi gereksinimler için şu sayfalara bakın:

| **Klasik** | **Resource Manager** |
| --- | --- |
| [PowerShell](https://docs.microsoft.com/powershell/module/servicemanagement/azure/?view=azuresmps-4.0.0#azure) |[PowerShell](https://docs.microsoft.com/powershell/module/az.network#networking) |
| [REST API](https://msdn.microsoft.com/library/jj154113.aspx) |[REST API](https://msdn.microsoft.com/library/mt163859.aspx) |

## <a name="next-steps"></a>Sonraki adımlar
Bkz: [Expressroute'a genel bakış](expressroute-introduction.md) kullanılabilir bağlantı yapılandırmaları hakkında daha fazla bilgi.

Bkz: [ExpressRoute için sanal ağ geçidi oluşturma](expressroute-howto-add-gateway-resource-manager.md) ExpressRoute ağ geçitleri oluşturma hakkında daha fazla bilgi için.

Bkz: [bölgesel olarak yedekli sanal ağ geçidi oluşturma](../../articles/vpn-gateway/create-zone-redundant-vnet-gateway.md) bölgesel olarak yedekli ağ geçitlerini yapılandırma hakkında daha fazla bilgi.

FastPath 'in nasıl etkinleştirileceği hakkında daha fazla bilgi için bkz. [sanal ağı ExpressRoute 'A bağlama](expressroute-howto-linkvnet-arm.md) . 
