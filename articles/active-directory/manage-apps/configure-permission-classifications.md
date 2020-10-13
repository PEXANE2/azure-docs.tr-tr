---
title: Azure AD ile izin sınıflandırmalarını yapılandırma
description: Temsil edilen izin sınıflandırmalarını yönetmeyi öğrenin.
services: active-directory
author: kenwith
manager: celestedg
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: how-to
ms.date: 06/01/2020
ms.author: phsignor
ms.reviewer: arvindh, luleon, phsignor
ms.openlocfilehash: 0add5001e2e62e26d448b06927210c14f17729f1
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91804348"
---
# <a name="configure-permission-classifications"></a>İzin sınıflandırmalarını yapılandırma

İzin sınıflandırmaları, farklı izinlerin kuruluşunuzun ilkelerine ve risk değerlendirmelere göre sahip olduğu etkiyi tanımlamanızı sağlar. Örneğin, kullanıcıların izin verdiği izin kümesini belirlemek için izin sınıflandırmalar ' da izin sınıflandırmalarını kullanabilirsiniz.

## <a name="manage-permission-classifications"></a>İzin sınıflandırmalarını yönetme

Şu anda yalnızca "düşük etki" izin sınıflandırması desteklenir. Yalnızca yönetici onayı gerektirmeyen temsilci izinleri "düşük etki" olarak sınıflandırılabilirler.

> [!TIP]
> Temel oturum açma için gereken en düşük izinler `openid` , `profile` `email` `User.Read` `offline_access` Microsoft Graph tüm temsilci izinleri olan,, ve ' dir. Bu izinlerle bir uygulama, oturum açmış kullanıcının tüm profil ayrıntılarını okuyabilir ve Kullanıcı artık uygulamayı kullanmıyor olsa bile bu erişimi koruyabilir.

# <a name="portal"></a>[Portal](#tab/azure-portal)

Azure portal kullanarak izinleri sınıflandırmak için aşağıdaki adımları izleyin:

1. [Azure Portal](https://portal.azure.com) [genel yönetici](../users-groups-roles/directory-assign-admin-roles.md#global-administrator--company-administrator)olarak oturum açın.
1. **Azure Active Directory**  >  **kurumsal uygulamaların**  >  **onay ve**izin  >  **izin sınıflandırmalarını**seçin.
1. Başka bir izni "düşük etki" olarak sınıflandırmak için **Izin Ekle** ' yi seçin.
1. API 'YI seçin ve ardından temsilci izinleri seçin.

Bu örnekte, çoklu oturum açma için gereken en düşük izin kümesini sınıflandırdık:

:::image type="content" source="media/configure-permission-classifications/permission-classifications.png" alt-text="İzin sınıflandırmaları":::

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

İzinleri sınıflandırmak için en son Azure AD PowerShell önizleme modülünü [Azureadpreview](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0-preview&preserve-view=true)' i kullanabilirsiniz. İzin sınıflandırmaları, izinleri yayımlayan API 'nin **ServicePrincipal** nesnesinde yapılandırılır.

#### <a name="list-the-current-permission-classifications-for-an-api"></a>Bir API için geçerli izin sınıflandırmalarını listeleme

1. API için **ServicePrincipal** nesnesini alın. Burada Microsoft Graph API 'SI için ServicePrincipal nesnesini alıyoruz:

   ```powershell
   $api = Get-AzureADServicePrincipal `
       -Filter "servicePrincipalNames/any(n:n eq 'https://graph.microsoft.com')"
   ```

1. API için Temsilcili izin sınıflandırmalarını okuyun:

   ```powershell
   Get-AzureADMSServicePrincipalDelegatedPermissionClassification `
       -ServicePrincipalId $api.ObjectId | Format-Table Id, PermissionName, Classification
   ```

#### <a name="classify-a-permission-as-low-impact"></a>Bir izni "düşük etki" olarak sınıflandırın

1. API için **ServicePrincipal** nesnesini alın. Burada Microsoft Graph API 'SI için ServicePrincipal nesnesini alıyoruz:

   ```powershell
   $api = Get-AzureADServicePrincipal `
       -Filter "servicePrincipalNames/any(n:n eq 'https://graph.microsoft.com')"
   ```

1. Sınıflandırmak istediğiniz temsilci iznini bulun:

   ```powershell
   $delegatedPermission = $api.OAuth2Permissions | Where-Object { $_.Value -eq "User.ReadBasic.All" }
   ```

1. İzin adını ve KIMLIĞINI kullanarak izin sınıflandırmasını ayarlayın:

   ```powershell
   Add-AzureADMSServicePrincipalDelegatedPermissionClassification `
      -ServicePrincipalId $api.ObjectId `
      -PermissionId $delegatedPermission.Id `
      -PermissionName $delegatedPermission.Value `
      -Classification "low"
   ```

#### <a name="remove-a-delegated-permission-classification"></a>Temsilci bir izin sınıflandırmasını kaldırma

1. API için **ServicePrincipal** nesnesini alın. Burada Microsoft Graph API 'SI için ServicePrincipal nesnesini alıyoruz:

   ```powershell
   $api = Get-AzureADServicePrincipal `
       -Filter "servicePrincipalNames/any(n:n eq 'https://graph.microsoft.com')"
   ```

1. Kaldırmak istediğiniz Temsilcili izin sınıflandırmasını bulun:

   ```powershell
   $classifications = Get-AzureADMSServicePrincipalDelegatedPermissionClassification `
       -ServicePrincipalId $api.ObjectId
   $classificationToRemove = $classifications | Where-Object {$_.PermissionName -eq "User.ReadBasic.All"}
   ```

1. İzin sınıflandırmasını silin:

   ```powershell
   Remove-AzureADMSServicePrincipalDelegatedPermissionClassification `
       -ServicePrincipalId $api.ObjectId `
       -Id $classificationToRemove.Id
   ```

---

## <a name="next-steps"></a>Sonraki adımlar

Daha fazlasını öğrenin:

* [Kullanıcı izin ayarlarını yapılandırma](configure-user-consent.md)
* [Yönetici onayı iş akışını yapılandırma](configure-admin-consent-workflow.md)
* [Uygulama onayını yönetmeyi ve izin isteklerini değerlendirmeyi öğrenin](manage-consent-requests.md)
* [Uygulamaya kiracı genelinde yönetici onayı verme](grant-admin-consent.md)
* [Microsoft Identity platformunda izinler ve onay](../develop/active-directory-v2-scopes.md)

Yardım almak veya sorularınıza yanıt bulmak için:
* [StackOverflow üzerinde Azure AD](https://stackoverflow.com/questions/tagged/azure-active-directory)
