---
title: Azure Sanal WAN kullanarak Azure'a Noktadan Siteye bağlantı oluşturma | Microsoft Docs
description: Bu öğreticide Azure Sanal WAN kullanarak Azure'a Noktadan Siteye bağlantı oluşturmayı öğreneceksiniz.
services: virtual-wan
author: anzaman
ms.service: virtual-wan
ms.topic: tutorial
ms.date: 06/29/2020
ms.author: alzam
ms.openlocfilehash: 9c93ad0357011008c45b2898260a655509b02dc2
ms.sourcegitcommit: 73ac360f37053a3321e8be23236b32d4f8fb30cf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/30/2020
ms.locfileid: "85560721"
---
# <a name="tutorial-create-a-user-vpn-connection-using-azure-virtual-wan"></a>Öğretici: Azure sanal WAN kullanarak bir kullanıcı VPN bağlantısı oluşturma

Bu öğreticide Sanal WAN kullanarak Azure'daki kaynaklarınıza bir IPsec/IKE (IKEv2) veya OpenVPN VPN bağlantısı üzerinden bağlanmayı öğreneceksiniz. Bu tür bir bağlantı, istemci bilgisayarda bir istemcinin yapılandırılmış olmasını gerektirir. Sanal WAN hakkında daha fazla bilgi için bkz. [sanal WAN genel bakış](virtual-wan-about.md)

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * WAN oluşturma
> * P2S yapılandırması oluşturma
> * Hub oluşturma
> * DNS sunucularını belirtme
> * VPN istemci profilini indir
> * Sanal WAN'ınızı görüntüleme

![Sanal WAN diyagramı](./media/virtual-wan-about/virtualwanp2s.png)

## <a name="before-you-begin"></a>Başlamadan önce

Yapılandırmanıza başlamadan önce aşağıdaki ölçütleri karşıladığınızı doğrulayın:

* Bağlanmak istediğiniz bir sanal ağınız var. Şirket içi ağlarınızın alt ağlarının hiçbirinin, bağlanmak istediğiniz sanal ağlarla çakışmadığından emin olun. Azure portal bir sanal ağ oluşturmak için [hızlı](../virtual-network/quick-create-portal.md)başlangıca bakın.

* Sanal ağınızda sanal ağ geçidi yok. Sanal ağınızda bir ağ geçidi (VPN veya ExpressRoute) varsa, tüm ağ geçitlerini kaldırmanız gerekir. Bu yapılandırma, sanal ağın bunun yerine sanal WAN hub ağ geçidine bağlanmasını gerektirir.

* Hub bölgenizden bir IP adresi aralığı edinin. Hub, sanal WAN tarafından oluşturulan ve kullanılan bir sanal ağ. Hub için belirttiğiniz adres aralığı, bağlandığınız mevcut sanal ağlarınızla çakışamaz. Ayrıca bağlandığınız şirket içi adres aralıklarıyla da çakışamaz. Şirket içi ağ yapılandırmanızda bulunan IP adresi aralıklarını tanımıyorsanız, sizin için bu ayrıntıları sağlayabilecek biriyle koordine edebilirsiniz.

* Azure aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)oluşturun.

## <a name="create-a-virtual-wan"></a><a name="wan"></a>Sanal WAN oluşturma

Bir tarayıcıdan [Azure portalına](https://portal.azure.com) gidin ve Azure hesabınızla oturum açın.

1. Sanal WAN sayfasına gidin. Portalda **+ kaynak oluştur**' u seçin. Arama kutusuna **sanal WAN** yazın ve **ENTER**' u seçin.
1. Sonuçlardan **sanal WAN** ' ı seçin. Sanal WAN sayfasında, **Oluştur** ' u seçerek WAN sayfası oluştur sayfasını açın.
1. **WAN oluştur** sayfasında, **temel bilgiler** sekmesinde aşağıdaki alanları girin:

   ![Sanal WAN](./media/virtual-wan-point-to-site-portal/vwan.png)

   * **Abonelik**: Kullanmak istediğiniz aboneliği seçin.
   * **Kaynak grubu** -yeni oluştur veya var olanı kullan.
   * **Kaynak grubu konumu** -açılan listeden bir kaynak konumu seçin. WAN, global bir kaynaktır ve belirli bir bölgeyle sınırlı değildir. Ancak oluşturduğunuz WAN kaynağını daha kolay yönetmek ve bulmak için bir bölge seçmeniz gerekir.
   * **Ad** -WAN 'nizi çağırmak istediğiniz adı yazın.
   * **Şunu yazın:** Stand. Temel bir WAN oluşturursanız, yalnızca temel bir hub oluşturabilirsiniz. Temel hub 'lar yalnızca VPN siteden siteye bağlantı özelliğine sahiptir.
1. Alanları doldurmayı tamamladıktan sonra, **gözden geçir + oluştur**' u seçin.
1. Doğrulama başarılı olduktan sonra, sanal WAN oluşturmak için **Oluştur** ' u seçin.

## <a name="create-a-p2s-configuration"></a><a name="p2sconfig"></a>P2S yapılandırması oluşturma

P2S yapılandırması, uzak istemcilerin bağlanmasına yönelik parametreleri tanımlar.

1. **Tüm kaynaklar**'a gidin.
1. Oluşturduğunuz sanal WAN 'ı seçin.
1. **Yeni Kullanıcı VPN yapılandırması oluştur** sayfasını açmak için sayfanın üst kısmındaki **+ Kullanıcı VPN yapılandırması oluştur** ' u seçin.

   :::image type="content" source="media/virtual-wan-point-to-site-portal/p2s1.jpg" alt-text="Kullanıcı VPN yapılandırması":::

1. **Yeni Kullanıcı VPN yapılandırması oluştur** sayfasında, aşağıdaki alanları girin:

   * **Yapılandırma adı**: Yapılandırmanıza vermek istediğiniz addır.
   * **Tünel türü** - Tünel için kullanılacak protokol.
   * **Kök Sertifika Adı** - Sertifika için açıklayıcı bir ad.
   * **Ortak sertifika verileri** -Base-64 Ile kodlanmış X. 509.440 sertifika verileri.
  
1. Yapılandırmayı oluşturmak için **Oluştur** ' u seçin.

## <a name="create-hub-with-point-to-site-gateway"></a><a name="hub"></a>Noktadan siteye ağ geçidi ile hub oluşturma

1. Sanal WAN 'ınız altında, hub ' ı seçin ve **+ yeni merkez**' i seçin.

   :::image type="content" source="media/virtual-wan-point-to-site-portal/hub1.jpg" alt-text="Yeni Merkez":::

1. Sanal hub Oluştur sayfasında aşağıdaki alanları girin.

   * **Bölge** -sanal hub 'ı dağıtmak istediğiniz bölgeyi seçin.
   * **Ad** -sanal hub 'ınızı çağırmak istediğiniz adı girin.
   * **Hub özel adres alanı** -hub 'ın CIDR gösteriminde adres aralığı.

   :::image type="content" source="media/virtual-wan-point-to-site-portal/hub2.jpg" alt-text="sanal hub oluştur":::

1. Noktadan siteye sekmesinde aşağıdaki alanları doldurun:

   * **Ağ geçidi ölçek birimleri** -Kullanıcı VPN ağ geçidinin toplam kapasitesini temsil eder.
   * Önceki adımda oluşturduğunuz **Site Yapılandırması ' nın üzerine gelin** .
   * **Istemci adres havuzu** -uzak kullanıcılar için.
   * **Özel DNS sunucusu IP 'si**.

   :::image type="content" source="media/virtual-wan-point-to-site-portal/hub-with-p2s.png" alt-text="Noktadan siteye sahip hub":::

1. **İncele ve oluştur**’u seçin.
1. **Doğrulama başarılı** sayfasında **Oluştur**' u seçin.

## <a name="specify-dns-server"></a><a name="dns"></a>DNS sunucusunu belirtin

Sanal WAN Kullanıcı VPN ağ geçitleri, en fazla 5 DNS sunucusu belirtmenizi sağlar. Bunu, hub oluşturma işlemi sırasında yapılandırabilir veya daha sonra değiştirebilirsiniz. Bunu yapmak için, sanal hub 'ı bulun. **Kullanıcı VPN (site üzerine gelin)** altında, Yapılandır ' a tıklayın ve **özel DNS SUNUCULARı** metın kutularına DNS sunucusu IP adreslerini girin.

   :::image type="content" source="media/virtual-wan-point-to-site-portal/custom-dns.png" alt-text="özel DNS" lightbox="media/virtual-wan-point-to-site-portal/custom-dns-expand.png":::

## <a name="download-vpn-profile"></a><a name="download"></a>VPN profili indirme

İstemcilerinizi yapılandırmak için VPN profilini kullanın.

1. Sanal WAN 'nizin sayfasında, **Kullanıcı VPN yapılandırması**' nı seçin.
2. Sayfanın üst kısmında, **Kullanıcı VPN yapılandırmasını indir**' i seçin. WAN düzeyinde bir yapılandırmanın indirilmesi, yerleşik bir Traffic Manager tabanlı kullanıcı VPN profili sağlar. Genel profiller veya hub tabanlı profiller hakkında daha fazla bilgi için, bkz. bu [hub profilleri](https://docs.microsoft.com/azure/virtual-wan/global-hub-profile).   Yük devretme senaryoları genel profille basitleştirilmiştir.

   Bir nedenle, bir hub kullanılamaz durumdaysa, hizmet tarafından sunulan yerleşik trafik yönetimi, Noktadan siteye kullanıcılar için Azure kaynaklarına farklı bir hub üzerinden bağlantı sağlar. Belirli bir hub 'a giderek, her zaman hub 'a özgü VPN yapılandırmasını indirebilirsiniz. **Kullanıcı VPN (site üzerine gelin)** altında, sanal hub **Kullanıcı VPN** profilini indirin.

1. Dosya oluşturma işlemi tamamlandıktan sonra, bunu indirmek için bağlantıyı seçebilirsiniz.
1. VPN istemcilerini yapılandırmak için profil dosyasını kullanın.

### <a name="configure-user-vpn-clients"></a>Kullanıcı VPN istemcilerini yapılandırma

Uzak erişim istemcilerini yapılandırmak için indirilen profili kullanın. Her işletim sisteminin yordamı farklıdır. Lütfen aşağıdaki doğru talimatları izleyin:

#### <a name="microsoft-windows"></a>Microsoft Windows
##### <a name="openvpn"></a>OpenVPN

1. Resmi web sitesinden OpenVPN istemcisini indirip yükleyin.
1. Ağ geçidinin VPN profilini indirin. Bu, Azure portal veya PowerShell 'de New-AzureRmVpnClientConfiguration içindeki kullanıcı VPN yapılandırmaları sekmesinden yapılabilir.
1. Profilin sıkıştırmasını açın. OpenVPN klasöründeki vpnconfig.ovpn yapılandırma dosyasını not defterinde açın.
1. P2S istemci sertifikası bölümünü base64’teki P2S istemci sertifikası genel anahtarı ile doldurun. PEM biçimli bir sertifikada .cer dosyasını açıp base64 anahtarını sertifika üst bilgileri arasına kopyalamanız yeterlidir. Adımlar için bkz [. kodlanmış ortak anahtarı almak için sertifikayı dışarı aktarma.](certificates-point-to-site.md)
1. Özel anahtar bölümünü, base64’teki P2S istemci sertifikası özel anahtarı ile doldurun. Adımlar için bkz [. özel anahtar ayıklama.](howto-openvpn-clients.md#windows).
1. Başka bir alanı değiştirmeyin. VPN’e bağlanmak için istemci girişinde doldurulmuş yapılandırmayı kullanın.
1. vpnconfig.ovpn dosyasını C:\Program Files\OpenVPN\config klasörüne kopyalayın.
1. Sistem tepsisindeki OpenVPN simgesine sağ tıklayın ve **Bağlan**' ı seçin.

##### <a name="ikev2"></a>IKEv2

1. Windows bilgisayarın mimarisine karşılık gelen VPN istemcisi yapılandırma dosyalarını seçin. 64 bit işlemci mimarisi için 'VpnClientSetupAmd64' yükleyici paketini seçin. 32 bit işlemci mimarisi için 'VpnClientSetupX86' yükleyici paketini seçin.
1. Yüklemek için pakete çift tıklayın. Bir SmartScreen açılır penceresi görürseniz **daha fazla bilgi**' yi ve **yine de Çalıştır**' ı seçin.
1. İstemci bilgisayarda **ağ ayarları** ' na gidin ve **VPN**' yi seçin. VPN bağlantısı, bağlandığı sanal ağın adını gösterir.
1. Bağlanmayı denemeden önce, istemci bilgisayara bir istemci sertifikası yüklediğinizi doğrulayın. Yerel Azure sertifika kimlik doğrulaması türü kullanılırken kimlik doğrulaması için bir istemci sertifikası gereklidir. Sertifika oluşturma hakkında daha fazla bilgi için bkz. [sertifika oluşturma](certificates-point-to-site.md). İstemci sertifikasını nasıl yükleyeceğiniz hakkında bilgi için bkz. [istemci sertifikası yüklemesi](../vpn-gateway/point-to-site-how-to-vpn-client-install-azure-cert.md).

## <a name="view-your-virtual-wan"></a><a name="viewwan"></a>Sanal WAN'ınızı görüntüleme

1. Sanal WAN'a gidin.
1. **Genel bakış** sayfasında, haritadaki her bir nokta bir hub 'ı temsil eder.
1. Hub **'lar ve bağlantılar** bölümünde, hub durumu, site, bölge, VPN bağlantı durumu ve baytları içinde ve dışarı izleyebilirsiniz.

## <a name="clean-up-resources"></a><a name="cleanup"></a>Kaynakları temizleme

Bu kaynaklar artık gerekli değilse, [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) komutunu kullanarak kaynak grubunu ve içerdiği tüm kaynakları kaldırabilirsiniz. "myResourceGroup" yerine kaynak grubunuzun adını yazın ve aşağıdaki PowerShell komutunu çalıştırın:

```azurepowershell-interactive
Remove-AzResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Sonraki adımlar

Sanal WAN hakkında daha fazla bilgi için [Sanal WAN'a Genel Bakış](virtual-wan-about.md) sayfasına bakın.
