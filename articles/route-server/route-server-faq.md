---
title: Azure Route sunucusu hakkında sık sorulan sorular
description: Azure Route sunucusu hakkında sık sorulan soruların yanıtlarını bulun.
services: route-server
author: duongau
ms.service: route-server
ms.topic: article
ms.date: 03/02/2021
ms.author: duau
ms.openlocfilehash: 994fdf3f929f1f64ab4a064d7f2bd24c19322629
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2021
ms.locfileid: "101680199"
---
# <a name="azure-route-server-preview-faq"></a>Azure rota sunucusu (Önizleme) SSS

> [!IMPORTANT]
> Azure yol sunucusu (Önizleme) Şu anda genel önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="what-is-azure-route-server"></a>Azure Route sunucusu nedir?

Azure yol sunucusu, ağ sanal gereci (NVA) ve sanal ağınız arasında yönlendirmeyi kolayca yönetmenizi sağlayan, tam olarak yönetilen bir hizmettir.

### <a name="is-azure-route-server-just-a-vm"></a>Azure rota sunucusu yalnızca bir VM mi?

Hayır. Azure rota sunucusu, yüksek kullanılabilirliğe sahip bir hizmettir. [Kullanılabilirlik alanları](../availability-zones/az-overview.md)destekleyen bir Azure bölgesinde dağıtılmışsa, bölge düzeyinde yedeklilik olur.

### <a name="what-routing-protocols-does-azure-route-server-support"></a><a name = "protocol"></a>Azure Route sunucusu hangi yönlendirme protokollerini destekler?

Azure Route sunucusu yalnızca Sınır Ağ Geçidi Protokolü (BGP) destekler. Azure Route Server 'ı sanal ağınızdaki ayrılmış bir alt ağda dağıtmanız gerekeceğinden, NVA 'nın çok duraklı dış BGP 'yi desteklemesi gerekir. Seçtiğiniz [ASN](https://en.wikipedia.org/wiki/Autonomous_system_(Internet)) , NVA 'da BGP 'yi yapılandırırken kullandığınız Azure yol sunucusundan farklı olmalıdır.

### <a name="does-azure-route-server-route-data-traffic-between-my-nva-and-my-vms"></a>Azure Route sunucusu, NVA ve sanal makinelerim arasındaki veri trafiğini yönlendirebilir mi?

Hayır. Azure Route sunucusu yalnızca NVA 'yla BGP yollarını değiş tokuş eder. Veri trafiği, doğrudan NVA 'dan seçilen VM 'ye ve doğrudan VM 'den NVA 'ya gider.

### <a name="if-azure-route-server-receives-the-same-route-from-more-than-one-nva-will-it-program-all-copies-of-the-route-but-each-with-a-different-next-hop-to-the-vms-in-the-virtual-network"></a>Azure Route sunucusu birden fazla NVA 'dan aynı rotayı alırsa, yolun tüm kopyalarını (her biri farklı bir sonraki atlamada) sanal ağdaki VM 'lere yeniden programsın mı?

Evet, yalnızca yol yol uzunluğu Ile aynı olduğunda. VM 'Ler bu yolun hedefine trafik gönderdiğinizde, VM Konakları Equal-Cost çoklu yol (ECMP) yönlendirmesi olur. Ancak, bir NVA yolu diğer NVA 'lar daha kısa bir yol uzunluğu ile gönderirse. Azure Route Server yalnızca bir sonraki atlamanın, sanal ağdaki VM 'lere bu NVA 'ya ayarlandığı yolu programlayacaktır.

### <a name="does-azure-route-server-support-vnet-peering"></a>Azure Route sunucusu VNet eşlemesini destekliyor mu?

Evet. Azure yol sunucusunu barındıran bir sanal ağı başka bir sanal ağa eşleyebilir ve bu VNet üzerinde uzak ağ geçidini kullanabilirsiniz. Azure yol sunucusu, bu sanal ağın adres alanlarını öğrenip, tüm eşlenmiş NVA 'lar 'e gönderecektir.

### <a name="what-autonomous-system-numbers-asns-can-i-use"></a>Hangi otonom sistem numaralarını (ASNs) kullanabilirim?

Ağ sanal gerecinizde kendi genel Ilişkinizi veya özel ans 'lerinizi kullanabilirsiniz. Azure veya ıANA tarafından ayrılan aralıkları kullanamazsınız.
Aşağıdaki ASNs 'ler Azure veya ıANA tarafından ayrılmıştır:

* Azure tarafından ayrılan ASNs:
    * Ortak ASN’ler: 8074, 8075, 12076
    * Özel ASN’ler: 65515, 65517, 65518, 65519, 65520
* [IANA tarafından ayrılmış](http://www.iana.org/assignments/iana-as-numbers-special-registry/iana-as-numbers-special-registry.xhtml)ASNs:
    * 23456, 64496-64511, 65535-65551

### <a name="can-i-use-32-bit-4-byte-asns"></a>32 bit (4 baytlık) ASNs kullanabilir miyim?

Hayır, Azure Route sunucusu yalnızca 16 bit (2 bayt) ASNs 'yi destekler.

## <a name="route-server-limits"></a><a name = "limitations"></a>Rota sunucusu sınırları

Azure Route sunucusu, aşağıdaki sınırlara sahiptir (dağıtım başına).

| Kaynak | Sınır |
|----------|-------|
| Desteklenen BGP eşlerinin sayısı | 8 |
| Her BGP eşinin Azure Route sunucusuna duyurdığı yolların sayısı | 200 |
| Azure Route sunucusunun ExpressRoute veya VPN Gateway 'e duyurdığı yolların sayısı | 200 |

Daha fazla bilgi için bkz. [Azure sanal makine yönlendirme sorununu tanılama](../virtual-network/diagnose-network-routing-problem.md).

## <a name="next-steps"></a>Sonraki adımlar

[Azure Route Server 'ı yapılandırmayı](quickstart-configure-route-server-powershell.md)öğrenin.
