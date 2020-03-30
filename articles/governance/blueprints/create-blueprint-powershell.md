---
title: 'Quickstart: PowerShell ile bir plan oluşturun'
description: Bu hızlı başlangıçta, PowerShell'i kullanarak yapı oluşturmak, tanımlamak ve dağıtmak için Azure Planları'nı kullanırsınız.
ms.date: 11/21/2019
ms.topic: quickstart
ms.openlocfilehash: 8b418fa2b5244c42b8597bbbe7ed4773133d03a8
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "75436630"
---
# <a name="quickstart-define-and-assign-an-azure-blueprint-with-powershell"></a>Quickstart: PowerShell ile Azure Blueprint tanımlayın ve Atayın

Şema oluşturma ve atama süreçlerini anlamak, ortak tutarlılık desenlerini tanımlamanızı ve Resource Manager şablonlarını, ilkelerini, güvenlik düzeyini ve daha fazlasını temel alan yeniden kullanılabilir ve hızla dağıtılabilir yapılandırmalar geliştirmenizi sağlar. Bu öğreticide kuruluşunuzda aşağıdakiler gibi şema oluşturma, yayımlama ve atama konusundaki yaygın görevlerin bazılarını yerine getirmek için Azure Blueprints'i kullanmayı öğreneceksiniz:

## <a name="prerequisites"></a>Ön koşullar

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free) bir hesap oluşturun.

Henüz yüklenmemişse, PowerShell Galerisi'nden **Az.Blueprint** modüllerini yüklemek ve doğrulamak için [Az.Blueprint modülekle modülündeki](./how-to/manage-assignments-ps.md#add-the-azblueprint-module) yönergeleri izleyin.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="create-a-blueprint"></a>Şema oluşturma

Uyumluluk için standart desen tanımlamanın ilk adımı kullanılabilir durumdaki kaynaklardan bir şema oluşturmaktır. Abonelik için rol ve ilke atamalarını yapılandırmak üzere 'MyBlueprint' adlı bir şema oluşturacağız. Ardından bir kaynak grubu ekleyecek ve bu kaynak grubuna da bir Resource Manager şablonu ve rol ataması ekleyeceğiz.

> [!NOTE]
> PowerShell kullanırken, _plan_ nesnesi önce oluşturulur. Eklenecek ve parametreye sahip olan her _yapıt_ için parametrelerin önceden ilk _şema_ içinde tanımlanması gerekir.

1. İlk _şema_ nesnesini oluşturun. **BlueprintFile** parametresi, plan, oluşturulacak tüm kaynak grupları ve tüm plan düzeyi parametreleri hakkında özellikleri içeren bir JSON dosyasını alır. Parametreler atama sırasında ayarlanır ve sonraki adımlarda eklenecek yapıtlar tarafından kullanılır.

   - JSON dosyası - blueprint.json

     ```json
     {
         "properties": {
             "description": "This blueprint sets tag policy and role assignment on the subscription, creates a ResourceGroup, and deploys a resource template and role assignment to that ResourceGroup.",
             "targetScope": "subscription",
             "parameters": {
                 "storageAccountType": {
                     "type": "string",
                     "defaultValue": "Standard_LRS",
                     "allowedValues": [
                         "Standard_LRS",
                         "Standard_GRS",
                         "Standard_ZRS",
                         "Premium_LRS"
                     ],
                     "metadata": {
                         "displayName": "storage account type.",
                         "description": null
                     }
                 },
                 "tagName": {
                     "type": "string",
                     "metadata": {
                         "displayName": "The name of the tag to provide the policy assignment.",
                         "description": null
                     }
                 },
                 "tagValue": {
                     "type": "string",
                     "metadata": {
                         "displayName": "The value of the tag to provide the policy assignment.",
                         "description": null
                     }
                 },
                 "contributors": {
                     "type": "array",
                     "metadata": {
                         "description": "List of AAD object IDs that is assigned Contributor role at the subscription",
                         "strongType": "PrincipalId"
                     }
                 },
                 "owners": {
                     "type": "array",
                     "metadata": {
                         "description": "List of AAD object IDs that is assigned Owner role at the resource group",
                         "strongType": "PrincipalId"
                     }
                 }
             },
             "resourceGroups": {
                 "storageRG": {
                     "description": "Contains the resource template deployment and a role assignment."
                 }
             }
         }
     }
     ```

   - PowerShell komutu

     ```azurepowershell-interactive
     # Login first with Connect-AzAccount if not using Cloud Shell

     # Get a reference to the new blueprint object, we'll use it in subsequent steps
     $blueprint = New-AzBlueprint -Name 'MyBlueprint' -BlueprintFile .\blueprint.json
     ```

     > [!NOTE]
     > Plan tanımlarınızı programlı bir şekilde oluştururken dosya adı _blueprint.json'u_ kullanın.
     > Bu dosya [adı, Alma-AzBlueprintWithArtifact'i](/powershell/module/az.blueprint/import-azblueprintwithartifact)ararken kullanılır.

     Plan nesnesi varsayılan olarak varsayılan abonelik oluşturulur. Yönetim grubunu belirtmek için **ManagementGroupId**parametresi kullanın. Aboneliği belirtmek **için, abonelik**parametresi kullanın.

1. Abonelikte rol ataması ekleyin. **ArtifactFile** yapı _türünü_ tanımlar, özellikler rol tanımı tanımlayıcısına hizalanır ve temel kimlikler bir dizi değer olarak geçirilir. Aşağıdaki örnekte belirtilen rolün verildiği sorumlu kimlikleri, şema ataması sırasında ayarlanan bir parametreyle yapılandırılmıştır. Bu _örnekte, Bir_ GUID ile Katılımcı `b24988ac-6180-42a0-ab88-20f7382dd24c`yerleşik rolü kullanır.

   - JSON dosyası - \artefakt\roleContributor.json

     ```json
     {
         "kind": "roleAssignment",
         "properties": {
             "roleDefinitionId": "/providers/Microsoft.Authorization/roleDefinitions/b24988ac-6180-42a0-ab88-20f7382dd24c",
             "principalIds": "[parameters('contributors')]"
         }
     }
     ```

   - PowerShell komutu

     ```azurepowershell-interactive
     # Use the reference to the new blueprint object from the previous steps
     New-AzBlueprintArtifact -Blueprint $blueprint -Name 'roleContributor' -ArtifactFile .\artifacts\roleContributor.json
     ```

1. Abonelikte ilke ataması ekleyin. **Artefakt Dosyası** yapı _türünü,_ bir ilke veya girişim tanımına uygun özellikleri tanımlar ve plan ataması sırasında yapılandırmak için tanımlanan plan parametrelerini kullanmak üzere ilke atamasını yapılandırır. Bu örnekte _Uygula etiketi ve varsayılan değeri,_ yerleşik kaynak gruplarına guid `49c88fc8-6fd1-46fd-a676-f12d1d3a4c71`ile .

   - JSON dosyası - \artefakt\policyTags.json

     ```json
     {
         "kind": "policyAssignment",
         "properties": {
             "displayName": "Apply tag and its default value to resource groups",
             "description": "Apply tag and its default value to resource groups",
             "policyDefinitionId": "/providers/Microsoft.Authorization/policyDefinitions/49c88fc8-6fd1-46fd-a676-f12d1d3a4c71",
             "parameters": {
                 "tagName": {
                     "value": "[parameters('tagName')]"
                 },
                 "tagValue": {
                     "value": "[parameters('tagValue')]"
                 }
             }
         }
     }
     ```

   - PowerShell komutu

     ```azurepowershell-interactive
     # Use the reference to the new blueprint object from the previous steps
     New-AzBlueprintArtifact -Blueprint $blueprint -Name 'policyTags' -ArtifactFile .\artifacts\policyTags.json
     ```

1. Abonelikte Depolama etiketi için (_storageAccountType_ parametresini yeniden kullanarak) başka bir ilke ataması ekleyin. Bu ek ilke ataması yapıtı, şemada tanımlanan bir parametrenin birden fazla yapıt tarafından kullanılabileceğini gösterir. Örnekte kaynak grubunda etiket ayarlamak için **storageAccountType** kullanılmıştır. Bu değer, bir sonraki adımda oluşturulan depolama hesabıyla ilgili bilgi sağlar. Bu örnekte _Uygula etiketi ve varsayılan değeri,_ yerleşik kaynak gruplarına guid `49c88fc8-6fd1-46fd-a676-f12d1d3a4c71`ile .

   - JSON dosyası - \artefakt\policyStorageTags.json

     ```json
     {
         "kind": "policyAssignment",
         "properties": {
             "displayName": "Apply storage tag to resource group",
             "description": "Apply storage tag and the parameter also used by the template to resource groups",
             "policyDefinitionId": "/providers/Microsoft.Authorization/policyDefinitions/49c88fc8-6fd1-46fd-a676-f12d1d3a4c71",
             "parameters": {
                 "tagName": {
                     "value": "StorageType"
                 },
                 "tagValue": {
                     "value": "[parameters('storageAccountType')]"
                 }
             }
         }
     }
     ```

   - PowerShell komutu

     ```azurepowershell-interactive
     # Use the reference to the new blueprint object from the previous steps
     New-AzBlueprintArtifact -Blueprint $blueprint -Name 'policyStorageTags' -ArtifactFile .\artifacts\policyStorageTags.json
     ```

1. Kaynak grubuna şablon ekleyin. Kaynak Yöneticisi şablonu için **TemplateFile** şablonun normal JSON bileşenini içerir. Şablon aynı zamanda **storageAccountType**, **tagName** ve **tagValue** şema parametrelerini şablona geçirerek hepsini yeniden kullanır. Plan parametreleri şablon için şablon için template **Parametre TemplateParameterFile** ve şablon içinde anahtar değeri çifti değeri enjekte etmek için kullanılır JSON kullanılabilir. Plan ve şablon parametre adları aynı olabilir.

   - JSON Azure Kaynak Yöneticisi şablon dosyası - \artifakı\templateStorage.json

     ```json
     {
         "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
         "contentVersion": "1.0.0.0",
         "parameters": {
             "storageAccountTypeFromBP": {
                 "type": "string",
                 "metadata": {
                     "description": "Storage Account type"
                 }
             },
             "tagNameFromBP": {
                 "type": "string",
                 "defaultValue": "NotSet",
                 "metadata": {
                     "description": "Tag name from blueprint"
                 }
             },
             "tagValueFromBP": {
                 "type": "string",
                 "defaultValue": "NotSet",
                 "metadata": {
                     "description": "Tag value from blueprint"
                 }
             }
         },
         "variables": {
             "storageAccountName": "[concat(uniquestring(resourceGroup().id), 'standardsa')]"
         },
         "resources": [{
             "type": "Microsoft.Storage/storageAccounts",
             "name": "[variables('storageAccountName')]",
             "apiVersion": "2016-01-01",
             "tags": {
                 "[parameters('tagNameFromBP')]": "[parameters('tagValueFromBP')]"
             },
             "location": "[resourceGroup().location]",
             "sku": {
                 "name": "[parameters('storageAccountTypeFromBP')]"
             },
             "kind": "Storage",
             "properties": {}
         }],
         "outputs": {
             "storageAccountSku": {
                 "type": "string",
                 "value": "[variables('storageAccountName')]"
             }
         }
     }
     ```

   - JSON Azure Kaynak Yöneticisi şablon parametre dosyası - \artifaks\templateStorageParams.json

     ```json
     {
         "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
         "contentVersion": "1.0.0.0",
         "parameters": {
             "storageAccountTypeFromBP": {
                 "value": "[parameters('storageAccountType')]"
             },
             "tagNameFromBP": {
                 "value": "[parameters('tagName')]"
             },
             "tagValueFromBP": {
                 "value": "[parameters('tagValue')]"
             }
         }
     }
     ```

   - PowerShell komutu

     ```azurepowershell-interactive
     # Use the reference to the new blueprint object from the previous steps
     New-AzBlueprintArtifact -Blueprint $blueprint -Type TemplateArtifact -Name 'templateStorage' -TemplateFile .\artifacts\templateStorage.json -TemplateParameterFile .\artifacts\templateStorageParams.json -ResourceGroupName storageRG
     ```

1. Rol atamasını kaynak grubuna ekleyin. Yukarıdaki rol ataması girişine benzer şekilde aşağıdaki örnekte de **Sahip** rolü için tanımlayıcı kullanılır ve şemadan farklı bir parametre sunulur. Bu örnek, Bir GUID ile _Sahibi_ `8e3af657-a8ff-443c-a75c-2fe8c4bcb635`yerleşik rolü kullanır.

   - JSON dosyası - \artefakt\roleOwner.json

     ```json
     {
         "kind": "roleAssignment",
         "properties": {
             "resourceGroup": "storageRG",
             "roleDefinitionId": "/providers/Microsoft.Authorization/roleDefinitions/8e3af657-a8ff-443c-a75c-2fe8c4bcb635",
             "principalIds": "[parameters('owners')]"
         }
     }
     ```

   - PowerShell komutu

     ```azurepowershell-interactive
     # Use the reference to the new blueprint object from the previous steps
     New-AzBlueprintArtifact -Blueprint $blueprint -Name 'roleOwner' -ArtifactFile .\artifacts\roleOwner.json
     ```

## <a name="publish-a-blueprint"></a>Şemayı yayımlama

Yapıtları ekledikten sonra şemayı yayımlayabilirsiniz. Yayımladığınızda şema bir aboneliğe atanmaya hazır hale gelir.

```azurepowershell-interactive
# Use the reference to the new blueprint object from the previous steps
Publish-AzBlueprint -Blueprint $blueprint -Version '{BlueprintVersion}'
```

`{BlueprintVersion}` değeri en fazla 20 karakter olmak üzere harf, rakam ve kısa çizgilerden oluşan bir dizedir (boşluk veya özel karakter kullanılamaz). **v20180622-135541** gibi benzersiz ve bilgilendirici bir değer kullanın.

## <a name="assign-a-blueprint"></a>Şema atama

PowerShell kullanılarak bir plan yayımlandıktan sonra, bir aboneye atayılabilir. Oluşturduğunuz şemayı yönetim grubu hiyerarşinizdeki aboneliklerden birine atayın. Plan bir aboneye kaydedilirse, yalnızca bu aboneye atanabilir. **Blueprint** parametresi atamak için planı belirtir. Ad, konum, kimlik, kilit ve plan parametrelerini sağlamak için `New-AzBlueprintAssignment` cmdlet'teki eşleşen PowerShell parametrelerini kullanın veya **Bunları AssignmentFile** parametre JSON dosyasında sağlayın.

1. Bir aboneliğe atayarak şema dağıtımını çalıştırın. Katılımcı **ve** **sahip** parametreleri rol atamasının verilebilmek için bir dizi anabilim prensibini gerektirdiğinden, kendi kullanıcılarınız, gruplarınız veya hizmet ilkeleriniz için **AssignmentFile'da** kullanılmak üzere objectId'leri toplamak için [Azure Active Directory Graph API'yi](../../active-directory/develop/active-directory-graph-api.md) kullanın.

   - JSON dosyası - blueprintAssignment.json

     ```json
     {
         "properties": {
             "blueprintId": "/providers/Microsoft.Management/managementGroups/{YourMG}/providers/Microsoft.Blueprint/blueprints/MyBlueprint",
             "resourceGroups": {
                 "storageRG": {
                     "name": "StorageAccount",
                     "location": "eastus2"
                 }
             },
             "parameters": {
                 "storageAccountType": {
                     "value": "Standard_GRS"
                 },
                 "tagName": {
                     "value": "CostCenter"
                 },
                 "tagValue": {
                     "value": "ContosoIT"
                 },
                 "contributors": {
                     "value": [
                         "7be2f100-3af5-4c15-bcb7-27ee43784a1f",
                         "38833b56-194d-420b-90ce-cff578296714"
                     ]
                 },
                 "owners": {
                     "value": [
                         "44254d2b-a0c7-405f-959c-f829ee31c2e7",
                         "316deb5f-7187-4512-9dd4-21e7798b0ef9"
                     ]
                 }
             }
         },
         "identity": {
             "type": "systemAssigned"
         },
         "location": "westus"
     }
     ```

   - PowerShell komutu

     ```azurepowershell-interactive
     # Use the reference to the new blueprint object from the previous steps
     New-AzBlueprintAssignment -Blueprint $blueprint -Name 'assignMyBlueprint' -AssignmentFile .\blueprintAssignment.json
     ```

   - Kullanıcı tarafından atanan yönetilen kimlik

     Plan [ataması, kullanıcı tarafından atanan yönetilen bir kimliği](../../active-directory/managed-identities-azure-resources/overview.md)de kullanabilir.
     Bu durumda, JSON atama dosyasının **kimlik** bölümü aşağıdaki gibi değişir. Sırasıyla, tenantId, subscriptionId, kaynak grup adı ve kullanıcı tarafından atanan yönetilen kimliğinizin adını değiştirin. `{tenantId}` `{subscriptionId}` `{yourRG}` `{userIdentity}`

     ```json
     "identity": {
         "type": "userAssigned",
         "tenantId": "{tenantId}",
         "userAssignedIdentities": {
             "/subscriptions/{subscriptionId}/resourceGroups/{yourRG}/providers/Microsoft.ManagedIdentity/userAssignedIdentities/{userIdentity}": {}
         }
     },
     ```

     **Kullanıcı tarafından atanan yönetilen kimlik,** planı atadığı kullanıcının izinleri olan herhangi bir abonelik ve kaynak grubunda olabilir.

     > [!IMPORTANT]
     > Planlar, kullanıcı tarafından atanan yönetilen kimliği yönetmez. Kullanıcılar yeterli rolleri ve izinleri atamaktan sorumludur veya plan ataması başarısız olur.

## <a name="clean-up-resources"></a>Kaynakları temizleme

### <a name="unassign-a-blueprint"></a>Şema atamasını kaldırma

Bir şemayı abonelikten kaldırabilirsiniz. Kaldırma işlemi genellikle yapıt kaynaklarına ihtiyaç duyulmadığında gerçekleştirilir. Bir şema kaldırıldığında o şemanın bir parçası olarak atanan yapıtlar geride kalır. Plan atamasını kaldırmak için `Remove-AzBlueprintAssignment` cmdlet'i kullanın:

atamaMyBlueprint

```azurepowershell-interactive
Remove-AzBlueprintAssignment -Name 'assignMyBlueprint'
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, PowerShell ile bir plan oluşturdunuz, atamış ve kaldırdınız. Azure Planları hakkında daha fazla bilgi edinmek için, plan yaşam döngüsü makalesine devam edin.

> [!div class="nextstepaction"]
> [Plan yaşam döngüsü hakkında bilgi edinin](./concepts/lifecycle.md)