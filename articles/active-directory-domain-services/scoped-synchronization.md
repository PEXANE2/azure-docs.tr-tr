---
title: Azure AD Domain Services için kapsamlı eşitleme | Microsoft Docs
description: Azure AD 'den Azure Active Directory Domain Services yönetilen bir etki alanına kapsamlı eşitlemeyi yapılandırmayı öğrenin
services: active-directory-ds
author: iainfoulds
manager: daveba
ms.assetid: 9389cf0f-0036-4b17-95da-80838edd2225
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: how-to
ms.date: 03/31/2020
ms.author: iainfou
ms.openlocfilehash: 5f2c823b0932db42876be6ab04ebcd82783729aa
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84734430"
---
# <a name="configure-scoped-synchronization-from-azure-ad-to-azure-active-directory-domain-services"></a>Azure AD 'den kapsamlı eşitlemeyi Azure Active Directory Domain Services olarak yapılandırma

Azure Active Directory Domain Services (Azure AD DS), kimlik doğrulama hizmetleri sağlamak için kullanıcıları ve grupları Azure AD 'den eşitler. Karma bir ortamda, şirket içi Active Directory Domain Services (AD DS) ortamından kullanıcılar ve gruplar önce Azure AD Connect kullanılarak Azure AD ile eşitlenebilir ve sonra Azure AD DS ile eşitlenebilir.

Varsayılan olarak, bir Azure AD dizininden tüm kullanıcılar ve gruplar Azure AD DS yönetilen bir etki alanıyla eşitlenir. Belirli gereksinimleriniz varsa, bunun yerine yalnızca tanımlı bir kullanıcı kümesini eşitlemeyi tercih edebilirsiniz.

Bu makalede, kapsamlı eşitleme kullanan yönetilen bir etki alanı oluşturma ve ardından kapsamlı kullanıcı kümesini değiştirme veya devre dışı bırakma işlemlerinin nasıl yapılacağı gösterilir.

## <a name="scoped-synchronization-overview"></a>Kapsamlı eşitlemeye genel bakış

Varsayılan olarak, bir Azure AD dizininden tüm kullanıcılar ve gruplar yönetilen bir etki alanıyla eşitlenir. Yönetilen etki alanına yalnızca birkaç kullanıcının erişmesi gerekiyorsa yalnızca bu kullanıcı hesaplarını eşitlemeniz yeterlidir. Bu kapsamlı eşitleme grup tabanlıdır. Grup tabanlı kapsamlı eşitleme yapılandırdığınızda, yalnızca belirttiğiniz gruplara ait olan kullanıcı hesapları yönetilen etki alanına eşitlenir.

Aşağıdaki tabloda kapsamlı eşitlemenin nasıl kullanılacağı özetlenmektedir:

| Geçerli durum | İstenen durum | Gerekli yapılandırma |
| --- | --- | --- |
| Mevcut bir yönetilen etki alanı, tüm Kullanıcı hesaplarını ve grupları eşitleyecek şekilde yapılandırılır. | Yalnızca belirli gruplara ait olan kullanıcı hesaplarını eşleştirmek istiyorsunuz. | Tüm kullanıcıları, kapsamlı eşitleme kullanarak eşitlemeye geçiş yapamazsınız. [Mevcut yönetilen etki alanını silin](delete-aadds.md)ve ardından bu makaledeki adımları izleyerek yönetilen bir etki alanını kapsamlı eşitleme yapılandırılmış bir şekilde yeniden oluşturun. |
| Mevcut bir yönetilen etki alanı yok. | Yeni bir yönetilen etki alanı oluşturmak ve yalnızca belirli gruplara ait kullanıcı hesaplarını eşleştirmek istiyorsunuz. | Kapsamlı eşitleme yapılandırılmış bir yönetilen etki alanı oluşturmak için bu makaledeki adımları izleyin. |
| Mevcut bir yönetilen etki alanı, yalnızca belirli gruplara ait olan hesapları eşitleyecek şekilde yapılandırılır. | Kullanıcıları yönetilen etki alanına eşitlenmesi gereken grupların listesini değiştirmek istiyorsunuz. | Kapsamlı eşitlemeyi değiştirmek için bu makaledeki adımları izleyin. |

Kapsamlı eşitleme ayarlarını yapılandırmak için Azure portal veya PowerShell kullanın:

| Eylem | | |
|--|--|--|
| Yönetilen bir etki alanı oluşturun ve kapsamlı eşitlemeyi yapılandırın | [Azure portalındaki](#enable-scoped-synchronization-using-the-azure-portal) | [PowerShell](#enable-scoped-synchronization-using-powershell) |
| Kapsamlı eşitlemeyi değiştirme | [Azure portalındaki](#modify-scoped-synchronization-using-the-azure-portal) | [PowerShell](#modify-scoped-synchronization-using-powershell) |
| Kapsamlı eşitlemeyi devre dışı bırak | [Azure portalındaki](#disable-scoped-synchronization-using-the-azure-portal) | [PowerShell](#disable-scoped-synchronization-using-powershell) |

> [!WARNING]
> Eşitleme kapsamını değiştirmek, yönetilen etki alanının tüm verileri yeniden eşitlemesine neden olur. Aşağıdaki noktalara dikkat edilmelidir:
> 
>  * Yönetilen bir etki alanı için eşitleme kapsamını değiştirdiğinizde, tam bir yeniden eşitleme gerçekleşir.
>  * Yönetilen etki alanında artık gerekli olmayan nesneler silinir. Yönetilen etki alanında yeni nesneler oluşturulur.
>  * Yeniden eşitlemenin tamamlanması uzun zaman alabilir. Eşitleme süresi, yönetilen etki alanındaki ve Azure AD dizinindeki kullanıcılar, gruplar ve grup üyelikleri gibi nesne sayısına bağlıdır. Yüzlerce binlerce nesne içeren büyük dizinler için yeniden eşitleme işlemi birkaç gün sürebilir.

## <a name="enable-scoped-synchronization-using-the-azure-portal"></a>Azure portal kullanarak kapsamlı eşitlemeyi etkinleştirme

Azure portal kapsamlı eşitlemeyi etkinleştirmek için aşağıdaki adımları izleyin:

1. [Yönetilen bir etki alanı oluşturmak ve yapılandırmak için öğreticiyi](tutorial-create-instance-advanced.md)izleyin. Eşitleme kapsamı dışında tüm önkoşulları ve dağıtım adımlarını doldurun.
1. Eşitleme adımında **kapsam** ' ı seçin ve ardından yönetilen etki alanı Ile EŞITLENECEK Azure AD gruplarını seçin.

Yönetilen etki alanının, dağıtımın tamamlanması bir saate kadar sürebilir. Azure portal, yönetilen etki alanınız için **genel bakış** sayfasında, bu dağıtım aşamasının tamamında geçerli durum gösterilir.

Azure portal yönetilen etki alanının sağlamayı bitirmiş olduğunu gösteriyorsa, aşağıdaki görevlerin tamamlanması gerekir:

* Sanal makinelerin, etki alanına katılması veya kimlik doğrulaması için yönetilen etki alanını bulabileceği şekilde sanal ağ için DNS ayarlarını güncelleştirin.
    * DNS 'yi yapılandırmak için portalda yönetilen etki alanınızı seçin. **Genel bakış** penceresinde, bu DNS ayarlarını otomatik olarak yapılandırmanız istenir.
* Son kullanıcıların şirket kimlik bilgilerini kullanarak yönetilen etki alanında oturum açmasını sağlamak [için parola eşitlemesini Azure AD Domain Services etkinleştirin](tutorial-create-instance-advanced.md#enable-user-accounts-for-azure-ad-ds) .

## <a name="modify-scoped-synchronization-using-the-azure-portal"></a>Azure portal kullanarak kapsamlı eşitlemeyi değiştirme

Kullanıcıları yönetilen etki alanıyla eşitlenmesi gereken grupların listesini değiştirmek için aşağıdaki adımları izleyin:

1. Azure portal, araması yapın ve **Azure AD Domain Services**seçin. *Aaddscontoso.com*gibi yönetilen etki alanınızı seçin.
1. Sol taraftaki menüden **eşitleme** ' yi seçin.
1. Bir grup eklemek için üstteki **grupları + seç** ' i seçin ve ardından eklenecek grupları seçin.
1. Bir grubu eşitleme kapsamından kaldırmak için, şu anda eşitlenmiş gruplar listesinden seçin ve **grupları kaldır**' ı seçin.
1. Tüm değişiklikler yapıldığında, **eşitleme kapsamını kaydet**' i seçin.

Eşitleme kapsamını değiştirmek, yönetilen etki alanının tüm verileri yeniden eşitlemesine neden olur. Yönetilen etki alanında artık gerekli olmayan nesneler silinir ve yeniden eşitlemenin tamamlanması uzun zaman alabilir.

## <a name="disable-scoped-synchronization-using-the-azure-portal"></a>Azure portal kullanarak kapsamlı eşitlemeyi devre dışı bırakma

Yönetilen bir etki alanı için grup tabanlı kapsamlı eşitlemeyi devre dışı bırakmak için aşağıdaki adımları izleyin:

1. Azure portal, araması yapın ve **Azure AD Domain Services**seçin. *Aaddscontoso.com*gibi yönetilen etki alanınızı seçin.
1. Sol taraftaki menüden **eşitleme** ' yi seçin.
1. Eşitleme kapsamını kapsam dışında tümü **olarak** ayarlayın **All**ve ardından **eşitleme kapsamını kaydet**' i seçin.

Eşitleme kapsamını değiştirmek, yönetilen etki alanının tüm verileri yeniden eşitlemesine neden olur. Yönetilen etki alanında artık gerekli olmayan nesneler silinir ve yeniden eşitlemenin tamamlanması uzun zaman alabilir.

## <a name="powershell-script-for-scoped-synchronization"></a>Kapsamlı eşitleme için PowerShell betiği

PowerShell kullanarak kapsamlı eşitlemeyi yapılandırmak için, önce aşağıdaki betiği adlı bir dosyaya kaydedin `Select-GroupsToSync.ps1` . Bu betik, Azure AD DS seçili grupları Azure AD 'den eşitleyecek şekilde yapılandırır. Belirtilen grupların parçası olan tüm Kullanıcı hesapları yönetilen etki alanıyla eşitlenir.

Bu komut dosyası, bu makaledeki ek adımlarda kullanılır.

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

## <a name="enable-scoped-synchronization-using-powershell"></a>PowerShell kullanarak kapsamlı eşitlemeyi etkinleştirme

Aşağıdaki adım kümesini gerçekleştirmek için PowerShell 'i kullanın. [PowerShell kullanarak Azure Active Directory Domain Services etkinleştirme](powershell-create-instance.md)yönergelerine bakın. Bu makaledeki birkaç adım kapsamlı eşitlemeyi yapılandırmak için biraz değiştirilmiştir.

1. Azure AD DS PowerShell kullanarak etkinleştirmek için aşağıdaki görevleri makalesini doldurun. Yönetilen etki alanını gerçekten oluşturmak için bu adımı durdurun. Yönetilen etki alanını oluşturduğunuz kapsamlı eşitlemeyi yapılandırırsınız.

   * [Gerekli PowerShell modüllerini yükler](powershell-create-instance.md#prerequisites).
   * [Yönetici erişimi için gerekli hizmet sorumlusu ve Azure AD grubunu oluşturun](powershell-create-instance.md#create-required-azure-ad-resources).
   * [Sanal ağ ve alt ağlar gibi destekleyici Azure kaynakları oluşturun](powershell-create-instance.md#create-supporting-azure-resources).

1. Azure AD 'den eşitlenmesini istediğiniz grupları ve kullanıcıları saptayın. Azure AD DS eşitlenmek üzere grupların görünen adlarının listesini oluşturun.

1. [Önceki bölümden betiği](#powershell-script-for-scoped-synchronization) çalıştırın ve *-groupstoadd* parametresini kullanarak, eşitlenmek üzere grupların listesini geçirin.

   > [!WARNING]
   > Kapsamlı eşitleme için grup listesine *AAD DC Administrators* grubunu dahil etmeniz gerekir. Bu grubu eklemezseniz, yönetilen etki alanı kullanılamaz olur.

   ```powershell
   .\Select-GroupsToSync.ps1 -groupsToAdd @("AAD DC Administrators", "GroupName1", "GroupName2")
   ```

1. Şimdi yönetilen etki alanını oluşturun ve grup tabanlı kapsamlı eşitlemeyi etkinleştirin. *-Properties* parametresinde *"filteredsync" = "Enabled"* ekleyin.

    Azure abonelik KIMLIĞINIZI ayarlayın ve ardından yönetilen etki alanı için *aaddscontoso.com*gibi bir ad sağlayın. [Get-AzSubscription][Get-AzSubscription] cmdlet 'ini kullanarak abonelik kimliğinizi alabilirsiniz. Destekleyici Azure kaynaklarını oluşturmak için kaynak grubu adı, sanal ağ adı ve bölgeyi önceki adımlarda kullanılan değerlere ayarlayın:

   ```powershell
   $AzureSubscriptionId = "YOUR_AZURE_SUBSCRIPTION_ID"
   $ManagedDomainName = "aaddscontoso.com"
   $ResourceGroupName = "myResourceGroup"
   $VnetName = "myVnet"
   $AzureLocation = "westus"

   # Enable Azure AD Domain Services for the directory.
   New-AzResource -ResourceId "/subscriptions/$AzureSubscriptionId/resourceGroups/$ResourceGroupName/providers/Microsoft.AAD/DomainServices/$ManagedDomainName" `
   -Location $AzureLocation `
   -Properties @{"DomainName"=$ManagedDomainName; "filteredSync" = "Enabled"; `
    "SubnetId"="/subscriptions/$AzureSubscriptionId/resourceGroups/$ResourceGroupName/providers/Microsoft.Network/virtualNetworks/$VnetName/subnets/DomainServices"} `
   -Force -Verbose
   ```

Kaynağın oluşturulması ve denetimin PowerShell istemine döndürülmesi birkaç dakika sürer. Yönetilen etki alanı arka planda sağlanmaya devam eder ve dağıtımın tamamlanması bir saate kadar sürebilir. Azure portal, yönetilen etki alanınız için **genel bakış** sayfasında, bu dağıtım aşamasının tamamında geçerli durum gösterilir.

Azure portal yönetilen etki alanının sağlamayı bitirmiş olduğunu gösteriyorsa, aşağıdaki görevlerin tamamlanması gerekir:

* Sanal makinelerin, etki alanına katılması veya kimlik doğrulaması için yönetilen etki alanını bulabileceği şekilde sanal ağ için DNS ayarlarını güncelleştirin.
    * DNS 'yi yapılandırmak için portalda yönetilen etki alanınızı seçin. **Genel bakış** penceresinde, bu DNS ayarlarını otomatik olarak yapılandırmanız istenir.
* Kullanılabilirlik Alanları destekleyen bir bölgede yönetilen bir etki alanı oluşturduysanız, yönetilen etki alanı için sanal ağdaki trafiği kısıtlamak üzere bir ağ güvenlik grubu oluşturun. Bu kuralların gerçekleşmesini gerektiren bir Azure Standart yük dengeleyici oluşturulur. Bu ağ güvenlik grubu, Azure AD DS güvenliğini sağlar ve yönetilen etki alanının düzgün çalışması için gereklidir.
    * Ağ güvenlik grubu ve gerekli kuralları oluşturmak için portalda yönetilen etki alanınızı seçin. **Genel bakış** penceresinde ağ güvenlik grubunu otomatik olarak oluşturmanız ve yapılandırmanız istenir.
* Son kullanıcıların şirket kimlik bilgilerini kullanarak yönetilen etki alanında oturum açmasını sağlamak [için parola eşitlemesini Azure AD Domain Services etkinleştirin](tutorial-create-instance-advanced.md#enable-user-accounts-for-azure-ad-ds) .

## <a name="modify-scoped-synchronization-using-powershell"></a>PowerShell kullanarak kapsamlı eşitlemeyi değiştirme

Kullanıcıları yönetilen etki alanıyla eşitlenmesi gereken grupların listesini değiştirmek için, [PowerShell betiğini](#powershell-script-for-scoped-synchronization) yeniden çalıştırın ve yeni grup listesini belirtin. Aşağıdaki örnekte, eşitlenmesi yapılacak gruplar artık *GroupName2*içermez ve artık *GroupName3*' yi içerir.

> [!WARNING]
> Kapsamlı eşitleme için grup listesine *AAD DC Administrators* grubunu dahil etmeniz gerekir. Bu grubu eklemezseniz, yönetilen etki alanı kullanılamaz olur.

```powershell
.\Select-GroupsToSync.ps1 -groupsToAdd @("AAD DC Administrators", "GroupName1", "GroupName3")
```

Eşitleme kapsamını değiştirmek, yönetilen etki alanının tüm verileri yeniden eşitlemesine neden olur. Yönetilen etki alanında artık gerekli olmayan nesneler silinir ve yeniden eşitlemenin tamamlanması uzun zaman alabilir.

## <a name="disable-scoped-synchronization-using-powershell"></a>PowerShell kullanarak kapsamlı eşitlemeyi devre dışı bırakma

Yönetilen bir etki alanı için grup tabanlı kapsamlı eşitlemeyi devre dışı bırakmak için, Azure AD DS kaynağında *"Filteredsync" = "Disabled"* seçeneğini belirleyin ve ardından yönetilen etki alanını güncelleştirin. Bu tamamlandığında, tüm kullanıcılar ve gruplar Azure AD 'den eşitlenmek üzere ayarlanır.

```powershell
// Retrieve the Azure AD DS resource.
$DomainServicesResource = Get-AzResource -ResourceType "Microsoft.AAD/DomainServices"

// Disable group-based scoped synchronization.
$disableScopedSync = @{"filteredSync" = "Disabled"}

// Update the Azure AD DS resource
Set-AzResource -Id $DomainServicesResource.ResourceId -Properties $disableScopedSync
```

Eşitleme kapsamını değiştirmek, yönetilen etki alanının tüm verileri yeniden eşitlemesine neden olur. Yönetilen etki alanında artık gerekli olmayan nesneler silinir ve yeniden eşitlemenin tamamlanması uzun zaman alabilir.

## <a name="next-steps"></a>Sonraki adımlar

Eşitleme işlemi hakkında daha fazla bilgi edinmek için bkz. [Azure AD Domain Services eşitlemeyi anlama](synchronization.md).

<!-- EXTERNAL LINKS -->
[Get-AzSubscription]: /powershell/module/Az.Accounts/Get-AzSubscription
