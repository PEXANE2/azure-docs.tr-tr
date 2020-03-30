---
title: ExpressRoute sanal ağ ağ geçitleri hakkında - Azure| Microsoft Dokümanlar
description: ExpressRoute için sanal ağ ağ geçitleri hakkında bilgi edinin. Bu makalede, ağ geçidi SUS'ları ve türleri hakkında bilgiler yer almaktadır.
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: conceptual
ms.date: 10/14/2019
ms.author: mialdrid
ms.openlocfilehash: 58e75e4efecf390c4c1449b7ec59684554fa7516
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79281424"
---
# <a name="about-expressroute-virtual-network-gateways"></a>ExpressRoute sanal ağ ağ geçitleri hakkında

Azure sanal ağınızı ve şirket içi ağınızı ExpressRoute üzerinden bağlamak için önce bir sanal ağ ağ geçidi oluşturmanız gerekir. Sanal ağ ağ geçidi iki amacla hizmet eder: ağlar ve rota ağı trafiği arasında IP yolları değişimi. Bu makalede, ağ geçidi türleri, ağ geçidi SKU'ları ve SKU tarafından tahmini performansı açıklanmaktadır. Bu makalede, performansı artırmak için sanal ağ ağ ağ geçidi atlamak için şirket içi ağ dan ağ trafiğini sağlayan bir özellik olan ExpressRoute [FastPath](#fastpath)açıklar.

## <a name="gateway-types"></a>Ağ geçidi türleri

Sanal ağ ağ geçidi oluşturduğunuzda, birkaç ayar belirtmeniz gerekir. Gerekli ayarlardan biri olan '-GatewayType', ağ geçidinin ExpressRoute veya VPN trafiği için kullanılıp kullanılmayacağını belirtir. İki ağ geçidi türü şunlardır:

* **Vpn** - Genel Internet üzerinden şifreli trafik göndermek için ağ geçidi türünü 'Vpn' kullanırsınız. Bu, VPN ağ geçidi olarak da adlandırılır. Siteden Siteye, Noktadan Siteye ve Sanal Ağdan Sanal Ağa bağlantıların tümü VPN ağ geçidi kullanır.

* **ExpressRoute** - Ağ trafiğini özel bir bağlantıya göndermek için 'ExpressRoute' ağ geçidi türünü kullanırsınız. Bu, ExpressRoute ağ geçidi olarak da adlandırılır ve ExpressRoute'u yapılandırırken kullanılan ağ geçidi türüdür.

Bir sanal ağın her ağ geçidi türü için yalnızca bir sanal ağ geçidi olabilir. Örneğin, GatewayType Vpn kullanan bir sanal ağ geçidiniz ve GatewayType ExpressRoute kullanan bir sanal ağ geçidiniz olabilir.

## <a name="gateway-skus"></a><a name="gwsku"></a>Ağ Geçidi SKU'ları
[!INCLUDE [expressroute-gwsku-include](../../includes/expressroute-gwsku-include.md)]

Ağ geçidinizi daha güçlü bir ağ geçidi olan SKU'ya yükseltmek istiyorsanız, çoğu durumda 'Resize-AzVirtualNetworkGateway' PowerShell cmdlet'ini kullanabilirsiniz. Bu, Standart ve Yüksek Performanslı SUS'lara yükseltmeler için çalışacaktır. Ancak, UltraPerformance SKU'ya yükseltmek için ağ geçidini yeniden oluşturmanız gerekir. Ağ geçidini yeniden oluşturmak kapalı kalma süresine neden oluyor.

### <a name="estimated-performances-by-gateway-sku"></a><a name="aggthroughput"></a>Ağ geçidi SKU tarafından tahmini performansları
Aşağıdaki tabloda ağ geçidi türleri ve tahmini performansları gösterilmektedir. Bu tablo hem Resource Manager, hem de klasik dağıtım modellerine uygulanır.

[!INCLUDE [expressroute-table-aggthroughput](../../includes/expressroute-table-aggtput-include.md)]

> [!IMPORTANT]
> Uygulama performansı, uçtan uca gecikme ve uygulamanın açtığı trafik akışları sayısı gibi birden çok etkene bağlıdır. Tablodaki sayılar, uygulamanın teorik olarak ideal bir ortamda elde edebileceği üst sınırı temsil eder.
>
>

## <a name="gateway-subnet"></a><a name="gwsub"></a>Ağ geçidi alt ağı

Bir ExpressRoute ağ geçidi oluşturmadan önce bir ağ geçidi alt ağı oluşturmanız gerekir. Ağ geçidi alt ağı, sanal ağ ağ geçidi VM'lerinin ve hizmetlerinin kullandığı IP adreslerini içerir. Sanal ağ ağ ağ geçidinizi oluşturduğunuzda, ağ geçidi VM'leri ağ geçidi alt ağına dağıtılır ve gerekli ExpressRoute ağ geçidi ayarlarıyla yapılandırılır. Ağ geçidi alt ağına başka bir şey (örneğin, ek VM'ler) dağıtmayın. Düzgün çalışması için ağ geçidi alt ağı 'GatewaySubnet' olarak adlandırılmalıdır. Ağ geçidi alt netini 'GatewaySubnet' olarak adlandırmak, Azure'un sanal ağ ağ vm'lerini ve hizmetlerini dağıtmak için alt ağ olduğunu bilmesini sağlar.

>[!NOTE]
>[!INCLUDE [vpn-gateway-gwudr-warning.md](../../includes/vpn-gateway-gwudr-warning.md)]
>

Ağ geçidi alt ağı oluştururken, alt ağın içerdiği IP adresi sayısını belirtirsiniz. Ağ geçidi alt netindeki IP adresleri ağ geçidi VM'lerine ve ağ geçidi hizmetlerine ayrılır. Bazı yapılandırmalar için diğerlerinden daha fazla IP adresi gerekir. 

Ağ geçidi alt ağ boyutunuzu planlarken, oluşturmayı planladığınız yapılandırma için belgelere bakın. Örneğin, ExpressRoute/VPN Ağ Geçidi birlikte var olan yapılandırma, diğer yapılandırmalardan daha büyük bir ağ geçidi alt ağı gerektirir. Ayrıca, ağ geçidi alt ağınızın gelecekteki olası ek yapılandırmaları barındıracak kadar IP adresi içerdiğinden emin olmak isteyebilirsiniz. /29 gibi küçük bir ağ geçidi alt ağı oluşturabilirsiniz, ancak bunu yapmak için kullanılabilir adres alanınız varsa /27 veya daha büyük (/27, /26 vb.) bir ağ geçidi alt ağı oluşturmanızı öneririz. Bu, çoğu yapılandırmayı barındırır.

Aşağıdaki Kaynak Yöneticisi PowerShell örneği, GatewaySubnet adlı bir ağ geçidi alt ağı gösterir. CIDR notasyonunun, şu anda var olan yapılandırmaların çoğu için yeterli IP adresine olanak tanıyan bir /27 belirtirolduğunu görebilirsiniz.

```azurepowershell-interactive
Add-AzVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.0.3.0/27
```

[!INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)]

### <a name="zone-redundant-gateway-skus"></a><a name="zrgw"></a>Bölge yedekli ağ geçidi SUS'lar

Azure Kullanılabilirlik Bölgelerinde ExpressRoute ağ geçitlerini de dağıtabilirsiniz. Bu, fiziksel ve mantıksal olarak onları farklı Kullanılabilirlik Bölgeleri'ne ayırır ve şirket içi ağ bağlantınızı Azure'a bölge düzeyindeki hatalardan korur.

![Bölge yedekli ExpressRoute ağ geçidi](./media/expressroute-about-virtual-network-gateways/zone-redundant.png)

Bölge yedekli ağ geçitleri, ExpressRoute ağ geçidi için belirli yeni ağ geçidi SUS'ları kullanır.

* ErGw1AZ
* Ergw2AZ
* Ergw3AZ

Yeni ağ geçidi STU'ları, gereksinimlerinize en iyi şekilde uyacak diğer dağıtım seçeneklerini de destekler. Yeni ağ geçidi STU'larını kullanarak sanal ağ ağ geçidi oluştururken, ağ geçidini belirli bir bölgede dağıtma seçeneğiniz de vardır. Buna zonal ağ geçidi denir. Bir bölge ağ geçidi dağıttığınızda, ağ geçidinin tüm örnekleri aynı Kullanılabilirlik Bölgesi'nde dağıtılır.

## <a name="fastpath"></a><a name="fastpath"></a>FastPath

ExpressRoute sanal ağ ağ geçidi, ağ yollarını ve ağ trafiğini yönlendirmek için tasarlanmıştır. FastPath, şirket içi ağınızla sanal ağınız arasındaki veri yolu performansını artırmak için tasarlanmıştır. FastPath etkinleştirildiğinde ağ trafiğini doğrudan sanal ağdaki sanal makinelere göndererek ağ geçidini atlar.

Sınırlamalar ve gereksinimler de dahil olmak üzere FastPath hakkında daha fazla bilgi için [FastPath hakkında](about-fastpath.md)bilgi edinebilirsiniz.

## <a name="rest-apis-and-powershell-cmdlets"></a><a name="resources"></a>REST API'ler ve PowerShell cmdlets
Sanal ağ ağ geçidi yapılandırmaları için REST API'leri ve PowerShell cmdlets kullanırken ek teknik kaynaklar ve belirli sözdizimi gereksinimleri için aşağıdaki sayfalara bakın:

| **Klasik** | **Kaynak Yöneticisi** |
| --- | --- |
| [Powershell](https://docs.microsoft.com/powershell/module/servicemanagement/azure/?view=azuresmps-4.0.0#azure) |[Powershell](https://docs.microsoft.com/powershell/module/az.network#networking) |
| [REST API](https://msdn.microsoft.com/library/jj154113.aspx) |[REST API](https://msdn.microsoft.com/library/mt163859.aspx) |

## <a name="next-steps"></a>Sonraki adımlar

Kullanılabilir bağlantı yapılandırmaları hakkında daha fazla bilgi için [ExpressRoute Genel Bakış'a](expressroute-introduction.md)bakın.

ExpressRoute ağ geçitleri oluşturma hakkında daha fazla bilgi için [bkz.](expressroute-howto-add-gateway-resource-manager.md)

Bölge artıklarının yapılandırılması hakkında daha fazla bilgi için [bkz.](../../articles/vpn-gateway/create-zone-redundant-vnet-gateway.md)

FastPath hakkında daha fazla bilgi için [FastPath hakkında](about-fastpath.md)bilgi için bkz.