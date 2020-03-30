---
title: "VPN Ağ Geçidi'ni kullanarak birden fazla siteye VNet bağlayın: Klasik"
description: VPN Ağ Geçidi'ni kullanarak birden çok yerel yerel siteyi klasik bir sanal ağa bağlayın.
services: vpn-gateway
titleSuffix: Azure VPN Gateway
author: yushwang
ms.service: vpn-gateway
ms.topic: article
ms.date: 02/11/2020
ms.author: yushwang
ms.openlocfilehash: a95cd6ea85a16b0e0bf5f67f5dfc20d57f11463b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77198113"
---
# <a name="add-a-site-to-site-connection-to-a-vnet-with-an-existing-vpn-gateway-connection-classic"></a>Varolan VPN ağ geçidi bağlantısına sahip bir VNet'e Siteden Siteye bağlantı ekleme (klasik)

[!INCLUDE [deployment models](../../includes/vpn-gateway-classic-deployment-model-include.md)]

> [!div class="op_single_selector"]
> * [Azure portalında](vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md)
> * [PowerShell (klasik)](vpn-gateway-multi-site.md)
>
>

Bu makale, varolan bir bağlantısı olan bir VPN ağ geçidine Site-to-Site (S2S) bağlantıları eklemek için PowerShell'i kullanmanız için size yol açar. Bu tür bir bağlantı genellikle "çok siteli" yapılandırma olarak adlandırılır. Bu makaledeki adımlar, klasik dağıtım modeli (Hizmet Yönetimi olarak da bilinir) kullanılarak oluşturulan sanal ağlar için geçerlidir. Bu adımlar ExpressRoute/Site-to-Site birlikte varolan bağlantı yapılandırmaları için geçerli değildir.

### <a name="deployment-models-and-methods"></a>Dağıtım modelleri ve yöntemleri

[!INCLUDE [vpn-gateway-classic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

Bu tabloyu, bu yapılandırma için yeni makaleler ve ek araçlar kullanılabilir hale geldikçe güncelliyoruz. Bir makale kullanılabilir olduğunda, bu tablodan doğrudan bağlantı.

[!INCLUDE [vpn-gateway-table-multi-site](../../includes/vpn-gateway-table-multisite-include.md)]

## <a name="about-connecting"></a>Bağlanma hakkında

Birden çok şirket içi siteyi tek bir sanal ağa bağlayabilirsiniz. Bu özellikle hibrit bulut çözümleri oluşturmak için caziptir. Azure sanal ağ ağ ağ geçidinize çok siteli bağlantı oluşturmak, diğer Site'den Siteye bağlantılar oluşturmaya benzer. Aslında, ağ geçidi dinamik (rota tabanlı) olduğu sürece varolan bir Azure VPN ağ geçidini kullanabilirsiniz.

Sanal ağınıza bağlı statik bir ağ geçidiniz varsa, çok siteli ağı barındırmak için sanal ağı yeniden oluşturmanıza gerek kalmadan ağ geçidi türünü dinamik olarak değiştirebilirsiniz. Yönlendirme türünü değiştirmeden önce, şirket içi VPN ağ geçidinizin rota tabanlı VPN yapılandırmalarını desteklediğinden emin olun.

![çok siteli diyagram](./media/vpn-gateway-multi-site/multisite.png "çok siteli")

## <a name="points-to-consider"></a>Dikkat edilmesi gereken noktalar

**Bu sanal ağda değişiklik yapmak için portalı kullanamazsınız.** Portalı kullanmak yerine ağ yapılandırma dosyasında değişiklik yapmanız gerekir. Portalda değişiklik yaparsanız, bu sanal ağ için çok siteli başvuru ayarlarınızın üzerine yazılırlar.

Çok siteli yordamı tamamlayana kadar ağ yapılandırma dosyasını kullanırken kendinizi rahat hissetmeniz gerekir. Ancak, ağ yapılandırmanızda çalışan birden çok kişi varsa, bu sınırlamayı herkesin bildiğinden emin olmanız gerekir. Bu portalı hiç kullanamayacağın anlamına gelmez. Bu sanal ağda yapılandırma değişiklikleri yapmak dışında, diğer her şey için kullanabilirsiniz.

## <a name="before-you-begin"></a>Başlamadan önce

Yapılandırmaya başlamadan önce aşağıdakileri yaptığınızı doğrulayın:

* Şirket içi her konum için uyumlu VPN donanımı. Kullanmak istediğiniz aygıtın uyumlu olduğu bilinen bir şey olup olmadığını doğrulamak [için Sanal Ağ Bağlantısı için VPN Aygıtları Hakkında'yı](vpn-gateway-about-vpn-devices.md) kontrol edin.
* Her VPN aygıtı için harici olarak bakan genel Bir IPv4 IP adresi. IP adresi bir NAT'nin arkasında bulunamaz. Bu gerekliliktir.
* VPN donanımınızı yapılandırmada yetkin biri. VPN aygıtınızı nasıl yapılandıracağınız veya yapıyı yapan biriyle nasıl çalışacağınız konusunda güçlü bir anlayışa sahip olmak gerekir.
* Sanal ağınız için kullanmak istediğiniz IP adresi aralıkları (henüz oluşturmadıysanız).
* IP adresi, bağlanacağınız yerel ağ sitelerinin her biri için aralıkları. Bağlanmak istediğiniz yerel ağ sitelerinin her biri için IP adresiaralıklarının çakışmadığından emin olmanız gerekir. Aksi takdirde, portal veya REST API yüklenen yapılandırmayı reddeder.<br>Örneğin, her ikisi de IP adresi aralığı 10.2.3.0/24 içeren iki yerel ağ siteniz varsa ve hedef adresi 10.2.3.3 olan bir paketiniz varsa, Azure adresi aralıkları çakıştığı için paketi hangi siteye göndermek istediğinizi bilmez. Yönlendirme sorunlarını önlemek için Azure, çakışan aralıklara sahip bir yapılandırma dosyası yüklemenize izin vermez.

### <a name="working-with-azure-powershell"></a>Azure PowerShell ile çalışma

[!INCLUDE [vpn-gateway-classic-powershell](../../includes/vpn-gateway-powershell-classic-locally.md)]

## <a name="1-create-a-site-to-site-vpn"></a>1. Siteden Siteye VPN Oluşturma
Zaten dinamik bir yönlendirme ağ geçidi ile Site-to-Site VPN varsa, harika! Sanal ağ [yapılandırma ayarlarını dışa aktarabilirsiniz.](#export) Değilse, aşağıdakileri yapın:

### <a name="if-you-already-have-a-site-to-site-virtual-network-but-it-has-a-static-policy-based-routing-gateway"></a>Siteden Siteye sanal ağınız zaten varsa, ancak statik (ilke tabanlı) yönlendirme ağ geçidi varsa:
1. Ağ geçidi türünü dinamik yönlendirme olarak değiştirin. Çok siteli VPN, dinamik (rota tabanlı olarak da bilinir) yönlendirme ağ geçidi gerektirir. Ağ geçidi türünüzü değiştirmek için önce varolan ağ geçidini silmeniz, ardından yeni bir ağ geçidi oluşturmanız gerekir.
2. Yeni ağ geçidinizi yapılandırın ve VPN tünelinizi oluşturun. Talimatlar için bkz. [Specify the SKU and VPN type](vpn-gateway-howto-site-to-site-classic-portal.md#sku) Yönlendirme Türünü 'Dinamik' olarak belirttiğinizi unutmayın.

### <a name="if-you-dont-have-a-site-to-site-virtual-network"></a>Siteden Siteye sanal ağınız yoksa:
1. Bu yönergeleri kullanarak Siteden Siteye sanal ağınızı oluşturun: [Siteden Siteye VPN Bağlantısı olan bir Sanal Ağ Oluşturun.](vpn-gateway-site-to-site-create.md)  
2. Bu yönergeleri kullanarak dinamik bir yönlendirme ağ geçidi ni yapılandırma: [VPN Ağ Geçidi'ni yapılandırın.](vpn-gateway-configure-vpn-gateway-mp.md) Ağ geçidi türünüz için **dinamik yönlendirmeyi** seçtiğinizden emin olun.

## <a name="2-export-the-network-configuration-file"></a><a name="export"></a>2. Ağ yapılandırma dosyasını dışa aktarma

PowerShell konsolunuzu yüksek haklarla açın. Hizmet yönetimine geçmek için şu komutu kullanın:

```powershell
azure config mode asm
```

Hesabınıza bağlanın. Bağlanmanıza yardımcı olması için aşağıdaki örneği kullanın:

```powershell
Add-AzureAccount
```

Aşağıdaki komutu çalıştırarak Azure ağ yapılandırma dosyanızı dışa aktarın. Gerekirse dosyanın konumunu farklı bir konuma dışa aktarmak için değiştirebilirsiniz.

```powershell
Get-AzureVNetConfig -ExportToFile C:\AzureNet\NetworkConfig.xml
```

## <a name="3-open-the-network-configuration-file"></a>3. Ağ yapılandırma dosyasını açma
Son adımda karşıdan yüklediğiniz ağ yapılandırma dosyasını açın. Beğendiğiniz herhangi bir xml düzenleyicikullanın. Dosya aşağıdakilere benzer olmalıdır:

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

## <a name="4-add-multiple-site-references"></a>4. Birden çok site referansı ekleme
Site başvuru bilgilerini eklediğinizde veya kaldırdığınızda, ConnectionsToLocalNetwork/LocalNetworkSiteRef'te yapılandırma değişiklikleri yaparsınız. Yeni bir yerel site başvurusu eklemek, yeni bir tünel oluşturmak için Azure'u tetikler. Aşağıdaki örnekte, ağ yapılandırması tek bir site bağlantısı içindir. Değişikliklerinizi yapmayı bitirdikten sonra dosyayı kaydedin.

```xml
  <Gateway>
    <ConnectionsToLocalNetwork>
      <LocalNetworkSiteRef name="Site1"><Connection type="IPsec" /></LocalNetworkSiteRef>
    </ConnectionsToLocalNetwork>
  </Gateway>
```

Ek site başvuruları eklemek için (çok siteyapılandırması oluşturmak), aşağıdaki örnekte gösterildiği gibi ek "LocalNetworkSiteRef" satırları eklemeniz yeterlidir:

```xml
  <Gateway>
    <ConnectionsToLocalNetwork>
      <LocalNetworkSiteRef name="Site1"><Connection type="IPsec" /></LocalNetworkSiteRef>
      <LocalNetworkSiteRef name="Site2"><Connection type="IPsec" /></LocalNetworkSiteRef>
    </ConnectionsToLocalNetwork>
  </Gateway>
```

## <a name="5-import-the-network-configuration-file"></a>5. Ağ yapılandırma dosyasını alma
Ağ yapılandırma dosyasını içe aktarın. Bu dosyayı değişikliklerle birlikte içe aktardığınızda, yeni tüneller eklenir. Tüneller, daha önce oluşturduğunuz dinamik ağ geçidini kullanır. Dosyayı almak için PowerShell'i kullanabilirsiniz.

## <a name="6-download-keys"></a>6. İndirme tuşları
Yeni tünelleriniz eklendikten sonra, her tünel için IPsec/IKE önceden paylaşılan anahtarları almak için PowerShell cmdlet 'Get-AzureVNetGatewayKey' kullanın.

Örnek:

```powershell
Get-AzureVNetGatewayKey –VNetName "VNet1" –LocalNetworkSiteName "Site1"
Get-AzureVNetGatewayKey –VNetName "VNet1" –LocalNetworkSiteName "Site2"
```

İsterseniz, önceden paylaşılan anahtarları almak için *Sanal Ağ Ağ Ağ Geçidi PaylaşPaylaşılan Anahtar* REST API'yi de kullanabilirsiniz.

## <a name="7-verify-your-connections"></a>7. Bağlantılarınızı doğrulayın
Çok siteli tünel durumunu kontrol edin. Her tünelin anahtarlarını indirdikten sonra bağlantıları doğrulamak istersiniz. Aşağıdaki örnekte gösterildiği gibi sanal ağ tünellerinin listesini almak için 'Get-AzureVnetConnection' kullanın. VNet1, VNet'in adıdır.

```powershell
Get-AzureVnetConnection -VNetName VNET1
```

Örnek getiri:

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

VPN Ağ Geçitleri hakkında daha fazla bilgi edinmek için [VPN Ağ Geçitleri hakkında](vpn-gateway-about-vpngateways.md)bilgi edinin.
