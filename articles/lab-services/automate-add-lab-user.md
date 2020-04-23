---
title: Azure DevTest Labs'da laboratuvar kullanıcıeklemeyi otomatikleştirin | Microsoft Dokümanlar
description: Bu makalede, Azure Kaynak Yöneticisi şablonları, PowerShell ve CLI kullanarak Azure DevTest Labs'daki bir laboratuvara kullanıcı eklemeyi otomatikleştirin.
services: devtest-lab,lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2020
ms.author: spelluru
ms.openlocfilehash: 81a8c5030f716246caf3dcd8b540bb47fcaf6520
ms.sourcegitcommit: 75089113827229663afed75b8364ab5212d67323
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "82023631"
---
# <a name="automate-adding-a-lab-user-to-a-lab-in-azure-devtest-labs"></a>Azure DevTest Labs'daki bir laboratuvara laboratuvar kullanıcıeklemeyi otomatikleştirme
Azure DevTest Labs, Azure portalını kullanarak self servis geliştirme test ortamlarını hızla oluşturmanıza olanak tanır. Ancak, birkaç takım ve birkaç DevTest Labs örnekleri varsa, oluşturma işlemini otomatikleştirmek zaman kazandırabilir. [Azure Kaynak Yöneticisi şablonları,](https://github.com/Azure/azure-devtestlab/tree/master/Environments) laboratuvarlar, laboratuvar VM'leri, özel resimler, formüller oluşturmanıza ve kullanıcıları otomatik bir şekilde eklemenize olanak tanır. Bu makale, özellikle bir DevTest Labs örneğine kullanıcı eklemeye odaklanır.

Bir kullanıcıyı laboratuvara eklemek için, kullanıcıyı laboratuvar için **DevTest Labs Kullanıcı** rolüne eklersiniz. Bu makalede, aşağıdaki yollardan birini kullanarak bir laboratuvara kullanıcı eklemeyi otomatikleştirin:

- Azure Resource Manager şablonları
- Azure PowerShell cmdlet’leri 
- Azure CLI.

## <a name="use-azure-resource-manager-templates"></a>Azure Resource Manager şablonlarını kullanma
Aşağıdaki örnek Kaynak Yöneticisi şablonu, bir laboratuvarın **DevTest Labs Kullanıcı** rolüne eklenecek bir kullanıcıyı belirtir. 

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

Rolü, laboratuarı oluşturan aynı şablonda atadıysanız, rol atama kaynağı ile laboratuar arasında bir bağımlılık eklemeyi unutmayın. Daha fazla bilgi için Azure [Kaynak Yöneticisi Şablonları makalesinde bağımlılıkları tanımlama makalesine](../azure-resource-manager/templates/define-resource-dependency.md) bakın.

### <a name="role-assignment-resource-information"></a>Rol Atama Kaynak Bilgileri
Rol atama kaynağının türü ve adını belirtmesi gerekir.

Unutulmaması gereken ilk şey, kaynak için tür `Microsoft.Authorization/roleAssignments` bir kaynak grubu için olduğu gibi değildir.  Bunun yerine, kaynak türü `{provider-namespace}/{resource-type}/providers/roleAssignments`deseni izler. Bu durumda, kaynak türü `Microsoft.DevTestLab/labs/providers/roleAssignments`.

Rol atama adı kendisi küresel benzersiz olması gerekir.  Atamanın adı deseni `{labName}/Microsoft.Authorization/{newGuid}`kullanır. Şablon `newGuid` için bir parametre değeridir. Rol atama adının benzersiz olmasını sağlar. GUID'ler oluşturmak için hiçbir şablon işlevleri olduğundan, herhangi bir GUID jeneratör aracı kullanarak kendiniz bir GUID oluşturmanız gerekir.  

Şablonda, rol atamasının adı `fullDevTestLabUserRoleName` değişken tarafından tanımlanır. Şablondan tam satır:

```json
"fullDevTestLabUserRoleName": "[concat(parameters('labName'), '/Microsoft.Authorization/', parameters('roleAssignmentGuid'))]"
```


### <a name="role-assignment-resource-properties"></a>Rol Atama Kaynak Özellikleri
Rol ataması kendisi üç özelliği tanımlar. İhtiyacı `roleDefinitionId`var. `principalId` `scope`

### <a name="role-definition"></a>Rol Tanımı
Rol tanımı kimliği, varolan rol tanımının dize tanımlayıcısIdır. Rol kimliği şeklindedir. `/subscriptions/{subscription-id}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id}` 

Abonelik kimliği şablon işlevi `subscription().subscriptionId` kullanılarak elde edilir.  

Yerleşik rol için rol tanımını `DevTest Labs User` almanız gerekir. [DevTest Labs Kullanıcı](../role-based-access-control/built-in-roles.md#devtest-labs-user) rolü için GUID almak için, [Rol Atamaları REST API](/rest/api/authorization/roleassignments) veya [Get-AzRoleDefinition](/powershell/module/az.resources/get-azroledefinition?view=azps-1.8.0) cmdlet kullanabilirsiniz.

```powershell
$dtlUserRoleDefId = (Get-AzRoleDefinition -Name "DevTest Labs User").Id
```

Rol kimliği değişkenler bölümünde tanımlanır ve `devTestLabUserRoleId`. Şablonda, rol kimliği ayarlanır: 11111111111-0000-1111111111111111. 

```json
"devTestLabUserRoleId": "[concat('/subscriptions/', subscription().subscriptionId, '/providers/Microsoft.Authorization/roleDefinitions/111111111-0000-0000-11111111111111111')]",
```

### <a name="principal-id"></a>Asıl Kimlik
Asıl kimlik, laboratuvar kullanıcısı olarak eklemek istediğiniz Active Directory kullanıcısının, grubunun veya hizmet yöneticisinin nesne kimliğidir. Şablon parametre `ObjectId` olarak kullanır.

ObjectId'yi [Get-AzureRMADUser](/powershell/module/azurerm.resources/get-azurermaduser?view=azurermps-6.13.0), [Get-AzureRMADGroup veya [Get-AzureRMADServicePrincipal](/powershell/module/azurerm.resources/get-azurermadserviceprincipal?view=azurermps-6.13.0) PowerShell cmdlets' i kullanarak alabilirsiniz. Bu cmdlets, ihtiyacınız olan nesne kimliği olan bir kimlik özelliğine sahip Etkin Dizin nesnelerinin tek bir veya listesini döndürer. Aşağıdaki örnek, bir şirketteki tek bir kullanıcının nesne kimliğini nasıl alacağınızı gösterir.

```powershell
$userObjectId = (Get-AzureRmADUser -UserPrincipalName ‘email@company.com').Id
```

[Ayrıca Get-MsolUser](/powershell/module/msonline/get-msoluser?view=azureadps-1.0), [Get-MsolGroup](/powershell/module/msonline/get-msolgroup?view=azureadps-1.0)ve [Get-MsolServicePrincipal](/powershell/module/msonline/get-msolserviceprincipal?view=azureadps-1.0)gibi Azure Active Directory PowerShell cmdlets'i de kullanabilirsiniz.

### <a name="scope"></a>Kapsam
Kapsam, rol atamasının uygulanması gereken kaynak veya kaynak grubunu belirtir. Kaynaklar için kapsam şu şekildedir: `/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/{provider-namespace}/{resource-type}/{resource-name}`. `subscription().subscriptionId` Şablon, parçayı `subscription-id` doldurmak için işlevi `resourceGroup().name` ve `resource-group-name` parçayı doldurmak için şablon işlevini kullanır. Bu işlevleri kullanmak, bir rol atadığınız laboratuvarın geçerli abonelikte ve şablon dağıtımının yapıldığı aynı kaynak grubunda bulunması gerektiği anlamına gelir. Son bölüm, `resource-name`laboratuvarın adı. Bu değer, bu örnekte şablon parametresi üzerinden alınır. 

Şablondaki rol kapsamı: 

```json
"roleScope": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.DevTestLab/labs/', parameters('labName'))]"
```

### <a name="deploying-the-template"></a>Şablon dağıtma
İlk olarak, Kaynak Yöneticisi şablonundaki parametreleriçin değerleri geçen bir parametre dosyası (örneğin: azuredeploy.parameters.json) oluşturun. 

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

Ardından, Kaynak Yöneticisi şablonuna dağıtmak için [Yeni AzureRmResourceGroupDeployment](/powershell/module/azurerm.resources/new-azurermresourcegroupdeployment?view=azurermps-6.13.0) PowerShell cmdlet'ini kullanın. Aşağıdaki örnek komut, bir laboratuvar için DevTest Labs Kullanıcı rolüne bir kişi, grup veya bir hizmet müdürü atar.

```powershell
New-AzureRmResourceGroupDeployment -Name "MyLabResourceGroup-$(New-Guid)" -ResourceGroupName 'MyLabResourceGroup' -TemplateParameterFile .\azuredeploy.parameters.json -TemplateFile .\azuredeploy.json
```

Grup dağıtım adı ve rol atamaGUID benzersiz olması gerektiğini unutmayın. Benzersiz olmayan bir GUID ile bir kaynak ataması dağıtmaya çalışırsanız, bir `RoleAssignmentUpdateNotPermitted` hata alırsınız.

Şablonu, laboratuvarınız için DevTest Labs Kullanıcı rolüne birkaç Active Directory nesnesi eklemek için birkaç kez kullanmayı planlıyorsanız, PowerShell komutunuzda dinamik nesneler kullanmayı düşünün. Aşağıdaki örnekte, kaynak grubu dağıtım adını ve rol atamaGUID'i dinamik olarak belirtmek için [Yeni Kılavuz](/powershell/module/Microsoft.PowerShell.Utility/New-Guid?view=powershell-5.0) cmdlet kullanır.

```powershell
New-AzureRmResourceGroupDeployment -Name "MyLabResourceGroup-$(New-Guid)" -ResourceGroupName 'MyLabResourceGroup' -TemplateFile .\azuredeploy.json -roleAssignmentGuid "$(New-Guid)" -labName "MyLab" -principalId "11111111-1111-1111-1111-111111111111"
```

## <a name="use-azure-powershell"></a>Azure PowerShell kullanma
Girişbölümünde anlatıldığı gibi, laboratuvar için **DevTest Labs Kullanıcı** rolüne bir kullanıcı eklemek için yeni bir Azure rol ataması oluşturursunuz. PowerShell'de, Bunu Yeni [AzureRMRoleAssignment](/powershell/module/azurerm.resources/new-azurermroleassignment?view=azurermps-6.13.0) cmdlet'ini kullanarak yaparsınız. Bu cmdlet esneklik sağlamak için birçok isteğe bağlı parametrelere sahiptir. `ObjectId`, `SigninName`, `ServicePrincipalName` veya izin verilen nesne olarak belirtilebilir.  

Burada, belirtilen laboratuvardaki DevTest Labs Kullanıcı rolüne bir kullanıcı ekleyen bir örnek Azure PowerShell komutu verem.

```powershell
New-AzureRmRoleAssignment -UserPrincipalName <email@company.com> -RoleDefinitionName 'DevTest Labs User' -ResourceName '<Lab Name>' -ResourceGroupName '<Resource Group Name>' -ResourceType 'Microsoft.DevTestLab/labs'
```

İzinlerin verildiği kaynağı belirtmek `ResourceName`için , `ResourceType` `ResourceGroup` yani parametrenin `scope` bir birleşimi ile belirtilebilir. Parametrelerin birleşimi ne olursa olsun, cmdlet'e Active Directory nesnesini (kullanıcı, grup veya hizmet anasını), kapsamı (kaynak grubu veya kaynak) ve rol tanımını benzersiz olarak tanımlamak için yeterli bilgi sağlayın.

## <a name="use-azure-command-line-interface-cli"></a>Azure Komut Satırı Arabirimi (CLI) kullanın
Azure CLI'de, laboratuvara laboratuvar kullanıcısı eklemek `az role assignment create` komutu kullanarak yapılır. Azure CLI cmdlets hakkında daha fazla bilgi için Bkz. [RBAC ve Azure CLI kullanarak Azure kaynaklarına erişimi yönet.](../role-based-access-control/role-assignments-cli.md)

Erişim hakkı verilen nesne `objectId`, , `signInName` `spn` parametrelertarafından belirtilebilir. Nesneye erişim izni `scope` verilen laboratuvar, url veya `resource-name`, ve `resource-type` `resource-group` parametrelerin bir kombinasyonu ile tanımlanabilir.

Aşağıdaki Azure CLI örneği, belirtilen Laboratuvar için DevTest Labs Kullanıcı rolüne bir kişiyi nasıl ekleyeceğinizgöster.  

```azurecli
az role assignment create --roleName "DevTest Labs User" --signInName <email@company.com> -–resource-name "<Lab Name>" --resource-type “Microsoft.DevTestLab/labs" --resource-group "<Resource Group Name>"
```

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makalelere bakın:

- [Azure CLI'yi kullanarak DevTest Labs ile sanal makineler oluşturun ve yönetin](devtest-lab-vmcli.md)
- [Azure PowerShell'i kullanarak DevTest Labs ile sanal bir makine oluşturun](devtest-lab-vm-powershell.md)
- [Azure DevTest Labs sanal makinelerini başlatmak ve durdurmak için komut satırı araçlarını kullanın](use-command-line-start-stop-virtual-machines.md)

