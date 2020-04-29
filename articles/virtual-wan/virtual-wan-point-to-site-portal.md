---
title: Azure Sanal WAN kullanarak Azure'a Noktadan Siteye bağlantı oluşturma | Microsoft Docs
description: Bu öğreticide Azure Sanal WAN kullanarak Azure'a Noktadan Siteye bağlantı oluşturmayı öğreneceksiniz.
services: virtual-wan
author: anzaman
ms.service: virtual-wan
ms.topic: tutorial
ms.date: 04/16/2020
ms.author: alzam
ms.openlocfilehash: 11007bc39cb1112799c89afaf0ca670aa6760de6
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "81482122"
---
# <a name="tutorial-create-a-user-vpn-connection-using-azure-virtual-wan"></a>Öğretici: Azure sanal WAN kullanarak bir kullanıcı VPN bağlantısı oluşturma

Bu öğreticide Sanal WAN kullanarak Azure'daki kaynaklarınıza bir IPsec/IKE (IKEv2) veya OpenVPN VPN bağlantısı üzerinden bağlanmayı öğreneceksiniz. Bu tür bir bağlantı, istemci bilgisayarda bir istemcinin yapılandırılmış olmasını gerektirir. Sanal WAN hakkında daha fazla bilgi için bkz. [sanal WAN genel bakış](virtual-wan-about.md)

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * WAN oluşturma
> * Hub oluşturma
> * P2S yapılandırması oluşturma
> * VPN istemci profilini indir
> * P2S yapılandırmasını bir hub'a uygulama
> * Bir sanal ağı bir hub'a bağlama
> * VPN istemci yapılandırmasını indirme ve uygulama
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

1. Sanal WAN sayfasına gidin. Portalda **+Kaynak oluştur**’a tıklayın. Arama kutusuna **sanal WAN** yazın ve ENTER ' u seçin.
2. Sonuçlardan **sanal WAN** ' ı seçin. Sanal WAN sayfasında, **Oluştur** ' a tıklayarak WAN sayfası oluştur sayfasını açın.
3. **WAN oluştur** sayfasında, **temel bilgiler** sekmesinde aşağıdaki alanları girin:

   ![Sanal WAN](./media/virtual-wan-point-to-site-portal/vwan.png)

   * **Abonelik**: Kullanmak istediğiniz aboneliği seçin.
   * **Kaynak grubu** -yeni oluştur veya var olanı kullan.
   * **Kaynak grubu konumu** -açılan listeden bir kaynak konumu seçin. WAN, global bir kaynaktır ve belirli bir bölgeyle sınırlı değildir. Ancak oluşturduğunuz WAN kaynağını daha kolay yönetmek ve bulmak için bir bölge seçmeniz gerekir.
   * **Ad** -WAN 'nizi çağırmak istediğiniz adı yazın.
   * **Şunu yazın:** Stand. Temel bir WAN oluşturursanız, yalnızca temel bir hub oluşturabilirsiniz. Temel hub 'lar yalnızca VPN siteden siteye bağlantı özelliğine sahiptir.
4. Alanları doldurmayı tamamladıktan sonra, **gözden geçir + oluştur**' u seçin.
5. Doğrulama başarılı olduktan sonra, sanal WAN oluşturmak için **Oluştur** ' u seçin.

## <a name="create-an-empty-virtual-hub"></a><a name="hub"></a>Boş bir sanal hub oluşturun

1. Sanal WAN 'ınız altında, hub ' ı seçin ve **+ yeni merkez** ' e tıklayın

   ![yeni site](media/virtual-wan-point-to-site-portal/hub1.jpg)
2. Sanal hub Oluştur sayfasında aşağıdaki alanları girin.

   **Bölge** -sanal hub 'ı dağıtmak istediğiniz bölgeyi seçin.

   **Ad** -sanal hub 'ınızı çağırmak istediğiniz adı girin.

   **Hub özel adres alanı** -hub 'ın CIDR gösteriminde adres aralığı.

   ![yeni site](media/virtual-wan-point-to-site-portal/hub2.jpg)  
3. **Gözden geçir + oluştur** 'a tıklayın
4. **Doğrulama başarılı** sayfasında **Oluştur** ' a tıklayın.

## <a name="create-a-p2s-configuration"></a><a name="p2sconfig"></a>P2S yapılandırması oluşturma

P2S yapılandırması, uzak istemcilerin bağlanmasına yönelik parametreleri tanımlar.

1. **Tüm kaynaklar**'a gidin.
2. Oluşturduğunuz sanal WAN'a tıklayın.
3. **Yeni Kullanıcı VPN yapılandırması oluştur** sayfasını açmak için sayfanın üst kısmındaki **+ Kullanıcı VPN yapılandırması oluştur** ' a tıklayın.

   ![yeni site](media/virtual-wan-point-to-site-portal/p2s1.jpg)
4. **Yeni Kullanıcı VPN yapılandırması oluştur** sayfasında, aşağıdaki alanları girin:

   **Yapılandırma adı**: Yapılandırmanıza vermek istediğiniz addır.

   **Tünel türü** - Tünel için kullanılacak protokol.

   **Kök Sertifika Adı** - Sertifika için açıklayıcı bir ad.

   **Ortak sertifika verileri** -Base-64 Ile kodlanmış X. 509.440 sertifika verileri.
  
5. Yapılandırmayı oluşturmak için **Oluştur** seçeneğine tıklayın.

## <a name="edit-hub-assignment"></a><a name="edit"></a>Hub atamasını düzenleme

1. Sanal WAN altındaki **hub** dikey penceresine gidin
2. VPN sunucusu yapılandırmasını ilişkilendirmek istediğiniz hub 'ı seçin ve **..** . öğesine tıklayın.

   ![yeni site](media/virtual-wan-point-to-site-portal/p2s4.jpg)
3. **Sanal hub 'ı Düzenle**' ye tıklayın.
4. **Noktadan siteye ağ geçidini dahil** et onay kutusunu işaretleyin ve Istediğiniz **ağ geçidi ölçek birimini** seçin.

   ![yeni site](media/virtual-wan-point-to-site-portal/p2s2.jpg)

Aşağıdaki tabloda kullanılabilir **ölçek birimleri** hakkındaki ayrıntılar gösterilmektedir

| **Ölçek birimi** | **Aktarım hızı** | **P2S Bağlantıları** |
| --- | --- | --- |
| 1| 500 Mbps | 500 |
| 2| 1 Gbps | 500 |
| 3| 1,5 Gbps | 500 |
| 4| 2 Gbps | 1000 |
| 5| 2,5 Gbps | 1000 |
| 6| 3 Gbps | 1000 |
| 7| 3,5 Gbps | 5000 |
| 8| 4 Gbps | 5000 |
| 9| 4,5 Gbps | 5000 |
| 10| 5 Gbps | 5000 |
| 11| 5,5 Gbps | 5000 |
| 12| 6 Gbps | 5000 |
| 13| 6,5 Gbps | 10000 |
| 14| 7 Gbps | 10000 |
| 15| 7,5 Gbps | 10000 |
| 16| 8 Gbps | 10000 |
| 17| 8,5 Gbps | 10000 |
| 18| 9 Gbps | 10000 |
| 19| 9,5 Gbps | 10000 |
| 20| 10 Gbps | 10000 |

5. VPN istemcilerine atanan IP adreslerine ait **adres havuzunu** girin.
6. **Onayla** 'yı tıklatın
7. İşlemin tamamlanması 30 dakika kadar sürebilir.

## <a name="download-vpn-profile"></a><a name="download"></a>VPN profili indirme

İstemcilerinizi yapılandırmak için VPN profilini kullanın.

1. Sanal WAN 'nizin sayfasında, **Kullanıcı VPN yapılandırması**' na tıklayın.
2. Sayfanın üst kısmında, **Kullanıcı VPN yapılandırması 'Nı indir**' e tıklayın.
3. Dosya oluşturulduktan sonra bağlantıya tıklayarak indirebilirsiniz.
4. VPN istemcilerini yapılandırmak için profil dosyasını kullanın.

### <a name="configure-user-vpn-clients"></a>Kullanıcı VPN istemcilerini yapılandırma
Uzak erişim istemcilerini yapılandırmak için indirilen profili kullanın. Her işletim sisteminin yordamı farklıdır. Lütfen aşağıdaki doğru talimatları izleyin:

#### <a name="microsoft-windows"></a>Microsoft Windows
##### <a name="openvpn"></a>OpenVPN

1. Resmi web sitesinden OpenVPN istemcisini indirip yükleyin.
2. Ağ geçidinin VPN profilini indirin. Bu, Azure portal veya PowerShell 'de New-AzureRmVpnClientConfiguration içindeki kullanıcı VPN yapılandırmaları sekmesinden yapılabilir.
3. Profilin sıkıştırmasını açın. OpenVPN klasöründeki vpnconfig.ovpn yapılandırma dosyasını not defterinde açın.
4. P2S istemci sertifikası bölümünü base64’teki P2S istemci sertifikası genel anahtarı ile doldurun. PEM biçimli bir sertifikada .cer dosyasını açıp base64 anahtarını sertifika üst bilgileri arasına kopyalamanız yeterlidir. Adımlar için bkz [. kodlanmış ortak anahtarı almak için sertifikayı dışarı aktarma.](certificates-point-to-site.md)
5. Özel anahtar bölümünü, base64’teki P2S istemci sertifikası özel anahtarı ile doldurun. Adımlar için bkz [. özel anahtar ayıklama.](howto-openvpn-clients.md#windows).
6. Başka bir alanı değiştirmeyin. VPN’e bağlanmak için istemci girişinde doldurulmuş yapılandırmayı kullanın.
7. vpnconfig.ovpn dosyasını C:\Program Files\OpenVPN\config klasörüne kopyalayın.
8. Sistem tepsisindeki OpenVPN simgesine sağ tıklayın ve Bağlan’a tıklayın.

##### <a name="ikev2"></a>IKEv2

1. Windows bilgisayarın mimarisine karşılık gelen VPN istemcisi yapılandırma dosyalarını seçin. 64 bit işlemci mimarisi için 'VpnClientSetupAmd64' yükleyici paketini seçin. 32 bit işlemci mimarisi için 'VpnClientSetupX86' yükleyici paketini seçin.
2. Yüklemek için pakete çift tıklayın. Bir SmartScreen açılır penceresi görürseniz Daha fazla bilgi’ye ve ardından Yine de çalıştır’a tıklayın.
3. İstemci bilgisayarda Ağ Ayarları’na gidin ve VPN öğesine tıklayın. VPN bağlantısı, bağlandığı sanal ağın adını gösterir.
4. Bağlanmayı denemeden önce, istemci bilgisayara bir istemci sertifikası yüklediğinizi doğrulayın. Yerel Azure sertifika kimlik doğrulaması türü kullanılırken kimlik doğrulaması için bir istemci sertifikası gereklidir. Sertifika oluşturma hakkında daha fazla bilgi için bkz. [sertifika oluşturma](certificates-point-to-site.md). İstemci sertifikasını nasıl yükleyeceğiniz hakkında bilgi için bkz. [istemci sertifikası yüklemesi](../vpn-gateway/point-to-site-how-to-vpn-client-install-azure-cert.md).

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
