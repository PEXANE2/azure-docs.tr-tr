---
title: "Azure ExpressRoute: S2S VPN'i Microsoft'un bakışlarına göre yapılandırın"
description: Siteden siteye VPN ağ geçidi kullanarak ExpressRoute Microsoft eşleme devresi üzerinden Azure'a IPsec/IKE bağlantısını yapılandırın.
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: conceptual
ms.date: 02/25/2019
ms.author: cherylmc
ms.custom: seodec18
ms.openlocfilehash: f3044a2701b0f1cd0e5f9ab3ab60c1d60cfb8f45
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75436805"
---
# <a name="configure-a-site-to-site-vpn-over-expressroute-microsoft-peering"></a>ExpressRoute Microsoft eşleme üzerinde siteden siteye VPN yapılandırma

Bu makale, bir ExpressRoute özel bağlantısı üzerinden şirket içi ağınızla Azure sanal ağlarınız (VNet) arasında güvenli şifreli bağlantı yapılandırmanıza yardımcı olur. Seçtiğiniz şirket içi ağlar la Azure VNet'leri arasında siteden siteye IPsec/IKE VPN tüneli oluşturmak için Microsoft eşlemasını kullanabilirsiniz. ExpressRoute üzerinde güvenli bir tünel yapılandırma gizlilik, anti-tekrar, özgünlük ve bütünlük ile veri alışverişi için izin verir.

>[!NOTE]
>Microsoft'un bakışları üzerinden siteden siteye VPN ayarladığınızda, VPN ağ geçidi ve VPN çıkış için ücretlendirilirsiniz. Daha fazla bilgi için [VPN Ağ Geçidi fiyatlandırması](https://azure.microsoft.com/pricing/details/vpn-gateway)için bkz.
>
>

[!INCLUDE [updated-for-az](../../includes/hybrid-az-ps.md)]

## <a name="architecture"></a><a name="architecture"></a>Mimari


  ![bağlantıya genel bakış](./media/site-to-site-vpn-over-microsoft-peering/IPsecER_Overview.png)


Yüksek kullanılabilirlik ve artıklık için, expressroute devresinin iki MSEE-PE çifti üzerinde birden çok tüneli yapılandırabilir ve tüneller arasında yük dengelemesini etkinleştirebilirsiniz.

  ![yüksek kullanılabilirlik seçenekleri](./media/site-to-site-vpn-over-microsoft-peering/HighAvailability.png)

Microsoft'un eşleçliği üzerindeki VPN tünelleri, VPN ağ geçidi kullanılarak veya Azure Marketi üzerinden kullanılabilen uygun bir Ağ Sanal Cihazı (NVA) kullanılarak sonlandırılabilir. Rota değişimini temel Microsoft bakışlarına maruz bırakmadan, şifreli tüneller üzerinde statik veya dinamik olarak rota değiştirebilirsiniz. Bu makaledeki örneklerde, BGP (Microsoft eşlemi oluşturmak için kullanılan BGP oturumundan farklı) şifrelenmiş tüneller üzerinde önekleri dinamik olarak değiştirmek için kullanılır.

>[!IMPORTANT]
>Şirket içi tarafta, genellikle Microsoft'un eşlemi DMZ'de sonlandırılır ve çekirdek ağ bölgesinde özel eşleme sonlandırılır. İki bölge güvenlik duvarları kullanılarak ayrı ştı. Yalnızca ExpressRoute üzerinden güvenli tünel leme sağlamak için Microsoft'a bakan ları yapılandırıyorsanız, yalnızca Microsoft'un bakışları aracılığıyla reklamı yapılan genel IP'lere filtre uygulama yapmayı unutmayın.
>
>

## <a name="workflow"></a><a name="workflow"></a>Iş akışı

1. ExpressRoute devreniz için Microsoft'u yeniden yapılandırma.
2. Microsoft'un bakışları aracılığıyla şirket içi ağınıza seçili Azure bölgesel genel öneklerinin reklamını yapmak.
3. VPN ağ geçidini yapılandırma ve IPsec tünelleri oluşturma
4. Şirket içi VPN aygıtını yapılandırın.
5. Siteden siteye IPsec/IKE bağlantısını oluşturun.
6. (İsteğe bağlı) Şirket içi VPN aygıtında güvenlik duvarlarını/filtrelemeyi yapılandırın.
7. ExpressRoute devresi üzerinden IPsec iletişimini test edin ve doğrulayın.

## <a name="1-configure-microsoft-peering"></a><a name="peering"></a>1. Microsoft'un eşleneme yapılandırma

ExpressRoute üzerinden siteden siteye VPN bağlantısını yapılandırmak için ExpressRoute Microsoft'un eşlemesi üzerinde yararlanmanız gerekir.

* Yeni bir ExpressRoute devresini yapılandırmak için [ExpressRoute önkoşul](expressroute-prerequisites.md) makalesiyle başlayın ve ardından [bir ExpressRoute devresi oluşturun ve değiştirin.](expressroute-howto-circuit-arm.md)

* Zaten bir ExpressRoute devreniz varsa, ancak Microsoft'un eşlemesi yapılandırılmamışsa, Create'i kullanarak Microsoft'un eşlemesi yapılandırın ve ExpressRoute devresi makalesi [için eşlemi değiştirin.](expressroute-howto-routing-arm.md#msft)

Devrenizi ve Microsoft bakışınızı yapılandırdıktan sonra, Azure portalındaki **Genel Bakış** sayfasını kullanarak bu devreyi kolayca görüntüleyebilirsiniz.

![Devre](./media/site-to-site-vpn-over-microsoft-peering/ExpressRouteCkt.png)

## <a name="2-configure-route-filters"></a><a name="routefilter"></a>2. Rota filtrelerini yapılandırma

Rota filtresi, ExpressRoute bağlantı hattınızın Microsoft eşlemesi üzerinden kullanmak istediğiniz hizmetleri tanımlamanızı sağlar. Aslında tüm BGP topluluk değerlerinin izin listesidir. 

![rota filtresi](./media/site-to-site-vpn-over-microsoft-peering/route-filter.png)

Bu örnekte, dağıtım yalnızca *Azure Batı ABD 2* bölgesindedir. BGP topluluk değeri *12076:51026*olan Azure West US 2 bölgesel öneklerinin yalnızca reklamına izin vermek için bir rota filtresi kuralı eklenir. **Yönet kuralını**seçerek izin vermek istediğiniz bölgesel önekleri belirtirsiniz.

Rota filtresi içinde, rota filtresinin uygulandığı ExpressRoute devrelerini de seçmeniz gerekir. **Add circuit'i**seçerek ExpressRoute devrelerini seçebilirsiniz. Önceki şekilde, rota filtresi örnek ExpressRoute devresi ile ilişkilidir.

### <a name="21-configure-the-route-filter"></a><a name="configfilter"></a>2.1 Rota filtresini yapılandırma

Rota filtresini yapılandırın. Adımlar için, [Microsoft eşleme için rota filtrelerini yapılandır'a](how-to-routefilter-portal.md)bakın.

### <a name="22-verify-bgp-routes"></a><a name="verifybgp"></a>2.2 BGP rotalarını doğrulayın

ExpressRoute devreniz üzerinden Microsoft'u başarıyla oluşturduktan ve devreyle bir rota filtresi ilişkilendirdikten sonra, MSEE'lerle bakan PE aygıtlarında MSEE'lerden alınan BGP rotalarını doğrulayabilirsiniz. Doğrulama komutu, PE cihazlarınızın işletim sistemine bağlı olarak değişir.

#### <a name="cisco-examples"></a>Cisco örnekleri

Bu örnekte bir Cisco IOS-XE komutu kullanabilirsiniz. Örnekte, sanal yönlendirme ve iletme (VRF) örneği, bakan trafiği yalıtmak için kullanılır.

```
show ip bgp vpnv4 vrf 10 summary
```

Aşağıdaki kısmi çıktı, komşudan \*68 önek alındığını göstermektedir .243.229.34 ASN 12076 (MSEE):

```
...

Neighbor        V           AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
X.243.229.34    4        12076   17671   17650    25228    0    0 1w4d           68
```

Komşudan alınan öneklistesini görmek için aşağıdaki örneği kullanın:

```
sh ip bgp vpnv4 vrf 10 neighbors X.243.229.34 received-routes
```

Doğru önek kümesini aldığınızı doğrulamak için çapraz doğrulama yapabilirsiniz. Aşağıdaki Azure PowerShell komut çıktısı, hizmetlerin her biri ve Azure bölgesi nin her biri için Microsoft tarafından bakan tarafından reklamı yapılan önekleri listeler:

```azurepowershell-interactive
Get-AzBgpServiceCommunity
```

## <a name="3-configure-the-vpn-gateway-and-ipsec-tunnels"></a><a name="vpngateway"></a>3. VPN ağ geçidini ve IPsec tünellerini yapılandırın

Bu bölümde, Azure VPN ağ geçidi ile şirket içi VPN aygıtı arasında IPsec VPN tünelleri oluşturulur. Örneklerde Cisco Cloud Service Router (CSR1000) VPN cihazları kullanılmaktadır.

Aşağıdaki diyagram, şirket içi VPN aygıtı 1 ile Azure VPN ağ geçidi örnek çifti arasında kurulan IPsec VPN tünellerini gösterir. Şirket içi VPN aygıtı 2 ile Azure VPN ağ geçidi örnek çifti arasında kurulan iki IPsec VPN tüneli diyagramda gösteriş yapmaz ve yapılandırma ayrıntıları listelenmez. Ancak, ek VPN tünelleri olması yüksek kullanılabilirliği artırır.

  ![VPN tünelleri](./media/site-to-site-vpn-over-microsoft-peering/EstablishTunnels.png)

IPsec tünel çifti üzerinde, özel ağ yollarını değiştirmek için bir eBGP oturumu oluşturulur. Aşağıdaki diyagram, IPsec tünel çifti üzerinde kurulan eBGP oturumunu gösterir:

  ![tünel çifti üzerinde eBGP oturumları](./media/site-to-site-vpn-over-microsoft-peering/TunnelBGP.png)

Aşağıdaki diyagram, örnek ağın soyutlanmış genel görünümünü gösterir:

  ![örnek ağ](./media/site-to-site-vpn-over-microsoft-peering/OverviewRef.png)

### <a name="about-the-azure-resource-manager-template-examples"></a>Azure Kaynak Yöneticisi şablon örnekleri hakkında

Örneklerde, VPN ağ geçidi ve IPsec tünel sonlandırmaları Azure Kaynak Yöneticisi şablonu kullanılarak yapılandırılır. Kaynak Yöneticisi şablonlarını kullanmakta yeniyseniz veya Kaynak Yöneticisi şablonu temellerini anlamak için [bkz.](../azure-resource-manager/templates/template-syntax.md) Bu bölümdeki şablon, yeşil alan azure ortamı (VNet) oluşturur. Ancak, varolan bir VNet'iniz varsa, şablonda başvuruda bulunabilirsiniz. VPN ağ geçidi IPsec/IKE siteden siteye yapılandırmalarına aşina değilseniz, [bkz.](../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md)

>[!NOTE]
>Bu yapılandırmayı oluşturmak için Azure Kaynak Yöneticisi şablonlarını kullanmanız gerekmez. Bu yapılandırmayı Azure portalını veya PowerShell'i kullanarak oluşturabilirsiniz.
>
>

### <a name="31-declare-the-variables"></a><a name="variables3"></a>3.1 Değişkenleri bildirin

Bu örnekte, değişken bildirimleri örnek ağa karşılık gelir. Değişkenleri bildirirken, ortamınızı yansıtacak şekilde bu bölümü değiştirin.

* Değişken **localAddressPrefix,** IPsec tünellerini sonlandırmak için bir dizi şirket içi IP adresidir.
* **Ağ geçidiSku** VPN çıkışını belirler. GatewaySku ve vpnType hakkında daha fazla bilgi için [VPN Ağ Geçidi yapılandırma ayarlarına](../vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md#gwsku)bakın. Fiyatlandırma için [VPN Ağ Geçidi fiyatlandırması'na](https://azure.microsoft.com/pricing/details/vpn-gateway)bakın.
* **vpnType'ı RouteBased**olarak ayarlayın. **vpnType**

```json
"variables": {
  "virtualNetworkName": "SecureVNet",       // Name of the Azure VNet
  "azureVNetAddressPrefix": "10.2.0.0/24",  // Address space assigned to the VNet
  "subnetName": "Tenant",                   // subnet name in which tenants exists
  "subnetPrefix": "10.2.0.0/25",            // address space of the tenant subnet
  "gatewaySubnetPrefix": "10.2.0.224/27",   // address space of the gateway subnet
  "localGatewayName": "localGW1",           // name of remote gateway (on-premises)
  "localGatewayIpAddress": "X.243.229.110", // public IP address of the on-premises VPN device
  "localAddressPrefix": [
    "172.16.0.1/32",                        // termination of IPsec tunnel-1 on-premises 
    "172.16.0.2/32"                         // termination of IPsec tunnel-2 on-premises 
  ],
  "gatewayPublicIPName1": "vpnGwVIP1",    // Public address name of the first VPN gateway instance
  "gatewayPublicIPName2": "vpnGwVIP2",    // Public address name of the second VPN gateway instance 
  "gatewayName": "vpnGw",                 // Name of the Azure VPN gateway
  "gatewaySku": "VpnGw1",                 // Azure VPN gateway SKU
  "vpnType": "RouteBased",                // type of VPN gateway
  "sharedKey": "string",                  // shared secret needs to match with on-premises configuration
  "asnVpnGateway": 65000,                 // BGP Autonomous System number assigned to the VPN Gateway 
  "asnRemote": 65010,                     // BGP Autonmous Syste number assigned to the on-premises device
  "bgpPeeringAddress": "172.16.0.3",      // IP address of the remote BGP peer on-premises
  "connectionName": "vpn2local1",
  "vnetID": "[resourceId('Microsoft.Network/virtualNetworks', variables('virtualNetworkName'))]",
  "gatewaySubnetRef": "[concat(variables('vnetID'),'/subnets/','GatewaySubnet')]",
  "subnetRef": "[concat(variables('vnetID'),'/subnets/',variables('subnetName'))]",
  "api-version": "2017-06-01"
},
```

### <a name="32-create-virtual-network-vnet"></a><a name="vnet"></a>3.2 Sanal ağ oluşturma (VNet)

Varolan bir VNet'i VPN tünelleriyle ilişkilendiriyorsanız, bu adımı atlayabilirsiniz.

```json
{
  "apiVersion": "[variables('api-version')]",
  "type": "Microsoft.Network/virtualNetworks",
  "name": "[variables('virtualNetworkName')]",
  "location": "[resourceGroup().location]",
  "properties": {
    "addressSpace": {
      "addressPrefixes": [
        "[variables('azureVNetAddressPrefix')]"
      ]
    },
    "subnets": [
      {
        "name": "[variables('subnetName')]",
        "properties": {
          "addressPrefix": "[variables('subnetPrefix')]"
        }
      },
      {
        "name": "GatewaySubnet",
        "properties": {
          "addressPrefix": "[variables('gatewaySubnetPrefix')]"
        }
      }
    ]
  },
  "comments": "Create a Virtual Network with Subnet1 and Gatewaysubnet"
},
```

### <a name="33-assign-public-ip-addresses-to-vpn-gateway-instances"></a><a name="ip"></a>3.3 Genel IP adreslerini VPN ağ geçidi örneklerine atama
 
VPN ağ geçidinin her örneği için ortak bir IP adresi atayın.

```json
{
  "apiVersion": "[variables('api-version')]",
  "type": "Microsoft.Network/publicIPAddresses",
    "name": "[variables('gatewayPublicIPName1')]",
    "location": "[resourceGroup().location]",
    "properties": {
      "publicIPAllocationMethod": "Dynamic"
    },
    "comments": "Public IP for the first instance of the VPN gateway"
  },
  {
    "apiVersion": "[variables('api-version')]",
    "type": "Microsoft.Network/publicIPAddresses",
    "name": "[variables('gatewayPublicIPName2')]",
    "location": "[resourceGroup().location]",
    "properties": {
      "publicIPAllocationMethod": "Dynamic"
    },
    "comments": "Public IP for the second instance of the VPN gateway"
  },
```

### <a name="34-specify-the-on-premises-vpn-tunnel-termination-local-network-gateway"></a><a name="termination"></a>3.4 Şirket içi VPN tünelisonlandırmasını (yerel ağ ağ geçidi) belirtin

Şirket içi VPN aygıtları yerel **ağ ağ geçidi**olarak adlandırılır. Aşağıdaki json snippet de uzak BGP eş ayrıntıları belirtir:

```json
{
  "apiVersion": "[variables('api-version')]",
  "type": "Microsoft.Network/localNetworkGateways",
  "name": "[variables('localGatewayName')]",
  "location": "[resourceGroup().location]",
  "properties": {
    "localNetworkAddressSpace": {
      "addressPrefixes": "[variables('localAddressPrefix')]"
    },
    "gatewayIpAddress": "[variables('localGatewayIpAddress')]",
    "bgpSettings": {
      "asn": "[variables('asnRemote')]",
      "bgpPeeringAddress": "[variables('bgpPeeringAddress')]",
      "peerWeight": 0
    }
  },
  "comments": "Local Network Gateway (referred to your on-premises location) with IP address of remote tunnel peering and IP address of remote BGP peer"
},
```

### <a name="35-create-the-vpn-gateway"></a><a name="creategw"></a>3.5 VPN ağ geçidini oluşturma

Şablonun bu bölümü, VPN ağ geçidini etkin-etkin yapılandırma için gerekli ayarlarla yapılandırır. Aşağıdaki gereksinimleri göz önünde bulundurun:

* **"RouteBased"** VpnType ile VPN ağ geçidi oluşturun. VPN ağ geçidi ile VPN'in şirket içi yönlendirmesini etkinleştirmek istiyorsanız bu ayar zorunludur.
* VPN ağ geçidinin iki örneği ile etkin etkin modda belirli bir şirket içi aygıt arasında VPN tünelleri oluşturmak için Kaynak Yöneticisi şablonunda **"activeActive"** parametresi **gerçek olarak** ayarlanır. Yüksek kullanılabilir VPN ağ geçitleri hakkında daha fazla şey anlamak için, [yüksek kullanılabilir VPN ağ geçidi bağlantısına](../vpn-gateway/vpn-gateway-highlyavailable.md)bakın.
* VPN tünelleri arasında eBGP oturumlarını yapılandırmak için, her iki tarafta iki farklı ASN belirtmeniz gerekir. Özel ASN numaraları belirtmek tercih edilir. Daha fazla bilgi için [BGP ve Azure VPN ağ geçitlerine genel bakış](../vpn-gateway/vpn-gateway-bgp-overview.md)bölümüne bakın.

```json
{
"apiVersion": "[variables('api-version')]",
"type": "Microsoft.Network/virtualNetworkGateways",
"name": "[variables('gatewayName')]",
"location": "[resourceGroup().location]",
"dependsOn": [
  "[concat('Microsoft.Network/publicIPAddresses/', variables('gatewayPublicIPName1'))]",
  "[concat('Microsoft.Network/publicIPAddresses/', variables('gatewayPublicIPName2'))]",
  "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]"
],
"properties": {
  "ipConfigurations": [
    {
      "properties": {
        "privateIPAllocationMethod": "Dynamic",
        "subnet": {
          "id": "[variables('gatewaySubnetRef')]"
        },
        "publicIPAddress": {
          "id": "[resourceId('Microsoft.Network/publicIPAddresses',variables('gatewayPublicIPName1'))]"
        }
      },
      "name": "vnetGtwConfig1"
    },
    {
      "properties": {
        "privateIPAllocationMethod": "Dynamic",
        "subnet": {
          "id": "[variables('gatewaySubnetRef')]"
        },
        "publicIPAddress": {
          "id": "[resourceId('Microsoft.Network/publicIPAddresses',variables('gatewayPublicIPName2'))]"
        }
      },
          "name": "vnetGtwConfig2"
        }
      ],
      "sku": {
        "name": "[variables('gatewaySku')]",
        "tier": "[variables('gatewaySku')]"
      },
      "gatewayType": "Vpn",
      "vpnType": "[variables('vpnType')]",
      "enableBgp": true,
      "activeActive": true,
      "bgpSettings": {
        "asn": "[variables('asnVpnGateway')]"
      }
    },
    "comments": "VPN Gateway in active-active configuration with BGP support"
  },
```

### <a name="36-establish-the-ipsec-tunnels"></a><a name="ipsectunnel"></a>3.6 IPsec tünellerini kurun

Komut dosyasının son eylemi, Azure VPN ağ geçidi ile şirket içi VPN aygıtı arasında IPsec tünelleri oluşturur.

```json
{
  "apiVersion": "[variables('api-version')]",
  "name": "[variables('connectionName')]",
  "type": "Microsoft.Network/connections",
  "location": "[resourceGroup().location]",
  "dependsOn": [
    "[concat('Microsoft.Network/virtualNetworkGateways/', variables('gatewayName'))]",
    "[concat('Microsoft.Network/localNetworkGateways/', variables('localGatewayName'))]"
  ],
  "properties": {
    "virtualNetworkGateway1": {
      "id": "[resourceId('Microsoft.Network/virtualNetworkGateways', variables('gatewayName'))]"
    },
    "localNetworkGateway2": {
      "id": "[resourceId('Microsoft.Network/localNetworkGateways', variables('localGatewayName'))]"
    },
    "connectionType": "IPsec",
    "routingWeight": 0,
    "sharedKey": "[variables('sharedKey')]",
    "enableBGP": "true"
  },
  "comments": "Create a Connection type site-to-site (IPsec) between the Azure VPN Gateway and the VPN device on-premises"
  }
```

## <a name="4-configure-the-on-premises-vpn-device"></a><a name="device"></a>4. Şirket içi VPN cihazını yapılandırın

Azure VPN ağ geçidi, farklı satıcılardan gelen birçok VPN aygıtıyla uyumludur. VPN ağ geçidiyle çalışmak üzere doğrulanmış yapılandırma bilgileri ve aygıtlar için [VPN aygıtları hakkında](../vpn-gateway/vpn-gateway-about-vpn-devices.md)bilgi bakın.

VPN aygıtınızı yapılandırırken aşağıdaki öğelere ihtiyacınız vardır:

* Paylaşılan bir anahtar. Bu, siteden siteye VPN bağlantınızı oluştururken belirttiğiniz paylaşılan anahtardır. Örneklerde temel paylaşılan anahtar kullanılır. Kullanmak için daha karmaşık bir anahtar oluşturmanız önerilir.
* VPN ağ geçidinizin Genel IP adresi. Azure Portal, PowerShell veya CLI kullanarak genel IP adresini görüntüleyebilirsiniz. Azure portalını kullanarak VPN ağ geçidinizin Genel IP adresini bulmak için Sanal ağ geçitleri’ne gidin ve ağ geçidinizin adına tıklayın.

Genellikle eBGP eşler doğrudan bağlanır (genellikle WAN bağlantısı üzerinden). Ancak, ExpressRoute Microsoft peering üzerinden IPsec VPN tünelleri üzerinden eBGP yapılandırırken, eBGP eşler arasında birden çok yönlendirme etki alanı vardır. EBGP komşu ilişkisini doğrudan bağlı olmayan iki eş arasında kurmak için **ebgp-multihop** komutunu kullanın. Ebgp-multihop komutunu izleyen birsonrakibacı BGP paketlerindeki TTL değerini belirtir. Komut **maksimum yollar eibgp 2** iki BGP yolları arasındaki trafiğin yük dengeleme sağlar.

### <a name="cisco-csr1000-example"></a><a name="cisco1"></a>Cisco CSR1000 örneği

Aşağıdaki örnek, Cisco CSR1000'ün hyper-v sanal makinedeki yapılandırmasını şirket içi VPN aygıtı olarak gösterir:

```
!
crypto ikev2 proposal az-PROPOSAL
 encryption aes-cbc-256 aes-cbc-128 3des
 integrity sha1
 group 2
!
crypto ikev2 policy az-POLICY
 proposal az-PROPOSAL
!
crypto ikev2 keyring key-peer1
 peer azvpn1
  address 52.175.253.112
  pre-shared-key secret*1234
 !
!
crypto ikev2 keyring key-peer2
 peer azvpn2
  address 52.175.250.191
  pre-shared-key secret*1234
 !
!
!
crypto ikev2 profile az-PROFILE1
 match address local interface GigabitEthernet1
 match identity remote address 52.175.253.112 255.255.255.255
 authentication remote pre-share
 authentication local pre-share
 keyring local key-peer1
!
crypto ikev2 profile az-PROFILE2
 match address local interface GigabitEthernet1
 match identity remote address 52.175.250.191 255.255.255.255
 authentication remote pre-share
 authentication local pre-share
 keyring local key-peer2
!
crypto ikev2 dpd 10 2 on-demand
!
!
crypto ipsec transform-set az-IPSEC-PROPOSAL-SET esp-aes 256 esp-sha-hmac
 mode tunnel
!
crypto ipsec profile az-VTI1
 set transform-set az-IPSEC-PROPOSAL-SET
 set ikev2-profile az-PROFILE1
!
crypto ipsec profile az-VTI2
 set transform-set az-IPSEC-PROPOSAL-SET
 set ikev2-profile az-PROFILE2
!
!
interface Loopback0
 ip address 172.16.0.3 255.255.255.255
!
interface Tunnel0
 ip address 172.16.0.1 255.255.255.255
 ip tcp adjust-mss 1350
 tunnel source GigabitEthernet1
 tunnel mode ipsec ipv4
 tunnel destination 52.175.253.112
 tunnel protection ipsec profile az-VTI1
!
interface Tunnel1
 ip address 172.16.0.2 255.255.255.255
 ip tcp adjust-mss 1350
 tunnel source GigabitEthernet1
 tunnel mode ipsec ipv4
 tunnel destination 52.175.250.191
 tunnel protection ipsec profile az-VTI2
!
interface GigabitEthernet1
 description External interface
 ip address x.243.229.110 255.255.255.252
 negotiation auto
 no mop enabled
 no mop sysid
!
interface GigabitEthernet2
 ip address 10.0.0.1 255.255.255.0
 negotiation auto
 no mop enabled
 no mop sysid
!
router bgp 65010
 bgp router-id interface Loopback0
 bgp log-neighbor-changes
 network 10.0.0.0 mask 255.255.255.0
 network 10.1.10.0 mask 255.255.255.128
 neighbor 10.2.0.228 remote-as 65000
 neighbor 10.2.0.228 ebgp-multihop 5
 neighbor 10.2.0.228 update-source Loopback0
 neighbor 10.2.0.228 soft-reconfiguration inbound
 neighbor 10.2.0.228 filter-list 10 out
 neighbor 10.2.0.229 remote-as 65000    
 neighbor 10.2.0.229 ebgp-multihop 5
 neighbor 10.2.0.229 update-source Loopback0
 neighbor 10.2.0.229 soft-reconfiguration inbound
 maximum-paths eibgp 2
!
ip route 0.0.0.0 0.0.0.0 10.1.10.1
ip route 10.2.0.228 255.255.255.255 Tunnel0
ip route 10.2.0.229 255.255.255.255 Tunnel1
!
```

## <a name="5-configure-vpn-device-filtering-and-firewalls-optional"></a><a name="firewalls"></a>5. VPN cihaz filtreleme ve güvenlik duvarları yapılandırma (isteğe bağlı)

Güvenlik duvarınızı ve filtrelemenizi gereksinimlerinize göre yapılandırın.

## <a name="6-test-and-validate-the-ipsec-tunnel"></a><a name="testipsec"></a>6. Test ve IPsec tünel doğrulamak

IPsec tünellerinin durumu Azure VPN ağ geçidinde Powershell komutları tarafından doğrulanabilir:

```azurepowershell-interactive
Get-AzVirtualNetworkGatewayConnection -Name vpn2local1 -ResourceGroupName myRG | Select-Object  ConnectionStatus,EgressBytesTransferred,IngressBytesTransferred | fl
```

Örnek çıktı:

```azurepowershell
ConnectionStatus        : Connected
EgressBytesTransferred  : 17734660
IngressBytesTransferred : 10538211
```

Azure VPN ağ geçidi örneklerindeki tünellerin durumunu bağımsız olarak denetlemek için aşağıdaki örneği kullanın:

```azurepowershell-interactive
Get-AzVirtualNetworkGatewayConnection -Name vpn2local1 -ResourceGroupName myRG | Select-Object -ExpandProperty TunnelConnectionStatus
```

Örnek çıktı:

```azurepowershell
Tunnel                           : vpn2local1_52.175.250.191
ConnectionStatus                 : Connected
IngressBytesTransferred          : 4877438
EgressBytesTransferred           : 8754071
LastConnectionEstablishedUtcTime : 11/04/2017 17:03:30

Tunnel                           : vpn2local1_52.175.253.112
ConnectionStatus                 : Connected
IngressBytesTransferred          : 5660773
EgressBytesTransferred           : 8980589
LastConnectionEstablishedUtcTime : 11/04/2017 17:03:13
```

Şirket içi VPN aygıtınızdaki tünel durumunu da kontrol edebilirsiniz.

Cisco CSR1000 örneği:

```
show crypto session detail
show crypto ikev2 sa
show crypto ikev2 session detail
show crypto ipsec sa
```

Örnek çıktı:

```
csr1#show crypto session detail

Crypto session current status

Code: C - IKE Configuration mode, D - Dead Peer Detection
K - Keepalives, N - NAT-traversal, T - cTCP encapsulation
X - IKE Extended Authentication, F - IKE Fragmentation
R - IKE Auto Reconnect

Interface: Tunnel1
Profile: az-PROFILE2
Uptime: 00:52:46
Session status: UP-ACTIVE
Peer: 52.175.250.191 port 4500 fvrf: (none) ivrf: (none)
      Phase1_id: 52.175.250.191
      Desc: (none)
  Session ID: 3
  IKEv2 SA: local 10.1.10.50/4500 remote 52.175.250.191/4500 Active
          Capabilities:DN connid:3 lifetime:23:07:14
  IPSEC FLOW: permit ip 0.0.0.0/0.0.0.0 0.0.0.0/0.0.0.0
        Active SAs: 2, origin: crypto map
        Inbound:  #pkts dec'ed 279 drop 0 life (KB/Sec) 4607976/433
        Outbound: #pkts enc'ed 164 drop 0 life (KB/Sec) 4607992/433

Interface: Tunnel0
Profile: az-PROFILE1
Uptime: 00:52:43
Session status: UP-ACTIVE
Peer: 52.175.253.112 port 4500 fvrf: (none) ivrf: (none)
      Phase1_id: 52.175.253.112
      Desc: (none)
  Session ID: 2
  IKEv2 SA: local 10.1.10.50/4500 remote 52.175.253.112/4500 Active
          Capabilities:DN connid:2 lifetime:23:07:17
  IPSEC FLOW: permit ip 0.0.0.0/0.0.0.0 0.0.0.0/0.0.0.0
        Active SAs: 2, origin: crypto map
        Inbound:  #pkts dec'ed 668 drop 0 life (KB/Sec) 4607926/437
        Outbound: #pkts enc'ed 477 drop 0 life (KB/Sec) 4607953/437
```

Sanal Tünel Arabirimi (VTI) üzerindeki hat protokolü, IKE aşaması 2 tamamlanana kadar "yukarı" olarak değişmez. Aşağıdaki komut güvenlik ilişkisini doğrular:

```
csr1#show crypto ikev2 sa

IPv4 Crypto IKEv2  SA

Tunnel-id Local                 Remote                fvrf/ivrf            Status
2         10.1.10.50/4500       52.175.253.112/4500   none/none            READY
      Encr: AES-CBC, keysize: 256, PRF: SHA1, Hash: SHA96, DH Grp:2, Auth sign: PSK, Auth verify: PSK
      Life/Active Time: 86400/3277 sec

Tunnel-id Local                 Remote                fvrf/ivrf            Status
3         10.1.10.50/4500       52.175.250.191/4500   none/none            READY
      Encr: AES-CBC, keysize: 256, PRF: SHA1, Hash: SHA96, DH Grp:2, Auth sign: PSK, Auth verify: PSK
      Life/Active Time: 86400/3280 sec

IPv6 Crypto IKEv2  SA

csr1#show crypto ipsec sa | inc encaps|decaps
    #pkts encaps: 177, #pkts encrypt: 177, #pkts digest: 177
    #pkts decaps: 296, #pkts decrypt: 296, #pkts verify: 296
    #pkts encaps: 554, #pkts encrypt: 554, #pkts digest: 554
    #pkts decaps: 746, #pkts decrypt: 746, #pkts verify: 746
```

### <a name="verify-end-to-end-connectivity-between-the-inside-network-on-premises-and-the-azure-vnet"></a><a name="verifye2e"></a>Şirket içi ağ ile Azure VNet arasındaki uçuçbağlantıyı doğrulayın

IPsec tünelleri dolduysa ve statik yollar doğru ayarlanmışsa, uzak BGP eşin IP adresini pingleme niz gerekir:

```
csr1#ping 10.2.0.228
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 10.2.0.228, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 5/5/5 ms

#ping 10.2.0.229
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 10.2.0.229, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 4/5/6 ms
```

### <a name="verify-the-bgp-sessions-over-ipsec"></a><a name="verifybgp"></a>IPsec üzerinden BGP oturumlarını doğrulayın

Azure VPN ağ geçidinde, BGP eşin durumunu doğrulayın:

```azurepowershell-interactive
Get-AzVirtualNetworkGatewayBGPPeerStatus -VirtualNetworkGatewayName vpnGtw -ResourceGroupName SEA-C1-VPN-ER | ft
```

Örnek çıktı:

```azurepowershell
  Asn ConnectedDuration LocalAddress MessagesReceived MessagesSent Neighbor    RoutesReceived State    
  --- ----------------- ------------ ---------------- ------------ --------    -------------- -----    
65010 00:57:19.9003584  10.2.0.228               68           72   172.16.0.10              2 Connected
65000                   10.2.0.228                0            0   10.2.0.228               0 Unknown  
65000 07:13:51.0109601  10.2.0.228              507          500   10.2.0.229               6 Connected
```

EBGP üzerinden vpn konsantratöründen şirket içinde alınan ağ önekleri listesini doğrulamak için "Origin" özelliğine göre filtre uygulayabilirsiniz:

```azurepowershell-interactive
Get-AzVirtualNetworkGatewayLearnedRoute -VirtualNetworkGatewayName vpnGtw -ResourceGroupName myRG  | Where-Object Origin -eq "EBgp" |ft
```

Örnek çıktıda, ASN 65010, VPN'deki BGP otonom sistem numarasıdır.

```azurepowershell
AsPath LocalAddress Network      NextHop     Origin SourcePeer  Weight
------ ------------ -------      -------     ------ ----------  ------
65010  10.2.0.228   10.1.10.0/25 172.16.0.10 EBgp   172.16.0.10  32768
65010  10.2.0.228   10.0.0.0/24  172.16.0.10 EBgp   172.16.0.10  32768
```

İlan edilen rotaların listesini görmek için:

```azurepowershell-interactive
Get-AzVirtualNetworkGatewayAdvertisedRoute -VirtualNetworkGatewayName vpnGtw -ResourceGroupName myRG -Peer 10.2.0.228 | ft
```

Örnek çıktı:

```azurepowershell
AsPath LocalAddress Network        NextHop    Origin SourcePeer Weight
------ ------------ -------        -------    ------ ---------- ------
       10.2.0.229   10.2.0.0/24    10.2.0.229 Igp                  0
       10.2.0.229   172.16.0.10/32 10.2.0.229 Igp                  0
       10.2.0.229   172.16.0.5/32  10.2.0.229 Igp                  0
       10.2.0.229   172.16.0.1/32  10.2.0.229 Igp                  0
65010  10.2.0.229   10.1.10.0/25   10.2.0.229 Igp                  0
65010  10.2.0.229   10.0.0.0/24    10.2.0.229 Igp                  0
```

Şirket içi Cisco CSR1000 için örnek:

```
csr1#show ip bgp neighbors 10.2.0.228 routes
BGP table version is 7, local router ID is 172.16.0.10
Status codes: s suppressed, d damped, h history, * valid, > best, i - internal,
              r RIB-failure, S Stale, m multipath, b backup-path, f RT-Filter,
              x best-external, a additional-path, c RIB-compressed,
              t secondary path,
Origin codes: i - IGP, e - EGP, ? - incomplete
RPKI validation codes: V valid, I invalid, N Not found

     Network          Next Hop            Metric LocPrf Weight Path
 *>   10.2.0.0/24      10.2.0.228                             0 65000 i
 r>   172.16.0.1/32    10.2.0.228                             0 65000 i
 r>   172.16.0.2/32    10.2.0.228                             0 65000 i
 r>   172.16.0.3/32   10.2.0.228                             0 65000 i

Total number of prefixes 4
```

Şirket içi Cisco CSR1000'den Azure VPN ağ geçidine reklamı yapılan ağların listesi aşağıdaki komut kullanılarak listelenebilir:

```
csr1#show ip bgp neighbors 10.2.0.228 advertised-routes
BGP table version is 7, local router ID is 172.16.0.10
Status codes: s suppressed, d damped, h history, * valid, > best, i - internal,
              r RIB-failure, S Stale, m multipath, b backup-path, f RT-Filter,
              x best-external, a additional-path, c RIB-compressed,
              t secondary path,
Origin codes: i - IGP, e - EGP, ? - incomplete
RPKI validation codes: V valid, I invalid, N Not found

     Network          Next Hop            Metric LocPrf Weight Path
 *>   10.0.0.0/24      0.0.0.0                  0         32768 i
 *>   10.1.10.0/25     0.0.0.0                  0         32768 i

Total number of prefixes 2
```

## <a name="next-steps"></a>Sonraki adımlar

* [ExpressRoute için Ağ Performansı İzleyicisi’ni Yapılandırma](how-to-npm.md)

* [Varolan VPN ağ geçidi bağlantısına sahip bir VNet'e siteden siteye bağlantı ekleme](../vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md)
