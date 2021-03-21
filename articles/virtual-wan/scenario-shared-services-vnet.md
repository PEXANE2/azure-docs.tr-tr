---
title: 'Senaryo: paylaşılan hizmetler sanal ağlarına yönlendirme'
titleSuffix: Azure Virtual WAN
description: Yönlendirme senaryoları-paylaşılan bir hizmet VNet 'e, her VNet ve dalın erişmesini istediğiniz bir iş yüküne erişmek için yolları ayarlayın.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 03/02/2021
ms.author: cherylmc
ms.custom: fasttrack-edit
ms.openlocfilehash: 8e0d05d2cb960e760809ab35a8f9e4ca04acf250
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102442970"
---
# <a name="scenario-route-to-shared-services-vnets"></a>Senaryo: paylaşılan hizmetler sanal ağlarına yönlendirme

Sanal WAN sanal hub 'ı yönlendirme ile çalışırken, kullanılabilecek oldukça az sayıda senaryo vardır. Bu senaryoda, hedef, her VNet ve dalın (VPN/ER/P2S) erişmesini istediğiniz iş yükleri ile **paylaşılan bir hizmet** VNET 'ine erişmek için yollar ayarlamaya yönelik olur. Bu paylaşılan iş yüklerine örnek olarak, etki alanı denetleyicileri veya dosya paylaşımları gibi hizmetlere sahip sanal makineler veya [Azure özel uç noktalar](../private-link/private-endpoint-overview.md)aracılığıyla dahili olarak kullanıma sunulan Azure hizmetleri bulunabilir.

Sanal hub yönlendirmesi hakkında daha fazla bilgi için bkz. [sanal hub yönlendirmesi hakkında](about-virtual-hub-routing.md).

## <a name="design"></a><a name="design"></a>Tasarım

Bu senaryonun gereksinimlerini özetlemek için bir bağlantı matrisi kullanabiliriz:

**Bağlantı matrisi**

| Kaynak             | Hedef:   |*Yalıtılmış VNET 'ler*|*Paylaşılan VNet*|*Dallar*|
|---|---|---|---|---|
|**Yalıtılmış VNET 'ler**| ->|        | Direct | Direct |
|**Paylaşılan sanal ağlar**  |->| Direct | Direct | Direct |
|**Dallar**      |->| Direct | Direct | Direct |

Önceki tablodaki hücrelerin her biri, bir sanal WAN bağlantısının (akışın "Kimden" tarafı, satır başlıkları) bir hedefle (akışın "to" tarafı, italik olan sütun üst bilgileri) iletişim kuracağını açıklar. Bu senaryoda, güvenlik duvarı veya ağ sanal gereçleri yoktur, bu nedenle iletişim doğrudan sanal WAN üzerinden akar (Bu nedenle tablodaki "doğrudan" sözcüğü).

[Yalıtılmış VNET senaryosuna](scenario-isolate-vnets.md)benzer şekilde, bu bağlantı matrisi iki farklı satır deseni sunar ve bu da iki yol tablosuna (paylaşılan hizmetler sanal ağları ve dallar aynı bağlantı gereksinimlerine sahiptir) çevirir. Sanal WAN zaten varsayılan bir yol tablosuna sahiptir, bu nedenle bu örnekteki **RT_SHARED** çağıracağız başka bir özel yol tablosuna ihtiyacımız olacak.

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

## <a name="workflow"></a><a name="workflow"></a>İş akışı

Senaryoyu yapılandırmak için aşağıdaki adımları göz önünde bulundurun:

1. **Paylaşılan hizmetler** VNET 'i tanımla.
2. Özel bir yol tablosu oluşturun. Örnekte, yol tablosuna **RT_SHARED** olarak başvurduk. Yol tablosu oluşturma adımları için bkz. [sanal hub yönlendirmeyi yapılandırma](how-to-virtual-hub-routing.md). Aşağıdaki değerleri bir kılavuz olarak kullanın:

   * **Kaldırma**
     * **Paylaşılan hizmetler VNET *hariç* sanal** ağlar Için, yalıtmak için sanal ağları seçin. Bu, tüm bu sanal ağların (paylaşılan hizmetler VNet hariç), RT_SHARED yol tablosunun yollarına göre hedefe ulaşabilmesi anlamına gelmez.

   * **Yayma**
      * **Dallar** için, daha önce seçmiş olduğunuz diğer yol tablolarının yanı sıra rotaları bu yol tablosuna yayın. Bu adım nedeniyle, RT_SHARED yol tablosu tüm dal bağlantılarından (VPN/ER/Kullanıcı VPN) yolları öğrenmeyecektir.
      * **Sanal** ağlar için, **paylaşılan hizmetler VNET**' i seçin. Bu adım nedeniyle RT_SHARED yol tablosu, paylaşılan hizmetler VNet bağlantısından yolları öğrenmeyecektir.

Bu, yönlendirme yapılandırmasına aşağıdaki şekilde gösterilen şekilde neden olur:

   :::image type="content" source="./media/routing-scenarios/shared-service-vnet/shared-services.png" alt-text="Paylaşılan hizmetler VNet için diyagram." lightbox="./media/routing-scenarios/shared-service-vnet/shared-services.png":::

## <a name="next-steps"></a>Sonraki adımlar

* ARM şablonunu kullanarak yapılandırmak için bkz. [hızlı başlangıç: ARM şablonunu kullanarak paylaşılan hizmetlere sanal ağlara yönlendirme](quickstart-route-shared-services-vnet-template.md).
* Sanal hub yönlendirmesi hakkında daha fazla bilgi için bkz. [sanal hub yönlendirmesi hakkında](about-virtual-hub-routing.md).
