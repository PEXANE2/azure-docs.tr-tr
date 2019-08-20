---
title: 'Azure Active Directory Domain Services: Kapsamlı eşitleme | Microsoft Docs'
description: Yönetilen etki alanlarınızı Azure AD 'den kapsamlı eşitleme yapılandırma
services: active-directory-ds
documentationcenter: ''
author: iainfoulds
manager: daveba
editor: curtand
ms.assetid: 9389cf0f-0036-4b17-95da-80838edd2225
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/20/2019
ms.author: iainfou
ms.openlocfilehash: 7d3bd8c6c62c0b8a1be6203e426337fcee7d2126
ms.sourcegitcommit: e42c778d38fd623f2ff8850bb6b1718cdb37309f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/19/2019
ms.locfileid: "69617125"
---
# <a name="configure-scoped-synchronization-from-azure-ad-to-your-managed-domain"></a>Yönetilen etki alanınızı Azure AD 'den kapsamlı eşitleme yapılandırma
Bu makalede, Azure AD dizininizden Azure AD Domain Services yönetilen etki alanına eşitlenecek yalnızca belirli kullanıcı hesaplarının nasıl yapılandırılacağı gösterilir.


## <a name="group-based-scoped-synchronization"></a>Grup tabanlı kapsamlı eşitleme
Varsayılan olarak, Azure AD dizininizde bulunan tüm kullanıcılar ve gruplar yönetilen etki alanınız ile eşitlenir. Yönetilen etki alanını yalnızca birkaç Kullanıcı kullanıyorsa yalnızca bu kullanıcı hesaplarını eşitlemeniz gerekebilir. Grup tabanlı kapsamlı eşitleme bunu yapmanızı sağlar. Yapılandırıldığında, yalnızca belirttiğiniz gruplara ait Kullanıcı hesapları yönetilen etki alanıyla eşitlenir.

Aşağıdaki tabloda kapsamlı eşitlemenin nasıl kullanılacağı saptanmanıza yardımcı olur:

| **Geçerli durum** | **İstenen durum** | **Gerekli yapılandırma** |
| --- | --- | --- |
| Mevcut yönetilen etki alanınız tüm Kullanıcı hesaplarını ve grupları eşitleyecek şekilde yapılandırılmıştır. | Yalnızca belirli gruplara ait kullanıcı hesaplarını yönetilen etki alanına eşitlemeyi tercih etmek istiyorsunuz. | [Mevcut yönetilen etki alanını silin](delete-aadds.md). Ardından, bu makaledeki yönergeleri izleyerek kapsamlı eşitleme yapılandırılmış şekilde yeniden oluşturun. |
| Mevcut bir yönetilen etki alanınız yok. | Yeni bir yönetilen etki alanı oluşturmak ve yalnızca belirli gruplara ait kullanıcı hesaplarını eşleştirmek istiyorsunuz. | Kapsamlı eşitleme yapılandırılmış yeni bir yönetilen etki alanı oluşturmak için bu makaledeki yönergeleri izleyin. |
| Mevcut yönetilen etki alanınız yalnızca belirli gruplara ait olan hesapları eşitleyecek şekilde yapılandırılmıştır. | Kullanıcıları Yönet etki alanına eşitlenmesi gereken grupların listesini değiştirmek istiyorsunuz. | Kapsamlı eşitlemeyi değiştirmek için bu makaledeki yönergeleri izleyin. |

> [!WARNING]
> **Eşitleme kapsamını değiştirmek, yönetilen etki alanının yeniden eşitleme yoluyla geçmesine neden olur.**
> 
>  * Yönetilen bir etki alanı için eşitleme kapsamını değiştirdiğinizde, tam bir yeniden eşitleme gerçekleşir.
>  * Yönetilen etki alanında artık gerekli olmayan nesneler silinir. Yönetilen etki alanında yeni nesneler oluşturulur.
>  * Yönetilen etki alanındaki ve Azure AD dizininizdeki nesne sayısına (kullanıcılar, gruplar ve grup üyelikleri) bağlı olarak yeniden eşitleme işleminin tamamlanması uzun sürebilir. Yüzlerce binlerce nesne içeren büyük dizinler için yeniden eşitleme işlemi birkaç gün sürebilir.


## <a name="create-a-new-managed-domain-and-enable-group-based-scoped-synchronization-using-azure-portal"></a>Azure portal kullanarak yeni bir yönetilen etki alanı oluşturun ve grup tabanlı kapsamlı eşitlemeyi etkinleştirin

1. Yönetilen bir etki alanı oluşturmak için [Başlarken Kılavuzunu](tutorial-create-instance.md) izleyin.
2. Azure AD Domain Services oluşturma sihirbazında eşitleme stili seçimi sırasında **kapsamı** belirleyin.

## <a name="create-a-new-managed-domain-and-enable-group-based-scoped-synchronization-using-powershell"></a>PowerShell kullanarak yeni bir yönetilen etki alanı oluşturma ve grup tabanlı kapsamlı eşitlemeyi etkinleştirme
Bu adım kümesini gerçekleştirmek için PowerShell 'i kullanın. [PowerShell kullanarak Azure Active Directory Domain Services etkinleştirme](powershell-create-instance.md)yönergelerine bakın. Bu makaledeki birkaç adım kapsamlı eşitlemeyi yapılandırmak için biraz değiştirilmiştir.

Yönetilen etki alanınız için grup tabanlı kapsamlı eşitlemeyi yapılandırmak için aşağıdaki adımları izleyin:

1. Aşağıdaki görevleri doldurun:
   * [Görev 1: Gerekli PowerShell modüllerini](powershell-create-instance.md#task-1-install-the-required-powershell-modules)yükler.
   * [Görev 2: Azure AD dizininizde](powershell-create-instance.md#task-2-create-the-required-service-principal-in-your-azure-ad-directory)gerekli hizmet sorumlusunu oluşturun.
   * [Görev 3: ' AAD DC Administrators ' grubunu oluşturun ve yapılandırın; PowerShell-Create-Instance. MD # Task-3-Create-ve-Configure-The-AAD-DC-Administrators-Group).
   * [Görev 4: Azure AD Domain Services kaynak sağlayıcısını](powershell-create-instance.md#task-4-register-the-azure-ad-domain-services-resource-provider)kaydedin.
   * [5. görev: Bir kaynak grubu](powershell-create-instance.md#task-5-create-a-resource-group)oluşturun.
   * [Görev 6: Sanal ağ](powershell-create-instance.md#task-6-create-and-configure-the-virtual-network)oluşturun ve yapılandırın.

2. Eşitlemek istediğiniz grupları seçin ve yönetilen etki alanınız ile eşitlemek istediğiniz grupların görünen adını sağlayın.

3. [Aşağıdaki bölümdeki betiği](scoped-synchronization.md#script-to-select-groups-to-synchronize-to-the-managed-domain-select-groupstosyncps1) adlı ```Select-GroupsToSync.ps1```bir dosyaya kaydedin. Aşağıdaki gibi betiği yürütün:

   ```powershell
   .\Select-GroupsToSync.ps1 -groupsToAdd @("AAD DC Administrators", "GroupName1", "GroupName2")
   ```

   > [!WARNING]
   > **' AAD DC Administrators ' grubunu eklemeyi unutmayın.**
   >
   > Kapsamlı eşitleme için yapılandırılmış gruplar listesine ' AAD DC Administrators ' grubunu dahil etmeniz gerekir. Bu grubu eklemezseniz, yönetilen etki alanı kullanılamaz olur.
   >

4. Şimdi, yönetilen etki alanını oluşturun ve yönetilen etki alanı için grup tabanlı kapsamlı eşitlemeyi etkinleştirin. ```"filteredSync" = "Enabled"``` Özelliğini```Properties``` parametresine ekleyin. Örneğin, görev 7 ' den [kopyalanmış aşağıdaki betik parçasına bakın: Azure AD Domain Services yönetilen etki alanını](powershell-create-instance.md#task-7-provision-the-azure-ad-domain-services-managed-domain)sağlayın.

   ```powershell
   $AzureSubscriptionId = "YOUR_AZURE_SUBSCRIPTION_ID"
   $ManagedDomainName = "contoso.com"
   $ResourceGroupName = "ContosoAaddsRg"
   $VnetName = "DomainServicesVNet_WUS"
   $AzureLocation = "westus"

   # Enable Azure AD Domain Services for the directory.
   New-AzResource -ResourceId "/subscriptions/$AzureSubscriptionId/resourceGroups/$ResourceGroupName/providers/Microsoft.AAD/DomainServices/$ManagedDomainName" `
   -Location $AzureLocation `
   -Properties @{"DomainName"=$ManagedDomainName; "filteredSync" = "Enabled"; `
    "SubnetId"="/subscriptions/$AzureSubscriptionId/resourceGroups/$ResourceGroupName/providers/Microsoft.Network/virtualNetworks/$VnetName/subnets/DomainServices"} `
   -ApiVersion 2017-06-01 -Force -Verbose
   ```

   > [!TIP]
   > ```-Properties``` Parametreye dahil etme ```"filteredSync" = "Enabled"``` seçeneğini unutmayın, bu nedenle yönetilen etki alanı için kapsamlı eşitleme etkinleştirilir.


## <a name="script-to-select-groups-to-synchronize-to-the-managed-domain-select-groupstosyncps1"></a>Yönetilen etki alanına (Select-GroupsToSync. ps1) eşitlenmek üzere grupları seçmek için betik
Aşağıdaki betiği bir dosyaya kaydedin (```Select-GroupsToSync.ps1```). Bu betik seçili grupları yönetilen etki alanına eşitlemeye Azure AD Domain Services yapılandırır. Belirtilen gruplara ait olan tüm Kullanıcı hesapları yönetilen etki alanıyla eşitlenir.

```powershell
param (
    [Parameter(Position = 0)]
    [String[]]$groupsToAdd
)

Connect-AzureAD
$sp = Get-AzureADServicePrincipal -Filter "AppId eq '2565bd9d-da50-47d4-8b85-4c97f669dc36'"
$role = $sp.AppRoles | where-object -FilterScript {$_.DisplayName -eq "User"}

Write-Output "`n****************************************************************************"

Write-Output "Total group-assignments need to be added: $($groupsToAdd.Count)"
$newGroupIds = New-Object 'System.Collections.Generic.HashSet[string]'
foreach ($groupName in $groupsToAdd)
{
    try
    {
        $group = Get-AzureADGroup -Filter "DisplayName eq '$groupName'"
        $newGroupIds.Add($group.ObjectId)

        Write-Output "Group-Name: $groupName, Id: $($group.ObjectId)"
    }
    catch
    {
        Write-Error "Failed to find group: $groupName. Exception: $($_.Exception)."
    }
}

Write-Output "****************************************************************************`n"
Write-Output "`n****************************************************************************"

$currentAssignments = Get-AzureADServiceAppRoleAssignment -ObjectId $sp.ObjectId
Write-Output "Total current group-assignments: $($currentAssignments.Count), SP-ObjectId: $($sp.ObjectId)"

$currAssignedObjectIds = New-Object 'System.Collections.Generic.HashSet[string]'
foreach ($assignment in $currentAssignments)
{
    Write-Output "Assignment-ObjectId: $($assignment.PrincipalId)"

    if ($newGroupIds.Contains($assignment.PrincipalId) -eq $false)
    {
        Write-Output "This assignment is not needed anymore. Removing it! Assignment-ObjectId: $($assignment.PrincipalId)"
        Remove-AzureADServiceAppRoleAssignment -ObjectId $sp.ObjectId -AppRoleAssignmentId $assignment.ObjectId
    }
    else
    {
        $currAssignedObjectIds.Add($assignment.PrincipalId)
    }
}

Write-Output "****************************************************************************`n"
Write-Output "`n****************************************************************************"

foreach ($id in $newGroupIds)
{
    try
    {
        if ($currAssignedObjectIds.Contains($id) -eq $false)
        {
            Write-Output "Adding new group-assignment. Role-Id: $($role.Id), Group-Object-Id: $id, ResourceId: $($sp.ObjectId)"
            New-AzureADGroupAppRoleAssignment -Id $role.Id -ObjectId $id -PrincipalId $id -ResourceId $sp.ObjectId
        }
        else
        {
            Write-Output "Group-ObjectId: $id is already assigned."
        }
    }
    catch
    {
        Write-Error "Exception occurred assigning Object-ID: $id. Exception: $($_.Exception)."
    }
}

Write-Output "****************************************************************************`n"
```


## <a name="modify-group-based-scoped-synchronization"></a>Grup tabanlı kapsamlı eşitlemeyi değiştirme
Kullanıcıları yönetilen etki alanınız ile eşitlenmesi gereken grupların listesini değiştirmek için, [PowerShell betiğini](scoped-synchronization.md#script-to-select-groups-to-synchronize-to-the-managed-domain-select-groupstosyncps1) yeniden çalıştırın ve yeni grup listesini belirtin. Bu listede her zaman ' AAD DC yöneticileri ' grubunu belirtmeyi unutmayın.

> [!WARNING]
> **' AAD DC Administrators ' grubunu eklemeyi unutmayın.**
>
> Kapsamlı eşitleme için yapılandırılmış gruplar listesine ' AAD DC Administrators ' grubunu dahil etmeniz gerekir. Bu grubu eklemezseniz, yönetilen etki alanı kullanılamaz olur.
>


## <a name="disable-group-based-scoped-synchronization"></a>Grup tabanlı kapsamlı eşitlemeyi devre dışı bırak
Yönetilen etki alanınız için grup tabanlı kapsamlı eşitlemeyi devre dışı bırakmak için aşağıdaki PowerShell betiğini kullanın:

```powershell
// Login to your Azure AD tenant
Login-AzAccount

// Retrieve the Azure AD Domain Services resource.
$DomainServicesResource = Get-AzResource -ResourceType "Microsoft.AAD/DomainServices"

// Disable group-based scoped synchronization.
$disableScopedSync = @{"filteredSync" = "Disabled"}

Set-AzResource -Id $DomainServicesResource.ResourceId -Properties $disableScopedSync
```

## <a name="next-steps"></a>Sonraki adımlar
* [Azure AD Domain Services eşitlemeyi anlama](synchronization.md)
* [PowerShell kullanarak Azure Active Directory Domain Services etkinleştirme](powershell-create-instance.md)
