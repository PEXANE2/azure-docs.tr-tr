---
title: VNet uç noktaları olmadan örnek anahtar kasaları
description: Bu örnek ilke tanımı, sanal ağ hizmeti uç noktaları olmayan örnekleri algılamak için Key Vault kasalarını denetler.
author: DCtheGeek
ms.service: azure-policy
ms.topic: sample
ms.date: 01/26/2019
ms.author: dacoulte
ms.openlocfilehash: 7bcbcdf68b3c8f882a1e0fbb9953fea575f96556
ms.sourcegitcommit: 1c2659ab26619658799442a6e7604f3c66307a89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/10/2019
ms.locfileid: "72255732"
---
# <a name="sample---key-vault-vaults-with-no-virtual-network-endpoints"></a>Sanal ağ uç noktaları olmadan örnek Key Vault kasaları

Bu ilke, sanal ağ uç noktaları olmayan Key Vault kasaları denetler. Güvenlik gereksinimlerinizi zorlamak için kullanın. Daha fazla bilgi için bkz. [Key Vault sanal ağ hizmeti uç noktaları](../../../key-vault/key-vault-overview-vnet-service-endpoints.md)

Bu örnek ilkeyi kullanarak dağıtabilirsiniz:

- [Azure Portal](#azure-portal)
- [Azure PowerShell](#azure-powershell)
- [Azure CLı](#azure-cli)
- [REST API](#rest-api)

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-policy"></a>Örnek ilke

### <a name="policy-definition"></a>İlke tanımı

REST API, ' Azure 'a Dağıt ' düğmeleri ve portalda el ile kullanılan bir bütün oluşturulan JSON ilke tanımı.

[!code-json[full](../../../../policy-templates/samples/KeyVault/audit-keyvault-vnet-rules/azurepolicy.json "KeyVault vnet rules")]

> [!NOTE]
> Portalda el ile ilke oluşturuyorsanız, yukarıdaki **Özellikler. Parameters** ve **Properties. policyrule** kısımlarını kullanın. İki bölümü, `{}` küme ayraçları ile birlikte, geçerli JSON yapmak için sarın.

### <a name="policy-rules"></a>İlke kuralları

Azure CLı ve Azure PowerShell tarafından kullanılan ilkenin kurallarını tanımlayan JSON.

[!code-json[rule](../../../../policy-templates/samples/KeyVault/audit-keyvault-vnet-rules/azurepolicy.rules.json "Policy rules (JSON)")]

### <a name="policy-parameters"></a>İlke parametreleri

Bu örnek ilke tanımında tanımlı parametre yok.

## <a name="azure-portal"></a>Azure portal

[![ ilke örneğini azure @no__t dağıtma](https://azuredeploy.net/deploybutton.png)](https://portal.azure.com/#blade/Microsoft_Azure_Policy/CreatePolicyDefinitionBlade/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-policy%2Fmaster%2Fsamples%2FKeyVault%2Faudit-keyvault-vnet-rules%2Fazurepolicy.json)-2[![Ilke örneğini Azure gov 'ye dağıtma](https://docs.microsoft.com/azure/governance/policy/media/deploy/deployGovbutton.png)](https://portal.azure.us/#blade/Microsoft_Azure_Policy/CreatePolicyDefinitionBlade/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-policy%2Fmaster%2Fsamples%2FKeyVault%2Faudit-keyvault-vnet-rules%2Fazurepolicy.json)

## <a name="azure-powershell"></a>Azure PowerShell

[!INCLUDE [sample-powershell-install](../../../../includes/sample-powershell-install-no-ssh-az.md)]

### <a name="deploy-with-azure-powershell"></a>Azure PowerShell ile dağıtma

```azurepowershell-interactive
# Create the Policy Definition (Subscription scope)
$definition = New-AzPolicyDefinition -Name "audit-keyvault-vnet-rules" -DisplayName "Audit if Key Vault has no virtual network rules" -description "Audits Key Vault vaults if they do not have virtual network service endpoints set up. More information on virtual network service endpoints in Key Vault is available here: https://docs.microsoft.com/azure/key-vault/key-vault-overview-vnet-service-endpoints" -Policy 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/KeyVault/audit-keyvault-vnet-rules/azurepolicy.rules.json' -Mode Indexed

# Set the scope to a resource group; may also be a subscription or management group
$scope = Get-AzResourceGroup -Name 'YourResourceGroup'

# Create the Policy Assignment
$assignment = New-AzPolicyAssignment -Name 'audit-keyvault-vnet-rules-assignment' -DisplayName 'Audit Key Vault Assignment' -Scope $scope.ResourceId -PolicyDefinition $definition
```

### <a name="remove-with-azure-powershell"></a>Azure PowerShell ile kaldır

Önceki atamayı ve tanımını kaldırmak için aşağıdaki komutları çalıştırın:

```azurepowershell-interactive
# Remove the Policy Assignment
Remove-AzPolicyAssignment -Id $assignment.ResourceId

# Remove the Policy Definition
Remove-AzPolicyDefinition -Id $definition.ResourceId
```

### <a name="azure-powershell-explanation"></a>Azure PowerShell açıklaması

Dağıtım ve kaldırma betikleri aşağıdaki komutları kullanır. Aşağıdaki tablodaki her komut, komuta özgü belgelere bağlanır:

| Komut | Notlar |
|---|---|
| [New-AzPolicyDefinition](/powershell/module/az.resources/New-Azpolicydefinition) | Yeni bir Azure Ilke tanımı oluşturur. |
| [Get-AzResourceGroup](/powershell/module/az.resources/Get-Azresourcegroup) | Tek bir kaynak grubunu alır. |
| [New-AzPolicyAssignment](/powershell/module/az.resources/New-Azpolicyassignment) | Yeni bir Azure Ilke ataması oluşturur. Bu örnekte, bir tanım sağlıyoruz, ancak aynı zamanda bir girişim alabilir. |
| [Remove-AzPolicyAssignment](/powershell/module/az.resources/Remove-Azpolicyassignment) | Var olan bir Azure Ilke atamasını kaldırır. |
| [Remove-AzPolicyDefinition](/powershell/module/az.resources/Remove-Azpolicydefinition) | Mevcut bir Azure Ilke tanımını kaldırır. |

## <a name="azure-cli"></a>Azure CLı

[!INCLUDE [sample-cli-install](../../../../includes/sample-cli-install.md)]

### <a name="deploy-with-azure-cli"></a>Azure CLı ile dağıtma

```azurecli-interactive
# Create the Policy Definition (Subscription scope)
definition=$(az policy definition create --name 'audit-keyvault-vnet-rules' --display-name 'Audit if Key Vault has no virtual network rules' --description 'Audits Key Vault vaults if they do not have virtual network service endpoints set up. More information on virtual network service endpoints in Key Vault is available here: https://docs.microsoft.com/azure/key-vault/key-vault-overview-vnet-service-endpoints' --rules 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/KeyVault/audit-keyvault-vnet-rules/azurepolicy.rules.json' --mode Indexed)

# Set the scope to a resource group; may also be a subscription or management group
scope=$(az group show --name 'YourResourceGroup')

# Create the Policy Assignment
assignment=$(az policy assignment create --name 'audit-keyvault-vnet-rules-assignment' --display-name 'Audit Key Vault Assignment' --scope `echo $scope | jq '.id' -r` --policy `echo $definition | jq '.name' -r`)
```

### <a name="remove-with-azure-cli"></a>Azure CLı ile kaldırma

Önceki atamayı ve tanımını kaldırmak için aşağıdaki komutları çalıştırın:

```azurecli-interactive
# Remove the Policy Assignment
az policy assignment delete --name `echo $assignment | jq '.name' -r`

# Remove the Policy Definition
az policy definition delete --name `echo $definition | jq '.name' -r`
```

### <a name="azure-cli-explanation"></a>Azure CLı açıklaması

| Komut | Notlar |
|---|---|
| [az Policy Definition Create](/cli/azure/policy/definition?view=azure-cli-latest#az-policy-definition-create) | Yeni bir Azure Ilke tanımı oluşturur. |
| [az Group Show](/cli/azure/group?view=azure-cli-latest#az-group-show) | Tek bir kaynak grubunu alır. |
| [az Policy atama Create](/cli/azure/policy/assignment?view=azure-cli-latest#az-policy-assignment-create) | Yeni bir Azure Ilke ataması oluşturur. Bu örnekte, bir tanım sağlıyoruz, ancak aynı zamanda bir girişim alabilir. |
| [az Policy atama Delete](/cli/azure/policy/assignment?view=azure-cli-latest#az-policy-assignment-delete) | Var olan bir Azure Ilke atamasını kaldırır. |
| [az Policy Definition Delete](/cli/azure/policy/definition?view=azure-cli-latest#az-policy-definition-delete) | Mevcut bir Azure Ilke tanımını kaldırır. |

## <a name="rest-api"></a>REST API

[Armclient](https://github.com/projectkudu/ARMClient) veya PowerShell gibi kaynak yöneticisi REST API etkileşimde bulunmak için kullanılabilecek çeşitli araçlar vardır.

### <a name="deploy-with-rest-api"></a>REST API ile dağıtma

- Ilke tanımını (abonelik kapsamı) oluşturun. Istek gövdesi için JSON [ilke tanımını](#policy-definition) kullanın.

  ```http
  PUT https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/policyDefinitions/audit-keyvault-vnet-rules?api-version=2018-05-01
  ```

- Ilke atamasını oluşturma (kaynak grubu kapsamı)

  ```http
  PUT https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/YourResourceGroup/providers/Microsoft.Authorization/policyAssignments/audit-keyvault-vnet-rules-assignment?api-version=2018-05-01
  ```

  Istek gövdesi için aşağıdaki JSON örneğini kullanın:

  ```json
  {
      "properties": {
          "displayName": "Audit Key Vault Assignment",
          "policyDefinitionId": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/policyDefinitions/audit-keyvault-vnet-rules"
      }
  }
  ```

### <a name="remove-with-rest-api"></a>REST API ile kaldır

- Ilke atamasını kaldırma

  ```http
  DELETE https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/policyAssignments/audit-keyvault-vnet-rules-assignment?api-version=2018-05-01
  ```

- Ilke tanımını kaldır

  ```http
  DELETE https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/policyDefinitions/audit-keyvault-vnet-rules?api-version=2018-05-01
  ```

### <a name="rest-api-explanation"></a>REST API açıklaması

| Hizmet | Grup | Çalışma | Notlar |
|---|---|---|---|
| Kaynak yönetimi | İlke tanımları | [Oluşturma](/rest/api/resources/policydefinitions/createorupdate) | Bir abonelikte yeni bir Azure Ilke tanımı oluşturur. Alternatif: [Yönetim grubunda Oluştur](/rest/api/resources/policydefinitions/createorupdateatmanagementgroup) |
| Kaynak yönetimi | İlke atamaları | [Oluşturma](/rest/api/resources/policyassignments/create) | Yeni bir Azure Ilke ataması oluşturur. Bu örnekte, bir tanım sağlıyoruz, ancak aynı zamanda bir girişim alabilir. |
| Kaynak yönetimi | İlke atamaları | [Delete](/rest/api/resources/policyassignments/delete) | Var olan bir Azure Ilke atamasını kaldırır. |
| Kaynak yönetimi | İlke tanımları | [Delete](/rest/api/resources/policydefinitions/delete) | Mevcut bir Azure Ilke tanımını kaldırır. Alternatif: [Yönetim grubunda Sil](/rest/api/resources/policydefinitions/deleteatmanagementgroup) |

## <a name="next-steps"></a>Sonraki adımlar

- Ek [Azure ilkesi örneklerini](index.md) gözden geçirin
- [Azure ilke tanımı yapısını](../concepts/definition-structure.md) gözden geçirin