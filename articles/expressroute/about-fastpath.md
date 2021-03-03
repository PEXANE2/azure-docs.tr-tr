---
title: Azure ExpressRoute FastPath hakkında
description: Ağ geçidini atlayarak ağ trafiğini göndermek için Azure ExpressRoute FastPath hakkında bilgi edinin
services: expressroute
author: duongau
ms.service: expressroute
ms.topic: conceptual
ms.date: 03/25/2020
ms.author: duau
ms.openlocfilehash: ba23319c35aed1d09da652e6f84b60e5f8e9495e
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2021
ms.locfileid: "101740894"
---
# <a name="about-expressroute-fastpath"></a>ExpressRoute FastPath hakkında

ExpressRoute sanal ağ geçidi, ağ yollarını Exchange ve ağ trafiğini yönlendirme için tasarlanmıştır. FastPath, şirket içi ağınız ve sanal ağınız arasındaki veri yolu performansını geliştirmek için tasarlanmıştır. Etkin olduğunda, FastPath ağ trafiğini, ağ geçidini atlayarak sanal ağdaki sanal makinelere doğrudan gönderir.

## <a name="requirements"></a>Gereksinimler

### <a name="circuits"></a>Uygulanıp

FastPath tüm ExpressRoute devrelerinin üzerinde kullanılabilir.

### <a name="gateways"></a>Ağ geçitleri

FastPath, sanal ağ ile şirket içi ağ arasındaki yolların değişimi için bir sanal ağ geçidinin oluşturulmasını gerektirir. Performans bilgileri ve ağ geçidi SKU 'Ları dahil sanal ağ geçitleri ve ExpressRoute hakkında daha fazla bilgi için bkz. [ExpressRoute sanal ağ geçitleri](expressroute-about-virtual-network-gateways.md).

FastPath 'i yapılandırmak için sanal ağ geçidi şunlardan biri olmalıdır:

* Ultra performans
* ErGw3AZ

> [!IMPORTANT]
> FastPath 'i ExpressRoute üzerinden IPv6 tabanlı özel eşleme ile kullanmayı planlıyorsanız, **SKU** için ErGw3AZ ' ı seçtiğinizden emin olun.
> 
>

## <a name="limitations"></a>Sınırlamalar

FastPath çoğu yapılandırmayı desteklese de, aşağıdaki özellikleri desteklemez:

* Ağ geçidi alt ağında UDR: sanal ağınızın ağ geçidi alt ağına UDR uygularsanız, şirket içi ağınızdan gelen ağ trafiği sanal ağ geçidine gönderilmeye devam edecektir.

* VNet eşlemesi: ExpressRoute 'a bağlanan başka sanal ağlarınız varsa, şirket içi ağınızdan diğer sanal ağlara (yani "bağlı bileşen" sanal ağları) ağ trafiği, sanal ağ geçidine gönderilmeye devam edecektir. Geçici çözüm tüm sanal ağları ExpressRoute devresine doğrudan bağlamak olur.

* Temel Load Balancer: sanal ağınızda temel bir iç yük dengeleyici dağıtırsanız veya sanal ağınızda dağıttığınız Azure PaaS hizmeti temel bir iç yük dengeleyici kullanıyorsa, şirket içi ağınızdan temel yük dengeleyicide barındırılan sanal IP 'lere olan ağ trafiği sanal ağ geçidine gönderilir. Çözüm, temel yük dengeleyiciyi [Standart yük dengeleyiciye](../load-balancer/load-balancer-overview.md)yükseltmeye yönelik bir çözümdür.

* Özel bağlantı: şirket içi ağınızdan sanal ağınızdaki [özel bir uç noktaya](../private-link/private-link-overview.md) bağlanırsanız bağlantı, sanal ağ geçidi üzerinden geçer.
 
## <a name="next-steps"></a>Sonraki adımlar

FastPath 'i etkinleştirmek için bkz. [sanal ağı ExpressRoute 'A bağlama](expressroute-howto-linkvnet-arm.md#configure-expressroute-fastpath).
