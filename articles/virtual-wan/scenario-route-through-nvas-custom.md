---
title: "Senaryo: özel ayarları kullanarak trafiği NVA 'lar aracılığıyla yönlendirme"
titleSuffix: Azure Virtual WAN
description: Bu senaryo, Internet 'e bağlanacak trafik için farklı bir NVA kullanarak trafiği NVA 'lar aracılığıyla yönlendirmenize yardımcı olur.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 08/06/2020
ms.author: cherylmc
ms.custom: fasttrack-edit
ms.openlocfilehash: f233aedf8b51967264994f5a4081f8f4cd99df01
ms.sourcegitcommit: 5a3b9f35d47355d026ee39d398c614ca4dae51c6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89400016"
---
# <a name="scenario-route-traffic-through-nvas---custom-preview"></a>Senaryo: trafiği NVA 'lar aracılığıyla yönlendirme-özel (Önizleme)

Sanal WAN sanal hub 'ı yönlendirme ile çalışırken, kullanılabilecek oldukça az sayıda senaryo vardır. Bu NVA (ağ sanal gereç) senaryosunda, hedef, trafiği VNET 'ler ve dallar arasındaki iletişim için bir NVA üzerinden yönlendirmektir ve Internet 'e yönelik trafik için farklı bir NVA kullanır. Sanal hub yönlendirmesi hakkında daha fazla bilgi için bkz. [sanal hub yönlendirmesi hakkında](about-virtual-hub-routing.md).

## <a name="design"></a><a name="design"></a>Tasarım

Bu senaryoda adlandırma kuralını kullanacağız:

* Internet olmayan trafiği incelemek için kullanıcıların bir NVA ( **Şekil 1**' de VNET) dağıttığının sanal ağları için "hizmet VNET".
* "DMZ VNet", kullanıcıların Internet 'e ait trafiği ( **Şekil 1**' de VNET 5) denetlemek için kullanılacak bir NVA dağıtmakta olduğu sanal ağlar için.
* Bir NVA VNet (VNet 1, VNet 2 ve **Şekil 1**' de VNET 3) bağlı sanal ağlar IÇIN "NVA ışınsal 'ler".
* Microsoft tarafından yönetilen sanal WAN hub 'Ları için "hub 'lar".

Aşağıdaki bağlantı matrisi, bu senaryoda desteklenen akışları özetler:

**Bağlantı matrisi**

| Kaynak          | Hedef:|*NVA tekerlek*|*Hizmet VNet*|*DMZ VNet*|*Dallar statik*|
|---|---|---|---|---|---|
| **NVA tekerlek**| &#8594;|      X |            X |   Eşleme |    Statik    |
| **Hizmet VNet**| &#8594;|    X |            X |      X    |      X       |
| **DMZ VNet** | &#8594;|       X |            X |      X    |      X       |
| **Dallar** | &#8594;|  Statik |            X |      X    |      X       |

Bağlantı matrisindeki hücrelerden her biri, bir sanal WAN bağlantısının (akışın "Kimden" tarafı, satır başlıkları) belirli bir trafik akışı için bir hedef ön eki (akışın "Kimden" tarafı, italik olan sütun üst bilgileri) öğrenip öğrenmediğini tanımlar. "X", bağlantının sanal WAN tarafından yerel olarak sağlandığı ve "static" anlamına gelir, bu da bağlantının statik yollar kullanılarak sanal WAN tarafından sağlandığı anlamına gelir. Farklı satırlarda ayrıntıya bakalım:

* NVA ışınsal 'ler:
  * Bağlı bileşen doğrudan sanal WAN hub 'ları üzerinde diğer bağlı ışınsal Lara ulaşacaktır.
  * Bağlı dallar, hizmet VNet 'e işaret eden statik bir yol aracılığıyla dallara bağlantı sağlar. Dallardan belirli önekleri öğrenmemelidir (Aksi halde bunlar daha belirgin olur ve Özeti geçersiz kılar).
  * Bağlı bileşen doğrudan VNet eşlemesi aracılığıyla DMZ VNet 'e Internet trafiği gönderecek.
* Dallar
  * Dallar, hizmet VNet 'e işaret eden statik bir yönlendirme aracılığıyla bağlı olarak yer alır. Özetlenen statik yolu geçersiz kılan sanal ağlardan belirli önekleri öğrenmemelidir.
* Hizmet sanal ağı, her VNet 'ten ve her dalda erişilebilir olması gereken bir paylaşılan hizmetler VNet 'ine benzer olacaktır.
* Yalnızca destekleyeceği trafik doğrudan VNet eşayarları üzerinden geldiği için DMZ VNet 'in sanal WAN üzerinden bağlantısı olması gerekmez. Ancak, yapılandırmayı basitleştirmek için DMZ VNet ile aynı bağlantı modelini kullanacağız.

Bu nedenle, bağlantı matrisi üç farklı bağlantı deseni sunar ve bu üç yol tablosuna çevrilir. Farklı VNET 'lere olan ilişkilendirmeler aşağıdaki gibi olacaktır:

* NVA ışınsal 'ler:
  * İlişkili yol tablosu: **RT_V2B**
  * Yol tablolarına yayma: **RT_V2B** ve **RT_SHARED**
* NVA sanal ağları (iç ve Internet):
  * İlişkili yol tablosu: **RT_SHARED**
  * Yol tablolarına yayma: **RT_SHARED**
* Dallar
  * İlişkili yol tablosu: **varsayılan**
  * Yol tablolarına yayma: **RT_SHARED** ve **varsayılan**

Sanal ağlar arası ve daldan VNet trafiğinin hizmet VNet 'teki NVA 'dan (VNet 4) geldiğinden emin olmak için bu statik yollara ihtiyacımız vardır:

| Description | Yol tablosu | Statik yol              |
| ----------- | ----------- | ------------------------- |
| Dallar    | RT_V2B      | 10.2.0.0/16-> vnet4conn  |
| NVA tekerlek  | Varsayılan     | 10.1.0.0/16-> vnet4conn  |

Artık sanal WAN, paketlerin gönderileceği bağlantıyı bilir, ancak bağlantının bu paketleri alırken ne yapılacağını bilmesi gerekir: bağlantı yolu tablolarının kullanıldığı yer.

| Description | Bağlantı | Statik yol            |
| ----------- | ---------- | ----------------------- |
| VNet2Branch | vnet4conn  | 10.2.0.0/16-> 10.4.0.5 |
| Branch2VNet | vnet4conn  | 10.1.0.0/16-> 10.4.0.5 |

Bu noktada her şey yerinde olmalıdır.

Sanal hub yönlendirmesi hakkında daha fazla bilgi için bkz. [sanal hub yönlendirmesi hakkında](about-virtual-hub-routing.md).

## <a name="architecture"></a><a name="architecture"></a>Mimari

**Şekil 1**' de bir hub, **hub 1**vardır.

* **Hub 1** , NVA sanal ağları **VNET 4** ve **VNET 5**' e doğrudan bağlanır.

* Sanal ağlar 1, 2 ve 3 ve dallar (VPN/ER/P2S) arasındaki trafiğin **VNET 4 NVA** 10.4.0.5 aracılığıyla gitmesi beklenir.

* Sanal ağlar 1, 2 ve 3 ' ten Internet 'e ait tüm trafiğin **VNET 5 NVA** 10.5.0.5 aracılığıyla gitmesi bekleniyor.

**Şekil 1**

:::image type="content" source="./media/routing-scenarios/nva-custom/figure-1.png" alt-text="Şekil 1":::

## <a name="workflow"></a><a name="workflow"></a>İş akışı

NVA aracılığıyla yönlendirmeyi ayarlamak için şunları göz önünde bulundurmanız gereken adımlar şunlardır:

1. Internet 'e bağlanan trafiğin VNet 5 üzerinden gitmesi için VNET eşlemesi ile VNet 5 ' e doğrudan bağlanmak için sanal ağlar 1, 2 ve 3 gerekir. Ayrıca 0.0.0.0/0 ve sonraki atlama 10.5.0.5 için VNET 'lerde ayarlanmış bir UDR gerekir. Şu anda sanal WAN, 0.0.0.0/0 için sanal hub 'da bir sonraki atlamaya izin vermiyor.

1. Azure portal, sanal hub 'ınıza gidin ve tüm VNET ve dal bağlantılarından yayma yoluyla rotaları öğrenmesi için bir özel yol tablosu oluşturun **RT_Shared** . **Şekil 2**' de, bu **RT_Shared**boş bir özel yol tablosu olarak gösterilmiştir.

   * **Rotalar:** Herhangi bir statik yol eklemeniz gerekmez.

   * **İlişkilendirme:** Vnetme 4 ve 5 ' i seçin. Bu, VNET 4 ve 5 bağlantılarının rota tablosu **RT_Shared**ile ilişkilendirilmesi anlamına gelir.

   * **Yayma:** Tüm dallar ve VNet bağlantılarının yollarını bu yol tablosuna dinamik olarak yaymasını istiyorsanız, dallar ve tüm sanal ağlar ' ı seçin.

1. Sanal ağlar 1, 2 ve 3 ' ten dallara giden trafiği yönlendirmek için **RT_V2B** özel bir yol tablosu oluşturun.

   * **Rotalar:** VNet 4 bağlantısı olarak sonraki atlamada, dallar (VPN/ER/P2S) (10.2.0.0/16 ' da **Şekil 2**) için toplanmış bir statik rota girişi ekleyin. Ayrıca, VNet 4 ' te şube ön ekleri için bir statik yol yapılandırmanız ve sonraki atlamanın VNet 4 ' te NVA 'nın belirli IP 'si olması gerektiğini belirtmeniz gerekir.

   * **İlişkilendirme:** Tüm VNET 'leri 1, 2 ve 3 ' ü seçin. Bu, 1, 2 ve 3 VNet bağlantılarının bu yol tablosuyla ilişkilendirileceğini ve bu yol tablosundaki yolları (yayma yoluyla statik ve dinamik) öğrenmenizi sağlar.

   * **Yayma:** Bağlantılar yolları rota tablolarına yayar. Vnetme 1, 2 ve 3 ' ün seçilmesi, sanal ağlar 1, 2 ve 3 ' ten bu yol tablosuna olan yolların yayılmasını sağlayacaktır. Şube VNet trafiği, VNet 4 ' te NVA üzerinden gitilerek, şube bağlantılarından RT_V2B yollar yaymaya gerek yoktur.
  
1. Varsayılan yol tablosu **Defaultroutetable**' i düzenleyin.

   Tüm VPN, ExpressRoute ve kullanıcı VPN bağlantıları, varsayılan yol tablosuyla ilişkilendirilir. Tüm VPN, ExpressRoute ve kullanıcı VPN bağlantıları, rotaları aynı yol tabloları kümesine yayar.

   * **Rotalar:** VNet 4 bağlantısı olarak sonraki atlamada, sanal ağlar 1, 2 ve 3 ( **Şekil 2**' de 10.1.0.0/16) için toplanmış bir statik rota girişi ekleyin. Ayrıca VNET 4, 2 ve 3 toplanmış ön ek için VNet 4 ' te bir statik yol yapılandırmanız ve sonraki atlamanın VNet 4 ' te NVA 'nın belirli IP 'si olması gerektiğini belirtmeniz gerekir.

   * **İlişkilendirme:** Dallar için seçeneğin (VPN/ER/P2S) seçildiğinden emin olun, şirket içi dal bağlantılarının *defaultroutetable*ile ilişkilendirilmesini sağlar.

   * **Yayma:** Şubelerin (VPN/ER/P2S) seçeneğinin belirlendiğinden emin olun, şirket içi bağlantıların, yolların *defaultroutetable*'a yayılmasını sağlar.

**Şekil 2**

:::image type="content" source="./media/routing-scenarios/nva-custom/figure-2.png" alt-text="Şekil 2" lightbox="./media/routing-scenarios/nva-custom/figure-2.png":::

## <a name="next-steps"></a>Sonraki adımlar

* Sanal WAN hakkında daha fazla bilgi için bkz. [SSS](virtual-wan-faq.md).
* Sanal hub yönlendirmesi hakkında daha fazla bilgi için bkz. [sanal hub yönlendirmesi hakkında](about-virtual-hub-routing.md).
