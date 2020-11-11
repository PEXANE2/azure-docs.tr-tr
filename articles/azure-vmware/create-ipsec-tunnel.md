---
title: Azure VMware çözümüne bir IPSec tüneli oluşturma
description: Azure VMware çözümlerinde IPSec tüneli oluşturmak için bir sanal WAN hub 'ı oluşturmayı öğrenin.
ms.topic: how-to
ms.date: 10/02/2020
ms.openlocfilehash: 9f869f04bf165f4791f13c626b63257ea98a7ca9
ms.sourcegitcommit: 4bee52a3601b226cfc4e6eac71c1cb3b4b0eafe2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/11/2020
ms.locfileid: "94506456"
---
# <a name="create-an-ipsec-tunnel-into-azure-vmware-solution"></a>Azure VMware çözümüne bir IPSec tüneli oluşturma

Bu makalede, Microsoft Azure sanal WAN hub 'ında bir VPN (IPSec IKEv1 ve Ikev2) siteden siteye tünel oluşturma adımlarını inceleyeceğiz. Bir Azure sanal WAN hub 'ı ve kendisine bağlı bir genel IP adresi olan bir VPN ağ geçidi oluşturacağız. Daha sonra bir Azure ExpressRoute ağ geçidi oluşturacak ve bir Azure VMware Çözüm uç noktası oluşturacağız. Ayrıca, ilke tabanlı bir VPN şirket içi kurulumunu etkinleştirme ayrıntılarına de gideceğiz. 

## <a name="topology"></a>Topoloji

![VPN siteden siteye tünel mimarisini gösteren diyagram.](media/create-ipsec-tunnel/vpn-s2s-tunnel-architecture.png)

Azure sanal hub, Azure VMware çözümü ExpressRoute ağ geçidini ve siteden siteye VPN ağ geçidini içerir. Azure VMware Çözüm uç noktası ile şirket içi VPN cihazını bağlar.

## <a name="before-you-begin"></a>Başlamadan önce

Siteden siteye VPN tüneli oluşturmak için, şirket içi VPN cihazında genel kullanıma yönelik bir IP adresi Sonlandırıcı oluşturmanız gerekir.

## <a name="create-a-virtual-wan-hub"></a>Sanal WAN hub 'ı oluşturma

1. Azure portal, **sanal WAN 'lar** üzerinde arama yapın. **+Ekle** ’yi seçin. WAN Oluştur sayfası açılır.  

2. **WAN oluştur** sayfasında gerekli alanları girin ve ardından **gözden geçir + oluştur** ' u seçin.
   
   | Alan | Değer |
   | --- | --- |
   | **Abonelik** | Değer, kaynak grubuna ait abonelikle önceden doldurulur. |
   | **Kaynak grubu** | Sanal WAN, genel bir kaynaktır ve belirli bir bölgeye karşı sınırlandırmamıştı.  |
   | **Kaynak grubu konumu** | Sanal WAN hub 'ını oluşturmak için kaynak grubu için bir konum ayarlamanız gerekir.  |
   | **Ad** |   |
   | **Tür** | Yalnızca VPN Gateway trafiğinin fazlasına izin veren **Standart** ' ı seçin.  |


    :::image type="content" source="media/create-ipsec-tunnel/create-wan.png" alt-text="Azure portal WAN sayfası oluştur sayfasını gösteren ekran görüntüsü.":::

3. Azure portal, önceki adımda oluşturduğunuz sanal WAN ' ı seçin, **sanal hub oluştur** ' u seçin, gerekli alanları girin ve ardından **İleri: siteden siteye** ' yı seçin. 

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

2. Sanal hub 'a **genel bakış** bölümünde, **bağlantı**  >  **VPN (siteden siteye)** öğesini seçin ve ardından **yeni VPN sitesi oluştur** ' u seçin.


    :::image type="content" source="media/create-ipsec-tunnel/create-vpn-site-basics.png" alt-text="Sanal hub 'ın VPN (siteden siteye) ile ilgili genel bakış sayfasının ekran görüntüsü ve yeni VPN sitesi oluştur seçilidir.":::  
 
3. **Temel bilgiler** sekmesinde, gerekli alanları girip **İleri: bağlantılar** ' ı seçin. 

   | Alan | Değer |
   | --- | --- |
   | **Bölge** | Önceki bölümde belirttiğiniz bölge.  |
   | **Ad** |  |
   | **Cihaz satıcısı** |  |
   | **Sınır Ağ Geçidi Protokolü** | Hem Azure VMware çözümünün hem de şirket içi sunucuların, yollarını tünelde tanıtmayı sağlamak için **etkin** olarak ayarlayın. Devre dışı bırakılırsa, tanıtılmak gereken alt ağların el ile saklanması gerekir. Alt ağlar kaçırıldığında, HCX hizmet kafesi oluşturmayacak. Daha fazla bilgi için bkz.  [Azure VPN Gateway Ile BGP hakkında](../vpn-gateway/vpn-gateway-bgp-overview.md). |
   | **Özel adres alanı**  | Şirket içi CıDR bloğunu girin.  Şirket içindeki tüm trafiği tünelde tünele yönlendirmek için kullanılır.  CıDR bloğu yalnızca BGP 'yi etkinleştirmezseniz gereklidir. |
   | **Bağlan** |   |

4. **Bağlantılar** sekmesinde, gerekli alanları doldurup **gözden geçir + oluştur** ' u seçin. Bağlantının ve sağlayıcı adlarının belirtilmesi, hub 'ın bir parçası olarak, sonunda oluşturulabilen ağ geçitleri sayısını ayırt etmenize olanak tanır. BGP ve otonom sistem numarası (ASN), kuruluşunuz içinde benzersiz olmalıdır.
 
## <a name="optional-defining-a-vpn-site-for-policy-based-vpn-site-to-site-tunnels"></a>Seçim İlke tabanlı VPN siteden siteye tünellerini için bir VPN sitesi tanımlama

Bu bölüm yalnızca ilke tabanlı VPN 'Ler için geçerlidir. İlke tabanlı (veya statik, rota tabanlı) VPN kurulumları çoğu durumda Şirket içi VPN cihazı özellikleri tarafından dağıtılır. Şirket içi ve Azure VMware Çözüm ağlarının belirtilmesini gerektirir. Azure sanal WAN hub ile Azure VMware çözümü için *hiçbir* ağ seçemezsiniz. Bunun yerine, ilgili tüm şirket içi ve Azure VMware çözümü sanal WAN hub aralıklarını belirtmeniz gerekir. Bu hub aralıkları, ilke tabanlı VPN tüneli şirket içi uç noktasının şifreleme etki alanını belirtmek için kullanılır. Azure VMware Çözüm tarafı yalnızca ilke tabanlı trafik Seçicisi göstergesinin etkinleştirilmesini gerektirir. 

1. Azure portal, sanal WAN hub sitenize gidin; **bağlantı** altında **VPN (siteden siteye)** seçeneğini belirleyin.

2. VPN sitenizin adını ve ardından en sağdaki üç nokta (...) simgesini seçin; sonra **Bu hub 'A VPN bağlantısını Düzenle** ' yi seçin.
 
    :::image type="content" source="media/create-ipsec-tunnel/edit-vpn-section-to-this-hub.png" alt-text="Sanal WAN hub sitesi için Azure 'daki sayfanın ekran görüntüsü, bu hub 'a yönelik VPN bağlantısını Düzenle ' ye erişmek için seçilen bir üç nokta gösterir." lightbox="media/create-ipsec-tunnel/edit-vpn-section-to-this-hub.png":::

3. VPN sitesi ile hub arasındaki bağlantıyı düzenleyin ve ardından **Kaydet** ' i seçin.
   - Internet Protokolü güvenliği (IPSec), **özel** ' i seçin.
   - İlke tabanlı trafik seçiciyi kullanın, **Etkinleştir** ' i seçin.
   - **Ike Aşama 1** ve **Ike aşama 2 (IPSec)** ayrıntılarını belirtin. 
 
    :::image type="content" source="media/create-ipsec-tunnel/edit-vpn-connection.png" alt-text="VPN bağlantısını Düzenle sayfasının ekran görüntüsü."::: 
 
    İlke tabanlı şifreleme etki alanının parçası olan trafik seçicileri veya alt ağlarınız şunlardır:
    
    - Sanal WAN hub/24
    - Azure VMware çözümü özel bulutu/22
    - Bağlı Azure sanal ağı (varsa)

## <a name="connect-your-vpn-site-to-the-hub"></a>VPN sitenizi hub 'a bağlama

1. VPN sitesi adınızın yanındaki kutuyu işaretleyin (bkz. Yukarıdaki **VPN sitesinden site** ekran görüntüsü) ve ardından **VPN sitelerini bağla** ' yı seçin. **Önceden paylaşılan anahtar** alanına, daha önce şirket içi uç nokta için tanımlanan anahtarı girin. Daha önce tanımlı bir anahtarınız yoksa, bu alanı boş bırakabilirsiniz ve sizin için otomatik olarak bir anahtar oluşturulur. 
 
    Yalnızca hub 'da bir güvenlik duvarı dağıtıyorsanız **Varsayılan rotayı yay** özelliğini etkinleştirin ve bu tünelden gelen bağlantılara yönelik bir sonraki atlama olur.

    **Bağlan** ’ı seçin. Bir bağlantı durumu ekranında, tünel oluşturma durumu gösterilir.

2. Sanal WAN 'a genel bakış konusuna gidin. VPN sitesi sayfasını açın ve VPN yapılandırma dosyasını şirket içi uç noktaya uygulamak üzere indirin.  

3. Şimdi Azure VMware çözümü ExpressRoute sanal WAN hub 'ına yacağız. (Bu adım önce özel bulutunuzu oluşturmayı gerektirir.)

    Azure VMware Çözüm özel bulutu 'nın **bağlantı** bölümüne gidin. **ExpressRoute** sekmesinde **+ bir yetkilendirme anahtarı iste** ' yi seçin. Adlandırın ve **Oluştur** ' u seçin. (Anahtarı oluşturmak yaklaşık 30 saniye sürebilir.) ExpressRoute KIMLIĞI ve yetkilendirme anahtarı ' nı kopyalayın. 

    :::image type="content" source="media/create-ipsec-tunnel/express-route-connectivity.png" alt-text="ExpressRoute sekmesi altında seçilmiş bir yetkilendirme anahtarı Isteğiyle özel bulutun bağlantı sayfasının ekran görüntüsü.":::

    > [!NOTE]
    > Yetkilendirme anahtarı bir süre sonra kaybolur, bu nedenle göründüğü anda kopyalayın.

4. Daha sonra, Azure VMware çözümünü ve VPN ağ geçidini sanal WAN hub 'ında birbirine bağlayacağız. Azure portal, daha önce oluşturduğunuz sanal WAN 'ı açın. Oluşturulan sanal WAN hub 'ını seçin ve ardından sol bölmedeki **ExpressRoute** ' ı seçin. **+ Kullan yetkilendirme anahtarı** ' nı seçin.

    :::image type="content" source="media/create-ipsec-tunnel/redeem-authorization-key.png" alt-text="Kullan yetkilendirme anahtarı seçiliyken özel bulutun ExpressRoute sayfasının ekran görüntüsü.":::

    Yetkilendirme anahtarını yetkilendirme anahtarı alanına ve ExpressRoute KIMLIĞINE **eş devre URI 'si** alanına yapıştırın. **Bu ExpressRoute devresini otomatik olarak hub ile ilişkilendir** ' i seçtiğinizden emin olun. Bağlantıyı kurmak için **Ekle** ' yi seçin. 

5. Bağlantınızı test etmek için [BIR NSX-T segmenti oluşturun](./tutorial-nsx-t-network-segment.md) ve ağda bir sanal makine sağlayın. Hem şirket içi hem de Azure VMware Çözüm uç noktalarına ping yaparak test edin.
