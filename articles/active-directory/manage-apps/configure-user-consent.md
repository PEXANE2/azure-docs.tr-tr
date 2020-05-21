---
title: Son kullanıcıların Azure AD 'yi kullanarak uygulamalara nasıl izin vermesini yapılandırın
description: Kullanıcıların kuruluşunuzun verilerine erişimi olacak uygulamaları nasıl ve ne zaman kabul edebilir.
services: active-directory
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: conceptual
ms.date: 05/19/2020
ms.author: mimart
ms.reviewer: arvindh, luleon, phsignor
ms.openlocfilehash: 0a508e52189938447ea6fc1928d441d81deab392
ms.sourcegitcommit: 958f086136f10903c44c92463845b9f3a6a5275f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2020
ms.locfileid: "83714041"
---
# <a name="configure-how-end-users-consent-to-applications"></a>Son kullanıcıların uygulamalara nasıl izin vermesini yapılandırın

Kullanıcılarınızın iş veya okul hesabıyla oturum açmalarına ve zengin veri odaklı deneyimler sunmak üzere kuruluşunuzun verilerine erişmesine izin vermek için uygulamalarınızı Microsoft Identity platformu ile tümleştirebilirsiniz.

Bir uygulamanın kuruluşunuzun verilerine erişebilmesi için, kullanıcının uygulama izinlerini vermesi gerekir. Farklı izinler farklı erişim düzeylerine izin verir. Varsayılan olarak, tüm kullanıcıların, yönetici onayı gerektirmeyen izinler için uygulamalara izin verme izni verilir. Örneğin, varsayılan olarak bir Kullanıcı, bir uygulamanın posta kutularına erişmesine izin verebilir, ancak bir uygulama sınırsız erişimine izin vermeyi, kuruluşunuzdaki tüm dosyalara okuma ve yazma izni veremez.

Kullanıcıların uygulamalara veri erişimi vermesini sağlayarak, kullanıcılar kolayca yararlı uygulamalar alabilir ve üretken olabilirler. Ancak, bazı durumlarda bu yapılandırma izlenmiyor ve denetlenmiyorsa riski temsil edebilir.

## <a name="user-consent-settings"></a>Kullanıcı izin ayarları

Kullanıcılara uygulamalara izin verebilecekleri durumları denetlemek için, tüm kullanıcılar için uygulanacak onay ilkesini seçin. Üç onay ilkesi seçeneği aşağıda verilmiştir:

* **Kullanıcı onayını devre dışı bırak** -kullanıcılar uygulamalara izin veremez. Kullanıcılar, daha önce kendilerine onaylı olan veya kendi adına yöneticiler tarafından onaylanan uygulamalar üzerinde oturum açmaya devam edebilir, ancak yeni izinler veya kendi kendilerine yeni uygulamalar için izin verilmez. Yalnızca izin verme iznini içeren bir dizin rolü verilen kullanıcılar, yeni izinleri veya yeni uygulamaları kabul edebilir.

* **Kullanıcılar doğrulanan yayımcıların uygulamalarına izin verebilir, ancak yalnızca seçtiğiniz izinler için (Önizleme),** tüm kullanıcılar yalnızca [doğrulanmış bir yayımcı](../develop/publisher-verification-overview.md) ve kiracınızda kayıtlı uygulamalar tarafından yayımlanan uygulamalara izin verebilir. Kullanıcılar yalnızca "düşük etki" olarak sınıflandırdığınız izinleri kabul edebilir.

  Kullanıcıların izin verdiği izinleri seçmek için [izinleri sınıflandırdığınızdan](#configure-permission-classifications-preview) emin olun.

* **Kullanıcılar tüm uygulamalara izin verebilir** -Bu seçenek, tüm kullanıcıların herhangi bir uygulama için yönetici onayı gerektirmeyen herhangi bir izni kabul etmesine izin verir. 

   Kötü amaçlı uygulamaların, kullanıcıların kuruluşunuzun verilerine erişim izni vermesini sağlayan riski azaltmak için, yalnızca [doğrulanmış bir yayımcı](../develop/publisher-verification-overview.md)tarafından yayımlanan uygulamalar için Kullanıcı onaylamasına izin vermenizi öneririz.

### <a name="configure-user-consent-settings-from-the-azure-portal"></a>Azure portal Kullanıcı izin ayarlarını yapılandırma

Azure portal aracılığıyla Kullanıcı izin ayarlarını yapılandırmak için:

1. [Azure Portal](https://portal.azure.com) [genel yönetici](../users-groups-roles/directory-assign-admin-roles.md#global-administrator--company-administrator)olarak oturum açın.
1. **Azure Active Directory**  >  **Kurumsal uygulamalar**  >  **onay ve izinler**  >  **Kullanıcı izin ayarları**' nı seçin.
1. **Uygulamalar Için kullanıcı onayı**altında, tüm kullanıcılar için yapılandırmak istediğiniz izin ayarını seçin.
1. Ayarlarınızı kaydetmek için **Kaydet** ' i seçin.

![Kullanıcı izin ayarları](./media/configure-user-consent/setting-for-all-users.png)

> [!TIP]
> Kullanıcıların, kullanıcı onayı devre dışı bırakıldığında veya bir uygulama kullanıcının izin vermediği izinleri istemesi durumunda kullanıcının izin vermediği bir uygulamanın gözden geçirilmesini ve onayını istememe olanağı tanımak için [yönetici onayı iş akışını etkinleştirmeyi](configure-admin-consent-workflow.md) düşünün.

### <a name="configure-user-consent-settings-using-powershell"></a>PowerShell kullanarak Kullanıcı izin ayarlarını yapılandırma

Uygulamalara yönelik Kullanıcı onayını hangi onay ilkesinin yöneteceğini seçmek için, en son Azure AD PowerShell önizleme modülünü [Azureadpreview](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2?view=azureadps-2.0-preview)' i kullanabilirsiniz.

* **Kullanıcı onayını devre dışı** bırak-Kullanıcı onayını devre dışı bırakmak için, Kullanıcı onayını boş olacak şekilde belirleyen onay ilkelerini ayarlayın:

  ```powershell
  Set-AzureADMSAuthorizationPolicy `
     -Id "authorizationPolicy" `
     -PermissionGrantPolicyIdsAssignedToDefaultUserRole @()
  ```

* **Seçili izinler (Önizleme) için doğrulanan yayımcıların uygulamalara yönelik Kullanıcı onaylamasına izin** vermek için, yalnızca kiracınızda kayıtlı Yayımcılar ve uygulamalar için sınırlı Kullanıcı izni vermek ve yalnızca "düşük etki" olarak sınıflandırdığınız izinler için, şu adlı yerleşik izin ilkesini yapılandırın `microsoft-user-default-low` :

  ```powershell
  Set-AzureADMSAuthorizationPolicy `
     -Id "authorizationPolicy" `
     -PermissionGrantPolicyIdsAssignedToDefaultUserRole @("microsoft-user-default-low")
  ```

   Kullanıcıların izin verdiği izinleri belirlemek için [izin sınıflandırmasını](#configure-permission-classifications-preview) unutmayın.

* **Tüm uygulamalar için Kullanıcı onaylamasına Izin ver** -tüm uygulamalar için Kullanıcı onaylamasına izin ver:

  ```powershell
  Set-AzureADMSAuthorizationPolicy `
     -Id "authorizationPolicy" `
     -PermissionGrantPolicyIdsAssignedToDefaultUserRole @("microsoft-user-default-legacy")
  ```

   Bu seçenek, tüm kullanıcıların herhangi bir uygulama için yönetici onayı gerektirmeyen herhangi bir izni kabul etmesine olanak tanır. Yalnızca uygulamaların doğrulanmış yayımcılar için Kullanıcı onay izni vermenizi öneririz.

## <a name="configure-permission-classifications-preview"></a>İzin sınıflandırmalarını yapılandırma (Önizleme)

İzin sınıflandırmaları, farklı izinlerin kuruluşunuzun ilkelerine ve risk değerlendirmelere göre sahip olduğu etkiyi tanımlamanızı sağlar. Örneğin, kullanıcıların izin verdiği izin kümesini belirlemek için izin sınıflandırmalar ' da izin sınıflandırmalarını kullanabilirsiniz.

> [!NOTE]
> Şu anda yalnızca "düşük etki" izin sınıflandırması desteklenir. Yalnızca yönetici onayı gerektirmeyen temsilci izinleri "düşük etki" olarak sınıflandırılabilirler.

### <a name="classify-permissions-using-the-azure-portal"></a>Azure portal kullanarak izinleri sınıflandır

1. [Azure Portal](https://portal.azure.com) [genel yönetici](../users-groups-roles/directory-assign-admin-roles.md#global-administrator--company-administrator)olarak oturum açın.
1. **Azure Active Directory**  >  **kurumsal uygulamaların**  >  **onay ve**izin  >  **izin sınıflandırmalarını**seçin.
1. Başka bir izni "düşük etki" olarak sınıflandırmak için **Izin Ekle** ' yi seçin. 
1. API 'YI seçin ve ardından temsilci izinleri seçin.

Bu örnekte, çoklu oturum açma için gereken en düşük izin kümesini sınıflandırdık:

![İzin sınıflandırmaları](./media/configure-user-consent/permission-classifications.png)

> [!TIP]
> Microsoft Graph API 'si için, temel çoklu oturum açma için gereken en düşük izinler `openid` , ve ' `profile` dir `User.Read` `offline_access` . Bu izinlerle bir uygulama, oturum açmış kullanıcının profil ayrıntılarını okuyabilir ve Kullanıcı artık uygulamayı kullanmıyor olsa bile bu erişimi koruyabilir.

### <a name="classify-permissions-using-powershell"></a>PowerShell kullanarak izinleri sınıflandır

İzinleri sınıflandırmak için en son Azure AD PowerShell önizleme modülünü [Azureadpreview](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0-preview)' i kullanabilirsiniz. İzin sınıflandırmaları, izinleri yayımlayan API 'nin **ServicePrincipal** nesnesinde yapılandırılır.

#### <a name="to-read-the-current-permission-classifications-for-an-api"></a>Bir API için geçerli izin sınıflandırmalarını okumak için:

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

#### <a name="to-classify-a-permission-as-low-impact"></a>Bir izni "düşük etki" olarak sınıflandırmak için:

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

#### <a name="to-remove-a-delegated-permission-classification"></a>Temsilci bir izin sınıflandırmasını kaldırmak için:

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

## <a name="configure-group-owner-consent-to-apps-accessing-group-data"></a>Grup verilerine erişen uygulamalar için Grup sahibi onayını yapılandırma

Grup sahipleri, üçüncü taraf satıcılar tarafından yayımlanan uygulamalar gibi, kuruluşunuzun bir grupla ilişkili verilerinize erişebilmesi için uygulamalara yetki verebilir. Örneğin, Microsoft ekipteki bir takım sahibi, bir uygulamanın ekipteki tüm ekip iletilerini okumasına veya bir grubun üyelerinin temel profilini listeliğine izin verebilir.

Hangi kullanıcıların grupların verilerine erişen uygulamalara izin verileceğini yapılandırabilir veya bu özelliği devre dışı bırakabilirsiniz.

### <a name="configure-group-owner-consent-using-the-azure-portal"></a>Azure portal kullanarak Grup sahibi onayını yapılandırma

1. [Azure Portal](https://portal.azure.com) [genel yönetici](../users-groups-roles/directory-assign-admin-roles.md#global-administrator--company-administrator)olarak oturum açın.
2. **Azure Active Directory**  >  **Kurumsal uygulamalar**  >  **onay ve izinler**  >  **Kullanıcı izin ayarları**' nı seçin.
3. **Veriye erişen uygulamalar Için Grup sahibi onayı** altında etkinleştirmek istediğiniz seçeneği belirleyin.
4. Ayarlarınızı kaydetmek için **Kaydet** ' i seçin.

Bu örnekte, tüm Grup sahiplerinin grupların verilerine erişen uygulamalara izin vermesini sağlar:

![İzin sınıflandırmaları](./media/configure-user-consent/group-owner-consent.png)

### <a name="configure-group-owner-consent-using-powershell"></a>PowerShell kullanarak Grup sahibi onayını yapılandırma

Grup sahiplerini, sahip oldukları gruplar için kuruluşunuzun verilerine erişen uygulamalara izin verme özelliğini etkinleştirmek veya devre dışı bırakmak için Azure AD PowerShell önizleme modülünü ( [Azureadpreview](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0-preview)) kullanabilirsiniz.

1. [Azureadpreview](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0-preview) modülünü kullandığınızdan emin olun. Hem [Azuread](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0) modülünü hem de [azureadpreview](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0-preview) modülünü yüklediyseniz bu adım önemlidir.

    ```powershell
    Remove-Module AzureAD
    Import-Module AzureADPreview
    ```

1. Azure AD PowerShell 'e bağlanın.

   ```powershell
   Connect-AzureAD
   ```

1. Kiracınızdaki **Izin Ilkesi ayarları** dizin ayarları için geçerli değeri alın. Bunun için, bu özelliğin Dizin ayarlarının oluşturulup oluşturulmadığından ve ilgili dizin ayarları şablonundaki değerler kullanılarak denetlenmesi gerekir.

    ```powershell
    $consentSettingsTemplateId = "dffd5d46-495d-40a9-8e21-954ff55e198a" # Consent Policy Settings
    $settings = Get-AzureADDirectorySetting -All $true | Where-Object { $_.TemplateId -eq $consentSettingsTemplateId }

    if (-not $settings) {
        $template = Get-AzureADDirectorySettingTemplate -Id $consentSettingsTemplateId
        $settings = $template.CreateDirectorySetting()
    }

    $enabledValue = $settings.Values | ? { $_.Name -eq "EnableGroupSpecificConsent" }
    $limitedToValue = $settings.Values | ? { $_.Name -eq "ConstrainGroupSpecificConsentToMembersOfGroupId" }
    ```

1. Ayar değerlerini anlayın. Bir uygulamanın grubun verilerine erişmesine izin veren kullanıcıları tanımlayan iki ayar değeri vardır:

    | Ayar       | Tür         | Açıklama  |
    | ------------- | ------------ | ------------ |
    | _Enablegroupspecificonayı_   | Boole | Grup sahiplerinin gruba özel izinler verip vermeyeceğini belirten bayrak. |
    | _ConstrainGroupSpecificConsentToMembersOfGroupId_ | Guid | _Enablegroupspecificonay_ değeri "true" olarak ayarlanırsa ve bu değer bir grubun nesne kimliği olarak ayarlandıysa, tanımlı grubun üyelerinin sahip oldukları gruplara gruba özel izinler verme yetkisine sahip olur. |

1. İstenen yapılandırma için ayar değerlerini güncelleştir:

    ```powershell
    # Disable group-specific consent entirely
    $enabledValue.Value = "False"
    $limitedToValue.Value = ""
    ```

    ```powershell
    # Enable group-specific consent for all users
    $enabledValue.Value = "True"
    $limitedToValue.Value = ""
    ```

    ```powershell
    # Enable group-specific consent for users in a given group
    $enabledValue.Value = "True"
    $limitedToValue.Value = "{group-object-id}"
    ```

1. Ayarlarınızı kaydedin.

    ```powershell
    if ($settings.Id) {
        # Update an existing directory settings
        Set-AzureADDirectorySetting -Id $settings.Id -DirectorySetting $settings
    } else {
        # Create a new directory settings to override the default setting 
        New-AzureADDirectorySetting -DirectorySetting $settings
    }
    ```

## <a name="configure-risk-based-step-up-consent"></a>Risk tabanlı adım adım onayı yapılandırma

Risk tabanlı adım adım onayı, Kullanıcı tarafından [izin istekleri](https://docs.microsoft.com/microsoft-365/security/office-365-security/detect-and-remediate-illicit-consent-grants)yapan kötü amaçlı uygulamalara yönelik pozlandırmayı azaltmaya yardımcı olur. Microsoft riskli bir Son Kullanıcı onay isteği algılarsa, isteğin yerine yönetici onayı için "adım adım" gerekir. Bu özellik varsayılan olarak etkindir, ancak son kullanıcı onayı etkinleştirildiğinde yalnızca bir davranış değişikliğine neden olur.

Riskli onay isteği algılandığında, onay istemi yönetici onayının gerekli olduğunu belirten bir ileti görüntüler. [Yönetici onayı isteği iş akışı](configure-admin-consent-workflow.md) etkinleştirilirse, Kullanıcı doğrudan onay isteminden daha fazla gözden geçirmek için isteği bir yöneticiye gönderebilir. Etkin değilse, aşağıdaki ileti görüntülenir:

* **AADSTS90094:** &lt; clientAppDisplayName &gt; , kuruluşunuzda yalnızca bir yöneticinin verebileceği kaynaklara erişmek için izne sahip olmalıdır. Kullanabilmek için önce lütfen yöneticiden bu uygulamaya izin vermesini isteyin.

Bu durumda, bir denetim olayı "ApplicationManagement" kategorisiyle, etkinlik türü "uygulamaya onay" ve "riskli uygulama algılandı" durum nedeni ile de günlüğe kaydedilir.

> [!IMPORTANT]
> Yöneticiler, özellikle Microsoft 'un risk algıladığında, isteği onaylamadan önce [tüm izin isteklerini dikkatle değerlendirmelidir](manage-consent-requests.md#evaluating-a-request-for-tenant-wide-admin-consent) .

### <a name="disable-or-re-enable-risk-based-step-up-consent-using-powershell"></a>PowerShell kullanarak risk tabanlı adım onayını devre dışı bırakma veya yeniden etkinleştirme

Microsoft 'un riski algıladığı veya daha önce devre dışı bırakılmışsa yeniden etkinleştirebileceği durumlarda yönetici onayını gerekli hale getirebilirsiniz. Azure AD PowerShell önizleme modülünü [Azureadpreview](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0-preview)' ı kullanarak devre dışı bırakabilirsiniz.

Bunu, [PowerShell kullanarak Grup sahibi onayını yapılandırmak](#configure-group-owner-consent-using-powershell)için yukarıda gösterilen adımları kullanarak, ancak farklı bir ayar değeri yerine getirebilirsiniz. Adımlarda üç fark vardır: 

1. Risk tabanlı adım onayı için ayar değerlerini anlayın:

    | Ayar       | Tür         | Açıklama  |
    | ------------- | ------------ | ------------ |
    | _BlockUserConsentForRiskyApps_   | Boole |  Riskli bir istek algılandığında Kullanıcı izninin engellenip engellenmediğini belirten bayrak. |

1. Adım 3 ' te aşağıdaki değeri değiştirin:

    ```powershell
    $riskBasedConsentEnabledValue = $settings.Values | ? { $_.Name -eq "BlockUserConsentForRiskyApps" }
    ```
    
1. 5. adımda aşağıdakilerden birini değiştirin:

    ```powershell
    # Disable risk-based step-up consent entirely
    $riskBasedConsentEnabledValue.Value = "False"
    ```

    ```powershell
    # Re-enable risk-based step-up consent, if disabled previously
    $riskBasedConsentEnabledValue.Value = "True"
    ```

## <a name="next-steps"></a>Sonraki adımlar

Daha fazlasını öğrenin:

* [Yönetici onayı iş akışını yapılandırma](configure-admin-consent-workflow.md)
* [Uygulama onayını yönetmeyi ve izin isteklerini değerlendirmeyi öğrenin](manage-consent-requests.md)
* [Uygulamaya kiracı genelinde yönetici onayı verme](grant-admin-consent.md)
* [Microsoft Identity platformunda izinler ve onay](../develop/active-directory-v2-scopes.md)

Yardım almak veya sorularınıza yanıt bulmak için:
* [StackOverflow üzerinde Azure AD](https://stackoverflow.com/questions/tagged/azure-active-directory)
