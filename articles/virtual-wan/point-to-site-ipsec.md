---
title: Sanal WAN Noktadan siteye IPSec ilkeleri
titleSuffix: Azure Virtual WAN
description: Azure sanal WAN Noktadan siteye IPSec bağlantı ilkeleri hakkında bilgi edinin.
services: virtual-wan
author: wellee
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 02/24/2021
ms.author: wellee
ms.openlocfilehash: d64748bf7719af52b76c5e4141bfbbabe80bbae2
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "101746984"
---
# <a name="point-to-site-ipsec-policies"></a>Noktadan siteye IPSec ilkeleri

Bu makalede, Azure sanal WAN 'da Noktadan siteye VPN bağlantısı için desteklenen IPSec ilke birleşimleri gösterilmektedir.

## <a name="default-ipsec-policies"></a>Varsayılan IPSec ilkeleri

Aşağıdaki tabloda, Noktadan siteye VPN bağlantıları için varsayılan IPSec parametreleri gösterilmektedir. Bu parametreler, Noktadan siteye bir profil oluşturulduğunda sanal WAN Noktadan siteye VPN ağ geçidi tarafından otomatik olarak seçilir.

| Ayar | Parametreler |
|--- |--- |
| 1. aşama ıKE şifrelemesi | AES256 |
| 1. aşama ıKE bütünlüğü |  SHA256 |
| DH Grubu | DHGroup24 |
| 2. aşama IPSec şifrelemesi | GCMAES256|
| 2. aşama IPSec bütünlüğü | GCMAES25 |
| PFS Grubu |PFS24|

## <a name="custom-ipsec-policies"></a>Özel IPSec ilkeleri

Özel IPSec ilkeleriyle çalışırken aşağıdaki gereksinimleri göz önünde bulundurun:

* **Ike** -1. aşama Için Ike şifrelemesi ' nden herhangi bir PARAMETREYI ve Ike bütünlüğünden herhangi bir PARAMETREYI ve DH grubundan herhangi bir parametreyi seçebilirsiniz.
* **IPSec** -2. aşama Için IPSec şifrelemesi ' nden herhangi bir parametreyi ve IPSec bütünlüğünden herhangi bir PARAMETREYI ve PFS ' yi seçebilirsiniz. IPSec şifrelemesi veya IPSec bütünlüğü için parametrelerden herhangi biri GCM ise, hem IPSec şifrelemesi hem de bütünlüğü aynı algoritmayı kullanmalıdır. Örneğin, IPSec şifrelemesi için GCMAES128 seçilirse, IPSec bütünlüğü için de GCMAES128 seçilmelidir.  

Aşağıdaki tabloda, Noktadan siteye VPN bağlantıları için kullanılabilir IPSec parametreleri gösterilmektedir.

| Ayar | Parametreler |
|--- |--- |
| 1. aşama ıKE şifrelemesi | AES128, AES256, GCMAES128, GCMAES256 |
| 1. aşama ıKE bütünlüğü |  SHA256, SHA384 |
| DH Grubu | DHGroup14, DHGroup24, ECP256, ECP384 |
| 2. aşama IPSec şifrelemesi | GCMAES128, GCMAES256, SHA256|
| 2. aşama IPSec bütünlüğü | GCMAES128, GCMAES256 |
| PFS Grubu |PFS14, PFS24, ECP256, ECP384|

## <a name="next-steps"></a>Sonraki adımlar

Bkz. [Noktadan siteye bağlantı oluşturma](virtual-wan-point-to-site-portal.md)

Sanal WAN hakkında daha fazla bilgi için bkz. [sanal WAN hakkında SSS](virtual-wan-faq.md).
