---
title: Etkin S2S Azure VPN Ağ Geçidi bağlantılarını yapılandırma
description: Bu makale, Azure Kaynak Yöneticisi ve PowerShell kullanarak Azure VPN Ağ Geçitleri ile etkin olan bağlantıları yapılandırmanız için size yardımcı olur.
services: vpn-gateway
author: yushwang
ms.service: vpn-gateway
ms.topic: article
ms.date: 07/24/2018
ms.author: yushwang
ms.reviewer: cherylmc
ms.openlocfilehash: ec3697208434eb971e47136416f2c2cc541b5cea
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79244647"
---
# <a name="configure-active-active-s2s-vpn-connections-with-azure-vpn-gateways"></a>Etkin S2S VPN bağlantılarını Azure VPN Ağ Geçitleri ile yapılandırma

Bu makalede, Kaynak Yöneticisi dağıtım modelini ve PowerShell'i kullanarak etkin-etkin binalar arası ve VNet'ten VNet'e bağlantılar oluşturmak için gereken adımlara rehberlik eder.



## <a name="about-highly-available-cross-premises-connections"></a>Yüksek kullanılabilir binalar arası bağlantılar hakkında
Tesisler arası ve VNet'e VNet bağlantısı için yüksek kullanılabilirlik elde etmek için, birden çok VPN ağ geçidi dağıtmanız ve ağlarınızla Azure arasında birden çok paralel bağlantı kurmanız gerekir. Bağlantı seçenekleri ve topolojiye genel bir bakış için [Yüksek Kullanılabilirlik Binaları ve VNet'e VNet Bağlantısı](vpn-gateway-highlyavailable.md) bölümüne bakın.

Bu makalede, etkin-etkin çapraz-premises VPN bağlantısı ve iki sanal ağ arasında etkin-etkin bağlantı kurmak için yönergeler sağlar.

* [Bölüm 1 - Azure VPN ağ geçidinizi etkin-etkin modda oluşturun ve yapılandırın](#aagateway)
* [Bölüm 2 - Aktif-etkin çapraz tesis bağlantıları kurun](#aacrossprem)
* [Bölüm 3 - Aktif VNet-vNet bağlantıları kurun](#aav2v)

Zaten bir VPN ağ geçidiniz varsa, şunları yapabilirsiniz:
* [Varolan bir VPN ağ geçidini etkin beklemeden etkin etkine veya tam tersi güncelleştirme](#aaupdate)

İhtiyaçlarınızı karşılayan daha karmaşık ve yüksek kullanılabilir bir ağ topolojisi oluşturmak için bunları bir araya getirebilirsiniz.

> [!IMPORTANT]
> Etkin-etkin modda yalnızca aşağıdaki SUS'lar kullanır: 
>   * VpnGw1, VpnGw2, VpnGw3
>   * HighPerformance (eski eski SUS'lar için)

## <a name="part-1---create-and-configure-active-active-vpn-gateways"></a><a name ="aagateway"></a>Bölüm 1 - Etkin-etkin VPN ağ geçitleri oluşturma ve yapılandırma
Aşağıdaki adımlar, Azure VPN ağ geçidinizi etkin etkin modlarda yapılandıracaktır. Etkin-etkin ve etkin bekleme ağ geçitleri arasındaki temel farklar:

* İki genel IP adresine sahip iki Ağ Geçidi IP yapılandırması oluşturmanız gerekir
* EnableActiveActiveFeature bayrağını ayarlamanız gerekir
* Ağ geçidi SKU VpnGw1, VpnGw2, VpnGw3 veya HighPerformance (eski SKU) olmalıdır.

Diğer özellikler etkin olmayan etkin ağ geçitleri ile aynıdır. 

### <a name="before-you-begin"></a>Başlamadan önce
* Azure aboneliğiniz olduğunu doğrulayın. Henüz Azure aboneliğiniz yoksa [MSDN abonelik avantajlarınızı](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) etkinleştirebilir veya [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) için kaydolabilirsiniz.
* Azure Resource Manager PowerShell cmdlet’lerini yüklemelisiniz. PowerShell cmdlets'i yükleme hakkında daha fazla bilgi için [Azure PowerShell'e Genel Bakış'a](/powershell/azure/overview) bakın.

### <a name="step-1---create-and-configure-vnet1"></a>Adım 1 - VNet1 oluşturma ve yapılandırma
#### <a name="1-declare-your-variables"></a>1. Değişkenlerinizi bildirin
Bu alıştırmaya değişkenlerimizi bildirerek başlayacağız. Aşağıdaki örnekte bu alıştırmada kullanılan değişkenler bildirilmektedir. Üretim için yapılandırma sırasında bu değerleri kendi değerlerinizle değiştirdiğinizden emin olun. Bu tür yapılandırmaları tanımaya başlamak için adımları gözden geçiriyorsanız bu değişkenleri kullanabilirsiniz. Değişkenleri değiştirin, daha sonra kopyalayın ve PowerShell konsolunuza yapıştırın.

```powershell
$Sub1 = "Ross"
$RG1 = "TestAARG1"
$Location1 = "West US"
$VNetName1 = "TestVNet1"
$FESubName1 = "FrontEnd"
$BESubName1 = "Backend"
$GWSubName1 = "GatewaySubnet"
$VNetPrefix11 = "10.11.0.0/16"
$VNetPrefix12 = "10.12.0.0/16"
$FESubPrefix1 = "10.11.0.0/24"
$BESubPrefix1 = "10.12.0.0/24"
$GWSubPrefix1 = "10.12.255.0/27"
$VNet1ASN = 65010
$DNS1 = "8.8.8.8"
$GWName1 = "VNet1GW"
$GW1IPName1 = "VNet1GWIP1"
$GW1IPName2 = "VNet1GWIP2"
$GW1IPconf1 = "gw1ipconf1"
$GW1IPconf2 = "gw1ipconf2"
$Connection12 = "VNet1toVNet2"
$Connection151 = "VNet1toSite5_1"
$Connection152 = "VNet1toSite5_2"
```

#### <a name="2-connect-to-your-subscription-and-create-a-new-resource-group"></a>2. Aboneliğinize bağlanın ve yeni bir kaynak grubu oluşturun
Resource Manager cmdlet’lerini kullanmak için PowerShell moduna geçtiğinizden emin olun. Daha fazla bilgi için [Windows PowerShell’i Resource Manager ile kullanma](../powershell-azure-resource-manager.md) konusuna bakın.

PowerShell konsolunuzu açın ve hesabınıza bağlanın. Bağlanmanıza yardımcı olması için aşağıdaki örneği kullanın:

```powershell
Connect-AzAccount
Select-AzSubscription -SubscriptionName $Sub1
New-AzResourceGroup -Name $RG1 -Location $Location1
```

#### <a name="3-create-testvnet1"></a>3. TestVNet1 oluştur
Aşağıdaki örnekte TestVNet1 adlı bir sanal ağ ve üç alt ağ oluşturulmaktadır. Biri GatewaySubnet, biri FrontEnd, diğeri BackEnd’dir. Kendi değerlerinizi yerleştirirken ağ geçidi alt ağınızı özellikle GatewaySubnet olarak adlandırmanız önem taşır. Başka bir ad kullanırsanız ağ geçidi oluşturma işleminiz başarısız olur.

```powershell
$fesub1 = New-AzVirtualNetworkSubnetConfig -Name $FESubName1 -AddressPrefix $FESubPrefix1
$besub1 = New-AzVirtualNetworkSubnetConfig -Name $BESubName1 -AddressPrefix $BESubPrefix1
$gwsub1 = New-AzVirtualNetworkSubnetConfig -Name $GWSubName1 -AddressPrefix $GWSubPrefix1

New-AzVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1 -Location $Location1 -AddressPrefix $VNetPrefix11,$VNetPrefix12 -Subnet $fesub1,$besub1,$gwsub1
```

### <a name="step-2---create-the-vpn-gateway-for-testvnet1-with-active-active-mode"></a>Adım 2 - Etkin modda TestVNet1 için VPN ağ geçidi oluşturun
#### <a name="1-create-the-public-ip-addresses-and-gateway-ip-configurations"></a>1. Ortak IP adreslerini ve ağ geçidi IP yapılandırmalarını oluşturun
VNet'iniz için oluşturacağınız ağ geçidine tahsis edilecek iki genel IP adresi isteyin. Ayrıca gerekli alt ağ ve IP yapılandırmalarını da tanımlarsınız.

```powershell
$gw1pip1 = New-AzPublicIpAddress -Name $GW1IPName1 -ResourceGroupName $RG1 -Location $Location1 -AllocationMethod Dynamic
$gw1pip2 = New-AzPublicIpAddress -Name $GW1IPName2 -ResourceGroupName $RG1 -Location $Location1 -AllocationMethod Dynamic

$vnet1 = Get-AzVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1
$subnet1 = Get-AzVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet1
$gw1ipconf1 = New-AzVirtualNetworkGatewayIpConfig -Name $GW1IPconf1 -Subnet $subnet1 -PublicIpAddress $gw1pip1
$gw1ipconf2 = New-AzVirtualNetworkGatewayIpConfig -Name $GW1IPconf2 -Subnet $subnet1 -PublicIpAddress $gw1pip2
```

#### <a name="2-create-the-vpn-gateway-with-active-active-configuration"></a>2. Aktif-etkin yapılandırma ile VPN ağ geçidi oluşturma
TestVNet1 için sanal ağ geçidini oluşturun. İki Ağ GeçidiConfig girişi olduğunu ve EnableActiveActiveFeature bayrağının ayarladığını unutmayın. Bir ağ geçidini oluşturmak biraz zaman alabilir (tamamlanması 45 dakika ya da daha fazla sürer).

```powershell
New-AzVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1 -Location $Location1 -IpConfigurations $gw1ipconf1,$gw1ipconf2 -GatewayType Vpn -VpnType RouteBased -GatewaySku VpnGw1 -Asn $VNet1ASN -EnableActiveActiveFeature -Debug
```

#### <a name="3-obtain-the-gateway-public-ip-addresses-and-the-bgp-peer-ip-address"></a>3. Ağ geçidi ortak IP adreslerini ve BGP Peer IP adresini edinin
Ağ geçidi oluşturulduktan sonra, Azure VPN Ağ Geçidi'ndeki BGP Peer IP adresini almanız gerekir. Bu adres, şirket içi VPN aygıtlarınız için Azure VPN Ağ Geçidi'ni BGP Eş olarak yapılandırmak için gereklidir.

```powershell
$gw1pip1 = Get-AzPublicIpAddress -Name $GW1IPName1 -ResourceGroupName $RG1
$gw1pip2 = Get-AzPublicIpAddress -Name $GW1IPName2 -ResourceGroupName $RG1
$vnet1gw = Get-AzVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
```

VPN ağ geçidiniz için ayrılan iki genel IP adresini ve her ağ geçidi örneği için karşılık gelen BGP Peer IP adreslerini göstermek için aşağıdaki cmdlets'i kullanın:

```powershell
PS D:\> $gw1pip1.IpAddress
40.112.190.5

PS D:\> $gw1pip2.IpAddress
138.91.156.129

PS D:\> $vnet1gw.BgpSettingsText
{
  "Asn": 65010,
  "BgpPeeringAddress": "10.12.255.4,10.12.255.5",
  "PeerWeight": 0
}
```

Ağ geçidi örnekleri için ortak IP adreslerinin sırası ve ilgili BGP Peering Adresleri aynıdır. Bu örnekte, 40.112.190.5 genel IP'li ağ geçidi VM, BGP Peering Adresi olarak 10.12.255.4, 138.91.156.129'lu ağ geçidi ise 10.12.255.5'i kullanacaktır. Bu bilgiler, etkin ağ geçidine bağlanan şirket içi VPN aygıtlarınızı ayarlarken gereklidir. Ağ geçidi aşağıdaki diyagramda tüm adresleri içeren olarak gösterilir:

![etkin ağ geçidi](./media/vpn-gateway-activeactive-rm-powershell/active-active-gw.png)

Ağ geçidi oluşturulduktan sonra, etkin olan binalar arası veya VNet'ten VNet bağlantısına bağlantı kurmak için bu ağ geçidini kullanabilirsiniz. Aşağıdaki bölümler de egzersizi tamamlamak için basamaklardan geçer.

## <a name="part-2---establish-an-active-active-cross-premises-connection"></a><a name ="aacrossprem"></a>Bölüm 2 - Aktif-etkin bir tesis içi bağlantısı kurun
Binalar arası bağlantı kurmak için, şirket içi VPN aygıtınızı temsil edecek bir Yerel Ağ Ağ Ağ Geçidi ve Azure VPN ağ geçidini yerel ağ ağ geçidine bağlamak için bir Bağlantı oluşturmanız gerekir. Bu örnekte, Azure VPN ağ geçidi etkin-etkin moddadır. Sonuç olarak, yalnızca bir şirket içi VPN aygıtı (yerel ağ ağ geçidi) ve bir bağlantı kaynağı olmasına rağmen, her iki Azure VPN ağ geçidi örneği de şirket içi aygıtla S2S VPN tünelleri kurar.

Devam etmeden önce, lütfen bu alıştırmanın [Bölüm 1'ini](#aagateway) tamamladığınızdan emin olun.

### <a name="step-1---create-and-configure-the-local-network-gateway"></a>Adım 1 - Yerel ağ ağ geçidioluşturma ve yapılandırma
#### <a name="1-declare-your-variables"></a>1. Değişkenlerinizi bildirin
Bu alıştırma, diyagramda gösterilen yapılandırmayı oluşturmaya devam edecektir. Değerleri, yapılandırma için kullanmak istediğiniz değerlerle değiştirdiğinizden emin olun.

```powershell
$RG5 = "TestAARG5"
$Location5 = "West US"
$LNGName51 = "Site5_1"
$LNGPrefix51 = "10.52.255.253/32"
$LNGIP51 = "131.107.72.22"
$LNGASN5 = 65050
$BGPPeerIP51 = "10.52.255.253"
```

Yerel ağ ağ geçidi parametreleri ile ilgili dikkat edilmesi gereken birkaç şey:

* Yerel ağ ağ geçidi VPN ağ geçidi yle aynı veya farklı konum ve kaynak grubunda olabilir. Bu örnek, bunları farklı kaynak gruplarında ancak aynı Azure konumunda gösterir.
* Yukarıda gösterildiği gibi şirket içinde yalnızca bir VPN aygıtı varsa, etkin-etkin bağlantı BGP protokolü ile veya olmadan çalışabilir. Bu örnek, binalar arası bağlantı için BGP kullanır.
* BGP etkinse, yerel ağ ağ geçidi için bildirmeniz gereken önek, VPN aygıtınızdaki BGP Peer IP adresinizin ana bilgisayar adresidir. Bu durumda, "10.52.255.253/32" bir /32 öneki's.
* Anımsatıcı olarak, şirket içi ağlarınız ve Azure VNet'iniz arasında farklı BGP ASN'ler kullanmanız gerekir. Bunlar aynıysa, şirket içi VPN aygıtınız diğer BGP komşularıyla eşlemek için ASN'yi zaten kullanıyorsa VNet ASN'nizi değiştirmeniz gerekir.

#### <a name="2-create-the-local-network-gateway-for-site5"></a>2. Site5 için yerel ağ ağ geçidi oluşturma
Devam etmeden önce lütfen 1. Abonelik’e hala bağlı olduğunuzdan emin olun. Henüz oluşturulamazsa kaynak grubunu oluşturun.

```powershell
New-AzResourceGroup -Name $RG5 -Location $Location5
New-AzLocalNetworkGateway -Name $LNGName51 -ResourceGroupName $RG5 -Location $Location5 -GatewayIpAddress $LNGIP51 -AddressPrefix $LNGPrefix51 -Asn $LNGASN5 -BgpPeeringAddress $BGPPeerIP51
```

### <a name="step-2---connect-the-vnet-gateway-and-local-network-gateway"></a>Adım 2 - VNet ağ geçidini ve yerel ağ ağ geçidini bağlayın
#### <a name="1-get-the-two-gateways"></a>1. İki ağ geçidini alın

```powershell
$vnet1gw = Get-AzVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
$lng5gw1 = Get-AzLocalNetworkGateway  -Name $LNGName51 -ResourceGroupName $RG5
```

#### <a name="2-create-the-testvnet1-to-site5-connection"></a>2. TestVNet1'den Site5 bağlantısına bağlantı oluşturun
Bu adımda, testvnet1'den Site5_1'a $True ayarlanmış "EnableBGP" ile bağlantı oluşturursunuz.

```powershell
New-AzVirtualNetworkGatewayConnection -Name $Connection151 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng5gw1 -Location $Location1 -ConnectionType IPsec -SharedKey 'AzureA1b2C3' -EnableBGP $True
```

#### <a name="3-vpn-and-bgp-parameters-for-your-on-premises-vpn-device"></a>3. Şirket içi VPN aygıtınız için VPN ve BGP parametreleri
Aşağıdaki örnekte, bu alıştırma için şirket içi VPN cihazınızdaki BGP yapılandırma bölümüne gireceğiniz parametreler listelenir:

```
- Site5 ASN            : 65050
- Site5 BGP IP         : 10.52.255.253
- Prefixes to announce : (for example) 10.51.0.0/16 and 10.52.0.0/16
- Azure VNet ASN       : 65010
- Azure VNet BGP IP 1  : 10.12.255.4 for tunnel to 40.112.190.5
- Azure VNet BGP IP 2  : 10.12.255.5 for tunnel to 138.91.156.129
- Static routes        : Destination 10.12.255.4/32, nexthop the VPN tunnel interface to 40.112.190.5
                         Destination 10.12.255.5/32, nexthop the VPN tunnel interface to 138.91.156.129
- eBGP Multihop        : Ensure the "multihop" option for eBGP is enabled on your device if needed
```

Bağlantı birkaç dakika sonra kurulmalıdır ve IPsec bağlantısı kurulduktan sonra BGP eşleme oturumu başlar. Bu örnek şimdiye kadar yalnızca bir şirket içi VPN aygıtını yapılandırarak aşağıda gösterilen diyagramla sonuçlanmıştır:

![aktif-aktif-çapraz prem](./media/vpn-gateway-activeactive-rm-powershell/active-active.png)

### <a name="step-3---connect-two-on-premises-vpn-devices-to-the-active-active-vpn-gateway"></a>Adım 3 - İki şirket içi VPN aygıtını aktif-etkin VPN ağ geçidine bağlayın
Aynı şirket içi ağda iki VPN aygıtınız varsa, Azure VPN ağ geçidini ikinci VPN aygıtına bağlayarak çift fazlalık elde edebilirsiniz.

#### <a name="1-create-the-second-local-network-gateway-for-site5"></a>1. Site5 için ikinci yerel ağ ağ geçidi ni oluşturma
İkinci yerel ağ ağ geçidinin ağ geçidi IP adresi, adres öneki ve BGP eşleme adresi, aynı şirket içi ağ için önceki yerel ağ ağ ağ geçidiyle çakışmamalıdır.

```powershell
$LNGName52 = "Site5_2"
$LNGPrefix52 = "10.52.255.254/32"
$LNGIP52 = "131.107.72.23"
$BGPPeerIP52 = "10.52.255.254"
```

```powershell
New-AzLocalNetworkGateway -Name $LNGName52 -ResourceGroupName $RG5 -Location $Location5 -GatewayIpAddress $LNGIP52 -AddressPrefix $LNGPrefix52 -Asn $LNGASN5 -BgpPeeringAddress $BGPPeerIP52
```

#### <a name="2-connect-the-vnet-gateway-and-the-second-local-network-gateway"></a>2. VNet ağ geçidini ve ikinci yerel ağ ağ geçidini bağlayın
TestVNet1'den Site5_2'a "EnableBGP" ayarlı bağlantıyı $True

```powershell
$lng5gw2 = Get-AzLocalNetworkGateway -Name $LNGName52 -ResourceGroupName $RG5
```

```powershell
New-AzVirtualNetworkGatewayConnection -Name $Connection152 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng5gw2 -Location $Location1 -ConnectionType IPsec -SharedKey 'AzureA1b2C3' -EnableBGP $True
```

#### <a name="3-vpn-and-bgp-parameters-for-your-second-on-premises-vpn-device"></a>3. İkinci şirket içi VPN aygıtınız için VPN ve BGP parametreleri
Benzer şekilde, aşağıda ikinci VPN cihazına gireceğiniz parametreler listelenir:

```
- Site5 ASN            : 65050
- Site5 BGP IP         : 10.52.255.254
- Prefixes to announce : (for example) 10.51.0.0/16 and 10.52.0.0/16
- Azure VNet ASN       : 65010
- Azure VNet BGP IP 1  : 10.12.255.4 for tunnel to 40.112.190.5
- Azure VNet BGP IP 2  : 10.12.255.5 for tunnel to 138.91.156.129
- Static routes        : Destination 10.12.255.4/32, nexthop the VPN tunnel interface to 40.112.190.5
                         Destination 10.12.255.5/32, nexthop the VPN tunnel interface to 138.91.156.129
- eBGP Multihop        : Ensure the "multihop" option for eBGP is enabled on your device if needed
```

Bağlantı (tüneller) kurulduktan sonra, şirket içi ağınızı ve Azure'unuzu bağlayan çift gereksiz VPN aygıtınız ve tüneliniz olacaktır:

![çift fazlalık-crossprem](./media/vpn-gateway-activeactive-rm-powershell/dual-redundancy.png)

## <a name="part-3---establish-an-active-active-vnet-to-vnet-connection"></a><a name ="aav2v"></a>Bölüm 3 - Etkin-etkin VNet-to-VNet bağlantısı kurun
Bu bölüm, BGP ile etkin vnet-to-VNet bağlantısı oluşturur. 

Aşağıdaki yönergeler, yukarıda listelenen geçmiş adımların devamı niteliğindedir. TestVNet1 ve BGP ile VPN Ağ Geçidi oluşturmak ve yapılandırmak için [Bölüm 1'i](#aagateway) tamamlamanız gerekir. 

### <a name="step-1---create-testvnet2-and-the-vpn-gateway"></a>Adım 1 - TestVNet2 ve VPN ağ geçidi oluşturun
Yeni sanal ağın IP adres alanı TestVNet2'nin VNet aralıklarınızın hiçbiriyle örtüşmediğinden emin olmak önemlidir.

Bu örnekte, sanal ağlar aynı aboneye aittir. Farklı abonelikler arasında VNet-vNet bağlantıları ayarlayabilirsiniz; daha fazla ayrıntı edinmek için lütfen [VNet'ten VNet'e bağlantı yapılat'a](vpn-gateway-vnet-vnet-rm-ps.md) bakın. BGP'yi etkinleştirmek için bağlantıları oluştururken "-EnableBgp $True" eklediğinizden emin olun.

#### <a name="1-declare-your-variables"></a>1. Değişkenlerinizi bildirin
Değerleri, yapılandırma için kullanmak istediğiniz değerlerle değiştirdiğinizden emin olun.

```powershell
$RG2 = "TestAARG2"
$Location2 = "East US"
$VNetName2 = "TestVNet2"
$FESubName2 = "FrontEnd"
$BESubName2 = "Backend"
$GWSubName2 = "GatewaySubnet"
$VNetPrefix21 = "10.21.0.0/16"
$VNetPrefix22 = "10.22.0.0/16"
$FESubPrefix2 = "10.21.0.0/24"
$BESubPrefix2 = "10.22.0.0/24"
$GWSubPrefix2 = "10.22.255.0/27"
$VNet2ASN = 65020
$DNS2 = "8.8.8.8"
$GWName2 = "VNet2GW"
$GW2IPName1 = "VNet2GWIP1"
$GW2IPconf1 = "gw2ipconf1"
$GW2IPName2 = "VNet2GWIP2"
$GW2IPconf2 = "gw2ipconf2"
$Connection21 = "VNet2toVNet1"
$Connection12 = "VNet1toVNet2"
```

#### <a name="2-create-testvnet2-in-the-new-resource-group"></a>2. Yeni kaynak grubunda TestVNet2 oluşturma

```powershell
New-AzResourceGroup -Name $RG2 -Location $Location2

$fesub2 = New-AzVirtualNetworkSubnetConfig -Name $FESubName2 -AddressPrefix $FESubPrefix2
$besub2 = New-AzVirtualNetworkSubnetConfig -Name $BESubName2 -AddressPrefix $BESubPrefix2
$gwsub2 = New-AzVirtualNetworkSubnetConfig -Name $GWSubName2 -AddressPrefix $GWSubPrefix2

New-AzVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2 -Location $Location2 -AddressPrefix $VNetPrefix21,$VNetPrefix22 -Subnet $fesub2,$besub2,$gwsub2
```

#### <a name="3-create-the-active-active-vpn-gateway-for-testvnet2"></a>3. TestVNet2 için aktif-aktif VPN ağ geçidi oluşturun
VNet'iniz için oluşturacağınız ağ geçidine tahsis edilecek iki genel IP adresi isteyin. Ayrıca gerekli alt ağ ve IP yapılandırmalarını da tanımlarsınız.

```powershell
$gw2pip1 = New-AzPublicIpAddress -Name $GW2IPName1 -ResourceGroupName $RG2 -Location $Location2 -AllocationMethod Dynamic
$gw2pip2 = New-AzPublicIpAddress -Name $GW2IPName2 -ResourceGroupName $RG2 -Location $Location2 -AllocationMethod Dynamic

$vnet2 = Get-AzVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2
$subnet2 = Get-AzVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet2
$gw2ipconf1 = New-AzVirtualNetworkGatewayIpConfig -Name $GW2IPconf1 -Subnet $subnet2 -PublicIpAddress $gw2pip1
$gw2ipconf2 = New-AzVirtualNetworkGatewayIpConfig -Name $GW2IPconf2 -Subnet $subnet2 -PublicIpAddress $gw2pip2
```

AS numarası ve "EnableActiveActiveFeature" bayrağıyla VPN ağ geçidini oluşturun. Azure VPN ağ geçitlerinizde varsayılan ASN'yi geçersiz kılmanız gerektiğini unutmayın. BGP ve transit yönlendirmeyi etkinleştirmek için bağlı VNet'lerin ASN'leri farklı olmalıdır.

```powershell
New-AzVirtualNetworkGateway -Name $GWName2 -ResourceGroupName $RG2 -Location $Location2 -IpConfigurations $gw2ipconf1,$gw2ipconf2 -GatewayType Vpn -VpnType RouteBased -GatewaySku VpnGw1 -Asn $VNet2ASN -EnableActiveActiveFeature
```

### <a name="step-2---connect-the-testvnet1-and-testvnet2-gateways"></a>Adım 2 - TestVNet1 ve TestVNet2 ağ geçitlerini bağlayın
Bu örnekte, her iki ağ geçidi de aynı aboneliktedir. Bu adımı aynı PowerShell oturumunda tamamlayabilirsiniz.

#### <a name="1-get-both-gateways"></a>1. Her iki ağ geçidi ni alın
1 Abonelikle oturum açıp bağlandığınızdan emin olun.

```powershell
$vnet1gw = Get-AzVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
$vnet2gw = Get-AzVirtualNetworkGateway -Name $GWName2 -ResourceGroupName $RG2
```

#### <a name="2-create-both-connections"></a>2. Her iki bağlantıyı da oluşturun
Bu adımda, TestVNet1'den TestVNet2'ye, TestVNet2'den TestVNet1'e bağlantı oluşturacaksınız.

```powershell
New-AzVirtualNetworkGatewayConnection -Name $Connection12 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -VirtualNetworkGateway2 $vnet2gw -Location $Location1 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3' -EnableBgp $True

New-AzVirtualNetworkGatewayConnection -Name $Connection21 -ResourceGroupName $RG2 -VirtualNetworkGateway1 $vnet2gw -VirtualNetworkGateway2 $vnet1gw -Location $Location2 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3' -EnableBgp $True
```

> [!IMPORTANT]
> Her iki bağlantı için BGP'yi etkinleştirdiğinden emin olun.
> 
> 

Bu adımları tamamladıktan sonra, bağlantı birkaç dakika içinde kurulacak ve VNet-to-VNet bağlantısı çift artıklıkla tamamlandığında BGP eşleme oturumu açılacaktır:

![aktif-aktif-v2v](./media/vpn-gateway-activeactive-rm-powershell/vnet-to-vnet.png)

## <a name="update-an-existing-vpn-gateway"></a><a name ="aaupdate"></a>Varolan bir VPN ağ geçidini güncelleştirme

Bu bölüm, varolan bir Azure VPN ağ geçidini etkin bekleme modundan etkin etkin moduna veya tam tersi olarak değiştirmenize yardımcı olur.

### <a name="change-an-active-standby-gateway-to-an-active-active-gateway"></a>Etkin standby ağ geçidini etkin etkin ağ geçidine değiştirme

Aşağıdaki örnek, etkin bir bekleme ağ geçidini etkin etkin bir ağ geçidine dönüştürür. Etkin bekleme ağ geçidini etkin etkin olarak değiştirdiğinizde, başka bir genel IP adresi oluşturursunuz ve ardından ikinci bir Ağ Geçidi IP yapılandırması eklersiniz.

#### <a name="1-declare-your-variables"></a>1. Değişkenlerinizi bildirin

Örnekler için kullanılan aşağıdaki parametreleri kendi yapılandırmanız için gereksinim duyduğunuz ayarlarla değiştirin ve bu değişkenleri bildirin.

```powershell
$GWName = "TestVNetAA1GW"
$VNetName = "TestVNetAA1"
$RG = "TestVPNActiveActive01"
$GWIPName2 = "gwpip2"
$GWIPconf2 = "gw1ipconf2"
```

Değişkenleri açıkladıktan sonra, bu örneği PowerShell konsolunuza kopyalayıp yapıştırabilirsiniz.

```powershell
$vnet = Get-AzVirtualNetwork -Name $VNetName -ResourceGroupName $RG
$subnet = Get-AzVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -VirtualNetwork $vnet
$gw = Get-AzVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG
$location = $gw.Location
```

#### <a name="2-create-the-public-ip-address-then-add-the-second-gateway-ip-configuration"></a>2. Ortak IP adresini oluşturun, ardından ikinci ağ geçidi IP yapılandırmasını ekleyin

```powershell
$gwpip2 = New-AzPublicIpAddress -Name $GWIPName2 -ResourceGroupName $RG -Location $location -AllocationMethod Dynamic
Add-AzVirtualNetworkGatewayIpConfig -VirtualNetworkGateway $gw -Name $GWIPconf2 -Subnet $subnet -PublicIpAddress $gwpip2
```

#### <a name="3-enable-active-active-mode-and-update-the-gateway"></a>3. Etkin modu etkinleştirin ve ağ geçidini güncelleştirin

Bu adımda, etkin modu etkinleştirin ve ağ geçidini güncelleştirin. Örnekte, VPN ağ geçidi şu anda eski bir Standart SKU kullanıyor. Ancak, etkin-etkin Standart SKU'yu desteklemez. Eski SKU'yu desteklenen bir sku olarak yeniden boyutlandırmak için (bu durumda, HighPerformance), yalnızca kullanmak istediğiniz desteklenen eski SKU'yu belirtmeniz yeterlidir.

* Bu adımı kullanarak eski bir SKU'yu yeni SKU'lardan birine değiştiremezsiniz. Bir mirası yalnızca desteklenen başka bir eski SKU'ya yeniden boyutlandırabilirsiniz. Örneğin, Standart eski bir SKU ve VpnGw1 geçerli bir SKU olduğundan (VpnGw1 aktif-aktif için desteklenmiş olsa bile) Standart VpnGw1 SKU değiştiremezsiniz. SK'leri yeniden boyutlandırma ve geçirme hakkında daha fazla bilgi için [Ağ Geçidi SK'lerine](vpn-gateway-about-vpngateways.md#gwsku)bakın.

* Mevcut bir SKU yeniden boyutlandırmak istiyorsanız, örneğin VpnGw1 VpnGw3 için, SKU aynı SKU ailesinde olduğu için bu adımı kullanarak yapabilirsiniz. Bunu yapmak için, değeri kullanırsınız:```-GatewaySku VpnGw3```

Bunu ortamınızda kullanırken, ağ geçidini yeniden boyutlandırmanız gerekmiyorsa, -GatewaySku'yu belirtmeniz gerekmez. Bu adımda, powershell'deki ağ geçidi nesnesini gerçek güncelleştirmeyi tetiklemek için ayarlamanız gerektiğine dikkat edin. Bu güncelleştirme, ağ geçidinizi yeniden boyutlandırmasanız bile 30 ila 45 dakika sürebilir.

```powershell
Set-AzVirtualNetworkGateway -VirtualNetworkGateway $gw -EnableActiveActiveFeature -GatewaySku HighPerformance
```

### <a name="change-an-active-active-gateway-to-an-active-standby-gateway"></a>Etkin ağ geçidini etkin bekleme ağ geçidine değiştirme
#### <a name="1-declare-your-variables"></a>1. Değişkenlerinizi bildirin

Örnekler için kullanılan aşağıdaki parametreleri kendi yapılandırmanız için gereksinim duyduğunuz ayarlarla değiştirin ve bu değişkenleri bildirin.

```powershell
$GWName = "TestVNetAA1GW"
$RG = "TestVPNActiveActive01"
```

Değişkenleri açıkladıktan sonra kaldırmak istediğiniz IP yapılandırmasının adını alın.

```powershell
$gw = Get-AzVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG
$ipconfname = $gw.IpConfigurations[1].Name
```

#### <a name="2-remove-the-gateway-ip-configuration-and-disable-the-active-active-mode"></a>2. Ağ geçidi IP yapılandırmasını kaldırın ve etkin modu devre dışı

Ağ geçidi IP yapılandırmasını kaldırmak ve etkin etkin modu devre dışı kaldırmak için bu örneği kullanın. Gerçek güncelleştirmeyi tetiklemek için PowerShell'deki ağ geçidi nesnesini ayarlamanız gerektiğine dikkat edin.

```powershell
Remove-AzVirtualNetworkGatewayIpConfig -Name $ipconfname -VirtualNetworkGateway $gw
Set-AzVirtualNetworkGateway -VirtualNetworkGateway $gw -DisableActiveActiveFeature
```

Bu güncelleştirme 30 ila 45 dakika kadar sürebilir.

## <a name="next-steps"></a>Sonraki adımlar
Bağlantınız tamamlandıktan sonra sanal ağlarınıza sanal makineler ekleyebilirsiniz. Adımlar için bkz. [Sanal Makine Oluşturma](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
