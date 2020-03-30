---
title: Azure Sanal WAN iş ortakları ve konumları | Microsoft Dokümanlar
description: Bu makalede, Azure Sanal WAN iş ortakları ve hub konumlarının bir listesi yer almaktadır.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 02/12/2020
ms.author: cherylmc
Customer intent: As someone with a networking background, I want to find a Virtual WAN partner
ms.openlocfilehash: 3976f4114df6222d34f19e2bb44fd05d53057321
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80123283"
---
# <a name="virtual-wan-partners-and-virtual-hub-locations"></a>Sanal WAN iş ortakları ve sanal hub konumları

Bu makalede, Sanal Hub'a bağlantı için Virtual WAN desteklenen bölgeler ve iş ortakları hakkında bilgi verilmektedir.

Azure Sanal WAN, Azure aracılığıyla şubeden şubeye iyileştirilmiş ve otomatik bağlantı sağlayan bir ağ hizmetidir. Sanal WAN, Azure ile iletişim kurmak için dal cihazlarını bağlamanızı ve yapılandırmanızı sağlar. Bu, el ile veya sağlayıcı aygıtlarını Sanal WAN iş ortağı aracılığıyla kullanarak yapılabilir. İş ortağı aygıtlarını kullanmak, kullanımınızı kolaylaştırmanızı, bağlantının basitleştirilmesini ve yapılandırma yönetimi yapmanızı sağlar.

Şirket içi aygıttan bağlantı, Sanal Hub'a otomatik bir şekilde kurulur. Sanal hub, Microsoft tarafından yönetilen bir sanal ağdır. Hub'da, şirket içi ağınızdan (vpnsite) gelen bağlantıyı etkinleştirmek için çeşitli hizmet uç noktaları bulunur. Bölge başına yalnızca bir hub'ınolabilir.

## <a name="automation-from-connectivity-partners"></a><a name="automation"></a>Bağlantı ortaklarından otomasyon

Azure Virtual WAN'a bağlanan aygıtlar bağlanmak için yerleşik otomasyona sahiptir. Bu genellikle, VPN dal aygıtı arasındaki bağlantı ve yapılandırma yönetimini Azure Sanal Hub bitiş noktasına (VPN ağ geçidi) ayarlayan aygıt yönetimi ui (veya eşdeğeri) olarak ayarlanır.

Aşağıdaki üst düzey otomasyon cihaz konsolu/yönetim merkezinde ayarlanır:

* Azure Sanal WAN Kaynak Grubuna erişmek için aygıt için uygun izinler
* Şube Aygıtının Azure Virtual WAN'a Yüklenmesi
* Azure bağlantı bilgilerinin otomatik olarak indirimi
* Şirket içi şube aygıtının konfigürasyonu 

Bazı bağlantı ortakları otomasyonu Azure Sanal Hub VNet ve VPN Ağ Geçidi oluşturmayı da kapsayacak şekilde genişletebilir. Otomasyon hakkında daha fazla bilgi almak istiyorsanız, [Virtual WAN iş ortakları için Otomasyon yönergelerine](virtual-wan-configure-automation-providers.md)bakın.

## <a name="connectivity-through-partners"></a><a name="partners"></a>İş ortakları aracılığıyla bağlantı

[!INCLUDE [partners](../../includes/virtual-wan-partners-include.md)]

128 Technologies, Arista, Aruba HPE, Cisco Systems, F5 Networks, Open Systems, Oracle SD-WAN, SharpLink ve VMWare Velocloud: Aşağıdaki iş ortakları yakın gelecek için yol haritası üzerinde tasarlanmıştır.

## <a name="locations"></a><a name="locations"></a>Konumlar

[!INCLUDE [regions](../../includes/virtual-wan-regions-include.md)]

## <a name="next-steps"></a>Sonraki adımlar

* Virtual WAN hakkında daha fazla bilgi için [Virtual WAN SSS](virtual-wan-faq.md)bölümüne bakın.

* Azure Virtual WAN'a bağlantı otomatikleştirme hakkında daha fazla bilgi için [Virtual WAN iş ortakları için Otomasyon yönergelerine](virtual-wan-configure-automation-providers.md)bakın.
