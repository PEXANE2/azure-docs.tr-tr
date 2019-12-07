---
title: ExpressRoute sanal ağ geçitleri - Azure hakkında | Microsoft Docs
description: ExpressRoute için sanal ağ geçitleri hakkında öğrenin. Bu makalede, ağ geçidi SKU'ları ve türler hakkında bilgi içerir.
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: conceptual
ms.date: 10/14/2019
ms.author: mialdrid
ms.openlocfilehash: 58e75e4efecf390c4c1449b7ec59684554fa7516
ms.sourcegitcommit: 8bd85510aee664d40614655d0ff714f61e6cd328
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2019
ms.locfileid: "74894368"
---
# <a name="about-expressroute-virtual-network-gateways"></a>ExpressRoute sanal ağ geçitleri hakkında

Azure Sanal ağınızı ve şirket içi ağınızı ExpressRoute aracılığıyla bağlamak için önce bir sanal ağ geçidi oluşturmanız gerekir. Bir sanal ağ geçidi iki amaca hizmet eder: ağlar arasında Exchange IP yolları ve ağ trafiği yönlendirme. Bu makalede, SKU 'ya göre ağ geçidi türleri, ağ geçidi SKU 'Ları ve tahmini performans açıklanmaktadır. Bu makalede ayrıca, şirket içi ağınızdan gelen ağ trafiğinin performansı artırmak için sanal ağ geçidini atlaması sağlayan bir özellik olan ExpressRoute [FastPath](#fastpath)açıklanır.

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

## <a name="gwsub"></a>Ağ geçidi alt ağı

ExpressRoute ağ geçidi oluşturmadan önce bir ağ geçidi alt ağı oluşturmanız gerekir. Ağ geçidi alt ağı, sanal ağ geçidi VM 'lerinin ve hizmetlerinin kullandığı IP adreslerini içerir. Sanal ağ geçidinizi oluşturduğunuzda, ağ geçidi VM 'Leri ağ geçidi alt ağına dağıtılır ve gerekli ExpressRoute ağ geçidi ayarlarıyla yapılandırılır. Ağ geçidi alt ağına hiçbir şeyi başka hiçbir şekilde (örneğin, ek VM 'Ler) dağıtmayın. Düzgün çalışması için ağ geçidi alt ağının ' GatewaySubnet ' olarak adlandırılması gerekir. Ağ geçidi alt ağının ' GatewaySubnet ' olarak adlandırılması, Azure 'un sanal ağ geçidi VM 'lerini ve hizmetlerini dağıtmak için bu alt ağ olduğunu bilmesini sağlar.

>[!NOTE]
>[!INCLUDE [vpn-gateway-gwudr-warning.md](../../includes/vpn-gateway-gwudr-warning.md)]
>

Ağ geçidi alt ağı oluştururken, alt ağın içerdiği IP adresi sayısını belirtirsiniz. Ağ geçidi alt ağındaki IP adresleri ağ geçidi VM 'lerine ve ağ geçidi hizmetlerine ayrılır. Bazı yapılandırmalar için diğerlerinden daha fazla IP adresi gerekir. 

Ağ geçidi alt ağınızın boyutunu planlarken, oluşturmayı planladığınız yapılandırma için belgelere bakın. Örneğin, ExpressRoute/VPN Gateway bir arada bulunan yapılandırma, diğer birçok yapılandırmadan daha büyük bir ağ geçidi alt ağı gerektirir. Ayrıca, ağ geçidi alt ağınızın olası gelecekteki ek yapılandırmalara uyum sağlamak için yeterli IP adresi içerdiğinden emin olmak isteyebilirsiniz. /29 kadar küçük bir ağ geçidi alt ağı oluşturabileceğiniz gibi, kullanılabilir adres alanınız varsa/27 veya daha büyük (/27,/26 vb.) bir ağ geçidi alt ağı oluşturmanızı öneririz. Bu, çoğu yapılandırmaya uyum sağlayacaktır.

Aşağıdaki Kaynak Yöneticisi PowerShell örneği, GatewaySubnet adlı bir ağ geçidi alt ağını göstermektedir. CıDR gösteriminin bir/27 olduğunu görebilirsiniz. Bu, şu anda mevcut olan çoğu yapılandırma için yeterli IP adresi sağlar.

```azurepowershell-interactive
Add-AzVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.0.3.0/27
```

[!INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)]

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

Sınırlamalar ve gereksinimler de dahil olmak üzere FastPath hakkında daha fazla bilgi için bkz. [FastPath hakkında](about-fastpath.md).

## <a name="resources"></a>REST API ve PowerShell cmdlet'leri
Ek teknik kaynaklar ve sanal ağ geçidi yapılandırması için REST API'ler ve PowerShell cmdlet'lerini kullanırken, belirli bir söz dizimi gereksinimler için şu sayfalara bakın:

| **Klasik** | **Resource Manager** |
| --- | --- |
| [PowerShell](https://docs.microsoft.com/powershell/module/servicemanagement/azure/?view=azuresmps-4.0.0#azure) |[PowerShell](https://docs.microsoft.com/powershell/module/az.network#networking) |
| [REST API](https://msdn.microsoft.com/library/jj154113.aspx) |[REST API](https://msdn.microsoft.com/library/mt163859.aspx) |

## <a name="next-steps"></a>Sonraki adımlar

Kullanılabilir bağlantı yapılandırması hakkında daha fazla bilgi için bkz. [ExpressRoute 'A genel bakış](expressroute-introduction.md).

ExpressRoute ağ geçitleri oluşturma hakkında daha fazla bilgi için bkz. [ExpressRoute için sanal ağ geçidi oluşturma](expressroute-howto-add-gateway-resource-manager.md).

Bölge yedekli ağ geçitlerini yapılandırma hakkında daha fazla bilgi için bkz. bölgesel olarak [yedekli sanal ağ geçidi oluşturma](../../articles/vpn-gateway/create-zone-redundant-vnet-gateway.md).

FastPath hakkında daha fazla bilgi için bkz. [FastPath hakkında](about-fastpath.md).