---
title: Azure Uygulama Ağ Geçidi'nde HTTP üstbilgilerini yeniden yazma
description: Bu makale, Azure PowerShell'i kullanarak Azure Uygulama Ağ Geçidi'ndeki HTTP üstbilgilerini yeniden yazma hakkında bilgi sağlar
services: application-gateway
author: abshamsft
ms.service: application-gateway
ms.topic: article
ms.date: 04/12/2019
ms.author: absha
ms.openlocfilehash: 47fe6a5247622e3ad3b3720955068580e0329913
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "64947198"
---
# <a name="rewrite-http-request-and-response-headers-with-azure-application-gateway---azure-powershell"></a>Azure Application Gateway - Azure PowerShell ile HTTP istek ve yanıt üstbilgilerini yeniden yazın

Bu makalede, taleplerde ve yanıtlarda HTTP üstbilgilerini yeniden yazmak için bir [Uygulama Ağ Geçidi v2 SKU](<https://docs.microsoft.com/azure/application-gateway/application-gateway-autoscaling-zone-redundant>) örneğini yapılandırmak için Azure PowerShell'in nasıl kullanılacağı açıklanmaktadır.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="before-you-begin"></a>Başlamadan önce

- Bu makaledeki adımları tamamlamak için Azure PowerShell'i yerel olarak çalıştırmanız gerekir. Ayrıca Az modülü sürüm 1.0.0 veya daha sonra yüklü olması gerekir. Yüklediğiniz sürümü belirlemek için çalıştırın `Import-Module Az` ve ardından `Get-Module Az` çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](https://docs.microsoft.com/powershell/azure/install-az-ps). PowerShell sürümünü doğruladıktan sonra, Azure ile bağlantı oluşturmak için `Login-AzAccount` komutunu çalıştırın.
- Bir Uygulama Ağ Geçidi v2 SKU örneği olması gerekir. Başlıkların yeniden yazılması v1 SKU'da desteklenmez. v2 SKU'nuz yoksa başlamadan önce bir [Uygulama Ağ Geçidi v2 SKU](https://docs.microsoft.com/azure/application-gateway/tutorial-autoscale-ps) örneği oluşturun.

## <a name="create-required-objects"></a>Gerekli nesneleri oluşturma

HTTP üstbilgi yeniden yapılandırmak için, bu adımları tamamlamanız gerekir.

1. HTTP üstbilgi yeniden yazmak için gerekli olan nesneleri oluşturun:

   - **RequestHeaderConfiguration**: Yeniden yazmak istediğiniz istek üstbilgi alanlarını ve üstbilgi için yeni değeri belirtmek için kullanılır.

   - **ResponseHeaderConfiguration**: Yeniden yazmak istediğiniz yanıt üstbilgi alanlarını ve üstbilgi için yeni değeri belirtmek için kullanılır.

   - **ActionSet**: Daha önce belirtilen istek ve yanıt üstbilgilerinin yapılandırmalarını içerir.

   - **Koşul**: İsteğe bağlı yapılandırma. Yeniden yazma koşulları, HTTP(S) istek ve yanıtlarının içeriğini değerlendirir. HTTP(S) isteği veya yanıtı yeniden yazma koşuluyla eşleşirse yeniden yazma eylemi gerçekleşir.

     Birden fazla koşulu bir eylemle ilişkilendirirseniz, eylem yalnızca tüm koşullar karşılandığında oluşur. Başka bir deyişle, işlem mantıksal ve işlemdir.

   - **RewriteRule**: Birden çok yeniden yazma eylemi / durum kombinasyonları yeniden içerir.

   - **RuleSequence**: Yeniden yazma kurallarının yürütülme sırasını belirlemeye yardımcı olan isteğe bağlı yapılandırma. Bu yapılandırma, yeniden yazma kümesinde birden çok yeniden yazma kuralınız olduğunda yararlıdır. Daha düşük kural sırası değeriolan bir yeniden yazma kuralı önce çalışır. Aynı kural sırası değerini iki yeniden yazma kuralına atarsanız, yürütme sırası belirleyici değildir.

     RuleSequence'i açıkça belirtmezseniz, varsayılan değeri 100 olarak ayarlanır.

   - **RewriteRuleSet**: İstek yönlendirme kuralıyla ilişkilendirilecek birden çok yeniden yazma kuralı içerir.

2. Yönlendirme kuralına RewriteRuleSet'i takın. Yeniden yazma yapılandırması yönlendirme kuralı ile kaynak dinleyiciye eklenir. Temel bir yönlendirme kuralı kullandığınızda, üstbilgi yeniden yazma yapılandırması bir kaynak dinleyiciyle ilişkilidir ve genel bir üstbilgi yeniden yazmaktır. Yol tabanlı yönlendirme kuralı kullandığınızda, üstbilgi yeniden yazma yapılandırması URL yol haritasında tanımlanır. Bu durumda, yalnızca bir sitenin belirli yol alanı için geçerlidir.

Birden çok HTTP üstbilgi yeniden yazma kümeleri oluşturabilir ve her yeniden yazma kümesini birden çok dinleyiciye uygulayabilirsiniz. Ancak belirli bir dinleyiciye yalnızca bir yeniden yazma kümesi uygulayabilirsiniz.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

```azurepowershell
Connect-AzAccount
Select-AzSubscription -Subscription "<sub name>"
```

## <a name="specify-the-http-header-rewrite-rule-configuration"></a>HTTP üstbilgi yeniden yazma kuralı yapılandırmasını belirtin

Bu örnekte, konum üstbilgisi azurewebsites.net bir başvuru içerdiğinde, HTTP yanıtındaki konum üstbilgisini yeniden yazarak yeniden yönlendirme URL'sini değiştiririz. Bunu yapmak için, yanıttaki konum üstbilgisinin azurewebsites.net içerip içermediğini değerlendirmek için bir koşul ekleriz. Deseni `(https?):\/\/.*azurewebsites\.net(.*)$`kullanacağız. Ve üstbilgi `{http_resp_Location_1}://contoso.com{http_resp_Location_2}` değeri olarak kullanacağız. Bu değer, *azurewebsites.net* yerine konum üstbilgisinde *contoso.com.*

```azurepowershell
$responseHeaderConfiguration = New-AzApplicationGatewayRewriteRuleHeaderConfiguration -HeaderName "Location" -HeaderValue "{http_resp_Location_1}://contoso.com{http_resp_Location_2}"
$actionSet = New-AzApplicationGatewayRewriteRuleActionSet -RequestHeaderConfiguration $requestHeaderConfiguration -ResponseHeaderConfiguration $responseHeaderConfiguration
$condition = New-AzApplicationGatewayRewriteRuleCondition -Variable "http_resp_Location" -Pattern "(https?):\/\/.*azurewebsites\.net(.*)$" -IgnoreCase
$rewriteRule = New-AzApplicationGatewayRewriteRule -Name LocationHeader -ActionSet $actionSet
$rewriteRuleSet = New-AzApplicationGatewayRewriteRuleSet -Name LocationHeaderRewrite -RewriteRule $rewriteRule
```

## <a name="retrieve-the-configuration-of-your-application-gateway"></a>Uygulama ağ geçidinizin yapılandırmasını alma

```azurepowershell
$appgw = Get-AzApplicationGateway -Name "AutoscalingAppGw" -ResourceGroupName "<rg name>"
```

## <a name="retrieve-the-configuration-of-your-request-routing-rule"></a>İstek yönlendirme kuralınızın yapılandırmasını alma

```azurepowershell
$reqRoutingRule = Get-AzApplicationGatewayRequestRoutingRule -Name rule1 -ApplicationGateway $appgw
```

## <a name="update-the-application-gateway-with-the-configuration-for-rewriting-http-headers"></a>HTTP üstbilgilerini yeniden yazmak için yapılandırmaile uygulama ağ geçidini güncelleştirme

```azurepowershell
Add-AzApplicationGatewayRewriteRuleSet -ApplicationGateway $appgw -Name LocationHeaderRewrite -RewriteRule $rewriteRuleSet.RewriteRules
Set-AzApplicationGatewayRequestRoutingRule -ApplicationGateway $appgw -Name rule1 -RuleType $reqRoutingRule.RuleType -BackendHttpSettingsId $reqRoutingRule.BackendHttpSettings.Id -HttpListenerId $reqRoutingRule.HttpListener.Id -BackendAddressPoolId $reqRoutingRule.BackendAddressPool.Id -RewriteRuleSetId $rewriteRuleSet.Id
Set-AzApplicationGateway -ApplicationGateway $appgw
```

## <a name="delete-a-rewrite-rule"></a>Yeniden yazma kuralını silme

```azurepowershell
$appgw = Get-AzApplicationGateway -Name "AutoscalingAppGw" -ResourceGroupName "<rg name>"
Remove-AzApplicationGatewayRewriteRuleSet -Name "LocationHeaderRewrite" -ApplicationGateway $appgw
$requestroutingrule= Get-AzApplicationGatewayRequestRoutingRule -Name "rule1" -ApplicationGateway $appgw
$requestroutingrule.RewriteRuleSet= $null
set-AzApplicationGateway -ApplicationGateway $appgw
```

## <a name="next-steps"></a>Sonraki adımlar

Bazı yaygın kullanım örneklerinin nasıl ayarlanılabildiği hakkında daha fazla bilgi edinmek için, [ortak üstbilgisenaryolarını yeniden yazma tarihine](https://docs.microsoft.com/azure/application-gateway/rewrite-http-headers)bakın.