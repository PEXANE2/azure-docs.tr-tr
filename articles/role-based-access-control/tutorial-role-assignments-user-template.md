---
title: Öğretici-RBAC ve Kaynak Yöneticisi şablonu kullanarak Azure kaynaklarına Kullanıcı erişimi verme
description: Bu öğreticide Azure Resource Manager şablonu kullanarak rol tabanlı erişim denetimi 'ni (RBAC) kullanarak Azure kaynaklarına Kullanıcı erişimi verme hakkında bilgi edinin.
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
ms.openlocfilehash: ed143f85b4372348baa1d74b4ec7a7447943a74f
ms.sourcegitcommit: 4c831e768bb43e232de9738b363063590faa0472
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/23/2019
ms.locfileid: "74418497"
---
# <a name="tutorial-grant-a-user-access-to-azure-resources-using-rbac-and-resource-manager-template"></a>Öğretici: RBAC ve Kaynak Yöneticisi şablonu kullanarak Azure kaynaklarına Kullanıcı erişimi verme

[Rol tabanlı erişim denetimi (RBAC)](overview.md) , Azure kaynaklarına erişimi yönetme yöntemidir. Bu öğreticide, bir kaynak grubu oluşturur ve kaynak grubundaki sanal makineleri oluşturmak ve yönetmek için bir kullanıcı erişimi verirsiniz. Bu öğretici, erişim izni vermek için bir Kaynak Yöneticisi şablonu dağıtma işlemine odaklanmaktadır. Kaynak Yöneticisi şablonları geliştirme hakkında daha fazla bilgi için bkz. [Kaynak Yöneticisi belgeleri](/azure/azure-resource-manager/) ve [şablon başvurusu](/azure/templates/microsoft.authorization/allversions
).

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Bir kaynak grubu kapsamındaki bir kullanıcı için erişim izni verme
> * Dağıtımı doğrulama
> * Temizleme

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

Rol atamaları eklemek ve kaldırmak için şunları yapmanız gerekir:

* `Microsoft.Authorization/roleAssignments/write` ve `Microsoft.Authorization/roleAssignments/delete` izinleri ( [Kullanıcı erişimi Yöneticisi](built-in-roles.md#user-access-administrator) veya [sahibi](built-in-roles.md#owner) gibi)

## <a name="grant-access"></a>Erişim verme

Bu hızlı başlangıçta kullanılan şablon [Azure hızlı başlangıç şablonlarından](https://azure.microsoft.com/resources/templates/101-rbac-builtinrole-resourcegroup/). Daha fazla Azure yetkilendirmesi ile ilgili şablon [burada](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Authorization)bulunabilir.

Şablonu dağıtmak için **dene** ' yi seçerek Azure Cloud Shell 'i açın ve sonra aşağıdaki PowerShell betiğini kabuk penceresine yapıştırın. Kodu yapıştırmak için kabuk penceresine sağ tıklayıp **Yapıştır**' ı seçin.

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

1. [Azure portalında](https://portal.azure.com) oturum açın.
1. Son yordamda oluşturulan kaynak grubunu açın. Varsayılan ad, **RG** eklenmiş proje adıdır.
1. Soldaki menüden **Erişim denetimi (IAM)** öğesini seçin.
1. **Rol atamaları**’nı seçin. 
1. **Ad**alanına, son yordamda yazdığınız e-posta adresini girin. Kullanıcının e-posta adresi ile **sanal makine katılımcısı** rolü olduğunu görürsünüz.

## <a name="clean-up"></a>Temizleme

Son yordamda oluşturulan kaynak grubunu kaldırmak için, **deneyin** ' i seçerek Azure Cloud Shell 'i açın ve sonra aşağıdaki PowerShell betiğini kabuk penceresine yapıştırın.

```azurepowershell-interactive
$projectName = Read-Host -Prompt "Enter a same project name you used in the last procedure"
$resourceGroupName = "${projectName}rg"

Remove-AzResourceGroup -Name $resourceGroupName
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Öğretici: RBAC ve Azure PowerShell kullanarak Azure kaynaklarına Kullanıcı erişimi verme](tutorial-role-assignments-user-powershell.md)