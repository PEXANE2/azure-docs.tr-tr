---
title: "P2S VPN & sertifikası kimlik doğrulaması kullanarak VNet 'e bağlanma: Portal"
titleSuffix: Azure VPN Gateway
description: P2S ve otomatik olarak imzalanan veya CA tarafından verilen sertifikaları kullanarak Windows, macOS ve Linux istemcilerini Azure sanal ağına güvenli bir şekilde bağlayın. Bu makalede Azure portalı kullanılmaktadır.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: how-to
ms.date: 11/09/2020
ms.author: cherylmc
ms.openlocfilehash: 0b2fa06bc04bdb584367312b1e89939ed386b4f2
ms.sourcegitcommit: cd9754373576d6767c06baccfd500ae88ea733e4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2020
ms.locfileid: "94952857"
---
# <a name="configure-a-point-to-site-vpn-connection-to-a-vnet-using-native-azure-certificate-authentication-azure-portal"></a>Yerel Azure sertifika kimlik doğrulaması kullanarak bir VNet 'e Noktadan siteye VPN bağlantısı yapılandırma: Azure portal

Bu makale Windows, Linux veya macOS çalıştıran ayrı istemcileri bir Azure VNet 'e güvenli bir şekilde bağlamanıza yardımcı olur. Noktadan Siteye VPN bağlantıları, ev veya bir konferans gibi uzak bir noktadan Vnet'inize bağlanmak istediğinizde faydalıdır. Bir sanal ağa bağlanması gereken yalnızca birkaç istemciniz olduğunda Siteden Siteye VPN yerine P2S’yi de kullanabilirsiniz. Noktadan Siteye bağlantılar için bir VPN cihazına veya genel kullanıma yönelik bir IP adresine gerek yoktur. P2S, VPN bağlantısını SSTP (Güvenli Yuva Tünel Protokolü) veya IKEv2 üzerinden oluşturur. Noktadan Siteye VPN hakkında daha fazla bilgi edinmek için bkz. [Noktadan Siteye VPN hakkında](point-to-site-about.md).

:::image type="content" source="./media\vpn-gateway-howto-point-to-site-resource-manager-portal/point-to-site-diagram.png" alt-text="Bir bilgisayardan Azure VNet 'e bağlanma-Noktadan siteye bağlantı diyagramı":::

Noktadan siteye VPN hakkında daha fazla bilgi için bkz. [noktadan sıteye VPN hakkında](point-to-site-about.md). Azure PowerShell kullanarak bu yapılandırmayı oluşturmak için, bkz. [Azure PowerShell kullanarak noktadan sıteye VPN yapılandırma](vpn-gateway-howto-point-to-site-rm-ps.md).

[!INCLUDE [P2S basic architecture](../../includes/vpn-gateway-p2s-architecture.md)]

## <a name="prerequisites"></a>Ön koşullar

Azure aboneliğiniz olduğunu doğrulayın. Henüz Azure aboneliğiniz yoksa [MSDN abonelik avantajlarınızı](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details) etkinleştirebilir veya [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial) için kaydolabilirsiniz.

### <a name="example-values"></a><a name="example"></a>Örnek değerler

Aşağıdaki değerleri kullanarak bir test ortamı oluşturabilir veya bu makaledeki örnekleri daha iyi anlamak için bu değerlere bakabilirsiniz:

* **VNet Adı:** VNet1
* **Adres alanı:** 10.1.0.0/16<br>Bu örnekte yalnızca bir adres alanı kullanılmaktadır. Sanal ağınıza ait birden fazla adres alanı olabilir.
* **Alt ağ adı:** Uçta
* **Alt ağ adres aralığı:** 10.1.0.0/24
* **Abonelik:** Birden fazla aboneliğiniz varsa doğru aboneliği kullandığınızdan emin olun.
* **Kaynak Grubu:** TestRG1
* **Konum:** Doğu ABD
* **Gatewaysubnet:** 10.1.255.0/27<br>
* **Sanal ağ geçidi adı:** VNet1GW
* **Ağ geçidi türü:** VPN
* **VPN türü:** Rota tabanlı
* **Genel IP adresi adı:** VNet1GWpip
* **Bağlantı türü:** Noktadan siteye
* **İstemci adres havuzu:** 172.16.201.0/24<br>Sanal ağa bu Noktadan Siteye bağlantıyı kullanarak bağlanan VPN istemcileri, istemci adres havuzdan bir IP adresi alır.

## <a name="1-create-a-virtual-network"></a><a name="createvnet"></a>1. sanal ağ oluşturma

Başlamadan önce, bir Azure aboneliğiniz olduğunu doğrulayın. Henüz Azure aboneliğiniz yoksa [MSDN abonelik avantajlarınızı](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details) etkinleştirebilir veya [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial) için kaydolabilirsiniz.

[!INCLUDE [About cross-premises addresses](../../includes/vpn-gateway-cross-premises.md)]

[!INCLUDE [Basic Point-to-Site VNet](../../includes/vpn-gateway-basic-vnet-rm-portal-include.md)]

## <a name="2-create-a-virtual-network-gateway"></a><a name="creategw"></a>2. sanal ağ geçidi oluşturma

Bu adımda sanal ağınız için sanal ağ geçidi oluşturacaksınız. Bir ağ geçidinin oluşturulması, seçili ağ geçidi SKU’suna bağlı olarak 45 dakika veya daha uzun sürebilir.

>[!NOTE]
>Temel ağ geçidi SKU 'SU Ikev2 veya RADIUS kimlik doğrulamasını desteklemez. Mac istemcilerinin sanal ağınıza bağlanmasını planlıyorsanız, temel SKU 'YU kullanmayın.
>

[!INCLUDE [About gateway subnets](../../includes/vpn-gateway-about-gwsubnet-portal-include.md)]

[!INCLUDE [Create a gateway](../../includes/vpn-gateway-add-gw-rm-portal-include.md)]

## <a name="3-generate-certificates"></a><a name="generatecert"></a>3. sertifika oluşturma

Sertifikalar, Noktadan Noktaya VPN bağlantısı üzerinden VNet’e bağlanan istemcilerin kimliğini doğrulamak için Azure tarafından kullanılır. Kök sertifika edindiğinizde, ortak anahtar bilgilerini Azure'a [yüklersiniz](#uploadfile). Bunun üzerine, kök sertifika P2S üzerinden sanal ağa bağlantı için Azure tarafından 'güvenilir' olarak görülür. Ayrıca, güvenilen kök sertifikadan istemci sertifikaları da oluşturur ve bunları her bir istemci bilgisayara yüklersiniz. İstemci sertifikası, sanal ağ ile bağlantı başlattığında istemcinin kimliğini doğrulamak için kullanılır. 

### <a name="generate-a-root-certificate"></a><a name="getcer"></a>Kök sertifikası oluşturma

[!INCLUDE [root-certificate](../../includes/vpn-gateway-p2s-rootcert-include.md)]

### <a name="generate-client-certificates"></a><a name="generateclientcert"></a>İstemci sertifikaları oluştur

[!INCLUDE [generate-client-cert](../../includes/vpn-gateway-p2s-clientcert-include.md)]

## <a name="4-add-the-client-address-pool"></a><a name="addresspool"></a>4. istemci adres havuzunu ekleme

İstemci adres havuzu, belirttiğiniz özel IP adresleri aralığıdır. Noktadan Siteye VPN üzerinden bağlanan istemciler dinamik olarak bu aralıktaki bir IP adresini alır. Bağlantı kurduğunuz şirket içi konum veya bağlanmak istediğiniz sanal ağ ile çakışmayan özel bir IP adresi aralığı kullanın. Birden çok protokol yapılandırırsanız ve SSTP protokollerden biri ise, yapılandırılmış adres havuzu yapılandırılan protokoller arasında eşit olarak bölünür.

1. Sanal ağ geçidi oluşturulduktan sonra sanal ağ geçidi sayfasının **Ayarlar** bölümüne gidin. **Ayarlar**' da, **Noktadan siteye yapılandırma**' yı seçin. Yapılandırma sayfasını açmak için **Şimdi Yapılandır** ' ı seçin.

   :::image type="content" source="./media/vpn-gateway-howto-point-to-site-resource-manager-portal/configure-now.png" alt-text="Noktadan siteye yapılandırma sayfası" lightbox="./media/vpn-gateway-howto-point-to-site-resource-manager-portal/configure-now.png":::
1. **Noktadan siteye yapılandırma** sayfasında, çeşitli ayarları yapılandırabilirsiniz. Bu sayfada tünel türü veya kimlik doğrulama türü görmüyorsanız, ağ geçidiniz temel SKU 'YU kullanıyor demektir. Temel SKU, IKEv2 veya RADIUS kimlik doğrulamasını desteklemez. Bu ayarları kullanmak istiyorsanız, farklı bir ağ geçidi SKU 'SU kullanarak ağ geçidini silip yeniden oluşturmanız gerekir.

   :::image type="content" source="./media/vpn-gateway-howto-point-to-site-resource-manager-portal/address-pool.png" alt-text="Adres havuzunu belirtin" lightbox="./media/vpn-gateway-howto-point-to-site-resource-manager-portal/address-pool.png":::
1. **Adres havuzu** kutusuna, kullanmak ISTEDIĞINIZ özel IP adresi aralığını ekleyin. VPN istemcileri, belirttiğiniz aralıktan dinamik olarak bir IP adresi alır. Etkin/etkin yapılandırma için en düşük alt ağ maskesi, etkin/Pasif ve 28 bit için 29 bittir.
1. Tünel türünü yapılandırmak için sonraki bölüme gidin.

## <a name="5-configure-tunnel-type"></a><a name="tunneltype"></a>5. tünel türünü yapılandırın

Tünel türünü seçin. Tünel seçenekleri OpenVPN, SSTP ve IKEv2 ' dir.

* Android ve Linux üzerindeki strongSwan istemcisi ile iOS ve OSX üzerindeki yerel IKEv2 VPN istemcisi, bağlanmak için yalnızca IKEv2 tünelini kullanır.
* Windows istemcileri önce Ikev2 'yi dener ve bağlanmazsa SSTP 'ye geri döner.
* OpenVPN istemcisini, OpenVPN tünel türüne bağlanmak için kullanabilirsiniz.

:::image type="content" source="./media/vpn-gateway-howto-point-to-site-resource-manager-portal/tunnel-ike.png" alt-text="Tünel türü":::

## <a name="6-configure-authentication-type"></a><a name="authenticationtype"></a>6. kimlik doğrulama türünü yapılandırın

**Kimlik doğrulama türü** için **Azure sertifikası**' nı seçin.

:::image type="content" source="./media/vpn-gateway-howto-point-to-site-resource-manager-portal/azure-certificate.png" alt-text="Kimlik doğrulaması türü":::

## <a name="7-upload-the-root-certificate-public-certificate-data"></a><a name="uploadfile"></a>7. kök sertifika genel sertifika verilerini karşıya yükleyin

Toplam 20 adede kadar güvenilir kök sertifikayı karşıya yükleyebilirsiniz. Ortak sertifika verileri karşıya yüklendikten sonra Azure, güvenilir kök sertifikadan oluşturulmuş bir istemci sertifikasının yüklü olduğu istemcilerin kimliklerini doğrulamak için bu dosyayı kullanabilir. Kök sertifikanın ortak anahtar bilgilerini Azure'a yükleyin.

1. Sertifikalar **Kök sertifikası** bölümünün **Noktadan siteye yapılandırma** sayfasına eklenir.
1. Kök sertifikayı Base-64 ile kodlanmış X.509 (.cer) dosyası olarak dışarı aktardığınızdan emin olun. Sertifikayı metin düzenleyiciyle açabilmek için bu biçimde dışa aktarmanız gerekir.
1. Sertifikayı Not Defteri gibi bir metin düzenleyiciyle açın. Sertifika verilerini kopyalarken, metni satır başları ya da satır besleme karakterleri içermeyen tek ve kesintisiz bir satır şeklinde kopyaladığınızdan emin olun. Satır başlarını ve satır besleme karakterlerini görmek için metin düzenleyicisindeki görünümünüzü 'Sembolü Göster/Tüm karakterleri göster' olarak değiştirmeniz gerekebilir. Yalnızca aşağıdaki bölümü kesintisiz bir satır olarak kopyalayın:

   :::image type="content" source="./media/vpn-gateway-howto-point-to-site-resource-manager-portal/notepadroot.png" alt-text="Sertifika verileri" border="false":::
1. Sertifika verilerini **Ortak Sertifika Verileri** alanına yapıştırın. Sertifikayı **adlandırın** ve ardından **Kaydet**' i seçin. En fazla 20 güvenilen kök sertifika ekleyebilirsiniz.

   :::image type="content" source="./media/vpn-gateway-howto-point-to-site-resource-manager-portal/uploaded.png" alt-text="Sertifika verilerini yapıştırma" border="false":::
1. Tüm yapılandırma ayarlarını kaydetmek için sayfanın en üstünde **Kaydet** ' i seçin.

   :::image type="content" source="./media/vpn-gateway-howto-point-to-site-resource-manager-portal/save.png" alt-text="Yapılandırmayı Kaydet" border="false":::

## <a name="8-install-an-exported-client-certificate"></a><a name="installclientcert"></a>8. bir içe aktarılmış istemci sertifikası yükler

İstemci sertifikalarını oluşturmak için kullandığınız bilgisayardan farklı bir istemci bilgisayarda bir P2S bağlantı oluşturmak istiyorsanız, bir istemci sertifikası yüklemeniz gerekir. Bir istemci sertifikası yüklenirken, istemci sertifikası dışarı aktarılırken oluşturulan parola gerekir.

İstemci sertifikasının tüm sertifika zinciriyle birlikte .pfx dosyası olarak (varsayılan seçenek) dışarı aktarılmadığından emin olun. Aksi takdirde kök sertifika bilgileri istemci bilgisayarda bulunmaz ve istemci, kimliğini düzgün bir biçimde doğrulayamaz.

Yükleme adımları için bkz. [İstemci sertifikası yükleme](point-to-site-how-to-vpn-client-install-azure-cert.md).

## <a name="9-generate-and-install-the-vpn-client-configuration-package"></a><a name="clientconfig"></a>9. VPN istemcisi yapılandırma paketini oluşturma ve yüklemeyi

VPN istemcisi yapılandırma dosyaları, P2S bağlantısı üzerinden bir sanal ağa bağlanmak üzere cihazları yapılandırmak için gereken ayarları içerir. VPN istemcisi yapılandırma dosyalarını oluşturma ve yükleme yönergeleri için bkz. [Yerel Azure sertifika doğrulaması P2S yapılandırmaları için VPN istemci yapılandırma dosyalarını oluşturma ve yükleme](point-to-site-vpn-client-configuration-azure-cert.md).

## <a name="10-connect-to-azure"></a><a name="connect"></a>10. Azure 'a bağlanma

### <a name="to-connect-from-a-windows-vpn-client"></a>Windows VPN istemcisinden bağlanmak için

[!INCLUDE [Connect from a Windows client](../../includes/vpn-gateway-p2s-connect-windows-client.md)]

[!INCLUDE [verifies client certificates](../../includes/vpn-gateway-certificates-verify-client-cert-include.md)]

### <a name="to-connect-from-a-mac-vpn-client"></a>Mac VPN istemcisinden bağlanmak için

Ağ iletişim kutusunda, kullanmak istediğiniz istemci profilini bulun, [VpnSettings.xml](point-to-site-vpn-client-configuration-azure-cert.md#installmac)ayarları belirtin ve sonra **Bağlan**' ı seçin.

Ayrıntılı yönergeler için [Install-Mac (OS X)](./point-to-site-vpn-client-configuration-azure-cert.md#installmac) ' i işaretleyin. Bağlanmayla ilgili sorun yaşıyorsanız, sanal ağ geçidinin temel bir SKU kullanmadığından emin olun. Temel SKU, Mac istemcileri için desteklenmez.

:::image type="content" source="./media/vpn-gateway-howto-point-to-site-rm-ps/applyconnect.png" alt-text="Mac VPN istemci bağlantısı" border="false":::

## <a name="to-verify-your-connection"></a><a name="verify"></a>Bağlantınızı doğrulamak için

Bu yönergeler, Windows istemcileri için geçerlidir.

1. VPN bağlantınızın etkin olduğunu doğrulamak için, yükseltilmiş bir komut istemi açın ve *ipconfig/all* komutunu çalıştırın.
2. Sonuçlara bakın. Aldığınız IP adresinin, yapılandırmanızda belirttiğiniz Noktadan Siteye VPN İstemcisi Adres Havuzu'ndaki adreslerden biri olduğuna dikkat edin. Sonuçları şu örneğe benzer:

   ```
   PPP adapter VNet1:
      Connection-specific DNS Suffix .:
      Description.....................: VNet1
      Physical Address................:
      DHCP Enabled....................: No
      Autoconfiguration Enabled.......: Yes
      IPv4 Address....................: 172.16.201.3(Preferred)
      Subnet Mask.....................: 255.255.255.255
      Default Gateway.................:
      NetBIOS over Tcpip..............: Enabled
   ```

## <a name="to-connect-to-a-virtual-machine"></a><a name="connectVM"></a>Sanal makineye bağlanma

Bu yönergeler, Windows istemcileri için geçerlidir.

[!INCLUDE [Connect to a VM](../../includes/vpn-gateway-connect-vm.md)]

* Sanal ağ için DNS sunucusu IP adresleri belirtildikten sonra VPN istemci yapılandırma paketinin oluşturulduğunu doğrulayın. DNS sunucusu IP adreslerini güncelleştirdiyseniz, yeni bir VPN istemci yapılandırma paketi oluşturup yükleyin.

* 'ipconfig' seçeneğini kullanarak bağlantıyı kurduğunuz bilgisayardaki Ethernet bağdaştırıcısına atanmış IPv4 adresini denetleyin. IP adresi bağlanacağınız sanal ağın adres aralığında veya VPNClientAddressPool adres aralığında ise, bu durum çakışan bir adres alanı olarak adlandırılır. Adres alanınız bu şekilde çakıştığında, ağ trafiği Azure’a ulaşmaz ve yerel ağda kalır.

## <a name="to-add-or-remove-trusted-root-certificates"></a><a name="add"></a>Güvenilen kök sertifika ekleme veya kaldırma

Azure’da güvenilen kök sertifikayı ekleyebilir veya kaldırabilirsiniz. Bir kök sertifikayı kaldırdığınızda, o kökten oluşturulmuş bir sertifikaya sahip istemciler kimlik doğrulaması yapamaz ve bu nedenle bağlantı kuramaz. Bir istemcinin kimlik doğrulaması yapmasını ve bağlanmasını istiyorsanız, Azure’da güvenilen (karşıya yüklenmiş) bir kök sertifikadan oluşturulmuş yeni bir istemci sertifikası yüklemeniz gerekir.

### <a name="to-add-a-trusted-root-certificate"></a>Güvenilen kök sertifika ekleme

Azure'a en fazla 20 güvenilen kök sertifika .cer dosyası ekleyebilirsiniz. Yönergeler için, bu makaledeki [Güvenilen bir kök sertifikayı karşıya yükleme](#uploadfile) bölümüne bakın.

### <a name="to-remove-a-trusted-root-certificate"></a>Güvenilen kök sertifikayı kaldırmak için

1. Güvenilen kök sertifikayı kaldırmak için, sanal ağ geçidinizin **Noktadan siteye yapılandırma** sayfasına gidin.
1. Sayfanın **Kök sertifika** bölümünde, kaldırmak istediğiniz sertifikayı bulun.
1. Sertifikanın yanındaki üç noktayı seçin ve ardından ' Kaldır ' seçeneğini belirleyin.

## <a name="to-revoke-a-client-certificate"></a><a name="revokeclient"></a>İstemci sertifikasını iptal etmek için

İstemci sertifikalarını iptal edebilirsiniz. Sertifika iptal listesi sayesinde, ayrı istemci sertifikalarına göre Noktadan Siteye bağlantıyı seçmeli olarak reddedebilirsiniz. Bu, güvenilen kök sertifika kaldırma işleminden farklıdır. Azure’dan güvenilen kök sertifika .cer dosyasını kaldırırsanız iptal edilen kök sertifika tarafından oluşturulan/imzalanan tüm istemci sertifikaları reddedilir. Kök sertifika yerine istemci sertifikasını iptal etmek, kök sertifikadan oluşturulan diğer sertifikaların kimlik doğrulaması amacıyla kullanılmaya devam edilmesine olanak sağlar.

Genellikle ekip ve kuruluş düzeylerinde erişimi yönetmek için kök sertifika kullanılırken ayrı kullanıcılar üzerinde ayrıntılı erişim denetimi için iptal edilen istemci sertifikaları kullanılır.

### <a name="revoke-a-client-certificate"></a>İstemci sertifikasını iptal etme

Parmak izini iptal listesine ekleyerek bir istemci sertifikasını iptal edebilirsiniz.

1. İstemci sertifikasının parmak izini alın. Daha fazla bilgi için bkz. [bir sertifikanın parmak izini alma](/dotnet/framework/wcf/feature-details/how-to-retrieve-the-thumbprint-of-a-certificate).
1. Bilgileri bir metin düzenleyicisine kopyalayın ve sürekli bir dize haline getirmek için tüm boşlukları kaldırın.
1. Sanal ağ geçidi **Noktadan siteye yapılandırma** sayfasına gidin. Bu, [güvenilen kök sertifika yüklemek](#uploadfile) için kullandığınız sayfanın aynısıdır.
1. **İptal edilen sertifikalar** bölümünde, sertifika için bir kolay ad girin (sertifika genel adından farklı olabilir).
1. Parmak izi dizesini kopyalayın ve **Parmak izi** alanına yapıştırın.
1. Parmak izi doğrulanır ve otomatik olarak iptal listesine eklenir. Ekranda listenin güncelleştirildiğini belirten bir ileti görünür. 
1. Güncelleştirme tamamlandıktan sonra sertifika artık bağlanmak için kullanılamaz. Bu sertifikayı kullanarak bağlanmaya çalışan istemciler sertifikanın artık geçerli olmadığını belirten bir ileti alır.

## <a name="point-to-site-faq"></a><a name="faq"></a>Noktadan siteye hakkında SSS

[!INCLUDE [Point-to-Site FAQ](../../includes/vpn-gateway-faq-p2s-azurecert-include.md)]

## <a name="next-steps"></a>Sonraki adımlar
Bağlantınız tamamlandıktan sonra sanal ağlarınıza sanal makineler ekleyebilirsiniz. Daha fazla bilgi için bkz. [Sanal Makineler](../index.yml). Ağ ve sanal makineler hakkında daha fazla bilgi edinmek için, bkz. [Azure ve Linux VM ağına genel bakış](../virtual-machines/network-overview.md).

P2S sorun giderme bilgileri için [Azure noktadan siteye bağlantıları sorununu giderme](vpn-gateway-troubleshoot-vpn-point-to-site-connection-problems.md).
