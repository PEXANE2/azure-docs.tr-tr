---
title: 'Quickstart: PowerShell kullanarak doğrudan web trafiği'
titleSuffix: Azure Application Gateway
description: Web trafiğini arka uç havuzundaki sanal makinelere yönlendiren bir Azure Uygulama Ağ Geçidi oluşturmak için Azure PowerShell'i nasıl kullanacağınızı öğrenin.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: quickstart
ms.date: 03/05/2020
ms.author: victorh
ms.custom: mvc
ms.openlocfilehash: abb38dfc342c8ff692ed1a3a05376b5dcefe8a3d
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "78399561"
---
# <a name="quickstart-direct-web-traffic-with-azure-application-gateway-using-azure-powershell"></a>Hızlı başlatma: Azure PowerShell'i kullanarak Azure Uygulama Ağ Geçidi ile doğrudan web trafiği

Bu hızlı başlangıçta, bir uygulama ağ geçidi oluşturmak için Azure PowerShell'i kullanırsınız. Sonra doğru çalıştığından emin olmak için test edin. 

Uygulama ağ geçidi, uygulama web trafiğini arka uç havuzundaki belirli kaynaklara yönlendirir. Dinleyicileri bağlantı noktalarına atar, kurallar oluşturur ve arka uç havuzuna kaynak eklersiniz. Basitlik uğruna, bu makalede, bir ortak ön uç IP, uygulama ağ geçidi, temel bir istek yönlendirme kuralı ve arka uç havuzunda iki sanal makine tek bir site barındırmak için temel bir dinleyici ile basit bir kurulum kullanır.

Bu hızlı başlatmayı [Azure CLI](quick-create-cli.md) veya [Azure portalını](quick-create-portal.md)kullanarak da tamamlayabilirsiniz.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Ön koşullar

- Etkin bir aboneliği olan bir Azure hesabı. [Ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
- [Azure PowerShell sürüm 1.0.0 veya sonraki](/powershell/azure/install-az-ps) (Azure PowerShell'i yerel olarak çalıştırıyorsanız).

## <a name="connect-to-azure"></a>Azure'a Bağlanma

Azure'a bağlanmak `Connect-AzAccount`için çalıştırın.

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Azure'da, ilgili kaynakları bir kaynak grubuna ayırırsınız. Varolan bir kaynak grubunu kullanabilir veya yeni bir kaynak oluşturabilirsiniz.

Yeni bir kaynak grubu oluşturmak `New-AzResourceGroup` için cmdlet'i kullanın: 

```azurepowershell-interactive
New-AzResourceGroup -Name myResourceGroupAG -Location eastus
```
## <a name="create-network-resources"></a>Ağ kaynakları oluşturma

Azure'un oluşturduğunuz kaynaklar arasında iletişim kurabilmesi için sanal bir ağa ihtiyacı vardır.  Uygulama ağ geçidi alt ağı yalnızca uygulama ağ geçitleri içerebilir. Başka kaynaklara izin verilmez.  Uygulama Ağ Geçidi için yeni bir alt ağ oluşturabilir veya varolan bir alt ağ oluşturabilirsiniz. Bu örnekte, bu örnekte iki alt ağ oluşturursunuz: biri uygulama ağ geçidi için, diğeri de arka uç sunucuları için. Uygulama Ağ Geçidi'nin Frontend IP'sini kullanım durumunuza göre Genel veya Özel olacak şekilde yapılandırabilirsiniz. Bu örnekte, bir Public Frontend IP'si seçersiniz.

1. Kullanarak alt ağ yapılandırmaları `New-AzVirtualNetworkSubnetConfig`oluşturun.
2. Kullanarak alt ağ yapılandırmaları ile `New-AzVirtualNetwork`sanal ağ oluşturun. 
3. Genel IP adresini `New-AzPublicIpAddress`kullanarak oluşturun. 

```azurepowershell-interactive
$agSubnetConfig = New-AzVirtualNetworkSubnetConfig `
  -Name myAGSubnet `
  -AddressPrefix 10.0.1.0/24
$backendSubnetConfig = New-AzVirtualNetworkSubnetConfig `
  -Name myBackendSubnet `
  -AddressPrefix 10.0.2.0/24
New-AzVirtualNetwork `
  -ResourceGroupName myResourceGroupAG `
  -Location eastus `
  -Name myVNet `
  -AddressPrefix 10.0.0.0/16 `
  -Subnet $agSubnetConfig, $backendSubnetConfig
New-AzPublicIpAddress `
  -ResourceGroupName myResourceGroupAG `
  -Location eastus `
  -Name myAGPublicIPAddress `
  -AllocationMethod Static `
  -Sku Standard
```
## <a name="create-an-application-gateway"></a>Uygulama ağ geçidi oluşturma

### <a name="create-the-ip-configurations-and-frontend-port"></a>IP yapılandırmaları ve ön uç bağlantı noktası oluşturma

1. Oluşturduğunuz alt ağı uygulama ağ geçidiyle ilişkilendiren yapılandırmayı oluşturmak için kullanın. `New-AzApplicationGatewayIPConfiguration` 
2. Daha `New-AzApplicationGatewayFrontendIPConfig` önce oluşturduğunuz genel IP adresini uygulama ağ geçidine atayan yapılandırmayı oluşturmak için kullanın. 
3. Uygulama `New-AzApplicationGatewayFrontendPort` ağ geçidine erişmek için bağlantı noktası 80'i atamak için kullanın.

```azurepowershell-interactive
$vnet   = Get-AzVirtualNetwork -ResourceGroupName myResourceGroupAG -Name myVNet
$subnet = Get-AzVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name myAGSubnet
$pip    = Get-AzPublicIPAddress -ResourceGroupName myResourceGroupAG -Name myAGPublicIPAddress 
$gipconfig = New-AzApplicationGatewayIPConfiguration `
  -Name myAGIPConfig `
  -Subnet $subnet
$fipconfig = New-AzApplicationGatewayFrontendIPConfig `
  -Name myAGFrontendIPConfig `
  -PublicIPAddress $pip
$frontendport = New-AzApplicationGatewayFrontendPort `
  -Name myFrontendPort `
  -Port 80
```

### <a name="create-the-backend-pool"></a>Arka uç havuzunu oluşturma

1. Uygulama `New-AzApplicationGatewayBackendAddressPool` ağ geçidi için arka uç havuzunu oluşturmak için kullanın. Arka uç havuzu şimdilik boş olacak ve bir sonraki bölümde arka uç sunucu NIC'lerini oluştururken, bunları arka uç havuzuna eklersiniz.
2. Arka uç havuzunun ayarlarını `New-AzApplicationGatewayBackendHttpSetting`.

```azurepowershell-interactive
$address1 = Get-AzNetworkInterface -ResourceGroupName myResourceGroupAG -Name myNic1
$address2 = Get-AzNetworkInterface -ResourceGroupName myResourceGroupAG -Name myNic2
$backendPool = New-AzApplicationGatewayBackendAddressPool `
  -Name myAGBackendPool
$poolSettings = New-AzApplicationGatewayBackendHttpSetting `
  -Name myPoolSettings `
  -Port 80 `
  -Protocol Http `
  -CookieBasedAffinity Enabled `
  -RequestTimeout 30
```

### <a name="create-the-listener-and-add-a-rule"></a>Dinleyiciyi oluşturma ve kural ekleme

Azure, trafiği arka uç havuzuna uygun şekilde yönlendirme için uygulama ağ geçidini etkinleştirmek için bir dinleyici gerektirir. Azure ayrıca dinleyicinin gelen trafik için hangi arka uç havuzunu kullanacağını bilmesi için bir kural da gerektirir. 

1. Daha önce oluşturduğunuz ön uç yapılandırması ve ön uç bağlantı noktasını kullanarak `New-AzApplicationGatewayHttpListener` bir dinleyici oluşturun. 
2. `New-AzApplicationGatewayRequestRoutingRule` *Kural1*adlı bir kural oluşturmak için kullanın. 

```azurepowershell-interactive
$defaultlistener = New-AzApplicationGatewayHttpListener `
  -Name myAGListener `
  -Protocol Http `
  -FrontendIPConfiguration $fipconfig `
  -FrontendPort $frontendport
$frontendRule = New-AzApplicationGatewayRequestRoutingRule `
  -Name rule1 `
  -RuleType Basic `
  -HttpListener $defaultlistener `
  -BackendAddressPool $backendPool `
  -BackendHttpSettings $poolSettings
```

### <a name="create-the-application-gateway"></a>Uygulama ağ geçidi oluşturma

Artık gerekli destekleyici kaynakları oluşturduğunuza göre, uygulama ağ geçidini oluşturun:

1. Uygulama `New-AzApplicationGatewaySku` ağ geçidi için parametreleri belirtmek için kullanın.
2. Uygulama `New-AzApplicationGateway` ağ geçidini oluşturmak için kullanın.

```azurepowershell-interactive
$sku = New-AzApplicationGatewaySku `
  -Name Standard_v2 `
  -Tier Standard_v2 `
  -Capacity 2
New-AzApplicationGateway `
  -Name myAppGateway `
  -ResourceGroupName myResourceGroupAG `
  -Location eastus `
  -BackendAddressPools $backendPool `
  -BackendHttpSettingsCollection $poolSettings `
  -FrontendIpConfigurations $fipconfig `
  -GatewayIpConfigurations $gipconfig `
  -FrontendPorts $frontendport `
  -HttpListeners $defaultlistener `
  -RequestRoutingRules $frontendRule `
  -Sku $sku
```

### <a name="backend-servers"></a>Arka uç sunucuları

Artık Uygulama Ağ Geçidi'ni oluşturduğunuza göre, web sitelerini barındıracak arka uç sanal makineleri oluşturun. Arka uç NIC'lerden, sanal makine ölçek kümelerinden, genel IP'lerden, dahili IP'lerden, tam nitelikli alan adlarından (FQDN) ve Azure Uygulama Hizmeti gibi çok kiracılı arka uçlardan oluşabilir. Bu örnekte, Azure'un uygulama ağ geçidi için arka uç sunucusu olarak kullanması için iki sanal makine oluşturursunuz. Azure'un uygulama ağ geçidini başarıyla oluşturduğunu doğrulamak için sanal makinelere IIS'yi de yüklersiniz.

#### <a name="create-two-virtual-machines"></a>İki sanal makine oluşturma

1. En son oluşturulan Application Gateway arka `Get-AzApplicationGatewayBackendAddressPool`uç havuzu yapılandırmasını alın.
2. '' ile `New-AzNetworkInterface`bir ağ arabirimi oluşturun.
3. `New-AzVMConfig`' ile sanal bir makine yapılandırması oluşturun
4. Ile sanal makine `New-AzVM`oluşturun.

Sanal makineleri oluşturmak için aşağıdaki kod örneğini çalıştırdığınızda, Azure sizden kimlik bilgileri ister. Kullanıcı adı ve parola için *azureuser* girin:
    
```azurepowershell-interactive
$appgw = Get-AzApplicationGateway -ResourceGroupName myResourceGroupAG -Name myAppGateway
$backendPool = Get-AzApplicationGatewayBackendAddressPool -Name myAGBackendPool -ApplicationGateway $appgw
$vnet   = Get-AzVirtualNetwork -ResourceGroupName myResourceGroupAG -Name myVNet
$subnet = Get-AzVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name myBackendSubnet
$cred = Get-Credential
for ($i=1; $i -le 2; $i++)
{
  $nic = New-AzNetworkInterface `
    -Name myNic$i `
    -ResourceGroupName myResourceGroupAG `
    -Location EastUS `
    -Subnet $subnet `
    -ApplicationGatewayBackendAddressPool $backendpool
  $vm = New-AzVMConfig `
    -VMName myVM$i `
    -VMSize Standard_DS2_v2
  Set-AzVMOperatingSystem `
    -VM $vm `
    -Windows `
    -ComputerName myVM$i `
    -Credential $cred
  Set-AzVMSourceImage `
    -VM $vm `
    -PublisherName MicrosoftWindowsServer `
    -Offer WindowsServer `
    -Skus 2016-Datacenter `
    -Version latest
  Add-AzVMNetworkInterface `
    -VM $vm `
    -Id $nic.Id
  Set-AzVMBootDiagnostic `
    -VM $vm `
    -Disable
  New-AzVM -ResourceGroupName myResourceGroupAG -Location EastUS -VM $vm
  Set-AzVMExtension `
    -ResourceGroupName myResourceGroupAG `
    -ExtensionName IIS `
    -VMName myVM$i `
    -Publisher Microsoft.Compute `
    -ExtensionType CustomScriptExtension `
    -TypeHandlerVersion 1.4 `
    -SettingString '{"commandToExecute":"powershell Add-WindowsFeature Web-Server; powershell Add-Content -Path \"C:\\inetpub\\wwwroot\\Default.htm\" -Value $($env:computername)"}' `
    -Location EastUS
}
```

## <a name="test-the-application-gateway"></a>Uygulama ağ geçidini test etme

Uygulama ağ geçidini oluşturmak için IIS gerekmese de, Azure'un uygulama ağ geçidini başarıyla oluşturup oluşturmadığını doğrulamak için bu hızlı başlangıçta bu uygulamayı yükledin. Uygulama ağ geçidini test etmek için IIS'yi kullanın:

1. Uygulama `Get-AzPublicIPAddress` ağ geçidinin genel IP adresini almak için çalıştırın. 
2. Genel IP adresini tarayıcınızın adres çubuğuna kopyalayıp yapıştırın. Tarayıcıyı yenilediğinizde, sanal makinenin adını görmeniz gerekir. Geçerli bir yanıt, uygulama ağ geçidinin başarıyla oluşturulduğunu ve arka uçla başarıyla bağlanabileceğini doğrular.

```azurepowershell-interactive
Get-AzPublicIPAddress -ResourceGroupName myResourceGroupAG -Name myAGPublicIPAddress
```

![Uygulama ağ geçidini test etme](./media/quick-create-powershell/application-gateway-iistest.png)


## <a name="clean-up-resources"></a>Kaynakları temizleme

Uygulama ağ geçidinde oluşturduğunuz kaynaklara artık ihtiyacınız olmadığında kaynak grubunu silin. Kaynak grubunu sildiğinizde, uygulama ağ geçidini ve ilgili tüm kaynaklarını da silersiniz. 

Kaynak grubunu silmek için `Remove-AzResourceGroup` cmdlet'i arayın:

```azurepowershell-interactive
Remove-AzResourceGroup -Name myResourceGroupAG
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure PowerShell kullanarak bir uygulama ağ geçidi ile web trafiğini yönetme](./tutorial-manage-web-traffic-powershell.md)

