---
title: Ön kapı Azure PowerShell için bir Web uygulaması güvenlik duvarı hız sınırı kuralı yapılandırma
description: Varolan bir ön kapı uç noktası için hız limiti kuralını yapılandırmayı öğrenin.
services: frontdoor
documentationcenter: ''
author: KumudD
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/31/2019
ms.author: kumud
ms.reviewer: tyao
ms.openlocfilehash: 99af39e996aaadd572603f63d019ff929b679550
ms.sourcegitcommit: fa45c2bcd1b32bc8dd54a5dc8bc206d2fe23d5fb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2019
ms.locfileid: "67846255"
---
# <a name="configure-a-web-application-firewall-rate-limit-rule-using-azure-powershell"></a>Azure PowerShell kullanarak bir Web uygulaması güvenlik duvarı hız sınırı kuralı yapılandırma
Azure ön kapısının Azure Web uygulaması güvenlik duvarı (WAF) hız sınırı kuralı, tek dakikalık bir süre içinde tek bir istemci IP 'sinden izin verilen isteklerin sayısını denetler.
Bu makalede, tek bir istemciden Azure PowerShell kullanarak URL 'de */promosyon* içeren bir Web uygulamasına izin verilen isteklerin sayısını denetleyen bir WAF hız sınırı kuralının nasıl yapılandırılacağı gösterilmektedir.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar
Bir hız sınırı ilkesi ayarlamaya başlamadan önce, PowerShell ortamınızı ayarlayın ve bir ön kapı profili oluşturun.
### <a name="set-up-your-powershell-environment"></a>PowerShell ortamınızı hazırlama
Azure PowerShell, Azure kaynaklarınızı yönetmek için [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) modelini kullanan bir dizi cmdlet sunar. 

[Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview)'i yerel makinenize yükleyebilir ve herhangi bir PowerShell oturumunda kullanabilirsiniz. Sayfadaki yönergeleri izleyerek Azure kimlik bilgilerinizle oturum açın ve az PowerShell Module ' ü kullanın.

#### <a name="connect-to-azure-with-an-interactive-dialog-for-sign-in"></a>Oturum açma için etkileşimli iletişim kutusuyla Azure 'a bağlanma
```
Connect-AzAccount

```
Front Door modülünü yüklemeden önce geçerli PowerShellGet sürümünün yüklü olduğundan emin olun. Aşağıdaki komutu çalıştırın ve PowerShell'i yeniden açın.

```
Install-Module PowerShellGet -Force -AllowClobber
``` 

#### <a name="install-azfrontdoor-module"></a>Install az. Frontkapı Module 

```
Install-Module -Name Az.FrontDoor
```
### <a name="create-a-front-door-profile"></a>Ön kapı profili oluşturma
Hızlı Başlangıç bölümünde [açıklanan yönergeleri izleyerek bir ön kapı profili oluşturun: Ön kapı profili oluşturma](quickstart-create-front-door.md)

## <a name="define-url-match-conditions"></a>URL eşleştirme koşullarını tanımlayın
[New-AzFrontDoorWafMatchConditionObject](/powershell/module/az.frontdoor/new-azfrontdoorwafmatchconditionobject)kullanarak bir URL eşleştirme koşulu TANıMLAYıN (URL/promosyon içerir).
Aşağıdaki örnek, *RequestUri* değişkeninin değeri olarak */promosyon* ile eşleşir:

```powershell-interactive
   $promoMatchCondition = New-AzFrontDoorWafMatchConditionObject `
     -MatchVariable RequestUri `
     -OperatorProperty Contains `
     -MatchValue "/promo"
```
## <a name="create-a-custom-rate-limit-rule"></a>Özel bir hız sınırı kuralı oluşturma
[New-AzFrontDoorWafCustomRuleObject](/powershell/module/az.frontdoor/new-azfrontdoorwafcustomruleobject)kullanarak bir hız sınırı ayarlayın. Aşağıdaki örnekte, sınır 1000 olarak ayarlanır. Bir dakika boyunca 1000 aşan promosyon sayfasına herhangi bir istemciden gelen istekler, bir sonraki dakika başlamadan önce engellenir.

```powershell-interactive
   $promoRateLimitRule = New-AzFrontDoorWafCustomRuleObject `
     -Name "rateLimitRule" `
     -RuleType RateLimitRule `
     -MatchCondition $promoMatchCondition `
     -RateLimitThreshold 1000 `
     -Action Block -Priority 1
```


## <a name="configure-a-security-policy"></a>Güvenlik ilkesi yapılandırma

Kullanarak `Get-AzureRmResourceGroup`ön kapı profilini içeren kaynak grubunun adını bulun. Ardından, ön kapı profilini içeren belirtilen kaynak grubunda [New-AzFrontDoorWafPolicy](/powershell/module/az.frontdoor/new-azfrontdoorwafpolicy) kullanarak bir güvenlik ilkesini özel bir hız sınırı kuralıyla yapılandırın.

Aşağıdaki örnek, [hızlı başlangıçta belirtilen yönergeleri kullanarak ön kapı profilini oluşturduğunuz varsayımıyla *myResourceGroupFD1* kaynak grubu adını kullanır: Ön kapı](quickstart-create-front-door.md) oluşturma makalesi.

 [New-AzFrontDoorWafPolicy](/powershell/module/az.frontdoor/new-azfrontdoorwafpolicy)kullanma.

```powershell-interactive
   $ratePolicy = New-AzFrontDoorWafPolicy `
     -Name "RateLimitPolicyExamplePS" `
     -resourceGroupName myResourceGroupFD1 `
     -Customrule $promoRateLimitRule `
     -Mode Prevention `
     -EnabledState Enabled
```
## <a name="link-policy-to-a-front-door-front-end-host"></a>İlkeyi bir ön kapı ön uç konağına bağlama
Güvenlik İlkesi nesnesini var olan bir ön kapı ön uç konağına bağlayın ve ön kapı özelliklerini güncelleştirin. İlk olarak [Get-Azfrontkapısı](/powershell/module/Az.FrontDoor/Get-AzFrontDoor) komutunu kullanarak ön kapı nesnesini alın.
Ardından, ön uç *Webapplicationfirewallpolicylink* özelliğini, [set-azfrontkapısı](/powershell/module/Az.FrontDoor/Set-AzFrontDoor) komutunu kullanarak önceki adımda oluşturulan "$ratePolicy" öğesinin *RESOURCEID* değerini olarak ayarlayın. 

Aşağıdaki örnek, [hızlı başlangıçta belirtilen yönergeleri kullanarak ön kapı profilini oluşturduğunuz varsayımıyla *myResourceGroupFD1* kaynak grubu adını kullanır: Ön kapı](quickstart-create-front-door.md) oluşturma makalesi. Ayrıca, aşağıdaki örnekte $frontDoorName, ön kapı profilinizin adıyla değiştirin. 

```powershell-interactive
   $FrontDoorObjectExample = Get-AzFrontDoor `
     -ResourceGroupName myResourceGroupFD1 `
     -Name $frontDoorName
   $FrontDoorObjectExample[0].FrontendEndpoints[0].WebApplicationFirewallPolicyLink = $ratePolicy.Id
   Set-AzFrontDoor -InputObject $FrontDoorObjectExample[0]
 ```

> [!NOTE]
> Bir güvenlik ilkesini ön kapı ön ucuna bağlamak için yalnızca *Webapplicationfirewallpolicylink* özelliğini bir kez ayarlamanız gerekir. Sonraki ilke güncelleştirmeleri otomatik olarak ön uca uygulanır.

## <a name="next-steps"></a>Sonraki adımlar

- [Ön kapı](front-door-overview.md) hakkında daha fazla bilgi 


