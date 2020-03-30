---
title: Klasik sanal ağları Azure Kaynak Yöneticisi VNets:Portal'a bağlayın | Microsoft Dokümanlar
description: VPN Ağ Geçidi ve portalı kullanarak klasik VNet'leri Kaynak Yöneticisi VNets'e bağlama adımları
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 09/24/2019
ms.author: cherylmc
ms.openlocfilehash: 5e64cb2db2bd16a881334779a1c6f1ef19296da2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77152032"
---
# <a name="connect-virtual-networks-from-different-deployment-models-using-the-portal"></a>Portalı kullanarak farklı dağıtım modellerinden sanal ağları bağlama

Bu makalede, ayrı dağıtım modellerinde bulunan kaynakların birbirleriyle iletişim kurmasına izin vermek için klasik VNet'leri Kaynak Yöneticisi VNet'lere nasıl bağlayabileceğinizi gösterilmektedir. Bu makaledeki adımlar öncelikle Azure portalını kullanır, ancak bu listeden makaleyi seçerek PowerShell'i kullanarak da bu yapılandırmayı oluşturabilirsiniz.

> [!div class="op_single_selector"]
> * [Portal](vpn-gateway-connect-different-deployment-models-portal.md)
> * [Powershell](vpn-gateway-connect-different-deployment-models-powershell.md)
> 
> 

Klasik bir VNet'i Kaynak Yöneticisi VNet'e bağlamak, bir VNet'i şirket içi bir konuma bağlamaya benzer. Her iki bağlantı türü de IPsec/IKE kullanarak güvenli bir tünel sunmak üzere bir VPN ağ geçidi kullanır. Farklı aboneliklerde ve farklı bölgelerde bulunan VNet'ler arasında bağlantı oluşturabilirsiniz. Ayrıca, yapılandırıldıkları ağ geçidi dinamik veya rota tabanlı olduğu sürece, şirket içi ağlara zaten bağlantısı olan VNet'leri de bağlayabilirsiniz. Sanal ağlar arası bağlantılar hakkında daha fazla bilgi için bu makalenin sonunda yer alan [Sanal ağlar arası bağlantılar hakkında SSS](#faq) bölümünü inceleyin. 

Zaten sanal bir ağ ağ ağ geçidiniz yoksa ve bir ağ oluşturmak istemiyorsanız, bunun yerine VNet'lerinizi VNet Peering kullanarak bağlamayı düşünebilirsiniz. VNet eşlemesi VPN ağ geçidini kullanmaz. Daha fazla bilgi için bkz. [VNet eşlemesi](../virtual-network/virtual-network-peering-overview.md).

### <a name="before-you-begin"></a><a name="before"></a>Başlamadan önce



* Bu adımlar, her iki VNet'in de zaten oluşturulduğunu varsayar. Bu makaleyi bir alıştırma olarak kullanıyorsanız ve VNet'inizi yoksa, bunları oluşturmanıza yardımcı olacak adımlarda bağlantılar vardır.
* VNet'lerin adres aralıklarının birbiriyle çakışmadığını veya ağ geçitlerinin bağlanabileceği diğer bağlantılar için aralıklarla çakışmadığını doğrulayın.
* Hem Kaynak Yöneticisi hem de Servis Yönetimi (klasik) için en son PowerShell cmdlet'lerini yükleyin. Bu makalede, hem Azure portalını hem de PowerShell'i kullanıyoruz. PowerShell'in klasik VNet'ten Resource Manager VNet'e bağlantı oluşturması gerekir. Daha fazla bilgi için bkz. [Azure PowerShell’i yükleme ve yapılandırma](/powershell/azure/overview). 

### <a name="example-settings"></a><a name="values"></a>Örnek ayarlar

Bu değerleri kullanarak bir test ortamı oluşturabilir veya bu makaledeki örnekleri daha iyi anlamak için bunlara bakabilirsiniz.

**Klasik VNet**

VNet adı = ClassicVNet <br>
Adres alanı = 10.0.0.0/24 <br>
Alt net adı = Subnet-1 <br>
Alt net adres aralığı = 10.0.0.0/27 <br>
Abonelik = kullanmak istediğiniz abonelik <br>
Kaynak Grubu = ClassicRG <br>
Yer = Batı ABD <br>
Ağ Geçidi Subnet = 10.0.0.32/28 <br>
Yerel site = RMVNetYerel <br>

**Kaynak Yöneticisi VNet**

VNet adı = RMVNet <br>
Adres alanı = 192.168.0.0/16 <br>
Kaynak Grubu = RG1 <br>
Yer = Doğu ABD <br>
Alt net adı = Subnet-1 <br>
Adres aralığı = 192.168.1.0/24 <br>
Ağ Geçidi Subnet = 192.168.0.0/26 <br>
Sanal ağ ağ geçidi adı = RMGateway <br>
Ağ geçidi türü = VPN <br>
VPN türü = Rota tabanlı <br>
SKU = VpnGw1 <br>
Yer = Doğu ABD <br>
Sanal ağ = RMVNet <br> (vpn ağ geçidini bu VNet'e ilişkilendirin) İlk IP yapılandırması = rmgwpip <br> (ağ geçidi genel IP adresi) Yerel ağ ağ geçidi = ClassicVNetLocal <br>
Bağlantı adı = RMtoClassic

### <a name="connection-overview"></a><a name="connectoverview"></a>Bağlantıya genel bakış

Bu yapılandırma için, sanal ağlar arasında bir IPsec/IKE VPN tüneli üzerinden bir VPN ağ geçidi bağlantısı oluşturursunuz. VNet aralıklarınızın hiçbirinin birbiriyle veya bağlandıkları yerel ağlarla örtüşmediğinden emin olun.

Aşağıdaki tablo, örnek VNets'lerin ve yerel sitelerin nasıl tanımlandığına bir örnek gösterilmektedir:

| Sanal Ağ | Adres Alanı | Bölge | Yerel ağ sitesine bağlanır |
|:--- |:--- |:--- |:--- |
| ClassicVNet |(10.0.0.0/24) |Batı ABD | RMVNetYerel (192.168.0.0/16) |
| RMVNet | (192.168.0.0/16) |Doğu ABD |ClassicVNetLocal (10.0.0.0/24) |

## <a name="section-1---configure-the-classic-vnet-settings"></a><a name="classicvnet"></a>Bölüm 1 - Klasik VNet ayarlarını yapılandırın

Bu bölümde, klasik VNet, yerel ağ (yerel site) ve sanal ağ ağ geçidi oluşturursunuz. Ekran görüntüleri örnek olarak verilmiştir. Değerleri kendi değerlerinizle değiştirdiğinizden veya [Örnek](#values) değerlerini kullandığınızdan emin olun.

### <a name="1-create-a-classic-vnet"></a>1. <a name="classicvnet"> </a>Klasik bir VNet oluşturun

Klasik bir VNet'iniz yoksa ve bu adımları bir alıştırma olarak çalıştırıyorsanız, [bu makaleyi](../virtual-network/virtual-networks-create-vnet-classic-pportal.md) ve yukarıdan [Örnek](#values) ayarları değerlerini kullanarak bir VNet oluşturabilirsiniz.

VPN ağ geçidine sahip bir VNet'iniz varsa, ağ geçidinin Dinamik olduğunu doğrulayın. Statik ise, [yerel siteyi Yapılandırmaya](#local)geçmeden önce VPN ağ geçidini silmeniz gerekir.

1. [Azure Portal](https://ms.portal.azure.com)'ı açın ve Azure hesabınızla oturum açın.
2. 'Yeni' sayfasını açmak için **+ Kaynak Oluştur'u** tıklatın.
3. 'Pazar yeri arama' alanına 'Sanal Ağ' yazın. Bunun yerine, Networking -> Virtual Network'ü seçin, klasik bir VNet oluşturma seçeneğine girmezsiniz.
4. Döndürülen listeden 'Sanal Ağ'ı bulun ve Sanal Ağ sayfasını açmak için tıklatın. 
5. Sanal ağ sayfasında, klasik bir VNet oluşturmak için 'Klasik' seçeneğini belirleyin. Varsayılanı burada alırsanız, bunun yerine bir Kaynak Yöneticisi VNet'i alırsınız.

### <a name="2-configure-the-local-site"></a>2. <a name="local"> </a>Yerel siteyi yapılandırın

1. Tüm **kaynaklara** gidin ve **listede ClassicVNet'i** bulun.
2. Menünün **Ayarlar** bölümünde **Ağ Geçidi'ni** tıklatın ve ardından ağ geçidi oluşturmak için banner'a tıklayın.
  ![VPN ağ geçidi yapılandırma](./media/vpn-gateway-connect-different-deployment-models-portal/gatewaygraphic.png "VPN ağ geçidi yapılandırma")
3. Yeni **VPN Bağlantısı** sayfasında, **Bağlantı türü**için **Siteden siteye'i**seçin.
4. **Yerel site için**gerekli ayarları **yapılandır'ı**tıklatın. Bu, **Yerel site** sayfasını açar.
5. Yerel **site** sayfasında, Kaynak Yöneticisi VNet'e başvurmak için bir ad oluşturun. Örneğin, 'RMVNetLocal'.
6. Kaynak Yöneticisi VNet'in VPN ağ geçidinde zaten bir Genel IP adresi varsa, **VPN ağ geçidi IP adresi** alanının değerini kullanın. Bu adımları bir alıştırma olarak yapıyorsanız veya Kaynak Yöneticisi VNet'iniz için henüz sanal ağ ağ ağ geçidiniz yoksa, yer tutucu IP adresi hazırlayabilirsiniz. Yer tutucu IP adresinin geçerli bir biçim kullandığından emin olun. Daha sonra, yer tutucu IP adresini Kaynak Yöneticisi sanal ağ ağ ağ geçidinin Genel IP adresiyle değiştirirsiniz.
7. **İstemci Adres Alanı**için, Kaynak Yöneticisi VNet için sanal ağ IP adres boşlukları için [değerleri](#connectoverview) kullanın. Bu ayar, Kaynak Yöneticisi sanal ağa yönlendirilen adres alanlarını belirtmek için kullanılır. Örnekte, RMVNet'in adres aralığı olan 192.168.0.0/16'yı kullanıyoruz.
8. Değerleri kaydetmek ve Yeni VPN **Bağlantısı** sayfasına dönmek için **Tamam'ı** tıklatın.

### <a name="3-create-the-virtual-network-gateway"></a><a name="classicgw"></a>3. Sanal ağ ağ geçidi oluşturma

1. Yeni **VPN Bağlantısı** sayfasında, **hemen ağ geçidi oluştur'u** seçin.
2. **İsteğe bağlı ağ geçidi yapılandırması**’na tıklayarak **Ağ geçidi yapılandırması** sayfasını açın.

   ![Ağ geçidi yapılandırma sayfasını aç](./media/vpn-gateway-connect-different-deployment-models-portal/optionalgatewayconfiguration.png "Ağ geçidi yapılandırma sayfasını aç")
3. Alt Ağ'ı tıklatın - **Alt ağ** ekle sayfasını açmak için gerekli **ayarları yapılandırın.** **Ad** zaten gerekli değerle yapılandırıldı: **GatewaySubnet**.
4. **Adres aralığı** ağ geçidi alt ağı aralığını ifade eder. /29 adres aralığı (3 adres) içeren bir ağ geçidi alt ağı oluşturabilirsiniz, ancak daha fazla IP adresi içeren bir ağ geçidi alt ağı oluşturmanızı öneririz. Bu, daha fazla kullanılabilir IP adresi gerektirebilecek gelecekteki yapılandırmaları barındırır. Mümkünse /27 veya /28 kullanın. Bu adımları bir alıştırma olarak kullanıyorsanız, [Örnek değerlere](#values)başvurabilirsiniz. Bu örnekiçin '10.0.0.32/28' kullanıyoruz. Ağ geçidi alt netini oluşturmak için **Tamam'ı** tıklatın.
5. Ağ **Geçidi yapılandırma** **sayfasında, Boyut** ağ geçidi SKU'yu ifade eder. VPN ağ geçidiniz için ağ geçidi SKU'yu seçin.
6. Yönlendirme **Türü** **Dinamik**olduğunu doğrulayın, ardından Yeni **VPN Bağlantısı** sayfasına dönmek için **Tamam'ı** tıklatın.
7. Yeni **VPN Bağlantısı** sayfasında, VPN ağ geçidinizi oluşturmaya başlamak için **Tamam'ı** tıklatın. Bir VPN ağ geçidinin oluşturulması 45 dakika sürebilir.

### <a name="4-copy-the-virtual-network-gateway-public-ip-address"></a><a name="ip"></a>4. Sanal ağ ağ geçidi Genel IP adresini kopyalama

Sanal ağ ağ geçidi oluşturulduktan sonra ağ geçidi IP adresini görüntüleyebilirsiniz. 

1. Klasik VNet'inize gidin ve **Genel Bakış'ı**tıklatın.
2. VPN bağlantıları sayfasını açmak için **VPN bağlantılarına** tıklayın. VPN bağlantıları sayfasında, Genel IP adresini görüntüleyebilirsiniz. Bu, sanal ağ ağ ağ geçidinize atanan Genel IP adresidir. IP adresini not alın. Kaynak Yöneticisi yerel ağ ağ ağ geçidi yapılandırma ayarlarınızla çalışırken sonraki adımlarda kullanırsınız. 
3. Ağ geçidi bağlantılarınızın durumunu görüntüleyebilirsiniz. Oluşturduğunuz yerel ağ sitesinin 'Bağlanma' olarak listelenmiş olduğuna dikkat edin. Bağlantılarınızı oluşturduktan sonra durum değişecektir. Durumu görüntülemeyi bitirdiğinizde bu sayfayı kapatabilirsiniz.

## <a name="section-2---configure-the-resource-manager-vnet-settings"></a><a name="rmvnet"></a>Bölüm 2 - Kaynak Yöneticisi VNet ayarlarını yapılandırın

Bu bölümde, Kaynak Yöneticisi VNet için sanal ağ ağ geçidi ni ve yerel ağ ağ geçidini oluşturursunuz. Ekran görüntüleri örnek olarak verilmiştir. Değerleri kendi değerlerinizle değiştirdiğinizden veya [Örnek](#values) değerlerini kullandığınızdan emin olun.

### <a name="1-create-a-virtual-network"></a>1. Sanal ağ oluşturma

**Örnek değerler:**

* VNet adı = RMVNet <br>
* Adres alanı = 192.168.0.0/16 <br>
* Kaynak Grubu = RG1 <br>
* Yer = Doğu ABD <br>
* Alt net adı = Subnet-1 <br>
* Adres aralığı = 192.168.1.0/24 <br>

Kaynak Yöneticisi VNet'iniz yoksa ve bu adımları alıştırma olarak çalıştırıyorsanız, örnek değerleri kullanarak sanal ağ oluşturma adımlarını içeren [bir sanal ağ](../virtual-network/quick-create-portal.md)oluşturun.

### <a name="2-create-a-virtual-network-gateway"></a><a name="creategw"></a>2. Sanal ağ ağ geçidi oluşturma

Bu adımda sanal ağınız için sanal ağ geçidi oluşturacaksınız. Bir ağ geçidinin oluşturulması, seçili ağ geçidi SKU’suna bağlı olarak 45 dakika veya daha uzun sürebilir.

[!INCLUDE [About gateway subnets](../../includes/vpn-gateway-about-gwsubnet-portal-include.md)]

**Örnek değerler:**

* Sanal ağ ağ geçidi adı = RMGateway <br>
* Ağ geçidi türü = VPN <br>
* VPN türü = Rota tabanlı <br>
* SKU = VpnGw1 <br>
* Yer = Doğu ABD <br>
* Sanal ağ = RMVNet <br>
* Ağ Geçidi Subnet = 192.168.0.0/26 <br>
* İlk IP yapılandırması = rmgwpip <br>

[!INCLUDE [vpn-gateway-add-gw-rm-portal](../../includes/vpn-gateway-add-gw-rm-portal-include.md)]

[!INCLUDE [vpn-gateway-no-nsg-include](../../includes/vpn-gateway-no-nsg-include.md)]

### <a name="3-create-a-local-network-gateway"></a><a name="createlng"></a>3. Yerel ağ ağ geçidi oluşturma

**Örnek değerler:** Yerel ağ ağ geçidi = ClassicVNetLocal

| Sanal Ağ | Adres Alanı | Bölge | Yerel ağ sitesine bağlanır |Ağ Geçidi Genel IP adresi|
|:--- |:--- |:--- |:--- |:--- |
| ClassicVNet |(10.0.0.0/24) |Batı ABD | RMVNetYerel (192.168.0.0/16) |ClassicVNet ağ geçidine atanan Genel IP adresi|
| RMVNet | (192.168.0.0/16) |Doğu ABD |ClassicVNetLocal (10.0.0.0/24) |RMVNet ağ geçidine atanan genel IP adresi.|

Yerel ağ ağ geçidi, adres aralığını ve klasik VNet'inizle ve sanal ağ ağ geçidiyle ilişkili Genel IP adresini belirtir. Bu adımları bir alıştırma olarak yapıyorsanız, Örnek değerlere bakın.

[!INCLUDE [vpn-gateway-add-lng-rm-portal](../../includes/vpn-gateway-add-lng-rm-portal-include.md)]

## <a name="section-3---modify-the-classic-vnet-local-site-settings"></a><a name="modifylng"></a>Bölüm 3 - Klasik VNet yerel site ayarlarını değiştirin

Bu bölümde, yerel site ayarlarını belirtirken kullandığınız yer tutucu IP adresini Kaynak Yöneticisi VPN ağ geçidi IP adresiyle değiştirirsiniz. Bu bölümde klasik (SM) PowerShell cmdlets kullanır.

1. Azure portalında klasik sanal ağa gidin.
2. Sanal ağınız için sayfada **Genel Bakış'ı**tıklatın.
3. VPN **bağlantıları** bölümünde, grafikte yerel sitenizin adını tıklatın.

   ![VPN bağlantıları](./media/vpn-gateway-connect-different-deployment-models-portal/vpnconnections.png "VPN Bağlantıları")
4. **Siteden siteye VPN bağlantıları** sayfasında, sitenin adını tıklatın.

   ![Site adı](./media/vpn-gateway-connect-different-deployment-models-portal/sitetosite3.png "Yerel site adı")
5. Yerel sitenizin bağlantı sayfasında, **Yerel site** sayfasını açmak için yerel sitenin adını tıklatın.

   ![Açık yerel alan](./media/vpn-gateway-connect-different-deployment-models-portal/openlocal.png "Yerel siteyi aç")
6. Yerel **site** sayfasında, **VPN ağ geçidi IP adresini** Kaynak Yöneticisi ağ geçidinin IP adresiyle değiştirin.

   ![Ağ geçidi-ip adresi](./media/vpn-gateway-connect-different-deployment-models-portal/gwipaddress.png "Ağ geçidi IP adresi")
7. IP adresini güncellemek için **Tamam'ı** tıklatın.

## <a name="section-4---create-resource-manager-to-classic-connection"></a><a name="RMtoclassic"></a>Bölüm 4 - Klasik bağlantıiçin Kaynak Yöneticisi oluşturma

Bu adımlarda, Azure portalını kullanarak Kaynak Yöneticisi VNet'ten klasik VNet'e olan bağlantıyı yapılandırabilirsiniz.

1. **Tüm kaynaklarda,** yerel ağ ağ geçidini bulun. Örneğimizde, yerel ağ ağ geçidi **ClassicVNetLocal'dır.**
2. **Yapılandırma'yı** tıklatın ve IP adresi değerinin klasik VNet'in VPN ağ geçidi olduğunu doğrulayın. Gerekirse güncelleştirin, ardından **Kaydet'i**tıklatın. Sayfayı kapatın.
3. **Tüm kaynaklarda,** yerel ağ ağ ağ geçidini tıklatın.
4. Bağlantılar sayfasını açmak için **Bağlantılar'ı** tıklatın.
5. **Bağlantılar** sayfasında bağlantı eklemek **+** için tıklatın.
6. Bağlantı **Ekle** sayfasında bağlantıyı adlandırın. Örneğin, 'RMtoClassic'.
7. **Siteden Siteye** zaten bu sayfada seçilir.
8. Bu siteyle ilişkilendirmek istediğiniz sanal ağ ağ ağ geçidini seçin.
9. Paylaşılan bir **anahtar**oluşturun. Bu anahtar, klasik VNet'ten Kaynak Yöneticisi VNet'e oluşturduğunuz bağlantıda da kullanılır. Anahtarı oluşturabilir veya bir tane oluşturabilirsiniz. Örneğimizde , 'abc123' kullanıyoruz, ancak daha karmaşık bir şey kullanabilirsiniz (ve kullanmalısınız).
10. Bağlantıyı oluşturmak için **Tamam'ı** tıklatın.

## <a name="section-5---create-classic-to-resource-manager-connection"></a><a name="classictoRM"></a>Bölüm 5 - Kaynak Yöneticisi bağlantısı için klasik oluşturma

Bu adımlarda, bağlantıyı klasik VNet'ten Kaynak Yöneticisi VNet'e yapılandırırsınız. Bu adımlar PowerShell gerektirir. Bu bağlantıyı portalda oluşturamazsınız. Hem klasik (SM) hem de Resource Manager (RM) PowerShell cmdlets'i indirip yüklediğinizden emin olun.

### <a name="1-connect-to-your-azure-account"></a>1. Azure hesabınıza bağlanın

PowerShell konsolunu yüksek haklara sahip olarak açın ve Azure hesabınızda oturum açın. Oturum açtıktan sonra hesap ayarlarınız Azure PowerShell tarafından kullanılabilecek şekilde indirilir. Kaynak Yöneticisi dağıtım modeli için Azure Hesabınız için giriş kimlik bilgilerini aşağıdaki cmdlet ister:

```powershell
Connect-AzAccount
```

Azure aboneliklerinizin bir listesini alın.

```powershell
Get-AzSubscription
```

Birden fazla aboneliğiniz varsa, kullanmak istediğiniz aboneliği belirtin.

```powershell
Select-AzSubscription -SubscriptionName "Name of subscription"
```

Ardından, klasik PowerShell cmdlets (Servis Yönetimi) kullanmak için giriş yapın. Klasik dağıtım modeli için Azure hesabınızı eklemek için aşağıdaki komutu kullanın:

```powershell
Add-AzureAccount
```

Aboneliklerinizin bir listesini alın. Azure modülü yüklemenize bağlı olarak, Hizmet Yönetimi cmdlets'i eklerken bu adım gerekebilir.

```powershell
Get-AzureSubscription
```

Birden fazla aboneliğiniz varsa, kullanmak istediğiniz aboneliği belirtin.

```powershell
Select-AzureSubscription -SubscriptionName "Name of subscription"
```

### <a name="2-view-the-network-configuration-file-values"></a>2. Ağ yapılandırma dosya değerlerini görüntüleme

Azure portalında bir VNet oluşturduğunuzda, Azure'un kullandığı tam ad Azure portalında görünmez. Örneğin, Azure portalında 'ClassicVNet' olarak adlandırılan bir VNet'in ağ yapılandırma dosyasında çok daha uzun bir adı olabilir. Adı gibi bir şey görünebilir: 'Grup ClassicRG ClassicVNet'. Bu adımlarda, ağ yapılandırma dosyasını karşıdan yükleyip değerleri görüntüleyebilirsiniz.

Bilgisayarınızda bir dizin oluşturun ve sonra ağ yapılandırma dosyasını dizine aktarın. Bu örnekte, ağ yapılandırma dosyası C:\AzureNet dizinine aktarılır.

```powershell
Get-AzureVNetConfig -ExportToFile C:\AzureNet\NetworkConfig.xml
```

Dosyayı bir metin düzenleyicisi ile açın ve klasik VNet'inizin adını görüntüleyin. PowerShell cmdlets'inizi çalıştırırken ağ yapılandırma dosyasındaki adları kullanın.

- VNet adları **VirtualNetworkSite adı** = olarak listelenir
- Site adları **LocalNetworkSite adı=** olarak listelenir

### <a name="3-create-the-connection"></a>3. Bağlantıyı oluşturma

Paylaşılan anahtarı ayarlayın ve klasik VNet'ten Kaynak Yöneticisi VNet'e bağlantı oluşturun. Paylaşılan anahtarı portalı kullanarak ayarlayamazsınız. PowerShell cmdlets'in klasik sürümünü kullanarak oturum açtığınızda bu adımları çalıştırdığınızdan emin olun. Bunu yapmak için **Ekle-AzureHesabı'nı**kullanın. Aksi takdirde, '-AzureVNetGatewayKey'i ayarlayamayacaktır.

- Bu örnekte- **VNetName,** ağ yapılandırma dosyanızda bulunan klasik VNet'in adıdır. 
- **-LocalNetworkSiteName,** ağ yapılandırma dosyanızda bulunduğu gibi yerel site için belirttiğiniz addır.
- **-SharedKey** oluşturduğunuz ve belirttiğiniz bir değerdir. Bu örnekiçin, *biz abc123*kullanılan, ancak daha karmaşık bir şey üretebilir. Önemli olan, burada belirttiğiniz değerin, Kaynak Yöneticinizi klasik bağlantıya oluştururken belirttiğiniz değerle aynı olması gerektiğidir.

```powershell
Set-AzureVNetGatewayKey -VNetName "Group ClassicRG ClassicVNet" `
-LocalNetworkSiteName "172B9E16_RMVNetLocal" -SharedKey abc123
```

## <a name="section-6---verify-your-connections"></a><a name="verify"></a>Bölüm 6 - Bağlantılarınızı doğrulayın

Azure portalını veya PowerShell'i kullanarak bağlantılarınızı doğrulayabilirsiniz. Doğrulama yaparken, bağlantı oluşturulurken bir veya iki dakika beklemeniz gerekebilir. Bir bağlantı başarılı olduğunda, bağlantı durumu 'Bağlanma'dan 'Bağlı'ya değişir.

### <a name="to-verify-the-connection-from-your-classic-vnet-to-your-resource-manager-vnet"></a>Klasik VNet'inizden Kaynak Yöneticisi VNet'inize olan bağlantıyı doğrulamak için

[!INCLUDE [vpn-gateway-verify-connection-azureportal-classic](../../includes/vpn-gateway-verify-connection-azureportal-classic-include.md)]

### <a name="to-verify-the-connection-from-your-resource-manager-vnet-to-your-classic-vnet"></a>Kaynak Yöneticisi VNet'inizden klasik VNet'inize olan bağlantıyı doğrulamak için

[!INCLUDE [vpn-gateway-verify-connection-portal-rm](../../includes/vpn-gateway-verify-connection-portal-rm-include.md)]

## <a name="vnet-to-vnet-faq"></a><a name="faq"></a>Sanal ağlar arası bağlantılar hakkında SSS

[!INCLUDE [vpn-gateway-vnet-vnet-faq](../../includes/vpn-gateway-faq-vnet-vnet-include.md)]
