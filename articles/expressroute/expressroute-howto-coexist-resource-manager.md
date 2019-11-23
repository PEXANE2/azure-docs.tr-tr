---
title: 'Configure ExpressRoute and S2S VPN coexisting connections: Azure PowerShell'
description: Configure ExpressRoute and a Site-to-Site VPN connection that can coexist for the Resource Manager model using PowerShell.
services: expressroute
author: charwen
ms.service: expressroute
ms.topic: conceptual
ms.date: 07/01/2019
ms.author: charwen
ms.custom: seodec18
ms.openlocfilehash: 0628de7c436836a8fdb5b00cac1d8e85963ba48e
ms.sourcegitcommit: 4c831e768bb43e232de9738b363063590faa0472
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/23/2019
ms.locfileid: "74423575"
---
# <a name="configure-expressroute-and-site-to-site-coexisting-connections-using-powershell"></a>Configure ExpressRoute and Site-to-Site coexisting connections using PowerShell
> [!div class="op_single_selector"]
> * [PowerShell - Resource Manager](expressroute-howto-coexist-resource-manager.md)
> * [PowerShell - Klasik](expressroute-howto-coexist-classic.md)
> 
> 

This article helps you configure ExpressRoute and Site-to-Site VPN connections that coexist. Siteden Siteye VPN ve ExpressRoute yapılandırma yeteneğine sahip olmanın çeşitli avantajları vardır. You can configure Site-to-Site VPN as a secure failover path for ExpressRoute, or use Site-to-Site VPNs to connect to sites that are not connected through ExpressRoute. Bu makalede iki senaryo için de yapılandırma adımları verilmektedir. Bu tablo Resource Manager dağıtım modelleri için geçerlidir.

Siteden Siteye VPN ve ExpressRoute eşzamanlı bağlantılarını yapılandırmanın çeşitli avantajları vardır:

* ExpressRoute için güvenli bir yük devretme yolu olarak siteden siteye bir VPN yapılandırın. 
* Alternatif olarak ExpressRoute aracılığıyla bağlı olmayan sitelere bağlanmak için Siteden Siteye VPN’ler kullanabilirsiniz. 

İki senaryo için de yapılandırma adımları bu makalede verilmektedir. Bu makale Resource Manager dağıtım modelleri için geçerlidir ve PowerShell kullanır. You can also configure these scenarios using the Azure portal, although documentation is not yet available. You can configure either gateway first. Typically, you will incur no downtime when adding a new gateway or gateway connection.

>[!NOTE]
>ExpressRoute devresi üzerinde bir Siteden Siteye VPN oluşturmak istiyorsanız, lütfen [bu makaleye](site-to-site-vpn-over-microsoft-peering.md) bakınız.
>

## <a name="limits-and-limitations"></a>Sınırlar ve sınırlamalar
* **Geçiş yönlendirmesi desteklenmez.** Siteden Siteye VPN aracılığıyla bağlanan yerel ağınız ve ExpressRoute aracılığıyla bağlanan yerel ağınız arasında (Azure aracılığıyla) yönlendirme yapamazsınız.
* **Temel SKU ağ geçidi desteklenmez.** Hem [ExpressRoute ağ geçidi](expressroute-about-virtual-network-gateways.md) hem de [VPN ağ geçidi](../vpn-gateway/vpn-gateway-about-vpngateways.md) için Temel SKU olmayan bir ağ geçidi kullanmanız gerekir.
* **Yalnızca rota tabanlı VPN ağ geçidi desteklenir.** You must use a route-based [VPN gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md). You also can use a route-based VPN gateway with a VPN connection configured for 'policy-based traffic selectors' as described in [Connect to multiple policy-based VPN devices](../vpn-gateway/vpn-gateway-connect-multiple-policybased-rm-ps.md).
* **VPN ağ geçidiniz için statik rota yapılandırılmalıdır.** Yerel ağınız hem ExpressRoute hem de Siteden Siteye VPN’e bağlıysa Siteden Siteye VPN bağlantısını genel İnternet’e yönlendirebilmeniz için yerel ağınızda statik bir rotanın yapılandırılmış olması gerekir.
* **VPN Gateway defaults to ASN 65515 if not specified.** Azure VPN Gateway supports the BGP routing protocol. You can specify ASN (AS Number) for a virtual network by adding the -Asn switch. If you don't specify this parameter, the default AS number is 65515. You can use any ASN for the configuration, but if you select something other than 65515, you must reset the gateway for the setting to take effect.

## <a name="configuration-designs"></a>Yapılandırma tasarımları
### <a name="configure-a-site-to-site-vpn-as-a-failover-path-for-expressroute"></a>Siteden siteye VPN’i ExpressRoute için bir yük devretme yolu olarak yapılandırma
Siteden siteye bir VPN bağlantısını ExpressRoute için yedek olarak yapılandırabilirsiniz. Bu bağlantı yalnızca Azure özel eşleme yoluna bağlı sanal ağlar için geçerlidir. Azure Microsoft eşlemeleri aracılığıyla erişilebilen hizmetler için VPN tabanlı yük devretme çözümü yoktur. ExpressRoute bağlantı hattı her zaman birincil bağlantıdır. Veriler yalnızca ExpressRoute bağlantı hattı başarısız olursa, Siteden Siteye VPN üzerinden akar. Asimetrik yönlendirmenin önüne geçmek için yerel ağ yapılandırmanız da Siteden Siteye VPN üzerinden ExpressRoute bağlantı hattını tercih etmelidir. ExpressRoute’u alan yönlendirmeler için daha yüksek yerel tercihleri ayarlayarak ExpressRoute yolunu tercih edebilirsiniz. 

> [!NOTE]
> Her iki yol da aynı olduğunda ExpressRoute bağlantı hattı Siteden Siteye VPN’ye tercih edilse de Azure paketin hedefine yönelik yolu seçmek için en uzun ön ek eşleşmesini kullanır.
> 
> 

![Bir arada var olma](media/expressroute-howto-coexist-resource-manager/scenario1.jpg)

### <a name="configure-a-site-to-site-vpn-to-connect-to-sites-not-connected-through-expressroute"></a>ExpressRoute aracılığıyla bağlanılmayan sitelere bağlanmak için Siteden Siteye VPN yapılandırma
Ağınızı bazı sitelerin Azure’a Siteden Siteye VPN üzerinden doğrudan ve bazı sitelerin ExpressRoute üzerinden bağlanması için yapılandırabilirsiniz. 

![Bir arada var olma](media/expressroute-howto-coexist-resource-manager/scenario2.jpg)

> [!NOTE]
> Sanal ağı, geçiş yönlendiricisi olarak yapılandıramazsınız.
> 
> 

## <a name="selecting-the-steps-to-use"></a>Kullanılacak adımları seçme
Aralarından seçim yapabileceğiniz iki farklı yordam kümesi vardır. Seçtiğiniz yapılandırma yordamı, bağlanmak istediğiniz mevcut bir sanal ağ olup olmadığına veya yeni bir sanal ağ oluşturmak isteyip istememenize bağlıdır.

* Bir VNet’im yok ve bir tane oluşturmam gerekiyor.
  
    Zaten bir sanal ağınız yoksa, bu yordam Resource Manager dağıtım modelini kullanarak yeni bir sanal ağ oluşturmak ve yeni ExpressRoute ve Siteden Siteye VPN bağlantıları oluşturmak için size yol gösterir. Sanal ağı yapılandırmak için, [Yeni bir sanal ağ ve bir arada var olabilen bağlantılar oluşturmak için](#new) bölümündeki adımları izleyin.
* Zaten bir Resource Manager dağıtım modeli VNet’im var.
  
    Mevcut bir Siteden Siteye VPN bağlantısı veya ExpressRoute bağlantısına sahip bir sanal ağınız zaten olabilir. Bu senaryoda ağ geçidi alt ağ maskesi /28 veya daha küçükse (/28, /29 vs.) var olan ağ geçidini silmeniz gerekir. [Zaten olan bir VNet için aynı anda mevcut bağlantılar yapılandırma](#add) bölümü, ağ geçidini silme ve ardından yeni ExpressRoute ve Siteden Siteye VPN bağlantıları oluşturma işlemi boyunca size yol gösterir.
  
    Ağ geçidinizi silip yeniden oluşturursanız şirket içi ve dışı karışık bağlantılarınız için kesinti süresi olur. Ancak VM'leriniz ve hizmetleriniz bunu yapmak için yapılandırılmışsa, ağ geçidi yapılandırması sırasında yük dengeleyici üzerinden iletişim kurmaya devam eder.

## <a name="before-you-begin"></a>Başlamadan önce

[!INCLUDE [updated-for-az](../../includes/hybrid-az-ps.md)]

[!INCLUDE [working with cloud shell](../../includes/expressroute-cloudshell-powershell-about.md)]


## <a name="new"></a>Yeni bir sanal ağ ve bir arada var olabilen bağlantılar oluşturmak için
Bu yordamda, bir VNet oluşturma ve bir arada var olabilen Siteden Siteye ve ExpressRoute bağlantıları oluşturma işlemleri adım adım açıklanmıştır. Bu yapılandırma için kullanacağınız cmdlet'ler tanıdıklarınızdan biraz farklı olabilir. Bu yönergelerde belirtilen cmdlet'leri kullandığınızdan emin olun.

1. Sign in and select your subscription.

   [!INCLUDE [sign in](../../includes/expressroute-cloud-shell-connect.md)]
2. Set variables.

   ```azurepowershell-interactive
   $location = "Central US"
   $resgrp = New-AzResourceGroup -Name "ErVpnCoex" -Location $location
   $VNetASN = 65515
   ```
3. Ağ Geçidi Alt Ağı içeren bir sanal ağ oluşturun. Sanal ağ oluşturma hakkında daha fazla bilgi için bkz. [Sanal ağ oluşturma](../virtual-network/manage-virtual-network.md#create-a-virtual-network). Alt ağ oluşturma hakkında daha fazla bilgi için bkz. [Alt ağ oluşturma](../virtual-network/virtual-network-manage-subnet.md#add-a-subnet)
   
   > [!IMPORTANT]
   > Ağ Geçidi Alt Ağı /27 veya daha kısa bir ön ek (örneğin /26 veya /25) olmalıdır.
   > 
   > 
   
    Yeni bir VNet oluşturun.

   ```azurepowershell-interactive
   $vnet = New-AzVirtualNetwork -Name "CoexVnet" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -AddressPrefix "10.200.0.0/16"
   ```
   
    Alt ağlar ekleyin.

   ```azurepowershell-interactive
   Add-AzVirtualNetworkSubnetConfig -Name "App" -VirtualNetwork $vnet -AddressPrefix "10.200.1.0/24"
   Add-AzVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet -AddressPrefix "10.200.255.0/24"
   ```
   
    VNet yapılandırmasını kaydedin.

   ```azurepowershell-interactive
   $vnet = Set-AzVirtualNetwork -VirtualNetwork $vnet
   ```
4. <a name="vpngw"></a>Ardından, Siteden Siteye VPN ağ geçidinizi oluşturun. VPN ağ geçidi yapılandırması hakkında daha fazla bilgi için bkz. [Siteden Siteye bağlantı ile VNet yapılandırma](../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md). GatewaySku yalnızca *VpnGw1*, *VpnGw2*, *VpnGw3*, *Standard* ve *HighPerformance* VPN ağ geçitlerinde desteklenir. ExpressRoute-VPN Gateway ağ geçidi bir arada var olabilen yapılandırmaları, temel SKU'da desteklenmez. VpnType değeri *RouteBased* olmalıdır.

   ```azurepowershell-interactive
   $gwSubnet = Get-AzVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
   $gwIP = New-AzPublicIpAddress -Name "VPNGatewayIP" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -AllocationMethod Dynamic
   $gwConfig = New-AzVirtualNetworkGatewayIpConfig -Name "VPNGatewayIpConfig" -SubnetId $gwSubnet.Id -PublicIpAddressId $gwIP.Id
   New-AzVirtualNetworkGateway -Name "VPNGateway" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -IpConfigurations $gwConfig -GatewayType "Vpn" -VpnType "RouteBased" -GatewaySku "VpnGw1"
   ```
   
    Azure VPN ağ geçidi, BGP yönlendirme protokolünü destekler. Aşağıdaki komuta -Asn anahtarını ekleyerek söz konusu Sanal Ağ için ASN (AS Numarası) belirtebilirsiniz. Bu parametreyi belirtmediğinizde AS numarası varsayılan olarak 65515 şeklinde ayarlanır.

   ```azurepowershell-interactive
   $azureVpn = New-AzVirtualNetworkGateway -Name "VPNGateway" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -IpConfigurations $gwConfig -GatewayType "Vpn" -VpnType "RouteBased" -GatewaySku "VpnGw1" -Asn $VNetASN
   ```
   
    BGP eşleme IP’sini ve Azure’un VPN ağ geçidi için kullandığı AS numarasını $azureVpn.BgpSettings.BgpPeeringAddress ve $azureVpn.BgpSettings.Asn’de bulabilirsiniz. Daha fazla bilgi için bkz. Azure VPN ağ geçidi için [BGP’yi yapılandırma](../vpn-gateway/vpn-gateway-bgp-resource-manager-ps.md).
5. Bir yerel site VPN ağ geçidi varlığı oluşturun. Bu komut, şirket içi VPN ağ geçidinizi yapılandırmaz. Azure VPN ağ geçidinin bağlanabilmesi için ortak IP ve şirket içi adres alanı gibi yerel ağ geçidi ayarlarını paylaşmanıza olanak tanır.
   
    Yerel VPN cihazınız yalnızca statik yönlendirmeyi destekliyorsa statik rotaları aşağıdaki şekilde yapılandırabilirsiniz:

   ```azurepowershell-interactive
   $MyLocalNetworkAddress = @("10.100.0.0/16","10.101.0.0/16","10.102.0.0/16")
   $localVpn = New-AzLocalNetworkGateway -Name "LocalVPNGateway" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -GatewayIpAddress *<Public IP>* -AddressPrefix $MyLocalNetworkAddress
   ```
   
    Yerel VPN cihazınız BGP’yi destekliyorsa ve dinamik yönlendirmeyi etkinleştirmek istiyorsanız yerel VPN cihazınızın kullandığı BGP eşleme IP’sini ve AS numarasını bilmeniz gerekir.

   ```azurepowershell-interactive
   $localVPNPublicIP = "<Public IP>"
   $localBGPPeeringIP = "<Private IP for the BGP session>"
   $localBGPASN = "<ASN>"
   $localAddressPrefix = $localBGPPeeringIP + "/32"
   $localVpn = New-AzLocalNetworkGateway -Name "LocalVPNGateway" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -GatewayIpAddress $localVPNPublicIP -AddressPrefix $localAddressPrefix -BgpPeeringAddress $localBGPPeeringIP -Asn $localBGPASN
   ```
6. Yerel VPN cihazınızı yeni Azure VPN ağ geçidine bağlanmak için yapılandırın. VPN cihazını yapılandırma hakkında daha fazla bilgi için bkz. [VPN Cihazı Yapılandırma](../vpn-gateway/vpn-gateway-about-vpn-devices.md).

7. Azure’da Siteden Siteye ağ geçidini yerel ağ geçidine bağlayın.

   ```azurepowershell-interactive
   $azureVpn = Get-AzVirtualNetworkGateway -Name "VPNGateway" -ResourceGroupName $resgrp.ResourceGroupName
   New-AzVirtualNetworkGatewayConnection -Name "VPNConnection" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -VirtualNetworkGateway1 $azureVpn -LocalNetworkGateway2 $localVpn -ConnectionType IPsec -SharedKey <yourkey>
   ```
 

8. Varolan bir ExpressRoute bağlantı hattına bağlanıyorsanız 8 ve 9. adımları atlayıp 10. adıma geçin. ExpressRoute bağlantı hatlarını yapılandırın. ExpressRoute bağlantı hattını yapılandırma hakkında daha fazla bilgi için bkz. [ExpressRoute bağlantı hattı oluşturma](expressroute-howto-circuit-arm.md).


9. ExpressRoute bağlantı hattı üzerinde Azure özel eşlemeyi yapılandırın. ExpressRoute bağlantı hattı üzerinde Azure özel eşlemeyi yapılandırma hakkında daha fazla bilgi için bkz. [eşlemeyi yapılandırma](expressroute-howto-routing-arm.md)

10. <a name="gw"></a>ExpressRoute ağ geçidi oluşturun. ExpressRoute ağ geçidi yapılandırması hakkında daha fazla bilgi için bkz. [ExpressRoute ağ geçidi yapılandırması](expressroute-howto-add-gateway-resource-manager.md). GatewaySKU değeri *Standard*, *HighPerformance* veya *UltraPerformance* olmalıdır.

    ```azurepowershell-interactive
    $gwSubnet = Get-AzVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
    $gwIP = New-AzPublicIpAddress -Name "ERGatewayIP" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -AllocationMethod Dynamic
    $gwConfig = New-AzVirtualNetworkGatewayIpConfig -Name "ERGatewayIpConfig" -SubnetId $gwSubnet.Id -PublicIpAddressId $gwIP.Id
    $gw = New-AzVirtualNetworkGateway -Name "ERGateway" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -IpConfigurations $gwConfig -GatewayType "ExpressRoute" -GatewaySku Standard
    ```
11. ExpressRoute ağ geçidini ExpressRoute bağlantı hattına bağlayın. Bu adım tamamlandıktan sonra, ExpressRoute aracılığıyla şirket içi ağınız ve Azure arasında bağlantı kurulur. Bağlantı işlemi hakkında daha fazla bilgi için bkz.[VNets’i ExpressRoute’a bağlama](expressroute-howto-linkvnet-arm.md).

    ```azurepowershell-interactive
    $ckt = Get-AzExpressRouteCircuit -Name "YourCircuit" -ResourceGroupName "YourCircuitResourceGroup"
    New-AzVirtualNetworkGatewayConnection -Name "ERConnection" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -VirtualNetworkGateway1 $gw -PeerId $ckt.Id -ConnectionType ExpressRoute
    ```

## <a name="add"></a>Zaten mevcut bir VNet için bir arada var olabilen bağlantılar yapılandırma
Yalnızca bir sanal ağ geçidine (Siteden Siteye VPN ağ geçidi gibi) sahip olan bir sanal ağınız varsa ve farklı türde (ExpressRoute ağ geçidi gibi) bir ağ geçidi eklemek istiyorsanız ağ geçidi alt ağ boyutunu kontrol edin. Ağ geçidi alt ağı /27 veya üzerindeyse aşağıdaki adımları atlayıp bir sonraki bölümdeki adımları izleyerek Siteden Siteye VPN ağ geçidi veya ExpressRoute ağ geçidi ekleyebilirsiniz. Ağ geçidi alt ağı /28 veya /29 ise, önce sanal ağ geçidi silmeniz ve ağ geçidi alt ağı boyutunu artırmanız gerekir. Bu bölümdeki adımlar bunu nasıl yapacağınızı gösterir.

Bu yapılandırma için kullanacağınız cmdlet'ler tanıdıklarınızdan biraz farklı olabilir. Bu yönergelerde belirtilen cmdlet'leri kullandığınızdan emin olun.

1. Mevcut ExpressRoute veya Siteden Siteye VPN ağ geçidini silin.

   ```azurepowershell-interactive 
   Remove-AzVirtualNetworkGateway -Name <yourgatewayname> -ResourceGroupName <yourresourcegroup>
   ```
2. Ağ Geçidi Alt Ağını silin.

   ```azurepowershell-interactive
   $vnet = Get-AzVirtualNetwork -Name <yourvnetname> -ResourceGroupName <yourresourcegroup> Remove-AzVirtualNetworkSubnetConfig -Name GatewaySubnet -VirtualNetwork $vnet
   ```
3. /27 veya daha büyük bir Ağ Geçidi Alt Ağı ekleyin.
   
   > [!NOTE]
   > Sanal ağınızda ağ geçidi alt ağı boyutunu artırmak için yeterli IP adresi kalmadıysa, daha fazla IP adresi alanı eklemeniz gerekir.
   > 
   > 

   ```azurepowershell-interactive
   $vnet = Get-AzVirtualNetwork -Name <yourvnetname> -ResourceGroupName <yourresourcegroup>
   Add-AzVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet -AddressPrefix "10.200.255.0/24"
   ```
   
    VNet yapılandırmasını kaydedin.

   ```azurepowershell-interactive
   $vnet = Set-AzVirtualNetwork -VirtualNetwork $vnet
   ```
4. Bu noktada, hiçbir ağ geçidi olmayan bir sanal ağa sahip olursunuz. To create new gateways and set up the connections, use the following examples:

   Değişkenleri ayarlayın.

    ```azurepowershell-interactive
   $gwSubnet = Get-AzVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
   $gwIP = New-AzPublicIpAddress -Name "ERGatewayIP" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -AllocationMethod Dynamic
   $gwConfig = New-AzVirtualNetworkGatewayIpConfig -Name "ERGatewayIpConfig" -SubnetId $gwSubnet.Id -PublicIpAddressId $gwIP.Id
   ```

   Create the gateway.

   ```azurepowershell-interactive
   $gw = New-AzVirtualNetworkGateway -Name <yourgatewayname> -ResourceGroupName <yourresourcegroup> -Location <yourlocation) -IpConfigurations $gwConfig -GatewayType "ExpressRoute" -GatewaySku Standard
   ```

   Bağlantıyı oluşturun.

   ```azurepowershell-interactive
   $ckt = Get-AzExpressRouteCircuit -Name "YourCircuit" -ResourceGroupName "YourCircuitResourceGroup"
   New-AzVirtualNetworkGatewayConnection -Name "ERConnection" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -VirtualNetworkGateway1 $gw -PeerId $ckt.Id -ConnectionType ExpressRoute
   ```

## <a name="to-add-point-to-site-configuration-to-the-vpn-gateway"></a>VPN ağ geçidine noktadan siteye yapılandırması eklemek için

You can follow the steps below to add Point-to-Site configuration to your VPN gateway in a coexistence setup. To upload the VPN root certificate, you must either install PowerShell locally to your computer, or use the Azure portal.

1. VPN İstemcisi adres havuzunu ekleyin.

   ```azurepowershell-interactive
   $azureVpn = Get-AzVirtualNetworkGateway -Name "VPNGateway" -ResourceGroupName $resgrp.ResourceGroupName
   Set-AzVirtualNetworkGatewayVpnClientConfig -VirtualNetworkGateway $azureVpn -VpnClientAddressPool "10.251.251.0/24"
   ```
2. VPN ağ geçidiniz için VPN kök sertifikasını Azure’a yükleyin. In this example, it's assumed that the root certificate is stored in the local machine where the following PowerShell cmdlets are run and that you are running PowerShell locally. You can also upload the certificate using the Azure portal.

   ```powershell
   $p2sCertFullName = "RootErVpnCoexP2S.cer" 
   $p2sCertMatchName = "RootErVpnCoexP2S" 
   $p2sCertToUpload=get-childitem Cert:\CurrentUser\My | Where-Object {$_.Subject -match $p2sCertMatchName} 
   if ($p2sCertToUpload.count -eq 1){write-host "cert found"} else {write-host "cert not found" exit} 
   $p2sCertData = [System.Convert]::ToBase64String($p2sCertToUpload.RawData) 
   Add-AzVpnClientRootCertificate -VpnClientRootCertificateName $p2sCertFullName -VirtualNetworkGatewayname $azureVpn.Name -ResourceGroupName $resgrp.ResourceGroupName -PublicCertData $p2sCertData
   ```

Noktadan Siteye VPN hakkında daha fazla bilgi içini bkz. [Noktadan Siteye bağlantı yapılandırma](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md).

## <a name="next-steps"></a>Sonraki adımlar
ExpressRoute hakkında daha fazla bilgi için, bkz. [ExpressRoute SSS](expressroute-faqs.md).
