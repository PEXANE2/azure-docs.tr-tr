---
title: Sanal ağ ağ geçidini Azure Sanal WAN'a bağlayın
description: Bu makale, bir Azure sanal ağ ağ geçidini Azure Virtual WAN VPN ağ geçidine bağlamanıza yardımcı olur
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 03/19/2020
ms.author: cherylmc
ms.openlocfilehash: 688183bc07aa14d5e5df182d7de0897cec93f0b9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80066245"
---
# <a name="connect-a-vpn-gateway-virtual-network-gateway-to-virtual-wan"></a>Sanal WAN'a VPN Ağ Geçidi (sanal ağ ağ geçidi) bağlama

Bu makale, bir Azure VPN Ağ Geçidi'nden (sanal ağ ağ geçidi) azure sanal wan (VPN ağ geçidi) bağlantısı ayarlamanıza yardımcı olur. VPN Ağ Geçidi'nden (sanal ağ ağ geçidi) Sanal WAN'a (VPN ağ geçidi) bağlantı oluşturmak, şube VPN sitelerinden sanal bir WAN bağlantısı kurmaya benzer.

İki özellik arasındaki olası karışıklığı en aza indirmek için, ağ geçidine atıfta bulunduğumuz özelliğin adı ile önsöz edeceğiz. Örneğin, VPN Ağ Geçidi sanal ağ ağ geçidi ve Virtual WAN VPN ağ geçidi.

## <a name="before-you-begin"></a>Başlamadan önce

Başlamadan önce aşağıdaki kaynakları oluşturun:

Azure Sanal WAN

* [Sanal bir WAN oluşturun.](virtual-wan-site-to-site-portal.md#openvwan)
* [Bir hub oluşturun.](virtual-wan-site-to-site-portal.md#hub) Sanal hub, Virtual WAN VPN ağ geçidini içerir.

Azure Sanal Ağ

* Herhangi bir sanal ağ ağ geçidi olmadan sanal ağ oluşturun. Şirket içi ağlarınızın hiçbirinin bağlanmak istediğiniz sanal ağlarla örtüşmediğini doğrulayın. Azure portalında sanal ağ oluşturmak için [Hızlı Başlangıç'a](../virtual-network/quick-create-portal.md)bakın.

## <a name="1-create-an-azure-virtual-network-gateway"></a><a name="vnetgw"></a>1. Azure sanal ağ ağ geçidi oluşturma

Sanal ağınız için etkin modda sanal ağınız için bir VPN Ağ Geçidi sanal ağ ağ geçidi oluşturun. Ağ geçidini oluşturduğunuzda, ağ geçidinin iki örneği için varolan genel IP adreslerini kullanabilir veya yeni genel IP'ler oluşturabilirsiniz. Sanal WAN sitelerini kurarken bu ortak IP'leri kullanırsınız. Etkin-etkin mod hakkında daha fazla bilgi için [bkz.](../vpn-gateway/vpn-gateway-activeactive-rm-powershell.md#aagateway)

### <a name="active-active-mode-setting"></a><a name="active-active"></a>Etkin mod ayarı

![etkin-edilgen](./media/connect-virtual-network-gateway-vwan/active.png "etkin-edilgen")

### <a name="bgp-setting"></a><a name="BGP"></a>BGP ayarı

BGP ASN 65515 olamaz. 66515, Azure Virtual WAN tarafından kullanılacaktır.

![BGP](./media/connect-virtual-network-gateway-vwan/bgp.png "Bgp")

### <a name="public-ip-addresses"></a><a name="pip"></a>Genel IP adresleri

Ağ geçidi oluşturulduğunda, **Özellikler** sayfasına gidin. Özellikler ve yapılandırma ayarları aşağıdaki örneğe benzer olacaktır. Ağ geçidi için kullanılan iki genel IP adresine dikkat edin.

![Özellikler](./media/connect-virtual-network-gateway-vwan/publicip.png "properties")

## <a name="2-create-virtual-wan-vpn-sites"></a><a name="vwansite"></a>2. Sanal WAN VPN siteleri oluşturun

Sanal WAN VPN siteleri oluşturmak için, sanal WAN'ınıza gidin ve **Connectivity**altında **VPN sitelerini**seçin. Bu bölümde, önceki bölümde oluşturduğunuz sanal ağ ağ ağ geçitlerine karşılık gelen iki Sanal WAN VPN sitesi oluşturacaksınız.

1. **+Site Oluştur'u**seçin.
2. VPN **siteleri oluştur** sayfasında aşağıdaki değerleri yazın:

   * **Bölge** - (Azure VPN Ağ Geçidi sanal ağ ağ geçidi ile aynı bölge)
   * **Aygıt satıcısı** - Aygıt satıcısını girin (herhangi bir ad)
   * **Özel adres alanı** - (Bir değer girin veya BGP etkinleştirildiğinde boş bırakın)
   * **Kenarlık Ağ Geçidi Protokolü** - (Azure VPN Ağ Geçidi sanal ağ ağ geçidi BGP etkinse **Etkinleştirme** için ayarlayın)
   * **Hub'lara bağlanın** (Açılır açılır daki ön koşullarda oluşturduğunuz hub'ı seçin)
3. **Linkler**altında, aşağıdaki değerleri girin:

   * **Sağlayıcı Adı** - Bağlantı adı ve Sağlayıcı adı (herhangi bir ad) girin
   * **Hız** - Hız (herhangi bir sayı)
   * **IP Adresi** - IP adresini girin ((VPN Ağ Geçidi) sanal ağ ağ geçidi özellikleri altında gösterilen ilk genel IP adresiyle aynı)
   * **BGP Adresi** ve **ASN** - BGP adresi ve ASN. Bunlar, [Adım 1'de](#vnetgw)yapılandırdığınız VPN Ağ Geçidi sanal ağ ağ geçidinden Gelen BGP eş IP adreslerinden biriyle ve ASN ile aynı olmalıdır.
4. Siteyi oluşturmak için onu gözden geçirin ve **Onayla'yı** seçin.
5. VPN Ağ Geçidi sanal ağ ağ ağ geçidinin ikinci örneğiyle eşleşen ikinci siteyi oluşturmak için önceki adımları yineleyin. VPN Ağ Geçidi yapılandırmasından ikinci genel IP adresi ve ikinci BGP eş IP adresi kullanmak dışında aynı ayarları tutarsınız.
6. Şimdi başarıyla sağlanan iki site var ve yapılandırma dosyalarını indirmek için bir sonraki bölüme geçebilirsiniz.

## <a name="3-download-the-vpn-configuration-files"></a><a name="downloadconfig"></a>3. VPN yapılandırma dosyalarını indirin

Bu bölümde, önceki bölümde oluşturduğunuz sitelerin her biri için VPN yapılandırma dosyasını indirin.

1. Sanal WAN **VPN siteleri** sayfasının üst kısmında, **Site'yi**seçin, ardından **Siteden siteye VPN yapılandırmasını karşıdan yükle'yi**seçin. Azure, ayarlarla birlikte bir yapılandırma dosyası oluşturur.

   ![yapılandırma dosyasını indir](./media/connect-virtual-network-gateway-vwan/download.png "indirin")
2. Yapılandırma dosyasını indirin ve açın.
3. İkinci site için bu adımları yineleyin. Her iki yapılandırma dosyasını da açtıktan sonra bir sonraki bölüme geçebilirsiniz.

## <a name="4-create-the-local-network-gateways"></a><a name="createlocalgateways"></a>4. Yerel ağ ağ geçitlerini oluşturma

Bu bölümde, iki Azure VPN Ağ Geçidi yerel ağ ağ geçidi oluşturursunuz. Önceki adımdaki yapılandırma dosyaları ağ geçidi yapılandırma ayarlarını içerir. Azure VPN Ağ Geçidi yerel ağ ağ ağ geçitlerini oluşturmak ve yapılandırmak için bu ayarları kullanın.

1. Bu ayarları kullanarak yerel ağ ağ geçidini oluşturun. VPN Ağ Geçidi yerel ağ ağ ağ geçidi oluşturma hakkında bilgi için VPN Ağ Geçidi makalesiyerel [ağ ağ geçidi oluşturun'a](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md#LocalNetworkGateway)bakın.

   * **IP adresi** - Yapılandırma dosyasından *ağ geçidi yapılandırması* için gösterilen Örnek0 IP Adresini kullanın.
   * **BGP** - Bağlantı BGP üzerindeyse, **BGP ayarlarını yapılandır'ı** seçin ve ASN '65515' girin. BGP eş IP adresini girin. Yapılandırma dosyasından *ağ geçidi yapılandırması* için 'Instance0 BgpPeeringAdress' kullanın.
   * **Abonelik, Kaynak Grubu ve Konum,** Virtual WAN hub'ı ile aynıdır.
2. Yerel ağ ağ ağ geçidini gözden geçirin ve oluşturun. Yerel ağ ağ ağ geçidiniz bu örneğe benzer olmalıdır.

   ![yapılandırma dosyasını indir](./media/connect-virtual-network-gateway-vwan/lng1.png "örnek0")
3. Başka bir yerel ağ ağ geçidi oluşturmak için bu adımları yineleyin, ancak bu sefer yapılandırma dosyasındaki 'Örnek0' değerleri yerine 'Örnek1' değerlerini kullanın.

   ![yapılandırma dosyasını indir](./media/connect-virtual-network-gateway-vwan/lng2.png "örnek1")

## <a name="5-create-connections"></a><a name="createlocalgateways"></a>5. Bağlantı oluşturma

Bu bölümde, VPN Ağ Geçidi yerel ağ ağ ağ geçitleri ile sanal ağ ağ geçidi arasında bir bağlantı oluşturursunuz. VPN Ağ Geçidi bağlantısı oluşturma adımlarını öğrenmek için [bkz.](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md#CreateConnection)

1. Portalda, sanal ağ ağ ağ geçidinize gidin ve **Bağlantılar'ı**tıklatın. Bağlantılar sayfasının üstündeki **+Ekle**’ye tıklayarak **Bağlantı ekle** sayfasını açın.
2. Bağlantı **Ekle** sayfasında bağlantınız için aşağıdaki değerleri yapılandırın:

   * **Ad:** Bağlantınızı adlandırın.
   * **Bağlantı türü:** **Siteden siteye(IPSec)** seçin
   * **Sanal ağ geçidi:** Bu ağ geçidinden bağlandığınız için değer sabittir.
   * **Yerel ağ ağ geçidi:** Bu bağlantı, sanal ağ ağ ağ geçidini yerel ağ ağ geçidine bağlar. Daha önce oluşturduğunuz yerel ağ ağ ağ geçitlerinden birini seçin.
   * **Paylaşılan Anahtar:** Paylaşılan bir anahtar girin.
   * **IKE Protokolü:** IKE protokolünü seçin.
   * **BGP:** Bağlantı BGP üzerindeyse **BGP'yi etkinleştir'i** seçin.
3. Bağlantınızı oluşturmak için **Tamam**’a tıklayın.
4. Bağlantıyı sanal ağ geçidinin **Bağlantılar** sayfasında görüntüleyebilirsiniz.

   ![Bağlantı](./media/connect-virtual-network-gateway-vwan/connect.png "bağlantı")
5. İkinci bir bağlantı oluşturmak için önceki adımları yineleyin. İkinci bağlantı için oluşturduğunuz diğer yerel ağ ağ ağ geçidini seçin.

## <a name="6-test-connections"></a><a name="test"></a>6. Test bağlantıları

İki sanal makine oluşturarak bağlantıyı test edebilirsiniz, vpn ağ geçidi nin yan tarafında bir, ve Sanal WAN için sanal ağ da bir, ve sonra iki sanal makineleri ping.

1. Azure VPN Ağ Geçidi (Test1-VNG) için sanal ağda (Test1-VNet) sanal bir makine oluşturun. GatewaySubnet'te sanal makineyi oluşturmayın.
2. Sanal WAN'a bağlanmak için başka bir sanal ağ oluşturun. Bu sanal ağın alt ağında sanal bir makine oluşturun. Bu sanal ağ herhangi bir sanal ağ ağ geçidi içeremez. [Siteden siteye bağlantı](virtual-wan-site-to-site-portal.md#vnet) makalesindeki PowerShell adımlarını kullanarak hızla sanal ağ oluşturabilirsiniz. Cmdlets çalıştırmadan önce değerleri değiştirmek için emin olun.
3. VNet'i Virtual WAN hub'ına bağlayın. Sanal WAN'ınız için sayfada **Sanal ağ bağlantılarını**seçin ve **ardından +Bağlantı Ekle'yi**seçin. **Bağlantı ekle** sayfasında aşağıdaki alanları doldurun:

    * **Bağlantı adı**: Bağlantınıza bir ad verin.
    * **Hub'lar**: Bu bağlantıyla ilişkilendirmek istediğiniz hub'ı seçin.
    * **Abonelik**: Aboneliği doğrulayın.
    * **Sanal ağ**: Bu hub'a bağlamak istediğiniz sanal ağı seçin. Sanal ağda önceden var olan bir sanal ağ geçidi bulunamaz.
4. Sanal ağ bağlantısını oluşturmak için **Tamam'ı** tıklatın.
5. Bağlantı artık VM'ler arasında ayarlanır. İletişimi engelleyen herhangi bir güvenlik duvarı veya başka ilkeler olmadığı sürece, bir VM'yi diğerinden pingleyebilmelisin.

## <a name="next-steps"></a>Sonraki adımlar

Özel bir IPsec ilkesini yapılandırmak için adımlar için, [Virtual WAN için özel bir IPsec ilkesini yapılandırın'a](virtual-wan-custom-ipsec-portal.md)bakın.
Virtual WAN hakkında daha fazla bilgi için Azure Sanal WAN ve Azure [Sanal WAN SSS](virtual-wan-faq.md) [hakkında](virtual-wan-about.md) bilgi alabiliyorum.