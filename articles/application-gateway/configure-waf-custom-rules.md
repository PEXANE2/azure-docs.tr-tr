---
title: Azure PowerShell kullanarak Web uygulaması güvenlik duvarı v2 özel kurallarını yapılandırma
description: Azure PowerShell kullanarak Web uygulaması güvenlik duvarı v2 özel kurallarını yapılandırma hakkında bilgi edinin
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.date: 6/18/2019
ms.author: victorh
ms.openlocfilehash: 9e50b47e22f5760c213cd0cafad82ecca592dec8
ms.sourcegitcommit: b4665f444dcafccd74415fb6cc3d3b65746a1a31
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2019
ms.locfileid: "72263736"
---
# <a name="configure-web-application-firewall-v2-custom-rules-by-using-azure-powershell"></a>Azure PowerShell kullanarak Web uygulaması güvenlik duvarı v2 özel kurallarını yapılandırma

<!--- If you make any changes to the PowerShell in this article, also make the change in the corresponding Sample file: azure-docs-powershell-samples/application-gateway/waf-rules/waf-custom-rules.ps1 --->

Özel kurallarla, Web uygulaması güvenlik duvarı (WAF) üzerinden geçen her istek için değerlendirilen kendi kurallarınızı oluşturabilirsiniz. Bu kurallar, yönetilen kural kümelerindeki kuralların geri kalanından daha yüksek bir öncelik tutar. Tam özelleştirmeye izin vermek için özel kurallarında bir eylem (izin vermek veya engellemek için), bir eşleşme koşulu ve bir işleci vardır.

Bu makalede, özel bir kural kullanan bir Azure Application Gateway WAF v2 oluşturulur. İstek üst bilgisi Kullanıcı Aracısı *evbot*içeriyorsa, özel kural trafiği engeller.

Daha fazla özel kural örneği görüntülemek için bkz. [özel Web uygulaması güvenlik duvarı kuralları oluşturma ve kullanma](create-custom-waf-rules.md).

Bu makaledeki Azure PowerShell kodunu kopyalayabilir, yapıştırabileceğiniz ve çalıştırabileceğiniz tek bir sürekli betik içinde çalıştırmak için bkz. [Azure Application Gateway PowerShell örnekleri](powershell-samples.md).

## <a name="prerequisites"></a>Önkoşullar
* [!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

* Bir Azure PowerShell modüle ihtiyacınız vardır. Azure PowerShell yerel olarak yükleyip kullanmayı tercih ederseniz, bu betik Azure PowerShell modül sürümü 2.1.0 veya üzerini gerektirir. Şunları yapın:

  1. Sürümü bulmak için `Get-Module -ListAvailable Az` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-az-ps).
  2. Azure ile bağlantı oluşturmak için `Connect-AzAccount` ' ı çalıştırın.

## <a name="example-script"></a>Örnek betik

### <a name="set-up-variables"></a>Değişkenleri ayarlama

Aşağıdaki kodu çalıştırın:

```azurepowershell
$rgname = "CustomRulesTest"

$location = "East US"

$appgwName = "WAFCustomRules"
```

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Aşağıdaki kodu çalıştırın:

```azurepowershell
$resourceGroup = New-AzResourceGroup -Name $rgname -Location $location
```

### <a name="create-a-virtual-network"></a>Sanal ağ oluşturun

Aşağıdaki kodu çalıştırın:

```azurepowershell
$sub1 = New-AzVirtualNetworkSubnetConfig -Name "appgwSubnet" -AddressPrefix "10.0.0.0/24"

$sub2 = New-AzVirtualNetworkSubnetConfig -Name "backendSubnet" -AddressPrefix "10.0.1.0/24"

$vnet = New-AzvirtualNetwork -Name "Vnet1" -ResourceGroupName $rgname -Location $location `
  -AddressPrefix "10.0.0.0/16" -Subnet @($sub1, $sub2)
```

### <a name="create-a-static-public-vip"></a>Statik ortak VIP oluşturma

Aşağıdaki kodu çalıştırın:

```azurepowershell
$publicip = New-AzPublicIpAddress -ResourceGroupName $rgname -name "AppGwIP" `
  -location $location -AllocationMethod Static -Sku Standard
```

### <a name="create-a-pool-and-front-end-port"></a>Havuz ve ön uç bağlantı noktası oluşturma

Aşağıdaki kodu çalıştırın:

```azurepowershell
$gwSubnet = Get-AzVirtualNetworkSubnetConfig -Name "appgwSubnet" -VirtualNetwork $vnet

$gipconfig = New-AzApplicationGatewayIPConfiguration -Name "AppGwIpConfig" -Subnet $gwSubnet

$fipconfig01 = New-AzApplicationGatewayFrontendIPConfig -Name "fipconfig" -PublicIPAddress $publicip

$pool = New-AzApplicationGatewayBackendAddressPool -Name "pool1" `
  -BackendIPAddresses testbackend1.westus.cloudapp.azure.com, testbackend2.westus.cloudapp.azure.com

$fp01 = New-AzApplicationGatewayFrontendPort -Name "port1" -Port 80
```

### <a name="create-a-listener-http-setting-rule-and-autoscale"></a>Dinleyici, HTTP ayarı, kural ve otomatik ölçeklendirme oluşturma

Aşağıdaki kodu çalıştırın:

```azurepowershell
$listener01 = New-AzApplicationGatewayHttpListener -Name "listener1" -Protocol Http `
  -FrontendIPConfiguration $fipconfig01 -FrontendPort $fp01

$poolSetting01 = New-AzApplicationGatewayBackendHttpSettings -Name "setting1" -Port 80 `
  -Protocol Http -CookieBasedAffinity Disabled

$rule01 = New-AzApplicationGatewayRequestRoutingRule -Name "rule1" -RuleType basic `
  -BackendHttpSettings $poolSetting01 -HttpListener $listener01 -BackendAddressPool $pool

$autoscaleConfig = New-AzApplicationGatewayAutoscaleConfiguration -MinCapacity 3

$sku = New-AzApplicationGatewaySku -Name WAF_v2 -Tier WAF_v2
```

### <a name="create-the-custom-rule-and-apply-it-to-waf-policy"></a>Özel kuralı oluşturma ve WAF ilkesine uygulama

Aşağıdaki kodu çalıştırın:

```azurepowershell
$variable = New-AzApplicationGatewayFirewallMatchVariable -VariableName RequestHeaders -Selector User-Agent

$condition = New-AzApplicationGatewayFirewallCondition -MatchVariable $variable -Operator Contains -MatchValue "evilbot" -Transform Lowercase -NegationCondition $False  

$rule = New-AzApplicationGatewayFirewallCustomRule -Name blockEvilBot -Priority 2 -RuleType MatchRule -MatchCondition $condition -Action Block

$wafPolicy = New-AzApplicationGatewayFirewallPolicy -Name wafPolicy -ResourceGroup $rgname -Location $location -CustomRule $rule

$wafConfig = New-AzApplicationGatewayWebApplicationFirewallConfiguration -Enabled $true -FirewallMode "Prevention"
```

### <a name="create-an-application-gateway"></a>Uygulama ağ geçidi oluşturun

Aşağıdaki kodu çalıştırın:

```azurepowershell
$appgw = New-AzApplicationGateway -Name $appgwName -ResourceGroupName $rgname `
   -Location $location -BackendAddressPools $pool `
   -BackendHttpSettingsCollection  $poolSetting01 `
   -GatewayIpConfigurations $gipconfig -FrontendIpConfigurations $fipconfig01 `
   -FrontendPorts $fp01 -HttpListeners $listener01 `
   -RequestRoutingRules $rule01 -Sku $sku -AutoscaleConfiguration $autoscaleConfig `
   -WebApplicationFirewallConfig $wafConfig `
   -FirewallPolicy $wafPolicy
```

## <a name="next-steps"></a>Sonraki adımlar

[Web uygulaması güvenlik duvarı hakkında daha fazla bilgi edinin](waf-overview.md)
