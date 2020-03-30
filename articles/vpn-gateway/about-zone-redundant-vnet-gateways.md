---
title: Azure Kullanılabilirlik Bölgelerinde bölge gereksiz sanal ağ ağ geçitleri hakkında
description: Kullanılabilirlik BölgelerindeVPN Ağ Geçidi ve ExpressRoute ağ geçitleri hakkında bilgi edinin.
titleSuffix: Azure VPN Gateway
services: vpn-gateway
author: cherylmc
Customer intent: As someone with a basic network background, I want to understand zone-redundant gateways.
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 12/05/2019
ms.author: cherylmc
ms.openlocfilehash: f1bbaab99b6422de4053839e2099869d2d08db95
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75864307"
---
# <a name="about-zone-redundant-virtual-network-gateways-in-azure-availability-zones"></a>Azure Kullanılabilirlik Bölgelerinde bölge gereksiz sanal ağ ağ geçitleri hakkında

[Azure Kullanılabilirlik Bölgelerinde](../availability-zones/az-overview.md)VPN ve ExpressRoute ağ geçitlerini dağıtabilirsiniz. Bu seçenek, sanal ağ geçitlerine dayanıklılık, ölçeklenebilirlik ve daha yüksek kullanılabilirlik getirir. Ağ geçitlerini Azure Kullanılabilirlik Alanları içinde dağıtmak, bir bölge içindeki ağ geçitlerini fiziksel ve mantıksal olarak birbirinden ayırırken, Azure ile şirket içi ağ bağlantınızı alan düzeyindeki hatalardan korur.

### <a name="zone-redundant-gateways"></a><a name="zrgw"></a>Alanlar arası yedekli ağ geçitleri

Sanal ağ ağ ağlarınızı kullanılabilirlik bölgeleri arasında otomatik olarak dağıtmak için, bölge yedekli sanal ağ ağ geçitlerini kullanabilirsiniz. Bölge artıklı ağ geçitleri ile, Azure'da görev açısından kritik, ölçeklenebilir hizmetlerinize erişmek için bölge esnekliğinden yararlanabilirsiniz.

<br>
<br>

![bölge yedekli ağ geçitleri grafiği](./media/create-zone-redundant-vnet-gateway/zonered.png)

### <a name="zonal-gateways"></a><a name="zgw"></a>Zonal ağ geçitleri

Belirli bir bölgede ağ geçitleri dağıtmak için bölge ağ geçitlerini kullanabilirsiniz. Bir bölge ağ geçidi dağıttığınızda, ağ geçidinin tüm örnekleri aynı Kullanılabilirlik Bölgesi'nde dağıtılır.

<br>
<br>

![zonal ağ geçitleri grafik](./media/create-zone-redundant-vnet-gateway/zonal.png)

## <a name="gateway-skus"></a><a name="gwskus"></a>Ağ Geçidi SKU'ları

Bölge yedekli ve bölge ağ geçitleri yeni ağ geçidi STU'ları olarak kullanılabilir. Azure AZ bölgelerine yeni sanal ağ ağ ağ geçidi STU'ları ekledik. Bu SNU'lar, bölge gereksiz ve zonal ağ geçitlerine özgü olmaları dışında ExpressRoute ve VPN Ağ Geçidi için karşılık gelen mevcut SNU'lara benzer. Bu SKU'ları SKU adına "AZ" ile tanımlayabilirsiniz.

Ağ geçidi SKUs'ları hakkında daha fazla bilgi için [VPN ağ geçidi SK'leri](vpn-gateway-about-vpngateways.md#gwsku) ve [ExpressRoute ağ geçidi SK'leri'ne](../expressroute/expressroute-about-virtual-network-gateways.md#gwsku)bakın.

## <a name="public-ip-skus"></a><a name="pipskus"></a>Kamu IP SKUs

Bölge artıkları ve bölge ağ geçitleri her ikisi de Azure genel IP kaynağı *Standart* SKU'ya dayanır. Azure genel IP kaynağının yapılandırması, dağıttığınız ağ geçidinin bölge gereksiz mi yoksa bölge seli mi olduğunu belirler. *Temel* SKU'lu bir genel IP kaynağı oluşturursanız, ağ geçidinde bölge artıklığı olmaz ve ağ geçidi kaynakları bölgesel olur.

### <a name="zone-redundant-gateways"></a><a name="pipzrg"></a>Alanlar arası yedekli ağ geçitleri

Bir bölge belirtmeden **Standart** genel IP SKU'yu kullanarak ortak bir IP adresi oluşturduğunuzda, ağ geçidinin VPN ağ geçidi mi yoksa ExpressRoute ağ geçidi mi olduğuna bağlı olarak davranış farklıdır. 

* VPN ağ geçidi için, iki ağ geçidi örneği, bölge artıklığı sağlamak için bu üç bölgeden herhangi 2'sinde dağıtılır. 
* Bir ExpressRoute ağ geçidi için, ikiden fazla örnek olabileceğinden, ağ geçidi üç bölgeye de yayılabilir.

### <a name="zonal-gateways"></a><a name="pipzg"></a>Zonal ağ geçitleri

**Standart** genel IP SKU'yu kullanarak ortak bir IP adresi oluşturduğunuzda ve Bölge 'yi (1, 2 veya 3) belirttiğinde, tüm ağ geçidi örnekleri aynı bölgede dağıtılır.

### <a name="regional-gateways"></a><a name="piprg"></a>Bölgesel ağ geçitleri

**Temel** genel IP SKU'yu kullanarak ortak bir IP adresi oluşturduğunuzda, ağ geçidi bölgesel bir ağ geçidi olarak dağıtılır ve ağ geçidinde yerleşik herhangi bir bölge artıklığı yoktur.

## <a name="faq"></a><a name="faq"></a>SSS

### <a name="what-will-change-when-i-deploy-these-new-skus"></a>Bu yeni STU'ları dağıtdığımda ne değişecek?

Sizin bakış açınızdan, ağ geçitlerinizi bölge artıklığıyla dağıtabilirsiniz. Bu, ağ geçitlerinin tüm örneklerinin Azure Kullanılabilirlik Bölgeleri arasında dağıtılacayaçalışacağı ve her Kullanılabilirlik Bölgesinin farklı bir hata ve güncelleştirme etki alanı olduğu anlamına gelir. Bu, ağ geçitlerinizi bölge hatalarına karşı daha güvenilir, kullanılabilir ve esnek hale getirir.

### <a name="can-i-use-the-azure-portal"></a>Azure portalını kullanabilir miyim?

Evet, yeni SNU'ları dağıtmak için Azure portalını kullanabilirsiniz. Ancak, bu yeni SK'leri yalnızca Azure Kullanılabilirlik Bölgeleri olan Azure bölgelerinde görürsünüz.

### <a name="what-regions-are-available-for-me-to-use-the-new-skus"></a>Yeni SNU'ları kullanabilmek için hangi bölgeler kullanılabilir?

Yeni SUK'lar Azure Kullanılabilirlik Bölgeleri olan Azure bölgelerinde (Orta ABD, Fransa Orta, Kuzey Avrupa, Batı Avrupa ve Batı ABD 2 bölgeleri, Doğu ABD, Doğu ABD 2, Güneydoğu Asya, Japonya Doğu, İngiltere Güneyi) kullanılabilir. İleriye dönük olarak, diğer Azure Ortak Bölgelerinde bölge artıkları artıkağlarını kullanıma sunacağız.

### <a name="can-i-changemigrateupgrade-my-existing-virtual-network-gateways-to-zone-redundant-or-zonal-gateways"></a>Can I change/migrate/upgrade my existing virtual network gateways to zone-redundant or zonal gateways?

Varolan sanal ağ ağağlarınızı bölge gereksiz veya bölge ağ geçitlerine geçirmek şu anda desteklenmez. Ancak, varolan ağ geçidinizi silebilir ve bölge gereksiz veya bölgesel ağ geçidini yeniden oluşturabilirsiniz.

### <a name="can-i-deploy-both-vpn-and-express-route-gateways-in-same-virtual-network"></a>Vpn ve Express Route ağ geçitlerini aynı sanal ağda dağıtabilir miyim?

Aynı sanal ağda hem VPN hem de Express Route ağ geçitlerinin birlikte liği desteklenir. Ancak, ağ geçidi alt ağı için bir /27 IP adresi aralığı rezerve etmelisiniz.

## <a name="next-steps"></a>Sonraki adımlar

[Alanlar arası yedekli sanal ağ geçidi oluşturma](create-zone-redundant-vnet-gateway.md)
