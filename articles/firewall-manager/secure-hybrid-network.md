---
title: 'Öğretici: Azure Güvenlik Duvarı Yöneticisi önizlemesini kullanarak hub sanal ağınızı güvenli hale'
description: Bu eğitimde, Azure portalını kullanarak Azure Güvenlik Duvarı Yöneticisi ile sanal ağınızı nasıl güvene diğinizi öğrenirsiniz.
services: firewall-manager
author: vhorne
ms.service: firewall-manager
ms.topic: tutorial
ms.date: 02/18/2020
ms.author: victorh
ms.openlocfilehash: cdd416bdb833e4784334a6847d724a7375e2ef8d
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "77459962"
---
# <a name="tutorial-secure-your-hub-virtual-network-using-azure-firewall-manager-preview"></a>Öğretici: Azure Güvenlik Duvarı Yöneticisi önizlemesini kullanarak hub sanal ağınızı güvenli hale 

[!INCLUDE [Preview](../../includes/firewall-manager-preview-notice.md)]

Karma bir ağ oluşturmak için şirket içi ağınızı bir Azure sanal ağına bağladığınızda, Azure ağ kaynaklarına erişimi denetleme olanağı genel bir güvenlik planının önemli bir parçasıdır.

Azure Güvenlik Duvarı Yöneticisi Önizleme'yi kullanarak, özel IP adreslerine, Azure PaaS'a ve Internet'e yönelik karma ağ trafiğinizi güvence altına almak için bir hub sanal ağı oluşturabilirsiniz. İzin verilen ve reddedilen ağ trafiğini tanımlayan ilkeleri kullanarak karma ağdaki ağ erişimini denetlemek için Azure Güvenlik Duvarı Yöneticisi'ni kullanabilirsiniz.

Güvenlik Duvarı Yöneticisi, güvenli bir sanal hub mimarisini de destekler. Güvenli sanal hub ve hub sanal ağ mimarisi türlerinin karşılaştırması için Azure [Güvenlik Duvarı Yöneticisi mimari seçenekleri nelerdir'](vhubs-and-vnets.md) e bakın?

Bu öğretici için üç sanal ağ oluşturursunuz:

- **VNet-Hub** - güvenlik duvarı bu sanal ağda.
- **VNet-Spoke** - konuşan sanal ağ Azure'da bulunan iş yükünü temsil eder.
- **VNet-Onprem** - Şirket içi sanal ağ, şirket içi bir ağı temsil eder. Gerçek bir dağıtımda, VPN veya ExpressRoute bağlantısı yla bağlanabilir. Basitlik için bu öğretici bir VPN ağ geçidi bağlantısı kullanır ve Azure'da bulunan bir sanal ağ şirket içi ağı temsil etmek için kullanılır.

![Hibrit ağ](media/tutorial-hybrid-portal/hybrid-network-firewall.png)

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Güvenlik duvarı ilkesi oluşturma
> * Sanal ağları oluşturma
> * Güvenlik duvarını yapılandırma ve dağıtma
> * VPN ağ geçitlerini oluşturma ve bağlama
> * Hub'ı eşleyin ve konuşan sanal ağlar
> * Yolları oluşturma
> * Sanal makineleri oluşturma
> * Güvenlik duvarını test etme


## <a name="prerequisites"></a>Ön koşullar

Karma ağ, Azure VNet'ler ile şirket içi ağlar arasındaki trafiği yönlendirmek için hub ve kollu mimari modelini kullanır. Hub ve spoke mimarisi aşağıdaki gereksinimlere sahiptir:

- VNet-Hub'ı VNet-Spoke'a bakarken **İzin Verme Ağılar** Geçidi'ni ayarlayın. Hub ve kollu ağ mimarisinde ağ geçidi geçişi, konuşan sanal ağların her konuşan sanal ağda VPN ağ geçitlerini dağıtmak yerine hub'daki VPN ağ geçidini paylaşmasına olanak tanır. 

   Ayrıca, ağ geçidine bağlı sanal ağlara veya şirket içi ağlara giden yollar, ağ geçidi geçişini kullanarak, eşlenen sanal ağlar için yönlendirme tablolarına otomatik olarak yayılır. Daha fazla bilgi için sanal [ağ eşleme için VPN ağ geçidi geçişini yapılandırın'](../vpn-gateway/vpn-gateway-peering-gateway-transit.md)a bakın.

- VNet-Spoke ile VNet-Hub'a baktığınızda **UseRemoteGateways'i** ayarlayın. **UseRemoteGateways** ayarlanır ve uzaktan bakanlıkta **AllowGatewayTransit** de ayarlanırsa, konuşan sanal ağ geçiş için uzak sanal ağın ağ geçitlerini kullanır.
- Sözcü alt ağ trafiğini hub güvenlik duvarından yönlendirmek için, Sanal ağ ağ geçidi rotası **yayılma** ayarı devre dışı bırakılmış olan güvenlik duvarını işaret eden bir Kullanıcı Tanımlı rota (UDR) gerekir. Bu seçenek, konuşan alt ağlara rota dağıtımını engeller. Bu, öğrenilen yolların UDR'nizle çakışmasını önler.
- Hub ağ geçidi alt netinde, işaretli ağlara bir sonraki atlama olarak güvenlik duvarı IP adresini işaret eden bir UDR yapılandırın. BGP'den rotalar öğrendiği için Azure Güvenlik Duvarı alt netinde UDR gerekmez.

Bu yolların nasıl oluşturulduğunu görmek için [Yolları Oluşturma](#create-the-routes) bölümüne bakın.

>[!NOTE]
>Azure Güvenlik Duvarı'nın doğrudan Internet bağlantısı olması gerekir. AzureFirewallSubnet'iniz BGP üzerinden şirket içi ağınıza varsayılan bir rota öğrenirse, doğrudan Internet bağlantısını korumak için **Internet** olarak ayarlanmış **NextHopType** değeriyle 0.0.0.0/0 UDR ile bunu geçersiz kılmanız gerekir.
>
>Azure Güvenlik Duvarı, zorunlu tünel kazmayı destekleyecek şekilde yapılandırılabilir. Daha fazla bilgi için Azure [Güvenlik Duvarı zorunlu tünel leme](../firewall/forced-tunneling.md)bölümüne bakın.

>[!NOTE]
>Bir UDR varsayılan ağ geçidi olarak Azure Güvenlik Duvarı'nı işaret etse bile, doğrudan bakan VNet'ler arasındaki trafik doğrudan yönlendirilir. Bu senaryoda alt ağ trafiğine alt ağ göndermek için, udr'nin her iki alt ağda da açıkça hedef alt ağ önekini içermesi gerekir.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="create-a-firewall-policy"></a>Güvenlik Duvarı İlkesi Oluşturma

1. Azure portalında oturum [https://portal.azure.com](https://portal.azure.com)aç.
2. Azure portalı arama çubuğunda **Güvenlik Duvarı Yöneticisi** yazın ve **Enter**tuşuna basın.
3. Azure Güvenlik Duvarı Yöneticisi sayfasında **Azure güvenlik duvarı ilkelerini görüntüleyin'i**seçin.

   ![Güvenlik duvarı ilkesi](media/tutorial-hybrid-portal/firewall-manager-policy.png)

1. **Azure Güvenlik Duvarı İlkesi Oluştur'u**seçin.
1. Aboneliğinizi seçin ve Kaynak grubu için **yeni oluştur'u** seçin ve **FW-Hybrid-Test**adlı bir kaynak grubu oluşturun.
2. İlke adı için **Pol-Net01**yazın.
3. Bölge için **Doğu ABD'yi**seçin.
4. **Sonraki:Kuralları**seçin.
5. **Kural koleksiyonu ekle'yi**seçin.
6. **Ad**için , **RCNet01**yazın.
7. **Kural toplama türü için** **Ağ'ı**seçin.
8. **Öncelik**için, **100**yazın.
9. **Eylem** alanında **İzin ver**'i seçin.
10. **Kurallar**altında , **Adı**için , türü **AllowWeb**.
11. **Kaynak Adresleri**için , yazın **192.168.1.0/24**.
12. **Protokol** alanında **TCP**'yi seçin.
13. **Hedef Bağlantı Noktaları**için, **80**yazın.
14. **Hedef Türü**için **IP Adresi'ni**seçin.
15. **Hedef**için , **10.6.0.0/16**yazın.
16. Bir sonraki kural satırında aşağıdaki bilgileri girin:
 
    Adı: **allowrdp** türü<br>
    Kaynak IP adresi: yazın **192.168.1.0/24**.<br>
    Protokol, **TCP'yi** seçin<br>
    Hedef Bağlantı Noktaları, **3389** yazın<br>
    Hedef Türü, **IP Adresi** seçin<br>
    Hedef için **10.6.0.0/16** yazın

1. **Ekle'yi**seçin.
2. **Gözden Geçir + Oluştur'u**seçin.
3. Ayrıntıları gözden geçirin ve ardından **Oluştur'u**seçin.

## <a name="create-the-firewall-hub-virtual-network"></a>Güvenlik duvarı hub'ı sanal ağ oluşturma

> [!NOTE]
> AzureFirewallSubnet alt ağının boyutu /26'dır. Alt ağ boyutu hakkında daha fazla bilgi için [Azure Güvenlik Duvarı SSS'si'ne](../firewall/firewall-faq.md#why-does-azure-firewall-need-a-26-subnet-size)bakın.

1. Azure portalı giriş sayfasından **kaynak oluştur'u**seçin.
2. **Ağ**altında, **Sanal ağ**seçin.
4. **Ad**için , **VNet hub**yazın.
5. **Adres alanı**için, **10.5.0.0/16**yazın.
6. **Abonelik** bölümünde aboneliğinizi seçin.
7. **Kaynak grubu**için **FW-Hybrid-Test'i**seçin.
8. **Konum**için Doğu **ABD'yi**seçin.
9. **Alt ağ** bölümünde **Ad** alanına **AzureFirewallSubnet** yazın. Güvenlik duvarı bu alt ağda yer alacaktır ve alt ağ adının **mutlaka** AzureFirewallSubnet olması gerekir.
10. **Adres aralığı**için, **10.5.0.0/26**yazın. 
11. Diğer varsayılan ayarları kabul edin ve sonra **Oluştur'u**seçin.

## <a name="create-the-spoke-virtual-network"></a>Konuşan sanal ağı oluşturma

1. Azure portalı giriş sayfasından **kaynak oluştur'u**seçin.
2. **Ağ**altında, **Sanal ağ**seçin.
4. **Ad**için , **VNet-Spoke**yazın.
5. **Adres alanı**için, **10.6.0.0/16**yazın.
6. **Abonelik** bölümünde aboneliğinizi seçin.
7. **Kaynak grubu**için **FW-Hybrid-Test'i**seçin.
8. **Konum** alanında önceden kullandığınız konumu seçin.
9. **Alt ağ** altında, **Ad** için **SN-Workload** yazın.
10. **Adres aralığı**için, **10.6.0.0/24**yazın.
11. Diğer varsayılan ayarları kabul edin ve sonra **Oluştur'u**seçin.

## <a name="create-the-on-premises-virtual-network"></a>Şirket içi sanal ağı oluşturma

1. Azure portalı giriş sayfasından **kaynak oluştur'u**seçin.
2. **Ağ**altında, **Sanal ağ**seçin.
4. **Ad**için , **VNet-OnPrem**yazın.
5. **Adres alanı** için **192.168.0.0/16** yazın.
6. **Abonelik** bölümünde aboneliğinizi seçin.
7. **Kaynak grubu**için **FW-Hybrid-Test'i**seçin.
8. **Konum** alanında önceden kullandığınız konumu seçin.
9. **Subnet**altında, **Ad** türü **SN-Corp**için.
10. **Adres aralığı** için **192.168.1.0/24** yazın.
11. Diğer varsayılan ayarları kabul edin ve sonra **Oluştur'u**seçin.

Sanal ağ dağıtıldıktan sonra ağ geçidi için ikinci bir alt ağ oluşturun.

1. **VNet-Onprem** sayfasında Alt **Ağlar'ı**seçin.
2. **+Subnet'i**seçin.
3. **Ad**için , **GatewaySubnet**yazın.
4. **Adres aralığı (CIDR bloğu)** türü **için 192.168.2.0/24**.
5. **Tamam'ı**seçin.

### <a name="create-a-public-ip-address"></a>Genel IP adresi oluşturma

Bu, şirket içi ağ geçidi için kullanılan genel IP adresidir.

1. Azure portalı giriş sayfasından **kaynak oluştur'u**seçin.
2. Arama metin kutusuna **genel IP adresini** yazın ve **Enter**tuşuna basın.
3. **Genel IP adresini** seçin ve ardından **Oluştur'u**seçin.
4. Ad için **VNet-Onprem-GW-pip**yazın.
5. Kaynak grubu için **FW-Hibrit Testi**yazın.
6. **Konum**için Doğu **ABD'yi**seçin.
7. Diğer varsayılanları kabul edin ve sonra **Oluştur'u**seçin.

## <a name="configure-and-deploy-the-firewall"></a>Güvenlik duvarını yapılandırma ve dağıtma

Güvenlik ilkeleri bir hub ile ilişkilendirildiğinde, hub *sanal ağ*olarak adlandırılır.

**VNet-Hub** sanal ağını hub *sanal ağına* dönüştürün ve Azure Güvenlik Duvarı ile güvenli hale getirin.

1. Azure portalı arama çubuğunda **Güvenlik Duvarı Yöneticisi** yazın ve **Enter**tuşuna basın.
3. Azure Güvenlik Duvarı Yöneticisi sayfasında, **sanal ağlara güvenlik ekleme**altında, hub sanal ağları **görüntüle'yi**seçin.
4. **Sanal ağları Dönüştür'ü**seçin.
5. **VNet hub'ı** seçin ve ardından **Sonraki : Azure Güvenlik Duvarı'nı**seçin.
6. Güvenlik **Duvarı İlkesi**için **Pol-Net01'i**seçin.
7. Sonraki ni Seçin **" İnceleme + onayla**
8. Ayrıntıları gözden geçirin ve ardından **Onayla'yı**seçin.


   Dağıtmak birkaç dakika sürer.
7. Dağıtım tamamlandıktan sonra **FW-Hybrid-Test** kaynak grubuna gidin ve güvenlik duvarını seçin.
9. **Genel Bakış** sayfasındaki Güvenlik Duvarı **özel IP** adresine dikkat edin. Varsayılan rotayı oluştururken bu adresi kullanacaksınız.

## <a name="create-and-connect-the-vpn-gateways"></a>VPN ağ geçitlerini oluşturma ve bağlama

Hub ve şirket içi sanal ağlar VPN ağ geçitleri üzerinden bağlanır.

### <a name="create-a-vpn-gateway-for-the-hub-virtual-network"></a>Hub sanal ağ için VPN ağ geçidi oluşturma

Şimdi hub sanal ağ için VPN ağ geçidi oluşturun. Ağdan ağa yapılandırmalar bir RouteBased VpnType gerektirir. VPN ağ geçidinin oluşturulması, seçili VPN ağ geçidi SKU’suna bağlı olarak 45 dakika veya daha uzun sürebilir.

1. Azure portalı giriş sayfasından **kaynak oluştur'u**seçin.
2. Arama metin kutusuna **sanal ağ ağ geçidi** yazın ve **Enter**tuşuna basın.
3. **Sanal ağ ağ geçidini**seçin ve **Oluştur'u**seçin.
4. **Ad**için , **GW hub**yazın.
5. **Bölge**için **(ABD) Doğu ABD'yi**seçin.
6. **Ağ Geçidi türü için** **VPN'i**seçin.
7. **VPN türü için**Rota **tabanlı'yı**seçin.
8. **SKU**için **Temel'i**seçin.
9. **Sanal ağ**için **VNet hub'ı**seçin.
10. **Genel IP adresi**için yeni **oluştur'u**ve ad için **VNet-hub-GW-pip** yazın'ı seçin.
11. Kalan varsayılanları kabul edin ve ardından **Gözden Geçir + oluştur'u**seçin.
12. Yapılandırmayı gözden geçirin, ardından **Oluştur'u**seçin.

### <a name="create-a-vpn-gateway-for-the-on-premises-virtual-network"></a>Şirket içi sanal ağ için VPN ağ geçidi oluşturma

Şimdi şirket içi sanal ağ için VPN ağ geçidi oluşturun. Ağdan ağa yapılandırmalar bir RouteBased VpnType gerektirir. VPN ağ geçidinin oluşturulması, seçili VPN ağ geçidi SKU’suna bağlı olarak 45 dakika veya daha uzun sürebilir.

1. Azure portalı giriş sayfasından **kaynak oluştur'u**seçin.
2. Arama metin kutusuna **sanal ağ ağ geçidi** yazın ve **Enter**tuşuna basın.
3. **Sanal ağ ağ geçidini**seçin ve **Oluştur'u**seçin.
4. **Ad**için , **GW-Onprem**yazın.
5. **Bölge**için **(ABD) Doğu ABD'yi**seçin.
6. **Ağ Geçidi türü için** **VPN'i**seçin.
7. **VPN türü için**Rota **tabanlı'yı**seçin.
8. **SKU**için **Temel'i**seçin.
9. **Sanal ağ**için **VNet-Onprem'i**seçin.
10. **Genel IP adresi**için **Varolan'ı kullanın*ve ad için **VNet-Onprem-GW-pip'i** seçin.
11. Kalan varsayılanları kabul edin ve ardından **Gözden Geçir + oluştur'u**seçin.
12. Yapılandırmayı gözden geçirin, ardından **Oluştur'u**seçin.

### <a name="create-the-vpn-connections"></a>VPN bağlantılarını oluşturma

Artık hub ve şirket içi ağ geçitleri arasında VPN bağlantıları oluşturabilirsiniz.

Bu adımda, hub sanal ağdan şirket içi sanal ağa bağlantı oluşturursunuz. Örneklerde paylaşılan bir anahtar göreceksiniz. Paylaşılan anahtar için kendi değerlerinizi kullanabilirsiniz. Paylaşılan anahtarın her iki bağlantıyla da eşleşiyor olması önemlidir. Bir bağlantı oluşturmak çok zaman almaz.

1. **FW-Hybrid-Test** kaynak grubunu açın ve **GW hub** ağ geçidini seçin.
2. Sol sütundaki **Bağlantılar'ı** seçin.
3. **Ekle'yi**seçin.
4. Bağlantı adı, **Hub-to-Onprem**yazın.
5. **Bağlantı türü**için **VNet'e VNet'i** seçin.
6. İkinci **sanal ağ ağ geçidi**için **GW-Onprem'i**seçin.
7. **Paylaşılan anahtar (PSK)** için **AzureA1b2C3**yazın.
8. **Tamam'ı**seçin.

Şirket içinde sanal ağ bağlantısını hub'a oluşturun. VNet-Onprem'den VNet hub'ına bağlantı oluşturmanız dışında, bu adım öncekine benzer. Paylaşılan anahtarların eşleştiğinden emin olun. Bağlantı birkaç dakika içerisinde kurulacaktır.

1. **FW-Hybrid-Test** kaynak grubunu açın ve **GW-Onprem** ağ geçidini seçin.
2. Sol sütundaki **Bağlantılar'ı** seçin.
3. **Ekle'yi**seçin.
4. Bağlantı adı, **Onprem-to-Hub**yazın.
5. **Bağlantı türü**için **VNet'e VNet'i** seçin.
6. İkinci **sanal ağ ağ geçidi**için **GW hub'ı**seçin.
7. **Paylaşılan anahtar (PSK)** için **AzureA1b2C3**yazın.
8. **Tamam'ı**seçin.


#### <a name="verify-the-connection"></a>Bağlantıyı doğrulama

Yaklaşık beş dakika kadar sonra, her iki bağlantının durumu **Bağlı**olmalıdır.

![Ağ geçidi bağlantıları](media/secure-hybrid-network/gateway-connections.png)

## <a name="peer-the-hub-and-spoke-virtual-networks"></a>Hub'ı eşleyin ve konuşan sanal ağlar

Şimdi hub eş ve sanal ağlar konuştu.

1. **FW-Hybrid-Test** kaynak grubunu açın ve **VNet hub** sanal ağını seçin.
2. Sol sütunda, **Eşler'i**seçin.
3. **Ekle'yi**seçin.
4. **Ad**için , **hubtoSpoke**yazın.
5. Sanal **ağ**için **VNet-spoke'yi** seçin
6. VNetSpoke'den VNet hub'ına kadar olan eşlemenin adı için **SpoketoHub**yazın.
7. **Ağ geçidi geçişine izin ver'i**seçin.
8. **Tamam'ı**seçin.

### <a name="configure-additional-settings-for-the-spoketohub-peering"></a>SpoketoHub eşleme için ek ayarları yapılandırın

SpoketoHub'da **iletilen trafiğe izin** verme olanağı sağlamanız gerekir.

1. **FW-Hybrid-Test** kaynak grubunu açın ve **VNet-Spoke** sanal ağını seçin.
2. Sol sütunda, **Eşler'i**seçin.
3. **SpoketoHub** eşlemi'ni seçin.
4. **VNet hub'dan VNet-Spoke'a iletilen trafiğe izin ver**altında **Etkin'i**seçin.
5. **Kaydet'i**seçin.

## <a name="create-the-routes"></a>Yolları oluşturma

Şimdi birkaç yol oluşturun:

- Güvenlik duvarı IP adresi üzerinden hub ağ geçidi alt ağından uç alt ağına giden bir yol
- Güvenlik duvarı IP adresi üzerinden uç alt ağından gelen varsayılan yol

1. Azure portalı giriş sayfasından **kaynak oluştur'u**seçin.
2. Arama metin kutusunda **rota tablosunu** yazın ve **Enter**tuşuna basın.
3. **Rota tablosunu**seçin.
4. **Oluştur'u**seçin.
5. Ad için **UDR-Hub-Spoke**yazın.
6. Kaynak grubu için **FW-Hybrid-Test'i** seçin.
8. **Konum**için **(ABD) Doğu ABD'yi**seçin.
9. **Oluştur'u**seçin.
10. Rota tablosu oluşturulduktan sonra, rota tablosu sayfasını açmak için onu seçin.
11. Sol sütunda **Rotalar'ı** seçin.
12. **Ekle'yi**seçin.
13. Rota adı için **ToSpoke**yazın.
14. Adres öneki için **10.6.0.0/16**yazın.
15. Sonraki atlama türü için **Sanal cihaz'ı**seçin.
16. Sonraki atlama adresi için, güvenlik duvarının daha önce belirttiğiniz özel IP adresini yazın.
17. **Tamam'ı**seçin.

Şimdi alt ağa giden yolu ilişkilendir.

1. **UDR-Hub-Spoke - Rotalar** sayfasında **Alt Ağları**seçin.
2. **Ortak'ı**seçin.
4. **Sanal ağ**altında **VNet hub'ı**seçin.
5. **Subnet**altında **GatewaySubnet'i**seçin.
6. **Tamam'ı**seçin.

Şimdi konuşan alt ağdan varsayılan rotayı oluşturun.

1. Azure portalı giriş sayfasından **kaynak oluştur'u**seçin.
2. Arama metin kutusunda **rota tablosunu** yazın ve **Enter**tuşuna basın.
3. **Rota tablosunu**seçin.
5. **Oluştur'u**seçin.
6. Ad için **UDR-DG**yazın.
7. Kaynak grubu için **FW-Hybrid-Test'i** seçin.
8. **Konum**için **(ABD) Doğu ABD'yi**seçin.
4. **Sanal ağ ağ geçidi rotası yayılımı**için Devre **Dışı'yı**seçin.
1. **Oluştur'u**seçin.
2. Rota tablosu oluşturulduktan sonra, rota tablosu sayfasını açmak için onu seçin.
3. Sol sütunda **Rotalar'ı** seçin.
4. **Ekle'yi**seçin.
5. Rota adı için **ToHub**yazın.
6. Adres öneki için **0.0.0.0/0**yazın.
7. Sonraki atlama türü için **Sanal cihaz'ı**seçin.
8. Sonraki atlama adresi için, güvenlik duvarının daha önce belirttiğiniz özel IP adresini yazın.
9. **Tamam'ı**seçin.

Şimdi alt ağa giden yolu ilişkilendir.

1. **UDR-DG - Rotalar** sayfasında **Alt Ağlar'ı**seçin.
2. **Ortak'ı**seçin.
4. **Sanal ağ**altında, **VNet-spoke'yi**seçin.
5. **Subnet**altında **SN-İş Yükü'nü**seçin.
6. **Tamam'ı**seçin.

## <a name="create-virtual-machines"></a>Sanal makineler oluşturma

Şimdi konuşan iş yükünü ve şirket içi sanal makineleri oluşturun ve bunları uygun alt ağlara yerleştirin.

### <a name="create-the-workload-virtual-machine"></a>İş yükü sanal makinesi oluşturma

Ortak IP adresi olmadan IIS çalıştıran, konuşan sanal ağda sanal bir makine oluşturun.

1. Azure portalı giriş sayfasından **kaynak oluştur'u**seçin.
2. **Popüler**altında, **Windows Server 2016 Datacenter'ı**seçin.
3. Sanal makine için şu değerleri girin:
    - **Kaynak grubu** - **FW-Hybrid-Test'i**seçin.
    - **Sanal makine adı**: *VM-Spoke-01*.
    - **Bölge** - *(ABD) Doğu ABD)*.
    - **Kullanıcı adı**: *azureuser*.
    - **Şifre**: şifrenizi yazın

4. **Sonraki:Diskleri**seçin.
5. Varsayılanları kabul edin ve **Sonraki: Ağ' ı**seçin.
6. Sanal ağ için **VNet-Spoke'yi** seçin ve alt ağ **SN-İş**Yükü'dür.
7. **Genel IP için Yok'u**seçin. **None**
8. **Ortak gelen bağlantı noktaları** **için, Seçili bağlantı noktalarına izin ver'i**seçin ve ardından HTTP **(80)** ve **RDP (3389)** 'yi seçin
9. **Sonraki'ni seçin:Yönetim.**
10. **Önyükleme tanılama için**, **Kapalı**seçin.
11. **Gözden Geçir+Oluştur'u**seçin, özet sayfasındaki ayarları gözden geçirin ve ardından **Oluştur'u**seçin.

### <a name="install-iis"></a>IIS yükleme

1. Azure portalından Bulut Kabuğu'nu açın ve **PowerShell**olarak ayarlandıklarından emin olun.
2. Sanal makineye IIS yüklemek ve gerekirse konumu değiştirmek için aşağıdaki komutu çalıştırın:

   ```azurepowershell-interactive
   Set-AzVMExtension `
           -ResourceGroupName FW-Hybrid-Test `
           -ExtensionName IIS `
           -VMName VM-Spoke-01 `
           -Publisher Microsoft.Compute `
           -ExtensionType CustomScriptExtension `
           -TypeHandlerVersion 1.4 `
           -SettingString '{"commandToExecute":"powershell Add-WindowsFeature Web-Server; powershell      Add-Content -Path \"C:\\inetpub\\wwwroot\\Default.htm\" -Value $($env:computername)"}' `
           -Location EastUS
   ```

### <a name="create-the-on-premises-virtual-machine"></a>Şirket içi sanal makineyi oluşturun

Bu, Uzak Masaüstü'nü kullanarak genel IP adresine bağlanmak için kullandığınız sanal bir makinedir. Buradan, güvenlik duvarı üzerinden şirket içi sunucuya bağlanırsınız.

1. Azure portalı giriş sayfasından **kaynak oluştur'u**seçin.
2. **Popüler**altında, **Windows Server 2016 Datacenter'ı**seçin.
3. Sanal makine için şu değerleri girin:
    - **Kaynak grubu** - Varolan'ı seçin ve ardından **FW-Hybrid-Test'i**seçin.
    - **Sanal makine adı** - *VM-Onprem*.
    - **Bölge** - *(ABD) Doğu ABD)*.
    - **Kullanıcı adı**: *azureuser*.
    - **Şifre**: şifrenizi yazın.

4. **Sonraki:Diskleri**seçin.
5. Varsayılanları kabul edin ve **İleri:Ağ'ı**seçin.
6. Sanal ağ için **VNet-Onprem'i** seçin ve alt ağın **SN-Corp**olduğunu doğrulayın.
7. **Ortak gelen bağlantı noktaları** **için, seçili bağlantı noktalarına izin ver'i**seçin ve ardından **RDP'yi seçin (3389)**
8. **Sonraki'ni seçin:Yönetim.**
9. **Önyükleme tanılama için** **Kapalı'yı**seçin.
10. **Gözden Geçir+Oluştur'u**seçin, özet sayfasındaki ayarları gözden geçirin ve ardından **Oluştur'u**seçin.

## <a name="test-the-firewall"></a>Güvenlik duvarını test etme

1. İlk olarak, VM-Spoke-01 sanal makinenin özel IP adresini VM-Spoke-01 Genel Bakış sayfasında not alın.

2. Azure portalından, **VM-Onprem** sanal makinesine bağlanın.
<!---2. Open a Windows PowerShell command prompt on **VM-Onprem**, and ping the private IP for **VM-spoke-01**.

   You should get a reply.--->
3. **VM-Onprem**üzerinde bir web tarayıcısı açın\<ve http:// VM-spoke-01 özel IP\>göz atın.

   **VM-spoke-01** web sayfasını görmelisiniz: ![VM-Spoke-01 web sayfası](media/secure-hybrid-network/vm-spoke-01-web.png)

4. **VM-Onprem** sanal makineden, özel IP adresinden uzak bir masaüstünü **VM-spoke-01'e** açın.

   Bağlantınız başarılı olmalı ve oturum açabilmelisiniz.

Şimdi güvenlik duvarı kurallarının işe yaradığını doğruladınız:

<!---- You can ping the server on the spoke VNet.--->
- Konuşan sanal ağdaki web sunucusuna göz atabilirsiniz.
- RDP kullanarak konuşan sanal ağdaki sunucuya bağlanabilirsiniz.

Ardından, güvenlik duvarı kurallarının beklendiği gibi çalıştığını doğrulamak için güvenlik duvarı ağ kuralı koleksiyonu eylemini **Reddet** olarak değiştirin.

1. **FW-Hybrid-Test** kaynak grubunu açın ve **Pol-Net01**güvenlik duvarı ilkesini seçin.
2. **Ayarlar**altında **Kurallar'ı**seçin.
3. **Ağ kurallarına** **göre, RCNet01** kural koleksiyonunu seçin, elipsleri (...) seçin ve **Edit'i**seçin.
4. **Kural toplama eylemi**için **Reddet'i**seçin.
5. **Kaydet'i**seçin.

Değiştirilen kuralları test etmeden önce **VM-Onprem'deki** mevcut uzak masaüstü ve tarayıcıları kapatın. Kural koleksiyonu güncelleştirmesi tamamlandıktan sonra testleri yeniden çalıştırın. Bu kez tümü başarısız olmalıdır.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Güvenlik duvarı kaynaklarını bir sonraki öğretici için tutabilirsiniz veya artık gerekli değilse **FW-Hybrid-Test** kaynak grubunu silerek güvenlik duvarıyla ilgili tüm kaynakları silebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Öğretici: Azure Güvenlik Duvarı Yöneticisi önizlemesini kullanarak sanal WAN'ınızı güvenli hale](secure-cloud-network.md)