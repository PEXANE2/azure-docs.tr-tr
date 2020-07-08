---
title: "Senaryo: VNET 'leri yalıtma"
titleSuffix: Azure Virtual WAN
description: Yönlendirme yalıtma sanal ağları için senaryolar
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 06/29/2020
ms.author: cherylmc
ms.openlocfilehash: f43f17a0f3742831920836e448de3ef757f2dfa6
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85569013"
---
# <a name="scenario-isolating-vnets"></a>Senaryo: VNET 'leri yalıtma

Sanal WAN sanal hub 'ı yönlendirme ile çalışırken, kullanılabilecek oldukça az sayıda senaryo vardır. Bu senaryoda, hedef, VNET 'lerin başka bir şekilde iletişime geçebilmesini engellemektir. Bu, sanal ağları yalıtma olarak bilinir. VNet içindeki iş yükü yalıtılmış kalır ve herhangi bir senaryoda olduğu gibi diğer VNET 'ler ile iletişim kuramaz. Ancak, sanal ağlar tüm dallara (VPN, ER ve kullanıcı VPN) ulaşabilmesi için gereklidir. Bu senaryoda, tüm VPN, ExpressRoute ve kullanıcı VPN bağlantıları aynı ve bir yol tablosuyla ilişkilendirilir. Tüm VPN, ExpressRoute ve kullanıcı VPN bağlantıları, rotaları aynı yol tabloları kümesine yayar. Sanal hub yönlendirmesi hakkında daha fazla bilgi için bkz. [sanal hub yönlendirmesi hakkında](about-virtual-hub-routing.md).

## <a name="scenario-workflow"></a><a name="workflow"></a>Senaryo iş akışı

Bu senaryoyu yapılandırmak için aşağıdaki adımları göz önünde bulundurun:

1. Özel bir yol tablosu oluşturun. Örnekte, yol tablosu **RT_VNet**. Bir yol tablosu oluşturmak için bkz. [sanal hub yönlendirmeyi yapılandırma](how-to-virtual-hub-routing.md). Yol tabloları hakkında daha fazla bilgi için bkz. [sanal hub yönlendirmesi hakkında](about-virtual-hub-routing.md).
2. **RT_VNet** yol tablosunu oluşturduğunuzda, aşağıdaki ayarları yapılandırın:

   * **İlişkilendirme**: yalıtmak istediğiniz sanal ağları seçin.
   * **Yayma**: dallara yönelik seçeneği belirleyin, diğer dal (VPN/er/P2S) bağlantıları bu yol tablosuna rotaları yayacaktır.

:::image type="content" source="./media/routing-scenarios/isolated/isolated-vnets.png" alt-text="Yalıtılmış VNET 'ler":::

## <a name="next-steps"></a>Sonraki adımlar

* Sanal WAN hakkında daha fazla bilgi için bkz. [SSS](virtual-wan-faq.md).
* Sanal hub yönlendirmesi hakkında daha fazla bilgi için bkz. [sanal hub yönlendirmesi hakkında](about-virtual-hub-routing.md).
