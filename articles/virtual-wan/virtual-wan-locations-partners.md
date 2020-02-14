---
title: Azure sanal WAN iş ortakları ve konumları | Microsoft Docs
description: Bu makale, Azure sanal WAN iş ortaklarının ve hub konumlarının bir listesini içerir.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 02/12/2020
ms.author: cherylmc
Customer intent: As someone with a networking background, I want to find a Virtual WAN partner
ms.openlocfilehash: 68379fd2334e517df4011c05bb32543f7ce4a173
ms.sourcegitcommit: b07964632879a077b10f988aa33fa3907cbaaf0e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2020
ms.locfileid: "77190397"
---
# <a name="virtual-wan-partners-and-virtual-hub-locations"></a>Sanal WAN iş ortakları ve sanal hub konumları

Bu makalede, sanal WAN ile desteklenen bölgeler ve sanal hub 'a bağlantı için iş ortakları hakkında bilgi sağlanır.

Azure Sanal WAN, Azure aracılığıyla şubeden şubeye iyileştirilmiş ve otomatik bağlantı sağlayan bir ağ hizmetidir. Sanal WAN, Azure ile iletişim kurmak için dal cihazlarını bağlamanızı ve yapılandırmanızı sağlar. Bu, el ile veya bir sanal WAN iş ortağı aracılığıyla sağlayıcı cihazları kullanılarak yapılabilir. İş ortağı cihazlarının kullanımı, kullanım kolaylığı, bağlantı basitleştirmesi ve yapılandırma yönetimi sağlar.

Şirket içi cihazdan bağlantı, sanal hub 'a yönelik otomatikleştirilmiş bir şekilde oluşturulur. Bir sanal hub, Microsoft tarafından yönetilen bir sanal ağ. Hub'da, şirket içi ağınızdan (vpnsite) gelen bağlantıyı etkinleştirmek için çeşitli hizmet uç noktaları bulunur. Her bölge için yalnızca bir hub olabilir.

## <a name="automation"></a>Bağlantı iş ortaklarından Otomasyon

Azure sanal WAN 'a bağlanan cihazlarda bağlantı kurmak için yerleşik Otomasyon vardır. Bu, genellikle VPN dalı cihazı ile bir Azure sanal hub VPN uç noktası (VPN Gateway) arasında bağlantı ve yapılandırma yönetimini ayarlayan cihaz yönetimi Kullanıcı arabiriminde (veya eşdeğer) ayarlanır.

Aşağıdaki üst düzey Otomasyon cihaz konsolunda/Yönetim merkezinde ayarlanır:

* Cihazın Azure sanal WAN kaynak grubuna erişmesi için uygun izinler
* Şube cihazını Azure sanal WAN 'a yükleme
* Azure bağlantı bilgilerini otomatik olarak indirme
* Şirket içi şube cihazının yapılandırması 

Bazı bağlantı ortakları, otomasyonu, Azure sanal hub VNet ve VPN Gateway oluşturmayı içerecek şekilde genişletebilir. Otomasyon hakkında daha fazla bilgi edinmek istiyorsanız bkz. [sanal WAN iş ortakları Için Otomasyon yönergeleri](virtual-wan-configure-automation-providers.md).

## <a name="partners"></a>İş ortakları üzerinden bağlantı

[!INCLUDE [partners](../../includes/virtual-wan-partners-include.md)]

Aşağıdaki iş ortakları, yakın gelecekte yer alan yol haritası üzerinde alınmıştır: Arista, Aruba HPE, Cisco sistemleri, F5 Networks, açık sistemler, Oracle SD-WAN, Keskinlink ve VMWare Velocloud.

## <a name="locations"></a>Yerlerini

[!INCLUDE [regions](../../includes/virtual-wan-regions-include.md)]

## <a name="next-steps"></a>Sonraki adımlar

* Sanal WAN hakkında daha fazla bilgi için bkz. [sanal WAN hakkında SSS](virtual-wan-faq.md).

* Azure sanal WAN bağlantısını otomatikleştirme hakkında daha fazla bilgi için bkz. [sanal WAN iş ortakları Için Otomasyon yönergeleri](virtual-wan-configure-automation-providers.md).
