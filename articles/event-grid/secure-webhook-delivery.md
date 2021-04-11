---
title: Azure Event Grid 'de Azure AD ile güvenli Web kancası teslimi
description: Azure Event Grid kullanılarak Azure Active Directory korunan HTTPS uç noktalarına olayların nasıl teslim edileceğini açıklar
ms.topic: how-to
ms.date: 03/20/2021
ms.openlocfilehash: 1298910db78ba468dd9744e84ee4629161e0a776
ms.sourcegitcommit: 3ee3045f6106175e59d1bd279130f4933456d5ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2021
ms.locfileid: "106076045"
---
# <a name="publish-events-to-azure-active-directory-protected-endpoints"></a>Olayları Azure Active Directory korumalı uç noktalara yayımlama
Bu makalede, **olay aboneliğiniz** ve **Web kancası uç** noktanız arasındaki bağlantıyı güvenli hale getirmek IÇIN Azure Active Directory (Azure AD) nasıl kullanılacağı açıklanır. Azure AD uygulamalarına ve hizmet sorumlularına genel bakış için bkz. [Microsoft Identity platform (v 2.0) genel bakış](../active-directory/develop/v2-overview.md).

Bu makale, tanıtım için Azure portal kullanır, ancak özellik CLı, PowerShell veya SDK 'lar kullanılarak da etkinleştirilebilir.


## <a name="create-an-azure-ad-application"></a>Azure AD uygulaması oluşturma
Korumalı uç noktanız için bir Azure AD uygulaması oluşturarak web kancasını Azure AD 'ye kaydedin. Bkz. [Senaryo: korumalı Web API 'si](https://docs.microsoft.com/azure/active-directory/develop/scenario-protected-web-api-overview). Korumalı API 'nizi bir Daemon uygulaması tarafından çağrılacak şekilde yapılandırın.
    
## <a name="enable-event-grid-to-use-your-azure-ad-application"></a>Azure AD uygulamanızı kullanmak için Event Grid etkinleştirme
Bu bölümde Azure AD uygulamanızı kullanmak için Event Grid nasıl etkinleştirileceği gösterilmektedir. 

> [!NOTE]
> Bu betiği yürütmek için [Azure AD uygulama Yöneticisi rolünün](../active-directory/roles/permissions-reference.md#all-roles) bir üyesi olmanız gerekir.

### <a name="connect-to-your-azure-tenant"></a>Azure kiracınıza bağlanın
İlk olarak, komutunu kullanarak Azure kiracınıza bağlanın `Connect-AzureAD` . 

```PowerShell
$myWebhookAadTenantId = "<Your Webhook's Azure AD tenant id>"

Connect-AzureAD -TenantId $myWebhookAadTenantId
```

### <a name="create-microsofteventgrid-service-principal"></a>Microsoft. EventGrid hizmet sorumlusu oluşturma
Zaten yoksa **Microsoft. EventGrid** için hizmet sorumlusu oluşturmak üzere aşağıdaki betiği çalıştırın. 

```PowerShell
# This is the "Azure Event Grid" Azure Active Directory (AAD) AppId
$eventGridAppId = "4962773b-9cdb-44cf-a8bf-237846a00ab7"

# Create the "Azure Event Grid" AAD Application service principal if it doesn't exist
$eventGridSP = Get-AzureADServicePrincipal -Filter ("appId eq '" + $eventGridAppId + "'")
if ($eventGridSP -match "Microsoft.EventGrid")
{
    Write-Host "The Service principal is already defined.`n"
}
else
{
    # Create a service principal for the "Azure Event Grid" AAD Application and add it to the role
    Write-Host "Creating the Azure Event Grid service principal"
    $eventGridSP = New-AzureADServicePrincipal -AppId $eventGridAppId
}
```

### <a name="create-a-role-for-your-application"></a>Uygulamanız için rol oluşturma   
Azure AD uygulamanız için bir rol oluşturmak üzere aşağıdaki betiği çalıştırın. Bu örnekte, rol adı: **AzureEventGridSecureWebhookSubscriber**. PowerShell betiğini, `$myTenantId` Azure AD KIRACı kimliğinizi ve `$myAzureADApplicationObjectId` Azure AD UYGULAMANıZıN nesne kimliğiyle birlikte kullanmak üzere değiştirin.

```PowerShell
# This is your Webhook's Azure AD Application's ObjectId. 
$myWebhookAadApplicationObjectId = "<Your webhook's aad application object id>"

# This is the name of the new role we will add to your Azure AD Application
$eventGridRoleName = "AzureEventGridSecureWebhookSubscriber"
       
# Create an application role of given name and description
Function CreateAppRole([string] $Name, [string] $Description)
{
    $appRole = New-Object Microsoft.Open.AzureAD.Model.AppRole
    $appRole.AllowedMemberTypes = New-Object System.Collections.Generic.List[string]
    $appRole.AllowedMemberTypes.Add("Application");
    $appRole.AllowedMemberTypes.Add("User");
    $appRole.DisplayName = $Name
    $appRole.Id = New-Guid
    $appRole.IsEnabled = $true
    $appRole.Description = $Description
    $appRole.Value = $Name;
    return $appRole
}
       
# Get my Azure AD Application, it's roles and service principal
$myApp = Get-AzureADApplication -ObjectId $myWebhookAadApplicationObjectId
$myAppRoles = $myApp.AppRoles

Write-Host "App Roles before addition of new role.."
Write-Host $myAppRoles
       
# Create the role if it doesn't exist
if ($myAppRoles -match $eventGridRoleName)
{
    Write-Host "The Azure Event Grid role is already defined.`n"
}
else
{      
    # Add our new role to the Azure AD Application
    Write-Host "Creating the Azure Event Grid role in Azure Ad Application: " $myWebhookAadApplicationObjectId
    $newRole = CreateAppRole -Name $eventGridRoleName -Description "Azure Event Grid Role"
    $myAppRoles.Add($newRole)
    Set-AzureADApplication -ObjectId $myApp.ObjectId -AppRoles $myAppRoles
}

# print application's roles
Write-Host "My Azure AD Application's Roles: "
Write-Host $myAppRoles

```

### <a name="create-a-role-assignment"></a>Rol ataması oluşturma
Rol atamasının, AAD uygulaması veya olay aboneliğini oluşturan AAD kullanıcısı için Web kancası Azure AD Uygulaması oluşturulması gerekir. AAD uygulaması veya AAD kullanıcısının olay aboneliğini oluşturup oluşturmadığına bağlı olarak aşağıdaki komut dosyalarından birini kullanın.

#### <a name="option-a-create-a-role-assignment-for-event-subscription-aad-app"></a>A seçeneği. olay aboneliği AAD uygulaması için rol ataması oluşturma 

```powershell
# This is the app id of the application which will create event subscription. Set to $null if you are not assigning the role to app.
$eventSubscriptionWriterAppId = "<the app id of the application which will create event subscription>"

$myServicePrincipal = Get-AzureADServicePrincipal -Filter ("appId eq '" + $myApp.AppId + "'")

$eventSubscriptionWriterSP = Get-AzureADServicePrincipal -Filter ("appId eq '" + $eventSubscriptionWriterAppId + "'")
if ($eventSubscriptionWriterSP -eq $null)
{
        $eventSubscriptionWriterSP = New-AzureADServicePrincipal -AppId $eventSubscriptionWriterAppId
}

Write-Host "Creating the Azure Ad App Role assignment for application: " $eventSubscriptionWriterAppId
New-AzureADServiceAppRoleAssignment -Id $myApp.AppRoles[0].Id -ResourceId $myServicePrincipal.ObjectId -ObjectId $eventSubscriptionWriterSP.ObjectId -PrincipalId $eventSubscriptionWriterSP.ObjectId
```

#### <a name="option-b-create-a-role-assignment-for-event-subscription-aad-user"></a>Seçenek B. olay aboneliği AAD kullanıcısı için bir rol ataması oluşturma 

```powershell
# This is the user principal name of the user who will create event subscription. Set to $null if you are not assigning the role to user.
$eventSubscriptionWriterUserPrincipalName = "<the user principal name of the user who will create event subscription>"

$myServicePrincipal = Get-AzureADServicePrincipal -Filter ("appId eq '" + $myApp.AppId + "'")
    
Write-Host "Creating the Azure Ad App Role assignment for user: " $eventSubscriptionWriterUserPrincipalName
$eventSubscriptionWriterUser = Get-AzureAdUser -ObjectId $eventSubscriptionWriterUserPrincipalName
New-AzureADUserAppRoleAssignment -Id $myApp.AppRoles[0].Id -ResourceId $myServicePrincipal.ObjectId -ObjectId $eventSubscriptionWriterUser.ObjectId -PrincipalId $eventSubscriptionWriterUser.ObjectId
```

### <a name="add-event-grid-service-principal-to-the-role"></a>Role Event Grid hizmet sorumlusu ekleyin
Önceki adımda oluşturduğunuz role Event Grid hizmet sorumlusu atamak için New-AzureADServiceAppRoleAssignment komutunu çalıştırın.

```powershell
New-AzureADServiceAppRoleAssignment -Id $myApp.AppRoles[0].Id -ResourceId $myServicePrincipal.ObjectId -ObjectId $eventGridSP.ObjectId -PrincipalId $eventGridSP.ObjectId
```

Daha sonra kullanacağınız bilgileri çıkarmak için aşağıdaki komutları çalıştırın.

```powershell
Write-Host "My Webhook's Azure AD Tenant Id:  $myWebhookAadTenantId"
Write-Host "My Webhook's Azure AD Application Id: $($myApp.AppId)"
Write-Host "My Webhook's Azure AD Application ObjectId Id$($myApp.ObjectId)"
```

    
## <a name="configure-the-event-subscription"></a>Olay aboneliğini yapılandırma
Bir olay aboneliği oluştururken, şu adımları izleyin:

1. **Web kancası** olarak uç nokta türünü seçin. 
1. Uç nokta **URI**'sini belirtin.

    ![Uç nokta türü Web kancasını seçin](./media/secure-webhook-delivery/select-webhook.png)
1. **Olay abonelikleri oluştur** sayfasının en üstündeki **ek özellikler** sekmesini seçin.
1. **Ek özellikler** sekmesinde şu adımları uygulayın:
    1. **AAD kimlik doğrulaması kullan**' ı seçin ve Kiracı kimliğini ve uygulama kimliğini yapılandırın:
    1. Azure AD kiracı KIMLIĞINI betiğin çıktısından kopyalayın ve **AAD KIRACı kimliği** alanına girin.
    1. Azure AD uygulama KIMLIĞI 'ni betiğin çıktısından kopyalayın ve **AAD uygulama kimliği** alanına girin.

        ![Güvenli Web kancası eylemi](./media/secure-webhook-delivery/aad-configuration.png)



## <a name="next-steps"></a>Sonraki adımlar

* Olay teslimatlarını izleme hakkında bilgi için bkz. [izleyici Event Grid ileti teslimi](monitor-event-delivery.md).
* Kimlik doğrulama anahtarı hakkında daha fazla bilgi için bkz. [Event Grid Security and Authentication](security-authentication.md).
* Azure Event Grid aboneliği oluşturma hakkında daha fazla bilgi için bkz. [Event Grid abonelik şeması](subscription-creation-schema.md).
