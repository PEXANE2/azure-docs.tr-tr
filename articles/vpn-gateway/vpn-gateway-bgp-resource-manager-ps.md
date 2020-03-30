---
title: "Azure VPN Ağ Geçidi: BGP'yi yapılandırın: PowerShell"
description: Bu makale, Azure Kaynak Yöneticisi ve PowerShell'i kullanarak BGP'yi Azure VPN Ağ Geçitleri ile yapılandırmakonusunda size yardımcı olur.
services: vpn-gateway
author: yushwang
ms.service: vpn-gateway
ms.topic: article
ms.date: 04/12/2017
ms.author: yushwang
ms.openlocfilehash: 78147a96d6d9e92c2602b6a83cbed743cf2abf37
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77152049"
---
# <a name="how-to-configure-bgp-on-azure-vpn-gateways-using-powershell"></a>PowerShell kullanarak Azure VPN Ağ Geçitlerinde BGP nasıl yapılandırılır?
Bu makale, BGP'yi, Kaynak Yöneticisi dağıtım modelini ve PowerShell'i kullanarak, tesisler arası Site-to-Site (S2S) VPN bağlantısı ve VNet-to-VNet bağlantısında etkinleştirme adımlarında size iletilmesine yardımcı olur.



## <a name="about-bgp"></a>BGP hakkında
BGP iki veya daha fazla ağ arasında yönlendirme ve ulaşılabilirlik bilgilerini takas etmek üzere İnternet’te yaygın olarak kullanılan standart yönlendirme protokolüdür. BGP, her iki ağ geçidini ön eklerin ilgili ağ geçitlerinden veya yönlendiricilerden geçmeye yönelik kullanılabilirliği ve ulaşılabilirliği konusunda bilgilendiren “yolları” değiştirmek amacıyla, Azure VPN Gateways’i ve BGP eşlikleri veya komşuları olarak adlandırılan şirket içi VPN cihazlarınızı etkinleştirir. BGP ayrıca bir BGP ağ geçidinin öğrendiği yolları bir BGP eşliğinden diğer tüm BGP eşliklerine yayarak birden fazla ağ arasında geçiş yönlendirmesi sağlayabilir.

BGP'nin yararları hakkında daha fazla tartışma yapmak ve BGP kullanmanın teknik gereksinimlerini ve dikkatlerini anlamak için [Azure VPN Ağ Geçitleri ile BGP'ye Genel Bakış'a](vpn-gateway-bgp-overview.md) bakın.

## <a name="getting-started-with-bgp-on-azure-vpn-gateways"></a>Azure VPN ağ geçitlerinde BGP ile başlarken

Bu makalede, aşağıdaki görevleri yapmak için adımlar üzerinden size yol:

* [Bölüm 1 - Azure VPN ağ geçidinizde BGP'yi etkinleştirme](#enablebgp)
* Bölüm 2 - BGP ile bir tesis arası bağlantı kurun
* [Bölüm 3 - BGP ile VNet-to-VNet bağlantısı kurun](#v2vbgp)

Yönergelerin her bölümü, ağ bağlantınızda BGP'yi etkinleştirmek için temel bir yapı taşı oluşturur. Üç parçayı da tamamlarsanız, aşağıdaki diyagramda gösterildiği gibi topolojiyi oluşturursunuz:

![BGP topolojisi](./media/vpn-gateway-bgp-resource-manager-ps/bgp-crosspremv2v.png)

İhtiyaçlarınızı karşılayan daha karmaşık, çok atlamalı, geçiş ağı oluşturmak için parçaları birleştirebilirsiniz.

## <a name="part-1---configure-bgp-on-the-azure-vpn-gateway"></a><a name ="enablebgp"></a>Bölüm 1 - Azure VPN Ağ Geçidi'nde BGP'yi yapılandırın
Yapılandırma adımları, aşağıdaki diyagramda gösterildiği gibi Azure VPN ağ geçidinin BGP parametrelerini ayarlar:

![BGP Ağ Geçidi](./media/vpn-gateway-bgp-resource-manager-ps/bgp-gateway.png)

### <a name="before-you-begin"></a>Başlamadan önce
* Azure aboneliğiniz olduğunu doğrulayın. Henüz Azure aboneliğiniz yoksa [MSDN abonelik avantajlarınızı](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) etkinleştirebilir veya [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) için kaydolabilirsiniz.
* Azure Kaynak Yöneticisi PowerShell cmdlets'i yükleyin. PowerShell cmdlet'lerini yükleme hakkında daha fazla bilgi için bkz. [Azure PowerShell'i yükleme ve yapılandırma](/powershell/azure/overview). 

### <a name="step-1---create-and-configure-vnet1"></a>Adım 1 - VNet1 oluşturma ve yapılandırma
#### <a name="1-declare-your-variables"></a>1. Değişkenlerinizi bildirin
Bu alıştırma için, değişkenlerimizi beyan ederek başlarız. Aşağıdaki örnekte, bu alıştırmaiçin değerleri kullanarak değişkenler beyan eder. Üretim için yapılandırma sırasında bu değerleri kendi değerlerinizle değiştirdiğinizden emin olun. Bu tür yapılandırmaları tanımaya başlamak için adımları gözden geçiriyorsanız bu değişkenleri kullanabilirsiniz. Değişkenleri değiştirin, daha sonra kopyalayın ve PowerShell konsolunuza yapıştırın.

```powershell
$Sub1 = "Replace_With_Your_Subscription_Name"
$RG1 = "TestBGPRG1"
$Location1 = "East US"
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
$GWIPName1 = "VNet1GWIP"
$GWIPconfName1 = "gwipconf1"
$Connection12 = "VNet1toVNet2"
$Connection15 = "VNet1toSite5"
```

#### <a name="2-connect-to-your-subscription-and-create-a-new-resource-group"></a>2. Aboneliğinize bağlanın ve yeni bir kaynak grubu oluşturun
Kaynak Yöneticisi cmdlets kullanmak için PowerShell moduna geçtiğinizi unutmayın. Daha fazla bilgi için [Windows PowerShell’i Resource Manager ile kullanma](../powershell-azure-resource-manager.md) konusuna bakın.

PowerShell konsolunuzu açın ve hesabınıza bağlanın. Bağlanmanıza yardımcı olması için aşağıdaki örneği kullanın:

```powershell
Connect-AzAccount
Select-AzSubscription -SubscriptionName $Sub1
New-AzResourceGroup -Name $RG1 -Location $Location1
```

#### <a name="3-create-testvnet1"></a>3. TestVNet1 oluştur
Aşağıdaki örnek, TestVNet1 adında bir sanal ağ ve biri GatewaySubnet, biri FrontEnd ve diğeri Backend olarak adlandırılan üç alt ağ oluşturur. Kendi değerlerinizi yerleştirirken ağ geçidi alt ağınızı özellikle GatewaySubnet olarak adlandırmanız önem taşır. Başka bir ad kullanırsanız ağ geçidi oluşturma işleminiz başarısız olur.

```powershell
$fesub1 = New-AzVirtualNetworkSubnetConfig -Name $FESubName1 -AddressPrefix $FESubPrefix1
$besub1 = New-AzVirtualNetworkSubnetConfig -Name $BESubName1 -AddressPrefix $BESubPrefix1
$gwsub1 = New-AzVirtualNetworkSubnetConfig -Name $GWSubName1 -AddressPrefix $GWSubPrefix1

New-AzVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1 -Location $Location1 -AddressPrefix $VNetPrefix11,$VNetPrefix12 -Subnet $fesub1,$besub1,$gwsub1
```

### <a name="step-2---create-the-vpn-gateway-for-testvnet1-with-bgp-parameters"></a>Adım 2 - BGP parametreleri ile TestVNet1 için VPN Ağ Geçidi oluşturma
#### <a name="1-create-the-ip-and-subnet-configurations"></a>1. IP ve alt ağ yapılandırmaları oluşturun
Sanal ağınız için oluşturacağınız ağ geçidine ayrılacak genel IP adresi isteyin. Ayrıca gerekli alt ağ ve IP yapılandırmalarını tanımlarsınız.

```powershell
$gwpip1 = New-AzPublicIpAddress -Name $GWIPName1 -ResourceGroupName $RG1 -Location $Location1 -AllocationMethod Dynamic

$vnet1 = Get-AzVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1
$subnet1 = Get-AzVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet1
$gwipconf1 = New-AzVirtualNetworkGatewayIpConfig -Name $GWIPconfName1 -Subnet $subnet1 -PublicIpAddress $gwpip1
```

#### <a name="2-create-the-vpn-gateway-with-the-as-number"></a>2. AS numarası ile VPN ağ geçidi oluşturma
TestVNet1 için sanal ağ geçidini oluşturun. BGP, TestVNet1 için ASN (AS Numarası) ayarlamak için Rota Tabanlı VPN ağ geçidi ve ek parametresi -Asn gerektirir. ASN parametresini ayarlamazsanız, ASN 65515 atanır. Bir ağ geçidini oluşturmak biraz zaman alabilir (tamamlanması 30 dakika ya da daha fazla sürer).

```powershell
New-AzVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1 -Location $Location1 -IpConfigurations $gwipconf1 -GatewayType Vpn -VpnType RouteBased -GatewaySku VpnGw1 -Asn $VNet1ASN
```

#### <a name="3-obtain-the-azure-bgp-peer-ip-address"></a>3. Azure BGP Eş IP adresini edinin
Ağ geçidi oluşturulduktan sonra, Azure VPN Ağ Geçidi'ndeki BGP Peer IP adresini almanız gerekir. Bu adres, şirket içi VPN aygıtlarınız için Azure VPN Ağ Geçidi'ni BGP Eş olarak yapılandırmak için gereklidir.

```powershell
$vnet1gw = Get-AzVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
$vnet1gw.BgpSettingsText
```

Son komut, Azure VPN Ağ Geçidi'ndeki ilgili BGP yapılandırmalarını gösterir; örneğin:

```powershell
$vnet1gw.BgpSettingsText
{
    "Asn": 65010,
    "BgpPeeringAddress": "10.12.255.30",
    "PeerWeight": 0
}
```

Ağ geçidi oluşturulduktan sonra, bgp ile binalar arası bağlantı veya VNet-vNet bağlantısı kurmak için bu ağ geçidini kullanabilirsiniz. Aşağıdaki bölümler de egzersizi tamamlamak için basamaklardan geçer.

## <a name="part-2---establish-a-cross-premises-connection-with-bgp"></a><a name ="crossprembbgp"></a>Bölüm 2 - BGP ile bir tesis arası bağlantı kurun

Bir şirket içi bağlantı oluşturmak için, şirket içi VPN aygıtınızı temsil edecek bir Yerel Ağ Ağ Ağ Geçidi ve VPN ağ geçidini yerel ağ ağ geçidine bağlamak için bir Bağlantı oluşturmanız gerekir. Bu adımlar arasında size yol gösteren makaleler olsa da, bu makale, BGP yapılandırma parametrelerini belirtmek için gereken ek özellikleri içerir.

![BGP Cross-Premises için](./media/vpn-gateway-bgp-resource-manager-ps/bgp-crossprem.png)

Devam etmeden önce, bu alıştırmanın [Bölüm 1'ini](#enablebgp) tamamladığınızdan emin olun.

### <a name="step-1---create-and-configure-the-local-network-gateway"></a>Adım 1 - Yerel ağ ağ geçidioluşturma ve yapılandırma

#### <a name="1-declare-your-variables"></a>1. Değişkenlerinizi bildirin

Bu alıştırma, diyagramda gösterilen yapılandırmayı oluşturmaya devam eder. Değerleri, yapılandırma için kullanmak istediğiniz değerlerle değiştirdiğinizden emin olun.

```powershell
$RG5 = "TestBGPRG5"
$Location5 = "East US 2"
$LNGName5 = "Site5"
$LNGPrefix50 = "10.52.255.254/32"
$LNGIP5 = "Your_VPN_Device_IP"
$LNGASN5 = 65050
$BGPPeerIP5 = "10.52.255.254"
```

Yerel ağ ağ geçidi parametreleri ile ilgili dikkat edilmesi gereken birkaç şey:

* Yerel ağ ağ geçidi VPN ağ geçidi yle aynı veya farklı konum ve kaynak grubunda olabilir. Bu örnek, bunları farklı konumlardaki farklı kaynak gruplarında gösterir.
* Yerel ağ ağ geçidi için bildirmeniz gereken önek, VPN aygıtınızdaki BGP Peer IP adresinizin ana bilgisayar adresidir. Bu durumda, "10.52.255.254/32" bir /32 öneki's.
* Anımsatıcı olarak, şirket içi ağlarınız ve Azure VNet'iniz arasında farklı BGP ASN'ler kullanmanız gerekir. Bunlar aynıysa, şirket içi VPN aygıtınız diğer BGP komşularıyla eşlemek için ASN'yi zaten kullanıyorsa VNet ASN'nizi değiştirmeniz gerekir.

Devam etmeden önce 1. Abonelik’e hala bağlı olduğunuzdan emin olun.

#### <a name="2-create-the-local-network-gateway-for-site5"></a>2. Site5 için yerel ağ ağ geçidi oluşturma

Yerel ağ ağ ağ geçidini oluşturmadan önce oluşturulmazsa kaynak grubunu oluşturduğunuzdan emin olun. Yerel ağ ağ ağ geçidi için iki ek parametreye dikkat edin: Asn ve BgpPeerAddress.

```powershell
New-AzResourceGroup -Name $RG5 -Location $Location5

New-AzLocalNetworkGateway -Name $LNGName5 -ResourceGroupName $RG5 -Location $Location5 -GatewayIpAddress $LNGIP5 -AddressPrefix $LNGPrefix50 -Asn $LNGASN5 -BgpPeeringAddress $BGPPeerIP5
```

### <a name="step-2---connect-the-vnet-gateway-and-local-network-gateway"></a>Adım 2 - VNet ağ geçidini ve yerel ağ ağ geçidini bağlayın

#### <a name="1-get-the-two-gateways"></a>1. İki ağ geçidini alın

```powershell
$vnet1gw = Get-AzVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
$lng5gw  = Get-AzLocalNetworkGateway -Name $LNGName5 -ResourceGroupName $RG5
```

#### <a name="2-create-the-testvnet1-to-site5-connection"></a>2. TestVNet1'den Site5 bağlantısına bağlantı oluşturun

Bu adımda, TestVNet1'den Site5'e bağlantı oluşturursunuz. Bu bağlantı için BGP'yi etkinleştirmek için "-EnableBGP $True" belirtmeniz gerekir. Daha önce de belirtildiği gibi, aynı Azure VPN Ağ Geçidi için hem BGP hem de BGP olmayan bağlantılara sahip olmak mümkündür. Bağlantı özelliğinde BGP etkinleştirilmediği sürece, BGP parametreleri her iki ağ geçidinde de yapılandırılmış olsa bile Azure bu bağlantı için BGP'yi etkinleştirmez.

```powershell
New-AzVirtualNetworkGatewayConnection -Name $Connection15 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng5gw -Location $Location1 -ConnectionType IPsec -SharedKey 'AzureA1b2C3' -EnableBGP $True
```

Aşağıdaki örnekte, bu alıştırma için şirket içi VPN aygıtınızdaki BGP yapılandırma bölümüne girdiğiniz parametreler listelenir:

```

- Site5 ASN            : 65050
- Site5 BGP IP         : 10.52.255.254
- Prefixes to announce : (for example) 10.51.0.0/16 and 10.52.0.0/16
- Azure VNet ASN       : 65010
- Azure VNet BGP IP    : 10.12.255.30
- Static route         : Add a route for 10.12.255.30/32, with nexthop being the VPN tunnel interface on your device
- eBGP Multihop        : Ensure the "multihop" option for eBGP is enabled on your device if needed
```

Bağlantı birkaç dakika sonra kurulur ve IPsec bağlantısı kurulduktan sonra BGP izleme oturumu başlar.

## <a name="part-3---establish-a-vnet-to-vnet-connection-with-bgp"></a><a name ="v2vbgp"></a>Bölüm 3 - BGP ile VNet-to-VNet bağlantısı kurun

Bu bölümde, aşağıdaki diyagramda gösterildiği gibi BGP ile VNet-to-VNet bağlantısı ekler:

![VNet-to-VNet için BGP](./media/vpn-gateway-bgp-resource-manager-ps/bgp-vnet2vnet.png)

Aşağıdaki yönergeler önceki adımlardan devam edin. TestVNet1 ve BGP ile VPN Ağ Geçidi oluşturmak ve yapılandırmak için [Bölüm I'i](#enablebgp) tamamlamanız gerekir. 

### <a name="step-1---create-testvnet2-and-the-vpn-gateway"></a>Adım 1 - TestVNet2 ve VPN ağ geçidi oluşturun

Yeni sanal ağın IP adres alanı TestVNet2'nin VNet aralıklarınızın hiçbiriyle örtüşmediğinden emin olmak önemlidir.

Bu örnekte, sanal ağlar aynı aboneye aittir. Farklı abonelikler arasında VNet-vNet bağlantıları ayarlayabilirsiniz. Daha fazla bilgi için [vnet-to-VNet bağlantısını yapılandırın.](vpn-gateway-vnet-vnet-rm-ps.md) BGP'yi etkinleştirmek için bağlantıları oluştururken "-EnableBgp $True" eklediğinizden emin olun.

#### <a name="1-declare-your-variables"></a>1. Değişkenlerinizi bildirin

Değerleri, yapılandırma için kullanmak istediğiniz değerlerle değiştirdiğinizden emin olun.

```powershell
$RG2 = "TestBGPRG2"
$Location2 = "West US"
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
$GWIPName2 = "VNet2GWIP"
$GWIPconfName2 = "gwipconf2"
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

#### <a name="3-create-the-vpn-gateway-for-testvnet2-with-bgp-parameters"></a>3. BGP parametreleri ile TestVNet2 için VPN ağ geçidi oluşturun

VNet'iniz için oluşturacağınız ağ geçidine tahsis edilecek genel bir IP adresi isteyin ve gerekli alt ağ ve IP yapılandırmalarını tanımlayın.

```powershell
$gwpip2    = New-AzPublicIpAddress -Name $GWIPName2 -ResourceGroupName $RG2 -Location $Location2 -AllocationMethod Dynamic

$vnet2     = Get-AzVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2
$subnet2   = Get-AzVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet2
$gwipconf2 = New-AzVirtualNetworkGatewayIpConfig -Name $GWIPconfName2 -Subnet $subnet2 -PublicIpAddress $gwpip2
```

AS numarasıyla VPN ağ geçidini oluşturun. Azure VPN ağ geçitlerinizde varsayılan ASN'yi geçersiz kılmanız gerekir. BGP ve transit yönlendirmeyi etkinleştirmek için bağlı VNet'lerin ASN'leri farklı olmalıdır.

```powershell
New-AzVirtualNetworkGateway -Name $GWName2 -ResourceGroupName $RG2 -Location $Location2 -IpConfigurations $gwipconf2 -GatewayType Vpn -VpnType RouteBased -GatewaySku VpnGw1 -Asn $VNet2ASN
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

Bu adımda, TestVNet1'den TestVNet2'ye, TestVNet2'den TestVNet1'e bağlantı oluşturursunuz.

```powershell
New-AzVirtualNetworkGatewayConnection -Name $Connection12 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -VirtualNetworkGateway2 $vnet2gw -Location $Location1 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3' -EnableBgp $True

New-AzVirtualNetworkGatewayConnection -Name $Connection21 -ResourceGroupName $RG2 -VirtualNetworkGateway1 $vnet2gw -VirtualNetworkGateway2 $vnet1gw -Location $Location2 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3' -EnableBgp $True
```

> [!IMPORTANT]
> Her iki bağlantı için BGP'yi etkinleştirdiğinden emin olun.
> 
> 

Bu adımları tamamladıktan sonra, bağlantı birkaç dakika sonra kurulur. VNet-vNet bağlantısı tamamlandıktan sonra BGP eşleme oturumu açılır.

Bu alıştırmanın üç bölümünü de tamamladıysanız, aşağıdaki ağ topolojisini oluşturmuşsunuz:

![VNet-to-VNet için BGP](./media/vpn-gateway-bgp-resource-manager-ps/bgp-crosspremv2v.png)

## <a name="next-steps"></a>Sonraki adımlar

Bağlantınız tamamlandıktan sonra sanal ağlarınıza sanal makineler ekleyebilirsiniz. Adımlar için bkz. [Sanal Makine Oluşturma](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
