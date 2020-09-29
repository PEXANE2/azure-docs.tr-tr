---
title: Azure sanal WAN iş ortakları ve konumları | Microsoft Docs
description: Bu makale, Azure sanal WAN iş ortaklarının ve hub konumlarının bir listesini içerir.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 09/22/2020
ms.author: cherylmc
Customer intent: As someone with a networking background, I want to find a Virtual WAN partner
ms.openlocfilehash: e8dcb188dc5aad59738e3eab4169bf9dbd3679ec
ms.sourcegitcommit: 3792cf7efc12e357f0e3b65638ea7673651db6e1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/29/2020
ms.locfileid: "91449349"
---
# <a name="virtual-wan-partners-and-virtual-hub-locations"></a>Sanal WAN iş ortakları ve sanal hub konumları

Bu makalede, sanal WAN ile desteklenen bölgeler ve sanal hub 'a bağlantı için iş ortakları hakkında bilgi sağlanır.

Azure Sanal WAN, Azure aracılığıyla şubeden şubeye iyileştirilmiş ve otomatik bağlantı sağlayan bir ağ hizmetidir. Sanal WAN, Azure ile iletişim kurmak için dal cihazlarını bağlamanızı ve yapılandırmanızı sağlar. Bu, el ile veya bir sanal WAN iş ortağı aracılığıyla sağlayıcı cihazları kullanılarak yapılabilir. İş ortağı cihazlarının kullanımı, kullanım kolaylığı, bağlantı basitleştirmesi ve yapılandırma yönetimi sağlar.

Şirket içi cihazdan bağlantı, sanal hub 'a yönelik otomatikleştirilmiş bir şekilde oluşturulur. Bir sanal hub, Microsoft tarafından yönetilen bir sanal ağ. Hub'da, şirket içi ağınızdan (vpnsite) gelen bağlantıyı etkinleştirmek için çeşitli hizmet uç noktaları bulunur. Her bölge için yalnızca bir hub olabilir.

## <a name="branch-ipsec-connectivity-automation-from-partners"></a><a name="automation"></a>İş ortaklarından şube IPSec bağlantısı Otomasyonu

Azure sanal WAN 'a bağlanan cihazlarda bağlantı kurmak için yerleşik Otomasyon vardır. Bu, genellikle VPN dalı cihazı ile bir Azure sanal hub VPN uç noktası (VPN Gateway) arasında bağlantı ve yapılandırma yönetimini ayarlayan cihaz yönetimi Kullanıcı arabiriminde (veya eşdeğer) ayarlanır.

Aşağıdaki üst düzey Otomasyon cihaz konsolunda/Yönetim merkezinde ayarlanır:

* Cihazın Azure sanal WAN kaynak grubuna erişmesi için uygun izinler
* Şube cihazını Azure sanal WAN 'a yükleme
* Azure bağlantı bilgilerini otomatik olarak indirme
* Şirket içi şube cihazının yapılandırması 

Bazı bağlantı ortakları, otomasyonu, Azure sanal hub VNet ve VPN Gateway oluşturmayı içerecek şekilde genişletebilir. Otomasyon hakkında daha fazla bilgi edinmek istiyorsanız bkz. [sanal WAN iş ortakları Için Otomasyon yönergeleri](virtual-wan-configure-automation-providers.md).

## <a name="branch-ipsec-connectivity-partners"></a><a name="partners"></a>Şube IPSec bağlantısı iş ortakları

[!INCLUDE [partners](../../includes/virtual-wan-partners-include.md)]

Aşağıdaki iş ortakları, yakın gelecekte 128 Technologies, Arista, Cisco Systems (vınla), F5 Networks, Oracle SD-WAN ve keskin bağlantı için yol haritasında alınmıştır.

## <a name="partners-with-integrated-virtual-hub-offerings"></a>Tümleşik sanal hub teklifleri olan iş ortakları
Otomatik şube ofis IPSec bağlantısına sahip olmanın yanı sıra, bazı iş ortakları doğrudan Azure sanal WAN hub 'ına tümleştirilen **ağ sanal gereçlerini (NVA 'lar)** sunar.  Bu, müşterilere şube bağlantılarını sanal hub 'daki uyumlu bir üçüncü taraf gerecine sonlandırma seçeneği sağlar.  

Sanal WAN hub 'ında NVA 'yı sunan iş ortakları şunları içermelidir:

* , Şube cihazlarından IPSec bağlantı Otomasyonu uyguladık ve Azure sanal WAN hub 'ına ait NVA tekliflerini eklenmediyse.
* Azure Marketi 'nde mevcut bir ağ sanal gereç teklifinin olması gerekir.

Bir iş ortağıysanız ve sanal hub teklifinde yönetilen NVA hakkında sorularınız varsa, lütfen adresine e-posta göndererek bizimle iletişime geçin vwannvaonboarding@microsoft.com

## <a name="integrated-virtual-hub-nva-partners"></a>Tümleşik sanal hub NVA iş ortakları
Bu iş ortakları, artık sanal WAN hub 'ına dağıtım için kullanılabilen **yönetilen uygulama** tekliflerine sahiptir.

|İş Ortakları|Yapılandırma/nasıl yapılır/dağıtım kılavuzu|
|---|---|
|[Barracuda Networks](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/barracudanetworks.barracuda_cloudgenwan_gateway?tab=Overviewus/marketplace/apps/barracudanetworks.barracuda_cloudgenwan_gateway?tab=Overview)| [Barrampada CloudGen WAN dağıtım kılavuzu](https://campus.barracuda.com/product/cloudgenwan/doc/91980640/deployment/)|
|[Cisco bulut hizmeti yönlendiricisi (CSR) VWAN](https://aka.ms/ciscoMarketPlaceOffer)| Cisco Cloud Services (CSR) WAN 'ı sanal hub 'a dağıtmak için lütfen e-posta gönderin vwan_public_preview@cisco.com |

Aşağıdaki iş ortakları, sanal hub 'daki NVA 'yı yakın gelecekte sunmakta olacak şekilde hazırlanmıştır: Citrix, tersi ağlar ve VeloCloud.

## <a name="locations"></a><a name="locations"></a>Konumlar

[!INCLUDE [regions](../../includes/virtual-wan-regions-include.md)]

## <a name="next-steps"></a>Sonraki adımlar

* Sanal WAN hakkında daha fazla bilgi için bkz. [sanal WAN hakkında SSS](virtual-wan-faq.md).

* Azure sanal WAN bağlantısını otomatikleştirme hakkında daha fazla bilgi için bkz. [sanal WAN iş ortakları Için Otomasyon yönergeleri](virtual-wan-configure-automation-providers.md).
