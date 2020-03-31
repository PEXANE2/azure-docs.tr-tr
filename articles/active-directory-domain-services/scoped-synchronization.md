---
title: Azure AD Etki Alanı Hizmetleri için kapsamlı eşitleme | Microsoft Dokümanlar
description: Azure AD'den Azure Etkin Dizin Etki Alanı Hizmetleri yönetilen bir etki alanına kapsamlı eşitleme yi nasıl yapılandırabileceğinizi öğrenin
services: active-directory-ds
author: iainfoulds
manager: daveba
ms.assetid: 9389cf0f-0036-4b17-95da-80838edd2225
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: article
ms.date: 11/26/2019
ms.author: iainfou
ms.openlocfilehash: cc126af67a0d8627d61e595cee56f3df8973340d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77613051"
---
# <a name="configure-scoped-synchronization-from-azure-ad-to-azure-active-directory-domain-services"></a>Azure AD'den Azure Etkin Dizin Etki Alanı Hizmetlerine kapsamlı eşitleme yapılandırma

Azure Active Directory Domain Services (Azure AD DS), kimlik doğrulama hizmetleri sağlamak için Azure AD'deki kullanıcıları ve grupları eşitler. Karma bir ortamda, şirket içi Active Directory Domain Services (AD DS) ortamındaki kullanıcılar ve gruplar önce Azure AD Connect kullanılarak Azure AD ile eşitlenebilir, ardından Azure AD DS ile senkronize edilebilir.

Varsayılan olarak, bir Azure REKLAM dizinindeki tüm kullanıcılar ve gruplar Azure AD DS yönetilen bir etki alanına eşitlenir. Belirli gereksinimleriniz varsa, bunun yerine yalnızca tanımlanmış bir kullanıcı kümesini eşitlemeyi seçebilirsiniz.

Bu makalede, kapsamlı eşitleme kullanan ve ardından kapsamlı kullanıcı kümesini değiştiren veya devre dışı bırakan bir Azure AD DS yönetilen etki alanının nasıl oluşturulabileceği gösterilmektedir.

## <a name="scoped-synchronization-overview"></a>Kapsamlı eşitleme genel bakış

Varsayılan olarak, bir Azure REKLAM dizinindeki tüm kullanıcılar ve gruplar Azure AD DS yönetilen bir etki alanına eşitlenir. Yönetilen etki alanına yalnızca birkaç kullanıcının erişmeniz gerekiyorsa, yalnızca bu kullanıcı hesaplarını eşitleyebilirsiniz. Bu kapsamdaki eşitleme grup tabanlıdır. Grup tabanlı kapsamlı eşitleme yapılandırdığınızda, yalnızca belirttiğiniz gruplara ait kullanıcı hesapları Azure AD DS yönetilen etki alanına eşitlenir.

Aşağıdaki tablo, kapsamlı eşitlemenin nasıl kullanılacağını özetley:

| Geçerli durum | İstenilen durum | Gerekli yapılandırma |
| --- | --- | --- |
| Varolan yönetilen etki alanı, tüm kullanıcı hesaplarını ve gruplarını eşitlemek üzere yapılandırılır. | Yalnızca belirli gruplara ait kullanıcı hesaplarını eşitlemek istiyorsunuz. | Tüm kullanıcıları eşitlemeden kapsamlı eşitleme kullanmaya geçiş yapamazsınız. [Varolan yönetilen etki alanını silin,](delete-aadds.md)ardından yapılandırılmış kapsamlı eşitleme ile Azure AD DS yönetilen bir etki alanını yeniden oluşturmak için bu makaledeki adımları izleyin. |
| Varolan yönetilen etki alanı yok. | Yeni yönetilen bir etki alanı oluşturmak ve yalnızca belirli gruplara ait kullanıcı hesaplarını eşitlemek istiyorsunuz. | Kapsamlı eşitleme yapılandırılan bir Azure AD DS yönetilen etki alanı oluşturmak için bu makaledeki adımları izleyin. |
| Varolan yönetilen bir etki alanı, yalnızca belirli gruplara ait hesapları eşitlemek üzere yapılandırılır. | Kullanıcıları Azure AD DS yönetilen etki alanıyla eşitlemesi gereken grupların listesini değiştirmek istiyorsunuz. | Kapsamlı eşitleme değiştirmek için bu makalede adımları izleyin. |

Kapsamlı eşitleme ayarlarını yapılandırmak için Azure portalını veya PowerShell'i kullanırsınız:

| Eylem | | |
|--|--|--|
| Azure AD DS yönetilen etki alanı oluşturun ve kapsamlı eşitleme yapılandırma | [Azure portalında](#enable-scoped-synchronization-using-the-azure-portal) | [Powershell](#enable-scoped-synchronization-using-powershell) |
| Kapsamlı eşitleme değiştirme | [Azure portalında](#modify-scoped-synchronization-using-the-azure-portal) | [Powershell](#modify-scoped-synchronization-using-powershell) |
| Kapsamlı eşitlemeyi devre dışı | [Azure portalında](#disable-scoped-synchronization-using-the-azure-portal) | [Powershell](#disable-scoped-synchronization-using-powershell) |

> [!WARNING]
> Eşitleme nin kapsamını değiştirmek, Azure AD DS yönetilen etki alanının tüm verileri yeniden eşitlemesi ne kadar önemli.
> 
>  * Azure AD DS yönetilen bir etki alanının eşitleme kapsamını değiştirdiğinizde, tam bir yeniden eşitleme oluşur.
>  * Azure AD DS yönetilen etki alanında artık gerekli olmayan nesneler silinir. Yönetilen etki alanında yeni nesneler oluşturulur.
>  * Yeniden senkronizasyonun tamamlanması uzun sürebilir. Eşitleme süresi, Azure AD DS yönetilen etki alanı ve Azure AD dizinindeki kullanıcılar, gruplar ve grup üyelikleri gibi nesnelerin sayısına bağlıdır. Yüz binlerce nesneye sahip büyük dizinler için yeniden eşitleme birkaç gün sürebilir.

## <a name="enable-scoped-synchronization-using-the-azure-portal"></a>Azure portalını kullanarak kapsamlı eşitleme sağlama

1. Bir [Azure AD DS örneği oluşturmak ve yapılandırmak için öğreticiyi](tutorial-create-instance-advanced.md)izleyin. Eşitleme kapsamı dışındaki tüm ön koşulları ve dağıtım adımlarını tamamlayın.
1. Eşitleme adımında **Scoped'u** seçin ve ardından Azure AD DS örneğine eşitlemek için Azure REKLAM gruplarını seçin.

Azure AD DS yönetilen etki alanının dağıtımı tamamlaması bir saat kadar sürebilir. Azure portalında, Azure AD DS yönetilen etki alanınız için **Genel Bakış** sayfası, dağıtım aşaması boyunca geçerli durumu gösterir.

Azure portalı Azure AD DS yönetilen etki alanının sağlanmasını tamamladığını gösterdiğinde, aşağıdaki görevlerin tamamlanması gerekir:

* Sanal makinelerin etki alanı birleştirme veya kimlik doğrulama için yönetilen etki alanını bulabilmesi için sanal ağ için DNS ayarlarını güncelleştirin.
    * DNS'yi yapılandırmak için portaldaki Azure AD DS yönetilen etki alanınızı seçin. Genel **Bakış** penceresinde, bu DNS ayarlarını otomatik olarak yapılandırmanız istenir.
* Son kullanıcıların kurumsal kimlik bilgilerini kullanarak yönetilen etki alanında oturum açabilmesi [için Azure AD Etki Alanı Hizmetlerine parola eşitlemesini etkinleştirin.](tutorial-create-instance-advanced.md#enable-user-accounts-for-azure-ad-ds)

## <a name="modify-scoped-synchronization-using-the-azure-portal"></a>Azure portalını kullanarak kapsamlı eşitleme değiştirme

Kullanıcıları Azure AD DS yönetilen etki alanıyla eşitlenmiş olması gereken grupların listesini değiştirmek için aşağıdaki adımları tamamlayın:

1. Azure **portalında, Azure AD Etki Alanı Hizmetlerini**arayın ve seçin. örneğinizi seçin, *örneğin aaddscontoso.com.*
1. Sol taraftaki menüden **Eşitleme'yi** seçin.
1. Bir grup eklemek için, üstteki **+ Grupları** seçin ve ardından ekleyecek grupları seçin.
1. Bir grubu eşitleme kapsamından kaldırmak için, grubu şu anda eşitlenmiş gruplar listesinden seçin ve **Grupları Kaldır'ı**seçin.
1. Tüm değişiklikler yapıldığında, **eşitleme kapsamını kaydet'i**seçin.

Eşitleme nin kapsamını değiştirmek, Azure AD DS yönetilen etki alanının tüm verileri yeniden eşitlemesi ne kadar önemli. Azure AD DS yönetilen etki alanında artık gerekli olmayan nesneler silinir ve yeniden eşitlemenin tamamlanması uzun sürebilir.

## <a name="disable-scoped-synchronization-using-the-azure-portal"></a>Azure portalını kullanarak kapsamlı eşitlemeyi devre dışı

Azure AD DS yönetilen etki alanı için grup tabanlı kapsamlı eşitlemeyi devre dışı katmak için aşağıdaki adımları tamamlayın:

1. Azure **portalında, Azure AD Etki Alanı Hizmetlerini**arayın ve seçin. örneğinizi seçin, *örneğin aaddscontoso.com.*
1. Sol taraftaki menüden **Eşitleme'yi** seçin.
1. Eşitleme kapsamını **Scoped'dan** **Tümüne**ayarlayın, ardından **eşitleme kapsamını kaydet'i**seçin.

Eşitleme nin kapsamını değiştirmek, Azure AD DS yönetilen etki alanının tüm verileri yeniden eşitlemesi ne kadar önemli. Azure AD DS yönetilen etki alanında artık gerekli olmayan nesneler silinir ve yeniden eşitlemenin tamamlanması uzun sürebilir.

## <a name="powershell-script-for-scoped-synchronization"></a>Kapsamlı eşitleme için PowerShell komut dosyası

PowerShell kullanarak kapsamlı eşitleme yapılandırmak için, önce aşağıdaki komut `Select-GroupsToSync.ps1`dosyasını adlı bir dosyaya kaydedin. Bu komut dosyası, Azure AD'den seçili grupları eşitlemek için Azure AD DS'yi yapılandırır. Belirtilen grupların bir parçası olan tüm kullanıcı hesapları Azure AD DS yönetilen etki alanına eşitlenir.

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

## <a name="enable-scoped-synchronization-using-powershell"></a>PowerShell'i kullanarak kapsamlı eşitleme sağlama

Bu adım kümesini tamamlamak için PowerShell'i kullanın. [PowerShell'i kullanarak Azure Active Directory Domain Services'ı etkinleştirmek](powershell-create-instance.md)için verilen talimatlara bakın. Bu makaledeki birkaç adım, kapsamlı eşitlemayı yapılandırmak için biraz değiştirilir.

1. PowerShell'i kullanarak Azure AD DS'yi etkinleştirmek için makaleden aşağıdaki görevleri tamamlayın. Yönetilen etki alanını oluşturmak için adımda durun. Azure AD DS yönetilen etki alanını oluşturduğunuz kapsamlı eşitlemeyi yapılandırırsınız.

   * [Gerekli PowerShell modüllerini kurun.](powershell-create-instance.md#prerequisites)
   * [Yönetim erişimi için gerekli hizmet sorumlusu ve Azure REKLAM grubunu oluşturun.](powershell-create-instance.md#create-required-azure-ad-resources)
   * [Sanal ağ ve alt ağlar gibi destekleyici Azure kaynakları oluşturun.](powershell-create-instance.md#create-supporting-azure-resources)

1. Azure AD'den eşitlemek istediğiniz grupları ve kullanıcıları belirleyin. Azure AD DS ile eşitlenecek grupların görüntü adlarının bir listesini yapın.

1. Önceki [bölümden komut dosyasını](#powershell-script-for-scoped-synchronization) çalıştırın ve eşitlemek için grupların listesini geçmek için *-groupsToAdd* parametresini kullanın.

   > [!WARNING]
   > Kapsamlı eşitleme için gruplar listesine *AAD DC Yöneticileri* grubunu eklemeniz gerekir. Bu grubu eklemezseniz, Azure AD DS yönetilen etki alanı kullanılamaz hale gelir.

   ```powershell
   .\Select-GroupsToSync.ps1 -groupsToAdd @("AAD DC Administrators", "GroupName1", "GroupName2")
   ```

1. Şimdi Azure AD DS yönetilen etki alanı oluşturun ve grup tabanlı kapsamlı eşitleme etkinleştirin. *-Properties* parametreye *"filteredSync" = "Enabled"* ekleyin.

    Azure abonelik kimliğinizi ayarlayın ve ardından yönetilen etki alanı için *aaddscontoso.com*gibi bir ad sağlayın. Abonelik kimliğinizi [Get-AzSubscription][Get-AzSubscription] cmdlet'i kullanarak alabilirsiniz. Destekleyen Azure kaynaklarını oluşturmak için kaynak grubu adını, sanal ağ adını ve bölgesini önceki adımlarda kullanılan değerlere ayarlayın:

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

Kaynağı oluşturmak ve denetimi PowerShell istemine döndürmek birkaç dakika sürer. Azure AD DS yönetilen etki alanı arka planda sağlanmaya devam eder ve dağıtımı tamamlamak bir saat kadar sürebilir. Azure portalında, Azure AD DS yönetilen etki alanınız için **Genel Bakış** sayfası, dağıtım aşaması boyunca geçerli durumu gösterir.

Azure portalı Azure AD DS yönetilen etki alanının sağlanmasını tamamladığını gösterdiğinde, aşağıdaki görevlerin tamamlanması gerekir:

* Sanal makinelerin etki alanı birleştirme veya kimlik doğrulama için yönetilen etki alanını bulabilmesi için sanal ağ için DNS ayarlarını güncelleştirin.
    * DNS'yi yapılandırmak için portaldaki Azure AD DS yönetilen etki alanınızı seçin. Genel **Bakış** penceresinde, bu DNS ayarlarını otomatik olarak yapılandırmanız istenir.
* Kullanılabilirlik Bölgelerini destekleyen bir bölgede Bir Azure AD DS yönetilen etki alanı oluşturduysanız, Azure AD DS yönetilen etki alanının sanal ağındaki trafiği kısıtlamak için bir ağ güvenlik grubu oluşturun. Bu kuralların yerine koymasını gerektiren bir Azure standart yük dengeleyicisi oluşturulur. Bu ağ güvenlik grubu Azure AD DS güvenliğini sağlar ve yönetilen etki alanının doğru çalışması için gereklidir.
    * Ağ güvenlik grubu ve gerekli kurallar oluşturmak için portaldaki Azure AD DS yönetilen etki alanınızı seçin. Genel **Bakış** penceresinde, ağ güvenlik grubunu otomatik olarak oluşturmanız ve yapılandırmanız istenir.
* Son kullanıcıların kurumsal kimlik bilgilerini kullanarak yönetilen etki alanında oturum açabilmesi [için Azure AD Etki Alanı Hizmetlerine parola eşitlemesini etkinleştirin.](tutorial-create-instance-advanced.md#enable-user-accounts-for-azure-ad-ds)

## <a name="modify-scoped-synchronization-using-powershell"></a>PowerShell'i kullanarak kapsamlı eşitleme modiyi değiştirme

Kullanıcıları Azure AD DS yönetilen etki alanına eşitlenmiş olması gereken grupların listesini değiştirmek için [PowerShell komut dosyasını](#powershell-script-for-scoped-synchronization) yeniden çalıştırın ve yeni grup listesini belirtin. Aşağıdaki örnekte, eşitlemek için gruplar artık *GroupName2*içerir ve şimdi *GroupName3*içerir.

> [!WARNING]
> Kapsamlı eşitleme için gruplar listesine *AAD DC Yöneticileri* grubunu eklemeniz gerekir. Bu grubu eklemezseniz, Azure AD DS yönetilen etki alanı kullanılamaz hale gelir.

```powershell
.\Select-GroupsToSync.ps1 -groupsToAdd @("AAD DC Administrators", "GroupName1", "GroupName3")
```

Eşitleme nin kapsamını değiştirmek, Azure AD DS yönetilen etki alanının tüm verileri yeniden eşitlemesi ne kadar önemli. Azure AD DS yönetilen etki alanında artık gerekli olmayan nesneler silinir ve yeniden eşitlemenin tamamlanması uzun sürebilir.

## <a name="disable-scoped-synchronization-using-powershell"></a>PowerShell kullanarak kapsamlı senkronizasyonu devre dışı

Azure AD DS yönetilen etki alanı için grup tabanlı kapsamlı eşitlemeyi devre dışı bırakarak, Azure AD DS kaynağında *"filteredSync" = "Devre dışı"* olarak ayarlayın ve yönetilen etki alanını güncelleştirin. Tamamlandığında, tüm kullanıcılar ve gruplar Azure AD'den eşitlemek üzere ayarlanır.

```powershell
// Retrieve the Azure AD DS resource.
$DomainServicesResource = Get-AzResource -ResourceType "Microsoft.AAD/DomainServices"

// Disable group-based scoped synchronization.
$disableScopedSync = @{"filteredSync" = "Disabled"}

// Update the Azure AD DS resource
Set-AzResource -Id $DomainServicesResource.ResourceId -Properties $disableScopedSync
```

Eşitleme nin kapsamını değiştirmek, Azure AD DS yönetilen etki alanının tüm verileri yeniden eşitlemesi ne kadar önemli. Azure AD DS yönetilen etki alanında artık gerekli olmayan nesneler silinir ve yeniden eşitlemenin tamamlanması uzun sürebilir.

## <a name="next-steps"></a>Sonraki adımlar

Eşitleme işlemi hakkında daha fazla bilgi edinmek için Azure [AD Etki Alanı Hizmetlerinde eşitlemeyi anlayın'](synchronization.md)a bakın.

<!-- EXTERNAL LINKS -->
[Get-AzSubscription]: /powershell/module/Az.Accounts/Get-AzSubscription
