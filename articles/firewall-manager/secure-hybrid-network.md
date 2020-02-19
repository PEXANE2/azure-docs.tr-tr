---
title: 'Öğretici: Azure Güvenlik Duvarı Yöneticisi önizleme kullanarak hub Sanal ağınızı güvenli hale getirme'
description: Bu öğreticide, Azure portal kullanarak sanal ağınızın güvenliğini Azure Güvenlik Duvarı Yöneticisi ile nasıl sağlayacağınızı öğreneceksiniz.
services: firewall-manager
author: vhorne
ms.service: firewall-manager
ms.topic: tutorial
ms.date: 02/18/2020
ms.author: victorh
ms.openlocfilehash: cdd416bdb833e4784334a6847d724a7375e2ef8d
ms.sourcegitcommit: 6ee876c800da7a14464d276cd726a49b504c45c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77459962"
---
# <a name="tutorial-secure-your-hub-virtual-network-using-azure-firewall-manager-preview"></a>Öğretici: Azure Güvenlik Duvarı Yöneticisi önizleme kullanarak hub Sanal ağınızı güvenli hale getirme 

[!INCLUDE [Preview](../../includes/firewall-manager-preview-notice.md)]

Karma ağ oluşturmak için şirket içi ağınızı bir Azure sanal ağına bağladığınızda, Azure ağ kaynaklarınıza erişimi denetleme özelliği, genel bir güvenlik planının önemli bir parçasıdır.

Azure Güvenlik Duvarı Yöneticisi önizlemesi 'ni kullanarak, özel IP adreslerine, Azure PaaS 'ye ve Internet 'e yönelik karma ağ trafiğinizi güvenli hale getirmek için bir hub sanal ağı oluşturabilirsiniz. Azure Güvenlik Duvarı Yöneticisi ' ni, izin verilen ve reddedilen ağ trafiğini tanımlayan ilkeleri kullanarak bir karma ağdaki ağ erişimini denetlemek için kullanabilirsiniz.

Güvenlik Duvarı Yöneticisi ayrıca güvenli bir sanal hub mimarisini destekler. Güvenli sanal hub ve hub sanal ağ mimarisi türleri karşılaştırması için bkz [. Azure Güvenlik Duvarı Yöneticisi mimari seçenekleri nelerdir?](vhubs-and-vnets.md)

Bu öğretici için üç sanal ağ oluşturursunuz:

- **VNET-hub** -güvenlik duvarı bu sanal ağda.
- **VNET-kol** -bağlı bileşen sanal ağı, Azure üzerinde bulunan iş yükünü temsil eder.
- **VNET-Onpred** -şirket içi sanal ağ, bir şirket içi ağı temsil eder. Gerçek bir dağıtımda, bir VPN veya ExpressRoute bağlantısı ile bağlanabilir. Kolaylık olması için, bu öğretici bir VPN Ağ Geçidi bağlantısı kullanır ve şirket içi ağı temsil etmek için Azure 'da bulunan bir sanal ağ kullanılır.

![Hibrit ağ](media/tutorial-hybrid-portal/hybrid-network-firewall.png)

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Güvenlik duvarı ilkesi oluşturma
> * Sanal ağları oluşturma
> * Güvenlik duvarını yapılandırma ve dağıtma
> * VPN ağ geçitlerini oluşturma ve bağlama
> * Hub ve bağlı bileşen sanal ağlarını eşler
> * Yolları oluşturma
> * Sanal makineleri oluşturma
> * Güvenlik duvarını test etme


## <a name="prerequisites"></a>Önkoşullar

Karma ağ, Azure sanal ağları ile şirket içi ağlar arasında trafiği yönlendirmek için hub ve bağlı bileşen mimarisi modelini kullanır. Hub ve bağlı bileşen mimarisi aşağıdaki gereksinimlere sahiptir:

- VNet-hub ' ı VNet 'e eşlemeden **Allowgatewaytransit** ayarlayın. Hub ve bağlı bileşen ağ mimarisinde, bir ağ geçidi geçişi, bağlı olan sanal ağların her bağlı olan sanal ağda VPN ağ geçitleri dağıtmak yerine hub 'da VPN ağ geçidini paylaşmasına izin verir. 

   Ayrıca, ağ geçidi ile bağlantılı sanal ağlara veya şirket içi ağlara yapılan yollar, ağ geçidi geçişi kullanılarak eşlenen sanal ağlar için otomatik olarak yönlendirme tablolarına yayılır. Daha fazla bilgi için bkz. [sanal ağ eşlemesi IÇIN VPN ağ geçidi geçişi yapılandırma](../vpn-gateway/vpn-gateway-peering-gateway-transit.md).

- Sanal ağ hub 'ına sanal ağa bağlı olarak, **Useremotegateway** 'leri ayarlayın. **Useremotegateway** 'ler ayarlandıysa ve uzaktan eşleme üzerinde **allowgatewaytransit** da ayarlandıysa, bağlı olan sanal ağ, aktarım için uzak sanal ağın ağ geçitlerini kullanır.
- Alt ağ trafiğini hub güvenlik duvarıyla yönlendirmek için, **sanal ağ geçidi yol yayma** ayarı devre dışı olan güvenlik duvarını Işaret eden Kullanıcı tanımlı bir yol (UDR) gerekir. Bu seçenek, bağlı olan alt ağlara yönlendirme dağıtımını önler. Bu, öğrenilen yolların UDR 'niz ile çakışmasını önler.
- Hub ağ geçidi alt ağında, bağlı olan ağların sonraki atlaması olarak güvenlik duvarı IP adresini işaret eden bir UDR yapılandırın. Azure Güvenlik Duvarı alt ağında BGP 'deki yolları öğrendiğinden dolayı UDR gerekmez.

Bu yolların nasıl oluşturulduğunu görmek için [Yolları Oluşturma](#create-the-routes) bölümüne bakın.

>[!NOTE]
>Azure Güvenlik duvarının doğrudan Internet bağlantısı olmalıdır. AzureFirewallSubnet, BGP aracılığıyla şirket içi ağınıza varsayılan bir yol öğrenirse, doğrudan Internet bağlantısını sürdürmek için **Nexthoptype** değeri **Internet** olarak ayarlanmış bir 0.0.0.0/0 UDR ile geçersiz kılmanız gerekir.
>
>Azure Güvenlik Duvarı, zorlamalı tüneli destekleyecek şekilde yapılandırılabilir. Daha fazla bilgi için bkz. [Azure Güvenlik Duvarı Zorlamalı tünel](../firewall/forced-tunneling.md).

>[!NOTE]
>Doğrudan eşlenmiş sanal ağlar arasındaki trafik, bir UDR varsayılan ağ geçidi olarak Azure Güvenlik Duvarı 'na işaret ediyorsa doğrudan yönlendirilir. Alt ağ trafiğine Bu senaryodaki güvenlik duvarının alt ağını göndermek için, her iki alt ağda de bir UDR 'nin hedef alt ağ önekini açıkça içermesi gerekir.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="create-a-firewall-policy"></a>Güvenlik Duvarı Ilkesi oluşturma

1. [https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın.
2. Azure portal arama çubuğuna **güvenlik duvarı Yöneticisi** yazın ve **ENTER**tuşuna basın.
3. Azure Güvenlik Duvarı Yöneticisi sayfasında, **Azure Güvenlik Duvarı Ilkelerini görüntüle**' yi seçin.

   ![Güvenlik duvarı ilkesi](media/tutorial-hybrid-portal/firewall-manager-policy.png)

1. **Azure Güvenlik Duvarı Ilkesi oluştur**' u seçin.
1. Aboneliğinizi seçin ve kaynak grubu için, **Yeni oluştur** ' u seçin ve **ILT-hibrit-test**adlı bir kaynak grubu oluşturun.
2. İlke adı için, **pol-Net01**yazın.
3. Bölge için **Doğu ABD**' yi seçin.
4. **Sonraki: kurallar**' ı seçin.
5. **Kural koleksiyonu Ekle**' yi seçin.
6. **Ad**için **RCNet01**yazın.
7. **Kural koleksiyonu türü**için **ağ**' ı seçin.
8. **Öncelik**için **100**yazın.
9. **Eylem** alanında **İzin ver**'i seçin.
10. **Kurallar**' ın altında, **ad**için **allowweb**yazın.
11. **Kaynak adresleri**için **192.168.1.0/24**yazın.
12. **Protokol** alanında **TCP**'yi seçin.
13. **Hedef bağlantı noktaları**için **80**yazın.
14. **Hedef türü**Için **IP adresi**' ni seçin.
15. **Hedef**için **10.6.0.0/16**yazın.
16. Sonraki kural satırına aşağıdaki bilgileri girin:
 
    Ad: **Allowrdp** yazın<br>
    Kaynak IP adresi: **192.168.1.0/24**yazın.<br>
    Protokol, **TCP** seçin<br>
    Hedef bağlantı noktaları, **3389** yazın<br>
    Hedef türü, **IP adresi** seçin<br>
    Hedef için **10.6.0.0/16** yazın

1. **Add (Ekle)** seçeneğini belirleyin.
2. **Gözden geçir + oluştur**' u seçin.
3. Ayrıntıları gözden geçirin ve ardından **Oluştur**' u seçin.

## <a name="create-the-firewall-hub-virtual-network"></a>Güvenlik Duvarı hub 'ı sanal ağını oluşturma

> [!NOTE]
> AzureFirewallSubnet alt ağının boyutu/26 ' dır. Alt ağ boyutu hakkında daha fazla bilgi için bkz. [Azure Güvenlik DUVARı SSS](../firewall/firewall-faq.md#why-does-azure-firewall-need-a-26-subnet-size).

1. Azure portal giriş sayfasında, **kaynak oluştur**' u seçin.
2. **Ağ**altında **sanal ağ**' ı seçin.
4. **Ad**için **VNET-hub**yazın.
5. **Adres alanı**için **10.5.0.0/16**yazın.
6. **Abonelik** bölümünde aboneliğinizi seçin.
7. **Kaynak grubu**Için, **FW-karma-test**' i seçin.
8. **Konum**için **Doğu ABD**' yi seçin.
9. **Alt ağ** bölümünde **Ad** alanına **AzureFirewallSubnet** yazın. Güvenlik duvarı bu alt ağda yer alacaktır ve alt ağ adının **mutlaka** AzureFirewallSubnet olması gerekir.
10. **Adres aralığı**için **10.5.0.0/26**yazın. 
11. Diğer varsayılan ayarları kabul edin ve **Oluştur**' u seçin.

## <a name="create-the-spoke-virtual-network"></a>Bağlı bileşen sanal ağını oluşturma

1. Azure portal giriş sayfasında, **kaynak oluştur**' u seçin.
2. **Ağ**altında **sanal ağ**' ı seçin.
4. **Ad**için **VNET-kol**yazın.
5. **Adres alanı**için **10.6.0.0/16**yazın.
6. **Abonelik** bölümünde aboneliğinizi seçin.
7. **Kaynak grubu**Için, **FW-karma-test**' i seçin.
8. **Konum** alanında önceden kullandığınız konumu seçin.
9. **Alt ağ** altında, **Ad** için **SN-Workload** yazın.
10. **Adres aralığı**için **10.6.0.0/24**yazın.
11. Diğer varsayılan ayarları kabul edin ve **Oluştur**' u seçin.

## <a name="create-the-on-premises-virtual-network"></a>Şirket içi sanal ağı oluşturma

1. Azure portal giriş sayfasında, **kaynak oluştur**' u seçin.
2. **Ağ**altında **sanal ağ**' ı seçin.
4. **Ad**için **VNET-onpred**yazın.
5. **Adres alanı** için **192.168.0.0/16** yazın.
6. **Abonelik** bölümünde aboneliğinizi seçin.
7. **Kaynak grubu**Için, **FW-karma-test**' i seçin.
8. **Konum** alanında önceden kullandığınız konumu seçin.
9. **Alt ağ**altında, **ad** türü **sn-Corp**için.
10. **Adres aralığı** için **192.168.1.0/24** yazın.
11. Diğer varsayılan ayarları kabul edin ve **Oluştur**' u seçin.

Sanal ağ dağıtıldıktan sonra, ağ geçidi için ikinci bir alt ağ oluşturun.

1. **VNET-Onpreb** sayfasında **alt ağlar**' ı seçin.
2. **+ Alt ağ**' ı seçin.
3. **Ad**Için **gatewaysubnet**yazın.
4. **Adres aralığı (CIDR bloğu)** için **192.168.2.0/24**yazın.
5. **Tamam**’ı seçin.

### <a name="create-a-public-ip-address"></a>Genel IP adresi oluşturma

Bu, şirket içi ağ geçidi için kullanılan genel IP adresidir.

1. Azure portal giriş sayfasında, **kaynak oluştur**' u seçin.
2. Arama metin kutusuna **genel IP adresi** yazın ve **ENTER**tuşuna basın.
3. **Genel IP adresi** ' ni seçin ve ardından **Oluştur**' u seçin.
4. Ad için **VNET-Onprea-GW-PI**yazın.
5. Kaynak grubu için, **FW-karma-test**yazın.
6. **Konum**için **Doğu ABD**Seç ' i seçin.
7. Diğer varsayılanları kabul edin ve **Oluştur**' u seçin.

## <a name="configure-and-deploy-the-firewall"></a>Güvenlik duvarını yapılandırma ve dağıtma

Güvenlik ilkeleri bir hub ile ilişkilendirildiğinde, *hub sanal ağı*olarak adlandırılır.

**VNET hub** sanal ağını bir *hub sanal ağına* dönüştürün ve Azure Güvenlik Duvarı ile güvenli hale getirin.

1. Azure portal arama çubuğuna **güvenlik duvarı Yöneticisi** yazın ve **ENTER**tuşuna basın.
3. Azure Güvenlik Duvarı Yöneticisi sayfasında, **sanal ağlara güvenlik Ekle**' nin altında, **hub sanal ağlarını görüntüle**' yi seçin.
4. **Sanal ağları Dönüştür**' ü seçin.
5. **VNET hub 'ı** seçin ve ardından İleri ' yi seçin **: Azure Güvenlik Duvarı**.
6. **Güvenlik duvarı ilkesi**Için, **pol-Net01**' ı seçin.
7. **İleri Seç "gözden geçir + Onayla**
8. Ayrıntıları gözden geçirin ve ardından **Onayla**' yı seçin.


   Bu, dağıtılması birkaç dakika sürer.
7. Dağıtım tamamlandıktan sonra, **FW-karma-test** kaynak grubuna gidin ve güvenlik duvarını seçin.
9. **Genel bakış** sayfasındaki **GÜVENLIK duvarı özel IP** adresine göz atın. Varsayılan rotayı oluştururken bu adresi kullanacaksınız.

## <a name="create-and-connect-the-vpn-gateways"></a>VPN ağ geçitlerini oluşturma ve bağlama

Hub ve şirket içi sanal ağlar VPN ağ geçitleri aracılığıyla bağlanır.

### <a name="create-a-vpn-gateway-for-the-hub-virtual-network"></a>Hub sanal ağı için bir VPN ağ geçidi oluşturma

Şimdi hub sanal ağı için VPN ağ geçidini oluşturun. Ağdan ağa yapılandırma, RouteBased bir VpnType gerektirir. VPN ağ geçidinin oluşturulması, seçili VPN ağ geçidi SKU’suna bağlı olarak 45 dakika veya daha uzun sürebilir.

1. Azure portal giriş sayfasında, **kaynak oluştur**' u seçin.
2. Arama metin kutusuna **sanal ağ geçidi** yazın ve **ENTER**tuşuna basın.
3. **Sanal ağ geçidi**' ni seçin ve **Oluştur**' u seçin.
4. **Ad**için **GW-hub**yazın.
5. **Bölge**Için **(US) Doğu ABD**seçin.
6. **Ağ geçidi türü**için **VPN**' yi seçin.
7. **VPN türü**için **Rota tabanlı**' ı seçin.
8. **SKU**için **temel**' yı seçin.
9. **Sanal ağ**için **VNET hub**' ı seçin.
10. **Genel IP adresi**Için **Yeni oluştur**' u seçin ve ad için **VNET-hub-GW-PI** yazın.
11. Kalan Varsayılanları kabul edin ve ardından **gözden geçir + oluştur**' u seçin.
12. Yapılandırmayı gözden geçirin ve **Oluştur**' u seçin.

### <a name="create-a-vpn-gateway-for-the-on-premises-virtual-network"></a>Şirket içi sanal ağ için bir VPN ağ geçidi oluşturma

Şimdi, şirket içi sanal ağ için VPN ağ geçidini oluşturun. Ağdan ağa yapılandırma, RouteBased bir VpnType gerektirir. VPN ağ geçidinin oluşturulması, seçili VPN ağ geçidi SKU’suna bağlı olarak 45 dakika veya daha uzun sürebilir.

1. Azure portal giriş sayfasında, **kaynak oluştur**' u seçin.
2. Arama metin kutusuna **sanal ağ geçidi** yazın ve **ENTER**tuşuna basın.
3. **Sanal ağ geçidi**' ni seçin ve **Oluştur**' u seçin.
4. **Ad**için **GW-onpred**yazın.
5. **Bölge**Için **(US) Doğu ABD**seçin.
6. **Ağ geçidi türü**için **VPN**' yi seçin.
7. **VPN türü**için **Rota tabanlı**' ı seçin.
8. **SKU**için **temel**' yı seçin.
9. **Sanal ağ**için **VNET-onpred**öğesini seçin.
10. **Genel IP adresi**için, **Varolanı kullan*' ı seçin ve ad Için **VNET-onprea-GW-PI** ' yi seçin.
11. Kalan Varsayılanları kabul edin ve ardından **gözden geçir + oluştur**' u seçin.
12. Yapılandırmayı gözden geçirin ve **Oluştur**' u seçin.

### <a name="create-the-vpn-connections"></a>VPN bağlantılarını oluşturma

Artık hub ve şirket içi ağ geçitleri arasında VPN bağlantıları oluşturabilirsiniz.

Bu adımda, hub sanal ağından şirket içi sanal ağa bağlantı oluşturursunuz. Örneklerde paylaşılan bir anahtar göreceksiniz. Paylaşılan anahtar için kendi değerlerinizi kullanabilirsiniz. Paylaşılan anahtarın her iki bağlantıyla da eşleşiyor olması önemlidir. Bir bağlantı oluşturmak çok zaman almaz.

1. **İlt-hibrit-test** kaynak grubunu açın ve **GW-hub** ağ geçidini seçin.
2. Sol sütundaki **Bağlantılar** ' ı seçin.
3. **Add (Ekle)** seçeneğini belirleyin.
4. Bağlantı adı, **hub-Onpree**yazın.
5. **Bağlantı türü**için **VNET-VNET** ' i seçin.
6. **İkinci sanal ağ geçidi**için **GW-onpred**öğesini seçin.
7. **Paylaşılan anahtar (PSK)** için **AzureA1b2C3**yazın.
8. **Tamam**’ı seçin.

Şirket içinden hub 'a sanal ağ bağlantısını oluşturun. Bu adım, VNet-Onpree ile VNet hub 'ına bağlantı oluşturmanız dışında öncekiyle benzerdir. Paylaşılan anahtarların eşleştiğinden emin olun. Bağlantı birkaç dakika içerisinde kurulacaktır.

1. ILT- **hibrit-test** kaynak grubunu açın ve **GW-onprea** Gateway ' i seçin.
2. Sol sütundaki **Bağlantılar** ' ı seçin.
3. **Add (Ekle)** seçeneğini belirleyin.
4. Bağlantı adı, **Onpree-hub**' ı yazın.
5. **Bağlantı türü**için **VNET-VNET** ' i seçin.
6. **İkinci sanal ağ geçidi**için **GW-hub**' ı seçin.
7. **Paylaşılan anahtar (PSK)** için **AzureA1b2C3**yazın.
8. **Tamam**’ı seçin.


#### <a name="verify-the-connection"></a>Bağlantıyı doğrulama

Yaklaşık beş dakika sonra, her iki bağlantının da durumu **bağlanmalıdır**.

![Ağ Geçidi bağlantıları](media/secure-hybrid-network/gateway-connections.png)

## <a name="peer-the-hub-and-spoke-virtual-networks"></a>Hub ve bağlı bileşen sanal ağlarını eşler

Artık hub ve bağlı bileşen sanal ağlarını eşler.

1. **İlt-hibrit-test** kaynak grubunu açın ve **VNET hub** sanal ağını seçin.
2. Sol sütunda, eşlemeler ' i **seçin.**
3. **Add (Ekle)** seçeneğini belirleyin.
4. **Ad**Için **hubtokol**yazın.
5. **Sanal ağ**için **VNET-bağlı bileşen** ' i seçin
6. Sanal ağ hub 'ından VNet hub 'ına eşleme adı için **Spoketohub**yazın.
7. **Ağ geçidi aktarımına Izin ver**' i seçin.
8. **Tamam**’ı seçin.

### <a name="configure-additional-settings-for-the-spoketohub-peering"></a>SpoketoHub eşlemesi için ek ayarlar yapılandırma

SpoketoHub eşlemesi için **iletilen trafiğe Izin ver** ' i etkinleştirmeniz gerekir.

1. **İlt-hibrit-test** kaynak grubunu açın ve **VNET-bağlı** sanal ağını seçin.
2. Sol sütunda, eşlemeler ' i **seçin.**
3. **Spoketohub** eşlemesini seçin.
4. **VNET hub 'ından sanal ağa yönlendirilen trafiğe Izin ver**altında, **etkin**' i seçin.
5. **Kaydet**’i seçin.

## <a name="create-the-routes"></a>Yolları oluşturma

Şimdi birkaç yol oluşturun:

- Güvenlik duvarı IP adresi üzerinden hub ağ geçidi alt ağından uç alt ağına giden bir yol
- Güvenlik duvarı IP adresi üzerinden uç alt ağından gelen varsayılan yol

1. Azure portal giriş sayfasında, **kaynak oluştur**' u seçin.
2. Arama metin kutusuna **yol tablosu** yazın ve **ENTER**tuşuna basın.
3. **Yol tablosu**' nu seçin.
4. **Oluştur**’u seçin.
5. Ad için **UDR-hub-kol**yazın.
6. Kaynak grubu için **FW-karma-test** ' i seçin.
8. **Konum**Için **(US) Doğu ABD)** seçeneğini belirleyin.
9. **Oluştur**’u seçin.
10. Yol tablosu oluşturulduktan sonra, yol tablosu sayfasını açmak için seçin.
11. Sol sütundaki **rotalar** ' ı seçin.
12. **Add (Ekle)** seçeneğini belirleyin.
13. Yol adı için, uzak **bileşene**yazın.
14. Adres ön eki için **10.6.0.0/16**yazın.
15. Sonraki atlama türü için **Sanal Gereç**' ı seçin.
16. Sonraki atlama adresi için, daha önce not ettiğiniz güvenlik duvarının özel IP adresini yazın.
17. **Tamam**’ı seçin.

Şimdi rota alt ağıyla ilişkilendirin.

1. **UDR-hub-bağlı bileşen-yollar** sayfasında, **alt ağlar**' ı seçin.
2. **İlişkilendir**' i seçin.
4. **Sanal ağ**altında **VNET-hub**' ı seçin.
5. **Alt ağ**altında **gatewaysubnet**' i seçin.
6. **Tamam**’ı seçin.

Şimdi, bağlı olan alt ağdan varsayılan yolu oluşturun.

1. Azure portal giriş sayfasında, **kaynak oluştur**' u seçin.
2. Arama metin kutusuna **yol tablosu** yazın ve **ENTER**tuşuna basın.
3. **Yol tablosu**' nu seçin.
5. **Oluştur**’u seçin.
6. Ad için **UDR-DG**yazın.
7. Kaynak grubu için **FW-karma-test** ' i seçin.
8. **Konum**Için **(US) Doğu ABD)** seçeneğini belirleyin.
4. **Sanal ağ geçidi yol yayma**Için **devre dışı**' yı seçin.
1. **Oluştur**’u seçin.
2. Yol tablosu oluşturulduktan sonra, yol tablosu sayfasını açmak için seçin.
3. Sol sütundaki **rotalar** ' ı seçin.
4. **Add (Ekle)** seçeneğini belirleyin.
5. Yol adı için, **Tohub**yazın.
6. Adres ön eki için **0.0.0.0/0**yazın.
7. Sonraki atlama türü için **Sanal Gereç**' ı seçin.
8. Sonraki atlama adresi için, daha önce not ettiğiniz güvenlik duvarının özel IP adresini yazın.
9. **Tamam**’ı seçin.

Şimdi rota alt ağıyla ilişkilendirin.

1. **UDR-DG-Routes** sayfasında **alt ağlar**' ı seçin.
2. **İlişkilendir**' i seçin.
4. **Sanal ağ**altında, **VNET-bağlı bileşen**' i seçin.
5. **Alt ağ**altında **sn-iş yükü**öğesini seçin.
6. **Tamam**’ı seçin.

## <a name="create-virtual-machines"></a>Sanal makineler oluşturma

Artık bağlı olan iş yükünü ve şirket içi sanal makineleri oluşturun ve uygun alt ağlara yerleştirin.

### <a name="create-the-workload-virtual-machine"></a>İş yükü sanal makinesi oluşturma

Genel IP adresi olmadan IIS çalıştıran, bağlı olan sanal ağda bir sanal makine oluşturun.

1. Azure portal giriş sayfasında, **kaynak oluştur**' u seçin.
2. **Popüler**bölümünde **Windows Server 2016 Datacenter**' u seçin.
3. Sanal makine için şu değerleri girin:
    - **Kaynak grubu** - **ILT-hibrit-test**' i seçin.
    - **Sanal makine adı**: *VM-ışınsal-01*.
    - **Bölge** -  *(US) Doğu ABD)* .
    - **Kullanıcı adı**: *azureuser*.
    - **Parola**: parolanızı yazın

4. **İleri ' yi seçin: diskler**.
5. Varsayılanları kabul edin ve **İleri ' yi seçin: ağ**.
6. Sanal ağ için **VNET-ışınsal** ve alt ağ **sn-iş yükü**' ı seçin.
7. **Genel IP**için **hiçbiri**' ni seçin.
8. **Ortak gelen bağlantı noktaları**için **Seçili bağlantı noktalarına izin ver**' i seçin ve ardından **http (80)** ve **RDP (3389)** öğesini seçin.
9. Ileri 'yi seçin **: yönetim**.
10. **Önyükleme tanılaması**için **kapalı**' yı seçin.
11. **Gözden geçir + oluştur**' u seçin, Özet sayfasında ayarları gözden geçirin ve ardından **Oluştur**' u seçin.

### <a name="install-iis"></a>IIS yükleme

1. Azure portal, Cloud Shell açın ve **PowerShell**olarak ayarlandığından emin olun.
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

### <a name="create-the-on-premises-virtual-machine"></a>Şirket içi sanal makine oluşturma

Bu, Uzak Masaüstü kullanarak genel IP adresine bağlanmak için kullandığınız bir sanal makinedir. Buradan, daha sonra güvenlik duvarı aracılığıyla şirket içi sunucuya bağlanırsınız.

1. Azure portal giriş sayfasında, **kaynak oluştur**' u seçin.
2. **Popüler**bölümünde **Windows Server 2016 Datacenter**' u seçin.
3. Sanal makine için şu değerleri girin:
    - **Kaynak grubu** -mevcut ' ı seçin ve ardından **FW-karma-test**' i seçin.
    - **Sanal makine adı** - *VM-onpred*.
    - **Bölge** -  *(US) Doğu ABD)* .
    - **Kullanıcı adı**: *azureuser*.
    - **Parola**: parolanızı yazın.

4. **İleri ' yi seçin: diskler**.
5. Varsayılanları kabul edin ve **İleri ' yi seçin: ağ**.
6. Sanal ağ için **VNET-Onpred** öğesini seçin ve alt ağın **sn-Corp**olduğunu doğrulayın.
7. **Ortak gelen bağlantı noktaları**için **Seçili bağlantı noktalarına izin ver**' i seçin ve ardından **RDP (3389)** öğesini seçin
8. Ileri 'yi seçin **: yönetim**.
9. **Önyükleme tanılaması**için **kapalı**' yı seçin.
10. **Gözden geçir + oluştur**' u seçin, Özet sayfasında ayarları gözden geçirin ve ardından **Oluştur**' u seçin.

## <a name="test-the-firewall"></a>Güvenlik duvarını test etme

1. İlk olarak, VM-ışınsal-01 Genel Bakış sayfasında VM-ışınsal-01 sanal makinesi için özel IP adresine göz atın.

2. Azure portalından, **VM-Onprem** sanal makinesine bağlanın.
<!---2. Open a Windows PowerShell command prompt on **VM-Onprem**, and ping the private IP for **VM-spoke-01**.

   You should get a reply.--->
3. **VM-Onpred**üzerinde bir Web tarayıcısı açın ve http://\<VM-ışınsal-01 özel IP\>gidin.

   VM-kol- **01** Web sayfasını görmeniz gerekir: ![VM-ışınsal-01 web sayfası](media/secure-hybrid-network/vm-spoke-01-web.png)

4. **VM-Onpree** sanal makinesinden, özel IP adresinde **VM-ışınsal-01 arası** uzak bir masaüstü açın.

   Bağlantınız başarılı olmalıdır ve oturum açabiliyor olmalısınız.

Artık Güvenlik Duvarı kurallarının çalıştığını doğruladınız:

<!---- You can ping the server on the spoke VNet.--->
- Web sunucusuna bağlı bileşen sanal ağı üzerinde gezinebilirsiniz.
- RDP kullanarak, bağlı olan sanal ağdaki sunucuya bağlanabilirsiniz.

Ardından, güvenlik duvarı kurallarının beklendiği gibi çalıştığını doğrulamak için güvenlik duvarı ağ kuralı koleksiyonu eylemini **Reddet** olarak değiştirin.

1. **İlt-hibrit-test** kaynak grubunu açın ve **pol-Net01**güvenlik duvarı ilkesini seçin.
2. **Ayarlar**altında **kurallar**' ı seçin.
3. **Ağ kuralları**altında **RCNet01** kural koleksiyonunu seçin, üç nokta (...) simgesini seçin ve **Düzenle**' yi seçin.
4. **Kural toplama eylemi**için **Reddet**' i seçin.
5. **Kaydet**’i seçin.

Değiştirilen kuralları test etmeden önce **VM** 'deki mevcut uzak masaüstlerini ve tarayıcıları kapatın. Kural koleksiyonu güncelleştirmesi tamamlandıktan sonra, testleri yeniden çalıştırın. Bu kez tümü başarısız olmalıdır.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Güvenlik duvarı kaynaklarını bir sonraki öğretici için tutabilirsiniz veya artık gerekli değilse **FW-Hybrid-Test** kaynak grubunu silerek güvenlik duvarıyla ilgili tüm kaynakları silebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Öğretici: Azure Güvenlik Duvarı Yöneticisi önizlemesi kullanarak sanal WAN 'nizin güvenliğini sağlama](secure-cloud-network.md)