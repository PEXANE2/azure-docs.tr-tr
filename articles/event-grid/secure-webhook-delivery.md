---
title: Azure Etkinlik Ağı'nda Azure AD ile Güvenli WebHook teslimatı
description: Azure Etkinlik Ağıt'ı kullanarak Azure Etkin Dizini tarafından korunan HTTPS uç noktalarına etkinlikleri nasıl teslim edileteceklerini açıklar
services: event-grid
author: banisadr
ms.service: event-grid
ms.topic: conceptual
ms.date: 11/18/2019
ms.author: babanisa
ms.openlocfilehash: 074378668b0516936e11968ea8c800d3daa667bb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74931548"
---
# <a name="publish-events-to-azure-active-directory-protected-endpoints"></a>Olayları Azure Active Directory korumalı uç noktalarına yayımlama

Bu makalede, Etkinlik Aboneliğiniz ile webhook bitiş noktanız arasındaki bağlantıyı güvence altına almak için Azure Etkin Dizin'den nasıl yararlanılanıncaaçıklanmaktadır. Azure AD Uygulamalarına ve hizmet ilkelerine genel bakış için [Microsoft kimlik platformuna (v2.0) genel bakış](https://docs.microsoft.com/azure/active-directory/develop/v2-overview)bölümüne bakın.

Bu makalede, gösteri için Azure portalı kullanır, ancak özellik CLI, PowerShell veya SDK'lar kullanılarak da etkinleştirilebilir.

[!INCLUDE [event-grid-preview-feature-note.md](../../includes/event-grid-preview-feature-note.md)]

## <a name="create-an-azure-ad-application"></a>Azure AD Uygulaması Oluşturma

Korumalı bitiş noktanız için bir Azure REKLAM Uygulaması oluşturarak başlayın. Bkz. https://docs.microsoft.com/azure/active-directory/develop/scenario-protected-web-api-overview.
    - Korumalı API'nizi bir daemon uygulaması tarafından çağrılacak şekilde yapılandırın.
    
## <a name="enable-event-grid-to-use-your-azure-ad-application"></a>Azure REKLAM Uygulamanızı kullanmak için Olay Kılavuzunu Etkinleştirme

Azure AD Uygulamanızda bir rol ve hizmet ilkesi oluşturmak için aşağıdaki PowerShell komut dosyasını kullanın. Azure REKLAM Uygulamanızdan Kiracı Kimliği ve Nesne Kimliği'ne ihtiyacınız olacaktır:

    > [!NOTE]
    > You must be a member of the [Azure AD Application Administrator role](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles#available-roles) to execute this script.
    
1. Azure AD Kiracı Kimliğinizi kullanmak için PowerShell komut dosyasının $myTenantId değiştirin.
1. Azure REKLAM Uygulamanızın Nesne Kimliğini kullanmak için PowerShell komut dosyasının $myAzureADApplicationObjectId değiştirme
1. Değiştirilmiş komut dosyasını çalıştırın.

```PowerShell
# This is your Tenant Id. 
$myTenantId = "<the Tenant Id of your Azure AD Application>"

Connect-AzureAD -TenantId $myTenantId
    
# This is your Azure AD Application's ObjectId. 
$myAzureADApplicationObjectId = "<the Object Id of your Azure AD Application>"
    
# This is the "Azure Event Grid" Azure Active Directory AppId
$eventGridAppId = "4962773b-9cdb-44cf-a8bf-237846a00ab7"
    
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
$eventGridSP = Get-AzureADServicePrincipal -Filter ("appId eq '" + $eventGridAppId + "'")

Write-Host "App Roles before addition of new role.."
Write-Host $myAppRoles
    
# Create the role if it doesn't exist
if ($myAppRoles -match $eventGridRoleName)
{
    Write-Host "The Azure Event Grid role is already defined.`n"
}
else
{
    $myServicePrincipal = Get-AzureADServicePrincipal -Filter ("appId eq '" + $myApp.AppId + "'")
    
    # Add our new role to the Azure AD Application
    $newRole = CreateAppRole -Name $eventGridRoleName -Description "Azure Event Grid Role"
    $myAppRoles.Add($newRole)
    Set-AzureADApplication -ObjectId $myApp.ObjectId -AppRoles $myAppRoles
}
    
# Create the service principal if it doesn't exist
if ($eventGridSP -match "Microsoft.EventGrid")
{
    Write-Host "The Service principal is already defined.`n"
}
else
{
    # Create a service principal for the "Azure Event Grid" Azure AD Application and add it to the role
    $eventGridSP = New-AzureADServicePrincipal -AppId $eventGridAppId
}
    
New-AzureADServiceAppRoleAssignment -Id $myApp.AppRoles[0].Id -ResourceId $myServicePrincipal.ObjectId -ObjectId $eventGridSP.ObjectId -PrincipalId $eventGridSP.ObjectId
    
Write-Host "My Azure AD Tenant Id" + $myTenantId
Write-Host "My Azure AD Application Id" + $myAzureADApplicationObjectId
Write-Host "My Azure AD Application ($myApp.ObjectId): " + $myApp.ObjectId
Write-Host "My Azure AD Application's Roles"
Write-Host $myApp.AppRoles
```
    
## <a name="configure-the-event-subscription"></a>Olay aboneliğini yapılandırma

Etkinlik aboneliğiniz için oluşturma akışında bitiş noktası türü 'Web Hook'u seçin. Bitiş noktası URI'nizi verdikten sonra, etkinlik abonelikleri oluşturma bıçağının üst kısmındaki ek özellikler sekmesine tıklayın.

![Uç nokta türü webhook'u seçin](./media/secure-webhook-delivery/select-webhook.png)

Ek özellikler sekmesinde, 'AAD kimlik doğrulamasını kullan' kutusunu işaretleyin ve Kiracı Kimliği ve Uygulama Kimliğini yapılandırın:

* Azure AD Kiracı Kimliğini komut dosyasının çıktısından kopyalayın ve AAD Kiracı Kimliği alanına girin.
* Azure AD Uygulama Kimliğini komut dosyasının çıktısından kopyalayın ve AAD Uygulama Kimliği alanına girin.

    ![Güvenli Webhook eylemi](./media/secure-webhook-delivery/aad-configuration.png)

## <a name="next-steps"></a>Sonraki adımlar

* Olay teslimlerini izleme hakkında daha fazla bilgi için [Bkz.](monitor-event-delivery.md)
* Kimlik doğrulama anahtarı hakkında daha fazla bilgi için [Olay Izgara güvenliği ve kimlik doğrulama](security-authentication.md)sı'na bakın.
* Azure Olay Ağı aboneliği oluşturma hakkında daha fazla bilgi için [Olay Ağı abonelik şemasına](subscription-creation-schema.md)bakın.
