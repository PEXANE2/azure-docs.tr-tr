---
title: PowerShell'i kullanarak birden fazla web sitesi barındırın
titleSuffix: Azure Application Gateway
description: Azure PowerShell kullanarak, birden çok web sitesi barındıran bir uygulama ağ geçidi oluşturmayı öğrenin.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.date: 11/14/2019
ms.author: victorh
ms.custom: mvc
ms.openlocfilehash: e05d84e8e06dbe63a1bc8e8ae1d401f186baac77
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80133054"
---
# <a name="create-an-application-gateway-that-hosts-multiple-web-sites-using-azure-powershell"></a>Azure PowerShell kullanarak birden çok web sitesi barındıran bir uygulama ağ geçidi oluşturma

Bir [uygulama ağ geçidi](overview.md) oluştururken Azure PowerShell’i [birden çok web sitesi barındırmayı yapılandırmak](multiple-site-overview.md) için kullanabilirsiniz. Bu makalede, sanal makine ölçek kümelerini kullanarak arka uç adres havuzlarını tanımlarsınız. Ardından sahip olduğunuz dinleyicileri ve kuralları, web trafiğinin havuzlardaki uygun sunuculara ulaşması için yapılandırırsınız. Bu makalede, birden çok etki alanına sahip olduğunuzu varsayar ve *www.contoso.com* ve *www.fabrikam.com*örneklerini kullanır.

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Ağı ayarlama
> * Uygulama ağ geçidi oluşturma
> * Arka uç dinleyicileri oluşturma
> * Yönlendirme kuralları oluşturma
> * Arka uç havuzları ile sanal makine ölçek kümeleri oluşturma
> * Etki alanınızda bir CNAME kaydı oluşturma

![Çok siteli yönlendirme örneği](./media/tutorial-multiple-sites-powershell/scenario.png)

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

PowerShell'i yerel olarak yüklemeyi ve kullanmayı seçerseniz, bu makalede Azure PowerShell modülü sürümü 1.0.0 veya daha sonra gerekir. Sürümü bulmak için `Get-Module -ListAvailable Az` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-az-ps). PowerShell'i yerel olarak çalıştırıyorsanız, Azure `Login-AzAccount` ile bağlantı oluşturmak için de çalışmanız gerekir.

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. [Yeni-AzResourceGroup'u](/powershell/module/az.resources/new-azresourcegroup)kullanarak bir Azure kaynak grubu oluşturun.  

```azurepowershell-interactive
New-AzResourceGroup -Name myResourceGroupAG -Location eastus
```

## <a name="create-network-resources"></a>Ağ kaynakları oluşturma

[New-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/new-azvirtualnetworksubnetconfig)kullanarak alt ağ yapılandırmaları oluşturun. Alt ağ yapılandırmaları ile [New-AzVirtualNetwork](/powershell/module/az.network/new-azvirtualnetwork) kullanarak sanal ağ oluşturun. Ve son olarak, [New-AzPublicIpAddress](/powershell/module/az.network/new-azpublicipaddress)kullanarak genel IP adresi oluşturun. Bu kaynaklar, uygulama ağ geçidi ve ilişkili kaynakları ile ağ bağlantısı sağlamak için kullanılır.

```azurepowershell-interactive
$backendSubnetConfig = New-AzVirtualNetworkSubnetConfig `
  -Name myBackendSubnet `
  -AddressPrefix 10.0.1.0/24

$agSubnetConfig = New-AzVirtualNetworkSubnetConfig `
  -Name myAGSubnet `
  -AddressPrefix 10.0.2.0/24

$vnet = New-AzVirtualNetwork `
  -ResourceGroupName myResourceGroupAG `
  -Location eastus `
  -Name myVNet `
  -AddressPrefix 10.0.0.0/16 `
  -Subnet $backendSubnetConfig, $agSubnetConfig

$pip = New-AzPublicIpAddress `
  -ResourceGroupName myResourceGroupAG `
  -Location eastus `
  -Name myAGPublicIPAddress `
  -AllocationMethod Dynamic
```

## <a name="create-an-application-gateway"></a>Uygulama ağ geçidi oluşturma

### <a name="create-the-ip-configurations-and-frontend-port"></a>IP yapılandırmaları ve ön uç bağlantı noktası oluşturma

Daha önce oluşturduğunuz alt ağı [New-AzApplicationGatewayIPConfiguration](/powershell/module/az.network/new-azapplicationgatewayipconfiguration)kullanarak uygulama ağ geçidine ilişkilendirin. [Yeni-AzApplicationGatewayFrontendIPConfig](/powershell/module/az.network/new-azapplicationgatewayfrontendipconfig)kullanarak uygulama ağ geçidine ortak IP adresini atama.

```azurepowershell-interactive
$vnet = Get-AzVirtualNetwork `
  -ResourceGroupName myResourceGroupAG `
  -Name myVNet

$subnet=$vnet.Subnets[0]

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

### <a name="create-the-backend-pools-and-settings"></a>Arka uç havuzları ve ayarları oluşturma

[New-AzApplicationGatewayBackendAddressPool](/powershell/module/az.network/new-azapplicationgatewaybackendaddresspool)kullanarak uygulama ağ geçidi için ilk arka uç adres havuzunu oluşturun. [Yeni-AzApplicationGatewayBackendHttpSettings](/powershell/module/az.network/new-azapplicationgatewaybackendhttpsetting)kullanarak havuz ayarlarını yapılandırın.

```azurepowershell-interactive
$contosoPool = New-AzApplicationGatewayBackendAddressPool `
  -Name contosoPool

$fabrikamPool = New-AzApplicationGatewayBackendAddressPool `
  -Name fabrikamPool

$poolSettings = New-AzApplicationGatewayBackendHttpSettings `
  -Name myPoolSettings `
  -Port 80 `
  -Protocol Http `
  -CookieBasedAffinity Enabled `
  -RequestTimeout 120
```

### <a name="create-the-listeners-and-rules"></a>Dinleyiciler ve kurallar oluşturma

Uygulama ağ geçidinin trafiği arka uç havuzlarına uygun şekilde yönlendirmesi için dinleyiciler gereklidir. Bu makalede, iki etki alanınız için iki dinleyici oluşturursunuz. Dinleyiciler *contoso.com* ve *fabrikam.com* etki alanları için oluşturulur.

Daha önce oluşturduğunuz ön uç yapılandırması ve önuç bağlantı noktası yla [New-AzApplicationGatewayHttpListener'ı](/powershell/module/az.network/new-azapplicationgatewayhttplistener) kullanarak ilk dinleyiciyi oluşturun. Dinleyicinin gelen trafik için kullanacağı arka uç havuzunu bilmesi için bir kural gerekir. [New-AzApplicationGatewayRequestRoutingRule](/powershell/module/az.network/new-azapplicationgatewayrequestroutingrule)kullanarak *contosoRule* adlı temel bir kural oluşturun.

```azurepowershell-interactive
$contosolistener = New-AzApplicationGatewayHttpListener `
  -Name contosoListener `
  -Protocol Http `
  -FrontendIPConfiguration $fipconfig `
  -FrontendPort $frontendport `
  -HostName "www.contoso.com"

$fabrikamlistener = New-AzApplicationGatewayHttpListener `
  -Name fabrikamListener `
  -Protocol Http `
  -FrontendIPConfiguration $fipconfig `
  -FrontendPort $frontendport `
  -HostName "www.fabrikam.com"

$contosoRule = New-AzApplicationGatewayRequestRoutingRule `
  -Name contosoRule `
  -RuleType Basic `
  -HttpListener $contosoListener `
  -BackendAddressPool $contosoPool `
  -BackendHttpSettings $poolSettings

$fabrikamRule = New-AzApplicationGatewayRequestRoutingRule `
  -Name fabrikamRule `
  -RuleType Basic `
  -HttpListener $fabrikamListener `
  -BackendAddressPool $fabrikamPool `
  -BackendHttpSettings $poolSettings
```

### <a name="create-the-application-gateway"></a>Uygulama ağ geçidi oluşturma

Şimdi gerekli destekleyici kaynakları oluşturduğunuzda, [New-AzApplicationGatewaySku](/powershell/module/az.network/new-azapplicationgatewaysku)kullanarak uygulama ağ geçidi için parametreleri belirtin ve sonra [New-AzApplicationGateway](/powershell/module/az.network/new-azapplicationgateway)kullanarak oluşturun.

```azurepowershell-interactive
$sku = New-AzApplicationGatewaySku `
  -Name Standard_Medium `
  -Tier Standard `
  -Capacity 2

$appgw = New-AzApplicationGateway `
  -Name myAppGateway `
  -ResourceGroupName myResourceGroupAG `
  -Location eastus `
  -BackendAddressPools $contosoPool, $fabrikamPool `
  -BackendHttpSettingsCollection $poolSettings `
  -FrontendIpConfigurations $fipconfig `
  -GatewayIpConfigurations $gipconfig `
  -FrontendPorts $frontendport `
  -HttpListeners $contosoListener, $fabrikamListener `
  -RequestRoutingRules $contosoRule, $fabrikamRule `
  -Sku $sku
```

## <a name="create-virtual-machine-scale-sets"></a>Sanal makine ölçek kümesi oluşturma

Bu örnekte, oluşturduğunuz iki arka uç havuzunu destekleyen iki sanal makine ölçek kümesi oluşturacaksınız. Oluşturduğunuz ölçek kümeleri *myvmss1* ve *myvmss2* olarak adlandırılır. Her bir ölçek kümesi IIS yükleyeceğiniz iki sanal makine örneği içerir. IP ayarlarını yapılandırırken ölçek kümesini arka uç havuzuna atayın.

```azurepowershell-interactive
$vnet = Get-AzVirtualNetwork `
  -ResourceGroupName myResourceGroupAG `
  -Name myVNet

$appgw = Get-AzApplicationGateway `
  -ResourceGroupName myResourceGroupAG `
  -Name myAppGateway

$contosoPool = Get-AzApplicationGatewayBackendAddressPool `
  -Name contosoPool `
  -ApplicationGateway $appgw

$fabrikamPool = Get-AzApplicationGatewayBackendAddressPool `
  -Name fabrikamPool `
  -ApplicationGateway $appgw

for ($i=1; $i -le 2; $i++)
{
  if ($i -eq 1) 
  {
    $poolId = $contosoPool.Id
  }
  if ($i -eq 2)
  {
    $poolId = $fabrikamPool.Id
  }

  $ipConfig = New-AzVmssIpConfig `
    -Name myVmssIPConfig$i `
    -SubnetId $vnet.Subnets[1].Id `
    -ApplicationGatewayBackendAddressPoolsId $poolId

  $vmssConfig = New-AzVmssConfig `
    -Location eastus `
    -SkuCapacity 2 `
    -SkuName Standard_DS2 `
    -UpgradePolicyMode Automatic

  Set-AzVmssStorageProfile $vmssConfig `
    -ImageReferencePublisher MicrosoftWindowsServer `
    -ImageReferenceOffer WindowsServer `
    -ImageReferenceSku 2016-Datacenter `
    -ImageReferenceVersion latest `
    -OsDiskCreateOption FromImage

  Set-AzVmssOsProfile $vmssConfig `
    -AdminUsername azureuser `
    -AdminPassword "Azure123456!" `
    -ComputerNamePrefix myvmss$i

  Add-AzVmssNetworkInterfaceConfiguration `
    -VirtualMachineScaleSet $vmssConfig `
    -Name myVmssNetConfig$i `
    -Primary $true `
    -IPConfiguration $ipConfig

  New-AzVmss `
    -ResourceGroupName myResourceGroupAG `
    -Name myvmss$i `
    -VirtualMachineScaleSet $vmssConfig
}
```

### <a name="install-iis"></a>IIS yükleme

```azurepowershell-interactive
$publicSettings = @{ "fileUris" = (,"https://raw.githubusercontent.com/Azure/azure-docs-powershell-samples/master/application-gateway/iis/appgatewayurl.ps1"); 
  "commandToExecute" = "powershell -ExecutionPolicy Unrestricted -File appgatewayurl.ps1" }

for ($i=1; $i -le 2; $i++)
{
  $vmss = Get-AzVmss `
    -ResourceGroupName myResourceGroupAG `
    -VMScaleSetName myvmss$i

  Add-AzVmssExtension -VirtualMachineScaleSet $vmss `
    -Name "customScript" `
    -Publisher "Microsoft.Compute" `
    -Type "CustomScriptExtension" `
    -TypeHandlerVersion 1.8 `
    -Setting $publicSettings

  Update-AzVmss `
    -ResourceGroupName myResourceGroupAG `
    -Name myvmss$i `
    -VirtualMachineScaleSet $vmss
}
```

## <a name="create-cname-record-in-your-domain"></a>Etki alanınızda CNAME kaydı oluşturma

Uygulama ağ geçidi genel IP adresiyle oluşturulduktan sonra DNS adresini alabilir ve etki alanınızda bir CNAME kaydı oluşturmak için kullanabilirsiniz. Uygulama ağ geçidinin DNS adresini almak için [Get-AzPublicIPAddress'i](/powershell/module/az.network/get-azpublicipaddress) kullanabilirsiniz. DNSSetting için *fqdn* değerini kopyalayın ve bu değeri oluşturduğunuz CNAME kaydının değeri olarak kullanın. V1 SKU'da uygulama ağ geçidi yeniden başlatıldığında VIP değişebileceğinden A kayıtlarının kullanılması önerilmez.

```azurepowershell-interactive
Get-AzPublicIPAddress -ResourceGroupName myResourceGroupAG -Name myAGPublicIPAddress
```

## <a name="test-the-application-gateway"></a>Uygulama ağ geçidini test etme

Tarayıcınızın adres çubuğuna, etki alanı adınızı girin. Mesela, http:\//www.contoso.com.

![Uygulama ağ geçidinde contoso test etme](./media/tutorial-multiple-sites-powershell/application-gateway-iistest.png)

Adresi diğer etki alanınızla değiştirin, aşağıdaki örneğe benzer bir şey görmeniz gerekir:

![Uygulama ağ geçidinde fabrikam sitesini test etme](./media/tutorial-multiple-sites-powershell/application-gateway-iistest2.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekmediğinde, Kaynak grubunu, uygulama ağ geçidini ve ilgili tüm kaynakları [Kaldır-AzResourceGroup'u](/powershell/module/az.resources/remove-azresourcegroup)kullanarak kaldırın.

```azurepowershell-interactive
Remove-AzResourceGroup -Name myResourceGroupAG
```

## <a name="next-steps"></a>Sonraki adımlar

[URL yolu tabanlı yönlendirme kuralları ile bir uygulama ağ geçidi oluşturma](./tutorial-url-route-powershell.md)
