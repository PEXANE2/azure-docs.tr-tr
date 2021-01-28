---
title: Özel ayarları kullanarak trafiği NVA 'lar aracılığıyla yönlendirme
titleSuffix: Azure Virtual WAN
description: Bu senaryo, internet 'e bağlanacak trafik için farklı bir NVA kullanarak trafiği NVA 'lar aracılığıyla yönlendirmenize yardımcı olur.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 09/22/2020
ms.author: cherylmc
ms.custom: fasttrack-edit
ms.openlocfilehash: 8e51d7d00120f6facb0fb53a8e379d157ae79ea4
ms.sourcegitcommit: 2f9f306fa5224595fa5f8ec6af498a0df4de08a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/28/2021
ms.locfileid: "98938574"
---
# <a name="scenario-route-traffic-through-nvas-by-using-custom-settings"></a>Senaryo: özel ayarları kullanarak trafiği NVA 'lar aracılığıyla yönlendirme

Azure sanal WAN sanal hub 'ı yönlendirme ile çalışırken kullanabileceğiniz çeşitli seçenekleriniz vardır. Bu makalenin odağı, sanal ağlar ve dallar arasındaki iletişim için trafiği bir ağ sanal gereci (NVA) ile yönlendirmek ve internet 'e yönelik trafik için farklı bir NVA kullanmak istediğinizde kullanılır. Daha fazla bilgi için bkz. [sanal hub yönlendirmesi hakkında](about-virtual-hub-routing.md).

## <a name="design"></a>Tasarım

* Sanal hub 'a bağlı sanal ağlar için **ışınsal** . (Örneğin, VNet 1, VNet 2 ve bu makalenin ilerleyen bölümlerindeki diyagramda VNet 3.)
* Kullanıcıların İnternet dışı trafiği incelemek için bir NVA dağıtmakta olduğu sanal ağlar için **hizmet sanal** ağı ve büyük olasılıkla bağlı bileşenler tarafından erişilen ortak hizmetler. (Örneğin, bu makalenin ilerleyen bölümlerindeki diyagramda VNet 4.) 
* Kullanıcıların İnternet 'e ait trafiği denetlemek için kullanılacak bir NVA dağıtdıkları sanal ağlar için **çevre VNET** . (Örneğin, bu makalenin ilerleyen bölümlerindeki diyagramda VNet 5.)
* Microsoft tarafından yönetilen sanal WAN hub 'ları için **hub 'lar** .

Aşağıdaki tabloda, bu senaryoda desteklenen bağlantılar özetlenmektedir:

| Kaynak          | Amaç|Bileşenler|Hizmet VNet|Dallar|İnternet|
|---|---|:---:|:---:|:---:|:---:|:---:|
| **Bileşenler**| ->| hemen |hemen | Hizmet VNet üzerinden |Çevre VNet üzerinden |
| **Hizmet VNet**| ->| hemen |yok| hemen | |
| **Dallar** | ->| Hizmet VNet üzerinden |hemen| hemen |  |

Bağlantı matrisindeki her bir hücre, bağlantının doğrudan sanal WAN veya bir NVA ile sanal ağlardan biri üzerinden akışı yapılıp yapılmayacağını açıklar. 

Aşağıdaki bilgileri unutmayın:
* Bileşen
  * Bağlı bileşen doğrudan sanal WAN hub 'ları üzerinde diğer bağlı ışınsal Lara ulaşacaktır.
  * Bağlı dallar, hizmet VNet 'e işaret eden statik bir yol aracılığıyla dallara bağlantı sağlar. Bunlar daha belirgin olduklarından ve Özeti geçersiz kıldığından, dallardan belirli önekleri öğrenirler.
  * Bağlı bileşen, doğrudan VNet eşlemesi aracılığıyla çevre VNet 'e internet trafiği gönderecek.
* Dallar, hizmet VNet 'e işaret eden statik bir yönlendirme aracılığıyla bağlı olarak yer alır. Özetlenen statik yolu geçersiz kılan sanal ağlardan belirli önekleri öğrenirler.
* Hizmet VNet, her sanal ağ ve her dalda erişilebilir olması gereken paylaşılan bir hizmet VNet 'ine benzer olacaktır.
* Yalnızca destekleyeceği trafik doğrudan sanal ağ eşayarları üzerinden geldiğinden, çevre VNet 'in sanal WAN üzerinden bağlantısı olması gerekmez. Ancak yapılandırmayı basitleştirmek için çevre VNet için ile aynı bağlantı modelini kullanın.

Üç farklı bağlantı deseni vardır ve bu üç yol tablosuna çevrilir. Farklı sanal ağlarla ilişkiler şunlardır:

* Bileşen
  * İlişkili yol tablosu: **RT_V2B**
  * Yol tablolarına yayma: **RT_V2B** ve **RT_SHARED**
* NVA sanal ağları (hizmet VNet ve DMZ VNet):
  * İlişkili yol tablosu: **RT_SHARED**
  * Yol tablolarına yayma: **RT_SHARED**
* Dallar
  * İlişkili yol tablosu: **varsayılan**
  * Yol tablolarına yayma: **RT_SHARED** ve **varsayılan**

> [!NOTE] 
> Lütfen bağlı olan sanal ağların varsayılan etikete yaymadığından emin olun. Bu, dallardan bağlı olan sanal ağlara giden trafiğin NVA 'lar 'e iletilmesini sağlar.

Bu statik yollar, sanal ağa ve daldan gelen ve giden trafiğin hizmet VNet 'inde (VNet 4) NVA üzerinden geçmemesini sağlar:

| Description | Yol tablosu | Statik yol              |
| ----------- | ----------- | ------------------------- |
| Dallar    | RT_V2B      | 10.2.0.0/16-> vnet4conn  |
| NVA tekerlek  | Varsayılan     | 10.1.0.0/16-> vnet4conn  |

Artık, sanal WAN 'ı kullanarak paketlerin gönderileceği doğru bağlantıyı seçebilirsiniz. Ayrıca, bu paketleri alırken gerçekleştirilecek doğru eylemi seçmek için sanal WAN kullanmanız gerekir. Bunun için bağlantı yolu tablolarını aşağıdaki gibi kullanırsınız:

| Description | Bağlantı | Statik yol            |
| ----------- | ---------- | ----------------------- |
| VNet2Branch | vnet4conn  | 10.2.0.0/16-> 10.4.0.5 |
| Branch2VNet | vnet4conn  | 10.1.0.0/16-> 10.4.0.5 |

Daha fazla bilgi için bkz. [sanal hub yönlendirmesi hakkında](about-virtual-hub-routing.md).

## <a name="architecture"></a>Mimari

Makalenin önceki kısımlarında açıklanan mimarinin bir diyagramı aşağıda verilmiştir.

**Hub 1** adlı bir hub vardır.

* **Hub 1** , NVA sanal ağları **VNET 4** ve **VNET 5**' e doğrudan bağlanır.

* Sanal ağlar 1, 2 ve 3 ve dallar arasındaki trafiğin **VNET 4 NVA** 10.4.0.5 üzerinden gitmesi beklenir.

* Sanal ağlar 1, 2 ve 3 ' ten internet 'e ait tüm trafiğin **VNET 5 NVA** 10.5.0.5 aracılığıyla gitmesi bekleniyor.

:::image type="content" source="./media/routing-scenarios/nva-custom/figure-1.png" alt-text="Ağ mimarisi diyagramı.":::

## <a name="workflow"></a>İş akışı

NVA aracılığıyla yönlendirmeyi ayarlamak için şunları göz önünde bulundurmanız gereken adımlar şunlardır:

1. İnternet 'e bağlanan trafiğin VNet 5 üzerinden gitmesi için sanal ağ eşlemesi ile VNet 5 arasında doğrudan bağlantı kurmak üzere VNET 'ler 1, 2 ve 3 gerekir. Ayrıca 0.0.0.0/0 ve sonraki atlama 10.5.0.5 için sanal ağlarda Kullanıcı tanımlı bir yol ayarlamış olmanız gerekir. Şu anda sanal WAN, 0.0.0.0/0 için sanal hub 'da bir sonraki atlamaya izin vermez.

1. Azure portal sanal hub 'ınıza gidin ve **RT_Shared** adlı özel bir yol tablosu oluşturun. Bu tablo, tüm sanal ağlardan ve şube bağlantılarından yayma yoluyla yolları öğrenir. Bu boş tabloyu aşağıdaki diyagramda görebilirsiniz.

   * **Rotalar:** Herhangi bir statik yol eklemeniz gerekmez.

   * **İlişkilendirme:** VNET 4 ve 5 ' i seçerek bu sanal ağların bağlantılarının yol tablosuyla ilişkili **RT_Shared**.

   * **Yayma:** Tüm dallar ve sanal ağ bağlantılarının rotalarını bu yol tablosuna dinamik olarak yaymasını istiyorsanız, dallar ve tüm sanal ağlar ' ı seçin.

1. Sanal ağlar 1, 2 ve 3 ' ten dallara giden trafiği yönlendirmek için **RT_V2B** adlı özel bir yol tablosu oluşturun.

   * **Rotalar:** Yeni VNet 4 bağlantısı olarak bir sonraki atlamada dallar için toplanmış bir statik rota girişi ekleyin. VNet 4 ' te şube ön ekleri için bir statik yol yapılandırın ve sonraki atlamayı VNet 4 ' te NVA 'nın belirli IP 'si olacak şekilde belirtin.

   * **İlişkilendirme:** Tüm VNET 'leri 1, 2 ve 3 ' ü seçin. Bu, 1, 2 ve 3 VNet bağlantılarının bu yol tablosuyla ilişkilendirileceğini ve bu yol tablosundaki yolları (yayma yoluyla statik ve dinamik) öğrenmenizi sağlar.

   * **Yayma:** Bağlantılar yolları rota tablolarına yayar. Vnetme 1, 2 ve 3 ' ün seçilmesi, sanal ağlar 1, 2 ve 3 ' ten bu yol tablosuna giden yolların yayılmasını sağlar. Şube bağlantılarından **RT_V2B**, şube sanal ağ trafiği VNET 4 ' te NVA üzerinden gittiği için, yolları dal bağlantılarından yaymaya gerek yoktur.
  
1. Varsayılan yol tablosunu düzenleyin, **Defaultroutetable**.

   Tüm VPN, Azure ExpressRoute ve kullanıcı VPN bağlantıları, varsayılan yol tablosuyla ilişkilendirilir. Tüm VPN, ExpressRoute ve kullanıcı VPN bağlantıları, rotaları aynı yol tabloları kümesine yayar.

   * **Rotalar:** VNet 4 bağlantısı olarak sonraki atlamada, VI 1, 2 ve 3 için toplanmış bir statik yol girişi ekleyin. VNET 1, 2 ve 3 toplanmış ön ekler için VNet 4 ' te bir statik yol yapılandırın ve sonraki atlamanın VNet 4 ' te NVA 'nın belirli IP 'si olduğunu belirtin.

   * **İlişkilendirme:** Dallar (VPN/ER/P2S) seçeneğinin seçildiğinden emin olun, şirket içi dal bağlantılarının varsayılan yol tablosuyla ilişkilendirilmesini sağlar.

   * **Yayma:** Dallar (VPN/ER/P2S) seçeneğinin seçildiğinden emin olun ve şirket içi bağlantıların yolları varsayılan yol tablosuna yaydığından emin olun.

:::image type="content" source="./media/routing-scenarios/nva-custom/figure-2.png" alt-text="İş akışının diyagramı." lightbox="./media/routing-scenarios/nva-custom/figure-2.png":::

## <a name="next-steps"></a>Sonraki adımlar

* Sanal WAN hakkında daha fazla bilgi için bkz. [SSS](virtual-wan-faq.md).
* Sanal hub yönlendirmesi hakkında daha fazla bilgi için bkz. [sanal hub yönlendirmesi hakkında](about-virtual-hub-routing.md).
