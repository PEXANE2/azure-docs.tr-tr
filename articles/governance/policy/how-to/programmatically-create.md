---
title: Programlı olarak ilkeler oluşturma
description: Bu makalede Azure CLı, Azure PowerShell ve REST API ile Azure Ilkesi için ilkeler oluşturma ve yönetme işlemi adım adım açıklanmaktadır.
ms.date: 01/31/2019
ms.topic: how-to
ms.openlocfilehash: 08ed43a464d1dd7de8220428dbc1c61ce9fc3ad6
ms.sourcegitcommit: 509b39e73b5cbf670c8d231b4af1e6cfafa82e5a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78386777"
---
# <a name="programmatically-create-policies"></a>Programlı olarak ilkeler oluşturma

Bu makalede, program aracılığıyla oluşturma ve ilkeleri yönetme gösterilmektedir. Azure Ilke tanımları, kaynaklarınız üzerinde farklı kurallar ve etkiler uygular. Zorlama kaynakları, Kurumsal standartlarınız ve hizmet düzeyi sözleşmeleri ile uyumluluğu sürdürün, emin olur.

Uyumluluk hakkında daha fazla bilgi için bkz. [Uyumluluk verileri alma](get-compliance-data.md).

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce aşağıdaki önkoşulların karşılandığından emin olun:

1. Henüz yapmadıysanız [ARMClient](https://github.com/projectkudu/ARMClient)’ı yükleyin. Bu, HTTP isteklerini Azure Resource Manager tabanlı API’lere gönderen bir araçtır.

1. Azure PowerShell modülünüzü en son sürüme güncelleştirin. Ayrıntılı bilgi için bkz. [Azure PowerShell modülünü Install](/powershell/azure/install-az-ps) . En son sürüm hakkında daha fazla bilgi için bkz. [Azure PowerShell](https://github.com/Azure/azure-powershell/releases).

1. Aboneliğinizin kaynak sağlayıcısıyla birlikte çalışıp çalışmadığını doğrulamak için Azure PowerShell kullanarak Azure Policy Insights kaynak sağlayıcısını kaydedin. Bir kaynak sağlayıcısını kaydetmek için kaynak sağlayıcısı kaydetme işlemini çalıştırma izni olmalıdır. Bu işlem, Katkıda Bulunan ve Sahip rolleriyle birlikte sunulur. Aşağıdaki komutu çalıştırarak kaynak sağlayıcısını kaydedin:

   ```azurepowershell-interactive
   Register-AzResourceProvider -ProviderNamespace 'Microsoft.PolicyInsights'
   ```

   Kaynak sağlayıcıları kaydetme ve görüntülemeyle ilgili daha fazla bilgi için bkz. [Kaynak Sağlayıcıları ve Türleri](../../../azure-resource-manager/management/resource-providers-and-types.md).

1. Henüz yapmadıysanız, Azure CLI'yı yükleyin. [Windows 'Da Azure CLI 'Yı yüklemeye](/cli/azure/install-azure-cli-windows)en son sürümü edinebilirsiniz.

## <a name="create-and-assign-a-policy-definition"></a>Bir ilke tanımı oluşturma ve atama

Kaynaklarınızın daha iyi görünürlük ilk adım, ilkeleri, kaynaklarınız üzerinden oluşturup sağlamaktır. Sonraki adım, program aracılığıyla oluşturma ve ilke atama işlemlerini öğrenmektir. Örnek ilke, PowerShell, Azure CLI ve HTTP isteklerini kullanarak tüm genel ağa açık olan depolama hesaplarını denetler.

### <a name="create-and-assign-a-policy-definition-with-powershell"></a>PowerShell ile bir ilke tanımı oluşturma ve atama

1. Aşağıdaki JSON kod parçacığında AuditStorageAccounts.json ada sahip bir JSON dosyası oluşturmak için kullanın.

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

1. AuditStorageAccounts.json dosyası kullanarak bir ilke tanımı oluşturmak için aşağıdaki komutu çalıştırın.

   ```azurepowershell-interactive
   New-AzPolicyDefinition -Name 'AuditStorageAccounts' -DisplayName 'Audit Storage Accounts Open to Public Networks' -Policy 'AuditStorageAccounts.json'
   ```

   Komut, _ortak ağlarda açık olan denetim depolama hesapları_adlı bir ilke tanımı oluşturur.
   Kullanabileceğiniz diğer parametreler hakkında daha fazla bilgi için, bkz. [New-AzPolicyDefinition](/powershell/module/az.resources/new-azpolicydefinition).

   Konum parametreleri olmadan çağrıldığında, varsayılan olarak `New-AzPolicyDefinition`, ilke tanımının oturum bağlamının seçili aboneliğine kaydedilmesini sağlar. Tanımı farklı bir konuma kaydetmek için aşağıdaki parametreleri kullanın:

   - **SubscriptionID** -farklı bir aboneliğe kaydedin. Bir _GUID_ değeri gerektirir.
   - **ManagementGroupName** -bir yönetim grubuna kaydedin. Bir _dize_ değeri gerektirir.

1. İlke tanımınız oluşturduktan sonra aşağıdaki komutları çalıştırarak ilke ataması oluşturabilirsiniz:

   ```azurepowershell-interactive
   $rg = Get-AzResourceGroup -Name 'ContosoRG'
   $Policy = Get-AzPolicyDefinition -Name 'AuditStorageAccounts'
   New-AzPolicyAssignment -Name 'AuditStorageAccounts' -PolicyDefinition $Policy -Scope $rg.ResourceId
   ```

   _ContosoRG_ değerini amaçlanan kaynak grubunuzun adıyla değiştirin.

   `New-AzPolicyAssignment` **kapsam** parametresi yönetim grubu, abonelik, kaynak grubu veya tek bir kaynakla birlikte kullanılabilir. Parametresi, `Get-AzResourceGroup` **RESOURCEID** özelliğinin döndürdüğü tam kaynak yolunu kullanır. Her kapsayıcının **kapsam** için olan model aşağıdaki gibidir. `{rName}`, `{rgName}`, `{subId}`ve `{mgName}`, sırasıyla kaynak adınızla, kaynak grubu adıyla, abonelik KIMLIĞINIZLE ve yönetim grubu adıyla değiştirin.
   `{rType}`, kaynağın **kaynak türüyle** (örneğin, bir VM için `Microsoft.Compute/virtualMachines`) değiştirilebilir.

   - Kaynak-`/subscriptions/{subID}/resourceGroups/{rgName}/providers/{rType}/{rName}`
   - Kaynak grubu-`/subscriptions/{subId}/resourceGroups/{rgName}`
   - Abonelik-`/subscriptions/{subId}/`
   - Yönetim grubu-`/providers/Microsoft.Management/managementGroups/{mgName}`

Azure Resource Manager PowerShell modülünü kullanarak kaynak ilkelerini yönetme hakkında daha fazla bilgi için bkz. [az. resources](/powershell/module/az.resources/#policies).

### <a name="create-and-assign-a-policy-definition-using-armclient"></a>ARMClient kullanarak bir ilke tanımı oluşturma ve atama

İlke tanımı oluşturmak için aşağıdaki yordamı kullanın.

1. Bir JSON dosyası oluşturmak için aşağıdaki JSON kod parçacığını kopyalayın. Sonraki adımda dosya ararız.

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

1. Aşağıdaki çağrıları birini kullanarak ilke tanımı oluşturun:

   ```console
   # For defining a policy in a subscription
   armclient PUT "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/policyDefinitions/AuditStorageAccounts?api-version=2016-12-01" @<path to policy definition JSON file>

   # For defining a policy in a management group
   armclient PUT "/providers/Microsoft.Management/managementgroups/{managementGroupId}/providers/Microsoft.Authorization/policyDefinitions/AuditStorageAccounts?api-version=2016-12-01" @<path to policy definition JSON file>
   ```

   Önceki {SubscriptionID} öğesini aboneliğinizin KIMLIĞIYLE veya {ManagementGroupId} yerine [Yönetim GRUBUNUZUN](../../management-groups/overview.md)kimliğiyle değiştirin.

   Sorgunun yapısı hakkında daha fazla bilgi için bkz. [Azure Ilke tanımları – oluşturma veya güncelleştirme](/rest/api/resources/policydefinitions/createorupdate) ve [Ilke tanımları – Yönetim grubunda Oluştur veya Güncelleştir](/rest/api/resources/policydefinitions/createorupdateatmanagementgroup)

Bir ilke ataması oluşturma ve kaynak grubu düzeyinde ilke tanımını atamak için aşağıdaki yordamı kullanın.

1. İlke ataması JSON dosyası oluşturmak için aşağıdaki JSON kod parçacığını kopyalayın. &lt;&gt; sembollerinde bulunan örnek bilgileri kendi değerlerinizle değiştirin.

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

1. Çağrısını kullanarak ilke ataması oluşturun:

   ```console
   armclient PUT "/subscriptions/<subscriptionID>/resourceGroups/<resourceGroupName>/providers/Microsoft.Authorization/policyAssignments/Audit Storage Accounts Open to Public Networks?api-version=2017-06-01-preview" @<path to Assignment JSON file>
   ```

   &lt;&gt; sembollerinde bulunan örnek bilgileri kendi değerlerinizle değiştirin.

   REST API HTTP çağrıları yapma hakkında daha fazla bilgi için bkz. [Azure REST API kaynakları](/rest/api/resources/).

### <a name="create-and-assign-a-policy-definition-with-azure-cli"></a>Azure CLI ile bir ilke tanımı oluşturma ve atama

Bir ilke tanımı oluşturmak için aşağıdaki yordamı kullanın:

1. İlke ataması JSON dosyası oluşturmak için aşağıdaki JSON kod parçacığını kopyalayın.

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

1. İlke tanımı oluşturmak için aşağıdaki komutu çalıştırın:

   ```azurecli-interactive
   az policy definition create --name 'audit-storage-accounts-open-to-public-networks' --display-name 'Audit Storage Accounts Open to Public Networks' --description 'This policy ensures that storage accounts with exposures to public networks are audited.' --rules '<path to json file>' --mode All
   ```

   Komut, _ortak ağlarda açık olan denetim depolama hesapları_adlı bir ilke tanımı oluşturur.
   Kullanabileceğiniz diğer parametreler hakkında daha fazla bilgi için, bkz. [az Policy Definition Create](/cli/azure/policy/definition#az-policy-definition-create).

   Konum parametreleri olmadan çağrıldığında, varsayılan olarak `az policy definition creation`, ilke tanımının oturum bağlamının seçili aboneliğine kaydedilmesini sağlar. Tanımı farklı bir konuma kaydetmek için aşağıdaki parametreleri kullanın:

   - **--Subscription** -farklı bir aboneliğe kaydedin. Abonelik KIMLIĞI için bir _GUID_ değeri veya abonelik adı için bir _dize_ değeri gerektirir.
   - **--Yönetim-Group** -bir yönetim grubuna Kaydet. Bir _dize_ değeri gerektirir.

1. Bir ilke ataması oluşturmak için aşağıdaki komutu kullanın. &lt;&gt; sembollerinde bulunan örnek bilgileri kendi değerlerinizle değiştirin.

   ```azurecli-interactive
   az policy assignment create --name '<name>' --scope '<scope>' --policy '<policy definition ID>'
   ```

   `az policy assignment create` üzerindeki **--scope** parametresi yönetim grubu, abonelik, kaynak grubu veya tek bir kaynakla birlikte kullanılabilir. Parametresi tam kaynak yolunu kullanır. Her kapsayıcının **kapsam** için olan model aşağıdaki gibidir. `{rName}`, `{rgName}`, `{subId}`ve `{mgName}`, sırasıyla kaynak adınızla, kaynak grubu adıyla, abonelik KIMLIĞINIZLE ve yönetim grubu adıyla değiştirin. `{rType}`, kaynağın **kaynak türüyle** (örneğin, bir VM için `Microsoft.Compute/virtualMachines`) değiştirilebilir.

   - Kaynak-`/subscriptions/{subID}/resourceGroups/{rgName}/providers/{rType}/{rName}`
   - Kaynak grubu-`/subscriptions/{subID}/resourceGroups/{rgName}`
   - Abonelik-`/subscriptions/{subID}`
   - Yönetim grubu-`/providers/Microsoft.Management/managementGroups/{mgName}`

Aşağıdaki komutla PowerShell kullanarak Azure Ilke tanımı KIMLIĞI ' ni edinebilirsiniz:

```azurecli-interactive
az policy definition show --name 'Audit Storage Accounts with Open Public Networks'
```

İlke tanım kimliği, oluşturduğunuz ilke tanımı için aşağıdaki örneğe benzemelidir:

```output
"/subscription/<subscriptionId>/providers/Microsoft.Authorization/policyDefinitions/Audit Storage Accounts Open to Public Networks"
```

Azure CLı ile kaynak ilkelerini yönetme hakkında daha fazla bilgi için bkz. [Azure CLI kaynak ilkeleri](/cli/azure/policy?view=azure-cli-latest).

## <a name="next-steps"></a>Sonraki adımlar

Bu makaledeki sorgular ve komutları hakkında daha fazla bilgi için aşağıdaki makaleleri inceleyin.

- [Azure REST API kaynakları](/rest/api/resources/)
- [Azure PowerShell modüller](/powershell/module/az.resources/#policies)
- [Azure CLı Ilkesi komutları](/cli/azure/policy?view=azure-cli-latest)
- [Azure Policy Insights kaynak sağlayıcısı REST API başvurusu](/rest/api/policy-insights)
- [Kaynaklarınızı Azure yönetim gruplarıyla düzenleme](../../management-groups/overview.md).