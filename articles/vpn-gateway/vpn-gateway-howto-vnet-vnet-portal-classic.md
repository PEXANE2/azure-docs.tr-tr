---
title: 'Sanal ağlar arasında bağlantı oluşturma: klasik: Azure portal'
description: PowerShell ve Azure portal kullanarak Azure sanal ağlarını birbirine bağlayın.
services: vpn-gateway
titleSuffix: Azure VPN Gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: article
ms.date: 02/12/2020
ms.author: cherylmc
ms.openlocfilehash: 63c6329ad62289cd127902c1438073b28fc8683e
ms.sourcegitcommit: 333af18fa9e4c2b376fa9aeb8f7941f1b331c11d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2020
ms.locfileid: "77201858"
---
# <a name="configure-a-vnet-to-vnet-connection-classic"></a>VNet 'ten VNet 'e bağlantı yapılandırma (klasik)

[!INCLUDE [deployment models](../../includes/vpn-gateway-classic-deployment-model-include.md)]

Bu makale, sanal ağlar arasında bir VPN Gateway bağlantısı oluşturmanıza yardımcı olur. Sanal ağlar aynı ya da farklı bölgelerde ve aynı ya da farklı aboneliklerde bulunuyor olabilirler. Bu makaledeki adımlar, klasik dağıtım modeli ve Azure portal için geçerlidir. Ayrıca aşağıdaki listeden farklı bir seçenek belirtip farklı bir dağıtım aracı veya dağıtım modeli kullanarak da bu yapılandırmayı oluşturabilirsiniz:

> [!div class="op_single_selector"]
> * [Azure Portal](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md)
> * [PowerShell](vpn-gateway-vnet-vnet-rm-ps.md)
> * [Azure CLI](vpn-gateway-howto-vnet-vnet-cli.md)
> * [Azure portal (klasik)](vpn-gateway-howto-vnet-vnet-portal-classic.md)
> * [Farklı dağıtım modellerini bağlama - Azure portalı](vpn-gateway-connect-different-deployment-models-portal.md)
> * [Farklı dağıtım modellerini bağlama - PowerShell](vpn-gateway-connect-different-deployment-models-powershell.md)
>
>

![VNet 'ten VNet 'e bağlantı diyagramı](./media/vpn-gateway-howto-vnet-vnet-portal-classic/v2vclassic.png)



## <a name="about-vnet-to-vnet-connections"></a>Sanal Ağdan Sanal Ağa bağlantıları hakkında

VPN ağ geçidi kullanarak, bir sanal ağı klasik dağıtım modelinde başka bir sanal ağa (VNet-VNet) bağlamak, sanal bir ağı şirket içi site konumuna bağlamaya benzer. Her iki bağlantı türü de IPsec/IKE kullanarak güvenli bir tünel sunmak üzere bir VPN ağ geçidi kullanır.

Bağlandığınız sanal ağlar farklı aboneliklerde ve farklı bölgelerde olabilir. VNet 'ten VNet 'e iletişimi çok siteli yapılandırmalara sahip bir şekilde birleştirebilirsiniz. Bu özellik şirket içi ve şirket dışı bağlantıyla ağ içi bağlantıyı birleştiren ağ topolojileri kurabilmenize olanak sağlar.

![VNet 'ten VNet 'e bağlantılar](./media/vpn-gateway-howto-vnet-vnet-portal-classic/aboutconnections.png)

### <a name="why"></a>Sanal ağları neden bağlamalıyız?

Sanal ağları aşağıdaki sebeplerden dolayı bağlamak isteyebilirsiniz:

* **Çapraz bölge coğrafi artıklığı ve coğrafi-durum**

  * Kendi coğrafi çoğaltma veya eşitlemenizi, güvenli bağlantıyla İnternet’te uç noktalara gitmeden ayarlayabilirsiniz.
  * Azure Load Balancer ve Microsoft ya da üçüncü taraf kümeleme teknolojisi sayesinde, birden fazla Azure bölgesinde coğrafi yedeklilik ile yüksek oranda kullanılabilir iş yükü ayarlayabilirsiniz. Buna önemli bir örnek olarak SQL Always On ile birden fazla Azure bölgesine yayılan Kullanılabilirlik Grupları’nı birlikte kurmak verilebilir.
* **Güçlü yalıtım sınırı olan bölgesel çok katmanlı uygulamalar**

  * Aynı bölge içinde çok katmanlı uygulamaları, güçlü yalıtımlı ve güvenli katmanlı iletişimlerle birbirine bağlı birden fazla VNET ile birlikte ayarlayabilirsiniz.
* **Azure 'da çapraz abonelik, şirket içi iletişim**

  * Birden çok Azure aboneliğiniz varsa, sanal ağlar arasında güvenli bir şekilde farklı aboneliklerden iş yüklerini birbirine bağlayabilirsiniz.
  * Kuruluşlar veya hizmet sağlayıcıları için Azure 'da güvenli VPN teknolojisiyle platformlar arası iletişimi etkinleştirebilirsiniz.

Sanal ağlar arası bağlantılar hakkında daha fazla bilgi için bu makalenin sonunda yer alan [Sanal ağdan sanal ağa dikkat edilecek noktalar](#faq) bölümünü inceleyin.

### <a name="powershell"></a>Azure PowerShell çalışma

Çoğu adım için portalı kullanıyoruz, ancak sanal ağlar arasındaki bağlantıları oluşturmak için PowerShell kullanmanız gerekir. Azure portal kullanarak bağlantı oluşturamazsınız. [!INCLUDE [vpn-gateway-classic-powershell](../../includes/vpn-gateway-powershell-classic-locally.md)]

## <a name="plan"></a>1. adım -, IP adres aralıklarını planlama

Sanal ağlarınızı yapılandırmak için kullanacağınız aralıklara karar vermeniz önemlidir. Bu yapılandırma için, VNet aralıklarından hiçbirinin birbirleriyle veya bağlandıkları yerel ağlarla çakışdıklarından emin olmanız gerekir.

Aşağıdaki tabloda, sanal ağlarınızı nasıl tanımlayacağınızı gösteren bir örnek gösterilmektedir. Aralıkları yalnızca bir kılavuz olarak kullanın. Sanal ağlarınız için aralıkları yazın. Sonraki adımlar için bu bilgilere ihtiyacınız vardır.

**Örnek**

| Sanal Ağ | Adres Alanı | Bölge | Yerel ağ sitesine bağlanır |
|:--- |:--- |:--- |:--- |
| TestVNet1 |TestVNet1<br>(10.11.0.0/16)<br>(10.12.0.0/16) |Doğu ABD |VNet4Local<br>(10.41.0.0/16)<br>(10.42.0.0/16) |
| TestVNet4 |TestVNet4<br>(10.41.0.0/16)<br>(10.42.0.0/16) |Batı ABD |VNet1Local<br>(10.11.0.0/16)<br>(10.12.0.0/16) |

## <a name="vnetvalues"></a>2. adım-sanal ağları oluşturma

[Azure Portal](https://portal.azure.com)iki sanal ağ oluşturun. Klasik sanal ağlar oluşturma adımları için bkz. [Klasik sanal ağ oluşturma](../virtual-network/virtual-networks-create-vnet-classic-pportal.md). 

Klasik bir sanal ağ oluşturmak için portalını kullanırken, aşağıdaki adımları kullanarak sanal ağ sayfasına gitmeniz gerekir, aksi takdirde klasik bir sanal ağ oluşturma seçeneği görünmez:

1. ' + ' Düğmesine tıklayarak ' yeni ' sayfasını açın.
2. ' Market 'te ara ' alanına ' sanal ağ ' yazın. Bunun yerine ağ-> sanal ağı ' nı seçerseniz, klasik VNet oluşturma seçeneğini kullanamazsınız.
3. Döndürülen listeden ' sanal ağ ' öğesini bulun ve sanal ağ sayfasını açmak için tıklayın. 
4. Sanal ağ sayfasında, klasik VNet oluşturmak için ' klasik ' seçeneğini belirleyin. 

Bu makaleyi bir alıştırma olarak kullanıyorsanız, aşağıdaki örnek değerleri kullanabilirsiniz:

**TestVNet1 için değerler**

Ad: TestVNet1<br>
Adres alanı: 10.11.0.0/16, 10.12.0.0/16 (isteğe bağlı)<br>
Alt ağ adı: varsayılan<br>
Alt ağ adres aralığı: 10.11.0.1/24<br>
Kaynak grubu: ClassicRG<br>
Location: East US<br>
GatewaySubnet: 10.11.1.0/27

**TestVNet4 için değerler**

Ad: TestVNet4<br>
Adres alanı: 10.41.0.0/16, 10.42.0.0/16 (isteğe bağlı)<br>
Alt ağ adı: varsayılan<br>
Alt ağ adres aralığı: 10.41.0.1/24<br>
Kaynak grubu: ClassicRG<br>
Location: West US<br>
GatewaySubnet: 10.41.1.0/27

**Sanal ağlarınızı oluştururken aşağıdaki ayarları aklınızda bulundurun:**

* **Sanal ağ adres alanları** – sanal ağ adres alanları sayfasında, sanal ağınız için kullanmak istediğiniz adres aralığını belirtin. Bunlar, bu sanal ağa dağıttığınız VM 'lere ve diğer rol örneklerine atanacak dinamik IP adresleridir.<br>Seçtiğiniz adres alanları, bu VNet 'in bağlanacağı diğer sanal ağlar veya şirket içi konumların herhangi biri için adres alanları ile çakışamaz.

* **Konum** – bir sanal ağ oluşturduğunuzda, bunu bir Azure konumuyla (bölge) ilişkilendirirsiniz. Örneğin, sanal ağınıza dağıtılan VM 'lerinin fiziksel olarak Batı ABD ' de bulunmasını istiyorsanız, bu konumu seçin. Oluşturduktan sonra sanal ağınızla ilişkili konumu değiştiremezsiniz.

**Sanal ağlarınızı oluşturduktan sonra aşağıdaki ayarları ekleyebilirsiniz:**

* **Adres alanı** – bu yapılandırma için ek adres alanı gerekli değildir, ancak sanal ağı oluşturduktan sonra ek adres alanı ekleyebilirsiniz.

* **Alt ağlar** – bu yapılandırma için ek alt ağlar gerekli değildir, ancak VM 'lerinizi diğer rol örneklerinizden ayrı bir alt ağda kullanmak isteyebilirsiniz.

* **DNS sunucuları** : DNS sunucusu adını ve IP adresini girin. Bu ayarla bir DNS sunucusu oluşturulmaz. Söz konusu ayar, bu sanal ağa ilişkin ad çözümlemesi için kullanmak istediğiniz DNS sunucularını belirtmenize olanak sağlar.

Bu bölümde, bağlantı türünü, yerel siteyi yapılandırır ve ağ geçidini oluşturursunuz.

## <a name="localsite"></a>3. adım-yerel siteyi yapılandırma

Azure, sanal ağlar arasında trafiğin nasıl yönlendirileceğini anlamak için her yerel ağ sitesinde belirtilen ayarları kullanır. Her VNet, trafiği yönlendirmek istediğiniz ilgili yerel ağı işaret etmelidir. Her yerel ağ sitesine başvurmak için kullanmak istediğiniz adı belirlersiniz. Açıklayıcı bir açıklama kullanmak en iyisidir.

Örneğin, TestVNet1 ' VNet4Local ' adında oluşturduğunuz bir yerel ağ sitesine bağlanır. VNet4Local ayarları, TestVNet4 için adres ön eklerini içerir.

Her VNet için yerel site diğer VNet 'dir. Yapılandırma için aşağıdaki örnek değerler kullanılır:

| Sanal Ağ | Adres Alanı | Bölge | Yerel ağ sitesine bağlanır |
|:--- |:--- |:--- |:--- |
| TestVNet1 |TestVNet1<br>(10.11.0.0/16)<br>(10.12.0.0/16) |Doğu ABD |VNet4Local<br>(10.41.0.0/16)<br>(10.42.0.0/16) |
| TestVNet4 |TestVNet4<br>(10.41.0.0/16)<br>(10.42.0.0/16) |Batı ABD |VNet1Local<br>(10.11.0.0/16)<br>(10.12.0.0/16) |

1. Azure portal TestVNet1 bulun. Sayfanın **VPN bağlantıları** bölümünde **ağ geçidi**' ne tıklayın.

    ![Ağ geçidi yok](./media/vpn-gateway-howto-vnet-vnet-portal-classic/nogateway.png)
2. **Yenı VPN bağlantısı** sayfasında **siteden siteye**' yı seçin.
3. Yerel **site ' ye** tıklayarak yerel site sayfasını açın ve ayarları yapılandırın.
4. **Yerel site** sayfasında, Yerel sitenizi adlandırın. Örneğimizde, ' VNet4Local ' yerel sitesini adlandırın.
5. **VPN ağ GEÇIDI IP adresi**için, istediğiniz HERHANGI bir IP adresini, geçerli bir biçimde olduğu sürece kullanabilirsiniz. Genellikle, bir VPN cihazının gerçek dış IP adresini kullanırsınız. Ancak, klasik VNet 'ten VNet 'e yönelik yapılandırma için sanal ağınız için ağ geçidine atanan genel IP adresini kullanırsınız. Sanal ağ geçidini henüz oluşturmadıysanız, geçerli bir genel IP adresini yer tutucu olarak belirtirsiniz.<br>Bu alanı boş bırakmayın; bu yapılandırma için isteğe bağlı değildir. Daha sonraki bir adımda, bu ayarlara geri dönerek Azure tarafından oluşturulduktan sonra karşılık gelen sanal ağ geçidi IP adresleriyle yapılandırırsınız.
6. **Istemci adres alanı**için, diğer sanal ağın adres alanını kullanın. Planlama örneğinizi inceleyin. Ayarlarınızı kaydetmek ve **yenı VPN bağlantısı** sayfasına geri dönmek için **Tamam** ' ı tıklatın.

    ![yerel site](./media/vpn-gateway-howto-vnet-vnet-portal-classic/localsite.png)

## <a name="gw"></a>4. adım-sanal ağ geçidini oluşturma

Her sanal ağın bir sanal ağ geçidine sahip olması gerekir. Sanal ağ geçidi, trafiği yönlendirir ve şifreler.

1. **Yeni VPN Bağlantısı** sayfasında, **Ağ geçidini hemen oluştur** onay kutusunu işaretleyin.
2. **Alt ağ, boyut ve yönlendirme türü**' ne tıklayın. **Ağ geçidi yapılandırması** sayfasında **alt ağ**' a tıklayın.
3. Ağ geçidi alt ağ adı, gereken ' GatewaySubnet ' adıyla otomatik olarak doldurulur. **Adres aralığı** , VPN Gateway hIzmetlerIne ayrılan IP adreslerini içerir. Bazı yapılandırmalara/29 ağ geçidi alt ağına izin verir, ancak ağ geçidi Hizmetleri için daha fazla IP adresi gerektirebilecek gelecek yapılandırmalara uyum sağlamak için bir/28 veya/27 kullanmak en iyisidir. Örnek ayarlarımızda 10.11.1.0/27 kullanıyoruz. Adres alanını ayarlayın ve ardından **Tamam**' a tıklayın.
4. **Ağ geçidi boyutunu**yapılandırın. Bu ayar [ağ geçidi SKU 'su](vpn-gateway-about-vpn-gateway-settings.md#gwsku)anlamına gelir.
5. **Yönlendirme türünü**yapılandırın. Bu yapılandırma için yönlendirme türü **dinamik**olmalıdır. Ağ geçidini koparmadığınız ve yeni bir tane oluşturmadığınız müddetçe yönlendirme türünü daha sonra değiştiremezsiniz.
6. **Tamam**'a tıklayın.
7. **Yenı VPN bağlantısı** sayfasında, sanal ağ geçidini oluşturmaya başlamak için **Tamam** ' ı tıklatın. Bir ağ geçidinin oluşturulması, seçili ağ geçidi SKU’suna bağlı olarak 45 dakika veya daha uzun sürebilir.

## <a name="vnet4settings"></a>5. adım-TestVNet4 ayarlarını yapılandırma

[Bir yerel site oluşturmak](#localsite) için adımları yineleyin ve TestVNet4 yapılandırmak için [sanal ağ geçidini oluşturun](#gw) , gerekirse değerleri değiştirin. Bunu bir alıştırma olarak yapıyorsanız [örnek değerleri](#vnetvalues)kullanın.

## <a name="updatelocal"></a>6. adım-yerel siteleri güncelleştirme

Sanal ağ geçitleriniz her iki VNET için de oluşturulduktan sonra, yerel siteler **VPN ağ GEÇIDI IP adresi** değerlerini ayarlamanız gerekir.

|VNet adı|Bağlı site|Ağ geçidi IP adresi|
|:--- |:--- |:--- |
|TestVNet1|VNet4Local|TestVNet4 için VPN Gateway IP adresi|
|TestVNet4|VNet1Local|TestVNet1 için VPN Gateway IP adresi|

### <a name="part-1---get-the-virtual-network-gateway-public-ip-address"></a>1\. Bölüm-sanal ağ geçidi genel IP adresini al

1. Azure portal Sanal ağınızı bulun.
2. VNet **genel bakış** sayfasını açmak için tıklayın. Sayfada, **VPN bağlantıları**' nda, sanal ağ GEÇIDINIZIN IP adresini görüntüleyebilirsiniz.

   ![Genel IP](./media/vpn-gateway-howto-vnet-vnet-portal-classic/publicIP.png)
3. IP adresini kopyalayın. Bunu bir sonraki bölümde kullanacaksınız.
4. TestVNet4 için bu adımları yineleyin

### <a name="part-2---modify-the-local-sites"></a>Bölüm 2-yerel siteleri değiştirme

1. Azure portal Sanal ağınızı bulun.
2. VNet **genel bakış** sayfasında, yerel siteye tıklayın.

   ![Yerel site oluşturuldu](./media/vpn-gateway-howto-vnet-vnet-portal-classic/local.png)
3. **Siteden sıteye VPN bağlantıları** sayfasında, değiştirmek istediğiniz yerel sitenin adına tıklayın.

   ![Yerel siteyi aç](./media/vpn-gateway-howto-vnet-vnet-portal-classic/openlocal.png)
4. Değiştirmek istediğiniz **yerel siteye** tıklayın.

   ![Siteyi değiştir](./media/vpn-gateway-howto-vnet-vnet-portal-classic/connections.png)
5. **VPN ağ GEÇIDI IP adresini** güncelleştirin ve **Tamam** ' a tıklayarak ayarları kaydedin.

   ![ağ geçidi IP 'si](./media/vpn-gateway-howto-vnet-vnet-portal-classic/gwupdate.png)
6. Diğer sayfaları kapatın.
7. TestVNet4 için bu adımları tekrarlayın.

## <a name="getvalues"></a>7. adım-ağ yapılandırma dosyasından değerleri alma

Azure portal klasik VNET oluştururken, görüntülediğiniz ad PowerShell için kullandığınız tam ad değildir. Örneğin, portalda **TestVNet1** adında görünen bir VNet, ağ yapılandırma dosyasında çok daha uzun bir ada sahip olabilir. Ad şöyle görünebilir: **Grup ClassicRG TestVNet1**. Bağlantılarınızı oluştururken, ağ yapılandırma dosyasında gördüğünüz değerlerin kullanılması önemlidir.

Aşağıdaki adımlarda, Azure hesabınıza bağlanır ve bağlantılarınız için gerekli olan değerleri almak için ağ yapılandırma dosyasını indirmeniz ve görüntülemeniz gerekir.

1. Azure hizmet yönetimi (SM) PowerShell cmdlet 'lerinin en son sürümünü indirip yükleyin. Daha fazla bilgi için bkz. [Azure PowerShell çalışma](#powershell).

2. PowerShell konsolunuzu yükseltilmiş haklarla açın. Bağlanmanıza yardımcı olması için aşağıdaki örnekleri kullanın. PowerShell hizmeti yönetim modülünü kullanarak bu komutları yerel olarak çalıştırmanız gerekir. Hizmet yönetimine geçiş yapmak için şu komutu kullanın:

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
6. Ağ yapılandırma dosyasını dışa aktarın ve görüntüleyin. Bilgisayarınızda bir dizin oluşturun ve sonra ağ yapılandırma dosyasını dizine aktarın. Bu örnekte, ağ yapılandırma dosyası **C:\azurenet dizinine**aktarılmalıdır.

   ```powershell
   Get-AzureVNetConfig -ExportToFile C:\AzureNet\NetworkConfig.xml
   ```
7. Dosyayı bir metin düzenleyici ile açın ve sanal ağlarınız ve sitelerinizin adlarını görüntüleyin. Bu adlar, bağlantılarınızı oluştururken kullandığınız adlar olacaktır.<br>VNet adları **Virtualnetworksite Name =** olarak listelenir.<br>Site adları **LocalNetworkSiteRef Name =** olarak listelenir

## <a name="createconnections"></a>8. adım-VPN Gateway bağlantılarını oluşturma

Tüm önceki adımlar tamamlandığında, IPSec/ıKE önceden paylaşılan anahtarlarını ayarlayabilir ve bağlantıyı oluşturabilirsiniz. Bu adım kümesi PowerShell kullanır. Klasik dağıtım modeli için VNet-VNet bağlantıları Azure portal yapılandırılamaz.

Örneklerde, paylaşılan anahtarın tam olarak aynı olduğuna dikkat edin. Paylaşılan anahtarın her zaman eşleşmesi gerekir. Bu örneklerdeki değerleri sanal ağlar ve yerel ağ siteleriniz için tam adlarla değiştirdiğinizden emin olun.

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
3. Bağlantıların başlatılmasını bekleyin. Ağ Geçidi başlatıldıktan sonra durum ' başarılı ' olur.

   ```
   Error          :
   HttpStatusCode : OK
   Id             :
   Status         : Successful
   RequestId      :
   StatusCode     : OK
   ```

## <a name="faq"></a>Klasik sanal ağlar için sanal ağdan sanal ağa dikkat edilecek noktalar
* Sanal ağlar aynı veya farklı aboneliklerde olabilir.
* Sanal ağlar aynı ya da farklı Azure bölgelerinde (konumlarında) bulunabilir.
* Bir bulut hizmeti veya yük dengeleme uç noktası, birbirine bağlı olsalar dahi sanal ağlar arasında yayılamaz.
* Birden çok sanal ağı birbirine bağlamak için herhangi bir VPN cihazı gerekmez.
* VNet 'ten VNet 'e Azure sanal ağlarını bağlamayı destekler. Bir sanal ağa dağıtılan sanal makinelerin veya bulut hizmetlerinin bağlanmasını desteklemez.
* VNet 'ten VNet 'e dinamik yönlendirme ağ geçitleri gerekir. Azure statik yönlendirme ağ geçitleri desteklenmez.
* Sanal ağ bağlantısı, çoklu site VPN’leri ile eşzamanlı olarak kullanılabilir. Diğer sanal ağlara veya şirket içi sitelere bağlanan bir sanal ağ VPN ağ geçidi için en fazla 10 VPN tüneli vardır.
* Sanal ağlar ve şirket içi yerel ağ sitelerinin adres alanları çakışmamalıdır. Çakışan adres alanları, sanal ağların oluşturulmasına veya netcfg yapılandırma dosyalarının başarısız olmasına neden olur.
* Bir çift sanal ağ arasındaki yedekli tüneller desteklenmez.
* VNet için P2S VPN 'Ler de dahil olmak üzere tüm VPN tünelleri, VPN Gateway için kullanılabilir bant genişliğini ve Azure 'da aynı VPN ağ geçidi çalışma süresi SLA 'sını paylaşır.
* VNet-VNet trafiği Azure omurgası üzerinden dolaşır.

## <a name="next-steps"></a>Sonraki adımlar
Bağlantılarınızı doğrulayın. Bkz. [VPN Gateway bağlantısını doğrulama](vpn-gateway-verify-connection-resource-manager.md).
