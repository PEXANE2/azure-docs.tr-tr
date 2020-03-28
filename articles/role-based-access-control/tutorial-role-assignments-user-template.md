---
title: 'Öğretici: RBAC ve Kaynak Yöneticisi şablonuyla Azure kaynaklarına kullanıcı erişimi verme'
description: Bu öğreticide Azure Kaynak Yöneticisi şablonu kullanarak rol tabanlı erişim denetimi (RBAC) kullanarak bir kullanıcıya Azure kaynaklarına nasıl erişiriz öğrenin.
services: role-based-access-control,azure-resource-manager
documentationCenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: role-based-access-control
ms.devlang: ''
ms.topic: tutorial
ms.tgt_pltfrm: ''
ms.workload: identity
ms.date: 05/15/2019
ms.author: rolyon
ms.openlocfilehash: 96ca4f65d2def5f5004388c533410f09cc2a71fa
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "77138211"
---
# <a name="tutorial-grant-a-user-access-to-azure-resources-using-rbac-and-resource-manager-template"></a>Öğretici: RBAC ve Kaynak Yöneticisi şablonlarını kullanarak kullanıcıya Azure kaynaklarına erişim hakkı tanıyın

[Rol tabanlı erişim denetimi (RBAC),](overview.md) Azure kaynaklarına erişimi yönetmenin yoludur. Bu öğreticide, bir kaynak grubu oluşturur ve kaynak grubunda sanal makineler oluşturmak ve yönetmek için bir kullanıcıerişimi verirsiniz. Bu öğretici, erişim sağlamak için bir Kaynak Yöneticisi şablonu dağıtma işlemine odaklanır. Kaynak Yöneticisi şablonları geliştirme hakkında daha fazla bilgi için [Kaynak Yöneticisi belgelerine](/azure/azure-resource-manager/) ve [şablon başvurusuna](/azure/templates/microsoft.authorization/allversions
)bakın.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Kaynak grubu kapsamındaki bir kullanıcıya erişim izni verme
> * Dağıtımı doğrulama
> * Temizleme

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="prerequisites"></a>Ön koşullar

Rol atamaları eklemek ve kaldırmak için şunları yapmış olmalısınız:

* `Microsoft.Authorization/roleAssignments/write`ve `Microsoft.Authorization/roleAssignments/delete` Kullanıcı Erişim [Yöneticisi](built-in-roles.md#user-access-administrator) veya [Sahibi](built-in-roles.md#owner) gibi izinler

## <a name="grant-access"></a>Erişim verme

Bu hızlı başlatmada kullanılan şablon [Azure quickstart şablonlarındandır.](https://azure.microsoft.com/resources/templates/101-rbac-builtinrole-resourcegroup/) Azure yetkilendirmeyle ilgili daha fazla şablonu [burada](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Authorization)bulabilirsiniz.

Şablonu dağıtmak için Azure Bulut uycuzunu açmak için **Deneyin'i** seçin ve ardından aşağıdaki PowerShell komut dosyasını kabuk penceresine yapıştırın. Kodu yapıştırmak için kabuk penceresine sağ tıklayın ve ardından **Yapıştır'ı**seçin.

```azurepowershell-interactive
$projectName = Read-Host -Prompt "Enter a project name that is used to generate Azure resource names"
$emailAddress = Read-Host -Prompt "Enter your email address that is associated with your Azure subscription (used to find the principal ID)"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"

$resourceGroupName = "${projectName}rg"
$roleAssignmentName = New-Guid
$principalId = (Get-AzAdUser -Mail $emailAddress).id
$roleDefinitionId = (Get-AzRoleDefinition -name "Virtual Machine Contributor").id
$templateUri = "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-rbac-builtinrole-resourcegroup/azuredeploy.json"

New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateUri $templateUri -roleAssignmentName $roleAssignmentName -roleDefinitionID $roleDefinitionId -principalId $principalId
```

## <a name="validate-the-deployment"></a>Dağıtımı doğrulama

1. [Azure portalında](https://portal.azure.com)oturum açın.
1. Son yordamda oluşturulan kaynak grubunu açın. Varsayılan ad **rg** eklenen proje adıdır.
1. Soldaki menüden **Erişim denetimi (IAM)** öğesini seçin.
1. **Rol atamaları**’nı seçin. 
1. **Ad**olarak, son yordamda yazdığınız e-posta adresini girin. E-posta adresine sahip kullanıcının **Sanal Makine Katılımcısı** rolüne sahip olduğunu göreceksiniz.

## <a name="clean-up"></a>Temizleme

Son yordamda oluşturulan kaynak grubunu kaldırmak için Azure Bulutu kabuğunu açmak için **Deneyin'i** seçin ve ardından aşağıdaki PowerShell komut dosyasını kabuk penceresine yapıştırın.

```azurepowershell-interactive
$projectName = Read-Host -Prompt "Enter a same project name you used in the last procedure"
$resourceGroupName = "${projectName}rg"

Remove-AzResourceGroup -Name $resourceGroupName
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Öğretici: RBAC ve Azure PowerShell'i kullanarak kullanıcıya Azure kaynaklarına erişim hakkı tanıyın](tutorial-role-assignments-user-powershell.md)