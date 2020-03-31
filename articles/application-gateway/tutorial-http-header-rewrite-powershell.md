---
title: HTTP üstbilgilerini yeniden yazmak & Azure Uygulama Ağ Geçidi oluşturma
description: Bu makalede, Azure PowerShell kullanarak bir Azure Uygulama Ağ Geçidi oluşturma ve HTTP üstbilgileri yeniden yazma hakkında bilgi sağlar
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.date: 11/19/2019
ms.author: absha
ms.openlocfilehash: 2663c049245a7025b5948a64fc5008bb9e7dee90
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74173728"
---
# <a name="create-an-application-gateway-and-rewrite-http-headers"></a>Bir uygulama ağ geçidi oluşturun ve HTTP üstbilgilerini yeniden yazın

Yeni otomatik ölçekleme ve [bölge yedekli uygulama ağ geçidi SKU](https://docs.microsoft.com/azure/application-gateway/application-gateway-autoscaling-zone-redundant) oluştururken [HTTP istek ve yanıt üstbilgilerini yeniden yazmak için kuralları](rewrite-http-headers.md) yapılandırmak için Azure PowerShell'i kullanabilirsiniz

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
>
> * Otomatik ölçeklendirme sanal ağ oluşturma
> * Ayrılmış genel IP adresi oluşturma
> * Uygulama ağ geçidi altyapınızı ayarlama
> * Http üstbilgi yeniden yazma kuralı yapılandırmanızı belirtin
> * Otomatik ölçeklendirmeyi belirtme
> * Uygulama ağ geçidi oluşturma
> * Uygulama ağ geçidini test etme

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="prerequisites"></a>Ön koşullar

Bu makalede, Azure PowerShell'i yerel olarak çalıştırmanız gerekmektedir. Az modülü sürüm 1.0.0 veya daha sonra yüklü olmalıdır. Çalıştırın `Import-Module Az` `Get-Module Az` ve sonra sürümü bulmak için. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](https://docs.microsoft.com/powershell/azure/install-az-ps). PowerShell sürümünü doğruladıktan sonra, Azure ile bağlantı oluşturmak için `Login-AzAccount` komutunu çalıştırın.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

```azurepowershell
Connect-AzAccount
Select-AzSubscription -Subscription "<sub name>"
```

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma
Kullanılabilir konumlardan birinde bir kaynak grubu oluşturun.

```azurepowershell
$location = "East US 2"
$rg = "<rg name>"

#Create a new Resource Group
New-AzResourceGroup -Name $rg -Location $location
```

## <a name="create-a-virtual-network"></a>Sanal ağ oluşturma

Otomatik ölçekleme uygulaması ağ geçidi için özel bir alt ağ içeren bir sanal ağ oluşturun. Şu anda her ayrılmış alt ağda yalnızca bir otomatik ölçeklendirme yapan uygulama ağ geçidi dağıtılabilir.

```azurepowershell
#Create VNet with two subnets
$sub1 = New-AzVirtualNetworkSubnetConfig -Name "AppGwSubnet" -AddressPrefix "10.0.0.0/24"
$sub2 = New-AzVirtualNetworkSubnetConfig -Name "BackendSubnet" -AddressPrefix "10.0.1.0/24"
$vnet = New-AzvirtualNetwork -Name "AutoscaleVNet" -ResourceGroupName $rg `
       -Location $location -AddressPrefix "10.0.0.0/16" -Subnet $sub1, $sub2
```

## <a name="create-a-reserved-public-ip"></a>Ayrılmış genel IP adresi oluşturma

PublicIPAddress'in ayırma yöntemini **Statik**olarak belirtin. Otomatik ölçeklendirme yapan uygulama ağ geçidi VIP’si yalnızca statik olabilir. Dinamik IP’ler desteklenmez. Yalnızca standart PublicIpAddress SKU’su desteklenir.

```azurepowershell
#Create static public IP
$pip = New-AzPublicIpAddress -ResourceGroupName $rg -name "AppGwVIP" `
       -location $location -AllocationMethod Static -Sku Standard
```

## <a name="retrieve-details"></a>Ayrıntıları alma

Uygulama ağ geçidiiçin IP yapılandırma ayrıntılarını oluşturmak için yerel bir nesnedeki kaynak grubu, alt ağ ve IP ayrıntılarını alın.

```azurepowershell
$resourceGroup = Get-AzResourceGroup -Name $rg
$publicip = Get-AzPublicIpAddress -ResourceGroupName $rg -name "AppGwVIP"
$vnet = Get-AzvirtualNetwork -Name "AutoscaleVNet" -ResourceGroupName $rg
$gwSubnet = Get-AzVirtualNetworkSubnetConfig -Name "AppGwSubnet" -VirtualNetwork $vnet
```

## <a name="configure-the-infrastructure"></a>Altyapıyı yapılandırma

IP config, front-end IP config, arka uç havuzu, HTTP ayarları, sertifika, bağlantı noktası ve dinleyiciyi varolan Standart uygulama ağ geçidiyle aynı biçimde yapılandırın. Yeni SKU, standart SKU ile aynı nesne modelini izler.

```azurepowershell
$ipconfig = New-AzApplicationGatewayIPConfiguration -Name "IPConfig" -Subnet $gwSubnet
$fip = New-AzApplicationGatewayFrontendIPConfig -Name "FrontendIPCOnfig" -PublicIPAddress $publicip
$pool = New-AzApplicationGatewayBackendAddressPool -Name "Pool1" `
       -BackendIPAddresses testbackend1.westus.cloudapp.azure.com, testbackend2.westus.cloudapp.azure.com
$fp01 = New-AzApplicationGatewayFrontendPort -Name "HTTPPort" -Port 80

$listener01 = New-AzApplicationGatewayHttpListener -Name "HTTPListener" `
             -Protocol Http -FrontendIPConfiguration $fip -FrontendPort $fp01

$setting = New-AzApplicationGatewayBackendHttpSettings -Name "BackendHttpSetting1" `
          -Port 80 -Protocol Http -CookieBasedAffinity Disabled
```

## <a name="specify-your-http-header-rewrite-rule-configuration"></a>HTTP üstbilgi yeniden yazma kuralı yapılandırmanızı belirtin

Http üstbilgilerini yeniden yazmak için gereken yeni nesneleri yapılandırın:

- **RequestHeaderConfiguration**: Bu nesne, yeniden yazmak istediğiniz istek üstbilgi alanlarını ve özgün üstbilginin yeniden yazılması gereken yeni değeri belirtmek için kullanılır.
- **ResponseHeaderConfiguration**: Bu nesne, yeniden yazmak istediğiniz yanıt üstbilgi alanlarını ve özgün üstbilginin yeniden yazılması gereken yeni değeri belirtmek için kullanılır.
- **ActionSet**: Bu nesne, yukarıda belirtilen istek ve yanıt üstbilgilerinin yapılandırmalarını içerir. 
- **RewriteRule**: Bu nesne yukarıda belirtilen tüm *actionSets* içerir. 
- **RewriteRuleSet**- bu nesne tüm *yeniden yazma Kuralları* içerir ve bir istek yönlendirme kuralına eklenmesi gerekir - temel veya yol tabanlı.

   ```azurepowershell
   $requestHeaderConfiguration = New-AzApplicationGatewayRewriteRuleHeaderConfiguration -HeaderName "X-isThroughProxy" -HeaderValue "True"
   $responseHeaderConfiguration = New-AzApplicationGatewayRewriteRuleHeaderConfiguration -HeaderName "Strict-Transport-Security" -HeaderValue "max-age=31536000"
   $actionSet = New-AzApplicationGatewayRewriteRuleActionSet -RequestHeaderConfiguration $requestHeaderConfiguration -ResponseHeaderConfiguration $responseHeaderConfiguration    
   $rewriteRule = New-AzApplicationGatewayRewriteRule -Name rewriteRule1 -ActionSet $actionSet    
   $rewriteRuleSet = New-AzApplicationGatewayRewriteRuleSet -Name rewriteRuleSet1 -RewriteRule $rewriteRule
   ```

## <a name="specify-the-routing-rule"></a>Yönlendirme kuralını belirtin

İstek yönlendirme kuralı oluşturun. Oluşturulduktan sonra, bu yeniden yazma yapılandırması yönlendirme kuralı ile kaynak dinleyiciye eklenir. Temel bir yönlendirme kuralı kullanırken, üstbilgi yeniden yazma yapılandırması bir kaynak dinleyiciyle ilişkilidir ve genel bir üstbilgi yeniden yazmaktır. Yol tabanlı yönlendirme kuralı kullanıldığında, üstbilgi yeniden yazma yapılandırması URL yol haritasında tanımlanır. Bu nedenle, yalnızca bir sitenin belirli yol alanı için geçerlidir. Aşağıda, temel bir yönlendirme kuralı oluşturulur ve yeniden yazma kuralı kümesi eklenir.

```azurepowershell
$rule01 = New-AzApplicationGatewayRequestRoutingRule -Name "Rule1" -RuleType basic `
         -BackendHttpSettings $setting -HttpListener $listener01 -BackendAddressPool $pool -RewriteRuleSet $rewriteRuleSet
```

## <a name="specify-autoscale"></a>Otomatik ölçeklendirmeyi belirtme

Artık uygulama ağ geçidi için otomatik ölçek yapılandırmasını belirtebilirsiniz. İki otomatik ölçeklendirme yapılandırması türü desteklenir:

* **Sabit kapasite modu**. Bu modda, uygulama ağ geçidi otomatik ölçeklendirme yapmaz ve sabit bir Ölçek Birimi kapasitesinde çalışır.

   ```azurepowershell
   $sku = New-AzApplicationGatewaySku -Name Standard_v2 -Tier Standard_v2 -Capacity 2
   ```

* **Otomatik ölçeklendirme modu**. Bu modda, uygulama ağ geçidi uygulamanın trafik desenine bağlı olarak otomatik ölçeklendirme yapar.

   ```azurepowershell
   $autoscaleConfig = New-AzApplicationGatewayAutoscaleConfiguration -MinCapacity 2
   $sku = New-AzApplicationGatewaySku -Name Standard_v2 -Tier Standard_v2
   ```

## <a name="create-the-application-gateway"></a>Uygulama ağ geçidi oluşturma

Uygulama ağ geçidini oluşturun ve artıklık bölgeleri ve otomatik ölçek yapılandırması ekleyin.

```azurepowershell
$appgw = New-AzApplicationGateway -Name "AutoscalingAppGw" -Zone 1,2,3 -ResourceGroupName $rg -Location $location -BackendAddressPools $pool -BackendHttpSettingsCollection $setting -GatewayIpConfigurations $ipconfig -FrontendIpConfigurations $fip -FrontendPorts $fp01 -HttpListeners $listener01 -RequestRoutingRules $rule01 -Sku $sku -AutoscaleConfiguration $autoscaleConfig -RewriteRuleSet $rewriteRuleSet
```

## <a name="test-the-application-gateway"></a>Uygulama ağ geçidini test etme

Uygulama ağ geçidinin genel IP adresini almak için Get-AzPublicIPAddress'i kullanın. Genel IP adresini veya DNS adını kopyalayıp tarayıcınızın adres çubuğuna yapıştırın.

```azurepowershell
Get-AzPublicIPAddress -ResourceGroupName $rg -Name AppGwVIP
```



## <a name="clean-up-resources"></a>Kaynakları temizleme

Önce uygulama ağ geçidi yle oluşturulan kaynakları keşfedin. Daha sonra, artık bunlara ihtiyaç duyulmadığında, kaynak grubunu, uygulama ağ geçidini ve ilgili tüm kaynakları kaldırmak için `Remove-AzResourceGroup` komutu kullanabilirsiniz.

`Remove-AzResourceGroup -Name $rg`

## <a name="next-steps"></a>Sonraki adımlar

- [URL yolu tabanlı yönlendirme kuralları ile bir uygulama ağ geçidi oluşturma](./tutorial-url-route-powershell.md)
