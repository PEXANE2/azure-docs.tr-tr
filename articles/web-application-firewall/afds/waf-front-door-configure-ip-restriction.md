---
title: Azure Ön Kapı için IP kısıtlaması WAF kuralını yapılandırma
description: Varolan bir Azure Ön Kapı bitiş noktasının IP adreslerini kısıtlamak için Bir Web Uygulaması Güvenlik Duvarı kuralını nasıl yapılandırıştırmayı öğrenin.
services: web-application-firewall
author: vhorne
ms.service: web-application-firewall
ms.topic: article
ms.date: 03/26/2020
ms.author: tyao
ms.openlocfilehash: 077f127648688b25d45b433fa2bc94ee011b3f2d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80336104"
---
# <a name="configure-an-ip-restriction-rule-with-a-web-application-firewall-for-azure-front-door"></a>Azure Ön Kapı için Bir Web Uygulaması Güvenlik Duvarı ile IP kısıtlama kuralını yapılandırma

Bu makalede, Azure portalı, Azure CLI, Azure PowerShell veya Azure Kaynak Yöneticisi şablonu kullanarak Azure Ön Kapısı için Bir Web Uygulama Güvenlik Duvarı'nda (WAF) IP kısıtlama kurallarının nasıl yapılandırılabildiğiniz gösterilmektedir.

IP adresi tabanlı erişim denetimi kuralı, web uygulamalarınıza erişimi denetlemenize olanak tanıyan özel bir WAF kuralıdır. Bunu, Sınıfsız Etki Alanları Yönlendirme (CIDR) biçiminde IP adreslerinin veya IP adresi aralıklarının listesini belirterek yapar.

Varsayılan olarak, web uygulamanıza Internet'ten erişilebilir. İstemcilen IP adresleri veya IP adresi aralıkları listesinden istemcilere erişimi sınırlamak istiyorsanız, IP adreslerinin listesini eşleşen değerler olarak içeren ve işleçi "Not" (negate doğrudur) ve **Block'a**yönelik eylemi ayarlayan bir IP eşleştirme kuralı oluşturabilirsiniz. BIR IP kısıtlama kuralı uygulandıktan sonra, bu izin verilen listenin dışındaki adreslerden gelen istekler 403 Yasak yanıtı alır.

## <a name="configure-a-waf-policy-with-the-azure-portal"></a>Azure portalı ile bir WAF ilkesini yapılandırma

### <a name="prerequisites"></a>Ön koşullar

[Quickstart:](../../frontdoor/quickstart-create-front-door.md)Son derece kullanılabilir bir küresel web uygulaması için Bir Ön Kapı Oluşturma'da açıklanan yönergeleri izleyerek bir Azure Ön Kapı profili oluşturun.

### <a name="create-a-waf-policy"></a>WAF ilkesi oluşturma

1. Azure portalında kaynak oluştur'u, arama **kutusuna** **Web uygulaması güvenlik duvarı** yazın'ı ve ardından Web Uygulaması Güvenlik **Duvarı'nı (WAF)** seçin.
2. **Oluştur'u**seçin.
3. **WAF ilkesi oluştur** sayfasında Temel **Bilgiler** sekmesini tamamlamak için aşağıdaki değerleri kullanın:
   
   |Ayar  |Değer  |
   |---------|---------|
   |İlke için     |Global WAF (Ön Kapı)|
   |Abonelik     |Aboneliğinizi seçme|
   |Kaynak grubu     |Ön Kapınızın olduğu kaynak grubunu seçin.|
   |İlke adı     |İlkeniz için bir ad yazın|
   |İlke durumu     |Etkin|

   **Sonraki'ni Seçin: İlke ayarları**

1. **İlke ayarları** sekmesinde **Önleme'yi**seçin. Blok **yanıt gövdesi**için, türü *Engellendiniz!* böylece özel kuralınızın geçerli olduğunu görebilirsiniz.
2. **Sonraki'ni seçin: Yönetilen kurallar.**
3. **Sonraki'ni seçin: Özel kurallar.**
4. **Özel kural ekle'yi**seçin.
5. Özel **kural ekle** sayfasında, özel bir kural oluşturmak için aşağıdaki test değerlerini kullanın:

   |Ayar  |Değer  |
   |---------|---------|
   |Özel kural adı     |FdWafCustRule|
   |Durum     |Etkin|
   |Kural türü     |Eşleştirme|
   |Öncelik    |100|
   |Eşleştirme türü     |IP adresi|
   |Eşleştirme değişkeni|RemoteAddr|
   |İşlem|İçermez|
   |IP adresi veya aralığı|10.10.10.0/24|
   |Ardından|Trafiği reddet|

   :::image type="content" source="../media/waf-front-door-configure-ip-restriction/custom-rule.png" alt-text="Özel kural":::

   **Ekle'yi**seçin.
6. **Sonraki seçin: Ilişkilendirme**.
7. **Ön uç ana bilgisayar ekle'yi**seçin.
8. **Frontend ana bilgisayar için,** ön uç ana bilgisayar Seçin ve **Ekle**seçin.
9. **İncele ve oluştur**’u seçin.
10. İlke doğrulamanız geçtikten sonra **Oluştur'u**seçin.

### <a name="test-your-waf-policy"></a>WAF politikanızı test edin

1. WAF ilke dağıtımınız tamamlandıktan sonra, Ön Kapı ön yüz ana bilgisayar adınıza göz atın.
2. Özel engelleme iletinizi görmeniz gerekir.

   :::image type="content" source="../media/waf-front-door-configure-ip-restriction/waf-rule-test.png" alt-text="WAF kural testi":::

   > [!NOTE]
   > Özel bir IP adresi, kuralın tetiklemeyeceğini garanti etmek için özel kuralda kasıtlı olarak kullanılmıştır. Gerçek bir dağıtımda, özel durumunuz için IP adreslerini kullanarak *kurallara izin verin* ve *reddedin.*

## <a name="configure-a-waf-policy-with-the-azure-cli"></a>Azure CLI ile bir WAF ilkesini yapılandırma

### <a name="prerequisites"></a>Ön koşullar
Bir IP kısıtlama ilkesini yapılandırmaya başlamadan önce CLI ortamınızı ayarlayın ve bir Azure Ön Kapı profili oluşturun.

#### <a name="set-up-the-azure-cli-environment"></a>Azure CLI ortamını ayarlama
1. Azure [CLI'yi](/cli/azure/install-azure-cli)yükleyin veya Azure Bulut Kabuğu'nu kullanın. Azure Cloud Shell doğrudan Azure portalının içinde çalıştırabileceğiniz ücretsiz bir Bash kabuğudur. Azure CLI, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır. Bunu izleyen CLI komutlarında **Try it** düğmesini seçin ve ardından açılan Bulut Kabuğu oturumunda Azure hesabınızda oturum açın. Oturum başladıktan sonra `az extension add --name front-door` Azure Ön Kapı uzantısını eklemek için girin.
 2. CLI'yi Bash'te yerel olarak kullanıyorsanız, kullanarak `az login`Azure'da oturum açın.

#### <a name="create-an-azure-front-door-profile"></a>Azure Ön Kapı profili oluşturma
[Quickstart:](../../frontdoor/quickstart-create-front-door.md)Son derece kullanılabilir bir küresel web uygulaması için Bir Ön Kapı Oluşturma'da açıklanan yönergeleri izleyerek bir Azure Ön Kapı profili oluşturun.

### <a name="create-a-waf-policy"></a>WAF ilkesi oluşturma

Az ağ ön kapı [waf-ilke oluşturma](/cli/azure/ext/front-door/network/front-door/waf-policy?view=azure-cli-latest#ext-front-door-az-network-front-door-waf-policy-create) komutunu kullanarak bir WAF ilkesi oluşturun. Aşağıdaki örnekte, *IPAllowPolicyExampleCLI* ilke adını benzersiz bir ilke adı ile değiştirin.

```azurecli-interactive 
az network front-door waf-policy create \
  --resource-group <resource-group-name> \
  --subscription <subscription ID> \
  --name IPAllowPolicyExampleCLI
  ```
### <a name="add-a-custom-ip-access-control-rule"></a>Özel IP erişim denetimi kuralı ekleme

Az ağ [ön kapı waf-ilke özel kural oluşturmak](/cli/azure/ext/front-door/network/front-door/waf-policy/rule?view=azure-cli-latest#ext-front-door-az-network-front-door-waf-policy-rule-create) komutu kullanarak waf ilkesi için özel bir IP erişim denetimi kuralı ekleyin.

Aşağıdaki örneklerde:
-  *IPAllowPolicyExampleCLI'yi* daha önce oluşturulmuş benzersiz ilkeniz ile değiştirin.
-  *ip-address-range-1*, *ip-address-range-2'yi* kendi aralığınızla değiştirin.

İlk olarak, önceki adımdan oluşturulan ilke için BIR IP izin kuralı oluşturun. 
> [!NOTE]
> **--bir** kuralın bir sonraki adımda eklenecek bir eşleşme koşulu olması gerektiğinden erteleme gereklidir.

```azurecli
az network front-door waf-policy rule create \
  --name IPAllowListRule \
  --priority 1 \
  --rule-type MatchRule \
  --action Block \
  --resource-group <resource-group-name> \
  --policy-name IPAllowPolicyExampleCLI --defer
```
Ardından, kurala eşleşme koşulu ekleyin:

```azurecli
az network front-door waf-policy rule match-condition add \
--match-variable RemoteAddr \
--operator IPMatch \
--values "ip-address-range-1" "ip-address-range-2" \
--negate true \
--name IPAllowListRule \
  --resource-group <resource-group-name> \
  --policy-name IPAllowPolicyExampleCLI 
  ```
                                                   
### <a name="find-the-id-of-a-waf-policy"></a>WAF ilkesinin kimliğini bulma 
[Az ağı ön kapı waf-policy show](/cli/azure/ext/front-door/network/front-door/waf-policy?view=azure-cli-latest#ext-front-door-az-network-front-door-waf-policy-show) komutunu kullanarak bir WAF ilkesinin kimliğini bulun. Aşağıdaki örnekte *IPAllowPolicyExampleCLI'yi* daha önce oluşturduğunuz benzersiz ilkeyle değiştirin.

   ```azurecli
   az network front-door  waf-policy show \
     --resource-group <resource-group-name> \
     --name IPAllowPolicyExampleCLI
   ```

### <a name="link-a-waf-policy-to-an-azure-front-door-front-end-host"></a>WAF ilkesini Azure Ön Kapı ön uç ana bilgisayara bağlama

[Az ağı ön kapı güncelleştirme](/cli/azure/ext/front-door/network/front-door?view=azure-cli-latest#ext-front-door-az-network-front-door-update) komutunu kullanarak Azure Ön Kapı *WebApplicationFirewallPolicyLink* Kimliğini ilke kimliğine ayarlayın. *IPAllowPolicyExampleCLI'yi* daha önce oluşturduğunuz benzersiz ilkeyle değiştirin.

   ```azurecli
   az network front-door update \
     --set FrontendEndpoints[0].WebApplicationFirewallPolicyLink.id=/subscriptions/<subscription ID>/resourcegroups/resource-group-name/providers/Microsoft.Network/frontdoorwebapplicationfirewallpolicies/IPAllowPolicyExampleCLI \
     --name <frontdoor-name>
     --resource-group <resource-group-name>
   ```
Bu örnekte, WAF ilkesi **FrontendEndpoints[0]** uygulanır. WAF ilkesini ön uçlarınızdan herhangi biri ile ilişkilendirebilirsiniz.
> [!Note]
> WAF ilkesini Azure Ön Kapı ön ucuna bağlamak için **WebApplicationFirewallPolicyLink** özelliğini yalnızca bir kez ayarlamanız gerekir. Sonraki ilke güncelleştirmeleri otomatik olarak ön uca uygulanır.

## <a name="configure-a-waf-policy-with-azure-powershell"></a>Azure PowerShell ile BIR WAF ilkesini yapılandırma

### <a name="prerequisites"></a>Ön koşullar
Bir IP kısıtlama ilkesini yapılandırmaya başlamadan önce PowerShell ortamınızı ayarlayın ve bir Azure Ön Kapı profili oluşturun.

#### <a name="set-up-your-powershell-environment"></a>PowerShell ortamınızı hazırlama
Azure PowerShell, Azure kaynaklarını yönetmek için [Azure Kaynak Yöneticisi](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) modelini kullanan bir cmdlet kümesi sağlar.

[Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview)'i yerel makinenize yükleyebilir ve herhangi bir PowerShell oturumunda kullanabilirsiniz. Azure kimlik bilgilerinizi kullanarak PowerShell'de oturum açmanız için sayfadaki yönergeleri izleyin ve ardından Az modülünüzü yükleyin.

1. Aşağıdaki komutu kullanarak Azure'a bağlanın ve oturum açmak için etkileşimli bir iletişim kutusu kullanın.
    ```
    Connect-AzAccount
    ```
 2. Azure Ön Kapı modüllerini yüklemeden önce PowerShellGet modülünün geçerli sürümünün yüklü olduğundan emin olun. Aşağıdaki komutu çalıştırın ve PowerShell'i yeniden açın.

    ```
    Install-Module PowerShellGet -Force -AllowClobber
    ``` 

3. Aşağıdaki komutu kullanarak Az.FrontDoor modüllerini yükleyin. 
    
    ```
    Install-Module -Name Az.FrontDoor
    ```
### <a name="create-an-azure-front-door-profile"></a>Azure Ön Kapı profili oluşturma
[Quickstart:](../../frontdoor/quickstart-create-front-door.md)Son derece kullanılabilir bir küresel web uygulaması için Bir Ön Kapı Oluşturma'da açıklanan yönergeleri izleyerek bir Azure Ön Kapı profili oluşturun.

### <a name="define-an-ip-match-condition"></a>IP eşleşmesi koşulunu tanımlama
Bir IP eşleşmesi koşulu tanımlamak için [New-AzFrontDoorWafMatchConditionObject](/powershell/module/az.frontdoor/new-azfrontdoorwafmatchconditionobject) komutunu kullanın.
Aşağıdaki örnekte *ip-address-range-1*, *ip-address-range-2'yi* kendi aralığınızla değiştirin.    
```powershell
$IPMatchCondition = New-AzFrontDoorWafMatchConditionObject `
-MatchVariable  RemoteAddr `
-OperatorProperty IPMatch `
-MatchValue "ip-address-range-1", "ip-address-range-2"
-NegateCondition 1
```
     
### <a name="create-a-custom-ip-allow-rule"></a>Özel bir IP izin kuralı oluşturma

Bir eylemi tanımlamak ve öncelik belirlemek için [New-AzFrontDoorWafCustomRuleObject](/powershell/module/Az.FrontDoor/New-azfrontdoorwafcustomruleobject) komutunu kullanın. Aşağıdaki örnekte, listeyle eşleşen istemci IP'lerden gelen istekler engellenir.

```azurepowershell
$IPAllowRule = New-AzFrontDoorWafCustomRuleObject `
-Name "IPAllowRule" `
-RuleType MatchRule `
-MatchCondition $IPMatchCondition `
-Action Block -Priority 1
```

### <a name="configure-a-waf-policy"></a>WAF ilkesini yapılandırma
Azure Ön Kapı profilini içeren kaynak grubunun adını `Get-AzResourceGroup`kullanarak bulun. Ardından, [New-AzFrontDoorWafPolicy](/powershell/module/az.frontdoor/new-azfrontdoorwafpolicy)kullanarak IP kuralı ile bir WAF ilkesi ni yapılandırın.

```azurepowershell
  $IPAllowPolicyExamplePS = New-AzFrontDoorWafPolicy `
    -Name "IPRestrictionExamplePS" `
    -resourceGroupName <resource-group-name> `
    -Customrule $IPAllowRule`
    -Mode Prevention `
    -EnabledState Enabled
   ```

### <a name="link-a-waf-policy-to-an-azure-front-door-front-end-host"></a>WAF ilkesini Azure Ön Kapı ön uç ana bilgisayara bağlama

WAF ilkesi nesnesini varolan bir ön uç ana bilgisayara bağla ve Azure Ön Kapı özelliklerini güncelleştir. İlk olarak, [Get-AzFrontDoor'u](/powershell/module/Az.FrontDoor/Get-AzFrontDoor)kullanarak Azure Ön Kapı nesnesini alın. Ardından, **WebApplicationFirewallPolicyLink** özelliğini, [Set-AzFrontDoor](/powershell/module/Az.FrontDoor/Set-AzFrontDoor) komutunu kullanarak önceki adımda oluşturulan *$IPAllowPolicyExamplePS*kaynak kimliğine ayarlayın.

```azurepowershell
  $FrontDoorObjectExample = Get-AzFrontDoor `
    -ResourceGroupName <resource-group-name> `
    -Name $frontDoorName
  $FrontDoorObjectExample[0].FrontendEndpoints[0].WebApplicationFirewallPolicyLink = $IPBlockPolicy.Id
  Set-AzFrontDoor -InputObject $FrontDoorObjectExample[0]
```

> [!NOTE]
> Bu örnekte, WAF ilkesi **FrontendEndpoints[0]** uygulanır. WAF ilkesini ön uçlarınızdan herhangi biri ile ilişkilendirebilirsiniz. WAF ilkesini Azure Ön Kapı ön ucuna bağlamak için **WebApplicationFirewallPolicyLink** özelliğini yalnızca bir kez ayarlamanız gerekir. Sonraki ilke güncelleştirmeleri otomatik olarak ön uca uygulanır.


## <a name="configure-a-waf-policy-with-a-resource-manager-template"></a>Kaynak Yöneticisi şablonuyla WAF ilkesini yapılandırma
Azure Ön Kapı ilkesi ve özel IP kısıtlama kurallarıiçeren bir WAF ilkesi oluşturan şablonu görüntülemek için [GitHub'a](https://github.com/Azure/azure-quickstart-templates/tree/master/201-front-door-waf-clientip)gidin.


## <a name="next-steps"></a>Sonraki adımlar

- Azure Ön Kapı profilini nasıl [oluşturabilirsiniz](../../frontdoor/quickstart-create-front-door.md)öğrenin.
