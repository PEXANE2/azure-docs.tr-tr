---
title: PowerShell kullanarak Azure Güvenlik Duvar'ı karma ağda dağıtma & yapılandırma
description: Bu makalede, Azure PowerShell'i kullanarak Azure Güvenlik Duvarı'nı nasıl dağıtabileceğinizi ve yapılandırabileceğinizi öğreneceksiniz.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: article
ms.date: 01/08/2020
ms.author: victorh
customer intent: As an administrator, I want to control network access from an on-premises network to an Azure virtual network.
ms.openlocfilehash: 37bb28419f23fee2c179171a2e5c0e4e851ac9a0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77471763"
---
# <a name="deploy-and-configure-azure-firewall-in-a-hybrid-network-using-azure-powershell"></a>Azure PowerShell kullanarak hibrit bir ağda Azure Güvenlik Duvarı'nı dağıtma ve yapılandırma

Karma bir ağ oluşturmak için şirket içi ağınızı bir Azure sanal ağına bağladığınızda, Azure ağ kaynaklarına erişimi denetleme olanağı genel bir güvenlik planının önemli bir parçasıdır.

İzin verilen ve reddedilen ağ trafiğini tanımlayan kuralları kullanarak karma ağdaki ağ erişimini denetlemek için Azure Güvenlik Duvarı'nı kullanabilirsiniz.

Bu makale için üç sanal ağ oluşturursunuz:

- **VNet-Hub** - güvenlik duvarı bu sanal ağda.
- **VNet-Spoke** - konuşan sanal ağ Azure'da bulunan iş yükünü temsil eder.
- **VNet-Onprem** - Şirket içi sanal ağ, şirket içi bir ağı temsil eder. Gerçek bir dağıtımda, VPN veya ExpressRoute bağlantısı yla bağlanabilir. Basitlik için bu makalede VPN ağ geçidi bağlantısı kullanılır ve Azure'da bulunan bir sanal ağ şirket içi ağı temsil etmek için kullanılır.

![Hibrit ağda güvenlik duvarı](media/tutorial-hybrid-ps/hybrid-network-firewall.png)

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Değişkenleri tanımlama
> * Güvenlik duvarı hub'ı sanal ağ oluşturma
> * Konuşan sanal ağı oluşturma
> * Şirket içi sanal ağı oluşturma
> * Güvenlik duvarını yapılandırma ve dağıtma
> * VPN ağ geçitlerini oluşturma ve bağlama
> * Hub'ı eşleyin ve konuşan sanal ağlar
> * Yolları oluşturma
> * Sanal makineleri oluşturma
> * Güvenlik duvarını test etme

Bu öğreticiyi tamamlamak için Azure portalını kullanmak istiyorsanız, [Bkz. Öğretici: Azure portalını kullanarak karma bir ağda Azure Güvenlik Duvarı'nı dağıtın ve yapılandırın.](tutorial-hybrid-portal.md)

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Ön koşullar

Bu makalede, PowerShell'i yerel olarak çalıştırmanız gerekmektedir. Azure PowerShell modüllerini yüklemiş olmalısınız. Sürümü bulmak için `Get-Module -ListAvailable Az` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](https://docs.microsoft.com/powershell/azure/install-Az-ps). PowerShell sürümünü doğruladıktan sonra, Azure ile bağlantı oluşturmak için `Login-AzAccount` komutunu çalıştırın.

Bu senaryonun doğru çalışması için üç önemli gereksinimi vardır:

- Varsayılan ağ geçidi olarak Azure Güvenlik Duvarı IP adresini işaret eden, sözleştirilen alt netteki User Defined Route (UDR). Sanal ağ ağ geçidi rotası bu rota tablosunda **devre dışı** bırakılmalı.
- Hub ağ geçidi alt netindeki bir UDR, konuşan ağlara bir sonraki atlama olarak güvenlik duvarı IP adresini işaret etmelidir.

   BGP'den rotalar öğrendiği için Azure Güvenlik Duvarı alt netinde UDR gerekmez.
- VNet-Hub'ı VNet-Spoke'a eşlerken **AllowGatewayTransit** ayarladığınızdan ve VNet-Spoke'u VNet-Hub'a eşlerken de **UseRemoteGateways** ayarladığınızdan emin olun.

Bu yolların nasıl oluşturulduğunu görmek için bu makaledeki [Rotaları Oluştur](#create-the-routes) bölümüne bakın.

>[!NOTE]
>Azure Güvenlik Duvarı'nın doğrudan Internet bağlantısı olması gerekir. AzureFirewallSubnet'iniz BGP üzerinden şirket içi ağınıza varsayılan bir rota öğrenirse, doğrudan Internet bağlantısını korumak için **Internet** olarak ayarlanmış **NextHopType** değeriyle 0.0.0.0/0 UDR ile bunu geçersiz kılmanız gerekir.
>
>Azure Güvenlik Duvarı, zorunlu tünel kazmayı destekleyecek şekilde yapılandırılabilir. Daha fazla bilgi için Azure [Güvenlik Duvarı zorunlu tünel leme](forced-tunneling.md)bölümüne bakın.

>[!NOTE]
>Bir UDR varsayılan ağ geçidi olarak Azure Güvenlik Duvarı'nı işaret etse bile, doğrudan bakan VNet'ler arasındaki trafik doğrudan yönlendirilir. Bu senaryoda alt ağ trafiğine alt ağ göndermek için, udr'nin her iki alt ağda da açıkça hedef alt ağ önekini içermesi gerekir.

İlgili Azure PowerShell başvuru belgelerini incelemek [için](https://docs.microsoft.com/powershell/module/az.network/new-azfirewall)bkz.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="declare-the-variables"></a>Değişkenleri tanımlama

Aşağıdaki örnekte, bu makalenin değerlerini kullanarak değişkenler beyan eder. Bazı durumlarda, aboneliğinizde çalışmak için bazı değerleri kendi değerlerinizle değiştirmeniz gerekebilir. Gerekirse değişkenleri değiştirin, daha sonra kopyalayın ve PowerShell konsolunuza yapıştırın.

```azurepowershell
$RG1 = "FW-Hybrid-Test"
$Location1 = "East US"

# Variables for the firewall hub VNet

$VNetnameHub = "VNet-hub"
$SNnameHub = "AzureFirewallSubnet"
$VNetHubPrefix = "10.5.0.0/16"
$SNHubPrefix = "10.5.0.0/24"
$SNGWHubPrefix = "10.5.1.0/24"
$GWHubName = "GW-hub"
$GWHubpipName = "VNet-hub-GW-pip"
$GWIPconfNameHub = "GW-ipconf-hub"
$ConnectionNameHub = "hub-to-Onprem"

# Variables for the spoke virtual network

$VnetNameSpoke = "VNet-Spoke"
$SNnameSpoke = "SN-Workload"
$VNetSpokePrefix = "10.6.0.0/16"
$SNSpokePrefix = "10.6.0.0/24"
$SNSpokeGWPrefix = "10.6.1.0/24"

# Variables for the on-premises virtual network

$VNetnameOnprem = "Vnet-Onprem"
$SNNameOnprem = "SN-Corp"
$VNetOnpremPrefix = "192.168.0.0/16"
$SNOnpremPrefix = "192.168.1.0/24"
$SNGWOnpremPrefix = "192.168.2.0/24"
$GWOnpremName = "GW-Onprem"
$GWIPconfNameOnprem = "GW-ipconf-Onprem"
$ConnectionNameOnprem = "Onprem-to-hub"
$GWOnprempipName = "VNet-Onprem-GW-pip"

$SNnameGW = "GatewaySubnet"
```


## <a name="create-the-firewall-hub-virtual-network"></a>Güvenlik duvarı hub'ı sanal ağ oluşturma

İlk olarak, bu makalenin kaynaklarını içerecek kaynak grubunu oluşturun:

```azurepowershell
  New-AzResourceGroup -Name $RG1 -Location $Location1
  ```

Sanal ağa dahil edilecek alt ağları tanımlayın:

```azurepowershell
$FWsub = New-AzVirtualNetworkSubnetConfig -Name $SNnameHub -AddressPrefix $SNHubPrefix
$GWsub = New-AzVirtualNetworkSubnetConfig -Name $SNnameGW -AddressPrefix $SNGWHubPrefix
```

Şimdi, güvenlik duvarı hub sanal ağ oluşturun:

```azurepowershell
$VNetHub = New-AzVirtualNetwork -Name $VNetnameHub -ResourceGroupName $RG1 `
-Location $Location1 -AddressPrefix $VNetHubPrefix -Subnet $FWsub,$GWsub
```

Sanal ağınız için oluşturacağınız VPN ağ geçidine tahsis edilecek genel bir IP adresi isteyin. *Ayırma Yönteminin* **Dinamik**olduğuna dikkat edin. Kullanmak istediğiniz IP adresini belirtemezsiniz. IP adresi, VPN ağ geçidinize dinamik olarak ayrılır.

  ```azurepowershell
  $gwpip1 = New-AzPublicIpAddress -Name $GWHubpipName -ResourceGroupName $RG1 `
  -Location $Location1 -AllocationMethod Dynamic
```

## <a name="create-the-spoke-virtual-network"></a>Konuşan sanal ağı oluşturma

Konuşan sanal ağa eklenecek alt ağları tanımlayın:

```azurepowershell
$Spokesub = New-AzVirtualNetworkSubnetConfig -Name $SNnameSpoke -AddressPrefix $SNSpokePrefix
$GWsubSpoke = New-AzVirtualNetworkSubnetConfig -Name $SNnameGW -AddressPrefix $SNSpokeGWPrefix
```

Konuşan sanal ağı oluşturun:

```azurepowershell
$VNetSpoke = New-AzVirtualNetwork -Name $VnetNameSpoke -ResourceGroupName $RG1 `
-Location $Location1 -AddressPrefix $VNetSpokePrefix -Subnet $Spokesub,$GWsubSpoke
```

## <a name="create-the-on-premises-virtual-network"></a>Şirket içi sanal ağı oluşturma

Sanal ağa dahil edilecek alt ağları tanımlayın:

```azurepowershell
$Onpremsub = New-AzVirtualNetworkSubnetConfig -Name $SNNameOnprem -AddressPrefix $SNOnpremPrefix
$GWOnpremsub = New-AzVirtualNetworkSubnetConfig -Name $SNnameGW -AddressPrefix $SNGWOnpremPrefix
```

Şimdi, şirket içi sanal ağı oluşturun:

```azurepowershell
$VNetOnprem = New-AzVirtualNetwork -Name $VNetnameOnprem -ResourceGroupName $RG1 `
-Location $Location1 -AddressPrefix $VNetOnpremPrefix -Subnet $Onpremsub,$GWOnpremsub
```

Sanal ağ için oluşturacağınız ağ geçidine tahsis edilecek ortak bir IP adresi isteyin. *Ayırma Yönteminin* **Dinamik**olduğuna dikkat edin. Kullanmak istediğiniz IP adresini belirtemezsiniz. IP adresi, ağ geçidinize dinamik olarak ayrılır.

  ```azurepowershell
  $gwOnprempip = New-AzPublicIpAddress -Name $GWOnprempipName -ResourceGroupName $RG1 `
  -Location $Location1 -AllocationMethod Dynamic
```

## <a name="configure-and-deploy-the-firewall"></a>Güvenlik duvarını yapılandırma ve dağıtma

Şimdi güvenlik duvarını sanal hub ağına dağıtın.

```azurepowershell
# Get a Public IP for the firewall
$FWpip = New-AzPublicIpAddress -Name "fw-pip" -ResourceGroupName $RG1 `
  -Location $Location1 -AllocationMethod Static -Sku Standard
# Create the firewall
$Azfw = New-AzFirewall -Name AzFW01 -ResourceGroupName $RG1 -Location $Location1 -VirtualNetworkName $VNetnameHub -PublicIpName fw-pip

#Save the firewall private IP address for future use

$AzfwPrivateIP = $Azfw.IpConfigurations.privateipaddress
$AzfwPrivateIP

```

### <a name="configure-network-rules"></a>Ağ kurallarını yapılandırma

<!--- $Rule3 = New-AzFirewallNetworkRule -Name "AllowPing" -Protocol ICMP -SourceAddress $SNOnpremPrefix `
   -DestinationAddress $VNetSpokePrefix -DestinationPort *--->

```azurepowershell
$Rule1 = New-AzFirewallNetworkRule -Name "AllowWeb" -Protocol TCP -SourceAddress $SNOnpremPrefix `
   -DestinationAddress $VNetSpokePrefix -DestinationPort 80

$Rule2 = New-AzFirewallNetworkRule -Name "AllowRDP" -Protocol TCP -SourceAddress $SNOnpremPrefix `
   -DestinationAddress $VNetSpokePrefix -DestinationPort 3389

$NetRuleCollection = New-AzFirewallNetworkRuleCollection -Name RCNet01 -Priority 100 `
   -Rule $Rule1,$Rule2 -ActionType "Allow"
$Azfw.NetworkRuleCollections = $NetRuleCollection
Set-AzFirewall -AzureFirewall $Azfw
```

## <a name="create-and-connect-the-vpn-gateways"></a>VPN ağ geçitlerini oluşturma ve bağlama

Hub ve şirket içi sanal ağlar VPN ağ geçitleri üzerinden bağlanır.

### <a name="create-a-vpn-gateway-for-the-hub-virtual-network"></a>Hub sanal ağ için VPN ağ geçidi oluşturma

VPN ağ geçidi yapılandırmasını oluşturun. VPN ağ geçidi yapılandırması, kullanılacak alt ağı ve genel IP adresini tanımlar.

  ```azurepowershell
  $vnet1 = Get-AzVirtualNetwork -Name $VNetnameHub -ResourceGroupName $RG1
  $subnet1 = Get-AzVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet1
  $gwipconf1 = New-AzVirtualNetworkGatewayIpConfig -Name $GWIPconfNameHub `
  -Subnet $subnet1 -PublicIpAddress $gwpip1
  ```

Şimdi hub sanal ağ için VPN ağ geçidi oluşturun. Ağdan ağa yapılandırmalar bir RouteBased VpnType gerektirir. VPN ağ geçidinin oluşturulması, seçili VPN ağ geçidi SKU’suna bağlı olarak 45 dakika veya daha uzun sürebilir.

```azurepowershell
New-AzVirtualNetworkGateway -Name $GWHubName -ResourceGroupName $RG1 `
-Location $Location1 -IpConfigurations $gwipconf1 -GatewayType Vpn `
-VpnType RouteBased -GatewaySku basic
```

### <a name="create-a-vpn-gateway-for-the-on-premises-virtual-network"></a>Şirket içi sanal ağ için VPN ağ geçidi oluşturma

VPN ağ geçidi yapılandırmasını oluşturun. VPN ağ geçidi yapılandırması, kullanılacak alt ağı ve genel IP adresini tanımlar.

  ```azurepowershell
$vnet2 = Get-AzVirtualNetwork -Name $VNetnameOnprem -ResourceGroupName $RG1
$subnet2 = Get-AzVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet2
$gwipconf2 = New-AzVirtualNetworkGatewayIpConfig -Name $GWIPconfNameOnprem `
  -Subnet $subnet2 -PublicIpAddress $gwOnprempip
  ```

Şimdi şirket içi sanal ağ için VPN ağ geçidi oluşturun. Ağdan ağa yapılandırmalar bir RouteBased VpnType gerektirir. VPN ağ geçidinin oluşturulması, seçili VPN ağ geçidi SKU’suna bağlı olarak 45 dakika veya daha uzun sürebilir.

```azurepowershell
New-AzVirtualNetworkGateway -Name $GWOnpremName -ResourceGroupName $RG1 `
-Location $Location1 -IpConfigurations $gwipconf2 -GatewayType Vpn `
-VpnType RouteBased -GatewaySku basic
```

### <a name="create-the-vpn-connections"></a>VPN bağlantılarını oluşturma

Artık hub ve şirket içi ağ geçitleri arasında VPN bağlantıları oluşturabilirsiniz

#### <a name="get-the-vpn-gateways"></a>VPN ağ geçitlerini alma

```azurepowershell
$vnetHubgw = Get-AzVirtualNetworkGateway -Name $GWHubName -ResourceGroupName $RG1
$vnetOnpremgw = Get-AzVirtualNetworkGateway -Name $GWOnpremName -ResourceGroupName $RG1
```

#### <a name="create-the-connections"></a>Bağlantıları oluşturma

Bu adımda, hub sanal ağdan şirket içi sanal ağa bağlantı oluşturursunuz. Örneklerde paylaşılan bir anahtar göreceksiniz. Paylaşılan anahtar için kendi değerlerinizi kullanabilirsiniz. Paylaşılan anahtarın her iki bağlantıyla da eşleşiyor olması önemlidir. Bir bağlantı oluşturmak çok zaman almaz.

```azurepowershell
New-AzVirtualNetworkGatewayConnection -Name $ConnectionNameHub -ResourceGroupName $RG1 `
-VirtualNetworkGateway1 $vnetHubgw -VirtualNetworkGateway2 $vnetOnpremgw -Location $Location1 `
-ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3'
```
Şirket içinde sanal ağ bağlantısını hub'a oluşturun. VNet-Onprem'den VNet hub'ına bağlantı oluşturmanız dışında, bu adım öncekine benzer. Paylaşılan anahtarların eşleştiğinden emin olun. Bağlantı birkaç dakika içerisinde kurulacaktır.

  ```azurepowershell
  New-AzVirtualNetworkGatewayConnection -Name $ConnectionNameOnprem -ResourceGroupName $RG1 `
  -VirtualNetworkGateway1 $vnetOnpremgw -VirtualNetworkGateway2 $vnetHubgw -Location $Location1 `
  -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3'
  ```

#### <a name="verify-the-connection"></a>Bağlantıyı doğrulama

*Get-AzVirtualNetworkGatewayConnection* cmdlet kullanarak başarılı bir bağlantıyı doğrulayabilirsiniz, *-Hata Ayıklama*ile veya olmadan. Aşağıdaki cmdlet örneğini kullanın ve değerleri, kendi değerlerinizle eşleşecek şekilde yapılandırın. İstendiğinde, **Tümü**'nü çalıştırmak için **A** seçin. Örnekte, *-Ad,* sınamak istediğiniz bağlantının adını ifade eder.

```azurepowershell
Get-AzVirtualNetworkGatewayConnection -Name $ConnectionNameHub -ResourceGroupName $RG1
```

Cmdlet tamamlandıktan sonra değerleri görüntüleyin. Aşağıdaki örnekte, bağlantı durumu *Bağlandı* olarak gösterilir; ayrıca giriş ve çıkış baytlarını görebilirsiniz.

```
"connectionStatus": "Connected",
"ingressBytesTransferred": 33509044,
"egressBytesTransferred": 4142431
```

## <a name="peer-the-hub-and-spoke-virtual-networks"></a>Hub'ı eşleyin ve konuşan sanal ağlar

Şimdi hub eş ve sanal ağlar konuştu.

```azurepowershell
# Peer hub to spoke
Add-AzVirtualNetworkPeering -Name HubtoSpoke -VirtualNetwork $VNetHub -RemoteVirtualNetworkId $VNetSpoke.Id -AllowGatewayTransit

# Peer spoke to hub
Add-AzVirtualNetworkPeering -Name SpoketoHub -VirtualNetwork $VNetSpoke -RemoteVirtualNetworkId $VNetHub.Id -AllowForwardedTraffic -UseRemoteGateways
```

## <a name="create-the-routes"></a>Yolları oluşturma

Şimdi birkaç yol oluşturun:

- Güvenlik duvarı IP adresi üzerinden hub ağ geçidi alt ağından uç alt ağına giden bir yol
- Güvenlik duvarı IP adresi üzerinden uç alt ağından gelen varsayılan yol

```azurepowershell
#Create a route table
$routeTableHubSpoke = New-AzRouteTable `
  -Name 'UDR-Hub-Spoke' `
  -ResourceGroupName $RG1 `
  -location $Location1

#Create a route
Get-AzRouteTable `
  -ResourceGroupName $RG1 `
  -Name UDR-Hub-Spoke `
  | Add-AzRouteConfig `
  -Name "ToSpoke" `
  -AddressPrefix $VNetSpokePrefix `
  -NextHopType "VirtualAppliance" `
  -NextHopIpAddress $AzfwPrivateIP `
 | Set-AzRouteTable

#Associate the route table to the subnet

Set-AzVirtualNetworkSubnetConfig `
  -VirtualNetwork $VNetHub `
  -Name $SNnameGW `
  -AddressPrefix $SNGWHubPrefix `
  -RouteTable $routeTableHubSpoke | `
Set-AzVirtualNetwork

#Now create the default route

#Create a table, with BGP route propagation disabled. The property is now called "Virtual network gateway route propagation," but the API still refers to the parameter as "DisableBgpRoutePropagation."
$routeTableSpokeDG = New-AzRouteTable `
  -Name 'UDR-DG' `
  -ResourceGroupName $RG1 `
  -location $Location1 `
  -DisableBgpRoutePropagation

#Create a route
Get-AzRouteTable `
  -ResourceGroupName $RG1 `
  -Name UDR-DG `
  | Add-AzRouteConfig `
  -Name "ToFirewall" `
  -AddressPrefix 0.0.0.0/0 `
  -NextHopType "VirtualAppliance" `
  -NextHopIpAddress $AzfwPrivateIP `
 | Set-AzRouteTable

#Associate the route table to the subnet

Set-AzVirtualNetworkSubnetConfig `
  -VirtualNetwork $VNetSpoke `
  -Name $SNnameSpoke `
  -AddressPrefix $SNSpokePrefix `
  -RouteTable $routeTableSpokeDG | `
Set-AzVirtualNetwork
```

## <a name="create-virtual-machines"></a>Sanal makineler oluşturma

Şimdi konuşan iş yükünü ve şirket içi sanal makineleri oluşturun ve bunları uygun alt ağlara yerleştirin.

### <a name="create-the-workload-virtual-machine"></a>İş yükü sanal makinesi oluşturma

Ortak IP adresi olmayan ve ping'e izin veren IIS çalıştıran, konuşan sanal ağda sanal bir makine oluşturun.
İstendiğinde, sanal makine için bir kullanıcı adı ve parola yazın.

```azurepowershell
# Create an inbound network security group rule for ports 3389 and 80
$nsgRuleRDP = New-AzNetworkSecurityRuleConfig -Name Allow-RDP  -Protocol Tcp `
  -Direction Inbound -Priority 200 -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix $SNSpokePrefix -DestinationPortRange 3389 -Access Allow
$nsgRuleWeb = New-AzNetworkSecurityRuleConfig -Name Allow-web  -Protocol Tcp `
  -Direction Inbound -Priority 202 -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix $SNSpokePrefix -DestinationPortRange 80 -Access Allow

# Create a network security group
$nsg = New-AzNetworkSecurityGroup -ResourceGroupName $RG1 -Location $Location1 -Name NSG-Spoke02 -SecurityRules $nsgRuleRDP,$nsgRuleWeb

#Create the NIC
$NIC = New-AzNetworkInterface -Name spoke-01 -ResourceGroupName $RG1 -Location $Location1 -SubnetId $VnetSpoke.Subnets[0].Id -NetworkSecurityGroupId $nsg.Id

#Define the virtual machine
$VirtualMachine = New-AzVMConfig -VMName VM-Spoke-01 -VMSize "Standard_DS2"
$VirtualMachine = Set-AzVMOperatingSystem -VM $VirtualMachine -Windows -ComputerName Spoke-01 -ProvisionVMAgent -EnableAutoUpdate
$VirtualMachine = Add-AzVMNetworkInterface -VM $VirtualMachine -Id $NIC.Id
$VirtualMachine = Set-AzVMSourceImage -VM $VirtualMachine -PublisherName 'MicrosoftWindowsServer' -Offer 'WindowsServer' -Skus '2016-Datacenter' -Version latest

#Create the virtual machine
New-AzVM -ResourceGroupName $RG1 -Location $Location1 -VM $VirtualMachine -Verbose

#Install IIS on the VM
Set-AzVMExtension `
    -ResourceGroupName $RG1 `
    -ExtensionName IIS `
    -VMName VM-Spoke-01 `
    -Publisher Microsoft.Compute `
    -ExtensionType CustomScriptExtension `
    -TypeHandlerVersion 1.4 `
    -SettingString '{"commandToExecute":"powershell Add-WindowsFeature Web-Server"}' `
    -Location $Location1
```

<!---#Create a host firewall rule to allow ping in
Set-AzVMExtension `
    -ResourceGroupName $RG1 `
    -ExtensionName IIS `
    -VMName VM-Spoke-01 `
    -Publisher Microsoft.Compute `
    -ExtensionType CustomScriptExtension `
    -TypeHandlerVersion 1.4 `
    -SettingString '{"commandToExecute":"powershell New-NetFirewallRule –DisplayName "Allow ICMPv4-In" –Protocol ICMPv4"}' `
    -Location $Location1--->

### <a name="create-the-on-premises-virtual-machine"></a>Şirket içi sanal makineyi oluşturun

Bu, Uzak Masaüstü'nü kullanarak genel IP adresine bağlanmak için kullandığınız basit bir sanal makinedir. Buradan, güvenlik duvarı üzerinden şirket içi sunucuya bağlanırsınız. İstendiğinde, sanal makine için bir kullanıcı adı ve parola yazın.

```azurepowershell
New-AzVm `
    -ResourceGroupName $RG1 `
    -Name "VM-Onprem" `
    -Location $Location1 `
    -VirtualNetworkName $VNetnameOnprem `
    -SubnetName $SNNameOnprem `
    -OpenPorts 3389 `
    -Size "Standard_DS2"
```

## <a name="test-the-firewall"></a>Güvenlik duvarını test etme

İlk olarak, **VM-spoke-01** sanal makinenin özel IP adresini alın ve not alın.

```azurepowershell
$NIC.IpConfigurations.privateipaddress
```

Azure portalından, **VM-Onprem** sanal makinesine bağlanın.
<!---2. Open a Windows PowerShell command prompt on **VM-Onprem**, and ping the private IP for **VM-spoke-01**.

   You should get a reply.--->
**VM-Onprem**üzerinde bir web tarayıcısı açın\<ve http:// VM-spoke-01 özel IP\>göz atın.

Internet Information Services varsayılan sayfasını görmelisiniz.

**VM-Onprem** sanal makinesinden özel IP adresinde **VM-spoke-01**'e uzak masaüstü açın.

Bağlantınız başarılı olmalı ve seçtiğiniz kullanıcı adıyla parolayı kullanarak oturum açabilmelisiniz.

Şimdi güvenlik duvarı kurallarının işe yaradığını doğruladınız:

<!---- You can ping the server on the spoke VNet.--->
- Konuşan sanal ağdaki web sunucusuna göz atabilirsiniz.
- RDP kullanarak konuşan sanal ağdaki sunucuya bağlanabilirsiniz.

Ardından, güvenlik duvarı kurallarının beklendiği gibi çalıştığını doğrulamak için güvenlik duvarı ağ kuralı koleksiyonu eylemini **Reddet** olarak değiştirin. Kural koleksiyonu eylemini **Reddet** olarak değiştirmek için aşağıdaki betiği çalıştırın.

```azurepowershell
$rcNet = $azfw.GetNetworkRuleCollectionByName("RCNet01")
$rcNet.action.type = "Deny"

Set-AzFirewall -AzureFirewall $azfw
```

Şimdi testleri yeniden çalıştırın. Bu kez tümü başarısız olmalıdır. Değişen kuralları test etmeden önce var olan tüm uzak masaüstlerini kapatın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Güvenlik duvarı kaynaklarını bir sonraki öğretici için tutabilirsiniz veya artık gerekli değilse **FW-Hybrid-Test** kaynak grubunu silerek güvenlik duvarıyla ilgili tüm kaynakları silebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Şimdi Azure Güvenlik Duvarı günlüklerini izleyebilirsiniz.

[Öğretici: Azure Güvenlik Duvarı günlüklerini izleme](./tutorial-diagnostics.md)
