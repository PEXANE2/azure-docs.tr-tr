---
title: Azure VPN ağ geçitlerine bağlanmak için iş ortağı VPN aygıt yapılandırmaları
description: Bu makalede, Azure VPN ağ geçitlerine bağlanmak için iş ortağı VPN aygıt yapılandırmalarına genel bir bakış sağlanmaktadır.
services: vpn-gateway
author: yushwang
ms.service: vpn-gateway
ms.topic: article
ms.date: 06/20/2017
ms.author: yushwang
ms.openlocfilehash: b914afaa6725920078da309981bcda5bb765e155
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79279409"
---
# <a name="overview-of-partner-vpn-device-configurations"></a>İş ortağı VPN cihaz yapılandırmalarına genel bakış
Bu makalede, Azure VPN ağ geçitlerine bağlanmak için şirket içi VPN aygıtlarını yapılandırmaya genel bir bakış sağlanmaktadır. Örnek bir Azure sanal ağ ve VPN ağ geçidi kurulumu, aynı parametreleri kullanarak farklı şirket içi VPN aygıt yapılandırmalarına nasıl bağlanabileceğinizi göstermek için kullanılır.



## <a name="device-requirements"></a>Cihaz gereksinimleri
Azure VPN ağ geçitleri, siteden siteye (S2S) VPN tünelleri için standart IPsec/IKE protokol paketlerini kullanır. Azure VPN ağ geçitleri için IPsec/IKE parametreleri ve şifreleme algoritmalarının listesi için [VPN aygıtları hakkında](vpn-gateway-about-vpn-devices.md)bilgi edinin. Ayrıca, [şifreleme gereksinimleri hakkında'da](vpn-gateway-about-compliance-crypto.md)açıklandığı gibi belirli bir bağlantıiçin tam algoritmaları ve anahtar güçlü yanlarını belirtebilirsiniz.

## <a name="single-vpn-tunnel"></a><a name ="singletunnel"></a>Tek VPN tüneli
Örnekteki ilk yapılandırma, Azure VPN ağ geçidi ile şirket içi VPN aygıtı arasındaki tek bir S2S VPN tünelinden oluşur. İsteğe bağlı olarak [VPN tüneli boyunca Kenarlık Ağ Geçidi Protokolü 'nü (BGP)](#bgp)yapılandırabilirsiniz.

![Tek bir S2S VPN tünelinin diyagramı](./media/vpn-gateway-3rdparty-device-config-overview/singletunnel.png)

Tek bir VPN tüneli kurmak için adım adım talimatlar için [siteden siteye bağlantı yapılandır'a](vpn-gateway-howto-site-to-site-resource-manager-portal.md)bakın. Aşağıdaki bölümler, örnek yapılandırmaiçin bağlantı parametrelerini belirtir ve başlamanıza yardımcı olacak bir PowerShell komut dosyası sağlar.

### <a name="connection-parameters"></a>Bağlantı parametreleri
Bu bölümde, önceki bölümlerde açıklanan örneklerin parametreleri listelenir.

| **Parametre**                | **Değer**                    |
| ---                          | ---                          |
| Sanal ağ adresi önekleri        | 10.11.0.0/16<br>10.12.0.0/16 |
| Azure VPN ağ geçidi IP         | Azure VPN Ağ Geçidi IP         |
| Şirket içi adres önekleri | 10.51.0.0/16<br>10.52.0.0/16 |
| Şirket içi VPN cihazı IP    | Şirket içi VPN cihazı IP    |
| * Sanal ağ BGP ASN                | 65010                        |
| * Azure BGP eş IP           | 10.12.255.30                 |
| * Şirket içi BGP ASN         | 65050                        |
| * Şirket içi BGP eş IP     | 10.52.255.254                |

\*Yalnızca BGP için isteğe bağlı parametre.

### <a name="sample-powershell-script"></a>Örnek PowerShell komut dosyası
Bu bölümde başlamak için örnek bir komut dosyası sağlar. Ayrıntılı talimatlar için [PowerShell'i kullanarak S2S VPN bağlantısı oluşturma'ya](vpn-gateway-create-site-to-site-rm-powershell.md)bakın.

```powershell
# Declare your variables

$Sub1          = "Replace_With_Your_Subscription_Name"
$RG1           = "TestRG1"
$Location1     = "East US 2"
$VNetName1     = "TestVNet1"
$FESubName1    = "FrontEnd"
$BESubName1    = "Backend"
$GWSubName1    = "GatewaySubnet"
$VNetPrefix11  = "10.11.0.0/16"
$VNetPrefix12  = "10.12.0.0/16"
$FESubPrefix1  = "10.11.0.0/24"
$BESubPrefix1  = "10.12.0.0/24"
$GWSubPrefix1  = "10.12.255.0/27"
$VNet1ASN      = 65010
$DNS1          = "8.8.8.8"
$GWName1       = "VNet1GW"
$GWIPName1     = "VNet1GWIP"
$GWIPconfName1 = "gwipconf1"
$Connection15  = "VNet1toSite5"
$LNGName5      = "Site5"
$LNGPrefix50   = "10.52.255.254/32"
$LNGPrefix51   = "10.51.0.0/16"
$LNGPrefix52   = "10.52.0.0/16"
$LNGIP5        = "Your_VPN_Device_IP"
$LNGASN5       = 65050
$BGPPeerIP5    = "10.52.255.254"

# Connect to your subscription and create a new resource group

Connect-AzAccount
Select-AzSubscription -SubscriptionName $Sub1
New-AzResourceGroup -Name $RG1 -Location $Location1

# Create virtual network

$fesub1 = New-AzVirtualNetworkSubnetConfig -Name $FESubName1 -AddressPrefix $FESubPrefix1 $besub1 = New-AzVirtualNetworkSubnetConfig -Name $BESubName1 -AddressPrefix $BESubPrefix1
$gwsub1 = New-AzVirtualNetworkSubnetConfig -Name $GWSubName1 -AddressPrefix $GWSubPrefix1

New-AzVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1 -Location $Location1 -AddressPrefix $VNetPrefix11,$VNetPrefix12 -Subnet $fesub1,$besub1,$gwsub1

# Create VPN gateway

$gwpip1    = New-AzPublicIpAddress -Name $GWIPName1 -ResourceGroupName $RG1 -Location $Location1 -AllocationMethod Dynamic
$vnet1     = Get-AzVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1
$subnet1   = Get-AzVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet1
$gwipconf1 = New-AzVirtualNetworkGatewayIpConfig -Name $GWIPconfName1 -Subnet $subnet1 -PublicIpAddress $gwpip1

New-AzVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1 -Location $Location1 -IpConfigurations $gwipconf1 -GatewayType Vpn -VpnType RouteBased -GatewaySku VpnGw1 -Asn $VNet1ASN

# Create local network gateway

New-AzLocalNetworkGateway -Name $LNGName5 -ResourceGroupName $RG1 -Location $Location1 -GatewayIpAddress $LNGIP5 -AddressPrefix $LNGPrefix51,$LNGPrefix52 -Asn $LNGASN5 -BgpPeeringAddress $BGPPeerIP5

# Create the S2S VPN connection

$vnet1gw = Get-AzVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
$lng5gw  = Get-AzLocalNetworkGateway -Name $LNGName5 -ResourceGroupName $RG1

New-AzVirtualNetworkGatewayConnection -Name $Connection15 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng5gw -Location $Location1 -ConnectionType IPsec -SharedKey 'AzureA1b2C3' -EnableBGP $False
```

### <a name="optional-use-custom-ipsecike-policy-with-usepolicybasedtrafficselectors"></a><a name ="policybased"></a>(İsteğe bağlı) UsePolicyBasedTrafficSelectors ile özel IPsec/IKE ilkesini kullanma
VPN aygıtlarınız rota tabanlı veya VTI tabanlı yapılandırmalar gibi herhangi bir trafik seçicisini desteklemiyorsa, [UsePolicyBasedTrafficSelectors](vpn-gateway-connect-multiple-policybased-rm-ps.md) seçeneğiyle özel bir IPsec/IKE ilkesi oluşturun.

> [!IMPORTANT]
> Bağlantıda **UsePolicyBasedTrafficSelectors** seçeneğini etkinleştirmek için bir IPsec/IKE ilkesi oluşturmanız gerekir.


Örnek komut dosyası, aşağıdaki algoritmalar ve parametreleriçeren bir IPsec/IKE ilkesi oluşturur:
* İkEv2: AES256, SHA384, DHGroup24
* IPsec: AES256, SHA1, PFS24, SA Ömür 7.200 saniye ve 20.480.000 KB (20 GB)

Komut dosyası IPsec/IKE ilkesini uygular ve bağlantıda **UsePolicyBasedTrafficSelectors** seçeneğini etkinleştirir.

```powershell
$ipsecpolicy5 = New-AzIpsecPolicy -IkeEncryption AES256 -IkeIntegrity SHA384 -DhGroup DHGroup24 -IpsecEncryption AES256 -IpsecIntegrity SHA1 -PfsGroup PFS24 -SALifeTimeSeconds 7200 -SADataSizeKilobytes 20480000

$vnet1gw = Get-AzVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
$lng5gw  = Get-AzLocalNetworkGateway -Name $LNGName5 -ResourceGroupName $RG1

New-AzVirtualNetworkGatewayConnection -Name $Connection15 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng5gw -Location $Location1 -ConnectionType IPsec -SharedKey 'AzureA1b2C3' -EnableBGP $False -IpsecPolicies $ipsecpolicy5 -UsePolicyBasedTrafficSelectors $True
```

### <a name="optional-use-bgp-on-s2s-vpn-connection"></a><a name ="bgp"></a>(İsteğe bağlı) S2S VPN bağlantısında BGP kullanın
S2S VPN bağlantısını oluşturduğunuzda, isteğe bağlı olarak [VPN ağ geçidi için BGP](vpn-gateway-bgp-resource-manager-ps.md)kullanabilirsiniz. Bu yaklaşımın iki farkı vardır:

* Şirket içi adres önekleri tek bir ana bilgisayar adresi olabilir. Şirket içi BGP eş IP adresi aşağıdaki gibi belirtilir:

    ```powershell
    New-AzLocalNetworkGateway -Name $LNGName5 -ResourceGroupName $RG1 -Location $Location1 -GatewayIpAddress $LNGIP5 -AddressPrefix $LNGPrefix50 -Asn $LNGASN5 -BgpPeeringAddress $BGPPeerIP5
    ```

* Bağlantıyı oluşturduğunuzda, **-EnableBGP** seçeneğini $True için ayarlamanız gerekir:

    ```powershell
    New-AzVirtualNetworkGatewayConnection -Name $Connection15 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng5gw -Location $Location1 -ConnectionType IPsec -SharedKey 'AzureA1b2C3' -EnableBGP $True
    ```

## <a name="next-steps"></a>Sonraki adımlar
Etkin-etkin VPN ağ geçitlerini ayarlamak için adım adım talimatlar için, [tesisler arası ve VNet'den VNet'e bağlantılar için etkin-etkin VPN ağ geçitlerini yapılandırma](vpn-gateway-activeactive-rm-powershell.md)ya da geliştirme'ye bakın.

