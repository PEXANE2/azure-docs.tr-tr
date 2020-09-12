---
title: Uzak kullanıcıları desteklemek için Azure ExpressRoute kullanma
description: Bu sayfada COVıD-19 pandemıc nedeniyle uzaktan çalışmayı etkinleştirmek için Azure ExpressRoute 'tan nasıl yararlanabileceğinizi açıklanmaktadır.
services: expressroute
author: duongau
ms.service: expressroute
ms.topic: how-to
ms.date: 03/22/2020
ms.author: duau
ms.openlocfilehash: d51f47b73fe572ce81d3e7b54f902f94fcd11b5b
ms.sourcegitcommit: d0541eccc35549db6381fa762cd17bc8e72b3423
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/09/2020
ms.locfileid: "89567682"
---
# <a name="using-azure-expressroute-to-create-hybrid-connectivity-to-support-remote-users"></a>Uzak kullanıcıları desteklemek üzere karma bağlantı oluşturmak için Azure ExpressRoute kullanma

Bu makalede, ExpressRoute, Azure, Microsoft Network ve Azure iş ortağı ekosisteminin uzaktan çalışması için nasıl kullanabileceğiniz açıklanır.

## <a name="connecting-to-azure-services-with-expressroute"></a>ExpressRoute ile Azure hizmetlerine bağlanma

>[!NOTE]
>Bu makalede, ExpressRoute, Azure, Microsoft Network ve Azure iş ortağı ekosisteminden yararlanarak uzaktan nasıl çalışabileceğinizi ve COVı-19 çapraz olduğu için kullanıma sunulacak ağ sorunlarını azaltmayı nasıl kullanabileceğiniz açıklanmaktadır.
>

[ExpressRoute](expressroute-introduction.md) , Microsoft veri merkezleri ile şirket içinde veya bir birlikte bulundurma tesisinde bulunan altyapı arasında özel bağlantı sağlayan bir Azure hizmetidir. ExpressRoute bağlantıları ortak İnternet üzerinden geçmemektedir. Internet üzerinden tipik bağlantılardan daha düşük ve tutarlı gecikme süreleriyle güvenli bağlantı, güvenilirlik ve hız sunar.

## <a name="useful-links"></a>Yararlı bağlantılar

* [Mevcut ExpressRoute devresi için bant genişliğini artırma](expressroute-howto-circuit-portal-resource-manager.md#modify)
* [ExpressRoute izleme, ölçümler ve uyarılar](expressroute-monitoring-metrics-alerts.md#expressroute-gateway-connections-in-bitsseconds)
* [ExpressRoute üzerinden rota Iyileştirmesi](expressroute-optimize-routing.md)
* [Microsoft 365 için Azure ExpressRoute](/microsoft-365/enterprise/azure-expressroute)
* [Asimetrik yönlendirme konuları](expressroute-asymmetric-routing.md)
* [Azure portal bir destek isteği açma](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview)

## <a name="troubleshoot"></a>Sorun giderme

* [ExpressRoute Bağlantısını Doğrulama](expressroute-troubleshooting-expressroute-overview.md)
* [Kaynak Yöneticisi](expressroute-troubleshooting-arp-resource-manager.md) ve [Klasik](expressroute-troubleshooting-arp-classic.md) bir ARP tablosunu alma
* [Başarısız bir devre sıfırlama](reset-circuit.md)
* [Ağ performansının sorunlarını giderme](expressroute-troubleshooting-network-performance.md)

## <a name="next-steps"></a>Sonraki Adımlar

* [ExpressRoute bağlantı modelleri hakkında bilgi edinin](expressroute-connectivity-models.md)
* ExpressRoute bağlantıları ve yönlendirme etki alanları hakkında bilgi edinin. Bkz. [ExpressRoute devreleri ve yönlendirme etki alanları](expressroute-circuit-peerings.md)
* Bir hizmet sağlayıcı bulun. Bkz. [ExpressRoute iş ortakları ve eşleme konumları](expressroute-locations.md)
* Tüm önkoşulların sağlandığından emin olun. Bkz. [ExpressRoute önkoşulları](expressroute-prerequisites.md).
* [ExpressRoute bağlantı hattını oluşturma ve değiştirme](expressroute-howto-circuit-portal-resource-manager.md)
* [Bir ExpressRoute bağlantı hattı için eşlemeyi oluşturma ve değiştirme](expressroute-howto-routing-portal-resource-manager.md)
* [Sanal ağı ExpressRoute bağlantı hattına bağlama](expressroute-howto-linkvnet-portal-resource-manager.md)
