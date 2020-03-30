---
title: Belirli laboratuvar ilkelerine kullanıcı izinleri verme | Microsoft Dokümanlar
description: DevTest Labs'daki belirli laboratuvar ilkelerine her kullanıcının ihtiyaçlarına göre kullanıcı izni vermeyi öğrenin
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: 5ca829f0-eb69-40a1-ae26-03a629db1d7e
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/07/2019
ms.author: spelluru
ms.openlocfilehash: 9b31f3e68fbabc32f301fdcd8066a3bfbf1c2dbd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79284219"
---
# <a name="grant-user-permissions-to-specific-lab-policies"></a>Belirli laboratuvar ilkelerine kullanıcı izinleri verme
## <a name="overview"></a>Genel Bakış
Bu makalede, kullanıcılara belirli bir laboratuvar ilkesine izin vermek için PowerShell'in nasıl kullanılacağı gösteriş verilmiştir. Bu şekilde, izinler her kullanıcının gereksinimlerine göre uygulanabilir. Örneğin, belirli bir kullanıcıya VM ilke ayarlarını değiştirme olanağı vermek isteyebilirsiniz, ancak maliyet ilkelerini değiştirmezsiniz.

## <a name="policies-as-resources"></a>Kaynak olarak ilkeler
[Azure Rol tabanlı Erişim Denetimi](../role-based-access-control/role-assignments-portal.md) makalesinde tartışıldığı gibi, RBAC Azure için kaynakların ince taneli erişim yönetimini sağlar. RBAC'ı kullanarak, DevOps ekibinizin görevlerini ayırabilir ve yalnızca işlerini gerçekleştirmek için gereken kullanıcılara erişim miktarını verebilirsiniz.

DevTest Labs'da bir ilke, RBAC eylemini sağlayan bir kaynak türüdür **Microsoft.DevTestLab/labs/policySets/policies/**. Her laboratuvar ilkesi İlke kaynak türündeki bir kaynaktır ve bir RBAC rolüne kapsam olarak atanabilir.

Örneğin, kullanıcılara **İzin Verilen VM Boyutlar** ilkesine okuma/yazma izni vermek için, **Microsoft.DevTestLab/labs/labs/policySets/policies/action** ile çalışan özel bir rol oluşturur ve ardından uygun kullanıcıları **Microsoft.DevTestLab/labs/policySets/policies/AllowedVmSizesInLab**kapsamındaki bu özel role atamış olursunuz.

RBAC'daki özel roller hakkında daha fazla bilgi edinmek için [Özel rollere erişim denetimine](../role-based-access-control/custom-roles.md)bakın.

## <a name="creating-a-lab-custom-role-using-powershell"></a>PowerShell'i kullanarak laboratuvarözel rolü oluşturma
Başlamak için [Azure PowerShell'i yüklemeniz](/powershell/azure/install-az-ps)gerekir. 

Azure PowerShell cmdlets'i kurduktan sonra aşağıdaki görevleri gerçekleştirebilirsiniz:

* Kaynak sağlayıcısının tüm işlemlerini/eylemlerini listele
* Belirli bir roldeki eylemleri listele:
* Özel rol oluşturma

Aşağıdaki PowerShell komut dosyası, bu görevlerin nasıl gerçekleştirilene kadar gerçekleştirilebildiğini gösteren örnekler gösterir:

    # List all the operations/actions for a resource provider.
    Get-AzProviderOperation -OperationSearchString "Microsoft.DevTestLab/*"

    # List actions in a particular role.
    (Get-AzRoleDefinition "DevTest Labs User").Actions

    # Create custom role.
    $policyRoleDef = (Get-AzRoleDefinition "DevTest Labs User")
    $policyRoleDef.Id = $null
    $policyRoleDef.Name = "Policy Contributor"
    $policyRoleDef.IsCustom = $true
    $policyRoleDef.AssignableScopes.Clear()
    $policyRoleDef.AssignableScopes.Add("/subscriptions/<SubscriptionID> ")
    $policyRoleDef.Actions.Add("Microsoft.DevTestLab/labs/policySets/policies/*")
    $policyRoleDef = (New-AzRoleDefinition -Role $policyRoleDef)

## <a name="assigning-permissions-to-a-user-for-a-specific-policy-using-custom-roles"></a>Özel rolleri kullanarak belirli bir ilke için kullanıcıya izin atama
Özel rollerinizi tanımladıktan sonra, bunları kullanıcılara atayabilirsiniz. Bir kullanıcıya özel bir rol atamak için, önce bu kullanıcıyı temsil eden **ObjectId'yi** almanız gerekir. Bunu yapmak için **Get-AzADUser** cmdlet'i kullanın.

Aşağıdaki örnekte, *SomeUser* kullanıcının **ObjectId** 05DEFF7B-0AC3-4ABF-B74D-6A72CD5BF3F3 olduğunu.

    PS C:\>Get-AzADUser -SearchString "SomeUser"

    DisplayName                    Type                           ObjectId
    -----------                    ----                           --------
    someuser@hotmail.com                                          05DEFF7B-0AC3-4ABF-B74D-6A72CD5BF3F3

Kullanıcı için **ObjectId'yi** ve özel bir rol adını aldıktan sonra, bu rolü **Yeni-AzRoleAssignment** cmdlet ile kullanıcıya atayabilirsiniz:

    PS C:\>New-AzRoleAssignment -ObjectId 05DEFF7B-0AC3-4ABF-B74D-6A72CD5BF3F3 -RoleDefinitionName "Policy Contributor" -Scope /subscriptions/<SubscriptionID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.DevTestLab/labs/<LabName>/policySets/default/policies/AllowedVmSizesInLab

Önceki örnekte, **AllowedVmSizesInLab** ilkesi kullanılır. Aşağıdaki polislerden herhangi birini kullanabilirsiniz:

* MaxVmsAllowedPerKullanıcı
* MaxVmsAllowedPerLab
* İzin VerilenVmSizesInLab
* LabVmsShutdown

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Sonraki adımlar
Belirli laboratuvar ilkeleri için kullanıcı izinleri verdikten sonra göz önünde bulundurulması gereken sonraki bazı adımlar şunlardır:

* [Laboratuvara erişimin güvenliğini sağlama](devtest-lab-add-devtest-user.md)
* [Laboratuvar ilkeleri belirleme](devtest-lab-set-lab-policy.md)
* [Laboratuvar şablonu oluşturma](devtest-lab-create-template.md)
* [VM'leriniz için özel yapıtlar oluşturma](devtest-lab-artifact-author.md)
* [VM'yi bir laboratuvara ekleme](devtest-lab-add-vm.md)

