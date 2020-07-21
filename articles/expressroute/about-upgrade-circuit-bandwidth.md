---
title: Devre bant genişliğini yükseltme hakkında | Azure ExpressRoute
description: Bu makalede, ExpressRoute bağlantı hattı bant genişliğini yükseltmek için en iyi yöntemleri öğrenin
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: conceptual
ms.date: 07/07/2020
ms.author: cherylmc
ms.openlocfilehash: a8f5aaa7b2a054aa31198779414387cebf0f0fbd
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86537046"
---
# <a name="about-upgrading-expressroute-circuit-bandwidth"></a>ExpressRoute devre bant genişliğini yükseltme hakkında

ExpressRoute, Microsoft 'un küresel ağına adanmış ve özel bağlantı sağlar. Bağlantı, bir ExpressRoute iş ortağının ağı veya Microsoft Enterprise Edge (MSEE) cihazlarına doğrudan bağlantı ile kolaylaştırılıyor. Fiziksel bağlantı yapılandırıldıktan ve test edildikten sonra, bir ExpressRoute bağlantı hattı oluşturup eşlemeyi yapılandırarak katman 2 ve katman 3 bağlantısını etkinleştirebilirsiniz.

## <a name="upgrade-circuit-bandwidth"></a><a name="upgrade"></a>Devre bant genişliğini yükselt

Devre bant genişliğini yükseltmek için, ExpressRoute Direct veya ExpressRoute iş ortağının, yükseltmenin başarılı olması için [yeterli kullanılabilir bant genişliğine](#considerations) sahip olması gerekir.

Kapasite varsa, aşağıdaki yöntemleri kullanarak devreyi yükseltebilirsiniz:

* [Azure Portal](expressroute-howto-circuit-portal-resource-manager.md#modify)
* [PowerShell](expressroute-howto-circuit-arm.md#modify)
* [Azure CLI](howto-circuit-cli.md#modify)

## <a name="capacity-considerations"></a><a name="considerations"></a>Kapasiteye ilişkin önemli noktalar

### <a name="insufficient-expressroute-partner-bandwidth"></a><a name="bandwidth"></a>Yetersiz ExpressRoute iş ortağı bant genişliği

ExpressRoute iş ortağının yeterli kapasitesi yoksa, istenen bant genişliğine göre yapılandırılmış yeni bir devre oluşturmanız gerekir. Bağlantıyı sürdürmek için, yeni oluşturulan bağlantı sağlandıktan, eşleme yapılandırıldıktan ve (özel eşleme ile ilgili) ExpressRoute sanal ağ geçidi için bağlantı nesnesi sağlanana kadar eski devre silmeyin.

ExpressRoute ortağınızda yeterli kullanılabilir kapasite yoksa, istenen eşleme konumunda ek kapasite istemeniz gerekir. Yeni kapasite sağlandıktan sonra, [yükseltme devre bant genişliği](#upgrade) bölümündeki makalelerde bulunan adımları kullanarak yeni bir devre oluşturabilir, bağlantıyı yapılandırabilir ve eski devreyi silebilirsiniz.


### <a name="insufficient-expressroute-direct-bandwidth"></a><a name="bandwidth"></a>Yetersiz ExpressRoute doğrudan bant genişliği

ExpressRoute Direct 'in yeterli kapasitesi yoksa, artık gerekmeyen ExpressRoute doğrudan kaynağıyla ilişkili devreleri silebilir ya da yeni bir ExpressRoute doğrudan kaynağı oluşturabilirsiniz. ExpressRoute Direct kaynağını yönetme Kılavuzu için bkz. [ExpressRoute Direct 'i yapılandırma](how-to-expressroute-direct-portal.md).

## <a name="next-steps"></a>Sonraki adımlar

* [Bağlantı hattı oluşturma ve değiştirme](expressroute-howto-circuit-portal-resource-manager.md)
* [Eşleme yapılandırması oluşturma ve değiştirme](expressroute-howto-routing-portal-resource-manager.md)
* [ExpressRoute bağlantı hattına bir sanal ağı bağlama](expressroute-howto-linkvnet-portal-resource-manager.md)
