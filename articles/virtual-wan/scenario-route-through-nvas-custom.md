---
title: "Senaryo: özel ayarları kullanarak trafiği NVA 'lar aracılığıyla yönlendirme"
titleSuffix: Azure Virtual WAN
description: Bu senaryo, Internet 'e bağlanacak trafik için farklı bir NVA kullanarak trafiği NVA 'lar aracılığıyla yönlendirmenize yardımcı olur.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 06/29/2020
ms.author: cherylmc
ms.openlocfilehash: 3fcad59bcfb75b8de6af14eae7e0bcb4962b17eb
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85569266"
---
# <a name="scenario-route-traffic-through-nvas---custom-preview"></a>Senaryo: trafiği NVA 'lar aracılığıyla yönlendirme-özel (Önizleme)

Sanal WAN sanal hub 'ı yönlendirme ile çalışırken, kullanılabilecek oldukça az sayıda senaryo vardır. Bu NVA (ağ sanal gereç) senaryosunda, hedef, trafiği VNet için bir NVA <-> dalı aracılığıyla yönlendirmektir ve Internet 'e yönelik trafik için farklı bir NVA kullanır. Sanal hub yönlendirmesi hakkında daha fazla bilgi için bkz. [sanal hub yönlendirmesi hakkında](about-virtual-hub-routing.md).

## <a name="scenario-architecture"></a><a name="architecture"></a>Senaryo mimarisi

**Şekil 1**' de bir hub, **hub 1**vardır.

* **Hub 1** , NVA sanal ağları **VNET 4** ve **VNET 5**' e doğrudan bağlanır.

* Sanal ağlar 1, 2 ve 3 ve dallar (VPN/ER/P2S) arasındaki trafiğin **VNET 4 NVA** 10.4.0.5 aracılığıyla gitmesi beklenir.

* Sanal ağlar 1, 2 ve 3 ' ten internet 'e ait tüm trafiğin **VNET 5 NVA** 10.5.0.5 aracılığıyla gitmesi bekleniyor.

**Şekil 1**

:::image type="content" source="./media/routing-scenarios/nva-custom/figure-1.png" alt-text="Şekil 1":::

## <a name="scenario-workflow"></a><a name="workflow"></a>Senaryo iş akışı

NVA aracılığıyla yönlendirmeyi ayarlamak için şunları göz önünde bulundurmanız gereken adımlar şunlardır:

1. Internet 'e bağlanan trafiğin VNET5 aracılığıyla gitmesi için VNET eşlemesi ile VNET 5 ' e doğrudan bağlanmak için sanal ağlar 1, 2 ve 3 gerekir. Ayrıca 0.0.0.0/0 ve sonraki atlama 10.5.0.5 için VNET 'lerde ayarlanmış bir UDR gerekir. Şu anda sanal WAN, 0.0.0.0/0 için sanal hub 'da bir sonraki atlamaya izin vermiyor.

1. Azure portal, sanal hub 'ınıza gidin ve tüm VNET ve dal bağlantılarından yayma yoluyla rotaları öğrenmesi için bir özel yol tablosu oluşturun **RT_Shared** . **Şekil 2**' de, bu **RT_Shared**boş bir özel yol tablosu olarak gösterilmiştir.

   * **Rotalar:** Herhangi bir statik yol eklemeniz gerekmez.

   * **İlişkilendirme:** Vnetme 4 ve 5 ' i seçin. Bu, VNET 4 ve 5 bağlantılarının rota tablosu **RT_Shared**ile ilişkilendirilmesi anlamına gelir.

   * **Yayma:** Tüm dallar ve VNet bağlantılarının yollarını bu yol tablosuna dinamik olarak yaymasını istiyorsanız, dallar ve tüm sanal ağlar ' ı seçin.

1. Sanal ağlar 1, 2 ve 3 ' ten dallara giden trafiği yönlendirmek için **RT_V2B** özel bir yol tablosu oluşturun.

   * **Rotalar:** VNET 4 bağlantısı olarak sonraki atlamada, dallar (VPN/ER/P2S) (10.2.0.0/16 ' da **Şekil 2**) için toplanmış bir statik rota girişi ekleyin. Ayrıca, VNET 4 ' te şube ön ekleri için bir statik yol yapılandırmanız ve sonraki atlamanın VNET 4 ' te NVA 'nın belirli IP 'si olması gerektiğini belirtmeniz gerekir.

   * **İlişkilendirme:** Tüm VNET 'leri 1, 2 ve 3 ' ü seçin. Bu, 1, 2 ve 3 VNet bağlantılarının bu yol tablosuyla ilişkilendirileceğini ve bu yol tablosundaki yolları (yayma yoluyla statik ve dinamik) öğrenmenizi sağlar.

   * **Yayma:** Bağlantılar yolları rota tablolarına yayar. Vnetme 1, 2 ve 3 ' ün seçilmesi, sanal ağlar 1, 2 ve 3 ' ten bu yol tablosuna olan yolların yayılmasını sağlayacaktır. Şube VNet trafiği, Ile VNET4 arasında içindeki NVA üzerinden geçilerek, şube bağlantılarından RT_V2B yollar yaymaya gerek yoktur.
  
1. Varsayılan yol tablosu **Defaultroutetable**' i düzenleyin.

   Tüm VPN, ExpressRoute ve kullanıcı VPN bağlantıları, varsayılan yol tablosuyla ilişkilendirilir. Tüm VPN, ExpressRoute ve kullanıcı VPN bağlantıları, rotaları aynı yol tabloları kümesine yayar.

   * **Rotalar:** VNET 4 bağlantısı olarak sonraki atlamada, sanal ağlar 1, 2 ve 3 ( **Şekil 2**' de 10.1.0.0/16) için toplanmış bir statik rota girişi ekleyin. Ayrıca VNET 4, 2 ve 3 toplanmış ön ek için VNET 4 ' te bir statik yol yapılandırmanız ve sonraki atlamanın VNET 4 ' te NVA 'nın belirli IP 'si olması gerektiğini belirtmeniz gerekir.

   * **İlişkilendirme:** Dallar için seçeneğin (VPN/ER/P2S) seçildiğinden emin olun, şirket içi dal bağlantılarının *defaultroutetable*ile ilişkilendirilmesini sağlar.

   * **Yayma:** Şubelerin (VPN/ER/P2S) seçeneğinin belirlendiğinden emin olun, şirket içi bağlantıların, yolların *defaultroutetable*'a yayılmasını sağlar.

**Şekil 2**

:::image type="content" source="./media/routing-scenarios/nva-custom/figure-2.png" alt-text="Şekil 2":::

## <a name="next-steps"></a>Sonraki adımlar

* Sanal WAN hakkında daha fazla bilgi için bkz. [SSS](virtual-wan-faq.md).
* Sanal hub yönlendirmesi hakkında daha fazla bilgi için bkz. [sanal hub yönlendirmesi hakkında](about-virtual-hub-routing.md).
