---
title: 'VNets: klasik: Azure portalı arasında bağlantı kurun'
description: PowerShell ve Azure portalını kullanarak Azure sanal ağlarını birbirine bağlayın.
services: vpn-gateway
titleSuffix: Azure VPN Gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: article
ms.date: 02/12/2020
ms.author: cherylmc
ms.openlocfilehash: 63c6329ad62289cd127902c1438073b28fc8683e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77201858"
---
# <a name="configure-a-vnet-to-vnet-connection-classic"></a>VNet'den VNet bağlantısına yapılandırın (klasik)

[!INCLUDE [deployment models](../../includes/vpn-gateway-classic-deployment-model-include.md)]

Bu makale, sanal ağlar arasında bir VPN ağ geçidi bağlantısı oluşturmanıza yardımcı olur. Sanal ağlar aynı ya da farklı bölgelerde ve aynı ya da farklı aboneliklerde bulunuyor olabilirler. Bu makaledeki adımlar klasik dağıtım modeli ve Azure portalı için geçerlidir. Ayrıca aşağıdaki listeden farklı bir seçenek belirtip farklı bir dağıtım aracı veya dağıtım modeli kullanarak da bu yapılandırmayı oluşturabilirsiniz:

> [!div class="op_single_selector"]
> * [Azure portalında](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md)
> * [Powershell](vpn-gateway-vnet-vnet-rm-ps.md)
> * [Azure CLI](vpn-gateway-howto-vnet-vnet-cli.md)
> * [Azure portal (klasik)](vpn-gateway-howto-vnet-vnet-portal-classic.md)
> * [Farklı dağıtım modellerini bağlama - Azure portalı](vpn-gateway-connect-different-deployment-models-portal.md)
> * [Farklı dağıtım modellerini bağlama - PowerShell](vpn-gateway-connect-different-deployment-models-powershell.md)
>
>

![VNet - VNet Bağlantı Diyagramı](./media/vpn-gateway-howto-vnet-vnet-portal-classic/v2vclassic.png)



## <a name="about-vnet-to-vnet-connections"></a>Sanal Ağdan Sanal Ağa bağlantıları hakkında

VPN ağ geçidi kullanarak klasik dağıtım modelinde sanal bir ağı başka bir sanal ağa (VNet-to-VNet) bağlamak, sanal ağı şirket içi bir site konumuna bağlamaya benzer. Her iki bağlantı türü de IPsec/IKE kullanarak güvenli bir tünel sunmak üzere bir VPN ağ geçidi kullanır.

Bağladiğiniz VNet'ler farklı aboneliklerde ve farklı bölgelerde olabilir. VNet ile VNet iletişimini çok siteli yapılandırmalarla birleştirebilirsiniz. Bu özellik şirket içi ve şirket dışı bağlantıyla ağ içi bağlantıyı birleştiren ağ topolojileri kurabilmenize olanak sağlar.

![VNet - VNet Bağlantıları](./media/vpn-gateway-howto-vnet-vnet-portal-classic/aboutconnections.png)

### <a name="why-connect-virtual-networks"></a><a name="why"></a>Sanal ağları neden bağlamalıyız?

Sanal ağları aşağıdaki sebeplerden dolayı bağlamak isteyebilirsiniz:

* **Çapraz bölge coğrafi artıklığı ve coğrafi-durum**

  * Kendi coğrafi çoğaltma veya eşitlemenizi, güvenli bağlantıyla İnternet’te uç noktalara gitmeden ayarlayabilirsiniz.
  * Azure Yük Dengeleyicisi ve Microsoft veya üçüncü taraf kümeleme teknolojisiyle, birden çok Azure bölgesinde coğrafi artıklıkla yüksek oranda kullanılabilir iş yükü ayarlayabilirsiniz. Buna önemli bir örnek olarak SQL Always On ile birden fazla Azure bölgesine yayılan Kullanılabilirlik Grupları’nı birlikte kurmak verilebilir.
* **Güçlü yalıtım sınırına sahip bölgesel çok katmanlı uygulamalar**

  * Aynı bölgede, güçlü yalıtım ve güvenli katmanlar arası iletişimle birbirine bağlı birden fazla VNet içeren çok katmanlı uygulamalar ayarlayabilirsiniz.
* **Azure'da çapraz abonelik, kurumlar arası iletişim**

  * Birden çok Azure aboneliğiniz varsa, farklı aboneliklerden gelen iş yüklerini sanal ağlar arasında güvenli bir şekilde birbirine bağlayabilirsiniz.
  * Kuruluşlar veya hizmet sağlayıcılar için Azure'da güvenli VPN teknolojisiyle kuruluşlar arası iletişimi etkinleştirebilirsiniz.

Sanal ağlar arası bağlantılar hakkında daha fazla bilgi için bu makalenin sonunda yer alan [Sanal ağdan sanal ağa dikkat edilecek noktalar](#faq) bölümünü inceleyin.

### <a name="working-with-azure-powershell"></a><a name="powershell"></a>Azure PowerShell ile çalışma

Portalı adımların çoğu için kullanırız, ancak VNets arasındaki bağlantıları oluşturmak için PowerShell'i kullanmanız gerekir. Azure portalını kullanarak bağlantıları oluşturamazsınız. [!INCLUDE [vpn-gateway-classic-powershell](../../includes/vpn-gateway-powershell-classic-locally.md)]

## <a name="step-1---plan-your-ip-address-ranges"></a><a name="plan"></a>1. Adım - IP adresi aralığınızı planlama

Sanal ağlarınızı yapılandırmak için kullanacağınız aralıklara karar vermeniz önemlidir. Bu yapılandırma için, VNet aralıklarınızın hiçbirinin birbiriyle veya bağlandıkları yerel ağlarla örtüşmediğinden emin olmalısınız.

Aşağıdaki tablovnet'lerinizi nasıl tanımlayabildiğini gösteren bir örnek gösterilmektedir. Aralıkları yalnızca bir kılavuz olarak kullanın. Sanal ağlarınızın aralıklarını yazın. Sonraki adımlar için bu bilgilere ihtiyacınız var.

**Örnek**

| Sanal Ağ | Adres Alanı | Bölge | Yerel ağ sitesine bağlanır |
|:--- |:--- |:--- |:--- |
| TestVNet1 |TestVNet1<br>(10.11.0.0/16)<br>(10.12.0.0/16) |Doğu ABD |VNet4Yerel<br>(10.41.0.0/16)<br>(10.42.0.0/16) |
| TestVNet4 |TestVNet4<br>(10.41.0.0/16)<br>(10.42.0.0/16) |Batı ABD |VNet1Yerel<br>(10.11.0.0/16)<br>(10.12.0.0/16) |

## <a name="step-2---create-the-virtual-networks"></a><a name="vnetvalues"></a>Adım 2 - Sanal ağlar oluşturma

[Azure portalında](https://portal.azure.com)iki sanal ağ oluşturun. Klasik sanal ağlar oluşturma adımları [için](../virtual-network/virtual-networks-create-vnet-classic-pportal.md)bkz. 

Portalı klasik bir sanal ağ oluşturmak için kullanırken, aşağıdaki adımları kullanarak sanal ağ sayfasına gitmelisiniz, aksi takdirde klasik bir sanal ağ oluşturma seçeneği görünmez:

1. 'Yeni' sayfasını açmak için '+' sayfasını tıklatın.
2. 'Pazar yeri arama' alanına 'Sanal Ağ' yazın. Bunun yerine, Networking -> Virtual Network'ü seçin, klasik bir VNet oluşturma seçeneğine girmezsiniz.
3. Döndürülen listeden 'Sanal Ağ'ı bulun ve Sanal Ağ sayfasını açmak için tıklatın. 
4. Sanal ağ sayfasında, klasik bir VNet oluşturmak için 'Klasik' seçeneğini belirleyin. 

Bu makaleyi bir alıştırma olarak kullanıyorsanız, aşağıdaki örnek değerleri kullanabilirsiniz:

**TestVNet1 değerleri**

Adı: TestVNet1<br>
Adres alanı: 10.11.0.0/16, 10.12.0.0/16 (isteğe bağlı)<br>
Alt ağ adı: varsayılan<br>
Alt net adres aralığı: 10.11.0.1/24<br>
Kaynak grubu: ClassicRG<br>
Location: East US<br>
Ağ Geçidi Subnet: 10.11.1.0/27

**TestVNet4 değerleri**

Adı: TestVNet4<br>
Adres alanı: 10.41.0.0/16, 10.42.0.0/16 (isteğe bağlı)<br>
Alt ağ adı: varsayılan<br>
Alt net adres aralığı: 10.41.0.1/24<br>
Kaynak grubu: ClassicRG<br>
Location: West US<br>
Ağ Geçidi Subnet: 10.41.1.0/27

**VNet'lerinizi oluştururken aşağıdaki ayarları aklınızda bulundurun:**

* **Sanal Ağ Adres Boşlukları** – Sanal Ağ Adres Boşlukları sayfasında, sanal ağınız için kullanmak istediğiniz adres aralığını belirtin. Bunlar, bu sanal ağa dağıttığınız VM'lere ve diğer rol örneklerine atanacak dinamik IP adresleridir.<br>Seçtiğiniz adres boşlukları, bu VNet'in bağlanacak diğer VNet'lerin veya şirket içi konumların adres alanlarıyla çakışamaz.

* **Konum** – Sanal ağ oluşturduğunuzda, onu bir Azure konumuyla (bölge) ilişkilendirin. Örneğin, sanal ağınıza dağıtılan Sanal M'lerinizin Batı ABD'de fiziksel olarak bulunmasını istiyorsanız, bu konumu seçin. Oluşturduktan sonra sanal ağınızla ilişkili konumu değiştiremezsiniz.

**VNet'lerinizi oluşturduktan sonra aşağıdaki ayarları ekleyebilirsiniz:**

* **Adres alanı** – Bu yapılandırma için ek adres alanı gerekmez, ancak VNet'i oluşturduktan sonra ek adres alanı ekleyebilirsiniz.

* **Alt ağlar** – Bu yapılandırma için ek alt ağlar gerekmez, ancak VM'lerinizin diğer rol örneklerinden ayrı bir alt ağda olmasını isteyebilirsiniz.

* **DNS sunucuları** – DNS sunucu adını ve IP adresini girin. Bu ayarla bir DNS sunucusu oluşturulmaz. Söz konusu ayar, bu sanal ağa ilişkin ad çözümlemesi için kullanmak istediğiniz DNS sunucularını belirtmenize olanak sağlar.

Bu bölümde, bağlantı türünü, yerel siteyi yapılandırır ve ağ geçidini oluşturursunuz.

## <a name="step-3---configure-the-local-site"></a><a name="localsite"></a>Adım 3 - Yerel siteyi yapılandır

Azure, VNet'ler arasındaki trafiği nasıl yönlendireceklerini belirlemek için her yerel ağ sitesinde belirtilen ayarları kullanır. Her VNet, trafiği yönlendirmek istediğiniz ilgili yerel ağı işaret etmelidir. Her yerel ağ sitesine başvurmak için kullanmak istediğiniz adı belirlersiniz. Açıklayıcı bir şey kullanmak en iyisidir.

Örneğin, TestVNet1 oluşturduğunuz 'VNet4Local' adlı yerel bir ağ sitesine bağlanır. VNet4Local ayarları TestVNet4 için adres önekleri içerir.

Her VNet için yerel site diğer VNet'tir. Yapılandırmamız için aşağıdaki örnek değerler kullanılır:

| Sanal Ağ | Adres Alanı | Bölge | Yerel ağ sitesine bağlanır |
|:--- |:--- |:--- |:--- |
| TestVNet1 |TestVNet1<br>(10.11.0.0/16)<br>(10.12.0.0/16) |Doğu ABD |VNet4Yerel<br>(10.41.0.0/16)<br>(10.42.0.0/16) |
| TestVNet4 |TestVNet4<br>(10.41.0.0/16)<br>(10.42.0.0/16) |Batı ABD |VNet1Yerel<br>(10.11.0.0/16)<br>(10.12.0.0/16) |

1. Azure portalında TestVNet1'i bulun. Sayfanın **VPN bağlantıları** bölümünde Ağ **Geçidi'ni**tıklatın.

    ![Ağ geçidi yok](./media/vpn-gateway-howto-vnet-vnet-portal-classic/nogateway.png)
2. Yeni **VPN Bağlantısı** **sayfasında, Siteden Siteye'i**seçin.
3. Yerel site sayfasını açmak ve ayarları yapılandırmak için **Yerel siteyi** tıklatın.
4. Yerel **site** sayfasında, yerel sitenizin adını adlandırın. Örneğimizde, yerel siteye 'VNet4Local' adını verdik.
5. **VPN ağ geçidi IP adresi**için, geçerli bir biçimde olduğu sürece istediğiniz herhangi bir IP adresini kullanabilirsiniz. Genellikle, bir VPN aygıtı için gerçek harici IP adresini kullanırsınız. Ancak, klasik bir VNet'ten VNet'e yapılandırmaiçin, VNet'inizin ağ geçidine atanan genel IP adresini kullanırsınız. Sanal ağ ağ ağ geçidini henüz oluşturmadığınızı göz önünde bulundurulun, yer tutucu olarak geçerli bir genel IP adresi belirtirsiniz.<br>Bu boş bırakmayın - bu yapılandırma için isteğe bağlı değildir. Daha sonraki bir adımda, bu ayarlara geri döner ve Azure oluşturduktan sonra bunları ilgili sanal ağ ağ IP adresleriyle yapılandırın.
6. **İstemci Adres Alanı**için, diğer VNet adres alanını kullanın. Planlama örneğinize bakın. Ayarlarınızı kaydetmek ve Yeni VPN **Bağlantısı** sayfasına geri dönmek için **Tamam'ı** tıklatın.

    ![yerel site](./media/vpn-gateway-howto-vnet-vnet-portal-classic/localsite.png)

## <a name="step-4---create-the-virtual-network-gateway"></a><a name="gw"></a>Adım 4 - Sanal ağ ağ geçidi oluşturma

Her sanal ağ bir sanal ağ ağ geçidi olmalıdır. Sanal ağ ağ geçidi yolları ve trafiği şifreler.

1. **Yeni VPN Bağlantısı** sayfasında, **Ağ geçidini hemen oluştur** onay kutusunu işaretleyin.
2. **Alt net, boyut ve yönlendirme türünü**tıklatın. Ağ **Geçidi yapılandırma** sayfasında **Subnet'i**tıklatın.
3. Ağ geçidi alt ağ adı otomatik olarak gerekli adı 'GatewaySubnet' ile doldurulur. **Adres aralığı,** VPN ağ geçidi hizmetlerine ayrılan IP adreslerini içerir. Bazı yapılandırmalar /29 ağ geçidi alt ağına izin verir, ancak ağ geçidi hizmetleri için daha fazla IP adresi gerektirebilecek gelecekteki yapılandırmaları karşılamak için bir /28 veya /27 kullanmak en iyisidir. Örnek ayarlarımızda 10.11.1.0/27'yi kullanıyoruz. Adres alanını ayarlayın, ardından **Tamam'ı**tıklatın.
4. **Ağ Geçidi Boyutunu**yapılandırın. Bu ayar [Ağ Geçidi SKU'yu](vpn-gateway-about-vpn-gateway-settings.md#gwsku)ifade eder.
5. **Yönlendirme Türünü**yapılandırın. Bu yapılandırma için yönlendirme türü **Dinamik**olmalıdır. Ağ geçidini yıkıp yeni bir ağ geçidi oluşturmadığınız sürece yönlendirme türünü daha sonra değiştiremezsiniz.
6. **Tamam**'a tıklayın.
7. Yeni **VPN Bağlantısı** sayfasında, sanal ağ ağ ağ geçidi oluşturmaya başlamak için **Tamam'ı** tıklatın. Bir ağ geçidinin oluşturulması, seçili ağ geçidi SKU’suna bağlı olarak 45 dakika veya daha uzun sürebilir.

## <a name="step-5---configure-testvnet4-settings"></a><a name="vnet4settings"></a>Adım 5 - TestVNet4 ayarlarını yapılandır

Yerel bir [site oluşturmak](#localsite) ve gerektiğinde değerleri ikame ederek TestVNet4'ü yapılandırmak için [sanal ağ ağ ağ geçidi](#gw) oluşturma adımlarını yineleyin. Bunu bir alıştırma olarak yapıyorsanız, [Örnek değerlerini](#vnetvalues)kullanın.

## <a name="step-6---update-the-local-sites"></a><a name="updatelocal"></a>Adım 6 - Yerel siteleri güncelleştirin

Sanal ağ ağ ağ geçitleriniz her iki VNet için de oluşturulduktan sonra, yerel siteler **VPN ağ geçidi IP adresi** değerlerini ayarlamanız gerekir.

|VNet adı|Bağlı site|Ağ geçidi IP adresi|
|:--- |:--- |:--- |
|TestVNet1|VNet4Yerel|TestVNet4 için VPN ağ geçidi IP adresi|
|TestVNet4|VNet1Yerel|TestVNet1 için VPN ağ geçidi IP adresi|

### <a name="part-1---get-the-virtual-network-gateway-public-ip-address"></a>Bölüm 1 - Sanal ağ ağ geçidi genel IP adresini alın

1. Sanal ağınızı Azure portalında bulun.
2. VNet **Genel Bakış** sayfasını açmak için tıklayın. Sayfada, VPN **bağlantılarında,** sanal ağ ağ ağ geçidinizin IP adresini görüntüleyebilirsiniz.

   ![Genel IP](./media/vpn-gateway-howto-vnet-vnet-portal-classic/publicIP.png)
3. IP adresini kopyalayın. Bir sonraki bölümde kullanacaksınız.
4. TestVNet4 için bu adımları yineleyin

### <a name="part-2---modify-the-local-sites"></a>Bölüm 2 - Yerel siteleri değiştirin

1. Sanal ağınızı Azure portalında bulun.
2. VNet **Genel Bakış** sayfasında yerel siteyi tıklatın.

   ![Oluşturulan yerel site](./media/vpn-gateway-howto-vnet-vnet-portal-classic/local.png)
3. **Siteden Siteye VPN Bağlantıları** sayfasında, değiştirmek istediğiniz yerel sitenin adını tıklatın.

   ![Yerel siteyi aç](./media/vpn-gateway-howto-vnet-vnet-portal-classic/openlocal.png)
4. Değiştirmek istediğiniz **Yerel siteyi** tıklatın.

   ![siteyi değiştirme](./media/vpn-gateway-howto-vnet-vnet-portal-classic/connections.png)
5. VPN **ağ geçidi IP adresini** güncelleştirin ve ayarları kaydetmek için **Tamam'ı** tıklatın.

   ![ağ geçidi IP](./media/vpn-gateway-howto-vnet-vnet-portal-classic/gwupdate.png)
6. Diğer sayfaları kapatın.
7. TestVNet4 için bu adımları yineleyin.

## <a name="step-7---retrieve-values-from-the-network-configuration-file"></a><a name="getvalues"></a>Adım 7 - Ağ yapılandırma dosyasından değerleri alma

Azure portalında klasik VNet'ler oluşturduğunuzda, görüntülediğiniz ad PowerShell için kullandığınız tam ad değildir. Örneğin, portalda **TestVNet1** olarak adlandırılan bir VNet'in ağ yapılandırma dosyasında çok daha uzun bir adı olabilir. Adı gibi bir şey görünebilir: **Grup ClassicRG TestVNet1**. Bağlantılarınızı oluştururken, ağ yapılandırma dosyasında gördüğünüz değerleri kullanmanız önemlidir.

Aşağıdaki adımlarda, Azure hesabınıza bağlanır ve bağlantılarınız için gerekli olan değerleri elde etmek için ağ yapılandırma dosyasını karşıdan yükleyip görüntüleyebilirsiniz.

1. Azure Hizmet Yönetimi (SM) PowerShell cmdlets'in en son sürümünü indirin ve yükleyin. Daha fazla bilgi için bkz: [Azure PowerShell ile Çalışma](#powershell).

2. PowerShell konsolunuzu yüksek haklarla açın. Bağlanmanıza yardımcı olmak için aşağıdaki örnekleri kullanın. PowerShell hizmet yönetimi modüllerini kullanarak bu komutları yerel olarak çalıştırmalısınız. Hizmet yönetimine geçmek için şu komutu kullanın:

   ```powershell
   azure config mode asm
   ```
3. Hesabınıza bağlanın. Bağlanmanıza yardımcı olması için aşağıdaki örneği kullanın:

   ```powershell
   Add-AzureAccount
   ```
4. Hesapla ilişkili abonelikleri kontrol edin.

   ```powershell
   Get-AzureSubscription
   ```
5. Birden fazla aboneliğiniz varsa, kullanmak istediğiniz aboneliği seçin.

   ```powershell
   Select-AzureSubscription -SubscriptionId "Replace_with_your_subscription_ID"
   ```
6. Ağ yapılandırma dosyasını dışa aktarın ve görüntüleyin. Bilgisayarınızda bir dizin oluşturun ve sonra ağ yapılandırma dosyasını dizine aktarın. Bu örnekte, ağ yapılandırma dosyası **C:\AzureNet'e dışa**aktarılır.

   ```powershell
   Get-AzureVNetConfig -ExportToFile C:\AzureNet\NetworkConfig.xml
   ```
7. Dosyayı bir metin düzenleyicisi ile açın ve VNet'lerinizin ve sitelerinizin adlarını görüntüleyin. Bu adlar, bağlantılarınızı oluştururken kullandığınız adlar olacaktır.<br>VNet adları **VirtualNetworkSite adı** = olarak listelenir<br>Site adları **LocalNetworkSiteRef adı** = olarak listelenir

## <a name="step-8---create-the-vpn-gateway-connections"></a><a name="createconnections"></a>Adım 8 - VPN ağ geçidi bağlantıları oluşturma

Önceki tüm adımlar tamamlandığında, IPsec/IKE önceden paylaşılan anahtarlarını ayarlayabilir ve bağlantıyı oluşturabilirsiniz. Bu adım kümesi PowerShell kullanır. Klasik dağıtım modeli için VNet-vNet bağlantıları Azure portalında yapılandırılamaz.

Örneklerde, paylaşılan anahtarın tamamen aynı olduğuna dikkat edin. Paylaşılan anahtar her zaman eşleşmelidir. Bu örneklerdeki değerleri VNet'lerinizin ve Yerel Ağ Sitelerinizin tam adlarıyla değiştirdiğinizden emin olun.

1. TestVNet1 - TestVNet4 bağlantısını oluşturun.

   ```powershell
   Set-AzureVNetGatewayKey -VNetName 'Group ClassicRG TestVNet1' `
   -LocalNetworkSiteName '17BE5E2C_VNet4Local' -SharedKey A1b2C3D4
   ```
2. TestVNet4 - TestVNet1 bağlantısı oluşturun.

   ```powershell
   Set-AzureVNetGatewayKey -VNetName 'Group ClassicRG TestVNet4' `
   -LocalNetworkSiteName 'F7F7BFC7_VNet1Local' -SharedKey A1b2C3D4
   ```
3. Bağlantıların başlatılmasını bekleyin. Ağ geçidi başharfe bulaştığında Durum 'Başarılı' olur.

   ```
   Error          :
   HttpStatusCode : OK
   Id             :
   Status         : Successful
   RequestId      :
   StatusCode     : OK
   ```

## <a name="vnet-to-vnet-considerations-for-classic-vnets"></a><a name="faq"></a>Klasik VNet'ler için VNet'e vnet'e değerlendirmeler
* Sanal ağlar aynı veya farklı aboneliklerde olabilir.
* Sanal ağlar aynı ya da farklı Azure bölgelerinde (konumlarında) bulunabilir.
* Bir bulut hizmeti veya yük dengeleme uç noktası, birbirine bağlı olsalar bile sanal ağlar arasında yayılmaz.
* Birden çok sanal ağı birbirine bağlamak için vpn aygıtı gerekmez.
* VNet'den VNet'e Azure Sanal Ağları bağlamayı destekler. Sanal ağa dağıtılmayan sanal makinelerin veya bulut hizmetlerinin bağlanmasını desteklemez.
* VNet'ten VNet'e dinamik yönlendirme ağ geçitleri gerekir. Azure statik yönlendirme ağ geçitleri desteklenmez.
* Sanal ağ bağlantısı, çoklu site VPN’leri ile eşzamanlı olarak kullanılabilir. Diğer sanal ağlara veya şirket içi sitelere bağlanan sanal ağ VPN ağ geçidi için en fazla 10 VPN tüneli vardır.
* Sanal ağlar ve şirket içi yerel ağ sitelerinin adres alanları çakışmamalıdır. Çakışan adres boşlukları sanal ağların oluşturulmasına veya netcfg yapılandırma dosyalarının yüklenmesinin başarısız lığa neden olur.
* Bir çift sanal ağ arasındaki yedekli tüneller desteklenmez.
* P2S VPN'ler de dahil olmak üzere VNet için tüm VPN tünelleri, VPN ağ geçidi için kullanılabilir bant genişliğini ve Azure'da aynı VPN ağ geçidi çalışma süresi SLA'yı paylaşır.
* VNet'ten VNet'e trafik Azure omurgası boyunca ilerlerken.

## <a name="next-steps"></a>Sonraki adımlar
Bağlantılarınızı doğrulayın. Bkz. [VPN Ağ Geçidi bağlantısını doğrula.](vpn-gateway-verify-connection-resource-manager.md)
