---
title: 'Senaryo: sanal ağlar için özel yalıtım'
titleSuffix: Azure Virtual WAN
description: Yönlendirme senaryoları-seçili VNET 'lerin birbirlerine ulaşabilmesi önlenir
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 06/29/2020
ms.author: cherylmc
ms.openlocfilehash: 3719956df0dce62ee69d8e306ff2cad27197616d
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85569019"
---
# <a name="scenario-custom-isolation-for-vnets"></a>Senaryo: sanal ağlar için özel yalıtım

Sanal WAN sanal hub 'ı yönlendirme ile çalışırken, kullanılabilecek oldukça az sayıda senaryo vardır. Sanal ağlar için özel bir yalıtım senaryosunda, amaç belirli bir sanal ağ kümesinin diğer belirli sanal ağlar kümesine ulaşabilme zorunluluğunu önlemektir. Ancak, tüm dallara ulaşmak için sanal ağlar gereklidir (VPN/ER/Kullanıcı VPN).

Bu senaryoda, VPN, ExpressRoute ve kullanıcı VPN bağlantıları (her ikisi de dal olarak adlandırılır) aynı yol tablosu (varsayılan yol tablosu) ile ilişkilendirilir. Tüm VPN, ExpressRoute ve kullanıcı VPN bağlantıları, rotaları aynı yol tabloları kümesine yayar. Sanal hub yönlendirmesi hakkında daha fazla bilgi için bkz. [sanal hub yönlendirmesi hakkında](about-virtual-hub-routing.md).

## <a name="scenario-workflow"></a><a name="architecture"></a>Senaryo iş akışı

**Şekil 1**' de mavi ve Red VNET bağlantıları vardır.

* Mavi bağlantılı VNET 'ler birbirine ulaşabilir ve tüm dallar (VPN/ER/P2S) bağlantılarına ulaşabilirler.
* Kırmızı VNET 'ler birbirine ulaşabilir ve tüm dallar (VPN/ER/P2S) bağlantılarına ulaşabilirler.

Yönlendirmeyi ayarlarken aşağıdaki adımları göz önünde bulundurun.

1. Azure portal, **RT_BLUE** ve **RT_RED**iki özel yol tablosu oluşturun.
2. Yol tablosu **RT_BLUE**için:
   * **İlişkilendirme**: tüm mavi VNET 'leri seçin
   * **Yayma**: dallar için, dalların seçeneğini belirleyin, ımpber DALı (VPN/er/P2S) bağlantıları bu yol tablosuna yolları yayar.
3. Kırmızı sanal ağlar ve dallar (VPN/ER/P2S) için **RT_RED** yol tablosu için aynı adımları yineleyin.

Bu, aşağıdaki şekilde görüldüğü gibi yönlendirme yapılandırması değişikliklerine neden olur

**Şekil 1**

:::image type="content" source="./media/routing-scenarios/custom-isolation/custom.png" alt-text="Şekil 1":::

## <a name="next-steps"></a>Sonraki adımlar

* Sanal WAN hakkında daha fazla bilgi için bkz. [SSS](virtual-wan-faq.md).
* Sanal hub yönlendirmesi hakkında daha fazla bilgi için bkz. [sanal hub yönlendirmesi hakkında](about-virtual-hub-routing.md).
