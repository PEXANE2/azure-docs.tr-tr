---
title: Öğretici - Coğrafi filtreleme WAF ilkesini yapılandırın - Azure Ön Kapı
description: Bu eğitimde, bir coğrafi filtreleme ilkesi oluşturmayı ve politikayı mevcut Ön Kapı ön yüz ana bağlantı sunucunuzla ilişkilendirmeyi öğreneceksiniz
services: frontdoor
documentationcenter: ''
author: teresayao
ms.service: frontdoor
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/21/2019
ms.author: tyao
ms.openlocfilehash: e3119745e35140d0344d25f34f54b63939d2542d
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "79471464"
---
# <a name="how-to-set-up-a-geo-filtering-waf-policy-for-your-front-door"></a>Front Door hizmetiniz için bir coğrafi filtreleme WAF ilkesi hazırlama
Bu öğreticide, örnek bir coğrafi filtreleme ilkesi oluşturmak ve bu ilkeyi mevcut bir Front Door ön uç konağı ile ilişkilendirmek için Azure PowerShell kullanma gösterilmektedir. Bu örnek coğrafi filtreleme ilkesi, Amerika Birleşik Devletleri dışındaki tüm diğer ülkelerden/bölgelerden gelen istekleri engeller.

Azure aboneliğiniz yoksa şimdi [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Ön koşullar
Bir coğrafi filtre ilkesi ayarlamaya başlamadan önce PowerShell ortamınızı ayarlayın ve bir Ön Kapı profili oluşturun.
### <a name="set-up-your-powershell-environment"></a>PowerShell ortamınızı hazırlama
Azure PowerShell, Azure kaynaklarınızı yönetmek için [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) modelini kullanan bir dizi cmdlet sunar. 

[Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview)'i yerel makinenize yükleyebilir ve herhangi bir PowerShell oturumunda kullanabilirsiniz. Azure kimlik bilgilerinizle oturum açmanız ve Az PowerShell modüllerini yüklemek için sayfadaki yönergeleri izleyin.

#### <a name="connect-to-azure-with-an-interactive-dialog-for-sign-in"></a>Oturum açmak için etkileşimli bir iletişim kutusuyla Azure'a bağlanma
```
Install-Module -Name Az
Connect-AzAccount
```
PowerShellGet'in geçerli sürümünün yüklü olduğundan emin olun. Aşağıdaki komutu çalıştırın ve PowerShell'i yeniden açın.

```
Install-Module PowerShellGet -Force -AllowClobber
``` 
#### <a name="install-azfrontdoor-module"></a>Az.FrontDoor modüllerini yükleyin 

```
Install-Module -Name Az.FrontDoor
```

### <a name="create-a-front-door-profile"></a>Ön Kapı profili oluşturma
[Quickstart: Ön Kapı profili](quickstart-create-front-door.md)oluşturma'da açıklanan yönergeleri izleyerek bir Ön Kapı profili oluşturun.

## <a name="define-geo-filtering-match-condition"></a>Coğrafi filtreleme eşleşmesi koşulunu tanımlama

Eşleşme koşulu oluştururken parametrelerde [New-AzFrontDoorWafMatchConditionObject](/powershell/module/az.frontdoor/new-azfrontdoorwafmatchconditionobject) kullanarak "ABD"den gelmeyen istekleri seçen örnek bir eşleşme koşulu oluşturun. Ülke haritalama için iki harfli ülke kodları [burada](front-door-geo-filtering.md)sağlanmaktadır.

```azurepowershell-interactive
$nonUSGeoMatchCondition = New-AzFrontDoorWafMatchConditionObject `
-MatchVariable RemoteAddr `
-OperatorProperty GeoMatch `
-NegateCondition $true `
-MatchValue "US"
```
 
## <a name="add-geo-filtering-match-condition-to-a-rule-with-action-and-priority"></a>Eylem ve Öncelik ile bir kurala coğrafi filtreleme eşleşme koşulu ekleme

`nonUSBlockRule` [New-AzFrontDoorWafCustomRuleObject](/powershell/module/az.frontdoor/new-azfrontdoorwafcustomruleobject)kullanarak eşleşme koşuluna, Eyleme ve Önceliğe dayalı bir CustomRule nesnesi oluşturun.  Bir CustomRule'un birden fazla MatchCondition'ı olabilir.  Bu örnekte Eylem Engelle değerine, Öncelik ise en yüksek öncelik olan 1 değerine ayarlanmıştır.

```
$nonUSBlockRule = New-AzFrontDoorWafCustomRuleObject `
-Name "geoFilterRule" `
-RuleType MatchRule `
-MatchCondition $nonUSGeoMatchCondition `
-Action Block `
-Priority 1
```

## <a name="add-rules-to-a-policy"></a>İlkelere kural ekleme
Ön Kapı profilini içeren kaynak grubunun adını `Get-AzResourceGroup`bulun. Ardından, Ön `geoPolicy` Kapı `nonUSBlockRule` profilini içeren belirtilen kaynak grubunda [New-AzFrontDoorWafPolicy](/powershell/module/az.frontdoor/new-azfrontdoorwafpolicy) kullanarak içeren bir ilke nesnesi oluşturun. Coğrafi filtreleme ilkesi için benzersiz bir ad sağlamanız gerekir. 

Aşağıdaki örnekte, [Quickstart: Create a Front Door](quickstart-create-front-door.md) makalesinde sağlanan yönergeleri kullanarak Ön Kapı profilini oluşturduğunuz varsayımıyla Kaynak Grubu adı *myResourceGroupFD1'i* kullanır. Aşağıdaki örnekte, *geoPolicyAllowUSYalnızca* benzersiz bir ilke adı ile ilke adını değiştirin.

```
$geoPolicy = New-AzFrontDoorWafPolicy `
-Name "geoPolicyAllowUSOnly" `
-resourceGroupName myResourceGroupFD1 `
-Customrule $nonUSBlockRule  `
-Mode Prevention `
-EnabledState Enabled
```

## <a name="link-waf-policy-to-a-front-door-frontend-host"></a>WAF politikasını Ön Kapı ön yüz ana bilgisayara bağla
WAF ilkesi nesnesini mevcut Ön Kapı ön yüz ana bilgisayarına bağla ve Ön Kapı özelliklerini güncelleştir. 

Bunu yapmak için, ilk [Get-AzFrontDoor](/powershell/module/az.frontdoor/get-azfrontdoor)kullanarak Ön Kapı nesnealmak. 

```
$geoFrontDoorObjectExample = Get-AzFrontDoor -ResourceGroupName myResourceGroupFD1
$geoFrontDoorObjectExample[0].FrontendEndpoints[0].WebApplicationFirewallPolicyLink = $geoPolicy.Id
```

Ardından, ön uç WebApplicationFirewallPolicyLink özelliğini `geoPolicy`kullanarak [Set-AzFrontDoor'un](/powershell/module/az.frontdoor/set-azfrontdoor)resourceId'ine ayarlayın.

```
Set-AzFrontDoor -InputObject $geoFrontDoorObjectExample[0]
```

> [!NOTE] 
> Bir WAF ilkesini Ön Kapı ön yüz ana bilgisayara bağlamak için WebApplicationFirewallPolicyLink özelliğini yalnızca bir kez ayarlamanız gerekir. Sonraki ilke güncelleştirmeleri otomatik olarak ön uç ana bilgisayara uygulanır.

## <a name="next-steps"></a>Sonraki adımlar
- Azure [web uygulaması güvenlik duvarı](waf-overview.md)hakkında bilgi edinin.
- [Front Door oluşturmayı](quickstart-create-front-door.md) öğrenin.
