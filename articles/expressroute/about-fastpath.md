---
title: Azure ExpressRoute FastPath Hakkında
description: Ağ geçidini atlayarak ağ trafiğini göndermek için Azure ExpressRoute FastPath hakkında bilgi edinin
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: conceptual
ms.date: 03/25/2020
ms.author: cherylmc
ms.openlocfilehash: 265004b1171d1df95b3090676d5836b951c28a28
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80282860"
---
# <a name="about-expressroute-fastpath"></a>ExpressRoute FastPath hakkında

ExpressRoute sanal ağ ağ geçidi, ağ yollarını ve ağ trafiğini yönlendirmek için tasarlanmıştır. FastPath, şirket içi ağınızla sanal ağınız arasındaki veri yolu performansını artırmak için tasarlanmıştır. FastPath etkinleştirildiğinde ağ trafiğini doğrudan sanal ağdaki sanal makinelere göndererek ağ geçidini atlar.

## <a name="requirements"></a>Gereksinimler

### <a name="circuits"></a>Devre

FastPath tüm ExpressRoute devrelerinde kullanılabilir.

### <a name="gateways"></a>Ağ geçitleri

FastPath, sanal ağ ve şirket içi ağ arasında yol alışverişi yapmak için bir sanal ağ ağ geçidi oluşturulmasını gerektirir. Performans bilgileri ve ağ geçidi SUS'ları da dahil olmak üzere sanal ağ ağ geçitleri ve ExpressRoute hakkında daha fazla bilgi için [ExpressRoute sanal ağ ağ ağ geçitlerine](expressroute-about-virtual-network-gateways.md)bakın.

FastPath'i yapılandırmak için sanal ağ ağ ağ geçidinin aşağıdakilerden biri olması gerekir:

* Ultra Performans
* Ergw3AZ

## <a name="supported-features"></a>Desteklenen özellikler

FastPath çoğu yapılandırmayı desteklerken, aşağıdaki özellikleri desteklemez:

* Ağ geçidi alt ağındaki UDR: Sanal ağınızın ağ geçidi alt ağına bir UDR uygularsanız, şirket içi ağınızdaki ağ trafiği sanal ağ ağ geçidine gönderilmeye devam edecektir.

* VNet Peering: ExpressRoute'a bağlı olana bakan diğer sanal ağlarınız varsa, şirket içi ağınızdaki ağ trafiği diğer sanal ağlara (yani "Spoke" VNets) sanal ağa gönderilmeye devam edecektir Ağ geçidi. Geçici çözüm, tüm sanal ağları doğrudan ExpressRoute devresine bağlamaktır.

* Temel Yük Dengeleyicisi: Sanal ağınızda bir Temel dahili yük dengeleyicisi dağıtırsanız veya sanal ağınızda dağıttığınız Azure PaaS hizmeti temel dahili yük dengeleyicisi kullanırsa, şirket içi ağınızdan, ağ trafiğinden, barındırılan sanal IP'lere kadar olan ağ trafiği Temel yük dengeleyicisi sanal ağ ağ geçidine gönderilir. Çözüm, Temel yük dengeleyicisini [Standart yük dengeleyicisine](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview)yükseltmektir.

* Özel Bağlantı: Şirket içi ağınızdan sanal ağınızdaki özel bir [bitiş noktasına](../private-link/private-link-overview.md) bağlanırsanız, bağlantı sanal ağ ağ geçidinden geçer.
 
## <a name="next-steps"></a>Sonraki adımlar

FastPath'i etkinleştirmek [için, expressroute'a sanal ağ bağlayın'a](expressroute-howto-linkvnet-arm.md#configure-expressroute-fastpath)bakın.
