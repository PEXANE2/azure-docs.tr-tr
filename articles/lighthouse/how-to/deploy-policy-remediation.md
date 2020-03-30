---
title: Düzeltilebilir bir ilke dağıtma
description: Bir müşteriyi Azure'a devredilen kaynak yönetimine nasıl bağlayıp kaynaklarına kendi kiracınız aracılığıyla erişilmesine ve yönetilmesine izin vermeyi öğrenin.
ms.date: 10/11/2019
ms.topic: conceptual
ms.openlocfilehash: c06ed4ea597808aee18d4a848bcfea7152b9cf8e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79270647"
---
# <a name="deploy-a-policy-that-can-be-remediated-within-a-delegated-subscription"></a>Devredilen abonelik içinde düzeltilebilen bir ilke dağıtma

[Azure Deniz Feneri,](../overview.md) hizmet sağlayıcıların devredilen bir abonelik içinde ilke tanımları oluşturmasına ve bu tanımlamaları oluşturmasına olanak tanır. Ancak, bir [düzeltme görevi](../../governance/policy/how-to/remediate-resources.md) kullanan ilkeleri dağıtmak için (diğer bir deyişle, [deployIfNotExists](../../governance/policy/concepts/effects.md#deployifnotexists) veya [değişiklik](../../governance/policy/concepts/effects.md#modify) efekti ile ilkeler), müşteri kiracı yönetilen bir [kimlik](../../active-directory/managed-identities-azure-resources/overview.md) oluşturmanız gerekir. Bu yönetilen kimlik, şablonu ilke içinde dağıtmak için Azure İlkesi tarafından kullanılabilir. Bu senaryoyu etkinleştirmek için gereken adımlar vardır, hem Azure temsilci kaynak yönetimi için müşteriye bindiğinizde hem de ilkenin kendisini dağıtırken.

## <a name="create-a-user-who-can-assign-roles-to-a-managed-identity-in-the-customer-tenant"></a>Müşteri kiracısında yönetilen bir kimliğe roller atayabilecek bir kullanıcı oluşturma

Azure temsilcikaynak yönetimi için bir müşteriye bindiğinizde, yönetici kiracınızdaki kullanıcıları, kullanıcı gruplarını ve hizmet ilkelerini tanımlayan ve müşteri kiracısında ki devredilen kaynaklara erişebilecek bir parametre dosyasıyla birlikte bir [Azure Kaynak Yöneticisi şablonu](onboard-customer.md#create-an-azure-resource-manager-template) kullanırsınız. Parametreler dosyanızda, bu kullanıcıların her birine **(principalId)** erişim düzeyini tanımlayan yerleşik bir [rol](../../role-based-access-control/built-in-roles.md) **(roleDefinitionId)** atanır.

Bir **principalId'in** müşteri kiracısında yönetilen bir kimlik oluşturmasına izin vermek için, **rolünüDefinitionId'i** **Kullanıcı Erişim Yöneticisi'ne**ayarlamanız gerekir. Bu rol genellikle desteklenmese de, bu özel senaryoda kullanılabilir ve bu izine sahip kullanıcıların yönetilen kimliklere bir veya daha fazla özel yerleşik rol atamasına olanak sağlar. Bu **roller, devredilen RoleDefinitionIds** özelliğinde tanımlanır. Kullanıcı Erişim Yöneticisi veya Sahibi dışında herhangi bir yerleşik rolü buraya ekleyebilirsiniz.

Müşteri gemiye bindikten sonra, bu yetkilendirmede oluşturulan **principalId,** bu yerleşik rolleri müşteri kiracısında yönetilen kimliklere atayabilecektir. Ancak, normalde Kullanıcı Erişim Yöneticisi rolüyle ilişkili başka izinleri olmayacaktır.

Aşağıdaki örnekte, Kullanıcı Erişim Yöneticisi rolüne sahip bir **principalId** gösterilmektedir. Bu kullanıcı, müşteri kiracısında yönetilen kimliklere iki yerleşik rol atayabilir: Katılımcı ve Log Analytics Katılımcısı.

```json
{
    "principalId": "3kl47fff-5655-4779-b726-2cf02b05c7c4",
    "principalIdDisplayName": "Policy Automation Account",
    "roleDefinitionId": "18d7d88d-d35e-4fb5-a5c3-7773c20a72d9",
    "delegatedRoleDefinitionIds": [
         "b24988ac-6180-42a0-ab88-20f7382dd24c",
         "92aaf0da-9dab-42b6-94a3-d43ce8d16293"
    ]
}
```

## <a name="deploy-policies-that-can-be-remediated"></a>Düzeltilebilen ilkeleri dağıtma

Kullanıcıyı yukarıda açıklandığı gibi gerekli izinlerle oluşturduktan sonra, bu kullanıcı düzeltme görevlerini kullanan müşteri kiracısında ilkeleri dağıtabilir.

Örneğin, bu örnekte gösterildiği gibi, müşteri kiracısında Azure Anahtar Kasası kaynaklarında tanılamayı etkinleştirmek [istediğinizi](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/Azure-Delegated-Resource-Management/templates/policy-enforce-keyvault-monitoring)varsayalım. Bu senaryoyu etkinleştirmek için uygun izinlere sahip (yukarıda açıklandığı gibi) yönetici kiracıdaki bir kullanıcı, bir [Azure Kaynak Yöneticisi şablonu](https://github.com/Azure/Azure-Lighthouse-samples/blob/master/Azure-Delegated-Resource-Management/templates/policy-enforce-keyvault-monitoring/enforceAzureMonitoredKeyVault.json) dağıtacak.

Devralınan bir abonelikle kullanılacak ilke atamasını oluşturmanın şu anda Azure portalında değil, API'ler aracılığıyla yapılması gerektiğini unutmayın. Bunu yaparken, **apiVersion** **2019-04-01-önizleme**, yeni **temsilciManagedIdentityResourceId** özelliği ni içeren olarak ayarlanmalıdır. Bu özellik, müşteri kiracısında bulunan yönetilen bir kimliği (Azure temsilci kaynak yönetimine dahil edilmiş bir abonelik veya kaynak grubuna) eklemenize olanak tanır.

Aşağıdaki örnek, **bir temsilciManagedIdentityResourceId**ile bir rol atama gösterir.

```json
"type": "Microsoft.Authorization/roleAssignments",
            "apiVersion": "2019-04-01-preview",
            "name": "[parameters('rbacGuid')]",
            "dependsOn": [
                "[variables('policyAssignment')]"
            ],
            "properties": {
                "roleDefinitionId": "[concat(subscription().id, '/providers/Microsoft.Authorization/roleDefinitions/', variables('rbacContributor'))]",
                "principalType": "ServicePrincipal",
                "delegatedManagedIdentityResourceId": "[concat(subscription().id, '/providers/Microsoft.Authorization/policyAssignments/', variables('policyAssignment'))]",
                "principalId": "[toLower(reference(concat('/providers/Microsoft.Authorization/policyAssignments/', variables('policyAssignment')), '2018-05-01', 'Full' ).identity.principalId)]"
            }
```

> [!TIP]
> Benzer bir [örnek,](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/Azure-Delegated-Resource-Management/templates/policy-add-or-replace-tag) devredilen bir aboneye etiket ekleyen veya kaldıran (değişiklik efektini kullanarak) bir ilkenin nasıl dağıtılsüreceğini göstermek için kullanılabilir.

## <a name="next-steps"></a>Sonraki adımlar

- Azure [İlkesi](../../governance/policy/index.yml)hakkında bilgi edinin.
- Azure [kaynakları için yönetilen kimlikler](../../active-directory/managed-identities-azure-resources/overview.md)hakkında bilgi edinin.
