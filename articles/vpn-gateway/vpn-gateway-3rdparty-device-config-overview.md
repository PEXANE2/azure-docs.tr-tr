---
title: Azure VPN ağ geçitlerine bağlanmak için iş ortağı VPN cihaz yapılandırması
description: Bu makalede, Azure VPN ağ geçitlerine bağlanmak için ortak VPN cihaz yapılandırmalarına genel bir bakış sunulmaktadır.
services: vpn-gateway
author: yushwang
ms.service: vpn-gateway
ms.topic: how-to
ms.date: 06/20/2017
ms.author: yushwang
ms.openlocfilehash: 00291373ae1e30eca2fdf5f9435fc2201a492e40
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84988076"
---
# <a name="overview-of-partner-vpn-device-configurations"></a>İş ortağı VPN cihaz yapılandırmalarına genel bakış
Bu makalede, Azure VPN ağ geçitlerine bağlanmak için şirket içi VPN cihazlarını yapılandırmaya ilişkin bir genel bakış sunulmaktadır. Aynı parametreleri kullanarak farklı şirket içi VPN cihaz yapılandırmalarına nasıl bağlanakullanacağınızı göstermek için örnek bir Azure sanal ağ ve VPN ağ geçidi kurulumu kullanılır.



## <a name="device-requirements"></a>Cihaz gereksinimleri
Azure VPN ağ geçitleri, siteden siteye (S2S) VPN tünelleri için standart IPSec/ıKE protokol paketleri kullanır. Azure VPN ağ geçitleri için IPSec/ıKE parametrelerinin ve şifreleme algoritmalarının bir listesi için bkz. [VPN cihazları hakkında](vpn-gateway-about-vpn-devices.md). Ayrıca, [Şifreleme gereksinimleri hakkında](vpn-gateway-about-compliance-crypto.md)bölümünde açıklandığı gibi, belirli bir bağlantı için tam algoritmaları ve anahtar güçlerini de belirtebilirsiniz.

## <a name="single-vpn-tunnel"></a><a name ="singletunnel"></a>Tek VPN tüneli
Örnekteki ilk yapılandırma, Azure VPN ağ geçidi ve şirket içi VPN cihazı arasındaki tek bir S2S VPN tünelinden oluşur. [VPN tüneli genelinde Sınır Ağ Geçidi Protokolü (BGP)](#bgp)isteğe bağlı olarak yapılandırabilirsiniz.

![Tek bir S2S VPN tünelinin diyagramı](./media/vpn-gateway-3rdparty-device-config-overview/singletunnel.png)

Tek bir VPN tüneli ayarlamaya yönelik adım adım yönergeler için bkz. [siteden siteye bağlantı yapılandırma](vpn-gateway-howto-site-to-site-resource-manager-portal.md). Aşağıdaki bölümler, örnek yapılandırma için bağlantı parametrelerini belirtir ve başlamanıza yardımcı olması için bir PowerShell betiği sağlar.

### <a name="connection-parameters"></a>Bağlantı parametreleri
Bu bölümde, önceki bölümlerde açıklanan örneklerin parametreleri listelenmektedir.

| **Parametre**                | **Değer**                    |
| ---                          | ---                          |
| Sanal ağ adresi önekleri        | 10.11.0.0/16<br>10.12.0.0/16 |
| Azure VPN ağ geçidi IP 'si         | Azure VPN Gateway IP 'si         |
| Şirket içi adres ön ekleri | 10.51.0.0/16<br>10.52.0.0/16 |
| Şirket içi VPN cihaz IP 'si    | Şirket içi VPN cihaz IP 'si    |
| * Sanal ağ BGP ASN                | 65010                        |
| * Azure BGP eş IP 'si           | 10.12.255.30                 |
| * Şirket içi BGP ASN         | 65050                        |
| * Şirket içi BGP eşi IP 'si     | 10.52.255.254                |

\*Yalnızca BGP için isteğe bağlı parametre.

### <a name="sample-powershell-script"></a>Örnek PowerShell betiği
Bu bölüm, başlamanıza olanak sağlayan örnek bir betik sağlar. Ayrıntılı yönergeler için bkz. [PowerShell kullanarak S2S VPN bağlantısı oluşturma](vpn-gateway-create-site-to-site-rm-powershell.md).

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

### <a name="optional-use-custom-ipsecike-policy-with-usepolicybasedtrafficselectors"></a><a name ="policybased"></a>Seçim Özel IPSec/ıKE ilkesini UsePolicyBasedTrafficSelectors ile kullanma
VPN cihazlarınız, rota tabanlı veya VTı tabanlı konfigürasyonlar gibi herhangi bir e-postayla trafik seçicileri desteklemiyorsa, [UsePolicyBasedTrafficSelectors](vpn-gateway-connect-multiple-policybased-rm-ps.md) seçeneğiyle özel bir ıPSEC/IKE ilkesi oluşturun.

> [!IMPORTANT]
> Bağlantıda **UsePolicyBasedTrafficSelectors** seçeneğini etkinleştirmek Için bir ıPSEC/IKE ilkesi oluşturmanız gerekir.


Örnek betik, aşağıdaki algoritmalara ve parametrelere sahip bir IPSec/ıKE ilkesi oluşturur:
* Ikev2: AES256, SHA384, DHGroup24
* IPSec: AES256, SHA1, PFS24, SA ömür 7.200 saniye ve 20.480.000 KB (20 GB)

Betik, IPSec/ıKE ilkesini uygular ve bağlantıda **UsePolicyBasedTrafficSelectors** seçeneğini sunar.

```powershell
$ipsecpolicy5 = New-AzIpsecPolicy -IkeEncryption AES256 -IkeIntegrity SHA384 -DhGroup DHGroup24 -IpsecEncryption AES256 -IpsecIntegrity SHA1 -PfsGroup PFS24 -SALifeTimeSeconds 7200 -SADataSizeKilobytes 20480000

$vnet1gw = Get-AzVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
$lng5gw  = Get-AzLocalNetworkGateway -Name $LNGName5 -ResourceGroupName $RG1

New-AzVirtualNetworkGatewayConnection -Name $Connection15 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng5gw -Location $Location1 -ConnectionType IPsec -SharedKey 'AzureA1b2C3' -EnableBGP $False -IpsecPolicies $ipsecpolicy5 -UsePolicyBasedTrafficSelectors $True
```

### <a name="optional-use-bgp-on-s2s-vpn-connection"></a><a name ="bgp"></a>Seçim S2S VPN bağlantısında BGP kullanma
S2S VPN bağlantısını oluşturduğunuzda, isteğe bağlı olarak [VPN Gateway Için BGP](vpn-gateway-bgp-resource-manager-ps.md)'yi kullanabilirsiniz. Bu yaklaşımın iki farkı vardır:

* Şirket içi adres önekleri tek bir ana bilgisayar adresi olabilir. Şirket içi BGP eşi IP adresi şu şekilde belirtilir:

    ```powershell
    New-AzLocalNetworkGateway -Name $LNGName5 -ResourceGroupName $RG1 -Location $Location1 -GatewayIpAddress $LNGIP5 -AddressPrefix $LNGPrefix50 -Asn $LNGASN5 -BgpPeeringAddress $BGPPeerIP5
    ```

* Bağlantıyı oluşturduğunuzda, **-enablebgp** seçeneğini $true olarak ayarlamanız gerekir:

    ```powershell
    New-AzVirtualNetworkGatewayConnection -Name $Connection15 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng5gw -Location $Location1 -ConnectionType IPsec -SharedKey 'AzureA1b2C3' -EnableBGP $True
    ```

## <a name="next-steps"></a>Sonraki adımlar
Etkin-etkin VPN Gateway 'leri ayarlamaya yönelik adım adım yönergeler için bkz. [şirketler arası ve sanal ağdan sanal ağa bağlantılar için etkin-ETKIN VPN ağ geçitleri yapılandırma](vpn-gateway-activeactive-rm-powershell.md).

