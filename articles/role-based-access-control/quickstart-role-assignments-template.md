---
title: 'Hızlı başlangıç: Azure Resource Manager şablonu kullanarak Azure rol ataması ekleme-Azure RBAC'
description: Azure Resource Manager şablonları ve Azure rol tabanlı erişim denetimi (Azure RBAC) kullanarak kaynak grubu kapsamındaki bir kullanıcı için Azure kaynaklarına erişim izni verme hakkında bilgi edinin.
services: role-based-access-control,azure-resource-manager
author: rolyon
manager: mtillman
ms.service: role-based-access-control
ms.topic: quickstart
ms.custom: subject-armqs
ms.workload: identity
ms.date: 05/21/2020
ms.author: rolyon
ms.openlocfilehash: 4cb25b0398dca05f32f944962e2fc8630ea10870
ms.sourcegitcommit: 309cf6876d906425a0d6f72deceb9ecd231d387c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2020
ms.locfileid: "84267656"
---
# <a name="quickstart-add-an-azure-role-assignment-using-an-azure-resource-manager-template"></a>Hızlı başlangıç: Azure Resource Manager şablonu kullanarak Azure rol ataması ekleme

Azure [rol tabanlı erişim denetimi (Azure RBAC)](overview.md) , Azure kaynaklarına erişimi yönetme yöntemidir. Bu hızlı başlangıçta, bir kaynak grubu oluşturur ve kaynak grubundaki sanal makineleri oluşturmak ve yönetmek için bir kullanıcı erişimi verirsiniz. Bu hızlı başlangıç, erişim izni vermek için bir Kaynak Yöneticisi şablonu kullanır.

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Ön koşullar

Rol atamaları eklemek için şunları yapmanız gerekir:

* `Microsoft.Authorization/roleAssignments/write`ve `Microsoft.Authorization/roleAssignments/delete` [Kullanıcı erişimi Yöneticisi](built-in-roles.md#user-access-administrator) veya [sahibi](built-in-roles.md#owner) gibi izinler

## <a name="create-a-role-assignment"></a>Rol ataması oluşturma

Rol ataması eklemek için üç öğe belirtmeniz gerekir: güvenlik sorumlusu, rol tanımı ve kapsam. Bu hızlı başlangıçta, güvenlik sorumlusu sizin ya da dizininizde bulunan başka bir Kullanıcı, rol tanımı [sanal makine katılımcısı](built-in-roles.md#virtual-machine-contributor)ve kapsam sizin belirttiğiniz bir kaynak grubudur.

### <a name="review-the-template"></a>Şablonu gözden geçirme

Bu hızlı başlangıçta kullanılan şablon [Azure hızlı başlangıç şablonlarından](https://azure.microsoft.com/resources/templates/101-rbac-builtinrole-resourcegroup/). Şablonda üç parametre ve bir kaynak bölümü vardır. Kaynaklar bölümünde rol atamasının üç öğesine sahip olduğuna dikkat edin: güvenlik sorumlusu, rol tanımı ve kapsam. 

:::code language="json" source="~/quickstart-templates/101-rbac-builtinrole-resourcegroup/azuredeploy.json" highlight="30-32":::

### <a name="deploy-the-template"></a>Şablonu dağıtma

1. [Azure Portal](https://portal.azure.com) oturum açın.

1. Azure aboneliğiniz ile ilişkili e-posta adresinizi saptayın. Ya da dizininizdeki başka bir kullanıcının e-posta adresini saptayın.

1. PowerShell için Azure Cloud Shell açın.

1. Aşağıdaki betiği kopyalayıp Cloud Shell yapıştırın.

    ```azurepowershell
    $resourceGroupName = Read-Host -Prompt "Enter a resource group name (i.e. ExampleGrouprg)"
    $emailAddress = Read-Host -Prompt "Enter an email address for a user in your directory"
    $location = Read-Host -Prompt "Enter a location (i.e. centralus)"
    
    $roleAssignmentName = New-Guid
    $principalId = (Get-AzAdUser -Mail $emailAddress).id
    $roleDefinitionId = (Get-AzRoleDefinition -name "Virtual Machine Contributor").id
    $templateUri = "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-rbac-builtinrole-resourcegroup/azuredeploy.json"
    
    New-AzResourceGroup -Name $resourceGroupName -Location $location
    New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateUri $templateUri -roleAssignmentName $roleAssignmentName -roleDefinitionID $roleDefinitionId -principalId $principalId
    ```

1. ExampleGrouprg gibi bir kaynak grubu adı girin.

1. Dizininizde kendiniz veya başka bir kullanıcı için bir e-posta adresi girin.

1. Kaynak grubu için tek merkezde ABD gibi bir konum girin.

1. Gerekirse, New-AzResourceGroupDeployment komutunu çalıştırmak için ENTER tuşuna basın.

    [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) komutu yeni bir kaynak grubu oluşturur ve [New-AzResourceGroupDeployment](/powershell/module/az.resources/new-azresourcegroupdeployment) komutu şablonu, rol atamasını eklemek için dağıtır.

    Aşağıdakine benzer bir çıktı görmeniz gerekir:

    ```azurepowershell
    PS> New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateUri $templateUri -roleAssignmentName $roleAssignmentName -roleDefinitionID $roleDefinitionId -principalId $principalId
    
    DeploymentName          : azuredeploy
    ResourceGroupName       : ExampleGrouprg
    ProvisioningState       : Succeeded
    Timestamp               : 5/22/2020 9:01:30 PM
    Mode                    : Incremental
    TemplateLink            :
                              Uri            : https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-rbac-builtinrole-resourcegroup/azuredeploy.json
                              ContentVersion : 1.0.0.0
    
    Parameters              :
                              Name                  Type                       Value
                              ====================  =========================  ==========
                              roleAssignmentName    String                     {roleAssignmentName}
                              roleDefinitionID      String                     9980e02c-c2be-4d73-94e8-173b1dc7cf3c
                              principalId           String                     {principalId}
    
    Outputs                 :
    DeploymentDebugLogLevel :
    ```

## <a name="review-deployed-resources"></a>Dağıtılan kaynakları gözden geçirme

1. Azure portal oluşturduğunuz kaynak grubunu açın.

1. Sol menüde **erişim denetimi (IAM)** öğesine tıklayın.

1. **Rol atamaları** sekmesine tıklayın.

1. **Sanal makine katılımcısı** rolünün belirttiğiniz kullanıcıya atandığını doğrulayın.

   ![Yeni rol ataması](./media/quickstart-role-assignments-template/role-assignment-portal.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Oluşturduğunuz rol atamasını ve kaynak grubunu kaldırmak için aşağıdaki adımları izleyin.

1. Aşağıdaki betiği kopyalayıp Cloud Shell yapıştırın.

    ```azurepowershell
    $emailAddress = Read-Host -Prompt "Enter the email address of the user with the role assignment to remove"
    $resourceGroupName = Read-Host -Prompt "Enter the resource group name to remove (i.e. ExampleGrouprg)"
    
    $principalId = (Get-AzAdUser -Mail $emailAddress).id
    
    Remove-AzRoleAssignment -ObjectId $principalId -RoleDefinitionName "Virtual Machine Contributor" -ResourceGroupName $resourceGroupName
    Remove-AzResourceGroup -Name $resourceGroupName
    ```
    
1. Kaldırılacak rol ataması olan kullanıcının e-posta adresini girin.

1. Örnek Gruburg gibi kaldırılacak kaynak grubu adını girin.

1. Gerekirse, Remove-AzResourceGroup komutunu çalıştırmak için ENTER tuşuna basın.

1. Kaynak grubunu kaldırmak istediğinizi onaylamak için **Y** yazın.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Öğretici: Azure PowerShell kullanarak Azure kaynaklarına Kullanıcı erişimi verme](tutorial-role-assignments-user-powershell.md)