---
title: 'Azure ExpressRoute: QoS gereksinimleri'
description: Bu sayfa, QoS'u yapılandırmak ve yönetmek için ayrıntılı gereksinimler sağlar. Skype for Business/ses hizmetleri tartışılmıştır.
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: conceptual
ms.date: 04/22/2019
ms.author: cherylmc
ms.openlocfilehash: debc5d91478d0a5c3cc16c7b09f5713ba09b467e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74080094"
---
# <a name="expressroute-qos-requirements"></a>ExpressRoute QoS gereksinimleri
Skype Kurumsal’da farklı QoS davranışları gerektiren çeşitli iş yükleri vardır. ExpressRoute aracılığıyla ses hizmetleri kullanmayı planlıyorsanız, aşağıda açıklanan gereksinimlere uymanız gerekir.

![](./media/expressroute-qos/expressroute-qos.png)

> [!NOTE]
> QoS gereksinimleri yalnızca Microsoft eşlemeleri için geçerlidir. Azure ortak eşleme ve Azure özel eşlemesinde alınan ağ trafiğinizdeki DSCP değerleri 0’a ayarlanacaktır. 
> 
> 

Aşağıdaki tablo, Microsoft Ekipleri ve Skype for Business tarafından kullanılan DSCP işaretlerinin bir listesini sağlar. Daha fazla bilgi için [Skype Kurumsal için QoS’i yönetme](https://docs.microsoft.com/SkypeForBusiness/manage/network-management/qos/managing-quality-of-service-QoS) bölümüne başvurun.

| **Trafik Sınıfı** | **İşleme (DSCP İşaretleme)** | **Microsoft Ekipleri ve Kurumsal İş Yükleri için Skype** |
| --- | --- | --- |
| **Ses** |EF (46) |Skype / Microsoft Teams / Lync ses |
| **Etkileşimli** |AF41 (34) |Video, VBSS |
| |AF21 (18) |Uygulama paylaşımı | 
| **Varsayılan** |AF11 (10) |Dosya aktarımı |
| |CS0 (0) |Diğer |

* İş yükleri sınıflandırmanız ve doğru DSCP değerlerini işaretlemeniz gerekir. Ağınızda DSCP işaretlerini ayarlamak için [burada](https://docs.microsoft.com/SkypeForBusiness/manage/network-management/qos/configuring-port-ranges-for-your-skype-clients#configure-quality-of-service-policies-for-clients-running-on-windows-10) sağlanan yönergeleri izleyin.
* Ağınızda çoklu QoS kuyruklarını yapılandırmanız ve desteklemeniz gerekir. Ses bağımsız bir sınıf olmalı ve [RFC 3246'da](https://www.ietf.org/rfc/rfc3246.txt)belirtilen EF tedavisini almalıdır. 
* Kuyruğa alma mekanizması, sıkışma algılama ilkesi ve trafik sınıfı başına bant genişliği ayırmayı siz belirleyebilirsiniz. Ancak, Skype Kurumsal iş yükleri için DSCP işaretlemesi korunmalıdır. AF31 (26) gibi yukarıda listelenmeyen DSCP işaretlerini kullanıyorsanız, paketi Microsoft'a göndermeden önce bu DSCP değerinin 0 olarak yeniden yazılması gerekir. Microsoft yalnızca yukarıdaki tabloda gösterilen DSCP değeriyle işaretlenen paketleri gönderir. 

## <a name="next-steps"></a>Sonraki adımlar
* [Yönlendirme](expressroute-routing.md) ve [NAT](expressroute-nat.md) için gereksinimlere bakın.
* ExpressRoute bağlantınızı yapılandırmak için aşağıdaki bağlantılara bakın.
  
  * [ExpressRoute bağlantı hattı oluşturma](expressroute-howto-circuit-classic.md)
  * [Yönlendirmeyi yapılandırma](expressroute-howto-routing-classic.md)
  * [ExpressRoute bağlantı hattına bir Sanal Ağ bağlama](expressroute-howto-linkvnet-classic.md)

