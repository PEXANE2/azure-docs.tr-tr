---
title: Öğretici-coğrafi filtreleme WAF ilkesini Yapılandırma-Azure ön kapısı
description: Bu öğreticide, coğrafi filtreleme ilkesi oluşturmayı ve ilkeyi mevcut ön kapılı ön uç konunuzla ilişkilendirmeyi öğreneceksiniz.
services: frontdoor
documentationcenter: ''
author: duongau
ms.service: frontdoor
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 09/14/2020
ms.author: duau
ms.openlocfilehash: 20aa038e15b1ae5734ad6f463c6f450368617119
ms.sourcegitcommit: 07166a1ff8bd23f5e1c49d4fd12badbca5ebd19c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/15/2020
ms.locfileid: "90090048"
---
# <a name="tutorial-how-to-set-up-a-geo-filtering-waf-policy-for-your-front-door"></a>Öğretici: ön kapılarınız için coğrafi filtreleme WAF ilkesini ayarlama
Bu öğreticide, örnek bir coğrafi filtreleme ilkesi oluşturmak ve bu ilkeyi mevcut bir Front Door ön uç konağı ile ilişkilendirmek için Azure PowerShell kullanma gösterilmektedir. Bu örnek coğrafi filtreleme ilkesi, Birleşik Devletler dışındaki diğer tüm ülke/bölgelerden gelen istekleri engeller.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:
> [!div class="checklist"]
> - Coğrafi filtreleme eşleştirme koşulunu tanımlayın.
> - Bir kurala coğrafi filtreleme eşleştirme koşulu ekleyin.
> - İlkeye kural ekleyin.
> - WAF ilkesini Frontkapılı ön uç konağına bağlayın.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Önkoşullar
* Bir coğrafi filtre İlkesi ayarlamaya başlamadan önce, PowerShell ortamınızı ayarlayın ve bir ön kapı profili oluşturun.
* [Hızlı başlangıç: ön kapı profili oluşturma](quickstart-create-front-door.md)bölümünde açıklanan yönergeleri Izleyerek ön kapı oluşturun.

## <a name="define-geo-filtering-match-condition"></a>Coğrafi filtreleme eşleştirme koşulunu tanımlayın

Bir eşleşme koşulu oluştururken [New-AzFrontDoorWafMatchConditionObject](/powershell/module/az.frontdoor/new-azfrontdoorwafmatchconditionobject) on Parameters KULLANıLARAK "ABD" den gelenlerin kullanıldığı istekleri seçen bir örnek eşleşme koşulu oluşturun. Ülke/bölge eşlemesine yönelik iki harfli ülke/bölge kodu [burada](front-door-geo-filtering.md)verilmiştir.

```azurepowershell-interactive
$nonUSGeoMatchCondition = New-AzFrontDoorWafMatchConditionObject `
-MatchVariable RemoteAddr `
-OperatorProperty GeoMatch `
-NegateCondition $true `
-MatchValue "US"
```
## <a name="add-geo-filtering-match-condition-to-a-rule-with-action-and-priority"></a>Eylem ve Öncelik ile bir kurala coğrafi filtreleme eşleşme koşulu ekleme

`nonUSBlockRule` [New-AzFrontDoorWafCustomRuleObject](/powershell/module/az.frontdoor/new-azfrontdoorwafcustomruleobject)kullanarak eşleşme koşuluna, eyleme ve önceliğe göre bir customrule nesnesi oluşturun.  Bir CustomRule'un birden fazla MatchCondition'ı olabilir.  Bu örnekte Eylem Engelle değerine, Öncelik ise en yüksek öncelik olan 1 değerine ayarlanmıştır.

```
$nonUSBlockRule = New-AzFrontDoorWafCustomRuleObject `
-Name "geoFilterRule" `
-RuleType MatchRule `
-MatchCondition $nonUSGeoMatchCondition `
-Action Block `
-Priority 1
```
## <a name="add-rules-to-a-policy"></a>İlkeye kural ekleme
Kullanarak ön kapı profilini içeren kaynak grubunun adını bulun `Get-AzResourceGroup` . Ardından, `geoPolicy` `nonUSBlockRule`  ön kapı profilini içeren belirtilen kaynak grubunda [New-AzFrontDoorWafPolicy](/powershell/module/az.frontdoor/new-azfrontdoorwafpolicy) kullanarak içeren bir ilke nesnesi oluşturun. Coğrafi filtreleme ilkesi için benzersiz bir ad sağlamanız gerekir. 

Aşağıdaki örnek, [hızlı başlangıç: ön kapı oluşturma](quickstart-create-front-door.md) makalesinde sunulan yönergeleri kullanarak ön kapı profilini oluşturduğunuz varsayımıyla *FrontDoorQS_rg0* kaynak grubu adını kullanır. Aşağıdaki örnekte, *Geopolicyallowusname* Ilke adını yalnızca benzersiz bir ilke adıyla değiştirin.

```
$geoPolicy = New-AzFrontDoorWafPolicy `
-Name "geoPolicyAllowUSOnly" `
-resourceGroupName FrontDoorQS_rg0 `
-Customrule $nonUSBlockRule  `
-Mode Prevention `
-EnabledState Enabled
```
## <a name="link-waf-policy-to-a-front-door-frontend-host"></a>WAF ilkesini bir ön kapılı ön uç konağına bağlama
WAF ilke nesnesini mevcut ön kapı ön uç konağına bağlayın ve ön kapı özelliklerini güncelleştirin. 

Bunu yapmak için ilk olarak [Get-Azfrontkapısı](/powershell/module/az.frontdoor/get-azfrontdoor)kullanarak ön kapı nesnenizin alınması gerekir. 

```
$geoFrontDoorObjectExample = Get-AzFrontDoor -ResourceGroupName FrontDoorQS_rg0
$geoFrontDoorObjectExample[0].FrontendEndpoints[0].WebApplicationFirewallPolicyLink = $geoPolicy.Id
```
Ardından, ön uç WebApplicationFirewallPolicyLink özelliğini, `geoPolicy` [set-Azfrontkapısının](/powershell/module/az.frontdoor/set-azfrontdoor)kullanıldığı RESOURCEID olarak ayarlayın.

```
Set-AzFrontDoor -InputObject $geoFrontDoorObjectExample[0]
```

> [!NOTE] 
> Bir WAF ilkesini bir ön kapılı ön uç konağına bağlamak için yalnızca WebApplicationFirewallPolicyLink özelliğini bir kez ayarlamanız gerekir. Sonraki ilke güncelleştirmeleri, ön uç konağına otomatik olarak uygulanır.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Yukarıdaki adımlarda, bir WAF ilkesiyle ilişkili bir coğrafi filtreleme kuralı yapılandırdınız. Ardından ilkeyi ön kapılarınızın ön uç konağına bağladınız. Artık coğrafi filtreleme kuralına veya WAF ilkesine ihtiyacınız yoksa, WAF ilkesi silinmeden önce ilkenin ön uç konağından ilişkilendirmesini kaldırmanız gerekir.

:::image type="content" source="media/front-door-geo-filtering/front-door-disassociate-policy.png" alt-text="WAF ilkesinin ilişkisini kaldır":::

## <a name="next-steps"></a>Sonraki adımlar

Ön kapılarınız için bir Web uygulaması güvenlik duvarını yapılandırma hakkında bilgi edinmek için sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [Web Uygulaması Güvenlik Duvarı ve Front Door](front-door-waf.md)
