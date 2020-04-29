---
title: Azure ExpressRoute FastPath hakkında
description: Ağ geçidini atlayarak ağ trafiğini göndermek için Azure ExpressRoute FastPath hakkında bilgi edinin
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: conceptual
ms.date: 03/25/2020
ms.author: cherylmc
ms.openlocfilehash: 265004b1171d1df95b3090676d5836b951c28a28
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80282860"
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

## <a name="supported-features"></a>Desteklenen özellikler

FastPath çoğu yapılandırmayı desteklese de, aşağıdaki özellikleri desteklemez:

* Ağ geçidi alt ağında UDR: sanal ağınızın ağ geçidi alt ağına UDR uygularsanız, şirket içi ağınızdan gelen ağ trafiği sanal ağ geçidine gönderilmeye devam edecektir.

* VNet eşlemesi: ExpressRoute 'a bağlanan başka sanal ağlarınız varsa, şirket içi ağınızdan diğer sanal ağlara (yani "bağlı bileşen" sanal ağları) ağ trafiği, sanal ağ geçidine gönderilmeye devam edecektir. Geçici çözüm tüm sanal ağları ExpressRoute devresine doğrudan bağlamak olur.

* Temel Load Balancer: sanal ağınızda temel bir iç yük dengeleyici dağıtırsanız veya sanal ağınızda dağıttığınız Azure PaaS hizmeti temel bir iç yük dengeleyici kullanıyorsa, şirket içi ağınızdan temel yük dengeleyicide barındırılan sanal IP 'lere olan ağ trafiği sanal ağ geçidine gönderilir. Çözüm, temel yük dengeleyiciyi [Standart yük dengeleyiciye](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview)yükseltmeye yönelik bir çözümdür.

* Özel bağlantı: şirket içi ağınızdan sanal ağınızdaki [özel bir uç noktaya](../private-link/private-link-overview.md) bağlanırsanız bağlantı, sanal ağ geçidi üzerinden geçer.
 
## <a name="next-steps"></a>Sonraki adımlar

FastPath 'i etkinleştirmek için bkz. [sanal ağı ExpressRoute 'A bağlama](expressroute-howto-linkvnet-arm.md#configure-expressroute-fastpath).
