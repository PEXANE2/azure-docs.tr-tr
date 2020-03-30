---
title: 'Azure VPN Ağ Geçidi: Zorunlu tünel leme yapılandırma - Siteden Siteye bağlantılar: klasik'
description: Internet'e bağlı tüm trafiği şirket içi konumunuza nasıl yönlendirilir veya 'zorlar'.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: article
ms.date: 08/01/2017
ms.author: cherylmc
ms.openlocfilehash: fe06257127ff352f68fb27d3507cee0229e31498
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77201586"
---
# <a name="configure-forced-tunneling-using-the-classic-deployment-model"></a>Klasik dağıtım modelini kullanarak zorlamalı tünel yapılandırma

Zorlamalı tünel oluşturma, İnternet’e yönelik tüm trafiğin inceleme ve denetim amacıyla Siteden Siteye VPN tüneli aracılığıyla şirket içi konumunuza geri yönlendirilmesini veya “zorlanmasını” sağlamanızı mümkün kılar. Bu, çoğu kurumsal BT ilkeleri için kritik bir güvenlik gereksinimidir. Zorunlu tünel leme olmadan, Azure'daki VM'lerinizden Gelen Internet'e bağlı trafik, trafiği denetlemenize veya denetlemenize izin vermeden azure ağ altyapısından doğrudan Internet'e geçer. Yetkisiz Internet erişimi, bilgi ifşasına veya diğer güvenlik ihlallerine yol açabilir.

[!INCLUDE [vpn-gateway-classic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

Bu makalede, klasik dağıtım modeli kullanılarak oluşturulan sanal ağlar için zorunlu tünel oluşturma yapılandırma yoluyla size yol. Zorunlu tünelleme portal üzerinden değil, PowerShell kullanılarak yapılandırılabilir. Kaynak Yöneticisi dağıtım modeli için zorunlu tünel yapılandırmak istiyorsanız, aşağıdaki açılır listeden Kaynak Yöneticisi makalesini seçin:

> [!div class="op_single_selector"]
> * [PowerShell - Klasik](vpn-gateway-about-forced-tunneling.md)
> * [PowerShell - Resource Manager](vpn-gateway-forced-tunneling-rm.md)
> 
> 

## <a name="requirements-and-considerations"></a>Gereksinimler ve hususlar
Azure'da zorunlu tünel leme, sanal ağ kullanıcı tanımlı rotalar (UDR) üzerinden yapılandırılır. Trafiği şirket içi bir siteye yönlendirmek, Varsayılan Rota olarak Azure VPN ağ geçidine ifade edilir. Aşağıdaki bölümde, bir Azure Sanal Ağı için yönlendirme tablosunun geçerli sınırlaması ve yolları listelemektedir:

* Her sanal ağ alt ağının yerleşik, sistem yönlendirme tablosu vardır. Sistem yönlendirme tablosuaşağıdaki üç rota grubuna sahiptir:

  * **Yerel VNet rotaları:** Doğrudan aynı sanal ağdaki hedef VM'lere.
  * **Şirket içi güzergahlar:** Azure VPN ağ geçidine.
  * **Varsayılan rota:** Doğrudan Internet'e. Önceki iki rota nın kapsamına giren özel IP adreslerine giden paketler bırakılır.
* Kullanıcı tanımlı yolların serbest bırakılması ile, varsayılan bir rota eklemek için bir yönlendirme tablosu oluşturabilir ve daha sonra bu alt ağlarda zorunlu tünel oluşturmayı etkinleştirmek için yönlendirme tablosunu VNet alt ağınıza ilişkilendirebilirsiniz.
* Sanal ağa bağlı binalar arası yerel siteler arasında bir "varsayılan site" ayarlamanız gerekir.
* Zorunlu tünelleme, dinamik yönlendirme VPN ağ geçidine (statik ağ geçidi olmayan) sahip bir VNet ile ilişkilendirilmelidir.
* ExpressRoute zorunlu tünelleme bu mekanizma üzerinden yapılandırılmamıştır, ancak bunun yerine, ExpressRoute BGP eşleme oturumları aracılığıyla varsayılan bir rotanın reklamını yaparak etkinleştirilir. Daha fazla bilgi için [ExpressRoute Belgeleri'ne](https://azure.microsoft.com/documentation/services/expressroute/) bakın.

## <a name="configuration-overview"></a>Yapılandırmaya genel bakış
Aşağıdaki örnekte, Frontend alt ağı tünele zorlanmaz. Frontend alt netindeki iş yükleri, Internet'ten gelen müşteri isteklerini doğrudan kabul etmeye ve yanıtlamaya devam edebilir. Orta seviye ve Arka uç alt ağları tünele atılmaya zorlanır. Bu iki alt ağdan Internet'e giden tüm bağlantılar, S2S VPN tünellerinden biri aracılığıyla zorunlu veya şirket içi bir siteye yönlendirilecektir.

Bu, Azure'daki sanal makinelerinizden veya bulut hizmetlerinizden Gelen Internet erişimini kısıtlamanıza ve denetlemenize olanak sağlarken, çok katmanlı hizmet mimarinizi etkinleştirmeye devam etmenizi sağlar. Sanal ağlarınızda Internet'e dönük iş yükleri yoksa, tüm sanal ağlara zorunlu tünel uygulama özelliği de uygulayabilirsiniz.

![Zorunlu Tünel](./media/vpn-gateway-about-forced-tunneling/forced-tunnel.png)

## <a name="before-you-begin"></a>Başlamadan önce
Yapılandırmaya başlamadan önce aşağıdaki öğelerin bulunduğunu doğrulayın:

* Azure aboneliği. Henüz Azure aboneliğiniz yoksa [MSDN abonelik avantajlarınızı](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) etkinleştirebilir veya [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) için kaydolabilirsiniz.
* Yapılandırılmış bir sanal ağ. 
* [!INCLUDE [vpn-gateway-classic-powershell](../../includes/vpn-gateway-powershell-classic-locally.md)]

### <a name="to-sign-in"></a>Oturum açma

1. PowerShell konsolunuzu yüksek haklarla açın. Hizmet yönetimine geçmek için şu komutu kullanın:

   ```powershell
   azure config mode asm
   ```
2. Hesabınıza bağlanın. Bağlanmanıza yardımcı olması için aşağıdaki örneği kullanın:

   ```powershell
   Add-AzureAccount
   ```

## <a name="configure-forced-tunneling"></a>Zorlamalı tünel yapılandırma
Aşağıdaki yordam, sanal ağ için zorunlu tünel belirlemenize yardımcı olur. Yapılandırma adımları VNet ağ yapılandırma dosyasına karşılık gelir.

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

Bu örnekte, 'MultiTier-VNet' sanal ağının üç alt ağı vardır: 'Frontend', 'Midtier', ve 'Backend' alt ağları, dört çapraz tesis bağlantısı vardır: 'DefaultSiteHQ', ve üç Şube. 

Adımlar, 'DefaultSiteHQ'yu zorunlu tünelleme için varsayılan site bağlantısı olarak ayarlar ve Midtier ve Backend alt ağlarını zorunlu tünelleme kullanacak şekilde yapılandıracaktır.

1. Yönlendirme tablosu oluşturun. Rota tablonuzu oluşturmak için aşağıdaki cmdlet'i kullanın.

   ```powershell
   New-AzureRouteTable –Name "MyRouteTable" –Label "Routing Table for Forced Tunneling" –Location "North Europe"
   ```

2. Yönlendirme tablosuna varsayılan bir rota ekleyin. 

   Aşağıdaki örnek, Adım 1'de oluşturulan yönlendirme tablosuna varsayılan bir yol ekler. Desteklenen tek rotanın "VPNGateway" NextHop'a "0.0.0.0/0" hedef öneki olduğunu unutmayın.

   ```powershell
   Get-AzureRouteTable -Name "MyRouteTable" | Set-AzureRoute –RouteTable "MyRouteTable" –RouteName "DefaultRoute" –AddressPrefix "0.0.0.0/0" –NextHopType VPNGateway
   ```

3. Yönlendirme tablosunu alt ağlarla ilişkilendirin. 

   Yönlendirme tablosu oluşturulduktan ve bir rota eklendikten sonra, rota tablosunu bir VNet alt ağına eklemek veya ilişkilendirmek için aşağıdaki örneği kullanın. Örnek, VNet MultiTier-VNet'in Orta ve Arka Uç alt ağlarına "MyRouteTable" rota tablosunu ekler.

   ```powershell
   Set-AzureSubnetRouteTable -VirtualNetworkName "MultiTier-VNet" -SubnetName "Midtier" -RouteTableName "MyRouteTable"
   Set-AzureSubnetRouteTable -VirtualNetworkName "MultiTier-VNet" -SubnetName "Backend" -RouteTableName "MyRouteTable"
   ```

4. Zorunlu tünel leme için varsayılan bir site atayın. 

   Önceki adımda, örnek cmdlet komut dosyaları yönlendirme tablosunu oluşturdu ve rota tablosunu VNet alt ağlarından ikisiyle ilişkilendirdi. Kalan adım varsayılan site veya tünel olarak sanal ağın çok site bağlantıları arasında yerel bir site seçmektir.

   ```powershell
   $DefaultSite = @("DefaultSiteHQ")
   Set-AzureVNetGatewayDefaultSite –VNetName "MultiTier-VNet" –DefaultSite "DefaultSiteHQ"
   ```

## <a name="additional-powershell-cmdlets"></a>Ek PowerShell cmdlets
### <a name="to-delete-a-route-table"></a>Rota tablosunu silmek için

```powershell
Remove-AzureRouteTable -Name <routeTableName>
```
  
### <a name="to-list-a-route-table"></a>Rota tablosunu listelemek için

```powershell
Get-AzureRouteTable [-Name <routeTableName> [-DetailLevel <detailLevel>]]
```

### <a name="to-delete-a-route-from-a-route-table"></a>Rota tablosundan rotayı silmek için

```powershell
Remove-AzureRouteTable –Name <routeTableName>
```

### <a name="to-remove-a-route-from-a-subnet"></a>Bir rotayı alt ağdan kaldırmak için

```powershell
Remove-AzureSubnetRouteTable –VirtualNetworkName <virtualNetworkName> -SubnetName <subnetName>
```

### <a name="to-list-the-route-table-associated-with-a-subnet"></a>Bir alt ağla ilişkili rota tablosunu listelemek için

```powershell
Get-AzureSubnetRouteTable -VirtualNetworkName <virtualNetworkName> -SubnetName <subnetName>
```

### <a name="to-remove-a-default-site-from-a-vnet-vpn-gateway"></a>Varsayılan bir siteyi Bir VNet VPN ağ geçidinden kaldırmak için

```powershell
Remove-AzureVnetGatewayDefaultSite -VNetName <virtualNetworkName>
```
