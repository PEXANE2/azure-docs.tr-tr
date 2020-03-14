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
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79244582"
---
# <a name="configure-a-point-to-site-connection-by-using-certificate-authentication-classic"></a>Sertifika kimlik doğrulaması kullanarak noktadan siteye bağlantı yapılandırma (klasik)

[!INCLUDE [deployment models](../../includes/vpn-gateway-classic-deployment-model-include.md)]

Bu makalede, Noktadan siteye bağlantı ile sanal ağ oluşturma konusu gösterilmektedir. Azure portal kullanarak bu VNET 'i klasik dağıtım modeliyle oluşturursunuz. Bu yapılandırma, bağlanan istemcinin kimliğini doğrulamak için otomatik olarak imzalanan veya CA tarafından verilen sertifikaları kullanır. Aşağıdaki makalelerde açıklanan seçenekleri kullanarak bu yapılandırmayı farklı bir dağıtım aracı veya modeliyle de oluşturabilirsiniz:

> [!div class="op_single_selector"]
> * [Azure portalında](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
> * [PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
> * [Azure portal (klasik)](vpn-gateway-howto-point-to-site-classic-azure-portal.md)
>

Tek bir istemci bilgisayarından sanal ağınıza güvenli bir bağlantı oluşturmak için Noktadan siteye (P2S) VPN ağ geçidi kullanırsınız. Sanal ağınıza uzak bir konumdan bağlanmak istediğinizde Noktadan siteye VPN bağlantıları yararlı olur. Bir sanal ağa bağlanması gereken yalnızca birkaç istemciniz olduğunda, P2S VPN, siteden siteye VPN yerine kullanmak üzere yararlı bir çözümdür. P2S VPN bağlantısı, istemci bilgisayardan başlatılmasıyla oluşturulur.

> [!IMPORTANT]
> Klasik dağıtım modeli yalnızca Windows VPN istemcilerini destekler ve SSL tabanlı VPN protokolü olan Güvenli Yuva Tünel Protokolünü (SSTP) kullanır. Windows dışı VPN istemcilerini desteklemek için, sanal ortamınızı Kaynak Yöneticisi dağıtım modeliyle oluşturmanız gerekir. Resource Manager dağıtım modeli SSTP’ye ek olarak IKEv2 VPN desteği sunar. Daha fazla bilgi için bkz. [P2S bağlantıları hakkında](point-to-site-about.md).
>
>

![Noktadan Siteye diyagramı](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/point-to-site-connection-diagram.png)

## <a name="prerequisites"></a>Önkoşullar

Noktadan siteye sertifika kimlik doğrulama bağlantıları aşağıdaki önkoşulları gerektirir:

* Bir Dinamik VPN ağ geçidi.
* Azure’a yüklenmiş bir kök sertifikanın ortak anahtarı (.cer dosyası). Bu anahtar, güvenilen bir sertifika olarak kabul edilir ve kimlik doğrulaması için kullanılır.
* Kök sertifikadan oluşturulmuş ve bağlanacak her bir istemci bilgisayara yüklenmiş istemci sertifikası. Bu sertifika, istemci kimlik doğrulaması için kullanılır.
* Bir VPN istemcisi yapılandırma paketi oluşturulmalı ve bağlanan her istemci bilgisayara yüklenmelidir. İstemci yapılandırma paketi, işletim sisteminde zaten bulunan yerel VPN istemcisini sanal ağa bağlanmak için gerekli bilgilerle yapılandırır.

Noktadan siteye bağlantılar için bir VPN cihazı veya şirket içi genel kullanıma yönelik bir IP adresi gerekmez. VPN bağlantısı SSTP (Güvenli Yuva Tünel Protokolü) üzerinden oluşturulur. Sunucu tarafında 1.0, 1.1 ve 1.2 SSTP sürümlerini destekliyoruz. Kullanılacak sürüm, istemci tarafından belirlenir. Windows 8.1 ve sonraki sürümlerinde, SSTP'de varsayılan olarak 1.2 kullanılır. 

Noktadan siteye bağlantılar hakkında daha fazla bilgi için bkz. [Noktadan siteye hakkında SSS](#point-to-site-faq).

### <a name="example-settings"></a>Örnek ayarlar

Aşağıdaki değerleri kullanarak bir test ortamı oluşturun veya bu makaledeki örnekleri daha iyi anlamak için bu değerlere başvurun:

- **Sanal ağ (klasik) ayarları oluşturma**
   - **Ad**: *VNet1*girin.
   - **Adres alanı**: *192.168.0.0/16*girin. Bu örnekte yalnızca bir adres alanı kullanılmaktadır. Diyagramda gösterildiği gibi, sanal ağınıza ait birden fazla adres alanı olabilir.
   - **Alt ağ adı**: *ön uç*girin.
   - **Alt ağ adres aralığı**: *192.168.1.0/24*girin.
   - **Abonelik**: kullanılabilir abonelikler listesinden bir abonelik seçin.
   - **Kaynak grubu**: *testrg*girin. Kaynak grubu yoksa, **Yeni oluştur**' u seçin.
   - **Konum**: listeden **Doğu ABD** seçin.

  - **VPN bağlantısı ayarları**
    - **Bağlantı türü**: **Noktadan siteye**seçeneğini belirleyin.
    - **Istemci adres alanı**: *172.16.201.0/24*girin. Sanal ağa bu noktadan siteye bağlantıyı kullanarak bağlanan VPN istemcileri belirtilen havuzdan bir IP adresi alır.

- **Ağ geçidi yapılandırması alt ağ ayarları**
   - **Ad**: *gatewaysubnet*ile oto doldurulmuştur.
   - **Adres aralığı**: *192.168.200.0/24*girin. 

- **Ağ geçidi yapılandırma ayarları**:
   - **Boyut**: kullanmak istediğiniz ağ geçidi SKU 'sunu seçin.
   - **Yönlendirme türü**: **dinamik**' i seçin.

## <a name="create-a-virtual-network-and-a-vpn-gateway"></a>Sanal ağ ve VPN ağ geçidi oluşturma

Başlamadan önce, bir Azure aboneliğiniz olduğunu doğrulayın. Henüz Azure aboneliğiniz yoksa [MSDN abonelik avantajlarınızı](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details) etkinleştirebilir veya [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial) için kaydolabilirsiniz.

### <a name="part-1-create-a-virtual-network"></a>1\. Kısım: Sanal ağ oluşturma

Zaten bir sanal ağınız (VNet) yoksa, bir tane oluşturun. Ekran görüntüleri örnek olarak verilmiştir. Değerlerin kendinizinkilerle değiştirildiğinden emin olun. Azure portalını kullanarak sanal ağ oluşturmak için şu adımları uygulayın:

1. [Azure Portal](https://portal.azure.com) menüsünde veya **giriş** sayfasında, **kaynak oluştur**' u seçin. **Yeni** sayfa açılır.

2. Market 'te **Ara** alanına *sanal ağ* girin ve döndürülen listeden **sanal ağ** ' ı seçin. **Sanal ağ** sayfası açılır.

3. **Bir dağıtım modeli seçin** listesinden **Klasik**' i seçin ve ardından **Oluştur**' u seçin. **Sanal ağ oluştur** sayfası açılır.

4. **Sanal ağ oluştur** sayfasında sanal ağ ayarlarını yapılandırın. Bu sayfada, ilk adres alanınızı ve tek alt ağ adres aralığınızı eklersiniz. VNet oluşturma işlemini tamamladıktan sonra geri dönüp ek alt ağları ve adres alanlarını ekleyin.

   ![Sanal ağ oluştur sayfası](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/vnet125.png)

5. Açılan listeden kullanmak istediğiniz **aboneliği** seçin.

6. Var olan bir **kaynak grubunu**seçin. Ya da yeni **Oluştur** ' u seçip bir ad girerek yeni bir kaynak grubu oluşturun. Yeni bir kaynak grubu oluşturuyorsanız, planlanan yapılandırma değerlerinize göre kaynak grubunu adlandırın. Kaynak grupları hakkında daha fazla bilgi için bkz. [Azure Resource Manager’a genel bakış](../azure-resource-manager/management/overview.md#resource-groups).

7. VNet 'iniz için bir **konum** seçin. Bu ayar, bu sanal ağa dağıttığınız kaynakların coğrafi konumunu belirler.

8. VNet oluşturmak için **Oluştur** ' u seçin. **Bildirimler** sayfasında, **devam eden bir dağıtım** iletisi görürsünüz.

8. Sanal ağınız oluşturulduktan sonra, **Bildirimler** sayfasındaki ileti **dağıtım başarılı**olarak değişir. Sanal ortamınızı panoda kolayca bulmak istiyorsanız **panoya sabitle ' yi** seçin. 

10. DNS sunucusu ekleme (isteğe bağlı). Sanal ağınızı oluşturduktan sonra ad çözümlemesi için bir DNS sunucusunun IP adresini ekleyebilirsiniz. Belirttiğiniz DNS sunucusu IP adresi, sanal ağınızdaki kaynakların adlarını çözümleyebilen bir DNS sunucusunun adresi olmalıdır.

    Bir DNS sunucusu eklemek için sanal ağ sayfanızda **DNS sunucuları** ' nı seçin. Ardından, kullanmak istediğiniz DNS sunucusunun IP adresini girin ve **Kaydet**' i seçin.

### <a name="part-2-create-a-gateway-subnet-and-a-dynamic-routing-gateway"></a>2\. Bölüm: ağ geçidi alt ağı ve dinamik yönlendirme ağ geçidi oluşturma

Bu adımda bir ağ geçidi alt ağı ve dinamik yönlendirme ağ geçidi oluşturursunuz. Klasik dağıtım modeli için Azure portal, ağ geçidi alt ağını ve ağ geçidini aynı yapılandırma sayfaları üzerinden oluşturursunuz. Yalnızca ağ geçidi Hizmetleri için ağ geçidi alt ağını kullanın. Ağ geçidi alt ağına hiçbir şeyi (VM’ler veya diğer hizmetler) doğrudan dağıtmayın.

1. Azure portal, ağ geçidi oluşturmak istediğiniz sanal ağa gidin.

2. Sanal ağınızın sayfasında **genel bakış**' ı seçin ve **VPN bağlantıları** bölümünde **ağ geçidi**' ni seçin.

   ![Ağ geçidi oluşturmak için seçin](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/beforegw125.png)
3. **Yeni VPN Bağlantısı** sayfasında **Noktadan siteye** öğesini seçin.

   ![Noktadan Siteye bağlantı türü](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/newvpnconnect.png)
4. **Istemci adres alanı**IÇIN, VPN istemcilerinin BAĞLANıRKEN bir IP adresı alacağı IP adresi aralığını ekleyin. ' Den bağlandığınız şirket içi konum ile veya bağlandığınız VNet ile örtüşmeyen özel bir IP adresi aralığı kullanın. , Kullanmak istediğiniz özel IP adresi aralığıyla, oto doldurulmuş aralığın üzerine yazabilirsiniz. Bu örnek, oto dolgulu aralığı gösterir. 

   ![İstemci adres alanı](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/clientaddress.png)
5. Ağ geçidini **hemen oluştur**' u ve ardından **isteğe bağlı ağ geçidi yapılandırması** ' nı seçerek **ağ geçidi yapılandırması** sayfasını açın.

   ![İsteğe bağlı ağ geçidi yapılandırması seçin](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/optsubnet125.png)

6. Ağ Geçidi **yapılandırması** sayfasında, ağ geçidi alt ağını eklemek Için **alt ağ** ' ı seçin. /29 kadar küçük bir ağ geçidi alt ağı oluşturmak mümkündür. Ancak, en az/28 veya/27 ' yi seçerek daha fazla adres içeren daha büyük bir alt ağ oluşturmanızı öneririz. Bunun yapılması, gelecekte isteyebileceğiniz olası ek yapılandırmalara uyum sağlamak için yeterli adrese izin verir. Ağ geçidi alt ağlarıyla çalışırken, ağ güvenlik grubunu (NSG) ağ geçidi alt ağıyla ilişkilendirmekten kaçının. Bir ağ güvenlik grubunun bu alt ağ ile ilişkilendirilmesi, VPN ağ geçidinizin beklendiği gibi çalışmamasına neden olabilir. Bu ayarı kaydetmek için **Tamam ' ı** seçin.

   ![GatewaySubnet ekleme](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/gwsubnet125.png)
7. Ağ geçidi **Boyutu** seçin. Boyut, sanal ağ geçidinizin ağ geçidi SKU’sudur. Azure portal **varsayılan SKU varsayılandır.** Ağ Geçidi SKU 'Ları hakkında daha fazla bilgi için bkz. [VPN Gateway ayarları hakkında](vpn-gateway-about-vpn-gateway-settings.md#gwsku).

   ![Ağ geçidi boyutu](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/gwsize125.png)
8. Ağ geçidiniz için **Yönlendirme Türü** seçin. P2S yapılandırmaları bir **Dinamik** yönlendirme türü gerektirir. Bu sayfayı yapılandırmayı bitirdiğinizde **Tamam ' ı** seçin.

   ![Yönlendirme türünü yapılandırma](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/routingtype125.png)

9. **Yenı VPN bağlantısı** sayfasında, sanal ağ geçidinizi oluşturmaya başlamak için sayfanın altındaki **Tamam** ' ı seçin. Bir VPN ağ geçidinin tamamlanması, seçtiğiniz ağ geçidi SKU 'suna bağlı olarak 45 dakika sürebilir.
 
## <a name="generatecerts"></a>Sertifika oluşturma

Azure, Noktadan siteye VPN 'Lerde VPN istemcilerinin kimliğini doğrulamak için sertifikaları kullanır. Kök sertifikanın ortak anahtar bilgilerini Azure'a yükleyin. Ortak anahtar daha sonra *güvenilir*olarak değerlendirilir. İstemci sertifikalarının güvenilir kök sertifikadan oluşturulması ve ardından Sertifikalar-Geçerli Kullanıcı \ kişisel \ sertifikalar sertifika deposundaki her bir istemci bilgisayara yüklenmesi gerekir. Sertifika, VNet 'e bağlanırken istemcinin kimliğini doğrulamak için kullanılır. 

Otomatik olarak imzalanan sertifikalar kullanıyorsanız, bunların belirli parametreler kullanılarak oluşturulması gerekir. [PowerShell ve Windows 10](vpn-gateway-certificates-point-to-site.md)veya [MakeCert](vpn-gateway-certificates-point-to-site-makecert.md)yönergelerini kullanarak otomatik olarak imzalanan bir sertifika oluşturabilirsiniz. Otomatik olarak imzalanan kök sertifikaları kullanırken ve otomatik olarak imzalanan kök sertifikadan istemci sertifikaları oluştururken bu talimatlardaki adımları izlemeniz önemlidir. Aksi halde, oluşturduğunuz sertifikalar P2S bağlantılarıyla uyumlu olmaz ve bağlantı hatası alırsınız.

### <a name="acquire-the-public-key-cer-for-the-root-certificate"></a>Kök sertifika için ortak anahtarı (. cer) alma

[!INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-p2s-rootcert-include.md)]

### <a name="generate-a-client-certificate"></a>İstemci sertifikası oluşturma

[!INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-p2s-clientcert-include.md)]

## <a name="upload-the-root-certificate-cer-file"></a>Kök sertifika .cer dosyasını karşıya yükleme

Ağ Geçidi oluşturulduktan sonra, güvenilen kök sertifikanın. cer dosyasını (ortak anahtar bilgilerini içerir) Azure sunucusuna yükleyin. Kök sertifika için özel anahtarı karşıya yüklemeyin. Sertifikayı karşıya yükledikten sonra Azure, güvenilir kök sertifikadan oluşturulmuş bir istemci sertifikası yüklemiş olan istemcilerin kimliğini doğrulamak için bunu kullanır. Gerekirse, daha sonra ek Güvenilen kök sertifika dosyalarını (en fazla 20) karşıya yükleyebilirsiniz.  

1. Sanal ağınız için sayfanın **VPN bağlantıları** bölümünde, istemci grafiği ' ni seçerek **Noktadan siteye VPN bağlantısı** sayfasını açın.

   ![İstemciler](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/clients125.png)

2. **Noktadan sıteye VPN bağlantısı** sayfasında, **Sertifikalar** sayfasını açmak için **sertifikayı Yönet** ' i seçin.

   ![Sertifikalar sayfası](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/ptsmanage.png)

1. **Sertifikalar** sayfasında, karşıya **Yükle** ' yi seçerek **sertifikayı karşıya yükle** sayfasını açın.

    ![Sertifikaları karşıya yükle sayfası](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/uploadcerts.png)

4. . Cer dosyasına gözatabir klasör grafiği seçin. Dosyayı seçin ve ardından **Tamam**' ı seçin. Karşıya yüklenen sertifika, **Sertifikalar** sayfasında görüntülenir.

   ![Sertifikayı karşıya yükleme](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/upload.png)


## <a name="configure-the-client"></a>İstemciyi yapılandırma

Noktadan siteye VPN kullanarak bir sanal ağa bağlanmak için, her istemcinin yerel Windows VPN istemcisini yapılandırmak üzere bir paket yüklemesi gerekir. Yapılandırma paketi, sanal ağa bağlanmak için gereken ayarlarla yerel Windows VPN istemcisini yapılandırır.

Sürümünün istemci mimarisiyle eşleşmesi şartıyla, her istemci bilgisayarda aynı VPN istemcisi yapılandırma paketini kullanabilirsiniz. Desteklenen istemci işletim sistemlerinin listesi için bkz. [Noktadan siteye BAĞLANTıLARı SSS](#point-to-site-faq).

### <a name="generate-and-install-a-vpn-client-configuration-package"></a>VPN istemcisi yapılandırma paketi oluşturma ve yüklemeyi

1. Azure portal, sanal ağınızın **genel bakış** sayfasında, **VPN bağlantıları**' nda, istemci GRAFIĞINI seçerek **Noktadan siteye VPN bağlantısı** sayfasını açın.

2. **Noktadan sıteye VPN bağlantısı** sayfasında, yüklü olduğu istemci işletim sistemine karşılık gelen indirme paketini seçin:

   * 64 bit istemciler için **VPN İstemcisi (64 bit)** seçeneğini belirleyin.
   * 32 bit istemciler için **VPN İstemcisi (32 bit)** seçeneğini belirleyin.

   ![VPN istemcisi yapılandırma paketini indirme](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/dlclient.png)

3. Paket oluşturulduktan sonra indirin ve istemci bilgisayarınıza yükleyin. Bir SmartScreen açılır penceresi görürseniz **daha fazla bilgi**' yi seçin ve **yine de Çalıştır**' ı seçin. Paketi ayrıca diğer istemci bilgisayarlara yüklemek üzere kaydedebilirsiniz.

### <a name="install-a-client-certificate"></a>İstemci sertifikası yükler

İstemci sertifikalarını oluşturmak için kullanılandan farklı bir istemci bilgisayardan bir P2S bağlantısı oluşturmak için, bir istemci sertifikası yüklersiniz. İstemci sertifikası yüklediğinizde, istemci sertifikası aktarıldığında oluşturulan parolanın olması gerekir. Genellikle, sertifikayı yalnızca çift tıklayarak yükleyebilirsiniz. Daha fazla bilgi için bkz. [Dışarı aktarılan istemci sertifikasını yükleme](vpn-gateway-certificates-point-to-site.md#install).


## <a name="connect-to-your-vnet"></a>Sanal ağınıza bağlanma

>[!NOTE]
>Bağlanmakta olduğunuz istemci bilgisayarında Yönetici haklarına sahip olmanız gerekir.
>
>

1. VNet 'iniz ile bağlantı kurmak için, istemci bilgisayarda Azure portal **VPN bağlantıları** ' na gidin ve oluşturduğunuz VPN bağlantısını bulun. VPN bağlantısı, sanal ağınızla aynı ada sahiptir. **Bağlan**’ı seçin. Sertifikayla ilgili bir açılır ileti görünürse, yükseltilmiş ayrıcalıkları kullanmak için **devam** ' ı seçin.

2. **Bağlantı durumu sayfasında** , bağlantıyı başlatmak için **Bağlan** ' ı seçin. **Sertifika seç** ekranını görürseniz, görüntülenen istemci sertifikasının doğru bir tane olduğunu doğrulayın. Aksi takdirde, açılan listeden doğru sertifikayı seçin ve ardından **Tamam**' ı seçin.

3. Bağlantınız başarılı olursa, **bağlı** bir bildirim görürsünüz.


### <a name="troubleshooting-p2s-connections"></a>P2S bağlantılarının sorunlarını giderme

[!INCLUDE [verify-client-certificates](../../includes/vpn-gateway-certificates-verify-client-cert-include.md)]

## <a name="verify-the-vpn-connection"></a>VPN bağlantısını doğrulama

1. VPN bağlantınızın etkin olduğunu doğrulayın. İstemci bilgisayarınızda yükseltilmiş bir komut istemi açın ve **ipconfig/all**komutunu çalıştırın.
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

## <a name="add-or-remove-trusted-root-certificates"></a>Güvenilen kök sertifikaları ekleme veya kaldırma

Azure’da güvenilen kök sertifikayı ekleyebilir veya kaldırabilirsiniz. Bir kök sertifikayı kaldırdığınızda, o kökten oluşturulmuş bir sertifikaya sahip istemciler artık kimlik doğrulaması yapabilir ve bağlanamaz. Bu istemcilerin kimlik doğrulaması ve yeniden bağlanması için, Azure tarafından güvenilen bir kök sertifikadan oluşturulan yeni bir istemci sertifikası yüklemelisiniz.

### <a name="to-add-a-trusted-root-certificate"></a>Güvenilen kök sertifika ekleme

Azure'a en fazla 20 güvenilen kök sertifika .cer dosyası ekleyebilirsiniz. Yönergeler için bkz. kök sertifika. cer dosyasını karşıya yükleme.

### <a name="to-remove-a-trusted-root-certificate"></a>Güvenilen kök sertifikayı kaldırmak için

1. Sanal ağınız için sayfanın **VPN bağlantıları** bölümünde, istemci grafiği ' ni seçerek **Noktadan siteye VPN bağlantısı** sayfasını açın.

   ![İstemciler](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/clients125.png)

2. **Noktadan sıteye VPN bağlantısı** sayfasında, **Sertifikalar** sayfasını açmak için **sertifikayı Yönet** ' i seçin.

   ![Sertifikalar sayfası](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/ptsmanage.png)

3. **Sertifikalar** sayfasında, kaldırmak istediğiniz sertifikanın yanındaki üç noktayı seçin ve **Sil**' i seçin.

   ![Kök sertifikayı sil](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/deleteroot.png)

## <a name="revoke-a-client-certificate"></a>İstemci sertifikasını iptal etme

Gerekirse, bir istemci sertifikasını iptal edebilirsiniz. Sertifika iptal listesi sayesinde, ayrı istemci sertifikalarına göre Noktadan Siteye bağlantıyı seçmeli olarak reddedebilirsiniz. Bu yöntem, güvenilen kök sertifikayı kaldırma işleminden farklıdır. Azure’dan güvenilen kök sertifika .cer dosyasını kaldırırsanız iptal edilen kök sertifika tarafından oluşturulan/imzalanan tüm istemci sertifikaları reddedilir. Kök sertifika yerine istemci sertifikasını iptal etmek, kök sertifikadan oluşturulan diğer sertifikaların Noktadan Siteye bağlantı için kimlik doğrulaması amacıyla kullanılmaya devam edilmesine olanak sağlar.

Genellikle ekip ve kuruluş düzeylerinde erişimi yönetmek için kök sertifika kullanılırken ayrı kullanıcılar üzerinde ayrıntılı erişim denetimi için iptal edilen istemci sertifikaları kullanılır.

### <a name="to-revoke-a-client-certificate"></a>İstemci sertifikasını iptal etme

Parmak izini iptal listesine ekleyerek bir istemci sertifikasını iptal edebilirsiniz.

1. İstemci sertifikasının parmak izini alın. Daha fazla bilgi için bkz. [Nasıl yapılır: Bir Sertifikanın Parmak İzini Alma](https://msdn.microsoft.com/library/ms734695.aspx).
2. Bilgileri bir metin düzenleyicisine kopyalayın ve alanlarını sürekli bir dize olacak şekilde kaldırın.
3. Klasik sanal ağa gidin. **Noktadan sıteye VPN bağlantısı**' nı seçin, sonra sertifika **sayfasını açmak** için **sertifikayı Yönet** ' i seçin.
4. İptal **listesi ' ni seçerek** **iptal listesi** sayfasını açın. 
5. Sertifika **Ekle** ' yi seçerek **Iptal listesine sertifika ekle** sayfasını açın.
6. **Parmak izi**' nde, sertifika parmak izini boşluk olmadan sürekli bir metin satırı olarak yapıştırın. Son ' u seçerek **Tamam ' ı** seçin.

Güncelleştirme tamamlandıktan sonra sertifika artık bağlanmak için kullanılamaz. Bu sertifikayı kullanarak bağlanmaya çalışır olan istemciler, sertifikanın artık geçerli olmadığını belirten bir ileti alır.

## <a name="point-to-site-faq"></a>Noktadan siteye hakkında SSS

[!INCLUDE [Point-to-Site FAQ](../../includes/vpn-gateway-faq-point-to-site-classic-include.md)]

## <a name="next-steps"></a>Sonraki adımlar

- Bağlantınız tamamlandıktan sonra sanal ağlarınıza sanal makineler ekleyebilirsiniz. Daha fazla bilgi için bkz. [Sanal Makineler](https://docs.microsoft.com/azure/). 

- Ağ ve Linux sanal makineleri hakkında daha fazla bilgi edinmek için bkz. [Azure ve LINUX VM ağına genel bakış](../virtual-machines/linux/network-overview.md).

- P2S sorun giderme bilgileri için [Azure noktadan siteye bağlantıları sorununu giderme](vpn-gateway-troubleshoot-vpn-point-to-site-connection-problems.md).
