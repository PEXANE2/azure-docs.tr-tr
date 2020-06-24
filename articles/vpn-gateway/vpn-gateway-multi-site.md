---
title: 'VPN Gateway kullanarak sanal ağı birden çok siteye bağlama: klasik'
description: VPN Gateway kullanarak birden çok yerel şirket içi siteyi klasik bir sanal ağa bağlayın.
services: vpn-gateway
titleSuffix: Azure VPN Gateway
author: yushwang
ms.service: vpn-gateway
ms.topic: how-to
ms.date: 02/11/2020
ms.author: yushwang
ms.openlocfilehash: 24c6d5272a97040889f80578ac55f6a895791ceb
ms.sourcegitcommit: 55b2bbbd47809b98c50709256885998af8b7d0c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84987593"
---
# <a name="add-a-site-to-site-connection-to-a-vnet-with-an-existing-vpn-gateway-connection-classic"></a>Mevcut bir VPN Ağ Geçidi bağlantısı ile bir VNet 'e siteden siteye bağlantı ekleme (klasik)

[!INCLUDE [deployment models](../../includes/vpn-gateway-classic-deployment-model-include.md)]

> [!div class="op_single_selector"]
> * [Azure portal](vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md)
> * [PowerShell (klasik)](vpn-gateway-multi-site.md)
>
>

Bu makalede, PowerShell kullanarak var olan bağlantısı olan bir VPN ağ geçidine siteden siteye (S2S) bağlantıları ekleme işlemi gösterilmektedir. Bu bağlantı türü genellikle "çok siteli" bir yapılandırma olarak adlandırılır. Bu makaledeki adımlar, klasik dağıtım modeli (hizmet yönetimi olarak da bilinir) kullanılarak oluşturulan sanal ağlar için geçerlidir. Bu adımlar, ExpressRoute/siteden siteye birlikte var olan bağlantı yapılandırmalarına yönelik olarak geçerli değildir.

### <a name="deployment-models-and-methods"></a>Dağıtım modelleri ve yöntemleri

[!INCLUDE [vpn-gateway-classic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

Bu tablo, bu yapılandırma için yeni makaleler ve ek araçlar kullanılabilir hale geldiğinde güncelleştiririz. Bir makale kullanılabildiğinde, bu tablodan doğrudan buna bağlantı veriyoruz.

[!INCLUDE [vpn-gateway-table-multi-site](../../includes/vpn-gateway-table-multisite-include.md)]

## <a name="about-connecting"></a>Bağlanma hakkında

Birden çok şirket içi siteyi tek bir sanal ağa bağlayabilirsiniz. Bu, karma bulut çözümleri oluşturmak için özellikle çekici bir çözümdür. Azure sanal ağ geçidinize çok siteli bir bağlantı oluşturmak, diğer siteden siteye bağlantılar oluşturmaya benzer. Aslında, ağ geçidi dinamik olduğu sürece (rota tabanlı) mevcut bir Azure VPN ağ geçidini kullanabilirsiniz.

Sanal ağınıza bağlı bir statik ağ geçidiniz zaten varsa, birden çok siteye uyum sağlamak için sanal ağı yeniden oluşturmaya gerek kalmadan ağ geçidi türünü dinamik olarak değiştirebilirsiniz. Yönlendirme türünü değiştirmeden önce, şirket içi VPN ağ geçidinizin yönlendirme tabanlı VPN yapılandırmalarının desteklediğinden emin olun.

![Çoklu site diyagramı](./media/vpn-gateway-multi-site/multisite.png "Çok siteli")

## <a name="points-to-consider"></a>Dikkat edilmesi gereken noktalar

**Bu sanal ağ üzerinde değişiklik yapmak için portalı kullanamayacaksınız.** Portalı kullanmak yerine ağ yapılandırma dosyasında değişiklik yapmanız gerekir. Portalda değişiklik yaparsanız, bu sanal ağ için çok siteli başvuru ayarlarınızın üzerine yazılır.

Ağ yapılandırma dosyasını çok siteli prosedürü tamamladığınız zamana göre rahat bir şekilde kullanmanız gerekir. Ancak, ağ yapılandırmanızda çalışan birden fazla kişi varsa, herkesin bu sınırlamayı bildiğinden emin olmanız gerekir. Bu, portalı hiç kullanmayacağınız anlamına gelmez. Bu belirli sanal ağ üzerinde yapılandırma değişiklikleri yapmak dışında, diğer her şey için kullanabilirsiniz.

## <a name="before-you-begin"></a>Başlamadan önce

Yapılandırmaya başlamadan önce, aşağıdakilere sahip olduğunuzu doğrulayın:

* Her şirket içi konum için uyumlu VPN donanımı. Kullanmak istediğiniz cihazın uyumlu olduğu bilinen bir şey olup olmadığını doğrulamak için [sanal ağ bağlantısı IÇIN VPN cihazları hakkında](vpn-gateway-about-vpn-devices.md) ' yı denetleyin.
* Her VPN aygıtı için dışarıdan kullanıma açık bir genel IPv4 IP adresi. IP adresi bir NAT 'ın arkasında bulunamıyor. Bu gereksinim.
* VPN donanımınızı yapılandırırken yetereden bir kişi. VPN cihazınızı nasıl yapılandıracağınızı veya çalıştıran biriyle nasıl çalışacağına ilişkin kesin bir anlaya sahip olmanız gerekir.
* Sanal ağınız için kullanmak istediğiniz IP adresi aralıkları (henüz bir oluşturmadıysanız).
* Bağlanacağınız her yerel ağ sitesinin IP adresi aralıkları. Bağlanmak istediğiniz her yerel ağ sitesi için IP adresi aralıklarının çakışmadığından emin olmanız gerekir. Aksi takdirde, portal veya REST API karşıya yüklenen yapılandırmayı reddeder.<br>Örneğin, 10.2.3.0/24 IP adres aralığını içeren iki yerel ağ siteniz varsa ve hedef adresi 10.2.3.3 olan bir paketiniz varsa, Azure, adres aralıkları örtüştiğinden paketi hangi siteye göndermek istediğinizi bilmez. Azure, yönlendirme sorunlarını engellemek için çakışan aralıklar içeren bir yapılandırma dosyasını karşıya yüklemeye izin vermez.

### <a name="working-with-azure-powershell"></a>Azure PowerShell çalışma

[!INCLUDE [vpn-gateway-classic-powershell](../../includes/vpn-gateway-powershell-classic-locally.md)]

## <a name="1-create-a-site-to-site-vpn"></a>1. siteden siteye VPN oluşturma
Dinamik yönlendirme ağ geçidi ile siteden siteye VPN zaten varsa harika! [Sanal ağ yapılandırma ayarlarını dışarı aktarmaya](#export)devam edebilirsiniz. Aksi takdirde, şunları yapın:

### <a name="if-you-already-have-a-site-to-site-virtual-network-but-it-has-a-static-policy-based-routing-gateway"></a>Zaten bir siteden siteye sanal ağınız varsa, ancak statik (ilke tabanlı) yönlendirme ağ geçidine sahiptir:
1. Ağ Geçidi türünü dinamik yönlendirme olarak değiştirin. Çok siteli VPN, dinamik (rota tabanlı) yönlendirme ağ geçidi gerektirir. Ağ Geçidi türünü değiştirmek için, önce mevcut ağ geçidini silip yeni bir tane oluşturmanız gerekir.
2. Yeni ağ geçidinizi yapılandırın ve VPN tünelinizi oluşturun. Yönergeler için, yönergeler Için bkz. [SKU ve VPN türünü belirtme](vpn-gateway-howto-site-to-site-classic-portal.md#sku). Yönlendirme türünü ' Dynamic ' olarak belirttiğinizden emin olun.

### <a name="if-you-dont-have-a-site-to-site-virtual-network"></a>Siteden siteye sanal ağınız yoksa:
1. Aşağıdaki yönergeleri kullanarak siteden siteye sanal ağınızı oluşturun: [siteden sıteye VPN bağlantısı olan bir sanal ağ oluşturun](vpn-gateway-site-to-site-create.md).  
2. Şu yönergeleri kullanarak dinamik yönlendirme ağ geçidi yapılandırın: [bir VPN Gateway yapılandırın](vpn-gateway-configure-vpn-gateway-mp.md). Ağ Geçidi türü için **dinamik yönlendirme** ' yi seçtiğinizden emin olun.

## <a name="2-export-the-network-configuration-file"></a><a name="export"></a>2. ağ yapılandırma dosyasını dışarı aktarma

PowerShell konsolunuzu yükseltilmiş haklarla açın. Hizmet yönetimine geçiş yapmak için şu komutu kullanın:

```powershell
azure config mode asm
```

Hesabınıza bağlanın. Bağlanmanıza yardımcı olması için aşağıdaki örneği kullanın:

```powershell
Add-AzureAccount
```

Aşağıdaki komutu çalıştırarak Azure ağ yapılandırma dosyanızı dışarı aktarın. Dosyanın konumunu, gerekirse farklı bir konuma dışarı aktarılacak şekilde değiştirebilirsiniz.

```powershell
Get-AzureVNetConfig -ExportToFile C:\AzureNet\NetworkConfig.xml
```

## <a name="3-open-the-network-configuration-file"></a>3. ağ yapılandırma dosyasını açın
Son adımda indirdiğiniz ağ yapılandırma dosyasını açın. Dilediğiniz XML düzenleyicisini kullanın. Dosya şuna benzer olmalıdır:

        <NetworkConfiguration xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
          <VirtualNetworkConfiguration>
            <LocalNetworkSites>
              <LocalNetworkSite name="Site1">
                <AddressSpace>
                  <AddressPrefix>10.0.0.0/16</AddressPrefix>
                  <AddressPrefix>10.1.0.0/16</AddressPrefix>
                </AddressSpace>
                <VPNGatewayAddress>131.2.3.4</VPNGatewayAddress>
              </LocalNetworkSite>
              <LocalNetworkSite name="Site2">
                <AddressSpace>
                  <AddressPrefix>10.2.0.0/16</AddressPrefix>
                  <AddressPrefix>10.3.0.0/16</AddressPrefix>
                </AddressSpace>
                <VPNGatewayAddress>131.4.5.6</VPNGatewayAddress>
              </LocalNetworkSite>
            </LocalNetworkSites>
            <VirtualNetworkSites>
              <VirtualNetworkSite name="VNet1" AffinityGroup="USWest">
                <AddressSpace>
                  <AddressPrefix>10.20.0.0/16</AddressPrefix>
                  <AddressPrefix>10.21.0.0/16</AddressPrefix>
                </AddressSpace>
                <Subnets>
                  <Subnet name="FE">
                    <AddressPrefix>10.20.0.0/24</AddressPrefix>
                  </Subnet>
                  <Subnet name="BE">
                    <AddressPrefix>10.20.1.0/24</AddressPrefix>
                  </Subnet>
                  <Subnet name="GatewaySubnet">
                    <AddressPrefix>10.20.2.0/29</AddressPrefix>
                  </Subnet>
                </Subnets>
                <Gateway>
                  <ConnectionsToLocalNetwork>
                    <LocalNetworkSiteRef name="Site1">
                      <Connection type="IPsec" />
                    </LocalNetworkSiteRef>
                  </ConnectionsToLocalNetwork>
                </Gateway>
              </VirtualNetworkSite>
            </VirtualNetworkSites>
          </VirtualNetworkConfiguration>
        </NetworkConfiguration>

## <a name="4-add-multiple-site-references"></a>4. birden çok site başvurusu ekleme
Site başvuru bilgilerini eklediğinizde veya kaldırdığınızda, ConnectionsToLocalNetwork/LocalNetworkSiteRef üzerinde yapılandırma değişiklikleri yaparsınız. Yeni bir yerel site başvurusu ekleme yeni bir tünel oluşturmak için Azure 'ı tetikler. Aşağıdaki örnekte, ağ yapılandırması tek siteli bir bağlantı içindir. Değişikliklerinizi yapmayı bitirdikten sonra dosyayı kaydedin.

```xml
  <Gateway>
    <ConnectionsToLocalNetwork>
      <LocalNetworkSiteRef name="Site1"><Connection type="IPsec" /></LocalNetworkSiteRef>
    </ConnectionsToLocalNetwork>
  </Gateway>
```

Ek site başvuruları eklemek için (çok siteli bir yapılandırma oluşturun), aşağıdaki örnekte gösterildiği gibi ek "LocalNetworkSiteRef" satırları eklemeniz yeterlidir:

```xml
  <Gateway>
    <ConnectionsToLocalNetwork>
      <LocalNetworkSiteRef name="Site1"><Connection type="IPsec" /></LocalNetworkSiteRef>
      <LocalNetworkSiteRef name="Site2"><Connection type="IPsec" /></LocalNetworkSiteRef>
    </ConnectionsToLocalNetwork>
  </Gateway>
```

## <a name="5-import-the-network-configuration-file"></a>5. ağ yapılandırma dosyasını içeri aktarın
Ağ yapılandırma dosyasını içeri aktarın. Bu dosyayı değişikliklerle içeri aktardığınızda yeni tüneller eklenecektir. Tüneller, daha önce oluşturduğunuz dinamik ağ geçidini kullanır. Dosyayı içeri aktarmak için PowerShell 'i kullanabilirsiniz.

## <a name="6-download-keys"></a>6. indirme anahtarları
Yeni tünellerinizi eklendikten sonra, her tünel için IPSec/ıKE önceden paylaşılan anahtarlarını almak üzere ' Get-AzureVNetGatewayKey ' PowerShell cmdlet 'ini kullanın.

Örneğin:

```powershell
Get-AzureVNetGatewayKey –VNetName "VNet1" –LocalNetworkSiteName "Site1"
Get-AzureVNetGatewayKey –VNetName "VNet1" –LocalNetworkSiteName "Site2"
```

İsterseniz, önceden paylaşılan anahtarları almak için *sanal ağ geçidi paylaşılan anahtar REST API al* ' ı da kullanabilirsiniz.

## <a name="7-verify-your-connections"></a>7. bağlantılarınızı doğrulayın
Çok siteli tünel durumunu kontrol edin. Her tünele ilgili anahtarları indirdikten sonra, bağlantıları doğrulamak isteyeceksiniz. Aşağıdaki örnekte gösterildiği gibi, sanal ağ tünellerinin bir listesini almak için ' Get-AzureVnetConnection ' kullanın. VNet1, VNet 'in adıdır.

```powershell
Get-AzureVnetConnection -VNetName VNET1
```

Örnek dönüş:

```
    ConnectivityState         : Connected
    EgressBytesTransferred    : 661530
    IngressBytesTransferred   : 519207
    LastConnectionEstablished : 5/2/2014 2:51:40 PM
    LastEventID               : 23401
    LastEventMessage          : The connectivity state for the local network site 'Site1' changed from Not Connected to Connected.
    LastEventTimeStamp        : 5/2/2014 2:51:40 PM
    LocalNetworkSiteName      : Site1
    OperationDescription      : Get-AzureVNetConnection
    OperationId               : 7f68a8e6-51e9-9db4-88c2-16b8067fed7f
    OperationStatus           : Succeeded

    ConnectivityState         : Connected
    EgressBytesTransferred    : 789398
    IngressBytesTransferred   : 143908
    LastConnectionEstablished : 5/2/2014 3:20:40 PM
    LastEventID               : 23401
    LastEventMessage          : The connectivity state for the local network site 'Site2' changed from Not Connected to Connected.
    LastEventTimeStamp        : 5/2/2014 2:51:40 PM
    LocalNetworkSiteName      : Site2
    OperationDescription      : Get-AzureVNetConnection
    OperationId               : 7893b329-51e9-9db4-88c2-16b8067fed7f
    OperationStatus           : Succeeded
```

## <a name="next-steps"></a>Sonraki adımlar

VPN ağ geçitleri hakkında daha fazla bilgi edinmek için bkz. [VPN ağ geçitleri hakkında](vpn-gateway-about-vpngateways.md).
