---
title: VNet-to-VNet bağlantıları & S2S VPN için IPsec/IKE ilkesi
titleSuffix: Azure VPN Gateway
description: Azure Kaynak Yöneticisi ve PowerShell kullanarak Azure VPN Ağ Geçitleri ile S2S veya VNet-vNet bağlantıları için IPsec/IKE ilkesini yapılandırın.
services: vpn-gateway
author: yushwang
ms.service: vpn-gateway
ms.topic: article
ms.date: 02/14/2018
ms.author: yushwang
ms.openlocfilehash: eaca48fc354f1cf37635e9729b04eaaaa882ba1c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77161911"
---
# <a name="configure-ipsecike-policy-for-s2s-vpn-or-vnet-to-vnet-connections"></a>S2S VPN veya VNet-VNet bağlantıları için IPsec/IKE ilkesi yapılandırma

Bu makalede, Kaynak Yöneticisi dağıtım modelini ve PowerShell'i kullanarak Siteden Siteye VPN veya VNet-to-VNet bağlantıları için IPsec/IKE ilkesini yapılandırma adımları size iletilmesi.



## <a name="about-ipsec-and-ike-policy-parameters-for-azure-vpn-gateways"></a><a name="about"></a>Azure VPN ağ geçitleri için IPsec ve IKE ilke parametreleri hakkında
IPsec ve IKE protokol standardı çeşitli kombinasyonlarda çok çeşitli şifreleme algoritmalarını destekler. Bunun, tesisler arası ve VNet'tan VNet'e bağlantının uyumluluk veya güvenlik gereksinimlerinizi karşılamasına nasıl yardımcı olabileceğini görmek için [şifreleme gereksinimleri ve Azure VPN ağ geçitleri hakkında](vpn-gateway-about-compliance-crypto.md) bilgi edinin.

Bu makalede, bir IPsec/IKE ilkesi oluşturmak ve yapılandırmak ve yeni veya varolan bir bağlantıiçin geçerli olmak için yönergeler sağlar:

* [Bölüm 1 - IPsec/IKE ilkesini oluşturmak ve ayarlamak için iş akışı](#workflow)
* [Bölüm 2 - Desteklenen şifreleme algoritmaları ve önemli güçlü yönleri](#params)
* [Bölüm 3 - IPsec/IKE ilkesiyle yeni bir S2S VPN bağlantısı oluşturun](#crossprem)
* [Bölüm 4 - IPsec/IKE ilkesiyle yeni bir VNet-vNet bağlantısı oluşturun](#vnet2vnet)
* [Bölüm 5 - Bağlantı için IPsec/IKE ilkesini yönetme (oluşturma, ekleme, kaldırma)](#managepolicy)

> [!IMPORTANT]
> 1. IPsec/IKE ilkesinin yalnızca aşağıdaki ağ geçidi SUS'larında çalıştığını unutmayın:
>    * ***VpnGw1, VpnGw2, VpnGw3*** (rota tabanlı)
>    * ***Standart*** ve ***Yüksek Performans*** (rota tabanlı)
> 2. Belirli bir bağlantı için yalnızca ***bir*** ilke birleşimi belirtebilirsiniz.
> 3. Hem IKE (Ana Mod) hem de IPsec (Hızlı Mod) için tüm algoritmaları ve parametreleri belirtmeniz gerekir. Kısmi ilke belirtimine izin verilmez.
> 4. İlkenin şirket içi VPN aygıtlarınızda desteklenmediğinden emin olmak için VPN cihaz satıcınızın belirtimlerine danışın. S2S veya VNet-to-VNet bağlantıları, ilkelerin uyumsuz olup olmadığını belirleyemez.

## <a name="part-1---workflow-to-create-and-set-ipsecike-policy"></a><a name ="workflow"></a>Bölüm 1 - IPsec/IKE ilkesini oluşturmak ve ayarlamak için iş akışı
Bu bölümde, S2S VPN veya VNet-to-VNet bağlantısında IPsec/IKE ilkesi oluşturmak ve güncelleştirmek için iş akışı özetlenir:
1. Sanal ağ ve VPN ağ geçidi oluşturma
2. Binalar arası bağlantı için yerel ağ ağ geçidi veya VNet-to-VNet bağlantısı için başka bir sanal ağ ve ağ geçidi oluşturun
3. Seçili algoritmalar ve parametrelerle bir IPsec/IKE ilkesi oluşturma
4. IPsec/IKE ilkesiyle bağlantı (IPsec veya VNet2VNet) oluşturma
5. Varolan bir bağlantı için bir IPsec/IKE ilkesi ekleme/güncelleme/kaldırma

Bu makaledeki yönergeler, diyagramda gösterildiği gibi IPsec/IKE ilkelerini ayarlamanıza ve yapılandırmanıza yardımcı olur:

![ipsec-ike-politikası](./media/vpn-gateway-ipsecikepolicy-rm-powershell/ipsecikepolicy.png)

## <a name="part-2---supported-cryptographic-algorithms--key-strengths"></a><a name ="params"></a>Bölüm 2 - Desteklenen şifreleme algoritmaları & önemli güçlü

Aşağıdaki tablo, müşteriler tarafından yapılandırılabilen desteklenen şifreleme algoritmalarını ve önemli güçlü yönlerini listeler:

| **IPsec/IKEv2**  | **Seçenekler**    |
| ---  | --- 
| IKEv2 Şifrelemesi | AES256, AES192, AES128, DES3, DES  
| IKEv2 Bütünlüğü  | SHA384, SHA256, SHA1, MD5  |
| DH Grubu         | DHGroup24, ECP384, ECP256, DHGroup14, DHGroup2048, DHGroup2, DHGroup1, Yok |
| IPsec Şifrelemesi | GCMAES256, GCMAES192, GCMAES128, AES256, AES192, AES128, DES3, DES, None    |
| IPsec Bütünlüğü  | GCMASE256, GCMAES192, GCMAES128, SHA256, SHA1, MD5 |
| PFS Grubu        | PFS24, ECP384, ECP256, PFS2048, PFS2, PFS1, Hiçbiri 
| QM SA Yaşam Süresi   | (**İsteğe bağlı**: varsayılan değerler belirtilmemişse kullanılır)<br>Saniye (tamsayı; **en az 300**/varsayılan 27000 saniye)<br>Kilobayt (tamsayı; **en az 1024**/varsayılan 102400000 kilobayt)   |
| Trafik Seçicisi | UsePolicyBasedTrafficSelectors** ($True/$False; **İsteğe bağlı**, varsayılan $False belirtilmemişse)    |
|  |  |

> [!IMPORTANT]
> 1. **Şirket içi VPN cihazı yapılandırmanızın Azure IPsec/IKE ilkesinde belirttiğiniz şu algoritmalar ve parametrelerle eşleşmesi ya da bunları içermesi gerekir:**
>    * IKE şifreleme algoritması (Ana Mod / Faz 1)
>    * IKE bütünlük algoritması (Ana Mod / Faz 1)
>    * DH Grubu (Ana Mod / Faz 1)
>    * IPsec şifreleme algoritması (Hızlı Mod / Faz 2)
>    * IPsec bütünlük algoritması (Hızlı Mod / Aşama 2)
>    * PFS Grubu (Hızlı Mod / Faz 2)
>    * Trafik Seçici (UsePolicyBasedTrafficSelectors kullanılıyorsa)
>    * SA yaşam süreleri yalnızca yerel belirtimlerdir ve bunların eşleşmesi gerekmez.
>
> 2. **GCMAES, IPsec Şifreleme algoritması için kullanılıyorsa, Aynı GCMAES algoritmasını ve IPsec Bütünlüğü için anahtar uzunluğunu seçmeniz gerekir; örneğin, her ikisi için de GCMAES128'i kullanarak**
> 3. Yukarıdaki tabloda:
>    * IKEv2 Ana Mod veya Faz 1'e karşılık gelir
>    * IPsec Hızlı Mod veya Faz 2 karşılık gelir
>    * DH Group, Ana Mod'da veya Faz 1'de kullanılan Diffie-Hellmen Grubunu belirtir
>    * PFS Grubu, Hızlı Mod veya Faz 2'de kullanılan Diffie-Hellmen Grubunu belirle
> 4. Azure VPN ağ geçitlerinde IKEv2 Ana Modu SA yaşam süresi 28.800 saniye olarak sabitlenmiştir
> 5. "UsePolicyBasedTrafficSelectors"u bir bağlantıda $True ayarlamak, Azure VPN ağ geçidini şirket içinde ilke tabanlı VPN güvenlik duvarına bağlanmak üzere yapılandıracaktır. PolicyBasedTrafficSelectors'u etkinleştirirseniz, VPN cihazınızın herhangi bir ağ yerine Azure sanal ağ önekleri için /gelen şirket içi ağınızın (yerel ağ ağ geçidi) önekleri ile tanımlanan eşleşen trafik seçicilerine sahip olduğundan emin olmanız gerekir. Örneğin, şirket içi ağınızın ön ekleri 10.1.0.0/16 ve 10.2.0.0/16; sanal ağınızın ön ekleriyse 192.168.0.0/16 ve 172.16.0.0/16 şeklindeyse, aşağıdaki trafik seçicileri belirtmeniz gerekir:
>    * 10.1.0.0/16 <====> 192.168.0.0/16
>    * 10.1.0.0/16 <====> 172.16.0.0/16
>    * 10.2.0.0/16 <====> 192.168.0.0/16
>    * 10.2.0.0/16 <====> 172.16.0.0/16

İlke tabanlı trafik seçicileri ile ilgili daha fazla bilgi için [bkz.](vpn-gateway-connect-multiple-policybased-rm-ps.md)

Aşağıdaki tabloda, özel ilke tarafından desteklenen ilgili Diffie-Hellman Grupları listelenir:

| **Diffie-Hellman Grubu**  | **DHGroup**              | **PFSGroup** | **Anahtar uzunluğu** |
| --- | --- | --- | --- |
| 1                         | DHGroup1                 | PFS1         | 768 bit MODP   |
| 2                         | DHGroup2                 | PFS2         | 1024 bit MODP  |
| 14                        | DHGroup14<br>DHGroup2048 | PFS2048      | 2048 bit MODP  |
| 19                        | ECP256                   | ECP256       | 256 bit ECP    |
| 20                        | ECP384                   | ECP384       | 384 bit ECP    |
| 24                        | DHGroup24                | PFS24        | 2048 bit MODP  |

Diğer ayrıntılar için [RFC3526](https://tools.ietf.org/html/rfc3526)ve [RFC5114](https://tools.ietf.org/html/rfc5114)’e bakın.

## <a name="part-3---create-a-new-s2s-vpn-connection-with-ipsecike-policy"></a><a name ="crossprem"></a>Bölüm 3 - IPsec/IKE ilkesiyle yeni bir S2S VPN bağlantısı oluşturun

Bu bölüm, bir IPsec/IKE ilkesiyle Bir S2S VPN bağlantısı oluşturma adımlarında size yol alar. Aşağıdaki adımlar, diyagramda gösterildiği gibi bağlantıyı oluşturur:

![s2s-politikası](./media/vpn-gateway-ipsecikepolicy-rm-powershell/s2spolicy.png)

Bkz. S2S VPN bağlantısı oluşturmak için daha ayrıntılı adım adım talimatlar için [Bir S2S VPN bağlantısı](vpn-gateway-create-site-to-site-rm-powershell.md) oluşturun.

### <a name="before-you-begin"></a><a name="before"></a>Başlamadan önce

* Azure aboneliğiniz olduğunu doğrulayın. Henüz Azure aboneliğiniz yoksa [MSDN abonelik avantajlarınızı](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) etkinleştirebilir veya [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) için kaydolabilirsiniz.
* Azure Kaynak Yöneticisi PowerShell cmdlets'i yükleyin. PowerShell cmdlets'i yükleme hakkında daha fazla bilgi için [Azure PowerShell'e Genel Bakış'a](/powershell/azure/overview) bakın.

### <a name="step-1---create-the-virtual-network-vpn-gateway-and-local-network-gateway"></a><a name="createvnet1"></a>Adım 1 - Sanal ağ, VPN ağ geçidi ve yerel ağ ağ geçidi oluşturma

#### <a name="1-declare-your-variables"></a>1. Değişkenlerinizi bildirin

Bu alıştırma için, değişkenlerimizi beyan ederek başlarız. Üretim için yapılandırma sırasında bu değerleri kendi değerlerinizle değiştirdiğinizden emin olun.

```powershell
$Sub1          = "<YourSubscriptionName>"
$RG1           = "TestPolicyRG1"
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
$DNS1          = "8.8.8.8"
$GWName1       = "VNet1GW"
$GW1IPName1    = "VNet1GWIP1"
$GW1IPconf1    = "gw1ipconf1"
$Connection16  = "VNet1toSite6"

$LNGName6      = "Site6"
$LNGPrefix61   = "10.61.0.0/16"
$LNGPrefix62   = "10.62.0.0/16"
$LNGIP6        = "131.107.72.22"
```

#### <a name="2-connect-to-your-subscription-and-create-a-new-resource-group"></a>2. Aboneliğinize bağlanın ve yeni bir kaynak grubu oluşturun

Resource Manager cmdlet’lerini kullanmak için PowerShell moduna geçtiğinizden emin olun. Daha fazla bilgi için [Windows PowerShell’i Resource Manager ile kullanma](../powershell-azure-resource-manager.md) konusuna bakın.

PowerShell konsolunuzu açın ve hesabınıza bağlanın. Bağlanmanıza yardımcı olması için aşağıdaki örneği kullanın:

```powershell
Connect-AzAccount
Select-AzSubscription -SubscriptionName $Sub1
New-AzResourceGroup -Name $RG1 -Location $Location1
```

#### <a name="3-create-the-virtual-network-vpn-gateway-and-local-network-gateway"></a>3. Sanal ağ, VPN ağ geçidi ve yerel ağ ağ geçidi oluşturun

Aşağıdaki örnek, üç alt ağ içeren sanal ağ TestVNet1 ve VPN ağ geçidi oluşturur. Kendi değerlerinizi yerleştirirken ağ geçidi alt ağınızı özellikle GatewaySubnet olarak adlandırmanız önem taşır. Başka bir ad kullanırsanız ağ geçidi oluşturma işleminiz başarısız olur.

```powershell
$fesub1 = New-AzVirtualNetworkSubnetConfig -Name $FESubName1 -AddressPrefix $FESubPrefix1
$besub1 = New-AzVirtualNetworkSubnetConfig -Name $BESubName1 -AddressPrefix $BESubPrefix1
$gwsub1 = New-AzVirtualNetworkSubnetConfig -Name $GWSubName1 -AddressPrefix $GWSubPrefix1

New-AzVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1 -Location $Location1 -AddressPrefix $VNetPrefix11,$VNetPrefix12 -Subnet $fesub1,$besub1,$gwsub1

$gw1pip1    = New-AzPublicIpAddress -Name $GW1IPName1 -ResourceGroupName $RG1 -Location $Location1 -AllocationMethod Dynamic
$vnet1      = Get-AzVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1
$subnet1    = Get-AzVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet1
$gw1ipconf1 = New-AzVirtualNetworkGatewayIpConfig -Name $GW1IPconf1 -Subnet $subnet1 -PublicIpAddress $gw1pip1

New-AzVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1 -Location $Location1 -IpConfigurations $gw1ipconf1 -GatewayType Vpn -VpnType RouteBased -GatewaySku VpnGw1

New-AzLocalNetworkGateway -Name $LNGName6 -ResourceGroupName $RG1 -Location $Location1 -GatewayIpAddress $LNGIP6 -AddressPrefix $LNGPrefix61,$LNGPrefix62
```

### <a name="step-2---create-a-s2s-vpn-connection-with-an-ipsecike-policy"></a><a name="s2sconnection"></a>Adım 2 - IPsec/IKE ilkesiyle S2S VPN bağlantısı oluşturma

#### <a name="1-create-an-ipsecike-policy"></a>1. Bir IPsec/IKE ilkesi oluşturma

Aşağıdaki örnek komut dosyası, aşağıdaki algoritmalar ve parametreleriçeren bir IPsec/IKE ilkesi oluşturur:

* İkEv2: AES256, SHA384, DHGroup24
* IPsec: AES256, SHA256, PFS Yok, SA Ömür Boyu 14400 saniye & 10240000KB

```powershell
$ipsecpolicy6 = New-AzIpsecPolicy -IkeEncryption AES256 -IkeIntegrity SHA384 -DhGroup DHGroup24 -IpsecEncryption AES256 -IpsecIntegrity SHA256 -PfsGroup None -SALifeTimeSeconds 14400 -SADataSizeKilobytes 102400000
```

IPsec için GCMAES kullanıyorsanız, hem IPsec şifreleme si hem de bütünlük için aynı GCMAES algoritmasını ve anahtar uzunluğunu kullanmanız gerekir. Yukarıdaki örneğin, GCMAES256 kullanırken ilgili parametreler "-IpsecEncryption GCMAES256 -IpsecIntegrity GCMAES256" olacaktır.

#### <a name="2-create-the-s2s-vpn-connection-with-the-ipsecike-policy"></a>2. IPsec/IKE ilkesiyle S2S VPN bağlantısını oluşturun

Bir S2S VPN bağlantısı oluşturun ve daha önce oluşturulan IPsec/IKE ilkesini uygulayın.

```powershell
$vnet1gw = Get-AzVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
$lng6 = Get-AzLocalNetworkGateway  -Name $LNGName6 -ResourceGroupName $RG1

New-AzVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng6 -Location $Location1 -ConnectionType IPsec -IpsecPolicies $ipsecpolicy6 -SharedKey 'AzureA1b2C3'
```

Azure VPN ağ geçidinin yukarıda açıklandığı gibi, şirket içinde ilke tabanlı VPN aygıtlarına bağlanmasını sağlamak için bağlantı cmdlet'ine isteğe bağlı olarak "-UsePolicyBasedTrafficSelectors $True" ekleyebilirsiniz.

> [!IMPORTANT]
> Bir bağlantıda Bir IPsec/IKE ilkesi belirtildikten sonra, Azure VPN ağ geçidi yalnızca belirtilen şifreleme algoritmaları ve söz konusu bağlantıdaki önemli güçlü yanları içeren IPsec/IKE teklifini gönderir veya kabul eder. Bağlantı için şirket içi VPN cihazınızın tam politika birleşimini kullandığından veya kabul edeceğinden emin olun, aksi takdirde S2S VPN tüneli kurulmayacaktır.


## <a name="part-4---create-a-new-vnet-to-vnet-connection-with-ipsecike-policy"></a><a name ="vnet2vnet"></a>Bölüm 4 - IPsec/IKE ilkesiyle yeni bir VNet-vNet bağlantısı oluşturun

Bir IPsec/IKE ilkesiyle VNet-to-VNet bağlantısı oluşturma adımları, S2S VPN bağlantısına benzer. Aşağıdaki örnek komut dosyaları diyagramda gösterildiği gibi bağlantıyı oluşturur:

![v2v-politika](./media/vpn-gateway-ipsecikepolicy-rm-powershell/v2vpolicy.png)

Bkz. [VNet'ten VNet bağlantısına](vpn-gateway-vnet-vnet-rm-ps.md) daha ayrıntılı adımlar için VNet'ten VNet'e bağlantı oluştur. TestVNet1 ve VPN Ağ Geçidi oluşturmak ve yapılandırmak için [Bölüm 3'ü](#crossprem) tamamlamanız gerekir.

### <a name="step-1---create-the-second-virtual-network-and-vpn-gateway"></a><a name="createvnet2"></a>Adım 1 - İkinci sanal ağ ve VPN ağ geçidi oluşturun

#### <a name="1-declare-your-variables"></a>1. Değişkenlerinizi bildirin

Değerleri, yapılandırma için kullanmak istediğiniz değerlerle değiştirdiğinizden emin olun.

```powershell
$RG2          = "TestPolicyRG2"
$Location2    = "East US 2"
$VNetName2    = "TestVNet2"
$FESubName2   = "FrontEnd"
$BESubName2   = "Backend"
$GWSubName2   = "GatewaySubnet"
$VNetPrefix21 = "10.21.0.0/16"
$VNetPrefix22 = "10.22.0.0/16"
$FESubPrefix2 = "10.21.0.0/24"
$BESubPrefix2 = "10.22.0.0/24"
$GWSubPrefix2 = "10.22.255.0/27"
$DNS2         = "8.8.8.8"
$GWName2      = "VNet2GW"
$GW2IPName1   = "VNet2GWIP1"
$GW2IPconf1   = "gw2ipconf1"
$Connection21 = "VNet2toVNet1"
$Connection12 = "VNet1toVNet2"
```

#### <a name="2-create-the-second-virtual-network-and-vpn-gateway-in-the-new-resource-group"></a>2. Yeni kaynak grubunda ikinci sanal ağ ve VPN ağ geçidi oluşturma

```powershell
New-AzResourceGroup -Name $RG2 -Location $Location2

$fesub2 = New-AzVirtualNetworkSubnetConfig -Name $FESubName2 -AddressPrefix $FESubPrefix2
$besub2 = New-AzVirtualNetworkSubnetConfig -Name $BESubName2 -AddressPrefix $BESubPrefix2
$gwsub2 = New-AzVirtualNetworkSubnetConfig -Name $GWSubName2 -AddressPrefix $GWSubPrefix2

New-AzVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2 -Location $Location2 -AddressPrefix $VNetPrefix21,$VNetPrefix22 -Subnet $fesub2,$besub2,$gwsub2

$gw2pip1    = New-AzPublicIpAddress -Name $GW2IPName1 -ResourceGroupName $RG2 -Location $Location2 -AllocationMethod Dynamic
$vnet2      = Get-AzVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2
$subnet2    = Get-AzVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet2
$gw2ipconf1 = New-AzVirtualNetworkGatewayIpConfig -Name $GW2IPconf1 -Subnet $subnet2 -PublicIpAddress $gw2pip1

New-AzVirtualNetworkGateway -Name $GWName2 -ResourceGroupName $RG2 -Location $Location2 -IpConfigurations $gw2ipconf1 -GatewayType Vpn -VpnType RouteBased -GatewaySku HighPerformance
```

### <a name="step-2---create-a-vnet-tovnet-connection-with-the-ipsecike-policy"></a>Adım 2 - IPsec/IKE ilkesiyle VNet-toVNet bağlantısı oluşturma

S2S VPN bağlantısına benzer şekilde, bir IPsec/IKE ilkesi oluşturun ve ardından yeni bağlantıya ilke uygulayın.

#### <a name="1-create-an-ipsecike-policy"></a>1. Bir IPsec/IKE ilkesi oluşturma

Aşağıdaki örnek betik, şu algoritmalar ve parametreler ile farklı bir IPsec/IKE ilkesi oluşturur:
* İkEv2: AES128, SHA1, DHGroup14
* IPsec: GCMAES128, GCMAES128, PFS14, SA Ömür Boyu 14400 saniye & 10240000KB

```powershell
$ipsecpolicy2 = New-AzIpsecPolicy -IkeEncryption AES128 -IkeIntegrity SHA1 -DhGroup DHGroup14 -IpsecEncryption GCMAES128 -IpsecIntegrity GCMAES128 -PfsGroup PFS14 -SALifeTimeSeconds 14400 -SADataSizeKilobytes 102400000
```

#### <a name="2-create-vnet-to-vnet-connections-with-the-ipsecike-policy"></a>2. IPsec/IKE ilkesiyle VNet-vNet bağlantıları oluşturma

Bir VNet-to-VNet bağlantısı oluşturun ve oluşturduğunuz IPsec/IKE ilkesini uygulayın. Bu örnekte, her iki ağ geçidi de aynı aboneliktedir. Bu nedenle, aynı PowerShell oturumunda aynı IPsec/IKE ilkesiyle her iki bağlantıyı da oluşturmak ve yapılandırmak mümkündür.

```powershell
$vnet1gw = Get-AzVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
$vnet2gw = Get-AzVirtualNetworkGateway -Name $GWName2  -ResourceGroupName $RG2

New-AzVirtualNetworkGatewayConnection -Name $Connection12 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -VirtualNetworkGateway2 $vnet2gw -Location $Location1 -ConnectionType Vnet2Vnet -IpsecPolicies $ipsecpolicy2 -SharedKey 'AzureA1b2C3'

New-AzVirtualNetworkGatewayConnection -Name $Connection21 -ResourceGroupName $RG2 -VirtualNetworkGateway1 $vnet2gw -VirtualNetworkGateway2 $vnet1gw -Location $Location2 -ConnectionType Vnet2Vnet -IpsecPolicies $ipsecpolicy2 -SharedKey 'AzureA1b2C3'
```

> [!IMPORTANT]
> Bir bağlantıda Bir IPsec/IKE ilkesi belirtildikten sonra, Azure VPN ağ geçidi yalnızca belirtilen şifreleme algoritmaları ve söz konusu bağlantıdaki önemli güçlü yanları içeren IPsec/IKE teklifini gönderir veya kabul eder. Her iki bağlantı için de IPsec ilkelerinin aynı olduğundan emin olun, aksi takdirde VNet-vNet bağlantısı kurulmayacaktır.

Bu adımları tamamladıktan sonra, bağlantı birkaç dakika içinde kurulur ve başlangıçta gösterildiği gibi aşağıdaki ağ topolojisi olacaktır:

![ipsec-ike-politikası](./media/vpn-gateway-ipsecikepolicy-rm-powershell/ipsecikepolicy.png)


## <a name="part-5---update-ipsecike-policy-for-a-connection"></a><a name ="managepolicy"></a>Bölüm 5 - Bağlantı için IPsec/IKE ilkesini güncelleştirme

Son bölümde, varolan bir S2S veya VNet-to-VNet bağlantısı için IPsec/IKE ilkesinin nasıl yönetilenolduğu gösterilmektedir. Aşağıdaki alıştırma, bir bağlantı üzerinde aşağıdaki işlemler de size yol kat edin:

1. Bağlantının IPsec/IKE ilkesini gösterme
2. Bağlantıya IPsec/IKE ilkesiekleme veya güncelleme
3. Bağlantıdan IPsec/IKE ilkesini kaldırma

Aynı adımlar hem S2S hem de VNet-to-VNet bağlantıları için de geçerlidir.

> [!IMPORTANT]
> IPsec/IKE ilkesi yalnızca *Standart* ve *Yüksek Performanslı* rota tabanlı VPN ağ geçitlerinde desteklenir. Temel ağ geçidi SKU veya ilke tabanlı VPN ağ geçidinde çalışmaz.

#### <a name="1-show-the-ipsecike-policy-of-a-connection"></a>1. Bir bağlantının IPsec/IKE ilkesini göster

Aşağıdaki örnek, Bağlantı üzerinde iPsec/IKE ilkesinin nasıl yapılandırılabildiğini gösterir. Komut dosyaları da yukarıdaki alıştırmalar devam ediyor.

```powershell
$RG1          = "TestPolicyRG1"
$Connection16 = "VNet1toSite6"
$connection6  = Get-AzVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1
$connection6.IpsecPolicies
```

Son komut, varsa bağlantıda yapılandırılan geçerli IPsec/IKE ilkesini listeler. Bağlantı için örnek çıktı aşağıda veda edinilir:

```powershell
SALifeTimeSeconds   : 14400
SADataSizeKilobytes : 102400000
IpsecEncryption     : AES256
IpsecIntegrity      : SHA256
IkeEncryption       : AES256
IkeIntegrity        : SHA384
DhGroup             : DHGroup24
PfsGroup            : PFS24
```

Yapılandırılmış IPsec/IKE ilkesi yoksa, komut (PS> $connection6. IpsecPolicies) boş bir dönüş alır. Bu, IPsec/IKE'nin bağlantıda yapılandırılmadığını, ancak özel Bir IPsec/IKE ilkesi olmadığı anlamına gelmez. Gerçek bağlantı, şirket içi VPN aygıtınız ile Azure VPN ağ geçidi arasında görüşülen varsayılan ilkeyi kullanır.

#### <a name="2-add-or-update-an-ipsecike-policy-for-a-connection"></a>2. Bağlantı için bir IPsec/IKE ilkesi ekleme veya güncelleme

Yeni bir ilke eklemek veya bağlantıda varolan bir ilkeyi güncelleştirmek için adımlar aynıdır: yeni bir ilke oluşturun ve ardından bağlantıya yeni ilke uygulayın.

```powershell
$RG1          = "TestPolicyRG1"
$Connection16 = "VNet1toSite6"
$connection6  = Get-AzVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1

$newpolicy6   = New-AzIpsecPolicy -IkeEncryption AES128 -IkeIntegrity SHA1 -DhGroup DHGroup14 -IpsecEncryption AES256 -IpsecIntegrity SHA256 -PfsGroup None -SALifeTimeSeconds 14400 -SADataSizeKilobytes 102400000

Set-AzVirtualNetworkGatewayConnection -VirtualNetworkGatewayConnection $connection6 -IpsecPolicies $newpolicy6
```

Şirket içi ilke tabanlı bir VPN aygıtına bağlanırken "UsePolicyBasedTrafficSelectors"u etkinleştirmek için cmdlet'e "-UsePolicyBaseTrafficSelectors" parametresini ekleyin veya seçeneği devre dışı bırakmak için $False olarak ayarlayın:

```powershell
Set-AzVirtualNetworkGatewayConnection -VirtualNetworkGatewayConnection $connection6 -IpsecPolicies $newpolicy6 -UsePolicyBasedTrafficSelectors $True
```

İlkenin güncelleştirilip güncellenmedisini kontrol etmek için bağlantıyı yeniden alabilirsiniz.

```powershell
$connection6  = Get-AzVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1
$connection6.IpsecPolicies
```

Aşağıdaki örnekte gösterildiği gibi, son satırdan çıktı yı görmeniz gerekir:

```powershell
SALifeTimeSeconds   : 14400
SADataSizeKilobytes : 102400000
IpsecEncryption     : AES256
IpsecIntegrity      : SHA256
IkeEncryption       : AES128
IkeIntegrity        : SHA1
DhGroup             : DHGroup14
PfsGroup            : None
```

#### <a name="3-remove-an-ipsecike-policy-from-a-connection"></a>3. Bir bağlantıdan bir IPsec/IKE ilkesini kaldırma

Özel ilkeyi bir bağlantıdan kaldırdığınızda, Azure VPN ağ geçidi [varsayılan IPsec/IKE teklifleri listesine](vpn-gateway-about-vpn-devices.md) geri döner ve şirket içi VPN aygıtınızla yeniden görüşür.

```powershell
$RG1           = "TestPolicyRG1"
$Connection16  = "VNet1toSite6"
$connection6   = Get-AzVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1

$currentpolicy = $connection6.IpsecPolicies[0]
$connection6.IpsecPolicies.Remove($currentpolicy)

Set-AzVirtualNetworkGatewayConnection -VirtualNetworkGatewayConnection $connection6
```

İlkenin bağlantıdan kaldırılıp kaldırılmadı sını denetlemek için aynı komut dosyasını kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

İlke tabanlı trafik seçicileri ile ilgili daha fazla ayrıntı için [birden çok şirket içi ilke tabanlı VPN aygıtını bağlayın.](vpn-gateway-connect-multiple-policybased-rm-ps.md)

Bağlantınız tamamlandıktan sonra sanal ağlarınıza sanal makineler ekleyebilirsiniz. Adımlar için bkz. [Sanal Makine Oluşturma](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
