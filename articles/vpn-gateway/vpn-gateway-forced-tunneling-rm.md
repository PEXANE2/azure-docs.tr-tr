---
title: Siteden Siteye bağlantılar için zorunlu tünel geliştirme
description: Internet'e bağlı tüm trafiği şirket içi konumunuza nasıl yönlendirilir veya 'zorlar'.
services: vpn-gateway
titleSuffix: Azure VPN Gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: article
ms.date: 02/01/2018
ms.author: cherylmc
ms.openlocfilehash: fc35654403bbe1375d4188476b11fd0453f74345
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79244634"
---
# <a name="configure-forced-tunneling-using-the-azure-resource-manager-deployment-model"></a>Azure Resource Manager dağıtım modelini kullanarak zorlamalı tünel yapılandırma

Zorlamalı tünel oluşturma, İnternet’e yönelik tüm trafiğin inceleme ve denetim amacıyla Siteden Siteye VPN tüneli aracılığıyla şirket içi konumunuza geri yönlendirilmesini veya “zorlanmasını” sağlamanızı mümkün kılar. Bu, çoğu kurumsal BT ilkeleri için kritik bir güvenlik gereksinimidir. Zorunlu tünel kazma olmadan, Azure'daki VM'lerinizden Gelen Internet'e bağlı trafik, trafiği denetlemenize veya denetlemenize izin vermeden azure ağ altyapısından doğrudan Internet'e geçer. Yetkisiz Internet erişimi, bilgi ifşasına veya diğer güvenlik ihlallerine yol açabilir.



[!INCLUDE [vpn-gateway-classic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

Bu makalede, Kaynak Yöneticisi dağıtım modeli kullanılarak oluşturulan sanal ağlar için zorunlu tünel yapılandırma yoluyla size yol. Zorunlu tünelleme portal üzerinden değil, PowerShell kullanılarak yapılandırılabilir. Klasik dağıtım modeli için zorunlu tünel yapılandırmak istiyorsanız, aşağıdaki açılır listeden klasik makaleyi seçin:

> [!div class="op_single_selector"]
> * [PowerShell - Klasik](vpn-gateway-about-forced-tunneling.md)
> * [PowerShell - Resource Manager](vpn-gateway-forced-tunneling-rm.md)
> 
> 

## <a name="about-forced-tunneling"></a>Zorunlu tünel leme hakkında

Aşağıdaki diyagram, zorunlu tünellemenin nasıl çalıştığını göstermektedir. 

![Zorunlu Tünel](./media/vpn-gateway-forced-tunneling-rm/forced-tunnel.png)

Yukarıdaki örnekte, Frontend alt ağı tünele zorlanmaz. Frontend alt netindeki iş yükleri, Internet'ten gelen müşteri isteklerini doğrudan kabul etmeye ve yanıtlamaya devam edebilir. Orta seviye ve Arka uç alt ağları tünele atılmaya zorlanır. Bu iki alt ağdan Internet'e giden tüm bağlantılar, S2S VPN tünellerinden biri aracılığıyla zorunlu veya şirket içi bir siteye yönlendirilecektir.

Bu, Azure'daki sanal makinelerinizden veya bulut hizmetlerinizden Gelen Internet erişimini kısıtlamanıza ve denetlemenize olanak sağlarken, çok katmanlı hizmet mimarinizi etkinleştirmeye devam etmenizi sağlar. Sanal ağlarınızda Internet'e bakan iş yükleri yoksa, tüm sanal ağlara zorunlu tünel uygulama da uygulayabilirsiniz.

## <a name="requirements-and-considerations"></a>Gereksinimler ve hususlar

Azure'da zorunlu tünel leme, sanal ağ kullanıcı tanımlı rotalar üzerinden yapılandırılır. Trafiği şirket içi bir siteye yönlendirmek, Varsayılan Rota olarak Azure VPN ağ geçidine ifade edilir. Kullanıcı tanımlı yönlendirme ve sanal ağlar hakkında daha fazla bilgi [için, Kullanıcı tanımlı rotalar ve IP yönlendirme](../virtual-network/virtual-networks-udr-overview.md)bakın.

* Her sanal ağ alt ağının yerleşik, sistem yönlendirme tablosu vardır. Sistem yönlendirme tablosuaşağıdaki üç rota grubuna sahiptir:
  
  * **Yerel VNet rotaları:** Doğrudan aynı sanal ağdaki hedef VM'lere.
  * **Şirket içi güzergahlar:** Azure VPN ağ geçidine.
  * **Varsayılan rota:** Doğrudan Internet'e. Önceki iki rota nın kapsamına giren özel IP adreslerine giden paketler bırakılır.
* Bu yordam, varsayılan bir rota eklemek için yönlendirme tablosu oluşturmak ve daha sonra bu alt ağlarda zorunlu tünel oluşturmayı etkinleştirmek için yönlendirme tablosunu VNet alt ağınıza ilişkilendirmek için kullanıcı tanımlı yolları (UDR) kullanır.
* Zorunlu tünelleme, rota tabanlı VPN ağ geçidiolan bir VNet ile ilişkilendirilmelidir. Sanal ağa bağlı binalar arası yerel siteler arasında bir "varsayılan site" ayarlamanız gerekir. Ayrıca, şirket içi VPN aygıtı 0.0.0.0/0 kullanılarak trafik seçici olarak yapılandırılmalıdır. 
* ExpressRoute zorunlu tünelleme bu mekanizma üzerinden yapılandırılmamıştır, ancak bunun yerine, ExpressRoute BGP eşleme oturumları aracılığıyla varsayılan bir rotanın reklamını yaparak etkinleştirilir. Daha fazla bilgi için [ExpressRoute Belgeleri'ne](https://azure.microsoft.com/documentation/services/expressroute/)bakın.

## <a name="configuration-overview"></a>Yapılandırmaya genel bakış

Aşağıdaki yordam, bir kaynak grubu ve bir VNet oluşturmanıza yardımcı olur. Daha sonra bir VPN ağ geçidi oluşturur ve zorunlu tünel oluşturmayı yapılandırırsınız. Bu yordamda, sanal ağ 'MultiTier-VNet' üç alt ağlar vardır: 'Frontend', 'Midtier', ve 'Backend', dört çapraz bağlantıları ile: 'DefaultSiteHQ', ve üç Şube.

Yordam adımları, 'DefaultSiteHQ'yu zorunlu tünelleme için varsayılan site bağlantısı olarak ayarlar ve 'Midtier' ve 'Backend' alt ağlarını zorunlu tünelleme kullanacak şekilde yapılandırın.

## <a name="before-you-begin"></a><a name="before"></a>Başlamadan önce

Azure Resource Manager PowerShell cmdlet'lerinin en son sürümünü yükleyin. PowerShell cmdlet'lerini yükleme hakkında daha fazla bilgi için bkz. [Azure PowerShell'i yükleme ve yapılandırma](/powershell/azure/overview).

> [!IMPORTANT]
> PowerShell cmdlets en son sürümünü yükleme gereklidir. Aksi takdirde, bazı cmdlets çalıştırırken doğrulama hataları alabilirsiniz.
>
>

### <a name="to-log-in"></a>Oturum açmak için

[!INCLUDE [To log in](../../includes/vpn-gateway-cloud-shell-ps-login.md)]

## <a name="configure-forced-tunneling"></a>Zorlamalı tünel yapılandırma

> [!NOTE]
> "Bu cmdletin çıktı nesnesi türü, ileriki bir sürümde değiştirilecektir" diyen uyarılar görebilirsiniz. Bu beklenen davranıştır ve bu uyarıları güvenle yoksayabilirsiniz.
>
>


1. Bir kaynak grubu oluşturun.

   ```powershell
   New-AzResourceGroup -Name 'ForcedTunneling' -Location 'North Europe'
   ```
2. Sanal ağ oluşturun ve alt ağlar belirtin.

   ```powershell 
   $s1 = New-AzVirtualNetworkSubnetConfig -Name "Frontend" -AddressPrefix "10.1.0.0/24"
   $s2 = New-AzVirtualNetworkSubnetConfig -Name "Midtier" -AddressPrefix "10.1.1.0/24"
   $s3 = New-AzVirtualNetworkSubnetConfig -Name "Backend" -AddressPrefix "10.1.2.0/24"
   $s4 = New-AzVirtualNetworkSubnetConfig -Name "GatewaySubnet" -AddressPrefix "10.1.200.0/28"
   $vnet = New-AzVirtualNetwork -Name "MultiTier-VNet" -Location "North Europe" -ResourceGroupName "ForcedTunneling" -AddressPrefix "10.1.0.0/16" -Subnet $s1,$s2,$s3,$s4
   ```
3. Yerel ağ ağ geçitlerini oluşturun.

   ```powershell
   $lng1 = New-AzLocalNetworkGateway -Name "DefaultSiteHQ" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -GatewayIpAddress "111.111.111.111" -AddressPrefix "192.168.1.0/24"
   $lng2 = New-AzLocalNetworkGateway -Name "Branch1" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -GatewayIpAddress "111.111.111.112" -AddressPrefix "192.168.2.0/24"
   $lng3 = New-AzLocalNetworkGateway -Name "Branch2" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -GatewayIpAddress "111.111.111.113" -AddressPrefix "192.168.3.0/24"
   $lng4 = New-AzLocalNetworkGateway -Name "Branch3" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -GatewayIpAddress "111.111.111.114" -AddressPrefix "192.168.4.0/24"
   ```
4. Rota tablosu nu ve rota kuralını oluşturun.

   ```powershell
   New-AzRouteTable –Name "MyRouteTable" -ResourceGroupName "ForcedTunneling" –Location "North Europe"
   $rt = Get-AzRouteTable –Name "MyRouteTable" -ResourceGroupName "ForcedTunneling" 
   Add-AzRouteConfig -Name "DefaultRoute" -AddressPrefix "0.0.0.0/0" -NextHopType VirtualNetworkGateway -RouteTable $rt
   Set-AzRouteTable -RouteTable $rt
   ```
5. Rota tablosunu Midtier ve Backend alt ağlarıyla ilişkilendirin.

   ```powershell
   $vnet = Get-AzVirtualNetwork -Name "MultiTier-Vnet" -ResourceGroupName "ForcedTunneling"
   Set-AzVirtualNetworkSubnetConfig -Name "MidTier" -VirtualNetwork $vnet -AddressPrefix "10.1.1.0/24" -RouteTable $rt
   Set-AzVirtualNetworkSubnetConfig -Name "Backend" -VirtualNetwork $vnet -AddressPrefix "10.1.2.0/24" -RouteTable $rt
   Set-AzVirtualNetwork -VirtualNetwork $vnet
   ```
6. Sanal ağ ağ ağ geçidini oluşturun. Bu adımın tamamlanması biraz zaman alır, bazen 45 dakika veya daha fazla, çünkü ağ geçidi oluşturuyor ve yapılandırıyorsunuz. GatewaySKU değeriyle ilgili Doğrulama Seti hatalarını görürseniz, [PowerShell cmdlets'in en son sürümünü yüklediğinizi](#before)doğrulayın. PowerShell cmdlets'in en son sürümü, en son Ağ Geçidi STU'ları için yeni doğrulanmış değerleri içerir.

   ```powershell
   $pip = New-AzPublicIpAddress -Name "GatewayIP" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -AllocationMethod Dynamic
   $gwsubnet = Get-AzVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
   $ipconfig = New-AzVirtualNetworkGatewayIpConfig -Name "gwIpConfig" -SubnetId $gwsubnet.Id -PublicIpAddressId $pip.Id
   New-AzVirtualNetworkGateway -Name "Gateway1" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -IpConfigurations $ipconfig -GatewayType Vpn -VpnType RouteBased -GatewaySku VpnGw1 -EnableBgp $false
   ```
7. Varsayılan bir siteyi sanal ağ ağ geçidine atayın. **-GatewayDefaultSite,** zorunlu yönlendirme yapılandırmasının çalışmasını sağlayan cmdlet parametresitir, bu nedenle bu ayarı düzgün bir şekilde yapılandırmaya özen gösteriyor. 

   ```powershell
   $LocalGateway = Get-AzLocalNetworkGateway -Name "DefaultSiteHQ" -ResourceGroupName "ForcedTunneling"
   $VirtualGateway = Get-AzVirtualNetworkGateway -Name "Gateway1" -ResourceGroupName "ForcedTunneling"
   Set-AzVirtualNetworkGatewayDefaultSite -GatewayDefaultSite $LocalGateway -VirtualNetworkGateway $VirtualGateway
   ```
8. Siteden Siteye VPN bağlantılarını kurun.

   ```powershell
   $gateway = Get-AzVirtualNetworkGateway -Name "Gateway1" -ResourceGroupName "ForcedTunneling"
   $lng1 = Get-AzLocalNetworkGateway -Name "DefaultSiteHQ" -ResourceGroupName "ForcedTunneling" 
   $lng2 = Get-AzLocalNetworkGateway -Name "Branch1" -ResourceGroupName "ForcedTunneling" 
   $lng3 = Get-AzLocalNetworkGateway -Name "Branch2" -ResourceGroupName "ForcedTunneling" 
   $lng4 = Get-AzLocalNetworkGateway -Name "Branch3" -ResourceGroupName "ForcedTunneling" 
    
   New-AzVirtualNetworkGatewayConnection -Name "Connection1" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -VirtualNetworkGateway1 $gateway -LocalNetworkGateway2 $lng1 -ConnectionType IPsec -SharedKey "preSharedKey"
   New-AzVirtualNetworkGatewayConnection -Name "Connection2" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -VirtualNetworkGateway1 $gateway -LocalNetworkGateway2 $lng2 -ConnectionType IPsec -SharedKey "preSharedKey"
   New-AzVirtualNetworkGatewayConnection -Name "Connection3" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -VirtualNetworkGateway1 $gateway -LocalNetworkGateway2 $lng3 -ConnectionType IPsec -SharedKey "preSharedKey"
   New-AzVirtualNetworkGatewayConnection -Name "Connection4" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -VirtualNetworkGateway1 $gateway -LocalNetworkGateway2 $lng4 -ConnectionType IPsec -SharedKey "preSharedKey"
    
   Get-AzVirtualNetworkGatewayConnection -Name "Connection1" -ResourceGroupName "ForcedTunneling"
   ```
