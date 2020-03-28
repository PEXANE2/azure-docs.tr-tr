---
title: Azure Sanal WAN kullanarak Azure'a Noktadan Siteye bağlantı oluşturma | Microsoft Docs
description: Bu öğreticide Azure Sanal WAN kullanarak Azure'a Noktadan Siteye bağlantı oluşturmayı öğreneceksiniz.
services: virtual-wan
author: anzaman
ms.service: virtual-wan
ms.topic: tutorial
ms.date: 03/18/2020
ms.author: alzam
ms.openlocfilehash: fd415e1da00f52a9a3b55c946a07a30cf841cf4a
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "80060303"
---
# <a name="tutorial-create-a-user-vpn-connection-using-azure-virtual-wan"></a>Öğretici: Azure Virtual WAN'ı kullanarak Kullanıcı VPN bağlantısı oluşturun

Bu öğreticide Sanal WAN kullanarak Azure'daki kaynaklarınıza bir IPsec/IKE (IKEv2) veya OpenVPN VPN bağlantısı üzerinden bağlanmayı öğreneceksiniz. Bu tür bir bağlantı, istemci bilgisayarda bir istemcinin yapılandırılmış olmasını gerektirir. Virtual WAN hakkında daha fazla bilgi için [Sanal WAN Genel Bakış](virtual-wan-about.md)

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * WAN oluşturma
> * Hub oluşturma
> * P2S yapılandırması oluşturma
> * VPN istemci profili indirin
> * P2S yapılandırmasını bir hub'a uygulama
> * Bir sanal ağı bir hub'a bağlama
> * VPN istemci yapılandırmasını indirme ve uygulama
> * Sanal WAN'ınızı görüntüleme

![Sanal WAN diyagramı](./media/virtual-wan-about/virtualwanp2s.png)

## <a name="before-you-begin"></a>Başlamadan önce

Yapılandırmanıza başlamadan önce aşağıdaki ölçütleri karşıladığınızı doğrulayın:

* Bağlanmak istediğiniz bir sanal ağınız var. Şirket içi ağlarınızın hiçbirinin bağlanmak istediğiniz sanal ağlarla örtüşmediğini doğrulayın. Azure portalında sanal ağ oluşturmak için [hızlı başlat'a](../virtual-network/quick-create-portal.md)bakın.

* Sanal ağınızın herhangi bir sanal ağ ağ geçidi yok. Sanal ağınızın bir ağ geçidi (VPN veya ExpressRoute) varsa, tüm ağ geçitlerini kaldırmanız gerekir. Bu yapılandırma, sanal ağların bunun yerine Sanal WAN hub ağ geçidine bağlanmasını gerektirir.

* Hub bölgenizden bir IP adresi aralığı edinin. Hub, Virtual WAN tarafından oluşturulan ve kullanılan sanal bir ağdır. Hub için belirttiğiniz adres aralığı, bağlandığınız varolan sanal ağlarınizle çakışamaz. Ayrıca bağlandığınız şirket içi adres aralıklarıyla da çakışamaz. Şirket içi ağ yapılandırmanızda bulunan IP adresi aralıklarını bilmiyorsanız, bu ayrıntıları sizin için sağlayabilecek biriyle işbirliği yapabilirsiniz.

* Azure aboneliğiniz yoksa, ücretsiz bir [hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)oluşturun.

## <a name="create-a-virtual-wan"></a><a name="wan"></a>Sanal WAN oluşturma

Bir tarayıcıdan [Azure portalına](https://portal.azure.com) gidin ve Azure hesabınızla oturum açın.

1. Sanal WAN sayfasına gidin. Portalda **+Kaynak oluştur**’a tıklayın. Arama kutusuna **Sanal WAN** yazın ve Enter'u seçin.
2. Sonuçlardan **Sanal WAN'ı** seçin. Sanal WAN sayfasında, WAN Oluştur sayfasını açmak için **Oluştur'u** tıklatın.
3. WAN **Oluştur** sayfasında, **Temel Bilgiler** sekmesinde aşağıdaki alanları doldurun:

   ![Sanal WAN](./media/virtual-wan-point-to-site-portal/vwan.png)

   * **Abonelik**: Kullanmak istediğiniz aboneliği seçin.
   * **Kaynak grubu** - Yeni oluşturun veya varolanları kullanın.
   * **Kaynak grubu konumu** - Açılır kaynak konumundan bir kaynak konumu seçin. WAN, global bir kaynaktır ve belirli bir bölgeyle sınırlı değildir. Ancak oluşturduğunuz WAN kaynağını daha kolay yönetmek ve bulmak için bir bölge seçmeniz gerekir.
   * **Ad** - WAN adını vermek istediğiniz Adı yazın.
   * **Türü:** Standart. Temel WAN oluşturursanız, yalnızca bir Temel hub oluşturabilirsiniz. Temel hub'lar yalnızca VPN'den siteye bağlantı yeteneğine sahiptir.
4. Alanları doldurmayı bitirdikten sonra **Gözden Geçir +Oluştur'u**seçin.
5. Doğrulama geçtikten sonra, sanal WAN oluşturmak için **Oluştur'u** seçin.

## <a name="create-an-empty-virtual-hub"></a><a name="hub"></a>Boş bir sanal hub oluşturma

1. Sanal WAN'ınızın altında Hub'ları seçin ve **+Yeni Hub'ı** tıklatın

   ![yeni site](media/virtual-wan-point-to-site-portal/hub1.jpg)
2. Sanal hub oluştur sayfasında aşağıdaki alanları doldurun.

   **Bölge** - Sanal hub'ı dağıtmak istediğiniz bölgeyi seçin.

   **Ad** - Sanal hub'ınızı aramak istediğiniz adı girin.

   **Hub özel adres alanı** - CIDR gösteriminde hub'ın adres aralığı.

   ![yeni site](media/virtual-wan-point-to-site-portal/hub2.jpg)  
3. **Gözden Geçir + oluştur'a** tıklayın
4. Geçirilen **doğrulama** **sayfasında, oluştur'u** tıklatın

## <a name="create-a-p2s-configuration"></a><a name="p2sconfig"></a>P2S yapılandırması oluşturma

P2S yapılandırması, uzak istemcilerin bağlanmasına yönelik parametreleri tanımlar.

1. **Tüm kaynaklar**'a gidin.
2. Oluşturduğunuz sanal WAN'a tıklayın.
3. **Yeni Kullanıcı VPN yapılandırma** sayfası oluşturmak için sayfanın üst kısmında **+Create user VPN config'i** tıklatın.

   ![yeni site](media/virtual-wan-point-to-site-portal/p2s1.jpg)
4. Yeni **kullanıcı VPN yapılandırması oluştur** sayfasında aşağıdaki alanları doldurun:

   **Yapılandırma adı**: Yapılandırmanıza vermek istediğiniz addır.

   **Tünel türü** - Tünel için kullanılacak protokol.

   **Kök Sertifika Adı** - Sertifika için açıklayıcı bir ad.

   **Genel Sertifika Verileri** - Base-64 kodlanmış X.509 sertifika verileri.
  
5. Yapılandırmayı oluşturmak için **Oluştur** seçeneğine tıklayın.

## <a name="edit-hub-assignment"></a><a name="edit"></a>Hub atamasını düzenleme

1. Sanal WAN altında **Hubs** bıçak gidin
2. Vpn sunucusu yapılandırmasını ilişkilendirmek istediğiniz hub'ı seçin ve **...**

   ![yeni site](media/virtual-wan-point-to-site-portal/p2s4.jpg)
3. **Sanal hub'ı edit'i**tıklatın.
4. **Yer-iş geçidi ekle onay** kutusunu işaretleyin ve istediğiniz **Ağ Geçidi ölçeği birimini** seçin.

   ![yeni site](media/virtual-wan-point-to-site-portal/p2s2.jpg)
5. VPN istemcilerine IP adreslerinin atanacağı **Adres havuzunu** girin.
6. **Onayla'yı** tıklatın
7. İşlemin tamamlanması 30 dakika kadar sürebilir.

## <a name="download-vpn-profile"></a><a name="download"></a>VPN profili indirme

İstemcilerinizi yapılandırmak için VPN profilini kullanın.

1. Sanal WAN'ınız için sayfada **Kullanıcı VPN yapılandırmalarını**tıklatın.
2. Sayfanın üst kısmında, **kullanıcı VPN config indir'i**tıklatın.
3. Dosya oluşturulduktan sonra bağlantıya tıklayarak indirebilirsiniz.
4. VPN istemcilerini yapılandırmak için profil dosyasını kullanın.

### <a name="configure-user-vpn-clients"></a>Kullanıcı VPN istemcilerini yapılandırma
Uzak erişim istemcilerini yapılandırmak için indirilen profili kullanın. Her işletim sisteminin yordamı farklıdır. Lütfen aşağıdaki doğru talimatları izleyin:

#### <a name="microsoft-windows"></a>Microsoft Windows
##### <a name="openvpn"></a>OpenVPN

1. Resmi web sitesinden OpenVPN istemcisini indirip yükleyin.
2. Ağ geçidinin VPN profilini indirin. Bu, Azure portalındaki Kullanıcı VPN yapılandırmaları sekmesinden veya PowerShell'de Yeni AzureRmVpnClientConfiguration'dan yapılabilir.
3. Profilin sıkıştırmasını açın. OpenVPN klasöründeki vpnconfig.ovpn yapılandırma dosyasını not defterinde açın.
4. P2S istemci sertifikası bölümünü base64’teki P2S istemci sertifikası genel anahtarı ile doldurun. PEM biçimli bir sertifikada .cer dosyasını açıp base64 anahtarını sertifika üst bilgileri arasına kopyalamanız yeterlidir. Adımlar için, kodlanmış ortak anahtarı almak için sertifika verme nasıl gün [eğretilir'](certificates-point-to-site.md) e bakın.
5. Özel anahtar bölümünü, base64’teki P2S istemci sertifikası özel anahtarı ile doldurun. Adımlar için, [özel anahtarın nasıl ayıklanır'](howto-openvpn-clients.md#windows)olduğunu görün.
6. Başka bir alanı değiştirmeyin. VPN’e bağlanmak için istemci girişinde doldurulmuş yapılandırmayı kullanın.
7. vpnconfig.ovpn dosyasını C:\Program Files\OpenVPN\config klasörüne kopyalayın.
8. Sistem tepsisindeki OpenVPN simgesine sağ tıklayın ve Bağlan’a tıklayın.

##### <a name="ikev2"></a>IKEv2

1. Windows bilgisayarın mimarisine karşılık gelen VPN istemcisi yapılandırma dosyalarını seçin. 64 bit işlemci mimarisi için 'VpnClientSetupAmd64' yükleyici paketini seçin. 32 bit işlemci mimarisi için 'VpnClientSetupX86' yükleyici paketini seçin.
2. Yüklemek için pakete çift tıklayın. Bir SmartScreen açılır penceresi görürseniz Daha fazla bilgi’ye ve ardından Yine de çalıştır’a tıklayın.
3. İstemci bilgisayarda Ağ Ayarları’na gidin ve VPN öğesine tıklayın. VPN bağlantısı, bağlandığı sanal ağın adını gösterir.
4. Bağlanmayı denemeden önce, istemci bilgisayara bir istemci sertifikası yüklediğinizi doğrulayın. Yerel Azure sertifika kimlik doğrulaması türü kullanılırken kimlik doğrulaması için bir istemci sertifikası gereklidir. Sertifika oluşturma hakkında daha fazla bilgi için [bkz.](certificates-point-to-site.md) İstemci sertifikasının nasıl yüklenirhakkında bilgi [için](../vpn-gateway/point-to-site-how-to-vpn-client-install-azure-cert.md)bkz.

## <a name="view-your-virtual-wan"></a><a name="viewwan"></a>Sanal WAN'ınızı görüntüleme

1. Sanal WAN'a gidin.
2. Genel bakış sayfasında haritadaki her bir nokta bir hub'ı temsil eder.
3. Hub'lar ve bağlantılar bölümünde herhangi bir hub'ın durumunu, sitesini, bölgesini, VPN bağlantısı durumunu ve gelen/giden baytları görüntüleyebilirsiniz.


## <a name="clean-up-resources"></a><a name="cleanup"></a>Kaynakları temizleme

Bu kaynaklar artık gerekli değilse, [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) komutunu kullanarak kaynak grubunu ve içerdiği tüm kaynakları kaldırabilirsiniz. "myResourceGroup" yerine kaynak grubunuzun adını yazın ve aşağıdaki PowerShell komutunu çalıştırın:

```azurepowershell-interactive
Remove-AzResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Sonraki adımlar

Sanal WAN hakkında daha fazla bilgi için [Sanal WAN'a Genel Bakış](virtual-wan-about.md) sayfasına bakın.
