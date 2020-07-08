---
title: 'Senaryo: any-any'
titleSuffix: Azure Virtual WAN
description: Any ile herhangi bir yönlendirme için senaryolar
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 06/29/2020
ms.author: cherylmc
ms.openlocfilehash: ecc2b3cf236cb2a78fd595189649e7f6b176d709
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85569026"
---
# <a name="scenario-any-to-any"></a>Senaryo: any-any

Sanal WAN sanal hub 'ı yönlendirme ile çalışırken, kullanılabilecek oldukça az sayıda senaryo vardır. Dilediğiniz bir senaryoda, herhangi bir bağlı bileşen başka bir bağlı bileşene erişebilir. Birden çok hub mevcut olduğunda, standart sanal WAN 'da hub-hub arası yönlendirme (hub 'lar olarak da bilinir) varsayılan olarak etkindir. 

Bu senaryoda, VPN, ExpressRoute ve kullanıcı VPN bağlantıları aynı yol tablosuyla ilişkilendirilir. Tüm VPN, ExpressRoute ve kullanıcı VPN bağlantıları, rotaları aynı yol tabloları kümesine yayar. Sanal hub yönlendirmesi hakkında daha fazla bilgi için bkz. [sanal hub yönlendirmesi hakkında](about-virtual-hub-routing.md).

## <a name="scenario-architecture"></a><a name="architecture"></a>Senaryo mimarisi

**Şekil 1**' de tüm sanal ağlar ve dallar (VPN, ExpressRoute, P2S) birbirine ulaşabilir. Bir sanal hub 'da bağlantılar aşağıdaki gibi çalışır:

* VPN bağlantısı VPN sitesini VPN ağ geçidine bağlar.
* Sanal ağ bağlantısı, sanal bir ağı bir sanal hub 'a bağlar. Sanal hub 'ın yönlendiricisi, sanal ağlar arasında aktarım işlevi sağlar.
* ExpressRoute bağlantısı, bir ExpressRoute bağlantı hattını ExpressRoute bağlantı hattına bağlar.

Bu bağlantılar (varsayılan olarak, oluşturma sırasında), bağlantının yönlendirme yapılandırmasını **hiçbiri**veya özel bir yol tablosu olarak ayarlamadığınız takdirde varsayılan yol tablosuyla ilişkilendirilir. Bu bağlantılar, varsayılan olarak varsayılan yol tablosuna aynı zamanda yolları da yayar. Bu, herhangi bir bağlı bileşen (VNet, VPN, ER, P2S) birbirlerine ulaşabulabildiği herhangi bir senaryoya olanak sağlar.

**Şekil 1**

:::image type="content" source="./media/routing-scenarios/any-any/figure-1.png" alt-text="Şekil 1":::

## <a name="scenario-workflow"></a><a name="workflow"></a>Senaryo iş akışı

Bu senaryo, standart sanal WAN için varsayılan olarak etkinleştirilmiştir. Şube dalı için ayarı WAN yapılandırmasında devre dışıysa, bu, dal arasında bağlantı yapılmasına izin vermez. VPN/ExpressRoute/Kullanıcı VPN, sanal WAN 'da dal olarak kabul edilir

## <a name="next-steps"></a>Sonraki adımlar

* Sanal WAN hakkında daha fazla bilgi için bkz. [SSS](virtual-wan-faq.md).
* Sanal hub yönlendirmesi hakkında daha fazla bilgi için bkz. [sanal hub yönlendirmesi hakkında](about-virtual-hub-routing.md).
