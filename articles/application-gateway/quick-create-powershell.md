---
title: 'Hızlı başlangıç: PowerShell kullanarak doğrudan web trafiği'
titleSuffix: Azure Application Gateway
description: Bu hızlı başlangıçta, Web trafiğini bir arka uç havuzundaki sanal makinelere yönlendiren bir Azure Application Gateway oluşturmak için Azure PowerShell kullanmayı öğreneceksiniz.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: quickstart
ms.date: 08/27/2020
ms.author: victorh
ms.custom: mvc
ms.openlocfilehash: 115f01bffc4664798682923cb83a99a23a8ce274
ms.sourcegitcommit: 648c8d250106a5fca9076a46581f3105c23d7265
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2020
ms.locfileid: "88958346"
---
# <a name="quickstart-direct-web-traffic-with-azure-application-gateway-using-azure-powershell"></a>Hızlı başlangıç: Azure PowerShell kullanarak Azure Application Gateway ile doğrudan web trafiği

Bu hızlı başlangıçta, uygulama ağ geçidi oluşturmak için Azure PowerShell kullanırsınız. Ardından, doğru çalıştığından emin olmak için test edersiniz. 

Uygulama ağ geçidi, uygulama Web trafiğini bir arka uç havuzundaki belirli kaynaklara yönlendirir. Bağlantı noktalarına dinleyicileri atar, kurallar oluşturur ve bir arka uç havuzuna kaynak ekleyebilirsiniz. Kolaylık sağlaması için bu makalede, genel ön uç IP 'si olan basit bir kurulum ve uygulama ağ geçidinde tek bir siteyi barındırmak için temel bir dinleyici, temel bir istek yönlendirme kuralı ve arka uç havuzundaki iki sanal makine kullanılmaktadır.

Bu hızlı başlangıcı [Azure CLI](quick-create-cli.md) veya [Azure Portal](quick-create-portal.md)kullanarak da tamamlayabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

- Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- [Azure PowerShell Version 1.0.0 veya üzeri](/powershell/azure/install-az-ps) (Azure PowerShell yerel olarak çalıştırırsanız).

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="connect-to-azure"></a>Azure'a Bağlanma

Azure ile bağlantı kurmak için çalıştırın `Connect-AzAccount` .

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Azure 'da, ilgili kaynakları bir kaynak grubuna ayırabilirsiniz. Var olan bir kaynak grubunu kullanabilir ya da yeni bir tane oluşturabilirsiniz.

Yeni bir kaynak grubu oluşturmak için `New-AzResourceGroup` cmdlet 'ini kullanın: 

```azurepowershell-interactive
New-AzResourceGroup -Name myResourceGroupAG -Location eastus
```
## <a name="create-network-resources"></a>Ağ kaynakları oluşturma

Azure 'un, oluşturduğunuz kaynaklar arasında iletişim kurması için bir sanal ağa ihtiyacı vardır.  Application Gateway alt ağı yalnızca uygulama ağ geçitleri içerebilir. Başka hiçbir kaynağa izin verilmez.  Application Gateway için yeni bir alt ağ oluşturabilir veya var olan bir alt ağı kullanabilirsiniz. Bu örnekte, bu örnekte iki alt ağ oluşturursunuz: uygulama ağ geçidi için bir diğeri, arka uç sunucuları için bir diğeri. Application Gateway ön uç IP 'sini kullanım çalışmanıza göre genel veya özel olacak şekilde yapılandırabilirsiniz. Bu örnekte, genel ön uç IP 'si seçersiniz.

1. Kullanarak alt ağ yapılandırması oluşturun `New-AzVirtualNetworkSubnetConfig` .
2. Kullanarak alt ağ yapılandırmalarına sahip sanal ağ oluşturun `New-AzVirtualNetwork` . 
3. Kullanarak genel IP adresini oluşturun `New-AzPublicIpAddress` . 

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

1. `New-AzApplicationGatewayIPConfiguration`Oluşturduğunuz alt ağı uygulama ağ geçidiyle ilişkilendiren yapılandırmayı oluşturmak için kullanın. 
2. `New-AzApplicationGatewayFrontendIPConfig`Daha önce oluşturduğunuz genel IP adresini uygulama ağ geçidine atayan yapılandırmayı oluşturmak için kullanın. 
3. `New-AzApplicationGatewayFrontendPort`Application Gateway 'e erişmek için 80 numaralı bağlantı noktasını atamak üzere kullanın.

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

1. `New-AzApplicationGatewayBackendAddressPool`Uygulama ağ geçidi için arka uç havuzu oluşturmak için kullanın. Arka uç havuzu şu anda boş olacak. Sonraki bölümde arka uç sunucusu NIC 'Leri oluşturduğunuzda, bunları arka uç havuzuna eklersiniz.
2. Arka uç havuzunun ayarlarını ile yapılandırın `New-AzApplicationGatewayBackendHttpSetting` .

```azurepowershell-interactive
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

Azure, arka uç havuzuna uygun trafiği yönlendirmek için Application Gateway 'i etkinleştirmek üzere bir dinleyici gerektirir. Azure Ayrıca, bir dinleyicinin gelen trafik için hangi arka uç havuzunun kullanılacağını bilmesi için bir kural gerektirir. 

1. `New-AzApplicationGatewayHttpListener`Önceden oluşturduğunuz ön uç yapılandırması ve ön uç bağlantı noktası ile kullanarak bir dinleyici oluşturun. 
2. `New-AzApplicationGatewayRequestRoutingRule` *Rule1*adlı bir kural oluşturmak için kullanın. 

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

Gerekli destekleyici kaynakları oluşturduğunuza göre, uygulama ağ geçidini oluşturun:

1. `New-AzApplicationGatewaySku`Uygulama ağ geçidi için parametreleri belirtmek üzere kullanın.
2. `New-AzApplicationGateway`Uygulama ağ geçidi oluşturmak için kullanın.

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

Application Gateway oluşturdığınıza göre, Web sitelerini barındıracak arka uç sanal makinelerini oluşturun. Arka uç, NIC 'Ler, sanal makine ölçek kümeleri, genel IP 'Ler, iç IP 'Ler, tam etki alanı adları (FQDN) ve Azure App Service gibi çok kiracılı arka uçlar olabilir. Bu örnekte, Azure için uygulama ağ geçidi için arka uç sunucular olarak kullanılacak iki sanal makine oluşturursunuz. Ayrıca, Azure 'un uygulama ağ geçidini başarıyla oluşturduğunu doğrulamak için sanal makinelere IIS yüklersiniz.

#### <a name="create-two-virtual-machines"></a>İki sanal makine oluşturma

1. İle en son oluşturulan Application Gateway arka uç havuzu yapılandırmasını alın `Get-AzApplicationGatewayBackendAddressPool` .
2. İle bir ağ arabirimi oluşturun `New-AzNetworkInterface` .
3. İle bir sanal makine yapılandırması oluşturun `New-AzVMConfig` .
4. İle sanal makineyi oluşturun `New-AzVM` .

Sanal makineleri oluşturmak için aşağıdaki kod örneğini çalıştırdığınızda Azure sizden kimlik bilgilerini ister. Kullanıcı adı ve parola için *azureuser* girin:
    
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

Uygulama ağ geçidini oluşturmak için IIS gerekli olmasa da, Azure 'un uygulama ağ geçidini başarıyla oluşturup oluşturmadığını doğrulamak için bu hızlı başlangıçta yüklediniz. Uygulama ağ geçidini test etmek için IIS kullanın:

1. `Get-AzPublicIPAddress`Uygulama ağ geçidinin genel IP adresini almak için ' i çalıştırın. 
2. Genel IP adresini kopyalayıp tarayıcınızın adres çubuğuna yapıştırın. Tarayıcıyı yenilediğinizde, sanal makinenin adını görmeniz gerekir. Geçerli bir yanıt, uygulama ağ geçidinin başarıyla oluşturulduğunu ve arka uca başarıyla bağlanabildiğini doğrular.

```azurepowershell-interactive
Get-AzPublicIPAddress -ResourceGroupName myResourceGroupAG -Name myAGPublicIPAddress
```

![Uygulama ağ geçidini test etme](./media/quick-create-powershell/application-gateway-iistest.png)


## <a name="clean-up-resources"></a>Kaynakları temizleme

Uygulama ağ geçidiyle oluşturduğunuz kaynaklara artık ihtiyacınız kalmadığında, kaynak grubunu silin. Kaynak grubunu sildiğinizde, uygulama ağ geçidini ve ilgili tüm kaynakları da silersiniz. 

Kaynak grubunu silmek için `Remove-AzResourceGroup` cmdlet 'ini çağırın:

```azurepowershell-interactive
Remove-AzResourceGroup -Name myResourceGroupAG
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure PowerShell kullanarak bir uygulama ağ geçidi ile web trafiğini yönetme](./tutorial-manage-web-traffic-powershell.md)

