---
title: Azure DevTest Labs laboratuvar kullanıcısı eklemeyi otomatikleştirin | Microsoft Docs
description: Azure DevTest Labs ' de laboratuvar kullanıcısı eklemeyi otomatik hale getirmeyi öğrenin.
services: devtest-lab,lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/02/2019
ms.author: spelluru
ms.openlocfilehash: deec67a2c64a57bbb380b3fd87bf820499e6efed
ms.sourcegitcommit: 3dc1a23a7570552f0d1cc2ffdfb915ea871e257c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/15/2020
ms.locfileid: "75980062"
---
# <a name="automate-adding-a-lab-user-to-a-lab-in-azure-devtest-labs"></a>Azure DevTest Labs bir laboratuvarda laboratuvar kullanıcısı eklemeyi otomatikleştirin
Azure DevTest Labs, Azure portal kullanarak hızlı bir şekilde self servis geliştirme ve test ortamları oluşturmanıza olanak sağlar. Ancak, birden fazla ekip ve çeşitli DevTest Labs örneği varsa, oluşturma işlemini otomatik hale getirmek zamandan tasarruf edebilir. [Azure Resource Manager şablonlar](https://github.com/Azure/azure-devtestlab/tree/master/ARMTemplates) , otomatik bir biçimde laboratuvar, laboratuar VM 'leri, özel görüntüler, formüller oluşturmanıza ve Kullanıcı eklemenize olanak tanır. Bu makale özellikle bir DevTest Labs örneğine kullanıcı eklemeye odaklanır.

Bir laboratuvara Kullanıcı eklemek için, kullanıcıyı laboratuvar için **DevTest Labs Kullanıcı** rolüne eklersiniz. Bu makalede, aşağıdaki yollarla bir kullanıcıyı laboratuvara ekleme işlemini nasıl otomatikleştirebileceğiniz gösterilmektedir:

- Azure Resource Manager şablonları
- Azure PowerShell cmdlet'leri 
- Azure CLı.

## <a name="use-azure-resource-manager-templates"></a>Azure Resource Manager şablonlarını kullanma
Aşağıdaki örnek Kaynak Yöneticisi şablonu, bir laboratuvarın **DevTest Labs Kullanıcı** rolüne eklenecek bir kullanıcı belirtir. 

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "principalId": {
      "type": "string",
      "metadata": {
        "description": "The objectId of the user, group, or service principal for the role."
      }
    },
    "labName": {
      "type": "string",
      "metadata": {
        "description": "The name of the lab instance to be created."
      }
    },
    "roleAssignmentGuid": {
      "type": "string",
      "metadata": {
        "description": "Guid to use as the name for the role assignment."
      }
    }
  },
  "variables": {
    "devTestLabUserRoleId": "[concat('/subscriptions/', subscription().subscriptionId, '/providers/Microsoft.Authorization/roleDefinitions/111111111-0000-0000-11111111111111111')]",
    "fullDevTestLabUserRoleName": "[concat(parameters('labName'), '/Microsoft.Authorization/', parameters('roleAssignmentGuid'))]",
    "roleScope": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.DevTestLab/labs/', parameters('labName'))]"
  },
  "resources": [
    {
      "apiVersion": "2016-05-15",
      "type": "Microsoft.DevTestLab/labs",
      "name": "[parameters('labName')]",
      "location": "[resourceGroup().location]"
    },
    {
      "apiVersion": "2016-07-01",
      "type": "Microsoft.DevTestLab/labs/providers/roleAssignments",
      "name": "[variables('fullDevTestLabUserRoleName')]",
      "properties": {
        "roleDefinitionId": "[variables('devTestLabUserRoleId')]",
        "principalId": "[parameters('principalId')]",
        "scope": "[variables('roleScope')]"
      },
      "dependsOn": [
        "[resourceId('Microsoft.DevTestLab/labs', parameters('labName'))]"
      ]
    }
  ]
}

```

Rolü Laboratuvarı oluşturan aynı şablona atarken, rol atama kaynağı ve laboratuvar arasına bir bağımlılık eklemeyi unutmayın. Daha fazla bilgi için bkz. [Azure Resource Manager şablonları makalesinde bağımlılıkları tanımlama](../azure-resource-manager/templates/define-resource-dependency.md) .

### <a name="role-assignment-resource-information"></a>Rol atama kaynak bilgileri
Rol atama kaynağının türü ve adı belirtmesi gerekir.

İlk olarak, kaynak türü kaynak grubu için olduğu gibi `Microsoft.Authorization/roleAssignments` değildir.  Bunun yerine, kaynak türü `{provider-namespace}/{resource-type}/providers/roleAssignments`desenler izler. Bu durumda, kaynak türü `Microsoft.DevTestLab/labs/providers/roleAssignments`olacaktır.

Rol atama adının kendisinin genel olarak benzersiz olması gerekir.  Atamanın adı `{labName}/Microsoft.Authorization/{newGuid}`modelini kullanır. `newGuid`, şablon için bir parametre değeridir. Rol atama adının benzersiz olmasını sağlar. GUID 'Leri oluşturmaya yönelik şablon işlevleri olmadığından, herhangi bir GUID Oluşturucu aracı kullanarak kendiniz bir GUID oluşturmanız gerekir.  

Şablonda, rol atamasının adı `fullDevTestLabUserRoleName` değişkeni tarafından tanımlanır. Şablondaki tam satır:

```json
"fullDevTestLabUserRoleName": "[concat(parameters('labName'), '/Microsoft.Authorization/', parameters('roleAssignmentGuid'))]"
```


### <a name="role-assignment-resource-properties"></a>Rol atama kaynağı özellikleri
Rol atamasının kendisi üç özelliği tanımlar. `roleDefinitionId`, `principalId`ve `scope`gerekir.

### <a name="role-definition"></a>Rol tanımı
Rol tanımı KIMLIĞI, mevcut rol tanımının dize tanımlayıcısıdır. Rol KIMLIĞI `/subscriptions/{subscription-id}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id}`formundadır. 

Abonelik KIMLIĞI `subscription().subscriptionId` şablonu işlevi kullanılarak elde edilir.  

`DevTest Labs User` yerleşik rolünün rol tanımını almanız gerekir. [DevTest Labs Kullanıcı](../role-based-access-control/built-in-roles.md#devtest-labs-user) rolü için GUID 'yi almak üzere REST API veya [Get-azroledefinition](/powershell/module/az.resources/get-azroledefinition?view=azps-1.8.0) cmdlet 'ini kullanarak [rol atamalarını](/rest/api/authorization/roleassignments) kullanabilirsiniz.

```powershell
$dtlUserRoleDefId = (Get-AzRoleDefinition -Name "DevTest Labs User").Id
```

Rol KIMLIĞI, değişkenler bölümünde ve `devTestLabUserRoleId`adlı olarak tanımlanır. Şablonda, rol KIMLIĞI şu şekilde ayarlanır: 111111111-0000-0000-11111111111111111. 

```json
"devTestLabUserRoleId": "[concat('/subscriptions/', subscription().subscriptionId, '/providers/Microsoft.Authorization/roleDefinitions/111111111-0000-0000-11111111111111111')]",
```

### <a name="principal-id"></a>Asıl KIMLIK
Asıl KIMLIK, laboratuvara laboratuvar kullanıcısı olarak eklemek istediğiniz Active Directory Kullanıcı, Grup veya hizmet sorumlusunun nesne KIMLIĞIDIR. Şablon, `ObjectId` bir parametre olarak kullanır.

ObjectID 'yi [Get-AzureRMADUser](/powershell/module/azurerm.resources/get-azurermaduser?view=azurermps-6.13.0), [Get-AzureRMADGroup veya [Get-AzureRMADServicePrincipal](/powershell/module/azurerm.resources/get-azurermadserviceprincipal?view=azurermps-6.13.0) PowerShell cmdlet 'lerini kullanarak edinebilirsiniz. Bu cmdlet 'ler, gereken nesne KIMLIĞI olan bir ID özelliğine sahip Active Directory nesnelerinin tek veya bir listesini döndürür. Aşağıdaki örnek, bir şirketteki tek bir kullanıcının nesne KIMLIĞINI nasıl alınacağını gösterir.

```powershell
$userObjectId = (Get-AzureRmADUser -UserPrincipalName ‘email@company.com').Id
```

[Get-MsolUser](/powershell/module/msonline/get-msoluser?view=azureadps-1.0), [Get-msolgroup](/powershell/module/msonline/get-msolgroup?view=azureadps-1.0)ve [Get-msolserviceprincipal](/powershell/module/msonline/get-msolserviceprincipal?view=azureadps-1.0)dahil Azure Active Directory PowerShell cmdlet 'lerini de kullanabilirsiniz.

### <a name="scope"></a>Kapsam
Kapsam, rol atamasının uygulanacağı kaynağı veya kaynak grubunu belirtir. Kaynaklar için kapsam şu biçimdedir: `/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/{provider-namespace}/{resource-type}/{resource-name}`. Şablon, `resource-group-name` parçasını dolduracak şekilde `subscription-id` bölümünü ve `resourceGroup().name` şablonu işlevini dolduracak `subscription().subscriptionId` işlevini kullanır. Bu işlevlerin kullanılması, rol atadığınız laboratuvarın geçerli abonelikte ve şablon dağıtımının yapıldığı aynı kaynak grubunda mevcut olması gerektiği anlamına gelir. Son bölüm `resource-name`, laboratuvarın adıdır. Bu değer, bu örnekteki şablon parametresi aracılığıyla alınır. 

Şablondaki rol kapsamı: 

```json
"roleScope": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.DevTestLab/labs/', parameters('labName'))]"
```

### <a name="deploying-the-template"></a>Şablon dağıtma
İlk olarak, Kaynak Yöneticisi şablonundaki parametrelerin değerlerini geçiren bir parametre dosyası (örneğin: azuredeploy. Parameters. JSON) oluşturun. 

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "principalId": {
      "value": "11111111-1111-1111-1111-111111111111"
    },
    "labName": {
      "value": "MyLab"
    },
    "roleAssignmentGuid": {
      "value": "22222222-2222-2222-2222-222222222222"
    }
  }
}
```

Ardından, Kaynak Yöneticisi şablonunu dağıtmak için [New-AzureRmResourceGroupDeployment](/powershell/module/azurerm.resources/new-azurermresourcegroupdeployment?view=azurermps-6.13.0) PowerShell cmdlet 'ini kullanın. Aşağıdaki örnek komut, bir Kullanıcı, Grup veya hizmet sorumlusunu bir laboratuvar için DevTest Labs kullanıcı rolüne atar.

```powershell
New-AzureRmResourceGroupDeployment -Name "MyLabResourceGroup-$(New-Guid)" -ResourceGroupName 'MyLabResourceGroup' -TemplateParameterFile .\azuredeploy.parameters.json -TemplateFile .\azuredeploy.json
```

Grup dağıtım adı ve rol atama GUID 'inin benzersiz olması gerektiğini unutmayın. Benzersiz olmayan bir GUID ile bir kaynak ataması dağıtmayı denerseniz, `RoleAssignmentUpdateNotPermitted` bir hata alırsınız.

Laboratuvar için DevTest Labs kullanıcı rolüne birkaç Active Directory nesnesi eklemek için şablonu kullanmayı planlıyorsanız, PowerShell komutınızda dinamik nesneleri kullanmayı düşünün. Aşağıdaki örnek, kaynak grubu dağıtım adı ve rol atama GUID 'sini dinamik olarak belirtmek için [New-Guid](/powershell/module/Microsoft.PowerShell.Utility/New-Guid?view=powershell-5.0) cmdlet 'ini kullanır.

```powershell
New-AzureRmResourceGroupDeployment -Name "MyLabResourceGroup-$(New-Guid)" -ResourceGroupName 'MyLabResourceGroup' -TemplateFile .\azuredeploy.json -roleAssignmentGuid "$(New-Guid)" -labName "MyLab" -principalId "11111111-1111-1111-1111-111111111111"
```

## <a name="use-azure-powershell"></a>Azure PowerShell’i kullanma
Giriş bölümünde anlatıldığı gibi, laboratuvar için **DevTest Labs Kullanıcı** rolüne bir kullanıcı eklemek için yeni bir Azure rol ataması oluşturursunuz. PowerShell 'de, [New-Azurermroleatama](/powershell/module/azurerm.resources/new-azurermroleassignment?view=azurermps-6.13.0) cmdlet 'ini kullanarak bunu yapabilirsiniz. Bu cmdlet 'in esneklik için izin verilen birçok isteğe bağlı parametresi vardır. `ObjectId`, `SigninName`veya `ServicePrincipalName`, izin verilen nesne olarak belirlenebilir.  

Aşağıda, belirtilen laboratuvardaki DevTest Labs kullanıcı rolüne bir Kullanıcı ekleyen örnek bir Azure PowerShell komutu verilmiştir.

```powershell
New-AzureRmRoleAssignment -UserPrincipalName <email@company.com> -RoleDefinitionName 'DevTest Labs User' -ResourceName '<Lab Name>' -ResourceGroupName '<Resource Group Name>' -ResourceType 'Microsoft.DevTestLab/labs'
```

İzinlerin verildiği kaynağı belirtmek için `ResourceName`, `ResourceType`, `ResourceGroup` veya `scope` parametresi ile birlikte belirtilebilir. Hangi parametre bileşimleri kullanılırsa, cmdlet 'e Active Directory nesnesini (Kullanıcı, Grup veya hizmet sorumlusu), kapsamı (kaynak grubu veya kaynağı) ve rol tanımını benzersiz şekilde tanımlamak için yeterli bilgi sağlayın.

## <a name="use-azure-command-line-interface-cli"></a>Azure komut satırı arabirimini (CLı) kullanma
Azure CLı 'de laboratuvara bir laboratuvar kullanıcısı eklemek `az role assignment create` komutu kullanılarak yapılır. Azure CLı cmdlet 'leri hakkında daha fazla bilgi için bkz. [RBAC ve Azure CLI kullanarak Azure kaynaklarına erişimi yönetme](../role-based-access-control/role-assignments-cli.md).

Erişim izni verilen nesne `objectId`, `signInName`, `spn` parametreleriyle belirtilebilir. Nesnenin erişim izni verildiği laboratuvar `scope` URL 'si veya `resource-name`, `resource-type`ve `resource-group` parametrelerinin bir birleşimi ile tanımlanabilir.

Aşağıdaki Azure CLı örneği, belirtilen laboratuvar için DevTest Labs kullanıcı rolüne nasıl bir kişi ekleneceğini gösterir.  

```azurecli
az role assignment create --roleName "DevTest Labs User" --signInName <email@company.com> -–resource-name "<Lab Name>" --resource-type “Microsoft.DevTestLab/labs" --resource-group "<Resource Group Name>"
```

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makalelere bakın:

- [Azure CLı kullanarak DevTest Labs ile sanal makineler oluşturma ve yönetme](devtest-lab-vmcli.md)
- [Azure PowerShell kullanarak DevTest Labs ile sanal makine oluşturma](devtest-lab-vm-powershell.md)
- [Azure DevTest Labs sanal makineleri başlatmak ve durdurmak için komut satırı araçlarını kullanma](use-command-line-start-stop-virtual-machines.md)

