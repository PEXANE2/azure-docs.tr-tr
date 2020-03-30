---
title: Uzak kullanıcıları desteklemek için Azure ExpressRoute'u kullanma
description: Bu sayfa, COVID-19 salgını nedeniyle uzaktan çalışmayı etkinleştirmek için Azure ExpressRoute'dan nasıl yararlanabileceğinizi açıklar.
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: article
ms.date: 03/22/2020
ms.author: ajitbhu
ms.openlocfilehash: 2fe990fd9c5922dd2e059dbb212cd8bb1cd3726e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80336660"
---
# <a name="using-azure-expressroute-to-create-hybrid-connectivity-to-support-remote-users"></a>Uzak kullanıcıları desteklemek için karma bağlantı oluşturmak için Azure ExpressRoute'u kullanma

Bu makalede, uzaktan çalışmak için ExpressRoute, Azure, Microsoft ağı ve Azure iş ortağı ekosisteminden nasıl yararlanabileceğiniz açıklanmaktadır.

## <a name="connecting-to-azure-services-with-expressroute"></a>ExpressRoute ile Azure hizmetlerine bağlanma

>[!NOTE]
>Bu makalede, COVID-19 krizi nedeniyle karşılaştığınız ağ sorunlarını uzaktan çalışmak ve ağ sorunlarını azaltmak için ExpressRoute, Azure, Microsoft ağı ve Azure iş ortağı ekosisteminden nasıl yararlanabileceğiniz açıklanmaktadır.
>

[ExpressRoute,](expressroute-introduction.md) Microsoft veri merkezleri ve binalarınızda veya bir birlikte konumlandırma tesisinde bulunan altyapı arasında özel bağlantı sağlayan bir Azure hizmetidir. ExpressRoute bağlantıları ortak İnternet üzerinden geçmemektedir. Internet üzerinden tipik bağlantılara göre daha düşük ve tutarlı gecikmelerle güvenli bağlantı, güvenilirlik ve hız sunarlar.

## <a name="useful-links"></a>Yararlı bağlantılar

* [Varolan ExpressRoute devresi için bant genişliği nasıl artırılalı?](expressroute-howto-circuit-portal-resource-manager.md#modify)
* [ExpressRoute izleme, ölçümler ve uyarılar](expressroute-monitoring-metrics-alerts.md#expressroute-gateway-connections-in-bitsseconds)
* [ExpressRoute üzerinden Rota Optimizasyonu](expressroute-optimize-routing.md)
* [O365 için Azure ExpressRoute](https://docs.microsoft.com/office365/enterprise/azure-expressroute?redirectSourcePath=%252farticle%252f6d2534a2-c19c-4a99-be5e-33a0cee5d3bd)
* [Asimetrik yönlendirme hususları](expressroute-asymmetric-routing.md)
* [Azure portalında destek isteği nasıl açılır?](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview)

## <a name="troubleshoot"></a>Sorun giderme

* [ExpressRoute Bağlantısını Doğrulama](expressroute-troubleshooting-expressroute-overview.md)
* [Kaynak Yöneticisi](expressroute-troubleshooting-arp-resource-manager.md) ve [Klasik](expressroute-troubleshooting-arp-classic.md) ARP tablosu alma
* [Arızalı Devreyi Sıfırla](reset-circuit.md)
* [Sorun Giderme Ağ Performansı](expressroute-troubleshooting-network-performance.md)

## <a name="next-steps"></a>Sonraki Adımlar

* [ExpressRoute bağlantı modelleri hakkında bilgi edinin](expressroute-connectivity-models.md)
* ExpressRoute bağlantıları ve yönlendirme etki alanları hakkında bilgi edinin. Bkz. [ExpressRoute devreleri ve yönlendirme etki alanları](expressroute-circuit-peerings.md)
* Bir hizmet sağlayıcı bulun. Bkz. [ExpressRoute iş ortakları ve bakan konumlar](expressroute-locations.md)
* Tüm önkoşulların sağlandığından emin olun. Bkz. [ExpressRoute önkoşulları](expressroute-prerequisites.md).
* [ExpressRoute bağlantı hattı oluşturma ve değiştirme](expressroute-howto-circuit-portal-resource-manager.md)
* [Bir ExpressRoute bağlantı hattı için eşlemeyi oluşturma ve değiştirme](expressroute-howto-routing-portal-resource-manager.md)
* [Bir sanal ağı ExpressRoute bağlantı hattına bağlama](expressroute-howto-linkvnet-portal-resource-manager.md)
