---
title: Programlı olarak ilkeler oluşturma
description: Bu makale, Azure CLI, Azure PowerShell ve REST API ile Azure İlkesi için programlı olarak ilkeler oluşturma ve yönetme konusunda size yol sunar.
ms.date: 01/31/2019
ms.topic: how-to
ms.openlocfilehash: 08ed43a464d1dd7de8220428dbc1c61ce9fc3ad6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79264550"
---
# <a name="programmatically-create-policies"></a>Programlı olarak ilkeler oluşturma

Bu makale, programlı bir şekilde ilkeler oluşturma ve yönetme yoluyla size yol eder. Azure İlkesi tanımları, kaynaklarınız üzerinde farklı kurallar ve etkiler uygular. Uygulama, kaynakların kurumsal standartlarınızla ve hizmet düzeyi anlaşmalarınızla uyumlu kalmasını sağlar.

Uyumluluk hakkında bilgi için [bkz.](get-compliance-data.md)

## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce, aşağıdaki ön koşulların karşılandığınızdan emin olun:

1. Henüz yapmadıysanız [ARMClient](https://github.com/projectkudu/ARMClient)’ı yükleyin. Bu, HTTP isteklerini Azure Resource Manager tabanlı API’lere gönderen bir araçtır.

1. Azure PowerShell modülünüzü en son sürüme güncelleyin. Ayrıntılı bilgi için [Azure PowerShell modüllerini yükle'ye](/powershell/azure/install-az-ps) bakın. En son sürüm hakkında daha fazla bilgi için [Azure PowerShell'e](https://github.com/Azure/azure-powershell/releases)bakın.

1. Aboneliğinizin kaynak sağlayıcıyla çalıştığını doğrulamak için Azure PowerShell'i kullanarak Azure İlkesi Öngörüleri kaynak sağlayıcısına kaydolun. Bir kaynak sağlayıcısı kaydetmek için kaynak sağlayıcısı için kayıt eylem işlemini çalıştırmak için izniniz olması gerekir. Bu işlem, Katkıda Bulunan ve Sahip rolleriyle birlikte sunulur. Aşağıdaki komutu çalıştırarak kaynak sağlayıcısını kaydedin:

   ```azurepowershell-interactive
   Register-AzResourceProvider -ProviderNamespace 'Microsoft.PolicyInsights'
   ```

   Kaynak sağlayıcılarını kaydetme ve görüntüleme hakkında daha fazla bilgi için [kaynak sağlayıcıları ve Türleri'ne](../../../azure-resource-manager/management/resource-providers-and-types.md)bakın.

1. Henüz yapmadıysanız Azure CLI'yi yükleyin. [Windows'da Azure CLI yükle'den](/cli/azure/install-azure-cli-windows)en son sürümü alabilirsiniz.

## <a name="create-and-assign-a-policy-definition"></a>İlke tanımı oluşturma ve atama

Kaynaklarınızın daha iyi görünür olması için ilk adım, kaynaklarınız üzerinde ilkeler oluşturmak ve atamaktır. Bir sonraki adım, programlı bir ilke oluşturmayı ve atamayı öğrenmektir. Örnek ilke, PowerShell, Azure CLI ve HTTP isteklerini kullanarak tüm ortak ağlara açık depolama hesaplarını denetler.

### <a name="create-and-assign-a-policy-definition-with-powershell"></a>PowerShell ile bir ilke tanımı oluşturma ve atama

1. AuditStorageAccounts.json adını içeren bir JSON dosyası oluşturmak için aşağıdaki JSON snippet'i kullanın.

   ```json
   {
       "if": {
           "allOf": [{
                   "field": "type",
                   "equals": "Microsoft.Storage/storageAccounts"
               },
               {
                   "field": "Microsoft.Storage/storageAccounts/networkAcls.defaultAction",
                   "equals": "Allow"
               }
           ]
       },
       "then": {
           "effect": "audit"
       }
   }
   ```

   İlke tanımı yazma hakkında daha fazla bilgi için [Azure İlkesi Tanım Yapısı'na](../concepts/definition-structure.md)bakın.

1. AuditStorageAccounts.json dosyasını kullanarak bir ilke tanımı oluşturmak için aşağıdaki komutu çalıştırın.

   ```azurepowershell-interactive
   New-AzPolicyDefinition -Name 'AuditStorageAccounts' -DisplayName 'Audit Storage Accounts Open to Public Networks' -Policy 'AuditStorageAccounts.json'
   ```

   Komut, _Genel Ağlara Açık Denetim Depolama Hesapları_adlı bir ilke tanımı oluşturur.
   Kullanabileceğiniz diğer parametreler hakkında daha fazla bilgi için [New-AzPolicyDefinition](/powershell/module/az.resources/new-azpolicydefinition)'a bakın.

   Konum parametreleri olmadan `New-AzPolicyDefinition` çağrıldığınızda, oturumlar bağlamının seçili aboneliğinde ilke tanımını kaydetme varsayılandır. Tanımı farklı bir konuma kaydetmek için aşağıdaki parametreleri kullanın:

   - **SubscriptionId** - Farklı bir aboneliğe kaydedin. _GUID_ değeri gerektirir.
   - **ManagementGroupName** - Bir yönetim grubuna kaydedin. Bir _dize_ değeri gerektirir.

1. İlke tanımınızı oluşturduktan sonra, aşağıdaki komutları çalıştırarak bir ilke ataması oluşturabilirsiniz:

   ```azurepowershell-interactive
   $rg = Get-AzResourceGroup -Name 'ContosoRG'
   $Policy = Get-AzPolicyDefinition -Name 'AuditStorageAccounts'
   New-AzPolicyAssignment -Name 'AuditStorageAccounts' -PolicyDefinition $Policy -Scope $rg.ResourceId
   ```

   _ContosoRG'u_ amaçlanan kaynak grubunun adı ile değiştirin.

   Yönetim **Scope** grubu, `New-AzPolicyAssignment` abonelik, kaynak grubu veya tek bir kaynakla yapılan çalışmalarda Kapsam parametresi. Parametre, **resourceid** özelliğinin döndürdüğü tam kaynak yolunu kullanır. `Get-AzResourceGroup` Her kapsayıcı için **Kapsam** deseni aşağıdaki gibidir. Sırasıyla kaynak `{mgName}` adınız, kaynak grup adınız, abonelik kimliğiniz ve yönetim grup adınız ile değiştirin. `{rName}` `{rgName}` `{subId}`
   `{rType}`vm gibi `Microsoft.Compute/virtualMachines` **kaynağın kaynak türü** yle değiştirilir.

   - Kaynak -`/subscriptions/{subID}/resourceGroups/{rgName}/providers/{rType}/{rName}`
   - Kaynak grubu -`/subscriptions/{subId}/resourceGroups/{rgName}`
   - Abonelik -`/subscriptions/{subId}/`
   - Yönetim grubu -`/providers/Microsoft.Management/managementGroups/{mgName}`

Azure Kaynak Yöneticisi PowerShell modüllerini kullanarak kaynak ilkeleri yönetme hakkında daha fazla bilgi için [Az.Resources'a](/powershell/module/az.resources/#policies)bakın.

### <a name="create-and-assign-a-policy-definition-using-armclient"></a>ARMClient kullanarak bir ilke tanımı oluşturma ve atama

İlke tanımı oluşturmak için aşağıdaki yordamı kullanın.

1. Bir JSON dosyası oluşturmak için aşağıdaki JSON snippet'i kopyalayın. Bir sonraki adımda dosyayı ararsınız.

   ```json
   "properties": {
       "displayName": "Audit Storage Accounts Open to Public Networks",
       "policyType": "Custom",
       "mode": "Indexed",
       "description": "This policy ensures that storage accounts with exposure to Public Networks are audited.",
       "parameters": {},
       "policyRule": {
           "if": {
               "allOf": [{
                       "field": "type",
                       "equals": "Microsoft.Storage/storageAccounts"
                   },
                   {
                       "field": "Microsoft.Storage/storageAccounts/networkAcls.defaultAction",
                       "equals": "Allow"
                   }
               ]
           },
           "then": {
               "effect": "audit"
           }
       }
   }
   ```

1. Aşağıdaki çağrılardan birini kullanarak ilke tanımını oluşturun:

   ```console
   # For defining a policy in a subscription
   armclient PUT "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/policyDefinitions/AuditStorageAccounts?api-version=2016-12-01" @<path to policy definition JSON file>

   # For defining a policy in a management group
   armclient PUT "/providers/Microsoft.Management/managementgroups/{managementGroupId}/providers/Microsoft.Authorization/policyDefinitions/AuditStorageAccounts?api-version=2016-12-01" @<path to policy definition JSON file>
   ```

   Önceki {subscriptionId} ile aboneliğinizin kimliğini veya {managementGroupId} ile [yönetim grubunuzun](../../management-groups/overview.md)kimliğini değiştirin.

   Sorgunun yapısı hakkında daha fazla bilgi için Azure [İlkesi Tanımları – Oluşturma veya Güncelleştirme](/rest/api/resources/policydefinitions/createorupdate) ve [İlke Tanımları – Yönetim Grubunda Oluşturma veya Güncelleştirme](/rest/api/resources/policydefinitions/createorupdateatmanagementgroup)

İlke ataması oluşturmak ve ilke tanımını kaynak grubu düzeyinde atamak için aşağıdaki yordamı kullanın.

1. Bir JSON ilke atama dosyası oluşturmak için aşağıdaki JSON snippet'i kopyalayın. Sembollerdeki &lt; &gt; örnek bilgileri kendi değerlerinizle değiştirin.

   ```json
   {
       "properties": {
           "description": "This policy assignment makes sure that storage accounts with exposure to Public Networks are audited.",
           "displayName": "Audit Storage Accounts Open to Public Networks Assignment",
           "parameters": {},
           "policyDefinitionId": "/subscriptions/<subscriptionId>/providers/Microsoft.Authorization/policyDefinitions/Audit Storage Accounts Open to Public Networks",
           "scope": "/subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>"
       }
   }
   ```

1. Aşağıdaki çağrıyı kullanarak ilke atamasını oluşturun:

   ```console
   armclient PUT "/subscriptions/<subscriptionID>/resourceGroups/<resourceGroupName>/providers/Microsoft.Authorization/policyAssignments/Audit Storage Accounts Open to Public Networks?api-version=2017-06-01-preview" @<path to Assignment JSON file>
   ```

   Sembollerdeki &lt; &gt; örnek bilgileri kendi değerlerinizle değiştirin.

   REST API'ye HTTP aramaları yapma hakkında daha fazla bilgi için [Azure REST API Kaynakları'na](/rest/api/resources/)bakın.

### <a name="create-and-assign-a-policy-definition-with-azure-cli"></a>Azure CLI ile bir ilke tanımı oluşturma ve atama

İlke tanımı oluşturmak için aşağıdaki yordamı kullanın:

1. Bir JSON ilke atama dosyası oluşturmak için aşağıdaki JSON snippet'i kopyalayın.

   ```json
   {
       "if": {
           "allOf": [{
                   "field": "type",
                   "equals": "Microsoft.Storage/storageAccounts"
               },
               {
                   "field": "Microsoft.Storage/storageAccounts/networkAcls.defaultAction",
                   "equals": "Allow"
               }
           ]
       },
       "then": {
           "effect": "audit"
       }
   }
   ```

   İlke tanımı yazma hakkında daha fazla bilgi için [Azure İlkesi Tanım Yapısı'na](../concepts/definition-structure.md)bakın.

1. İlke tanımı oluşturmak için aşağıdaki komutu çalıştırın:

   ```azurecli-interactive
   az policy definition create --name 'audit-storage-accounts-open-to-public-networks' --display-name 'Audit Storage Accounts Open to Public Networks' --description 'This policy ensures that storage accounts with exposures to public networks are audited.' --rules '<path to json file>' --mode All
   ```

   Komut, _Genel Ağlara Açık Denetim Depolama Hesapları_adlı bir ilke tanımı oluşturur.
   Kullanabileceğiniz diğer parametreler hakkında daha fazla bilgi için [az ilke tanımı oluşturma](/cli/azure/policy/definition#az-policy-definition-create)'ya bakın.

   Konum parametreleri olmadan `az policy definition creation` çağrıldığınızda, oturumlar bağlamının seçili aboneliğinde ilke tanımını kaydetme varsayılandır. Tanımı farklı bir konuma kaydetmek için aşağıdaki parametreleri kullanın:

   - **--abonelik** - Farklı bir aboneye kaydedin. Abonelik kimliği için _GUID_ değeri veya abonelik adı için bir _dize_ değeri gerektirir.
   - **--management-group** - Yönetim grubuna kaydedin. Bir _dize_ değeri gerektirir.

1. İlke ataması oluşturmak için aşağıdaki komutu kullanın. Sembollerdeki &lt; &gt; örnek bilgileri kendi değerlerinizle değiştirin.

   ```azurecli-interactive
   az policy assignment create --name '<name>' --scope '<scope>' --policy '<policy definition ID>'
   ```

   Yönetim grubu, abonelik, kaynak grubu veya tek bir kaynakla yapılan çalışmalarda `az policy assignment create` **--kapsam** parametresi. Parametre tam kaynak yolunu kullanır. Her kapsayıcı için **--kapsam** için desen aşağıdaki gibidir. Sırasıyla kaynak `{mgName}` adınız, kaynak grup adınız, abonelik kimliğiniz ve yönetim grup adınız ile değiştirin. `{rName}` `{rgName}` `{subId}` `{rType}`vm gibi `Microsoft.Compute/virtualMachines` **kaynağın kaynak türü** yle değiştirilir.

   - Kaynak -`/subscriptions/{subID}/resourceGroups/{rgName}/providers/{rType}/{rName}`
   - Kaynak grubu -`/subscriptions/{subID}/resourceGroups/{rgName}`
   - Abonelik -`/subscriptions/{subID}`
   - Yönetim grubu -`/providers/Microsoft.Management/managementGroups/{mgName}`

Aşağıdaki komutla PowerShell'i kullanarak Azure İlkesi Tanım Kimliği'ni alabilirsiniz:

```azurecli-interactive
az policy definition show --name 'Audit Storage Accounts with Open Public Networks'
```

Oluşturduğunuz ilke tanımının ilke tanımı kimliği aşağıdaki örneğe benzemelidir:

```output
"/subscription/<subscriptionId>/providers/Microsoft.Authorization/policyDefinitions/Audit Storage Accounts Open to Public Networks"
```

Azure CLI ile kaynak ilkelerini nasıl yönetebileceğiniz hakkında daha fazla bilgi için [Azure CLI Kaynak İlkeleri'ne](/cli/azure/policy?view=azure-cli-latest)bakın.

## <a name="next-steps"></a>Sonraki adımlar

Bu makaledeki komutlar ve sorgular hakkında daha fazla bilgi için aşağıdaki makaleleri gözden geçirin.

- [Azure REST API Kaynakları](/rest/api/resources/)
- [Azure PowerShell Modülleri](/powershell/module/az.resources/#policies)
- [Azure CLI İlke Komutları](/cli/azure/policy?view=azure-cli-latest)
- [Azure İlkesi Öngörüleri kaynak sağlayıcısı REST API başvurusu](/rest/api/policy-insights)
- [Kaynaklarınızı Azure yönetim gruplarıyla düzenleme](../../management-groups/overview.md).