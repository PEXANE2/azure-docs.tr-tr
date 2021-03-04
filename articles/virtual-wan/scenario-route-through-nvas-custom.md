---
title: Özel ayarları kullanarak trafiği NVA 'lar aracılığıyla yönlendirme
titleSuffix: Azure Virtual WAN
description: Bu senaryo, internet 'e bağlanacak trafik için farklı bir NVA kullanarak trafiği NVA 'lar aracılığıyla yönlendirmenize yardımcı olur.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 02/25/2021
ms.author: cherylmc
ms.custom: fasttrack-edit
ms.openlocfilehash: 6291964aff7c215df7f738ab103947dcb9b1b1d4
ms.sourcegitcommit: f3ec73fb5f8de72fe483995bd4bbad9b74a9cc9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/04/2021
ms.locfileid: "102036898"
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
|---|:---:|:---:|:---:|:---:|:---:|
| **Bileşenler**| ->| hemen |hemen | Hizmet VNet üzerinden |Çevre VNet üzerinden |
| **Hizmet VNet**| ->| hemen |yok| hemen | |
| **Dallar** | ->| Hizmet VNet üzerinden |hemen| hemen |  |

Bağlantı matrisindeki her bir hücre, bağlantının doğrudan sanal WAN veya bir NVA ile sanal ağlardan biri üzerinden akışı yapılıp yapılmayacağını açıklar.

Aşağıdaki bilgileri unutmayın:

* Bileşen
  * Bağlı bileşen doğrudan sanal WAN hub 'ları üzerinde diğer bağlı ışınsal Lara ulaşacaktır.
  * Bağlı dallar, hizmet VNet 'e işaret eden statik bir yol aracılığıyla dallara bağlantı sağlar. Bu önekler daha belirgin olduğundan ve Özeti geçersiz kıldığından, dallardan belirli önekleri öğrenmezsiniz.
  * Bağlı bileşen, doğrudan VNet eşlemesi aracılığıyla çevre VNet 'e internet trafiği gönderecek.
* Dallar, hizmet VNet 'e işaret eden statik bir yönlendirme aracılığıyla bağlı olarak yer alır. Özetlenen statik yolu geçersiz kılan sanal ağlardan belirli önekleri öğrenirler.
* Hizmet VNet, her sanal ağ ve her dalda erişilebilir olması gereken paylaşılan bir hizmet VNet 'ine benzer olacaktır.
* Yalnızca destekleyeceği trafik doğrudan sanal ağ eşayarları üzerinden geldiğinden, çevre VNet 'in sanal WAN üzerinden bağlantısı olması gerekmez. Ancak yapılandırmayı basitleştirmek için çevre VNet için ile aynı bağlantı modelini kullanın.

Üç yol tablosuna çeviren üç farklı bağlantı deseni vardır. Farklı sanal ağlarla ilişkiler şunlardır:

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

| Açıklama | Yol tablosu | Statik yol              |
| ----------- | ----------- | ------------------------- |
| Dallar    | RT_V2B      | 10.2.0.0/16-> vnet4conn  |
| NVA tekerlek  | Varsayılan     | 10.1.0.0/16-> vnet4conn  |

Artık, sanal WAN 'ı kullanarak paketlerin gönderileceği doğru bağlantıyı seçebilirsiniz. Ayrıca, bu paketleri alırken gerçekleştirilecek doğru eylemi seçmek için sanal WAN kullanmanız gerekir. Bunun için bağlantı yolu tablolarını aşağıdaki gibi kullanırsınız:

| Açıklama | Bağlantı | Statik yol            |
| ----------- | ---------- | ----------------------- |
| VNet2Branch | vnet4conn  | 10.2.0.0/16-> 10.4.0.5 |
| Branch2VNet | vnet4conn  | 10.1.0.0/16-> 10.4.0.5 |

Daha fazla bilgi için bkz. [sanal hub yönlendirmesi hakkında](about-virtual-hub-routing.md).

## <a name="architecture"></a>Mimari

Aşağıdaki diyagramda, makalesinde daha önce açıklanan mimari gösterilmektedir.

Diyagramda bir hub vardır; **Merkez 1**.

* **Hub 1** , NVA sanal ağları **VNET 4** ve **VNET 5**' e doğrudan bağlanır.

* Sanal ağlar 1, 2, 3 ve dallar arasındaki trafiğin **VNET 4 NVA** 10.4.0.5 üzerinden gitmesi beklenir.

* Sanal ağlar 1, 2 ve 3 ' ten internet 'e ait tüm trafiğin **VNET 5 NVA** 10.5.0.5 aracılığıyla gitmesi bekleniyor.

:::image type="content" source="./media/routing-scenarios/nva-custom/architecture.png" alt-text="Ağ mimarisi diyagramı.":::

## <a name="workflow"></a><a name="workflow"></a>İş akışı

:::image type="content" source="./media/routing-scenarios/nva-custom/workflow.png" alt-text="İş akışının diyagramı." lightbox="./media/routing-scenarios/nva-custom/workflow.png":::

NVA aracılığıyla yönlendirmeyi ayarlamak için aşağıdaki adımları göz önünde bulundurun:

1. İnternet 'e bağlanan trafiğin VNet 5 üzerinden gitmesi için sanal ağ eşlemesi ile VNet 5 arasında doğrudan bağlantı kurmak üzere VNET 'ler 1, 2 ve 3 gerekir. Ayrıca 0.0.0.0/0 ve sonraki atlama 10.5.0.5 için sanal ağlarda Kullanıcı tanımlı bir yol ayarlamış olmanız gerekir.

   VNet 'leri 1, 2 ve 3 ' ten VNet 5 ' e bağlamak istemiyorsanız ve bunun yerine yalnızca VNet 4 ' te NVA 'yı kullanarak dallardan 0.0.0.0/0 trafiğini yönlendirin (Şirket içi VPN veya ExpressRoute bağlantıları), [Alternatif iş akışına](#alternate)gidin.

   Ancak, VNet-VNet trafiğinin NVA aracılığıyla geçiş için olmasını istiyorsanız sanal hub 'dan VNET 1, 2, 3 ' ün bağlantısını kesmeniz ve bu bağlantıyı bağlayıp NVA bağlı Ile vnet4 arasında üzerinde yığmak gerekir. Sanal WAN 'da VNet-VNet trafiği, sanal WAN hub 'ı veya bir sanal WAN hub 'ının Azure Güvenlik Duvarı (güvenli hub) aracılığıyla geçiş yapabilir. VNet eşlemesi doğrudan VNet eşlemesini kullanıyorsa, sanal hub aracılığıyla doğrudan geçişi atlayarak iletişim kurabilir.

1. Azure portal sanal hub 'ınıza gidin ve **RT_Shared** adlı özel bir yol tablosu oluşturun. Bu tablo, tüm sanal ağlardan ve şube bağlantılarından yayma yoluyla yolları öğrenir. Bu boş tabloyu aşağıdaki diyagramda görebilirsiniz.

   * **Rotalar:** Herhangi bir statik yol eklemeniz gerekmez.

   * **İlişkilendirme:** VNET 4 ve 5 ' i seçin, bu da bu sanal ağların bağlantılarının yol tablosuyla ilişkili olduğu anlamına gelir **RT_Shared**.

   * **Yayma:** Tüm dallar ve sanal ağ bağlantılarının rotalarını bu yol tablosuna dinamik olarak yaymasını istiyorsanız, dallar ve tüm sanal ağlar ' ı seçin.

1. Sanal ağlar 1, 2 ve 3 ' ten dallara giden trafiği yönlendirmek için **RT_V2B** adlı özel bir yol tablosu oluşturun.

   * **Rotalar:** Yeni VNet 4 bağlantısı olarak bir sonraki atlamada dallar için toplanmış bir statik rota girişi ekleyin. VNet 4 ' te dal önekleri için bir statik yol yapılandırın. VNet 4 ' teki NVA 'nın belirli IP 'si olarak bir sonraki atlamayı belirtin.

   * **İlişkilendirme:** Tüm **VNET 'leri 1, 2 ve 3 ' ü** seçin. Bu, 1, 2 ve 3 VNet bağlantılarının bu yol tablosuyla ilişkilendirileceğini ve bu yol tablosundaki yolları (yayma yoluyla statik ve dinamik) öğrenmenizi sağlar.

   * **Yayma:** Bağlantılar yolları rota tablolarına yayar. Vnetme 1, 2 ve 3 ' ün seçilmesi, sanal ağlar 1, 2 ve 3 ' ten bu yol tablosuna yol yaymalarını sağlar. Şube bağlantılarından **RT_V2B**, şube sanal ağ trafiği VNET 4 ' te NVA üzerinden gittiği için, yolları dal bağlantılarından yaymaya gerek yoktur.
  
1. Varsayılan yol tablosunu düzenleyin, **Defaultroutetable**.

   Tüm VPN, Azure ExpressRoute ve kullanıcı VPN bağlantıları, varsayılan yol tablosuyla ilişkilendirilir. Tüm VPN, ExpressRoute ve kullanıcı VPN bağlantıları, rotaları aynı yol tabloları kümesine yayar.

   * **Rotalar:** VNet 4 bağlantısı olarak sonraki atlamada, VI 1, 2 ve 3 için toplanmış bir statik yol girişi ekleyin. VNET 1, 2 ve 3 toplanmış ön ek için VNet 4 ' te bir statik yol yapılandırın. VNet 4 ' teki NVA 'nın belirli IP 'si olarak bir sonraki atlamayı belirtin.

   * **İlişkilendirme:** Dallar (VPN/ER/P2S) seçeneğinin seçildiğinden emin olun, şirket içi dal bağlantılarının varsayılan yol tablosuyla ilişkilendirilmesini sağlar.

   * **Yayma:** Dallar (VPN/ER/P2S) seçeneğinin seçildiğinden emin olun ve şirket içi bağlantıların yolları varsayılan yol tablosuna yaydığından emin olun.

## <a name="alternate-workflow"></a><a name="alternate"></a>Alternatif iş akışı

Bu iş akışında, sanal ağlar 1, 2 ve 3 VNet 5 ' e bağlanmazsınız. Bunun yerine, sanal ağ 4 ' teki NVA 'yı dallardan (Şirket içi VPN veya ExpressRoute bağlantıları) 0.0.0.0/0 trafiğini yönlendirmek için kullanırsınız.

:::image type="content" source="./media/routing-scenarios/nva-custom/alternate.png" alt-text="Alternatif iş akışının diyagramı." lightbox="./media/routing-scenarios/nva-custom/alternate.png":::

NVA aracılığıyla yönlendirmeyi ayarlamak için aşağıdaki adımları göz önünde bulundurun:

1. Azure portal, sanal hub 'ınıza gidin ve NVA 10.4.0.5 üzerinden trafiği yönlendirmek için **RT_NVA** adlı özel bir yol tablosu oluşturun  

   * **Rotalar:**   Eylem gerekmiyor.

   * **İlişkilendirme:**   **İle vnet4 arasında** öğesini seçin. Bu, VNet bağlantısı 4 ' ün bu yol tablosuyla ilişkilendireceği ve bu yol tablosundaki yolları (yayma yoluyla statik ve dinamik) öğreneceği anlamına gelir.

   * **Yayma:**   Bağlantılar yolları rota tablolarına yayar. Vnetme 1, 2 ve 3 ' ün seçilmesi, sanal ağlar 1, 2 ve 3 ' ten bu yol tablosuna yol yaymalarını sağlar. Dalları (VPN/ER/P2S) seçmek, yolların veya şirket içi bağlantıların bu yol tablosuna yayılmasını sağlar. Tüm VPN, ExpressRoute ve kullanıcı VPN bağlantıları, rotaları aynı yol tabloları kümesine yayar.

1. Sanal ağlar 1, 2 ve 3 ' ten gelen trafiği Ile vnet4 arasında NVA üzerinden dallara veya internet 'e (0.0.0.0/0) yönlendirmek için **RT_VNET** adlı özel bir yol tablosu oluşturun. VNET-VNet trafiği, VNet 4 ' ün NVA 'dan değil, doğrudan olur. Trafiğin NVA aracılığıyla gitmesini istiyorsanız, VNet 1, 2 ve 3 ' ü sökün ve Ile vnet4 arasında ' ye VNet eşlemesi kullanarak bunları bağlayın.

   * **Yolların**  

     * Sanal ağlar 1, 2 ve 3 ' ten dallara giden trafik için VNet 4 bağlantısı olarak sonraki atlamanın bulunduğu toplu bir ' 10.2.0.0/16 ' yolunu ekleyin. Ile vnet4 arasında bağlantısında, ' 10.2.0.0/16 ' için bir yol yapılandırın ve sonraki atlamayı VNet 4 ' te NVA 'nın belirli IP 'si olacak şekilde belirtin.

     * VNet 4 bağlantısı olarak sonraki atlamada ' 0.0.0.0/0 ' yolunu ekleyin. ' 0.0.0.0/0 ', internet 'e trafik gönderen anlamına eklenir. Sanal ağlar veya dallarla ilgili belirli adres öneklerini göstermez. Ile vnet4 arasında bağlantısında, ' 0.0.0.0/0 ' için bir yol yapılandırın ve sonraki atlamayı VNet 4 ' te NVA 'nın belirli IP 'si olacak şekilde belirtin.

   * **İlişkilendirme:**   Tüm **VNET 'leri 1, 2 ve 3 ' ü** seçin. Bu, 1, 2 ve 3 VNet bağlantılarının bu yol tablosuyla ilişkilendirileceğini ve bu yol tablosundaki yolları (yayma yoluyla statik ve dinamik) öğrenmenizi sağlar.

   * **Yayma:**   Bağlantılar yolları rota tablolarına yayar. Vnetme 1, 2 ve 3 ' ün seçilmesi, sanal ağlar 1, 2 ve 3 ' ten bu yol tablosuna giden yolların yayılmasını sağlar. Dallar (VPN/ER/P2S) seçeneğinin seçili olmadığından emin olun. Bu, şirket içi bağlantıların doğrudan sanal ağları 1, 2 ve 3 ' e alamıyor olmasını sağlar.

1. Varsayılan yol tablosunu düzenleyin, **Defaultroutetable**.

   Tüm VPN, Azure ExpressRoute ve kullanıcı VPN bağlantıları, varsayılan yol tablosuyla ilişkilendirilir. Tüm VPN, ExpressRoute ve kullanıcı VPN bağlantıları, rotaları aynı yol tabloları kümesine yayar.

   * **Yolların**  

     * **VNET 4 bağlantısı** olarak sonraki atlamada **, VI 1, 2 ve 3** için toplu bir ' 10.1.0.0/16 ' yolu ekleyin.

     * **VNET 4 bağlantısı** olarak sonraki atlamada ' 0.0.0.0/0 ' yolunu ekleyin. ' 0.0.0.0/0 ', internet 'e trafik gönderen anlamına eklenir. Sanal ağlar veya dallarla ilgili belirli adres öneklerini göstermez. Ile vnet4 arasında bağlantısının önceki adımında, ' 0.0.0.0/0 ' için bir yol yapılandırmış olabilirsiniz ve sonraki atlama, VNet 4 ' te NVA 'nın belirli IP 'si olacak.

   * **İlişkilendirme:**   Dallar **(VPN/er/P2S)** seçeneğinin seçildiğinden emin olun. Bu, şirket içi dal bağlantılarının varsayılan yol tablosuyla ilişkilendirilmesini sağlar. Tüm VPN, Azure ExpressRoute ve kullanıcı VPN bağlantıları yalnızca varsayılan yol tablosuyla ilişkilendirilir.

   * **Yayma:**   Dallar **(VPN/er/P2S)** seçeneğinin seçildiğinden emin olun. Bu, şirket içi bağlantıların yolları varsayılan yol tablosuna yaymasını sağlar. Tüm VPN, ExpressRoute ve kullanıcı VPN bağlantıları, yolları ' aynı yol tabloları kümesine yayar.

   >[!Note]
   >
   > * 0.0.0.0/0 yolunun etkili olması için portal kullanıcıları bağlantılarda (VPN/ER/P2S/VNet) ' varsayılan rotaya yay ' özelliğini etkinleştirmelidir.
   > * 0.0.0.0/0 yolunun etkili olabilmesi için PS/CLı/REST kullanıcılarının ' enableınternetsecurity ' bayrağını True olarak ayarlaması gerekir.
   > * Sanal ağ bağlantısı ' çoklu/benzersiz ' bir sonraki atlama IP 'si, bir sonraki atlama IP 'si olan yollardan biri genel IP adresi veya 0.0.0.0/0 (internet) olarak belirtilemez.

## <a name="next-steps"></a>Sonraki adımlar

* Sanal WAN hakkında daha fazla bilgi için bkz. [SSS](virtual-wan-faq.md).
* Sanal hub yönlendirmesi hakkında daha fazla bilgi için bkz. [sanal hub yönlendirmesi hakkında](about-virtual-hub-routing.md).
