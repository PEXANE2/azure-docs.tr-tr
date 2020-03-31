---
title: Azure Ön Kapı için WAF özel kurallarını & Varsayılan Kural Kümesi'ni yapılandırma
description: WaF ilkesini nasıl yapılandırıştırılacaya kadar varolan bir Ön Kapı bitiş noktası için hem özel hem de yönetilen kurallardan oluşan bir yapıya nasıl konfigürasyon yapılacağını öğrenin.
services: web-application-firewall
author: vhorne
ms.service: web-application-firewall
ms.topic: article
ms.date: 09/05/2019
ms.author: victorh
ms.openlocfilehash: 493ed1a31a23366a90b80d3ab510218c8dce0e9c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74186646"
---
# <a name="configure-a-web-application-firewall-policy-using-azure-powershell"></a>Azure PowerShell'i kullanarak Bir Web Uygulaması Güvenlik Duvarı ilkesini yapılandırma

Azure Web Uygulaması Güvenlik Duvarı (WAF) ilkesi, bir istek Ön Kapı'ya geldiğinde gerekli denetimleri tanımlar.
Bu makalede, bazı özel kurallardan oluşan ve Azure tarafından yönetilen Varsayılan Kural Kümesi etkinleştirilmiş bir WAF ilkesinin nasıl yapılandırılabildiğini gösterilmektedir.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="prerequisites"></a>Ön koşullar

Bir fiyat sınırı ilkesi ayarlamaya başlamadan önce PowerShell ortamınızı ayarlayın ve bir Ön Kapı profili oluşturun.

### <a name="set-up-your-powershell-environment"></a>PowerShell ortamınızı hazırlama

Azure PowerShell, Azure kaynaklarınızı yönetmek için [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) modelini kullanan bir dizi cmdlet sunar. 

[Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview)'i yerel makinenize yükleyebilir ve herhangi bir PowerShell oturumunda kullanabilirsiniz. Azure kimlik bilgilerinizle oturum açmanız ve Az PowerShell modüllerini yüklemek için sayfadaki yönergeleri izleyin.

#### <a name="sign-in-to-azure"></a>Azure'da oturum açma

```
Connect-AzAccount

```
Front Door modülünü yüklemeden önce geçerli PowerShellGet sürümünün yüklü olduğundan emin olun. Aşağıdaki komutu çalıştırın ve PowerShell'i yeniden açın.

```
Install-Module PowerShellGet -Force -AllowClobber
``` 

#### <a name="install-azfrontdoor-module"></a>Az.FrontDoor modüllerini yükleyin 

```
Install-Module -Name Az.FrontDoor
```
### <a name="create-a-front-door-profile"></a>Ön Kapı profili oluşturma

[Quickstart:Ön Kapı profili](../../frontdoor/quickstart-create-front-door.md) oluşturma

## <a name="custom-rule-based-on-http-parameters"></a>Http parametrelerine göre özel kural

Aşağıdaki örnek, [Yeni-AzFrontDoorWafMatchConditionObject](/powershell/module/az.frontdoor/new-azfrontdoorwafmatchconditionobject)kullanarak iki maç koşulları ile özel bir kural yapılandırmak için nasıl gösterir. İstekler, başvurucu tarafından tanımlandığı şekilde belirli bir siteden gelir ve sorgu dizesi "parola" içermez. 

```powershell-interactive
$referer = New-AzFrontDoorWafMatchConditionObject -MatchVariable RequestHeader -OperatorProperty Equal -Selector "Referer" -MatchValue "www.mytrustedsites.com/referpage.html"
$password = New-AzFrontDoorWafMatchConditionObject -MatchVariable QueryString -OperatorProperty Contains -MatchValue "password"
$AllowFromTrustedSites = New-AzFrontDoorWafCustomRuleObject -Name "AllowFromTrustedSites" -RuleType MatchRule -MatchCondition $referer,$password -Action Allow -Priority 1
```

## <a name="custom-rule-based-on-http-request-method"></a>Http istek yöntemine dayalı özel kural

[New-AzFrontDoorWafCustomRuleObject](/powershell/module/az.frontdoor/new-azfrontdoorwafcustomruleobject) kullanarak "PUT" yöntemini engelleyen bir kural oluşturun:

```powershell-interactive
$put = New-AzFrontDoorWafMatchConditionObject -MatchVariable RequestMethod -OperatorProperty Equal -MatchValue PUT
$BlockPUT = New-AzFrontDoorWafCustomRuleObject -Name "BlockPUT" -RuleType MatchRule -MatchCondition $put -Action Block -Priority 2
```

## <a name="create-a-custom-rule-based-on-size-constraint"></a>Boyut kısıtlaması temel alan özel bir kural oluşturma

Aşağıdaki örnek, Azure PowerShell kullanarak 100 karakterden uzun url ile istekleri engelleyen bir kural oluşturur:
```powershell-interactive
$url = New-AzFrontDoorWafMatchConditionObject -MatchVariable RequestUri -OperatorProperty GreaterThanOrEqual -MatchValue 100
$URLOver100 = New-AzFrontDoorWafCustomRuleObject -Name "URLOver100" -RuleType MatchRule -MatchCondition $url -Action Block -Priority 3
```
## <a name="add-managed-default-rule-set"></a>Yönetilen Varsayılan Kural Kümesi Ekle

Aşağıdaki örnek, Azure PowerShell kullanarak yönetilen bir Varsayılan Kural Kümesi oluşturur:
```powershell-interactive
$managedRules =  New-AzFrontDoorWafManagedRuleObject -Type DefaultRuleSet -Version 1.0
```
## <a name="configure-a-security-policy"></a>Güvenlik ilkesini yapılandırma

Ön Kapı profilini içeren kaynak grubunun adını `Get-AzResourceGroup`bulun. Ardından, Ön Kapı profilini içeren belirtilen kaynak grubunda [New-AzFrontDoorWafPolicy](/powershell/module/az.frontdoor/new-azfrontdoorwafpolicy) kullanarak önceki adımlarda oluşturulan kurallarla bir güvenlik ilkesini yapılandırın.

```powershell-interactive
$myWAFPolicy=New-AzFrontDoorWafPolicy -Name $policyName -ResourceGroupName $resourceGroupName -Customrule $AllowFromTrustedSites,$BlockPUT,$URLOver100 -ManagedRule $managedRules -EnabledState Enabled -Mode Prevention
```

## <a name="link-policy-to-a-front-door-front-end-host"></a>Ön Kapı ön uç ana bilgisayara bağlantı ilkesi

Güvenlik ilkesi nesnesini varolan bir Ön Kapı ön uç ana bilgisayara bağla ve Ön Kapı özelliklerini güncelleştir. İlk olarak, [Get-AzFrontDoor](/powershell/module/Az.FrontDoor/Get-AzFrontDoor)kullanarak Ön Kapı nesnesini alın.
Ardından, ön uç *WebApplicationFirewallPolicyLink* özelliğini [Set-AzFrontDoor](/powershell/module/Az.FrontDoor/Set-AzFrontDoor)kullanarak önceki adımda oluşturulan "$myWAFPolicy$" *kaynağına* ayarlayın. 

Aşağıdaki örnekte, [Quickstart: Create the Front Door](../../frontdoor/quickstart-create-front-door.md) makalesinde sağlanan yönergeleri kullanarak Ön Kapı profilini oluşturduğunuz varsayımıyla Kaynak Grubu adı *myResourceGroupFD1'i* kullanır. Ayrıca, aşağıdaki örnekte, $frontDoorName'yi Ön Kapı profilinizin adı ile değiştirin. 

```powershell-interactive
   $FrontDoorObjectExample = Get-AzFrontDoor `
     -ResourceGroupName myResourceGroupFD1 `
     -Name $frontDoorName
   $FrontDoorObjectExample[0].FrontendEndpoints[0].WebApplicationFirewallPolicyLink = $myWAFPolicy.Id
   Set-AzFrontDoor -InputObject $FrontDoorObjectExample[0]
 ```

> [!NOTE]
> Bir güvenlik ilkesini Ön Kapı ön ucuna bağlamak için *WebApplicationFirewallPolicyLink* özelliğini yalnızca bir kez ayarlamanız gerekir. Sonraki ilke güncelleştirmeleri otomatik olarak ön uca uygulanır.

## <a name="next-steps"></a>Sonraki adımlar

- [Ön Kapı](../../frontdoor/front-door-overview.md) hakkında daha fazla bilgi edinin 
- [Ön Kapı ile WAF](afds-overview.md) hakkında daha fazla bilgi edinin
