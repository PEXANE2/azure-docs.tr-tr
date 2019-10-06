---
title: Programlı olarak ilkeler oluşturma
description: Bu makalede, Azure Ilkesi için ilkeleri programlı bir şekilde oluşturma ve yönetme işlemi adım adım açıklanmaktadır.
author: DCtheGeek
ms.author: dacoulte
ms.date: 01/31/2019
ms.topic: conceptual
ms.service: azure-policy
ms.openlocfilehash: 047e9cab8d7776fc3b5353aebc571e28ad780ae8
ms.sourcegitcommit: d7689ff43ef1395e61101b718501bab181aca1fa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/06/2019
ms.locfileid: "71977946"
---
# <a name="programmatically-create-policies"></a>Programlı olarak ilkeler oluşturma

Bu makale, ilkeleri programlı olarak oluşturma ve yönetme konusunda size kılavuzluk eder. Azure Ilke tanımları, kaynaklarınız üzerinde farklı kurallar ve etkiler uygular. Zorlama, kaynakların kurumsal standartlarınız ve hizmet düzeyi Sözleşmelerinizle uyumlu kalmasını sağlar.

Uyumluluk hakkında daha fazla bilgi için bkz. [Uyumluluk verileri alma](getting-compliance-data.md).

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce, aşağıdaki önkoşulların karşılandığından emin olun:

1. Henüz yapmadıysanız [ARMClient](https://github.com/projectkudu/ARMClient)’ı yükleyin. Bu, HTTP isteklerini Azure Resource Manager tabanlı API’lere gönderen bir araçtır.

1. Azure PowerShell modülünüzü en son sürüme güncelleştirin. Ayrıntılı bilgi için bkz. [Azure PowerShell modülünü Install](/powershell/azure/install-az-ps) . En son sürüm hakkında daha fazla bilgi için bkz. [Azure PowerShell](https://github.com/Azure/azure-powershell/releases).

1. Aboneliğinizin kaynak sağlayıcısıyla birlikte çalışıp çalışmadığını doğrulamak için Azure PowerShell kullanarak Azure Policy Insights kaynak sağlayıcısını kaydedin. Bir kaynak sağlayıcısını kaydetmek için, kaynak sağlayıcısı için kaydetme eylemi işlemini çalıştırma izninizin olması gerekir. Bu işlem, Katkıda Bulunan ve Sahip rolleriyle birlikte sunulur. Aşağıdaki komutu çalıştırarak kaynak sağlayıcısını kaydedin:

   ```azurepowershell-interactive
   Register-AzResourceProvider -ProviderNamespace 'Microsoft.PolicyInsights'
   ```

   Kaynak sağlayıcıları kaydetme ve görüntülemeyle ilgili daha fazla bilgi için bkz. [Kaynak Sağlayıcıları ve Türleri](../../../azure-resource-manager/resource-manager-supported-services.md).

1. Henüz yapmadıysanız Azure CLı 'yı yükleyemezsiniz. [Windows 'Da Azure CLI 'Yı yüklemeye](/cli/azure/install-azure-cli-windows)en son sürümü edinebilirsiniz.

## <a name="create-and-assign-a-policy-definition"></a>İlke tanımı oluşturma ve atama

Kaynaklarınızın daha iyi görünürlüğüne yönelik ilk adım, kaynaklarınız üzerinde ilkeler oluşturmak ve atamak. Bir sonraki adım, programlı olarak bir ilke oluşturma ve atamayı öğrenirsiniz. Örnek ilke, PowerShell, Azure CLı ve HTTP istekleri kullanılarak tüm ortak ağlarda açık olan depolama hesaplarını denetler.

### <a name="create-and-assign-a-policy-definition-with-powershell"></a>PowerShell ile ilke tanımı oluşturma ve atama

1. AuditStorageAccounts. JSON adlı bir JSON dosyası oluşturmak için aşağıdaki JSON kod parçacığını kullanın.

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

   İlke tanımı yazma hakkında daha fazla bilgi için bkz. [Azure Ilke tanımı yapısı](../concepts/definition-structure.md).

1. AuditStorageAccounts. json dosyasını kullanarak bir ilke tanımı oluşturmak için aşağıdaki komutu çalıştırın.

   ```azurepowershell-interactive
   New-AzPolicyDefinition -Name 'AuditStorageAccounts' -DisplayName 'Audit Storage Accounts Open to Public Networks' -Policy 'AuditStorageAccounts.json'
   ```

   Komut, _ortak ağlarda açık olan denetim depolama hesapları_adlı bir ilke tanımı oluşturur.
   Kullanabileceğiniz diğer parametreler hakkında daha fazla bilgi için, bkz. [New-AzPolicyDefinition](/powershell/module/az.resources/new-azpolicydefinition).

   Konum parametreleri olmadan çağrıldığında, `New-AzPolicyDefinition` varsayılan olarak, ilke tanımının oturum bağlamının seçili aboneliğine kaydedilmesini sağlar. Tanımı farklı bir konuma kaydetmek için aşağıdaki parametreleri kullanın:

   - **SubscriptionID** -farklı bir aboneliğe kaydedin. Bir _GUID_ değeri gerektirir.
   - **ManagementGroupName** -bir yönetim grubuna kaydedin. Bir _dize_ değeri gerektirir.

1. İlke tanımınızı oluşturduktan sonra, aşağıdaki komutları çalıştırarak bir ilke ataması oluşturabilirsiniz:

   ```azurepowershell-interactive
   $rg = Get-AzResourceGroup -Name 'ContosoRG'
   $Policy = Get-AzPolicyDefinition -Name 'AuditStorageAccounts'
   New-AzPolicyAssignment -Name 'AuditStorageAccounts' -PolicyDefinition $Policy -Scope $rg.ResourceId
   ```

   _ContosoRG_ değerini amaçlanan kaynak grubunuzun adıyla değiştirin.

   @No__t-1 ' deki **kapsam** parametresi yönetim grubu, abonelik, kaynak grubu veya tek bir kaynakla birlikte kullanılabilir. Parametresi, `Get-AzResourceGroup` ' deki **RESOURCEID** özelliğinin döndürdüğü tam kaynak yolunu kullanır. Her kapsayıcının **kapsam** için olan model aşağıdaki gibidir. @No__t-0, `{rgName}`, `{subId}` ve `{mgName}` ' ü kaynak adı, kaynak grubu adı, abonelik KIMLIĞI ve yönetim grubu adıyla değiştirin.
   `{rType}`, kaynağın **kaynak türüyle** (örneğin, bir VM için `Microsoft.Compute/virtualMachines`) yerine konur.

   - Kaynak-`/subscriptions/{subID}/resourceGroups/{rgName}/providers/{rType}/{rName}`
   - Kaynak grubu-`/subscriptions/{subId}/resourceGroups/{rgName}`
   - Abonelik-`/subscriptions/{subId}/`
   - Yönetim grubu-`/providers/Microsoft.Management/managementGroups/{mgName}`

Azure Resource Manager PowerShell modülünü kullanarak kaynak ilkelerini yönetme hakkında daha fazla bilgi için bkz. [az. resources](/powershell/module/az.resources/#policies).

### <a name="create-and-assign-a-policy-definition-using-armclient"></a>ARMClient kullanarak ilke tanımı oluşturma ve atama

Bir ilke tanımı oluşturmak için aşağıdaki yordamı kullanın.

1. JSON dosyası oluşturmak için aşağıdaki JSON kod parçacığını kopyalayın. Sonraki adımda dosyayı çağıracaksınız.

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

   Önceki {SubscriptionID} öğesini aboneliğinizin KIMLIĞIYLE veya {ManagementGroupId} yerine [Yönetim GRUBUNUZUN](../../management-groups/overview.md)kimliğiyle değiştirin.

   Sorgunun yapısı hakkında daha fazla bilgi için bkz. [Azure Ilke tanımları – oluşturma veya güncelleştirme](/rest/api/resources/policydefinitions/createorupdate) ve [Ilke tanımları – Yönetim grubunda Oluştur veya Güncelleştir](/rest/api/resources/policydefinitions/createorupdateatmanagementgroup)

Bir ilke ataması oluşturmak ve ilke tanımını kaynak grubu düzeyinde atamak için aşağıdaki yordamı kullanın.

1. JSON ilke atama dosyası oluşturmak için aşağıdaki JSON kod parçacığını kopyalayın. @No__t-0 @ no__t-1 sembollerinde bulunan örnek bilgileri kendi değerlerinizle değiştirin.

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

   @No__t-0 @ no__t-1 sembollerinde bulunan örnek bilgileri kendi değerlerinizle değiştirin.

   REST API HTTP çağrıları yapma hakkında daha fazla bilgi için bkz. [Azure REST API kaynakları](/rest/api/resources/).

### <a name="create-and-assign-a-policy-definition-with-azure-cli"></a>Azure CLı ile ilke tanımı oluşturma ve atama

Bir ilke tanımı oluşturmak için aşağıdaki yordamı kullanın:

1. JSON ilke atama dosyası oluşturmak için aşağıdaki JSON kod parçacığını kopyalayın.

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

   İlke tanımı yazma hakkında daha fazla bilgi için bkz. [Azure Ilke tanımı yapısı](../concepts/definition-structure.md).

1. Bir ilke tanımı oluşturmak için aşağıdaki komutu çalıştırın:

   ```azurecli-interactive
   az policy definition create --name 'audit-storage-accounts-open-to-public-networks' --display-name 'Audit Storage Accounts Open to Public Networks' --description 'This policy ensures that storage accounts with exposures to public networks are audited.' --rules '<path to json file>' --mode All
   ```

   Komut, _ortak ağlarda açık olan denetim depolama hesapları_adlı bir ilke tanımı oluşturur.
   Kullanabileceğiniz diğer parametreler hakkında daha fazla bilgi için, bkz. [az Policy Definition Create](/cli/azure/policy/definition#az-policy-definition-create).

   Konum parametreleri olmadan çağrıldığında, `az policy definition creation` varsayılan olarak, ilke tanımının oturum bağlamının seçili aboneliğine kaydedilmesini sağlar. Tanımı farklı bir konuma kaydetmek için aşağıdaki parametreleri kullanın:

   - **--Subscription** -farklı bir aboneliğe kaydedin. Abonelik KIMLIĞI için bir _GUID_ değeri veya abonelik adı için bir _dize_ değeri gerektirir.
   - **--Yönetim-Group** -bir yönetim grubuna Kaydet. Bir _dize_ değeri gerektirir.

1. Bir ilke ataması oluşturmak için aşağıdaki komutu kullanın. @No__t-0 @ no__t-1 sembollerinde bulunan örnek bilgileri kendi değerlerinizle değiştirin.

   ```azurecli-interactive
   az policy assignment create --name '<name>' --scope '<scope>' --policy '<policy definition ID>'
   ```

   @No__t-1 ' deki **--scope** parametresi yönetim grubu, abonelik, kaynak grubu veya tek bir kaynak ile birlikte kullanılabilir. Parametresi tam kaynak yolunu kullanır. Her kapsayıcının **kapsam** için olan model aşağıdaki gibidir. @No__t-0, `{rgName}`, `{subId}` ve `{mgName}` ' ü kaynak adı, kaynak grubu adı, abonelik KIMLIĞI ve yönetim grubu adıyla değiştirin. `{rType}`, kaynağın **kaynak türüyle** (örneğin, bir VM için `Microsoft.Compute/virtualMachines`) yerine konur.

   - Kaynak-`/subscriptions/{subID}/resourceGroups/{rgName}/providers/{rType}/{rName}`
   - Kaynak grubu-`/subscriptions/{subID}/resourceGroups/{rgName}`
   - Abonelik-`/subscriptions/{subID}`
   - Yönetim grubu-`/providers/Microsoft.Management/managementGroups/{mgName}`

Aşağıdaki komutla PowerShell kullanarak Azure Ilke tanımı KIMLIĞI ' ni edinebilirsiniz:

```azurecli-interactive
az policy definition show --name 'Audit Storage Accounts with Open Public Networks'
```

Oluşturduğunuz ilke tanımının ilke tanımı KIMLIĞI aşağıdaki örneğe benzemelidir:

```output
"/subscription/<subscriptionId>/providers/Microsoft.Authorization/policyDefinitions/Audit Storage Accounts Open to Public Networks"
```

Azure CLı ile kaynak ilkelerini yönetme hakkında daha fazla bilgi için bkz. [Azure CLI kaynak ilkeleri](/cli/azure/policy?view=azure-cli-latest).

## <a name="next-steps"></a>Sonraki adımlar

Bu makaledeki komutlar ve sorgular hakkında daha fazla bilgi için aşağıdaki makalelere göz atın.

- [Azure REST API kaynakları](/rest/api/resources/)
- [Azure PowerShell modüller](/powershell/module/az.resources/#policies)
- [Azure CLı Ilkesi komutları](/cli/azure/policy?view=azure-cli-latest)
- [Azure Policy Insights kaynak sağlayıcısı REST API başvurusu](/rest/api/policy-insights)
- [Kaynaklarınızı Azure yönetim gruplarıyla düzenleme](../../management-groups/overview.md).