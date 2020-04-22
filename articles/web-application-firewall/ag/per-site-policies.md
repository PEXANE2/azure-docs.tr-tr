---
title: PowerShell kullanarak site başına WAF ilkelerini yapılandırın
titleSuffix: Azure Web Application Firewall
description: Azure PowerShell'i kullanarak bir uygulama ağ geçidinde site başına Web Uygulaması Güvenlik Duvarı ilkelerini nasıl yapılandırıştırmayı öğrenin.
services: web-application-firewall
author: winthrop28
ms.service: web-application-firewall
ms.date: 01/24/2020
ms.author: victorh
ms.topic: conceptual
ms.openlocfilehash: 1301db56cab36ae623bb94cfac97b8e4bdb934e5
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81682491"
---
# <a name="configure-per-site-waf-policies-using-azure-powershell"></a>Azure PowerShell'i kullanarak site başına WAF ilkelerini yapılandırın

Web Uygulama Güvenlik Duvarı (WAF) ayarları WAF ilkelerinde bulunur ve WAF yapılandırmanızı değiştirmek için WAF ilkesini değiştirirsiniz.

Uygulama Ağ Geçidinizle ilişkilendirildiğinde, ilkeler ve tüm ayarlar genel olarak yansıtılır. Yani, WAF arkasında beş site varsa, tüm beş site aynı WAF Politikası tarafından korunmaktadır. Her site için aynı güvenlik ayarlarına ihtiyacınız varsa bu harika. Ancak, siteye özgü WAF yapılandırmasına izin vermek için tek tek dinleyicilere WAF ilkeleri de uygulayabilirsiniz.

Bir dinleyiciye WAF ilkeleri uygulayarak, her siteyi etkileyen değişiklikler olmadan tek tek siteler için WAF ayarlarını yapılandırabilirsiniz. En özel politika emsal teşkil eder. Genel bir ilke ve site başına bir ilke (dinleyiciyle ilişkili bir WAF ilkesi) varsa, site başına politika, o dinleyici için genel WAF ilkesini geçersiz kılar. Kendi politikaları olmayan diğer dinleyiciler sadece küresel WAF politikasından etkilenecektir.

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Ağı ayarlama
> * WAF ilkesi oluşturma
> * WAF etkinken bir uygulama ağ geçidi oluşturma
> * WAF ilkesini genel olarak, site başına ve URI başına uygulayın
> * Sanal makine ölçek kümesi oluşturma
> * Bir depolama hesabı oluşturma ve tanılamaları yapılandırma
> * Uygulama ağ geçidini test etme

![Web uygulaması güvenlik duvarı örneği](../media/tutorial-restrict-web-traffic-powershell/scenario-waf.png)

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

PowerShell'i yerel olarak yüklemeyi ve kullanmayı seçerseniz, bu makalede Azure PowerShell modülü sürümü 1.0.0 veya daha sonra gerekir. Sürümü bulmak için `Get-Module -ListAvailable Az` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-az-ps). PowerShell'i yerel olarak çalıştırıyorsanız, Azure `Login-AzAccount` ile bağlantı oluşturmak için de çalışmanız gerekir.

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. [Yeni-AzResourceGroup'u](/powershell/module/az.resources/new-azresourcegroup)kullanarak bir Azure kaynak grubu oluşturun.  

```azurepowershell-interactive
$rgname = New-AzResourceGroup -Name myResourceGroupAG -Location eastus
```

## <a name="create-network-resources"></a>Ağ kaynakları oluşturma 

[New-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/new-azvirtualnetworksubnetconfig)kullanarak *myBackendSubnet* ve *myAGSubnet* adlı alt net yapılandırmaları oluşturun. Subnet yapılandırmaları ile [New-AzVirtualNetwork](/powershell/module/az.network/new-azvirtualnetwork) kullanarak *myVNet* adlı sanal ağı oluşturun. Ve son olarak, [Yeni-AzPublicIpAddress](/powershell/module/az.network/new-azpublicipaddress)kullanarak *myAGPublicIPAddress* adlı genel IP adresi oluşturun. Bu kaynaklar, uygulama ağ geçidi ve ilişkili kaynakları ile ağ bağlantısı sağlamak için kullanılır.

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
  -AllocationMethod Static `
  -Sku Standard
```

## <a name="create-an-application-gateway"></a>Uygulama ağ geçidi oluşturma

Bu bölümde, uygulama ağ geçidini destekleyen kaynaklar oluşturursunuz ve sonunda onu ve WAF'ı oluşturursunuz. Şu kaynakları oluşturacaksınız:

- *IP yapılandırmaları ve ön uç bağlantı noktası* - Daha önce oluşturduğunuz alt ağı uygulama ağ geçidi ile ilişkilendirir ve ona erişmek için kullanılacak bir bağlantı noktası atar.
- *Varsayılan havuz* - Tüm uygulama ağ geçitlerinde en az bir sunucu arka uç havuzu olmalıdır.
- *Varsayılan dinleyici ve kural* - Varsayılan dinleyici, atanan bağlantı noktası üzerindeki trafiği dinler, varsayılan kural ise trafiği varsayılan havuza gönderir.

### <a name="create-the-ip-configurations-and-frontend-port"></a>IP yapılandırmaları ve ön uç bağlantı noktası oluşturma

Daha önce oluşturduğunuz *myAGSubnet'i* [New-AzApplicationGatewayIPConfiguration](/powershell/module/az.network/new-azapplicationgatewayipconfiguration)kullanarak uygulama ağ geçidine ilişkilendirin. *MyAGPublicIPAddress'i* [New-AzApplicationGatewayFrontendIPConfig](/powershell/module/az.network/new-azapplicationgatewayfrontendipconfig)kullanarak uygulama ağ geçidine atayın.

```azurepowershell-interactive
$vnet = Get-AzVirtualNetwork `
  -ResourceGroupName myResourceGroupAG `
  -Name myVNet

$subnet=$vnet.Subnets[1]

$gipconfig = New-AzApplicationGatewayIPConfiguration `
  -Name myAGIPConfig `
  -Subnet $subnet

$fipconfig = New-AzApplicationGatewayFrontendIPConfig `
  -Name myAGFrontendIPConfig `
  -PublicIPAddress $pip

$frontendport80 = New-AzApplicationGatewayFrontendPort `
  -Name myFrontendPort `
  -Port 80
  
$frontendport8080 = New-AzApplicationGatewayFrontendPort `
  -Name myFrontendPort `
  -Port 8080
```

### <a name="create-the-backend-pool-and-settings"></a>Arka uç havuzu ve ayarları oluşturma

[New-AzApplicationGatewayBackendAddressPool](/powershell/module/az.network/new-azapplicationgatewaybackendaddresspool)kullanarak uygulama ağ geçidi için *appGatewayBackendPool* adlı arka uç havuzu oluşturun. [New-AzApplicationGatewayBackendHttpSettings](/powershell/module/az.network/new-azapplicationgatewaybackendhttpsetting)kullanarak arka uç adres havuzları için ayarları yapılandırın.

```azurepowershell-interactive
$defaultPool = New-AzApplicationGatewayBackendAddressPool `
  -Name appGatewayBackendPool 

$poolSettings = New-AzApplicationGatewayBackendHttpSettings `
  -Name myPoolSettings `
  -Port 80 `
  -Protocol Http `
  -CookieBasedAffinity Enabled `
  -RequestTimeout 120
```

### <a name="create-two-waf-policies"></a>İki WAF ilkeleri oluşturma

Biri genel ve bir site başına olmak üzere iki WAF ilkesi oluşturun ve özel kurallar ekleyin. 

Site başına ilke, dosya yükleme sınırını 5 MB ile sınırlar. Diğer her şey aynı.

```azurepowershell-interactive
$variable = New-AzApplicationGatewayFirewallMatchVariable -VariableName RequestUri
$condition = New-AzApplicationGatewayFirewallCondition -MatchVariable $variable -Operator Contains -MatchValue "globalAllow" 
$rule = New-AzApplicationGatewayFirewallCustomRule -Name globalAllow -Priority 5 -RuleType MatchRule -MatchCondition $condition -Action Allow

$variable1 = New-AzApplicationGatewayFirewallMatchVariable -VariableName RequestUri
$condition1 = New-AzApplicationGatewayFirewallCondition -MatchVariable $variable1 -Operator Contains -MatchValue "globalBlock" 
$rule1 = New-AzApplicationGatewayFirewallCustomRule -Name globalAllow -Priority 10 -RuleType MatchRule -MatchCondition $condition1 -Action Block

$variable2 = New-AzApplicationGatewayFirewallMatchVariable -VariableName RequestUri
$condition2 = New-AzApplicationGatewayFirewallCondition -MatchVariable $variable2 -Operator Contains -MatchValue "siteAllow" 
$rule2 = New-AzApplicationGatewayFirewallCustomRule -Name globalAllow -Priority 5 -RuleType MatchRule -MatchCondition $condition2 -Action Allow

$variable3 = New-AzApplicationGatewayFirewallMatchVariable -VariableName RequestUri
$condition3 = New-AzApplicationGatewayFirewallCondition -MatchVariable $variable3 -Operator Contains -MatchValue "siteBlock" 
$rule3 = New-AzApplicationGatewayFirewallCustomRule -Name globalAllow -Priority 10 -RuleType MatchRule -MatchCondition $condition3 -Action Block

$variable4 = New-AzApplicationGatewayFirewallMatchVariable -VariableName RequestUri
$condition4 = New-AzApplicationGatewayFirewallCondition -MatchVariable $variable4 -Operator Contains -MatchValue "URIAllow" 
$rule4 = New-AzApplicationGatewayFirewallCustomRule -Name globalAllow -Priority 5 -RuleType MatchRule -MatchCondition $condition4 -Action Allow

$variable5 = New-AzApplicationGatewayFirewallMatchVariable -VariableName RequestUri
$condition5 = New-AzApplicationGatewayFirewallCondition -MatchVariable $variable5 -Operator Contains -MatchValue "URIBlock" 
$rule5 = New-AzApplicationGatewayFirewallCustomRule -Name globalAllow -Priority 10 -RuleType MatchRule -MatchCondition $condition5 -Action Block

$policySettingGlobal = New-AzApplicationGatewayFirewallPolicySetting `
  -Mode Prevention `
  -State Enabled `
  -MaxRequestBodySizeInKb 100 `
  -MaxFileUploadInMb 256

$wafPolicyGlobal = New-AzApplicationGatewayFirewallPolicy `
  -Name wafpolicyGlobal `
  -ResourceGroup myResourceGroupAG `
  -Location eastus `
  -PolicySetting $PolicySettingGlobal `
  -CustomRule $rule, $rule1

$policySettingSite = New-AzApplicationGatewayFirewallPolicySetting `
  -Mode Prevention `
  -State Enabled `
  -MaxRequestBodySizeInKb 100 `
  -MaxFileUploadInMb 5

$wafPolicySite = New-AzApplicationGatewayFirewallPolicy `
  -Name wafpolicySite `
  -ResourceGroup myResourceGroupAG `
  -Location eastus `
  -PolicySetting $PolicySettingSite `
  -CustomRule $rule2, $rule3
```

### <a name="create-the-default-listener-and-rule"></a>Varsayılan dinleyici ve kural oluşturma

Uygulama ağ geçidinin trafiği arka uç havuzlarına uygun şekilde yönlendirmesi için bir dinleyici gereklidir. Bu örnekte, kök URL’deki trafiği dinleyen temel bir dinleyici oluşturacaksınız. 

Daha önce oluşturduğunuz ön uç yapılandırması ve önuç bağlantı noktası ile [New-AzApplicationGatewayHttpListener](/powershell/module/az.network/new-azapplicationgatewayhttplistener) kullanarak *mydefaultListener* adlı bir dinleyici oluşturun. Dinleyicinin gelen trafik için kullanacağı arka uç havuzunu bilmesi için bir kural gerekir. [New-AzApplicationGatewayRequestRoutingRule](/powershell/module/az.network/new-azapplicationgatewayrequestroutingrule)kullanarak *kural1* adlı temel bir kural oluşturun.

```azurepowershell-interactive
$globalListener = New-AzApplicationGatewayHttpListener `
  -Name mydefaultListener `
  -Protocol Http `
  -FrontendIPConfiguration $fipconfig `
  -FrontendPort $frontendport80

$frontendRule = New-AzApplicationGatewayRequestRoutingRule `
  -Name rule1 `
  -RuleType Basic `
  -HttpListener $globallistener `
  -BackendAddressPool $defaultPool `
  -BackendHttpSettings $poolSettings
  
$siteListener = New-AzApplicationGatewayHttpListener `
  -Name mydefaultListener `
  -Protocol Http `
  -FrontendIPConfiguration $fipconfig `
  -FrontendPort $frontendport8080 `
  -FirewallPolicy $wafPolicySite
  
$frontendRuleSite = New-AzApplicationGatewayRequestRoutingRule `
  -Name rule2 `
  -RuleType Basic `
  -HttpListener $siteListener `
  -BackendAddressPool $defaultPool `
  -BackendHttpSettings $poolSettings
```

### <a name="create-the-application-gateway-with-the-waf"></a>WAF ile uygulama ağ geçidi oluşturma

Şimdi gerekli destekleyici kaynakları oluşturduğunuzda, [Yeni-AzApplicationGatewaySku](/powershell/module/az.network/new-azapplicationgatewaysku)kullanarak uygulama ağ geçidi için parametreleri belirtin. [Yeni-AzApplicationGatewayFirewallPolicy](/powershell/module/az.network/new-azapplicationgatewayfirewallpolicy)kullanarak Güvenlik Duvarı İlkesi belirtin. Ve sonra [New-AzApplicationGateway](/powershell/module/az.network/new-azapplicationgateway)kullanarak *myAppGateway* adlı uygulama ağ geçidi oluşturun.

```azurepowershell-interactive
$sku = New-AzApplicationGatewaySku `
  -Name WAF_v2 `
  -Tier WAF_v2 `
  -Capacity 2

$appgw = New-AzApplicationGateway `
  -Name myAppGateway `
  -ResourceGroupName myResourceGroupAG `
  -Location eastus `
  -BackendAddressPools $defaultPool `
  -BackendHttpSettingsCollection $poolSettings `
  -FrontendIpConfigurations $fipconfig `
  -GatewayIpConfigurations $gipconfig `
  -FrontendPorts $frontendport80 `
  -HttpListeners $globallistener `
  -RequestRoutingRules $frontendRule `
  -Sku $sku `
  -FirewallPolicy $wafPolicyGlobal
```

### <a name="apply-a-per-uri-policy"></a>URI başına bir ilke uygulayın

URI başına bir ilke uygulamak için yeni bir ilke oluşturmanız ve yol kuralı config'ine uygulamanız yeterlidir. 

```azurepowershell-interactive
$policySettingURI = New-AzApplicationGatewayFirewallPolicySetting `
  -Mode Prevention `
  -State Enabled `
  -MaxRequestBodySizeInKb 100 `
  -MaxFileUploadInMb 5

$wafPolicyURI = New-AzApplicationGatewayFirewallPolicy `
  -Name wafpolicySite `
  -ResourceGroup myResourceGroupAG `
  -Location eastus `
  -PolicySetting $PolicySettingURI `
  -CustomRule $rule4, $rule5

$Gateway = Get-AzApplicationGateway -Name "myAppGateway"

$PathRuleConfig = New-AzApplicationGatewayPathRuleConfig -Name "base" `
  -Paths "/base" `
  -BackendAddressPool $defaultPool `
  -BackendHttpSettings $poolSettings `
  -FirewallPolicy $wafPolicyURI

$PathRuleConfig1 = New-AzApplicationGatewayPathRuleConfig `
  -Name "base" -Paths "/test" `
  -BackendAddressPool $defaultPool `
  -BackendHttpSettings $poolSettings

$URLPathMap = New-AzApplicationGatewayUrlPathMapConfig -Name "PathMap" `
  -PathRules $PathRuleConfig, $PathRuleConfig1 `
  -DefaultBackendAddressPoolId $defaultPool.Id `
  -DefaultBackendHttpSettingsId $poolSettings.Id

Add-AzApplicationGatewayRequestRoutingRule -ApplicationGateway $AppGw `
  -Name "RequestRoutingRule" `
  -RuleType PathBasedRouting `
  -HttpListener $siteListener `
  -UrlPathMapId $URLPathMap.Id
```

## <a name="create-a-virtual-machine-scale-set"></a>Sanal makine ölçek kümesi oluşturma

Bu örnekte uygulama ağ geçidinde arka uç havuzu için sunucu sağlayan bir sanal makine ölçek kümesi oluşturacaksınız. IP ayarlarını yapılandırırken ölçek kümesini arka uç havuzuna atayın.

```azurepowershell-interactive
$vnet = Get-AzVirtualNetwork `
  -ResourceGroupName myResourceGroupAG `
  -Name myVNet

$appgw = Get-AzApplicationGateway `
  -ResourceGroupName myResourceGroupAG `
  -Name myAppGateway

$backendPool = Get-AzApplicationGatewayBackendAddressPool `
  -Name defaultPool `
  -ApplicationGateway $appgw

$ipConfig = New-AzVmssIpConfig `
  -Name myVmssIPConfig `
  -SubnetId $vnet.Subnets[1].Id `
  -ApplicationGatewayBackendAddressPoolsId $backendPool.Id

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
  -ComputerNamePrefix myvmss

Add-AzVmssNetworkInterfaceConfiguration `
  -VirtualMachineScaleSet $vmssConfig `
  -Name myVmssNetConfig `
  -Primary $true `
  -IPConfiguration $ipConfig

New-AzVmss `
  -ResourceGroupName myResourceGroupAG `
  -Name myvmss `
  -VirtualMachineScaleSet $vmssConfig
```

### <a name="install-iis"></a>IIS yükleme

```azurepowershell-interactive
$publicSettings = @{ "fileUris" = (,"https://raw.githubusercontent.com/Azure/azure-docs-powershell-samples/master/application-gateway/iis/appgatewayurl.ps1"); 
  "commandToExecute" = "powershell -ExecutionPolicy Unrestricted -File appgatewayurl.ps1" }

$vmss = Get-AzVmss -ResourceGroupName myResourceGroupAG -VMScaleSetName myvmss

Add-AzVmssExtension -VirtualMachineScaleSet $vmss `
  -Name "customScript" `
  -Publisher "Microsoft.Compute" `
  -Type "CustomScriptExtension" `
  -TypeHandlerVersion 1.8 `
  -Setting $publicSettings

Update-AzVmss `
  -ResourceGroupName myResourceGroupAG `
  -Name myvmss `
  -VirtualMachineScaleSet $vmss
```

## <a name="create-a-storage-account-and-configure-diagnostics"></a>Bir depolama hesabı oluşturma ve tanılamaları yapılandırma

Bu makalede, uygulama ağ geçidi algılama ve önleme amacıyla verileri depolamak için bir depolama hesabı kullanır. Verileri kaydetmek için Azure Monitor günlüklerini veya Etkinlik Hub'Larını da kullanabilirsiniz.

### <a name="create-the-storage-account"></a>Depolama hesabı oluşturma

[Yeni-AzStorageAccount](/powershell/module/az.storage/new-azstorageaccount)kullanarak *myagstore1* adlı bir depolama hesabı oluşturun.

```azurepowershell-interactive
$storageAccount = New-AzStorageAccount `
  -ResourceGroupName myResourceGroupAG `
  -Name myagstore1 `
  -Location eastus `
  -SkuName "Standard_LRS"
```

### <a name="configure-diagnostics"></a>Tanılama yapılandırma

[Set-AzDiagnosticSetting](/powershell/module/az.monitor/set-azdiagnosticsetting)kullanarak ApplicationGatewayAccessLog, ApplicationGatewayPerformanceLog ve ApplicationGatewayFirewallLog günlüklerine veri kaydetmek için tanılamayı yapılandırın.

```azurepowershell-interactive
$appgw = Get-AzApplicationGateway `
  -ResourceGroupName myResourceGroupAG `
  -Name myAppGateway

$store = Get-AzStorageAccount `
  -ResourceGroupName myResourceGroupAG `
  -Name myagstore1

Set-AzDiagnosticSetting `
  -ResourceId $appgw.Id `
  -StorageAccountId $store.Id `
  -Categories ApplicationGatewayAccessLog, ApplicationGatewayPerformanceLog, ApplicationGatewayFirewallLog `
  -Enabled $true `
  -RetentionEnabled $true `
  -RetentionInDays 30
```

## <a name="test-the-application-gateway"></a>Uygulama ağ geçidini test etme

Uygulama ağ geçidinin genel IP adresini almak için [Get-AzPublicIPAddress'i](/powershell/module/az.network/get-azpublicipaddress) kullanabilirsiniz. Daha sonra bu IP adresini kıvrık (aşağıda gösterilen 1.1.1.1'i değiştirin). 

```azurepowershell-interactive
Get-AzPublicIPAddress -ResourceGroupName myResourceGroupAG -Name myAGPublicIPAddress

#should be blocked
curl 1.1.1.1/globalBlock
curl 1.1.1.1/?1=1

#should be allowed
curl 1.1.1.1/globalAllow?1=1

#should be blocked
curl 1.1.1.1:8080/siteBlock
curl 1.1.1.1/?1=1

#should be allowed
curl 1.1.1.1:8080/siteAllow?1=1

#should be blocked
curl 1.1.1.1/URIBlock
curl 1.1.1.1/?1=1

#should be allowed
curl 1.1.1.1/URIAllow?1=1
```

![Temel URL’yi uygulama ağ geçidinde test etme](../media/tutorial-restrict-web-traffic-powershell/application-gateway-iistest.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekmediğinde, Kaynak grubunu, uygulama ağ geçidini ve ilgili tüm kaynakları [Kaldır-AzResourceGroup'u](/powershell/module/az.resources/remove-azresourcegroup)kullanarak kaldırın.

```azurepowershell-interactive
Remove-AzResourceGroup -Name myResourceGroupAG
```

## <a name="next-steps"></a>Sonraki adımlar

[Web uygulaması güvenlik duvarı kurallarını özelleştirme](application-gateway-customize-waf-rules-portal.md)
