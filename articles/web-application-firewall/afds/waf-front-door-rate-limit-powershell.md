---
title: Ön Kapı için WAF oranı sınır kuralını yapılandırın - Azure PowerShell
description: Varolan bir Ön Kapı bitiş noktası için bir oran sınırı kuralını nasıl yapılandırıştırmayı öğrenin.
author: vhorne
ms.service: web-application-firewall
ms.topic: article
services: web-application-firewall
ms.date: 02/26/2020
ms.author: victorh
ms.openlocfilehash: b034159c3d12927f6425b3dc3c5b5609af9b0b76
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77649373"
---
# <a name="configure-a-web-application-firewall-rate-limit-rule-using-azure-powershell"></a>Azure PowerShell'i kullanarak Web Uygulaması Güvenlik Duvarı oranı sınır kuralını yapılandırma
Azure Ön Kapı için Azure Web Uygulaması Güvenlik Duvarı (WAF) oranı sınır kuralı, bir dakikalık süre boyunca istemcilerden izin verilen istek sayısını denetler.
Bu makalede, azure PowerShell kullanarak URL'de */promosyon* içeren bir web uygulamasına istemcilerden izin verilen istek sayısını kontrol eden WAF oranı sınır kuralının nasıl yapılandırılabildiğini gösterilmektedir.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="prerequisites"></a>Ön koşullar
Bir fiyat sınırı ilkesi ayarlamaya başlamadan önce PowerShell ortamınızı ayarlayın ve bir Ön Kapı profili oluşturun.
### <a name="set-up-your-powershell-environment"></a>PowerShell ortamınızı hazırlama
Azure PowerShell, Azure kaynaklarınızı yönetmek için [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) modelini kullanan bir dizi cmdlet sunar. 

[Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview)'i yerel makinenize yükleyebilir ve herhangi bir PowerShell oturumunda kullanabilirsiniz. Azure kimlik bilgilerinizle oturum açmanız ve Az PowerShell modüllerini yüklemek için sayfadaki yönergeleri izleyin.

#### <a name="connect-to-azure-with-an-interactive-dialog-for-sign-in"></a>Oturum açmak için etkileşimli bir iletişim kutusuyla Azure'a bağlanın
```
Connect-AzAccount

```
Ön Kapı modüllerini yüklemeden önce PowerShellGet'in geçerli sürümünün yüklü olduğundan emin olun. Aşağıdaki komutu çalıştırın ve PowerShell'i yeniden açın.

```
Install-Module PowerShellGet -Force -AllowClobber
``` 

#### <a name="install-azfrontdoor-module"></a>Az.FrontDoor modüllerini yükleyin 

```
Install-Module -Name Az.FrontDoor
```
### <a name="create-a-front-door-profile"></a>Ön Kapı profili oluşturma
[Quickstart:Ön Kapı profili](../../frontdoor/quickstart-create-front-door.md) oluşturma

## <a name="define-url-match-conditions"></a>url eşleşme koşullarını tanımlama
[New-AzFrontDoorWafMatchConditionObject](/powershell/module/az.frontdoor/new-azfrontdoorwafmatchconditionobject)kullanarak bir URL eşleşmesi koşulu tanımlayın (URL içerir /promosyon içerir).
Aşağıdaki *örnek, RequestUri* değişkeninin değeri olarak */promo* ile eşleşir:

```powershell-interactive
   $promoMatchCondition = New-AzFrontDoorWafMatchConditionObject `
     -MatchVariable RequestUri `
     -OperatorProperty Contains `
     -MatchValue "/promo"
```
## <a name="create-a-custom-rate-limit-rule"></a>Özel oran sınır kuralı oluşturma
[New-AzFrontDoorWafCustomRuleObject](/powershell/module/az.frontdoor/new-azfrontdoorwafcustomruleobject)kullanarak bir oran sınırı ayarlayın. Aşağıdaki örnekte, sınır 1000 olarak ayarlanır. Bir dakika içinde 1000'i aşan promosyon sayfasına herhangi bir istemciden gelen istekler, bir sonraki dakika başlayana kadar engellenir.

```powershell-interactive
   $promoRateLimitRule = New-AzFrontDoorWafCustomRuleObject `
     -Name "rateLimitRule" `
     -RuleType RateLimitRule `
     -MatchCondition $promoMatchCondition `
     -RateLimitThreshold 1000 `
     -Action Block -Priority 1
```


## <a name="configure-a-security-policy"></a>Güvenlik ilkesini yapılandırma

Ön Kapı profilini içeren kaynak grubunun adını `Get-AzureRmResourceGroup`bulun. Ardından, Ön Kapı profilini içeren belirtilen kaynak grubunda [New-AzFrontDoorWafPolicy](/powershell/module/az.frontdoor/new-azfrontdoorwafpolicy) kullanarak özel bir oran sınırı kuralıyla bir güvenlik ilkesini yapılandırın.

Aşağıdaki örnekte, [Quickstart: Create the Front Door](../../frontdoor/quickstart-create-front-door.md) makalesinde sağlanan yönergeleri kullanarak Ön Kapı profilini oluşturduğunuz varsayımıyla Kaynak Grubu adı *myResourceGroupFD1'i* kullanır.

 [New-AzFrontDoorWafPolicy](/powershell/module/az.frontdoor/new-azfrontdoorwafpolicy)kullanarak.

```powershell-interactive
   $ratePolicy = New-AzFrontDoorWafPolicy `
     -Name "RateLimitPolicyExamplePS" `
     -resourceGroupName myResourceGroupFD1 `
     -Customrule $promoRateLimitRule `
     -Mode Prevention `
     -EnabledState Enabled
```
## <a name="link-policy-to-a-front-door-front-end-host"></a>Ön Kapı ön uç ana bilgisayara bağlantı ilkesi
Güvenlik ilkesi nesnesini varolan bir Ön Kapı ön uç ana bilgisayara bağla ve Ön Kapı özelliklerini güncelleştir. Önce [Get-AzFrontDoor](/powershell/module/Az.FrontDoor/Get-AzFrontDoor) komutunu kullanarak Ön Kapı nesnesini alın.
Ardından, ön uç *WebApplicationFirewallPolicyLink* özelliğini [Set-AzFrontDoor](/powershell/module/Az.FrontDoor/Set-AzFrontDoor) komutunu kullanarak önceki adımda oluşturulan "$ratePolicy" *kaynağının kaynağına* ayarlayın. 

Aşağıdaki örnekte, [Quickstart: Create the Front Door](../../frontdoor/quickstart-create-front-door.md) makalesinde sağlanan yönergeleri kullanarak Ön Kapı profilini oluşturduğunuz varsayımıyla Kaynak Grubu adı *myResourceGroupFD1'i* kullanır. Ayrıca, aşağıdaki örnekte, $frontDoorName'yi Ön Kapı profilinizin adı ile değiştirin. 

```powershell-interactive
   $FrontDoorObjectExample = Get-AzFrontDoor `
     -ResourceGroupName myResourceGroupFD1 `
     -Name $frontDoorName
   $FrontDoorObjectExample[0].FrontendEndpoints[0].WebApplicationFirewallPolicyLink = $ratePolicy.Id
   Set-AzFrontDoor -InputObject $FrontDoorObjectExample[0]
 ```

> [!NOTE]
> Bir güvenlik ilkesini Ön Kapı ön ucuna bağlamak için *WebApplicationFirewallPolicyLink* özelliğini yalnızca bir kez ayarlamanız gerekir. Sonraki ilke güncelleştirmeleri otomatik olarak ön uca uygulanır.

## <a name="next-steps"></a>Sonraki adımlar

- [Ön Kapı](../../frontdoor/front-door-overview.md)hakkında daha fazla bilgi edinin. 


