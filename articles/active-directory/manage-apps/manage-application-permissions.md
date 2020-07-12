---
title: Kullanıcı ve yönetici izinlerini yönetme-Azure Active Directory | Microsoft Docs
description: Azure AD 'de uygulama için izinleri incelemeyi ve yönetmeyi öğrenin. Örneğin, bir uygulamaya verilen tüm izinleri iptal etmek istiyorsanız.
services: active-directory
author: mimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: conceptual
ms.date: 7/10/2020
ms.author: mimart
ms.reviewer: luleonpla
ms.collection: M365-identity-device-management
ms.openlocfilehash: 00d878c7b2f78d037e89235f3bb30c02fd11a7ae
ms.sourcegitcommit: 0b2367b4a9171cac4a706ae9f516e108e25db30c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2020
ms.locfileid: "86277652"
---
# <a name="take-action-on-overpriviledged-or-suspicious-application-in-azure-active-directory"></a>Fazla ayrıcalıklı veya Azure Active Directory şüpheli uygulama üzerinde işlem gerçekleştirin

Uygulama izinlerini incelemeyi ve yönetmeyi öğrenin. Senaryoya bağlı olarak, bu makale uygulamanızın güvenliğini sağlamak için gerçekleştirebileceğiniz farklı eylemler sağlar. Bu, Kullanıcı veya yönetici izni aracılığıyla Azure Active Directory (Azure AD) kiracısına eklenen tüm uygulamalar için geçerlidir.

Uygulamalara yönelik yarışmaya yönelik daha fazla bilgi için bkz. [Azure Active Directory izin çerçevesi](../develop/consent-framework.md).

## <a name="prerequisites"></a>Önkoşullar

Aşağıdaki eylemleri gerçekleştirebilmek için genel yönetici, uygulama Yöneticisi veya bulut uygulama Yöneticisi olarak oturum açmanız gerekir.

Uygulamalara erişimi kısıtlamak için Kullanıcı ataması yapmanız ve ardından kullanıcılara veya grupları uygulamaya atamanız gerekir.  Daha fazla bilgi için bkz. [kullanıcıları ve grupları atamaya yönelik yöntemler](methods-for-assigning-users-and-groups.md).

Eylemleri gerçekleştirmek üzere bağlamsal PowerShell betikleri almak için Azure AD portalına erişebilirsiniz.
 
1. [Azure Portal](https://portal.azure.com) genel yönetici, uygulama Yöneticisi veya bulut uygulama Yöneticisi olarak oturum açın.
2. **Azure Active Directory**  >  **Kurumsal uygulamalar**' ı seçin.
3. Erişimi kısıtlamak istediğiniz uygulamayı seçin.
4. **İzinler**' i seçin. Komut çubuğunda, **Izinleri gözden geçir**' i seçin.

![İzinleri gözden geçir](./media/manage-application-permissions/review-permissions.png)

## <a name="i-want-to-control-access-to-an-application"></a>Bir uygulamaya erişimi denetlemek istiyorum

Üzerinde kullanıcı atama ayarını açıp bu uygulamaya erişimi kısıtlamanız önerilir.

1. [Azure Portal](https://portal.azure.com) genel yönetici, uygulama Yöneticisi veya bulut uygulama Yöneticisi olarak oturum açın.
2. **Azure Active Directory**  >  **Kurumsal uygulamalar**' ı seçin.
3. Erişimi kısıtlamak istediğiniz uygulamayı seçin.
4. **Özellikler** ' i seçin ve ardından Kullanıcı gereksinimi gerekli ayarını Evet olarak ayarlayın.
5. **Kullanıcı ve gruplar** ' ı seçin ve ardından uygulamaya atanan istenmeyen kullanıcıları kaldırın.
6. Uygulamaya Kullanıcı veya grup atayın.

İsteğe bağlı, PowerShell kullanarak uygulamaya atanan tüm kullanıcıları kaldırabilirsiniz.

## <a name="i-want-to-revoke-all-permissions-for-an-application"></a>Bir uygulama için tüm izinleri iptal etmek istiyorum

PowerShell 'in kullanılması, bu uygulamaya verilen tüm izinleri iptal eder.

> [!NOTE]
> Verilen geçerli izin iptal edildiğinde, kullanıcılara reconseing için kullanıcılar tarafından durdurulmayacaktır. Kullanıcıların uygulamaya yarışmasına engel olmak istiyorsanız, [son kullanıcıların uygulamaları nasıl kabul](configure-user-consent.md)ettıklarını okuyun.

İsteğe bağlı olarak, kullanıcıların uygulamaya erişimini engellemek için uygulamayı devre dışı bırakabilir ve uygulamanın verilerinize erişimini engelleyebilirsiniz.

1. [Azure Portal](https://portal.azure.com) genel yönetici, uygulama Yöneticisi veya bulut uygulama Yöneticisi olarak oturum açın.
2. **Azure Active Directory**  >  **Kurumsal uygulamalar**' ı seçin.
3. Erişimi kısıtlamak istediğiniz uygulamayı seçin.
4. **Özellikler** ' i seçin ve ardından kullanıcıların oturum açması için etkin ' i ayarlayın. Hayır.

## <a name="application-is-suspicious-and-i-want-to-investigate"></a>Uygulama şüpheli ve araştırmak istiyorum

Üzerinde kullanıcı atama ayarını açıp Kullanıcı ve yöneticilerin uygulamaya verdiği izinleri gözden geçirerek bu uygulamaya erişimi kısıtlamanız önerilir.

1. [Azure Portal](https://portal.azure.com) genel yönetici, uygulama Yöneticisi veya bulut uygulama Yöneticisi olarak oturum açın.
3. **Azure Active Directory**  >  **Kurumsal uygulamalar**' ı seçin.
5. Erişimi kısıtlamak istediğiniz uygulamayı seçin.
6. **Özellikler** ' i seçin ve ardından Kullanıcı gereksinimi gerekli ayarını Evet olarak ayarlayın.
7. **İzinler** ' i seçin ve yönetici ve Kullanıcı tarafından onaylanan izinleri gözden geçirin.

İsteğe bağlı, şunları yapabilirsiniz:

- PowerShell kullanarak, uygulamanın oturum açmasını durdurmak için atanan tüm kullanıcıları kaldırın.
- PowerShell kullanarak, uygulamaya erişimi olan kullanıcılar için yenileme belirteçlerini geçersiz kılar.
- PowerShell kullanarak bu uygulama için tüm izinleri iptal et
- Kullanıcıları erişimini engellemek ve bu uygulamaların verilerinize erişimini durdurmak için uygulamayı devre dışı bırakın.


## <a name="application-is-malicious-and-im-compromised"></a>Uygulama kötü amaçlı ve tehlikede

Kullanıcıların uygulamaya ve uygulamanın verilerinize erişmesini engellemek için uygulamayı devre dışı bırakmanızı öneririz. Bunun yerine uygulamayı silerseniz, son kullanıcılar uygulamaya reconsent ve verilerinize erişim verebilir.

1. [Azure Portal](https://portal.azure.com) genel yönetici, uygulama Yöneticisi veya bulut uygulama Yöneticisi olarak oturum açın.
2. **Azure Active Directory**  >  **Kurumsal uygulamalar**' ı seçin.
3. Erişimi kısıtlamak istediğiniz uygulamayı seçin.
4. **Özellikler** ' i seçin ve ardından nesne kimliğini kopyalayın.

### <a name="powershell-commands"></a>PowerShell komutları


Hizmet sorumlusu nesne KIMLIĞINI al

1. [Azure Portal](https://portal.azure.com) genel yönetici, uygulama Yöneticisi veya bulut uygulama Yöneticisi olarak oturum açın.
2. **Azure Active Directory**  >  **Kurumsal uygulamalar**' ı seçin.
3. Erişimi kısıtlamak istediğiniz uygulamayı seçin.
4. **Özellikler** ' i seçin ve ardından nesne kimliğini kopyalayın.

```powershell
    $sp = Get-AzureADServicePrincipal -Filter "displayName eq '$app_name'"
    $sp.ObjectId
```
Uygulamaya atanan tüm kullanıcıları kaldırın.
 ```powershell
    Connect-AzureAD

    # Get Service Principal using objectId
    $sp = Get-AzureADServicePrincipal -ObjectId "<ServicePrincipal objectID>"

    # Get Azure AD App role assignments using objectId of the Service Principal
    $assignments = Get-AzureADServiceAppRoleAssignment -ObjectId $sp.ObjectId -All $true

    # Remove all users and groups assigned to the application
    $assignments | ForEach-Object {
        if ($_.PrincipalType -eq "User") {
            Remove-AzureADUserAppRoleAssignment -ObjectId $_.PrincipalId -AppRoleAssignmentId $_.ObjectId
        } elseif ($_.PrincipalType -eq "Group") {
            Remove-AzureADGroupAppRoleAssignment -ObjectId $_.PrincipalId -AppRoleAssignmentId $_.ObjectId
        }
    }
 ```

Uygulamaya verilen izinleri iptal et

```powershell
    Connect-AzureAD

    # Get Service Principal using objectId
    $sp = Get-AzureADServicePrincipal -ObjectId "<ServicePrincipal objectID>"

    # Get all delegated permissions for the service principal
    $spOAuth2PermissionsGrants = Get-AzureADOAuth2PermissionGrant -All $true| Where-Object { $_.clientId -eq $sp.ObjectId }

    # Remove all delegated permissions
    $spOAuth2PermissionsGrants | ForEach-Object {
        Remove-AzureADOAuth2PermissionGrant -ObjectId $_.ObjectId
    }

    # Get all application permissions for the service principal
    $spApplicationPermissions = Get-AzureADServiceAppRoleAssignedTo -ObjectId $sp.ObjectId -All $true | Where-Object { $_.PrincipalType -eq "ServicePrincipal" }

    # Remove all delegated permissions
    $spApplicationPermissions | ForEach-Object {
        Remove-AzureADServiceAppRoleAssignment -ObjectId $_.PrincipalId -AppRoleAssignmentId $_.objectId
    }
```
Yenileme belirteçlerini geçersiz kıl
```powershell
        Connect-AzureAD

        # Get Service Principal using objectId
        $sp = Get-AzureADServicePrincipal -ObjectId "<ServicePrincipal objectID>"

        # Get Azure AD App role assignments using objectID of the Service Principal
        $assignments = Get-AzureADServiceAppRoleAssignment -ObjectId $sp.ObjectId -All $true | Where-Object {$_.PrincipalType -eq "User"}

        # Revoke refresh token for all users assigned to the application
        $assignments | ForEach-Object {
            Revoke-AzureADUserAllRefreshToken -ObjectId $_.PrincipalId
        }
```
## <a name="next-steps"></a>Sonraki adımlar
- [Uygulamalara onayı yönetin ve onay isteğini değerlendirin](manage-consent-requests.md)
- [Kullanıcı onayını yapılandırma](configure-user-consent.md)
- [Yönetici onayı iş akışını yapılandırma](configure-admin-consent-workflow.md)
