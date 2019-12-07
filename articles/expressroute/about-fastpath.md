---
title: Azure ExpressRoute FastPath hakkında
description: Ağ geçidini atlayarak ağ trafiğini göndermek için Azure ExpressRoute FastPath hakkında bilgi edinin
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: conceptual
ms.date: 12/05/2019
ms.author: cherylmc
ms.openlocfilehash: 6ff1dac312bcb4df1e1afc9679df09fc8a2b28ff
ms.sourcegitcommit: 8bd85510aee664d40614655d0ff714f61e6cd328
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2019
ms.locfileid: "74897370"
---
# <a name="about-expressroute-fastpath"></a>ExpressRoute FastPath hakkında

ExpressRoute sanal ağ geçidi, ağ yollarını Exchange ve ağ trafiğini yönlendirme için tasarlanmıştır. FastPath, şirket içi ağınız ve sanal ağınız arasındaki veri yolu performansını geliştirmek için tasarlanmıştır. Etkin olduğunda, FastPath ağ trafiğini, ağ geçidini atlayarak sanal ağdaki sanal makinelere doğrudan gönderir.

## <a name="requirements"></a>Gereksinimler

### <a name="circuits"></a>Uygulanıp

FastPath tüm ExpressRoute devrelerinin üzerinde kullanılabilir.

### <a name="gateways"></a>Ağ geçitleri

FastPath, sanal ağ ile şirket içi ağ arasındaki yolların değişimi için bir sanal ağ geçidinin oluşturulmasını gerektirir. Sanal ağ geçitleri ve ExpressRoute hakkında daha fazla bilgi için bkz. [ExpressRoute sanal ağ geçitleri](expressroute-about-virtual-network-gateways.md).

Sanal ağ geçidi şunlardan biri olmalıdır:

* Ultra Yüksek Performans
* ErGw3AZ

## <a name="supported-features"></a>Desteklenen özellikler

FastPath çoğu yapılandırmayı desteklese de, aşağıdaki özellikleri desteklemez:

* Ağ geçidi alt ağında UDR: sanal ağınızın ağ geçidi alt ağına UDR uygularsanız, şirket içi ağınızdan gelen ağ trafiği sanal ağ geçidine gönderilmeye devam edecektir.

* VNet eşlemesi: ExpressRoute 'a bağlanan başka sanal ağlarınız varsa, şirket içi ağınızdan diğer sanal ağlara ağ trafiği (yani, "bağlı bileşen" sanal ağları), sanal ağa gönderilmeye devam eder geçidinde. Geçici çözüm tüm sanal ağları ExpressRoute devresine doğrudan bağlamak olur.

* Temel Load Balancer: sanal ağınızda temel bir iç yük dengeleyici dağıtırsanız veya sanal ağınızda dağıttığınız Azure PaaS hizmeti temel bir iç yük dengeleyici kullanıyorsa, şirket içi ağınızdan gelen ağ trafiği, üzerinde barındırılan sanal IP 'lere Temel yük dengeleyici sanal ağ geçidine gönderilir. Çözüm, temel yük dengeleyiciyi [Standart yük dengeleyiciye](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview)yükseltmeye yönelik bir çözümdür.

* Özel bağlantı: şirket içi ağınızdan sanal ağınızdaki [özel bir uç noktaya](../private-link/private-link-overview.md) bağlanırsanız bağlantı, sanal ağ geçidi üzerinden geçer.
 
## <a name="next-steps"></a>Sonraki adımlar

FastPath 'i etkinleştirmek için bkz. [sanal ağı ExpressRoute 'A bağlama](expressroute-howto-linkvnet-arm.md#configure-expressroute-fastpath).
