---
title: Azure VMware çözümüne bir IPSec tüneli oluşturma
description: Azure VMware çözümlerinde IPSec tüneli oluşturmak için bir sanal WAN hub 'ı oluşturmayı öğrenin.
ms.topic: how-to
ms.date: 10/02/2020
ms.openlocfilehash: 21df674862b65ef6573a8a3fcfd7538b1053f04e
ms.sourcegitcommit: 4bda786435578ec7d6d94c72ca8642ce47ac628a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2021
ms.locfileid: "103491869"
---
# <a name="create-an-ipsec-tunnel-into-azure-vmware-solution"></a>Azure VMware çözümüne bir IPSec tüneli oluşturma

Bu makalede, Microsoft Azure sanal WAN hub 'ında bir VPN (IPSec IKEv1 ve Ikev2) siteden siteye tünel oluşturma adımlarını inceleyeceğiz. Bir Azure sanal WAN hub 'ı ve kendisine bağlı bir genel IP adresi olan bir VPN ağ geçidi oluşturacağız. Daha sonra bir Azure ExpressRoute ağ geçidi oluşturacak ve bir Azure VMware Çözüm uç noktası oluşturacağız. Ayrıca, ilke tabanlı bir VPN şirket içi kurulumunu etkinleştirme ayrıntılarına de gideceğiz. 

## <a name="topology"></a>Topoloji

![VPN siteden siteye tünel mimarisini gösteren diyagram.](media/create-ipsec-tunnel/vpn-s2s-tunnel-architecture.png)

Azure sanal hub, Azure VMware çözümü ExpressRoute ağ geçidini ve siteden siteye VPN ağ geçidini içerir. Azure VMware Çözüm uç noktası ile şirket içi VPN cihazını bağlar.

## <a name="before-you-begin"></a>Başlamadan önce

Siteden siteye VPN tüneli oluşturmak için, şirket içi VPN cihazında genel kullanıma yönelik bir IP adresi Sonlandırıcı oluşturmanız gerekir.

## <a name="create-a-virtual-wan-hub"></a>Sanal WAN hub 'ı oluşturma

1. Azure portal, **sanal WAN 'lar** üzerinde arama yapın. **+Ekle**’yi seçin. WAN Oluştur sayfası açılır.  

2. **WAN oluştur** sayfasında gerekli alanları girin ve ardından **gözden geçir + oluştur**' u seçin.
   
   | Alan | Değer |
   | --- | --- |
   | **Abonelik** | Değer, kaynak grubuna ait abonelikle önceden doldurulur. |
   | **Kaynak grubu** | Sanal WAN, genel bir kaynaktır ve belirli bir bölgeye karşı sınırlandırmamıştı.  |
   | **Kaynak grubu konumu** | Sanal WAN hub 'ını oluşturmak için kaynak grubu için bir konum ayarlamanız gerekir.  |
   | **Ad** |   |
   | **Tür** | Yalnızca VPN Gateway trafiğinin fazlasına izin veren **Standart**' ı seçin.  |

   :::image type="content" source="media/create-ipsec-tunnel/create-wan.png" alt-text="Azure portal WAN sayfası oluştur sayfasını gösteren ekran görüntüsü.":::

3. Azure portal, önceki adımda oluşturduğunuz sanal WAN ' ı seçin, **sanal hub oluştur**' u seçin, gerekli alanları girin ve ardından **İleri: siteden siteye**' yı seçin. 

   | Alan | Değer |
   | --- | --- |
   | **Bölge** | Yönetim perspektifinden bölge seçilmesi gerekir.  |
   | **Ad** |    |
   | **Hub özel adres alanı** | Alt ağı `/24` (minimum) kullanarak girin.  |

   :::image type="content" source="media/create-ipsec-tunnel/create-virtual-hub.png" alt-text="Sanal hub oluştur sayfasını gösteren ekran görüntüsü.":::

4. **Siteden siteye** sekmesinde, **ağ geçidi ölçek birimleri** açılır listesinden toplam aktarım hızını ayarlayarak siteden siteye ağ geçidini tanımlayın. 

   >[!TIP]
   >Tek ölçekli birim = 500 Mbps. Ölçek birimleri, her biri 500 MB/sn destekleyen artıklık için çiftler halinde bulunur.
  
5. **ExpressRoute** sekmesinde bir ExpressRoute Ağ Geçidi oluşturun. 

   >[!TIP]
   >Ölçek birimi değeri 2 GB/sn 'dir. 

    Her hub 'ın oluşturulması yaklaşık 30 dakika sürer. 

## <a name="create-a-vpn-site"></a>VPN sitesi oluşturma 

1. Azure portal **son kaynaklarda** , önceki bölümde oluşturduğunuz sanal WAN ' ı seçin.

2. Sanal hub 'a **genel bakış** bölümünde, **bağlantı**  >  **VPN (siteden siteye)** öğesini seçin ve ardından **yeni VPN sitesi oluştur**' u seçin.

   :::image type="content" source="media/create-ipsec-tunnel/create-vpn-site-basics.png" alt-text="Sanal hub 'ın VPN (siteden siteye) ile ilgili genel bakış sayfasının ekran görüntüsü ve yeni VPN sitesi oluştur seçilidir.":::  
 
3. **Temel bilgiler** sekmesinde, gerekli alanları girip **İleri: bağlantılar**' ı seçin. 

   | Alan | Değer |
   | --- | --- |
   | **Bölge** | Önceki bölümde belirttiğiniz bölge.  |
   | **Ad** |  |
   | **Cihaz satıcısı** |  |
   | **Sınır Ağ Geçidi Protokolü** | Hem Azure VMware çözümünün hem de şirket içi sunucuların, yollarını tünelde tanıtmayı sağlamak için **etkin** olarak ayarlayın. Devre dışı bırakılırsa, tanıtılmak gereken alt ağların el ile saklanması gerekir. Alt ağlar kaçırıldığında, HCX hizmet kafesi oluşturmayacak. Daha fazla bilgi için bkz.  [Azure VPN Gateway Ile BGP hakkında](../vpn-gateway/vpn-gateway-bgp-overview.md). |
   | **Özel adres alanı**  | Şirket içi CıDR bloğunu girin.  Şirket içindeki tüm trafiği tünelde tünele yönlendirmek için kullanılır.  CıDR bloğu yalnızca BGP 'yi etkinleştirmezseniz gereklidir. |
   | **Bağlan** |   |

4. **Bağlantılar** sekmesinde, gerekli alanları doldurup **gözden geçir + oluştur**' u seçin. Bağlantının ve sağlayıcı adlarının belirtilmesi, hub 'ın bir parçası olarak, sonunda oluşturulabilen ağ geçitleri sayısını ayırt etmenize olanak tanır. BGP ve otonom sistem numarası (ASN), kuruluşunuz içinde benzersiz olmalıdır.
 
## <a name="optional-defining-a-vpn-site-for-policy-based-vpn-site-to-site-tunnels"></a>Seçim İlke tabanlı VPN siteden siteye tünellerini için bir VPN sitesi tanımlama

Bu bölüm yalnızca ilke tabanlı VPN 'Ler için geçerlidir. İlke tabanlı (veya statik, rota tabanlı) VPN kurulumları çoğu durumda Şirket içi VPN cihazı özellikleri tarafından dağıtılır. Şirket içi ve Azure VMware Çözüm ağlarının belirtilmesini gerektirir. Azure sanal WAN hub ile Azure VMware çözümü için *hiçbir* ağ seçemezsiniz. Bunun yerine, ilgili tüm şirket içi ve Azure VMware çözümü sanal WAN hub aralıklarını belirtmeniz gerekir. Bu hub aralıkları, ilke tabanlı VPN tüneli şirket içi uç noktasının şifreleme etki alanını belirtmek için kullanılır. Azure VMware Çözüm tarafı yalnızca ilke tabanlı trafik Seçicisi göstergesinin etkinleştirilmesini gerektirir. 

1. Azure portal, sanal WAN hub sitenize gidin. **Bağlantı** altında **VPN (siteden siteye)** seçeneğini belirleyin.

2. VPN sitenizin adını, en sağdaki üç noktayı (...) seçin ve ardından **VPN bağlantısını bu hub 'a düzenleyin**.
 
   :::image type="content" source="media/create-ipsec-tunnel/edit-vpn-section-to-this-hub.png" alt-text="Sanal WAN hub sitesi için Azure 'daki sayfanın ekran görüntüsü, bu hub 'a yönelik VPN bağlantısını Düzenle ' ye erişmek için seçilen bir üç nokta gösterir." lightbox="media/create-ipsec-tunnel/edit-vpn-section-to-this-hub.png":::

3. VPN sitesi ile hub arasındaki bağlantıyı düzenleyin ve ardından **Kaydet**' i seçin.
   - Internet Protokolü güvenliği (IPSec), **özel**' i seçin.
   - İlke tabanlı trafik seçiciyi kullanın, **Etkinleştir** ' i seçin.
   - **Ike Aşama 1** ve **Ike aşama 2 (IPSec)** ayrıntılarını belirtin. 
 
   :::image type="content" source="media/create-ipsec-tunnel/edit-vpn-connection.png" alt-text="VPN bağlantısını Düzenle sayfasının ekran görüntüsü."::: 
 
   İlke tabanlı şifreleme etki alanının parçası olan trafik seçicileri veya alt ağlarınız şunlardır:
    
   - Sanal WAN hub/24
   - Azure VMware çözümü özel bulutu/22
   - Bağlı Azure sanal ağı (varsa)

## <a name="connect-your-vpn-site-to-the-hub"></a>VPN sitenizi hub 'a bağlama

1. VPN sitenizin adını seçin ve ardından **VPN sitelerini bağla**' yı seçin. 
1. **Önceden paylaşılan anahtar** alanına, daha önce şirket içi uç nokta için tanımlanan anahtarı girin. 

   >[!TIP]
   >Daha önce tanımlı bir anahtarınız yoksa, bu alanı boş bırakabilirsiniz. Sizin için otomatik olarak bir anahtar oluşturulur. 
 
   >[!IMPORTANT]
   >Yalnızca hub 'da bir güvenlik duvarı dağıtıyorsanız **Varsayılan rotayı yay** özelliğini etkinleştirin ve bu tünelden gelen bağlantılara yönelik bir sonraki atlama olur.

1. **Bağlan**’ı seçin. Bir bağlantı durumu ekranında, tünel oluşturma durumu görüntülenir.

2. Sanal WAN 'a genel bakış ' a gidin ve VPN site sayfasını açarak şirket içi uç noktaya ait VPN yapılandırma dosyasını indirin.  

3. Sanal WAN hub 'ında Azure VMware çözümü ExpressRoute 'a yama yapın. Bu adım önce özel bulutunuzu oluşturmayı gerektirir.

   [!INCLUDE [request-authorization-key](includes/request-authorization-key.md)]

4. Azure VMware çözümünü ve VPN ağ geçidini sanal WAN hub 'ında birbirine bağlayın. 
   1. Azure portal, daha önce oluşturduğunuz sanal WAN 'ı açın. 
   1. Oluşturulan sanal WAN hub 'ını seçin ve ardından sol bölmedeki **ExpressRoute** ' ı seçin. 
   1. **+ Kullan yetkilendirme anahtarı**' nı seçin.

      :::image type="content" source="media/create-ipsec-tunnel/redeem-authorization-key.png" alt-text="Kullan yetkilendirme anahtarı seçiliyken özel bulutun ExpressRoute sayfasının ekran görüntüsü.":::

   1. Yetkilendirme anahtarını yetkilendirme anahtarı alanına yapıştırın.
   1. ExpressRoute KIMLIĞI, **eş devre URI 'si** alanına geçti. 
   1. **Bu ExpressRoute devresini otomatik olarak hub ile ilişkilendir** ' i seçin. 
   1. Bağlantıyı kurmak için **Ekle** ' yi seçin. 

5. [NSX-T segmenti oluşturup](./tutorial-nsx-t-network-segment.md) ağ ÜZERINDE bir VM sağlayarak bağlantınızı test edin. Hem şirket içi hem de Azure VMware Çözüm uç noktalarına ping gönderin.
