---
title: 'Azure VPN Gateway: zorlamalı tünelleme-siteden siteye bağlantıları yapılandırma: klasik'
description: Internet 'e bağlı tüm trafiği şirket içi konumunuza geri yönlendirme veya ' zorlama '.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: article
ms.date: 08/01/2017
ms.author: cherylmc
ms.openlocfilehash: fe06257127ff352f68fb27d3507cee0229e31498
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "77201586"
---
# <a name="configure-forced-tunneling-using-the-classic-deployment-model"></a>Klasik dağıtım modelini kullanarak zorlamalı tünel yapılandırma

Zorlamalı tünel oluşturma, İnternet’e yönelik tüm trafiğin inceleme ve denetim amacıyla Siteden Siteye VPN tüneli aracılığıyla şirket içi konumunuza geri yönlendirilmesini veya “zorlanmasını” sağlamanızı mümkün kılar. Bu, çoğu kurumsal BT İlkesi için kritik bir güvenlik gereksinimidir. Zorlamalı tünel olmadan, Azure 'daki sanal makinelerinizden Internet 'e yönelik trafik, trafiği incelemenize veya denetlemenizi sağlayan seçenek olmadan Azure ağ altyapısından her zaman doğrudan Internet 'e çapraz geçiş yapar. Yetkisiz Internet erişimi, bilgilerin açığa çıkmasına veya diğer güvenlik ihlallerine neden olabilir.

[!INCLUDE [vpn-gateway-classic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

Bu makalede, klasik dağıtım modeli kullanılarak oluşturulan sanal ağlar için Zorlamalı tünel oluşturma işlemi adım adım açıklanmaktadır. Zorlamalı tünel, Portal üzerinden değil, PowerShell kullanılarak yapılandırılabilir. Kaynak Yöneticisi dağıtım modeli için Zorlamalı tünel yapılandırmak istiyorsanız, aşağıdaki açılan listeden Kaynak Yöneticisi makale ' i seçin:

> [!div class="op_single_selector"]
> * [PowerShell - Klasik](vpn-gateway-about-forced-tunneling.md)
> * [PowerShell - Resource Manager](vpn-gateway-forced-tunneling-rm.md)
> 
> 

## <a name="requirements-and-considerations"></a>Gereksinimler ve önemli noktalar
Azure 'da Zorlamalı tünel, sanal ağ kullanıcı tanımlı yollar (UDR) ile yapılandırılır. Trafiği şirket içi siteye yeniden yönlendirmek, Azure VPN ağ geçidi için varsayılan rota olarak ifade edilir. Aşağıdaki bölümde, bir Azure sanal ağı için yönlendirme tablosunun ve yollarının geçerli sınırlaması listelenmektedir:

* Her sanal ağ alt ağı yerleşik, sistem yönlendirme tablosuna sahiptir. Sistem yönlendirme tablosu aşağıdaki üç yol grubuna sahiptir:

  * **Yerel VNET yolları:** Aynı sanal ağdaki hedef VM 'lere doğrudan.
  * **Şirket içi rotalar:** Azure VPN ağ geçidine.
  * **Varsayılan yol:** Doğrudan Internet 'e. Önceki iki yolun kapsamadığı özel IP adreslerine gidecek paketler bırakılır.
* Kullanıcı tanımlı yolların yayımlanmasından sonra, varsayılan bir yol eklemek için bir yönlendirme tablosu oluşturabilir ve ardından bu alt ağlarda Zorlamalı tünel sağlamak için yönlendirme tablosunu VNet alt ağlarınıza ilişkilendirebilirsiniz.
* Sanal ağa bağlı şirketler arası yerel siteler arasında "varsayılan site" ayarlamanız gerekir.
* Zorlamalı tünelleme, dinamik yönlendirme VPN Gateway (statik ağ geçidi değil) olan bir VNet ile ilişkilendirilmelidir.
* ExpressRoute Zorlamalı tünel bu mekanizma aracılığıyla yapılandırılmaz, ancak bunun yerine ExpressRoute BGP eşleme oturumları aracılığıyla varsayılan bir yol tanıtıarak etkinleştirilir. Daha fazla bilgi için bkz. [ExpressRoute belgeleri](https://azure.microsoft.com/documentation/services/expressroute/) .

## <a name="configuration-overview"></a>Yapılandırmaya genel bakış
Aşağıdaki örnekte, ön uç alt ağı zorlamalı tünellenmiş değildir. Ön uç alt ağındaki iş yükleri doğrudan Internet 'ten gelen müşteri isteklerini kabul etmeye ve bunlara yanıt vermeye devam edebilir. Orta katman ve arka uç alt ağları Zorlamalı tünel. Bu iki alt ağın Internet 'e giden tüm bağlantıları, S2S VPN tünellerinden biri aracılığıyla şirket içi bir siteye yeniden yönlendirilir veya yeniden yönlendirilir.

Bu sayede, Azure 'daki sanal makinelerinizden veya bulut hizmetinizden Internet erişimini kısıtlayıp incelemenize olanak sağlarken çok katmanlı hizmet mimarinizi etkinleştirmeye devam edersiniz. Sanal ağlarınızda Internet 'e yönelik iş yükleri yoksa, sanal ağların tamamına Zorlamalı tünel de uygulayabilirsiniz.

![Zorlamalı tünel](./media/vpn-gateway-about-forced-tunneling/forced-tunnel.png)

## <a name="before-you-begin"></a>Başlamadan önce
Yapılandırmaya başlamadan önce aşağıdaki öğelerin bulunduğunu doğrulayın:

* Azure aboneliği. Henüz Azure aboneliğiniz yoksa [MSDN abonelik avantajlarınızı](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) etkinleştirebilir veya [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) için kaydolabilirsiniz.
* Yapılandırılmış bir sanal ağ. 
* [!INCLUDE [vpn-gateway-classic-powershell](../../includes/vpn-gateway-powershell-classic-locally.md)]

### <a name="to-sign-in"></a>Oturum açmak için

1. PowerShell konsolunuzu yükseltilmiş haklarla açın. Hizmet yönetimine geçiş yapmak için şu komutu kullanın:

   ```powershell
   azure config mode asm
   ```
2. Hesabınıza bağlanın. Bağlanmanıza yardımcı olması için aşağıdaki örneği kullanın:

   ```powershell
   Add-AzureAccount
   ```

## <a name="configure-forced-tunneling"></a>Zorlamalı tünel yapılandırma
Aşağıdaki yordam, bir sanal ağ için Zorlamalı tünel belirtmenize yardımcı olur. Yapılandırma adımları VNet ağ yapılandırma dosyasına karşılık gelir.

```xml
<VirtualNetworkSite name="MultiTier-VNet" Location="North Europe">
     <AddressSpace>
      <AddressPrefix>10.1.0.0/16</AddressPrefix>
        </AddressSpace>
        <Subnets>
          <Subnet name="Frontend">
            <AddressPrefix>10.1.0.0/24</AddressPrefix>
          </Subnet>
          <Subnet name="Midtier">
            <AddressPrefix>10.1.1.0/24</AddressPrefix>
          </Subnet>
          <Subnet name="Backend">
            <AddressPrefix>10.1.2.0/23</AddressPrefix>
          </Subnet>
          <Subnet name="GatewaySubnet">
            <AddressPrefix>10.1.200.0/28</AddressPrefix>
          </Subnet>
        </Subnets>
        <Gateway>
          <ConnectionsToLocalNetwork>
            <LocalNetworkSiteRef name="DefaultSiteHQ">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
            <LocalNetworkSiteRef name="Branch1">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
            <LocalNetworkSiteRef name="Branch2">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
            <LocalNetworkSiteRef name="Branch3">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
        </Gateway>
      </VirtualNetworkSite>
    </VirtualNetworkSite>
```

Bu örnekte, ' MultiTier-VNet ' adlı sanal ağın üç alt ağı vardır: ' ön uç ', ' orta katman ' ve ' arka uç ' alt ağları, dört adet çapraz şirket içi bağlantıyla: ' DefaultSiteHQ ' ve üç dal. 

Bu adımlar, ' DefaultSiteHQ ' değerini Zorlamalı tünel için varsayılan site bağlantısı olarak ayarlar ve zorlamalı tüneli kullanmak için orta katman ve arka uç alt ağlarını yapılandırır.

1. Yönlendirme tablosu oluşturun. Yönlendirme tablonuzu oluşturmak için aşağıdaki cmdlet 'i kullanın.

   ```powershell
   New-AzureRouteTable –Name "MyRouteTable" –Label "Routing Table for Forced Tunneling" –Location "North Europe"
   ```

2. Yönlendirme tablosuna varsayılan bir yol ekleyin. 

   Aşağıdaki örnek, adım 1 ' de oluşturulan yönlendirme tablosuna varsayılan bir yol ekler. Desteklenen tek yol "0.0.0.0/0" öğesinin hedef ön ekinin "VPNGateway" sonrasına olduğunu unutmayın.

   ```powershell
   Get-AzureRouteTable -Name "MyRouteTable" | Set-AzureRoute –RouteTable "MyRouteTable" –RouteName "DefaultRoute" –AddressPrefix "0.0.0.0/0" –NextHopType VPNGateway
   ```

3. Yönlendirme tablosunu alt ağlarla ilişkilendirin. 

   Bir yönlendirme tablosu oluşturulup bir yol eklendikten sonra, yol tablosunu VNet alt ağına eklemek veya ilişkilendirmek için aşağıdaki örneği kullanın. Örnek, "MyRouteTable" yol tablosunu VNet MultiTier-VNet ' in orta katman ve arka uç alt ağlarına ekler.

   ```powershell
   Set-AzureSubnetRouteTable -VirtualNetworkName "MultiTier-VNet" -SubnetName "Midtier" -RouteTableName "MyRouteTable"
   Set-AzureSubnetRouteTable -VirtualNetworkName "MultiTier-VNet" -SubnetName "Backend" -RouteTableName "MyRouteTable"
   ```

4. Zorlamalı tünel için varsayılan bir site atayın. 

   Önceki adımda örnek cmdlet betikleri, yönlendirme tablosunu oluşturdu ve yol tablosunu VNet alt ağlarının ikisi ile ilişkilendirdi. Kalan adım, sanal ağın varsayılan site veya tünel olarak çok siteli bağlantıları arasında yerel bir site seçmek.

   ```powershell
   $DefaultSite = @("DefaultSiteHQ")
   Set-AzureVNetGatewayDefaultSite –VNetName "MultiTier-VNet" –DefaultSite "DefaultSiteHQ"
   ```

## <a name="additional-powershell-cmdlets"></a>Ek PowerShell cmdlet 'leri
### <a name="to-delete-a-route-table"></a>Bir yol tablosunu silmek için

```powershell
Remove-AzureRouteTable -Name <routeTableName>
```
  
### <a name="to-list-a-route-table"></a>Bir yol tablosu listelemek için

```powershell
Get-AzureRouteTable [-Name <routeTableName> [-DetailLevel <detailLevel>]]
```

### <a name="to-delete-a-route-from-a-route-table"></a>Yol tablosundan bir yolu silmek için

```powershell
Remove-AzureRouteTable –Name <routeTableName>
```

### <a name="to-remove-a-route-from-a-subnet"></a>Bir alt ağdan bir yolu kaldırmak için

```powershell
Remove-AzureSubnetRouteTable –VirtualNetworkName <virtualNetworkName> -SubnetName <subnetName>
```

### <a name="to-list-the-route-table-associated-with-a-subnet"></a>Bir alt ağla ilişkili yol tablosunu listelemek için

```powershell
Get-AzureSubnetRouteTable -VirtualNetworkName <virtualNetworkName> -SubnetName <subnetName>
```

### <a name="to-remove-a-default-site-from-a-vnet-vpn-gateway"></a>VNet VPN Gateway 'ten varsayılan bir siteyi kaldırmak için

```powershell
Remove-AzureVnetGatewayDefaultSite -VNetName <virtualNetworkName>
```
