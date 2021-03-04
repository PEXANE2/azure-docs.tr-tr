---
title: Azure Event Grid 'de Azure AD ile güvenli Web kancası teslimi
description: Azure Event Grid kullanılarak Azure Active Directory korunan HTTPS uç noktalarına olayların nasıl teslim edileceğini açıklar
ms.topic: how-to
ms.date: 02/03/2021
ms.openlocfilehash: be38edfb4edfee5b3a3b42d7b1c5a44c9d72ceef
ms.sourcegitcommit: f3ec73fb5f8de72fe483995bd4bbad9b74a9cc9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/04/2021
ms.locfileid: "102035454"
---
# <a name="publish-events-to-azure-active-directory-protected-endpoints"></a>Olayları Azure Active Directory korumalı uç noktalara yayımlama

Bu makalede, olay aboneliğiniz ve Web kancası uç noktanız arasındaki bağlantıyı güvenli hale getirmek için Azure Active Directory avantajlarından nasıl yararlanabilmeniz açıklanmaktadır. Azure AD uygulamalarına ve hizmet sorumlularına genel bakış için bkz. [Microsoft Identity platform (v 2.0) genel bakış](../active-directory/develop/v2-overview.md).

Bu makale, tanıtım için Azure portal kullanır, ancak özellik CLı, PowerShell veya SDK 'lar kullanılarak da etkinleştirilebilir.


## <a name="create-an-azure-ad-application"></a>Azure AD uygulaması oluşturma

Korumalı uç noktanız için bir Azure AD uygulaması oluşturarak başlayın. Bkz. https://docs.microsoft.com/azure/active-directory/develop/scenario-protected-web-api-overview.
    - Korumalı API 'nizi bir Daemon uygulaması tarafından çağrılacak şekilde yapılandırın.
    
## <a name="enable-event-grid-to-use-your-azure-ad-application"></a>Azure AD uygulamanızı kullanmak için Event Grid etkinleştirme
Bu bölümde Azure AD uygulamanızı kullanmak için Event Grid nasıl etkinleştirileceği gösterilmektedir. 

> [!NOTE]
> Bu betiği yürütmek için [Azure AD uygulama Yöneticisi rolünün](../active-directory/roles/permissions-reference.md#all-roles) bir üyesi olmanız gerekir.

### <a name="connect-to-your-azure-tenant"></a>Azure kiracınıza bağlanın
İlk olarak, komutunu kullanarak Azure kiracınıza bağlanın `Connect-AzureAD` . 

```PowerShell
# This is your Tenant Id. 
$myTenantId = "<the Tenant Id of your Azure AD Application>"
Connect-AzureAD -TenantId $myTenantId
```

### <a name="create-microsofteventgrid-service-principal"></a>Microsoft. EventGrid hizmet sorumlusu oluşturma
Zaten yoksa **Microsoft. EventGrid** için hizmet sorumlusu oluşturmak üzere aşağıdaki betiği çalıştırın. 

```PowerShell
# This is the "Azure Event Grid" Azure Active Directory AppId
$eventGridAppId = "4962773b-9cdb-44cf-a8bf-237846a00ab7"
    
$eventGridSP = Get-AzureADServicePrincipal -Filter ("appId eq '" + $eventGridAppId + "'")

# Create the service principal if it doesn't exist
if ($eventGridSP -match "Microsoft.EventGrid")
{
    Write-Host "The Service principal is already defined.`n"
} else
{
    # Create a service principal for the "Azure Event Grid" Azure AD Application and add it to the role
    $eventGridSP = New-AzureADServicePrincipal -AppId $eventGridAppId
}
```

### <a name="create-a-role-for-your-application"></a>Uygulamanız için rol oluşturma   
Azure AD uygulamanız için bir rol oluşturmak üzere aşağıdaki betiği çalıştırın. Bu örnekte, rol adı: **AzureEventGridSecureWebhook**. PowerShell betiğini, `$myTenantId` Azure AD KIRACı kimliğinizi ve `$myAzureADApplicationObjectId` Azure AD UYGULAMANıZıN nesne kimliğiyle birlikte kullanmak üzere değiştirin.

```PowerShell
# This is your Azure AD Application's ObjectId. 
$myAzureADApplicationObjectId = "<the Object Id of your Azure AD Application>"
    
# This is the name of the new role we will add to your Azure AD Application
$eventGridRoleName = "AzureEventGridSecureWebhook"
    
# Create an application role of given name and description
Function CreateAppRole([string] $Name, [string] $Description)
{
    $appRole = New-Object Microsoft.Open.AzureAD.Model.AppRole
    $appRole.AllowedMemberTypes = New-Object System.Collections.Generic.List[string]
    $appRole.AllowedMemberTypes.Add("Application");
    $appRole.DisplayName = $Name
    $appRole.Id = New-Guid
    $appRole.IsEnabled = $true
    $appRole.Description = $Description
    $appRole.Value = $Name;
    return $appRole
}

# Get my Azure AD Application, it's roles and service principal
$myApp = Get-AzureADApplication -ObjectId $myAzureADApplicationObjectId
$myAppRoles = $myApp.AppRoles

Write-Host "App Roles before addition of new role.."
Write-Host $myAppRoles
    
# Create the role if it doesn't exist
if ($myAppRoles -match $eventGridRoleName)
{
    Write-Host "The Azure Event Grid role is already defined.`n"
    $myServicePrincipal = Get-AzureADServicePrincipal -Filter ("appId eq '" + $myApp.AppId + "'")
} else
{
    $myServicePrincipal = Get-AzureADServicePrincipal -Filter ("appId eq '" + $myApp.AppId + "'")
    
    # Add our new role to the Azure AD Application
    $newRole = CreateAppRole -Name $eventGridRoleName -Description "Azure Event Grid Role"
    $myAppRoles.Add($newRole)
    Set-AzureADApplication -ObjectId $myApp.ObjectId -AppRoles $myAppRoles
}

# print application's roles
Write-Host "My Azure AD Application's Roles: "
Write-Host $myAppRoles
```

### <a name="add-event-grid-service-principal-to-the-role"></a>Role Event Grid hizmet sorumlusu ekleyin    
Şimdi, `New-AzureADServiceAppRoleAssignment` önceki adımda oluşturduğunuz role Event Grid hizmet sorumlusu atamak için komutunu çalıştırın. 

```powershell
New-AzureADServiceAppRoleAssignment -Id $myApp.AppRoles[0].Id -ResourceId $myServicePrincipal.ObjectId -ObjectId $eventGridSP.ObjectId -PrincipalId $eventGridSP.ObjectId
```

Sonraki adımları kullanacağınız bilgileri çıkarmak için aşağıdaki komutları çalıştırın. 

```powershell    
Write-Host "My Azure AD Tenant Id: $myTenantId"
Write-Host "My Azure AD Application Id: $($myApp.AppId)"
Write-Host "My Azure AD Application ObjectId: $($myApp.ObjectId)"
```
    
## <a name="configure-the-event-subscription"></a>Olay aboneliğini yapılandırma

Olay aboneliğiniz için oluşturma akışında, ' Web kancası ' uç nokta türünü seçin. Uç nokta URI 'nizi aldıktan sonra, olay abonelikleri oluştur dikey penceresinin en üstündeki ek özellikler sekmesine tıklayın.

![Uç nokta türü Web kancasını seçin](./media/secure-webhook-delivery/select-webhook.png)

Ek Özellikler sekmesinde, ' AAD kimlik doğrulaması kullan ' kutusunu işaretleyin ve kiracı KIMLIĞINI ve uygulama KIMLIĞINI yapılandırın:

* Azure AD kiracı KIMLIĞINI betiğin çıktısından kopyalayın ve AAD kiracı KIMLIĞI alanına girin.
* Azure AD uygulama KIMLIĞI 'ni betiğin çıktısından kopyalayın ve AAD uygulama KIMLIĞI alanına girin.

    ![Güvenli Web kancası eylemi](./media/secure-webhook-delivery/aad-configuration.png)

## <a name="next-steps"></a>Sonraki adımlar

* Olay teslimatlarını izleme hakkında bilgi için bkz. [izleyici Event Grid ileti teslimi](monitor-event-delivery.md).
* Kimlik doğrulama anahtarı hakkında daha fazla bilgi için bkz. [Event Grid Security and Authentication](security-authentication.md).
* Azure Event Grid aboneliği oluşturma hakkında daha fazla bilgi için bkz. [Event Grid abonelik şeması](subscription-creation-schema.md).
