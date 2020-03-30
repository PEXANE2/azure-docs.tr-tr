---
title: 'Noktadan Siteye bağlantısı ve istemci kimlik doğrulaması kullanarak bir bilgisayarı sanal ağa bağlama: Klasik Azure portalı | Microsoft Docs'
description: Azure portalını kullanarak klasik bir Noktadan siteye VPN ağ geçidi bağlantısı oluşturun.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 01/09/2020
ms.author: cherylmc
ms.openlocfilehash: 5d80cb2f2ed844126d1e9311151e6c53fcb11840
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79244582"
---
# <a name="configure-a-point-to-site-connection-by-using-certificate-authentication-classic"></a>Sertifika kimlik doğrulaması (klasik) kullanarak Bir Noktadan Siteye bağlantıyı yapılandırma

[!INCLUDE [deployment models](../../includes/vpn-gateway-classic-deployment-model-include.md)]

Bu makalede, Bir Noktaya Sayfa bağlantısı ile bir VNet oluşturmak için nasıl gösterir. Bu Vnet'i, Azure portalını kullanarak klasik dağıtım modeliyle oluşturursunuz. Bu yapılandırma, bağlanan istemcinin kimliğini doğrulamak için otomatik olarak imzalanan veya CA tarafından verilen sertifikaları kullanır. Aşağıdaki makalelerde açıklanan seçenekleri kullanarak bu yapılandırmayı farklı bir dağıtım aracı veya modeliyle de oluşturabilirsiniz:

> [!div class="op_single_selector"]
> * [Azure portalında](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
> * [Powershell](vpn-gateway-howto-point-to-site-rm-ps.md)
> * [Azure portal (klasik)](vpn-gateway-howto-point-to-site-classic-azure-portal.md)
>

Tek bir istemci bilgisayardan sanal ağınıza güvenli bir bağlantı oluşturmak için Bir Noktaya Sayfa (P2S) VPN ağ geçidi kullanırsınız. VNet'inize uzak bir konumdan bağlanmak istediğinizde, Site'ye noktalı VPN bağlantıları kullanışlıdır. Bir VNet'e bağlanması gereken yalnızca birkaç istemciniz olduğunda, P2S VPN Siteden Siteye VPN yerine kullanmak için yararlı bir çözümdür. P2S VPN bağlantısı, istemci bilgisayardan başlatılmasıyla oluşturulur.

> [!IMPORTANT]
> Klasik dağıtım modeli yalnızca Windows VPN istemcilerini destekler ve SSL tabanlı VPN protokolü olan Güvenli Yuva Tünel Protokolünü (SSTP) kullanır. Windows VPN olmayan istemcileri desteklemek için, Kaynak Yöneticisi dağıtım modeliyle VNet'inizi oluşturmanız gerekir. Resource Manager dağıtım modeli SSTP’ye ek olarak IKEv2 VPN desteği sunar. Daha fazla bilgi için bkz. [P2S bağlantıları hakkında](point-to-site-about.md).
>
>

![Noktadan Siteye diyagramı](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/point-to-site-connection-diagram.png)

## <a name="prerequisites"></a>Ön koşullar

Noktadan Siteye sertifika kimlik doğrulama bağlantıları aşağıdaki ön koşulları gerektirir:

* Bir Dinamik VPN ağ geçidi.
* Azure’a yüklenmiş bir kök sertifikanın ortak anahtarı (.cer dosyası). Bu anahtar güvenilir bir sertifika olarak kabul edilir ve kimlik doğrulaması için kullanılır.
* Kök sertifikadan oluşturulmuş ve bağlanacak her bir istemci bilgisayara yüklenmiş istemci sertifikası. Bu sertifika, istemci kimlik doğrulaması için kullanılır.
* Bir VPN istemcisi yapılandırma paketi oluşturulmalı ve bağlanan her istemci bilgisayara yüklenmelidir. İstemci yapılandırma paketi, vnet'e bağlanmak için gerekli bilgilerle işletim sisteminde bulunan yerel VPN istemcisini yapılandırır.

Noktadan Siteye bağlantılar, VPN aygıtı veya şirket içinde halka açık bir IP adresi gerektirmez. VPN bağlantısı SSTP (Güvenli Yuva Tünel Protokolü) üzerinden oluşturulur. Sunucu tarafında 1.0, 1.1 ve 1.2 SSTP sürümlerini destekliyoruz. Kullanılacak sürüm, istemci tarafından belirlenir. Windows 8.1 ve sonraki sürümlerinde, SSTP'de varsayılan olarak 1.2 kullanılır. 

Yer-To-Site bağlantıları hakkında daha fazla bilgi için [bkz.](#point-to-site-faq)

### <a name="example-settings"></a>Örnek ayarlar

Bir test ortamı oluşturmak için aşağıdaki değerleri kullanın veya bu makaledeki örnekleri daha iyi anlamak için bu değerlere bakın:

- **Sanal ağ (klasik) ayarları oluşturma**
   - **Adı**: *VNet1*girin .
   - **Adres alanı**: *192.168.0.0/16*girin. Bu örnekte yalnızca bir adres alanı kullanılmaktadır. Diyagramda gösterildiği gibi, sanal ağınıza ait birden fazla adres alanı olabilir.
   - **Alt ağ adı**: *FrontEnd*girin .
   - **Alt adres aralığı**: *192.168.1.0/24*girin.
   - **Abonelik**: Kullanılabilir abonelikler listesinden bir abonelik seçin.
   - **Kaynak grubu**: *TestRG*girin. Kaynak grubu yoksa **yeni oluştur'u**seçin.
   - **Yer**: Listeden **Doğu ABD'yi** seçin.

  - **VPN bağlantı ayarları**
    - **Bağlantı türü**: **Noktadan siteye**seçin.
    - **Müşteri Adresi Alanı**: *172.16.201.0/24*girin. Bu Noktadan Siteye bağlantıyı kullanarak VNet'e bağlanan VPN istemcileri, belirtilen havuzdan bir IP adresi alır.

- **Ağ geçidi yapılandırma alt ağ ayarları**
   - **Adı**: *GatewaySubnet*ile otomatik doldurulmuş .
   - **Adres aralığı**: *192.168.200.0/24*girin. 

- **Ağ geçidi yapılandırma ayarları:**
   - **Boyut**: Kullanmak istediğiniz ağ geçidi SKU'yu seçin.
   - **Yönlendirme Türü**: **Dinamik'i**seçin.

## <a name="create-a-virtual-network-and-a-vpn-gateway"></a>Sanal ağ ve VPN ağ geçidi oluşturma

Başlamadan önce, azure aboneliğiniz olduğunu doğrulayın. Henüz Azure aboneliğiniz yoksa [MSDN abonelik avantajlarınızı](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details) etkinleştirebilir veya [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial) için kaydolabilirsiniz.

### <a name="part-1-create-a-virtual-network"></a>1. Kısım: Sanal ağ oluşturma

Zaten sanal bir ağınız (VNet) yoksa, bir tane oluşturun. Ekran görüntüleri örnek olarak verilmiştir. Değerlerin kendinizinkilerle değiştirildiğinden emin olun. Azure portalını kullanarak sanal ağ oluşturmak için şu adımları uygulayın:

1. Azure [portalı](https://portal.azure.com) menüsünde veya **Ana** sayfadan **kaynak oluştur'u**seçin. **Yeni** sayfa açılır.

2. Pazar **yeri arama** alanında, *sanal ağa* girin ve döndürülen listeden Sanal **ağı** seçin. **Sanal ağ** sayfası açılır.

3. Dağıtım **modeli** listesini seç'ten **Klasik'i**seçin ve ardından **Oluştur'u**seçin. **Sanal ağ oluştur** sayfası açılır.

4. **Sanal ağ oluştur** sayfasında sanal ağ ayarlarını yapılandırın. Bu sayfada, ilk adres alanınızı ve tek alt ağ adres aralığınızı eklersiniz. VNet oluşturma işlemini tamamladıktan sonra geri dönüp ek alt ağları ve adres alanlarını ekleyin.

   ![Sanal ağ oluştur sayfası](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/vnet125.png)

5. Açılan listeden kullanmak istediğiniz **Aboneliği** seçin.

6. Varolan bir **Kaynak Grubu**seçin. Veya **yeni oluştur'u** seçip bir ad girerek yeni bir kaynak grubu oluşturun. Yeni bir kaynak grubu oluşturuyorsanız, kaynak grubunu planladığınız yapılandırma değerlerine göre adlandırın. Kaynak grupları hakkında daha fazla bilgi için bkz. [Azure Resource Manager'a genel bakış](../azure-resource-manager/management/overview.md#resource-groups).

7. VNet'iniz için bir **Konum** seçin. Bu ayar, bu VNet'e dağıttığınız kaynakların coğrafi konumunu belirler.

8. VNet'i oluşturmak için **Oluştur'u** seçin. **Bildirimler** sayfasından, devam eden bir Dağıtım **iletisi** görürsünüz.

8. Sanal ağınız oluşturulduktan sonra, **Bildirimler** sayfasındaki ileti **Dağıtım'a**dönüşür. VNet'inizi panoda kolayca bulmak istiyorsanız **panoya Pin'i** seçin. 

10. DNS sunucusu ekleme (isteğe bağlı). Sanal ağınızı oluşturduktan sonra ad çözümlemesi için bir DNS sunucusunun IP adresini ekleyebilirsiniz. Belirttiğiniz DNS sunucusu IP adresi, sanal ağınızdaki kaynakların adlarını çözümleyebilen bir DNS sunucusunun adresi olmalıdır.

    Bir DNS sunucusu eklemek için VNet sayfanızdan **DNS sunucularını** seçin. Ardından, kullanmak istediğiniz DNS sunucusunun IP adresini girin ve **Kaydet'i**seçin.

### <a name="part-2-create-a-gateway-subnet-and-a-dynamic-routing-gateway"></a>Bölüm 2: Ağ geçidi alt ağı ve dinamik bir yönlendirme ağ geçidi oluşturma

Bu adımda, bir ağ geçidi alt ağı ve dinamik bir yönlendirme ağ geçidi oluşturursunuz. Klasik dağıtım modeli için Azure portalında, aynı yapılandırma sayfaları aracılığıyla ağ geçidi alt ağını ve ağ geçidini oluşturursunuz. Yalnızca ağ geçidi hizmetleri için ağ geçidi alt netini kullanın. Ağ geçidi alt ağına hiçbir şeyi (VM’ler veya diğer hizmetler) doğrudan dağıtmayın.

1. Azure portalında, ağ geçidi oluşturmak istediğiniz sanal ağa gidin.

2. Sanal ağınızın sayfasında Genel **Bakış'ı**seçin ve **VPN bağlantıları** bölümünde **Ağ Geçidi'ni**seçin.

   ![Ağ geçidi oluşturmak için seçin](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/beforegw125.png)
3. **Yeni VPN Bağlantısı** sayfasında **Noktadan siteye** öğesini seçin.

   ![Noktadan Siteye bağlantı türü](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/newvpnconnect.png)
4. **İstemci Adres Alanı**için, VPN istemcilerinin bağlanırken IP adresi aldığı IP adresi aralığını ekleyin. Bağlantı kurduğunuz şirket içi konumla veya bağlandığınız VNet ile örtüşmeyen özel bir IP adresi aralığı kullanın. Kullanmak istediğiniz özel IP adresi aralığıyla otomatik doldurulmuş aralığın üzerine yazabilirsiniz. Bu örnek, otomatik doldurulmuş aralığı gösterir. 

   ![İstemci adres alanı](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/clientaddress.png)
5. **Hemen ağ geçidi oluştur'u**seçin ve ardından Ağ Geçidi **yapılandırma** sayfasını açmak için **İsteğe Bağlı ağ geçidi yapılandırmasını** seçin.

   ![İsteğe bağlı ağ geçidi yapılandırması seçin](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/optsubnet125.png)

6. Ağ **Geçidi yapılandırma** sayfasından ağ geçidi alt netini eklemek için **Alt ağ'ı** seçin. /29 gibi küçük bir ağ geçidi alt ağı oluşturmak mümkündür. Ancak, en az /28 veya /27 seçerek daha fazla adres içeren daha büyük bir alt ağ oluşturmanızı öneririz. Bunu yapmak, gelecekte isteyebileceğiniz olası ek yapılandırmaları barındıracak yeterli adrese olanak sağlar. Ağ geçidi alt ağlarıyla çalışırken, ağ güvenlik grubunu (NSG) ağ geçidi alt ağıyla ilişkilendirmekten kaçının. Bir ağ güvenlik grubunu bu alt ağa ilişkilendirmek VPN ağ geçidinizin beklendiği gibi çalışmamasını sağlayabilir. Bu ayarı kaydetmek için **Tamam'ı** seçin.

   ![GatewaySubnet ekleme](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/gwsubnet125.png)
7. Ağ geçidi **Boyutu** seçin. Boyut, sanal ağ geçidinizin ağ geçidi SKU’sudur. Azure portalında varsayılan SKU **Varsayılandır.** Ağ geçidi SK'leri hakkında daha fazla bilgi için [VPN ağ geçidi ayarları hakkında](vpn-gateway-about-vpn-gateway-settings.md#gwsku)bilgi alalım.

   ![Ağ geçidi boyutu](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/gwsize125.png)
8. Ağ geçidiniz için **Yönlendirme Türü** seçin. P2S yapılandırmaları bir **Dinamik** yönlendirme türü gerektirir. Bu sayfayı yapılandırmayı bitirdiğinizde **Tamam'ı** seçin.

   ![Yönlendirme türünü yapılandırma](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/routingtype125.png)

9. Yeni **VPN Bağlantısı** sayfasında, sanal ağ ağ ağ ağınızı oluşturmaya başlamak için sayfanın altındaki **Tamam'ı** seçin. VPN ağ geçidinin tamamlanması, seçtiğiniz ağ geçidi SKU'ya bağlı olarak tamamlanması 45 dakika kadar sürebilir.
 
## <a name="create-certificates"></a><a name="generatecerts"></a>Sertifika oluşturma

Azure, VPN istemcilerini Site'ye Giriş VPN'leri için doğrulamak için sertifikalar kullanır. Kök sertifikanın ortak anahtar bilgilerini Azure'a yükleyin. Ortak anahtar daha sonra güvenilir olarak kabul *edilir.* İstemci sertifikaları güvenilir kök sertifikadan oluşturulmalı ve ardından Sertifikalar-Geçerli Kullanıcı\Kişisel\Sertifikalar sertifika deposunda her istemci bilgisayara yüklenmelidir. Sertifika, VNet'e bağlandığında istemcinin kimliğini doğrulamak için kullanılır. 

Kendi imzalı sertifikalar kullanıyorsanız, bunlar belirli parametreler kullanılarak oluşturulmalıdır. [PowerShell ve Windows 10](vpn-gateway-certificates-point-to-site.md)veya [MakeCert](vpn-gateway-certificates-point-to-site-makecert.md)yönergelerini kullanarak kendi imzalı bir sertifika oluşturabilirsiniz. Kendi imzalı kök sertifikaları kullanırken ve kendi imzalı kök sertifikadan istemci sertifikaları oluştururken bu yönergelerde yer alan adımları izlemeniz önemlidir. Aksi takdirde, oluşturduğunuz sertifikalar P2S bağlantılarıyla uyumlu olmaz ve bir bağlantı hatası alırsınız.

### <a name="acquire-the-public-key-cer-for-the-root-certificate"></a>Kök sertifika için ortak anahtarı (.cer) edinin

[!INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-p2s-rootcert-include.md)]

### <a name="generate-a-client-certificate"></a>İstemci sertifikası oluşturma

[!INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-p2s-clientcert-include.md)]

## <a name="upload-the-root-certificate-cer-file"></a>Kök sertifika .cer dosyasını karşıya yükleme

Ağ geçidi oluşturulduktan sonra, güvenilir bir kök sertifika için .cer dosyasını (ortak anahtar bilgilerini içeren) Azure sunucusuna yükleyin. Kök sertifikanın özel anahtarını yüklemeyin. Sertifikayı yükledikten sonra Azure, sertifikayı güvenilen kök sertifikasından oluşturulan istemci sertifikasını yükleyen istemcilerin kimliğini doğrulamak için kullanır. Daha sonra gerekirse ek güvenilen kök sertifika dosyaları (en fazla 20) yükleyebilirsiniz.  

1. VNet'iniz için sayfanın **VPN bağlantıları** bölümünde, **noktaya sayfa VPN bağlantı** sayfasını açmak için istemciler grafiğini seçin.

   ![İstemciler](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/clients125.png)

2. **Siteden siteye VPN bağlantı** sayfasında, **Sertifikalar** sayfasını açmak için **Sertifikayı Yönet'i** seçin.

   ![Sertifikalar sayfası](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/ptsmanage.png)

1. **Sertifikalar** sayfasında, **Yükle sertifikası** sayfasını açmak için **Yükle'yi** seçin.

    ![Sertifikaları karşıya yükle sayfası](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/uploadcerts.png)

4. .cer dosyasına göz atmak için klasör grafiğini seçin. Dosyayı seçin, ardından **Tamam'ı**seçin. Yüklenen sertifika **Sertifikalar** sayfasında görünür.

   ![Sertifikayı karşıya yükleme](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/upload.png)


## <a name="configure-the-client"></a>İstemciyi yapılandırma

Bir VNet'e Bir Sayfa-To-Site VPN kullanarak bağlanmak için, her istemcinin yerel Windows VPN istemcisini yapılandırmak için bir paket yüklemesi gerekir. Yapılandırma paketi, sanal ağa bağlanmak için gereken ayarlarla yerel Windows VPN istemcisini yapılandırır.

Sürümünün istemci mimarisiyle eşleşmesi şartıyla, her istemci bilgisayarda aynı VPN istemcisi yapılandırma paketini kullanabilirsiniz. Desteklenen istemci işletim sistemleri listesi için, [Site'ye Nokta lı bağlantılar SSS'ye](#point-to-site-faq)bakın.

### <a name="generate-and-install-a-vpn-client-configuration-package"></a>VPN istemcisi yapılandırma paketi oluşturma ve yükleme

1. Azure portalında, VNet'inizin **Genel Bakış** sayfasında, **VPN bağlantılarında,** **noktaya bağlantı** sayfasını açmak için istemci grafiğini seçin.

2. Siteye **vpn bağlantı** sayfasından, yüklendiği istemci işletim sistemine karşılık gelen indirme paketini seçin:

   * 64 bit istemciler için **VPN İstemcisi (64 bit)** seçeneğini belirleyin.
   * 32 bit istemciler için **VPN İstemcisi (32 bit)** seçeneğini belirleyin.

   ![VPN istemcisi yapılandırma paketini indirme](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/dlclient.png)

3. Paket oluşturduktan sonra indirin ve istemci bilgisayarınıza yükleyin. SmartScreen açılır penceresi görürseniz, **Daha Fazla Bilgi'yi**seçin, ardından **yine de Çalıştır'ı**seçin. Paketi ayrıca diğer istemci bilgisayarlara yüklemek üzere kaydedebilirsiniz.

### <a name="install-a-client-certificate"></a>İstemci sertifikası yükleme

İstemci sertifikalarını oluşturmak için kullanılandan farklı bir istemci bilgisayarından P2S bağlantısı oluşturmak için bir istemci sertifikası yükleyin. İstemci sertifikası yüklediğinizde, istemci sertifikası dışa aktarıldığında oluşturulan parolaya ihtiyacınız var. Genellikle, yalnızca çift tıklatarak sertifika yükleyebilirsiniz. Daha fazla bilgi için [dışa aktarılan istemci sertifikasını yükleyin'](vpn-gateway-certificates-point-to-site.md#install)e bakın.


## <a name="connect-to-your-vnet"></a>Sanal ağınıza bağlanma

>[!NOTE]
>Bağlanmakta olduğunuz istemci bilgisayarında Yönetici haklarına sahip olmanız gerekir.
>
>

1. VNet'inize istemci bilgisayarda bağlanmak için Azure portalındaki **VPN bağlantılarına** gidin ve oluşturduğunuz VPN bağlantısını bulun. VPN bağlantısı sanal ağınızla aynı ada sahiptir. **Bağlan**’ı seçin. Sertifikayla ilgili açılır bir ileti görünüyorsa, yükseltilmiş ayrıcalıkları kullanmaya **devam et'i** seçin.

2. **Bağlantı** durumu sayfasında bağlantıyı başlatmak için **Bağlan'ı** seçin. **Sertifika Seç** ekranını görürseniz, görüntülenen istemci sertifikasının doğru onay ladığını doğrulayın. Değilse, açılan listeden doğru sertifikayı seçin ve ardından **Tamam'ı**seçin.

3. Bağlantınız başarılı olursa, **Bağlı** bir bildirim görürsünüz.


### <a name="troubleshooting-p2s-connections"></a>P2S bağlantılarının sorunlarını giderme

[!INCLUDE [verify-client-certificates](../../includes/vpn-gateway-certificates-verify-client-cert-include.md)]

## <a name="verify-the-vpn-connection"></a>VPN bağlantısını doğrulama

1. VPN bağlantınızın etkin olduğunu doğrulayın. İstemci bilgisayarınızda yükseltilmiş bir komut istemi açın ve **ipconfig/all**çalıştırın.
2. Sonuçlara bakın. Aldığınız IP adresinin, sanal ağınızı oluştururken belirlediğiniz Noktadan Siteye bağlantı adres aralığı içerisinden bir adres olduğuna dikkat edin. Sonuçlar şu örneğe benzer olmalıdır:

   ```
    PPP adapter VNet1:
        Connection-specific DNS Suffix .:
        Description.....................: VNet1
        Physical Address................:
        DHCP Enabled....................: No
        Autoconfiguration Enabled.......: Yes
        IPv4 Address....................: 192.168.130.2(Preferred)
        Subnet Mask.....................: 255.255.255.255
        Default Gateway.................:
        NetBIOS over Tcpip..............: Enabled
   ```

## <a name="connect-to-a-virtual-machine"></a>Sanal makineye bağlanma

[!INCLUDE [Connect to a VM](../../includes/vpn-gateway-connect-vm-p2s-classic-include.md)]

## <a name="add-or-remove-trusted-root-certificates"></a>Güvenilen kök sertifika ekleme veya kaldırma

Azure’da güvenilen kök sertifikayı ekleyebilir veya kaldırabilirsiniz. Bir kök sertifikayı kaldırdığınızda, bu kökten oluşturulan bir sertifikaya sahip istemciler artık kimlik doğrulaması yapamaz ve bağlanabilir. Bu istemcilerin kimlik doğrulaması ve yeniden bağlanması için Azure tarafından güvenilen bir kök sertifikadan oluşturulan yeni bir istemci sertifikası yüklemeniz gerekir.

### <a name="to-add-a-trusted-root-certificate"></a>Güvenilen kök sertifika ekleme

Azure'a en fazla 20 güvenilen kök sertifika .cer dosyası ekleyebilirsiniz. Talimatlar için bkz.

### <a name="to-remove-a-trusted-root-certificate"></a>Güvenilen kök sertifikayı kaldırmak için

1. VNet'iniz için sayfanın **VPN bağlantıları** bölümünde, **noktaya sayfa VPN bağlantı** sayfasını açmak için istemciler grafiğini seçin.

   ![İstemciler](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/clients125.png)

2. **Siteden siteye VPN bağlantı** sayfasında, **Sertifikalar** sayfasını açmak için **Sertifikayı Yönet'i** seçin.

   ![Sertifikalar sayfası](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/ptsmanage.png)

3. **Sertifikalar** sayfasında, kaldırmak istediğiniz sertifikanın yanındaki elipsleri seçin ve ardından **Sil'i**seçin.

   ![Kök sertifikayı sil](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/deleteroot.png)

## <a name="revoke-a-client-certificate"></a>İstemci sertifikasını iptal etme

Gerekirse, bir istemci sertifikasını iptal edebilirsiniz. Sertifika iptal listesi sayesinde, ayrı istemci sertifikalarına göre Noktadan Siteye bağlantıyı seçmeli olarak reddedebilirsiniz. Bu yöntem, güvenilir bir kök sertifikasını kaldırmaktan farklıdır. Azure’dan güvenilen kök sertifika .cer dosyasını kaldırırsanız iptal edilen kök sertifika tarafından oluşturulan/imzalanan tüm istemci sertifikaları reddedilir. Kök sertifika yerine istemci sertifikasını iptal etmek, kök sertifikadan oluşturulan diğer sertifikaların Noktadan Siteye bağlantı için kimlik doğrulaması amacıyla kullanılmaya devam edilmesine olanak sağlar.

Genellikle ekip ve kuruluş düzeylerinde erişimi yönetmek için kök sertifika kullanılırken ayrı kullanıcılar üzerinde ayrıntılı erişim denetimi için iptal edilen istemci sertifikaları kullanılır.

### <a name="to-revoke-a-client-certificate"></a>İstemci sertifikasını iptal etme

Parmak izini iptal listesine ekleyerek bir istemci sertifikasını iptal edebilirsiniz.

1. İstemci sertifikasının parmak izini alın. Daha fazla bilgi için bkz. [Nasıl yapılır: Bir Sertifikanın Parmak İzini Alma](https://msdn.microsoft.com/library/ms734695.aspx).
2. Bilgileri bir metin düzenleyicisine kopyalayın ve sürekli bir dize olacak şekilde boşluklarını kaldırın.
3. Klasik sanal ağa gidin. **Site ye VPN bağlantısını**seçin ve ardından **Sertifikalar** sayfasını açmak için **Sertifikayı Yönet'i** seçin.
4. **İptal listesi** sayfasını açmak için İptal **listesini** seçin. 
5. İptal listesi sayfasına **sertifika ekle'yi** açmak için sertifika **ekle'yi** seçin.
6. **Thumbprint'te,** sertifika parmak izini boşluk suz, sürekli bir metin satırı olarak yapıştırın. Bitirmek için **Tamam'ı** seçin.

Güncelleştirme tamamlandıktan sonra sertifika artık bağlanmak için kullanılamaz. Bu sertifikayı kullanarak bağlanmaya çalışan istemciler, sertifikanın artık geçerli olmadığını belirten bir ileti alır.

## <a name="point-to-site-faq"></a>Noktadan Siteye SSS

[!INCLUDE [Point-to-Site FAQ](../../includes/vpn-gateway-faq-point-to-site-classic-include.md)]

## <a name="next-steps"></a>Sonraki adımlar

- Bağlantınız tamamlandıktan sonra sanal ağlarınıza sanal makineler ekleyebilirsiniz. Daha fazla bilgi için bkz. [Sanal Makineler](https://docs.microsoft.com/azure/). 

- Ağ ve Linux sanal makineleri hakkında daha fazla bilgi almak için [Azure ve Linux VM ağına genel bakış](../virtual-machines/linux/network-overview.md)alabiliyorum.

- P2S sorun giderme bilgileri için [Azure noktadan siteye bağlantıları sorununu giderme](vpn-gateway-troubleshoot-vpn-point-to-site-connection-problems.md).
