---
title: Azure ön kapı hizmeti için bir Web uygulaması güvenlik duvarı kuralıyla bir IP kısıtlama kuralı yapılandırma
description: Mevcut bir Azure ön kapısı hizmeti uç noktası için IP adreslerini kısıtlamak üzere bir Web uygulaması güvenlik duvarı kuralı yapılandırma hakkında bilgi edinin.
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
ms.openlocfilehash: a610a2c01a1e935c55942b621e5b3799cb002fc0
ms.sourcegitcommit: 800f961318021ce920ecd423ff427e69cbe43a54
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2019
ms.locfileid: "68698638"
---
# <a name="configure-an-ip-restriction-rule-with-a-web-application-firewall-for-azure-front-door-service"></a>Azure ön kapı hizmeti için Web uygulaması güvenlik duvarıyla bir IP kısıtlama kuralı yapılandırma
Bu makalede, Azure CLı, Azure PowerShell veya bir Azure Resource Manager şablonu kullanarak Azure ön kapısı hizmeti için bir Web uygulaması güvenlik duvarında (WAF) IP kısıtlama kurallarını yapılandırma işlemi gösterilmektedir.

IP adresi tabanlı erişim denetimi kuralı, Web uygulamalarınıza erişimi denetlemenize olanak tanıyan özel bir WAF kuralıdır. Bu, sınıfsız etki alanları arası yönlendirme (CıDR) biçimindeki IP adresleri veya IP adresi aralıklarının bir listesini belirterek yapar.

Varsayılan olarak, Web uygulamanıza internet 'ten erişilebilir. İstemcilerle erişimi bilinen IP adresleri veya IP adresi aralıkları listesinden sınırlandırmak istiyorsanız, iki IP eşleştirme kuralı oluşturmanız gerekir. İlk IP eşleştirme kuralı, IP adreslerinin listesini eşleşen değerler olarak içerir ve eylemi **Izin ver**olarak ayarlar. İkinci bir, daha düşük önceliğe sahip diğer tüm IP adreslerini, **All** işlecini kullanarak engeller ve eylemini **engelleyecek**şekilde ayarlar. Bir IP kısıtlama kuralı uygulandıktan sonra, bu izin verilen listenin dışındaki adreslerden kaynaklanan istekler 403 yasaklanmış bir yanıt alır.  

## <a name="configure-a-waf-policy-with-the-azure-cli"></a>Azure CLı ile bir WAF ilkesi yapılandırma

### <a name="prerequisites"></a>Önkoşullar
Bir IP kısıtlama ilkesini yapılandırmaya başlamadan önce, CLı ortamınızı ayarlayın ve bir Azure ön kapı hizmeti profili oluşturun.

#### <a name="set-up-the-azure-cli-environment"></a>Azure CLı ortamını ayarlama
1. [Azure CLI](/cli/azure/install-azure-cli)'yi veya Azure Cloud Shell kullanın. Azure Cloud Shell doğrudan Azure portalının içinde çalıştırabileceğiniz ücretsiz bir Bash kabuğudur. Azure CLI, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır. Aşağıdaki CLı komutlarında **deneyin** düğmesini seçin ve ardından açılan Cloud Shell oturumunda Azure hesabınızda oturum açın. Oturum başladıktan sonra Azure ön kapı `az extension add --name front-door` hizmeti uzantısını eklemek için girin.
 2. CLı 'yi Bash içinde yerel olarak kullanıyorsanız, kullanarak `az login`Azure 'da oturum açın.

#### <a name="create-an-azure-front-door-service-profile"></a>Azure ön kapı hizmeti profili oluşturma
Hızlı Başlangıç bölümünde [açıklanan yönergeleri izleyerek bir Azure ön kapı hizmeti profili oluşturun: Yüksek oranda kullanılabilir Küresel Web uygulaması](quickstart-create-front-door.md)Için bir ön kapı oluşturun.

### <a name="create-a-waf-policy"></a>WAF ilkesi oluşturma

[Az Network WAF-Policy Create](/cli/azure/ext/front-door/network/front-door/waf-policy?view=azure-cli-latest#ext-front-door-az-network-front-door-waf-policy-create) komutunu kullanarak bir WAF ilkesi oluşturun. Aşağıdaki örnekte, *ıpallowpolicyexampleclı* ilke adı ' nı benzersiz bir ilke adıyla değiştirin.

```azurecli-interactive 
az network waf-policy create \
  --resource-group <resource-group-name> \
  --subscription <subscription ID> \
  --name IPAllowPolicyExampleCLI
  ```
### <a name="add-a-custom-ip-access-control-rule"></a>Özel bir IP erişim denetimi kuralı ekleme

Az [Network WAF-Policy Custom-Rule Create](/cli/azure/ext/front-door/network/front-door/waf-policy/rule?view=azure-cli-latest#ext-front-door-az-network-front-door-waf-policy-rule-create) komutunu kullanarak yeni oluşturduğunuz WAF ilkesi için özel bir IP erişim denetimi kuralı ekleyin.

Aşağıdaki örneklerde:
-  *Ipallowpolicyexampleclı* öğesini daha önce oluşturduğunuz benzersiz ilkenize değiştirin.
-  *IP adresi-aralığı-1*, *IP-adres-aralığı-2* ' yi kendi aralığınızla değiştirin.

İlk olarak, belirtilen adresler için IP izin verme kuralını oluşturun.

```azurecli
az network waf-policy custom-rule create \
  --name IPAllowListRule \
  --priority 1 \
  --rule-type MatchRule \
  --match-condition RemoteAddr IPMatch ("<ip-address-range-1>","<ip-address-range-2>") \
  --action Allow \
  --resource-group <resource-group-name> \
  --policy-name IPAllowPolicyExampleCLI
```
Ardından, önceki **izin verme** kuralından daha düşük önceliğe sahip bir **blok All** kuralı oluşturun. Daha önce oluşturduğunuz benzersiz ilkenize aşağıdaki örnekteki *ıpallowpolicyexampleclı* öğesini de değiştirin.

```azurecli
az network waf-policy custom-rule create \
  --name IPDenyAllRule\
  --priority 2 \
  --rule-type MatchRule \
  --match-condition RemoteAddr Any
  --action Block \
  --resource-group <resource-group-name> \
  --policy-name IPAllowPolicyExampleCLI
```
    
### <a name="find-the-id-of-a-waf-policy"></a>Bir WAF ilkesinin KIMLIĞINI bulma 
[Az Network WAF-Policy Show](/cli/azure/ext/front-door/network/front-door/waf-policy?view=azure-cli-latest#ext-front-door-az-network-front-door-waf-policy-show) komutunu kullanarak bir WAF ilkesinin kimliğini bulun. Aşağıdaki örnekteki *ıpallowpolicyexampleclı* değerini, daha önce oluşturduğunuz benzersiz ilkenize göre değiştirin.

   ```azurecli
   az network waf-policy show \
     --resource-group <resource-group-name> \
     --name IPAllowPolicyExampleCLI
   ```

### <a name="link-a-waf-policy-to-an-azure-front-door-service-front-end-host"></a>Bir WAF ilkesini bir Azure ön kapısı hizmeti ön uç konağına bağlama

[Az Network ön kapı Update](/cli/azure/ext/front-door/network/front-door?view=azure-cli-latest#ext-front-door-az-network-front-door-update) komutunu kullanarak Azure ön kapı hizmeti *Webapplicationfirewallpolicylink* kimliğini ilke kimliğine ayarlayın. *Ipallowpolicyexampleclı* öğesini daha önce oluşturduğunuz benzersiz ilkenize değiştirin.

   ```azurecli
   az network front-door update \
     --set FrontendEndpoints[0].WebApplicationFirewallPolicyLink.id=/subscriptions/<subscription ID>/resourcegroups/<resource- name>/providers/Microsoft.Network/frontdoorwebapplicationfirewallpolicies/IPAllowPolicyExampleCLI \
     --name <frontdoor-name>
     --resource-group <resource-group-name>
   ```
Bu örnekte, WAF ilkesi **Frontendendpoints [0]** öğesine uygulanır. WAF ilkesini ön uçlarınızın herhangi birine bağlayabilirsiniz.
> [!Note]
> Bir WAF ilkesini bir Azure ön kapı hizmeti ön ucuna bağlamak için **Webapplicationfirewallpolicylink** özelliğini yalnızca bir kez ayarlamanız gerekir. Sonraki ilke güncelleştirmeleri ön uca otomatik olarak uygulanır.

## <a name="configure-a-waf-policy-with-azure-powershell"></a>Azure PowerShell bir WAF ilkesi yapılandırma

### <a name="prerequisites"></a>Önkoşullar
Bir IP kısıtlama ilkesini yapılandırmaya başlamadan önce, PowerShell ortamınızı ayarlayın ve bir Azure ön kapı hizmeti profili oluşturun.

#### <a name="set-up-your-powershell-environment"></a>PowerShell ortamınızı hazırlama
Azure PowerShell, Azure kaynaklarını yönetmek için [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) modelini kullanan bir cmdlet kümesi sağlar.

[Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview)'i yerel makinenize yükleyebilir ve herhangi bir PowerShell oturumunda kullanabilirsiniz. Azure kimlik bilgilerinizi kullanarak PowerShell 'de oturum açmak için sayfasındaki yönergeleri izleyin ve sonra az Module ' ü kullanın.

1. Aşağıdaki komutu kullanarak Azure 'a bağlanın ve ardından oturum açmak için etkileşimli bir iletişim kutusu kullanın.
    ```
    Connect-AzAccount
    ```
 2. Bir Azure ön kapı hizmeti modülünü yüklemeden önce, PowerShellGet modülünün güncel sürümünün yüklü olduğundan emin olun. Aşağıdaki komutu çalıştırın ve ardından PowerShell 'i yeniden açın.

    ```
    Install-Module PowerShellGet -Force -AllowClobber
    ``` 

3. Aşağıdaki komutu kullanarak az. Frontkapısı modülünü yükler. 
    
    ```
    Install-Module -Name Az.FrontDoor
    ```
### <a name="create-an-azure-front-door-service-profile"></a>Azure ön kapı hizmeti profili oluşturma
Hızlı Başlangıç bölümünde [açıklanan yönergeleri izleyerek bir Azure ön kapı hizmeti profili oluşturun: Yüksek oranda kullanılabilir Küresel Web uygulaması](quickstart-create-front-door.md)Için bir ön kapı oluşturun.

### <a name="define-an-ip-match-condition"></a>Bir IP eşleştirme koşulu tanımlayın
Bir IP eşleştirme koşulu tanımlamak için [New-AzFrontDoorWafMatchConditionObject](/powershell/module/az.frontdoor/new-azfrontdoorwafmatchconditionobject) komutunu kullanın.
Aşağıdaki örnekte, *IP adresi-aralığı-1*, *IP-adres-aralığı-2* ' yi kendi aralığınızla değiştirin.    
```powershell
$IPMatchCondition = New-AzFrontDoorWafMatchConditionObject `
-MatchVariable  RemoteAddr `
-OperatorProperty IPMatch `
-MatchValue "ip-address-range-1", "ip-address-range-2"
```
Aşağıdaki komutu kullanarak bir IP *eşleşmesi tüm koşul* kuralı oluşturun:
```powershell
$IPMatchALlCondition = New-AzFrontDoorWafMatchConditionObject `
-MatchVariable  RemoteAddr `
-OperatorProperty Any        
  ```
    
### <a name="create-a-custom-ip-allow-rule"></a>Özel IP izin verme kuralı oluşturma

Bir eylem tanımlamak ve öncelik ayarlamak için [New-AzFrontDoorCustomRuleObject](/powershell/module/Az.FrontDoor/New-azfrontdoorwafcustomruleobject) komutunu kullanın. Aşağıdaki örnekte, listeyle eşleşen istemci IP 'Lerinin isteklerine izin verilir.

```powershell
$IPAllowRule = New-AzFrontDoorCustomRuleObject `
-Name "IPAllowRule" `
-RuleType MatchRule `
-MatchCondition $IPMatchCondition `
-Action Allow -Priority 1
```
Önceki IP **izin verme** kuralından daha düşük önceliğe sahip bir **blok All** kuralı oluşturun.
```powershell
$IPBlockAll = New-AzFrontDoorCustomRuleObject `
-Name "IPDenyAll" `
-RuleType MatchRule `
-MatchCondition $IPMatchALlCondition `
-Action Block `
-Priority 2
```

### <a name="configure-a-waf-policy"></a>WAF ilkesini yapılandırma
Kullanarak `Get-AzResourceGroup`Azure ön kapı hizmeti profilini içeren kaynak grubunun adını bulun. Ardından, [New-AzFrontDoorWafPolicy](/powershell/module/az.frontdoor/new-azfrontdoorwafpolicy)kullanarak bir WAF ilkesini IP **blok All** kuralıyla yapılandırın.

```powershell
  $IPAllowPolicyExamplePS = New-AzFrontDoorWafPolicy `
    -Name "IPRestrictionExamplePS" `
    -resourceGroupName <resource-group-name> `
    -Customrule $IPAllowRule $IPBlockAll `
    -Mode Prevention `
    -EnabledState Enabled
   ```

### <a name="link-a-waf-policy-to-an-azure-front-door-service-front-end-host"></a>Bir WAF ilkesini bir Azure ön kapısı hizmeti ön uç konağına bağlama

Bir WAF ilke nesnesini mevcut bir ön uç konağına bağlayın ve Azure ön kapı hizmeti özelliklerini güncelleştirin. İlk olarak [Get-Azfrontkapısı](/powershell/module/Az.FrontDoor/Get-AzFrontDoor)kullanarak Azure ön kapı hizmeti nesnesini alın. Ardından, **Webapplicationfirewallpolicylink** özelliğini, önceki adımda oluşturulan *$IPAllowPolicyExamplePS*kaynak kimliği olarak ayarlayın, [set-azfrontkapısı](/powershell/module/Az.FrontDoor/Set-AzFrontDoor) komutunu kullanın.

```powershell
  $FrontDoorObjectExample = Get-AzFrontDoor `
    -ResourceGroupName <resource-group-name> `
    -Name $frontDoorName
  $FrontDoorObjectExample[0].FrontendEndpoints[0].WebApplicationFirewallPolicyLink = $IPBlockPolicy.Id
  Set-AzFrontDoor -InputObject $FrontDoorObjectExample[0]
```

> [!NOTE]
> Bu örnekte, WAF ilkesi **Frontendendpoints [0]** öğesine uygulanır. Bir WAF ilkesini, ön uçlarınızın herhangi birine bağlayabilirsiniz. Bir WAF ilkesini bir Azure ön kapı hizmeti ön ucuna bağlamak için **Webapplicationfirewallpolicylink** özelliğini yalnızca bir kez ayarlamanız gerekir. Sonraki ilke güncelleştirmeleri ön uca otomatik olarak uygulanır.


## <a name="configure-a-waf-policy-with-a-resource-manager-template"></a>Kaynak Yöneticisi şablonuyla bir WAF ilkesi yapılandırma
Bir Azure ön kapısı hizmet ilkesi ve özel IP kısıtlama kuralları içeren bir WAF ilkesi oluşturan şablonu görüntülemek için [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/201-front-door-waf-clientip)' a gidin.


## <a name="next-steps"></a>Sonraki adımlar

- [Azure ön kapı hizmeti profili oluşturmayı](quickstart-create-front-door.md)öğrenin.
