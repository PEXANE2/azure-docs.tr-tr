---
title: 'Senaryo: paylaşılan hizmetler sanal ağlarına yönlendirme'
titleSuffix: Azure Virtual WAN
description: Yönlendirme senaryoları-paylaşılan bir hizmet VNet 'e, her VNet ve dalın erişmesini istediğiniz bir iş yüküne erişmek için yolları ayarlayın.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 08/07/2020
ms.author: cherylmc
ms.custom: fasttrack-edit
ms.openlocfilehash: 6045c491ea68d759b2a1739e20aa2f12b8520c87
ms.sourcegitcommit: 98854e3bd1ab04ce42816cae1892ed0caeedf461
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/07/2020
ms.locfileid: "88006494"
---
# <a name="scenario-route-to-shared-services-vnets"></a>Senaryo: paylaşılan hizmetler sanal ağlarına yönlendirme

Sanal WAN sanal hub 'ı yönlendirme ile çalışırken, kullanılabilecek oldukça az sayıda senaryo vardır. Bu senaryoda, hedef, her VNet ve dalın (VPN/ER/P2S) erişmesini istediğiniz iş yükleri ile **paylaşılan bir hizmet** VNET 'ine erişmek için yollar ayarlamaya yönelik olur. Bu paylaşılan iş yüklerine örnek olarak, etki alanı denetleyicileri veya dosya paylaşımları gibi hizmetlere sahip sanal makineler veya [Azure özel uç noktalar](../private-link/private-endpoint-overview.md)aracılığıyla dahili olarak kullanıma sunulan Azure hizmetleri bulunabilir.

Sanal hub yönlendirmesi hakkında daha fazla bilgi için bkz. [sanal hub yönlendirmesi hakkında](about-virtual-hub-routing.md).

## <a name="design"></a><a name="design"></a>Tasarım

Bu senaryonun gereksinimlerini özetlemek için bir bağlantı matrisi kullanabiliriz. Matriste her hücrede, belirli bir trafik akışı için bir sanal WAN bağlantısının (akışın "Kimden" tarafı, tablodaki satır başlıkları) bir hedef ön eki (akışın "to" tarafı, tablodaki sütun üst bilgileri) öğrenip öğrenmediğini açıklanmaktadır.

**Bağlantı matrisi**

| Kaynak             | Hedef:   |*Yalıtılmış VNET 'ler*|*Paylaşılan VNet*|*Dallar*|
|---|---|---|---|---|
|**Yalıtılmış VNET 'ler**|&#8594;|                |        X        |       X      |
|**Paylaşılan sanal ağlar**  |&#8594;|       X        |        X        |       X      |
|**Dallar**      |&#8594;|       X        |        X        |       X      |

[Yalıtılmış VNET senaryosuna](scenario-isolate-vnets.md)benzer şekilde, bu bağlantı matrisi, iki yol tablosuna (paylaşılan hizmetler sanal ağları ve dallar aynı bağlantı gereksinimlerine sahiptir) çeviren iki farklı satır deseni sunar. Sanal WAN zaten varsayılan bir yol tablosuna sahiptir, bu nedenle bu örnekteki **RT_SHARED** çağıracağız başka bir özel yol tablosuna ihtiyacımız olacak.

VNET 'ler **RT_SHARED** yol tablosuyla ilişkilendirilir. Dallara ve paylaşılan hizmet sanal ağlarına bağlantı gerektirdiğinden, paylaşılan hizmet VNet ve dalların **RT_SHARED** 'e yayılması gerekir (Aksi halde sanal ağlar dalı ve paylaşılan VNET öneklerini öğrenmez). Dallar her zaman varsayılan yol tablosuyla ilişkilendirildiğinden ve bağlantı gereksinimleri paylaşılan hizmetler sanal ağları için aynıysa, paylaşılan hizmet VNET 'leri varsayılan yol tablosuyla de ilişkilendireceğiz.

Sonuç olarak, bu son tasarımdır:

* Yalıtılmış sanal ağlar:
  * İlişkili yol tablosu: **RT_SHARED**
  * Yol tablolarına yayma: **varsayılan**
* Paylaşılan hizmetler sanal ağları:
  * İlişkili yol tablosu: **varsayılan**
  * Yol tablolarına yayma: **RT_SHARED** ve **varsayılan**
* Dallar
  * İlişkili yol tablosu: **varsayılan**
  * Yol tablolarına yayma: **RT_SHARED** ve **varsayılan**

> [!NOTE]
> Sanal WAN 'ınız birden çok bölgeye dağıtılmışsa, her hub 'da **RT_SHARED** yol tablosu oluşturmanız ve her bir paylaşılan hizmet VNET ve dal bağlantısının, yayma etiketleri kullanılarak her sanal hub 'daki yol tablolarına yayılması gerekir.

Sanal hub yönlendirmesi hakkında daha fazla bilgi için bkz. [sanal hub yönlendirmesi hakkında](about-virtual-hub-routing.md).

## <a name="workflow"></a><a name="workflow"></a>Akışıyla

Senaryoyu yapılandırmak için aşağıdaki adımları göz önünde bulundurun:

1. **Paylaşılan hizmetler** VNET 'i tanımla.
2. Özel bir yol tablosu oluşturun. Örnekte, yol tablosuna **RT_SHARED**olarak başvurduk. Yol tablosu oluşturma adımları için bkz. [sanal hub yönlendirmeyi yapılandırma](how-to-virtual-hub-routing.md). Aşağıdaki değerleri bir kılavuz olarak kullanın:

   * **Kaldırma**
     * **Paylaşılan hizmetler VNET *hariç* sanal**ağlar Için, yalıtmak için sanal ağları seçin. Bu, tüm bu sanal ağların (paylaşılan hizmetler VNet hariç), RT_SHARED yol tablosunun yollarına göre hedefe ulaşabilmesi anlamına gelmez.

   * **Yayma**
      * **Dallar**için, daha önce seçmiş olduğunuz diğer yol tablolarının yanı sıra rotaları bu yol tablosuna yayın. Bu adım nedeniyle, RT_SHARED yol tablosu tüm dal bağlantılarından (VPN/ER/Kullanıcı VPN) yolları öğrenmeyecektir.
      * **Sanal**ağlar Için, **paylaşılan hizmetler VNET**' i seçin. Bu adım nedeniyle RT_SHARED yol tablosu, paylaşılan hizmetler VNet bağlantısından yolları öğrenmeyecektir.

Bu, yönlendirme yapılandırmasına aşağıdaki şekilde gösterilen şekilde neden olur:

   :::image type="content" source="./media/routing-scenarios/shared-service-vnet/shared-services.png" alt-text="Paylaşılan hizmetler VNet" lightbox="./media/routing-scenarios/shared-service-vnet/shared-services.png":::

## <a name="next-steps"></a>Sonraki adımlar

* Sanal WAN hakkında daha fazla bilgi için bkz. [SSS](virtual-wan-faq.md).
* Sanal hub yönlendirmesi hakkında daha fazla bilgi için bkz. [sanal hub yönlendirmesi hakkında](about-virtual-hub-routing.md).
