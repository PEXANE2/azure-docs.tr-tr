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
ms.date: 10/22/2018
ms.author: mimart
ms.reviewer: arvindh
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5bd305d2943d1b12756171748f28d32300081d71
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75443403"
---
# <a name="configure-how-end-users-consent-to-applications"></a>Son kullanıcıların uygulamalara nasıl izin vermesini yapılandırın

Uygulamalar, kullanıcıların Azure Active Directory (Azure AD) ' deki iş veya okul hesabını kullanarak oturum açmalarına ve zengin veri odaklı deneyimler sunmak için kuruluşunuzun verilerine erişmesine olanak tanımak üzere Microsoft Identity platformu ile tümleştirilebilir. Farklı izinler, uygulamaya kullanıcılarınızın ve kuruluşunuzun verilerine farklı erişim düzeyi sağlar.

Varsayılan olarak, kullanıcılar kuruluşunuzun verilerine erişen uygulamalara yalnızca bazı izinler için izin verebilir. Örneğin, varsayılan olarak bir Kullanıcı, kullanıcının sahip olduğu bir ekip için kendi posta kutularına veya takımlar görüşlerine erişmesine izin verebilir, ancak kuruluştaki tüm SharePoint sitelerini okumak ve bunlara yazmak için bir uygulamanın katılımsız erişimine izin vermez. Kullanıcıların kendilerine izin vermesini sağlarken, kullanıcıların Microsoft 365, Azure ve diğer hizmetlerle tümleştirilen yararlı uygulamaları kolayca almasına izin verirken, bu, kullanılmıyorsa ve izlenmezse bir riski temsil edebilir.

Microsoft, Surface alanınızı azaltmaya ve bu riski azaltmaya yardımcı olmak için gelecekteki Kullanıcı onay işlemlerinin devre dışı bırakılmasını öneriyor. Kullanıcı onayı devre dışıysa, önceki onay onayları yine de kabul edilir, ancak gelecekteki tüm onay işlemleri bir yönetici tarafından gerçekleştirilmelidir. Kiracı genelinde yönetici onayı, kullanıcılar tarafından tümleşik [Yönetici onay isteği iş akışı](configure-admin-consent-workflow.md) aracılığıyla veya kendi destek süreçleriniz aracılığıyla istenebilir. Daha fazla ayrıntı için [kimlik altyapınızı güvenli hale getirmenin beş adımını](../../security/fundamentals/steps-secure-identity.md) inceleyin.

## <a name="configure-user-consent-to-applications"></a>Uygulamalar için Kullanıcı onayı yapılandırma
### <a name="disable-or-enable-user-consent-from-the-azure-portal"></a>Azure portal Kullanıcı onayını devre dışı bırakma veya etkinleştirme

Kullanıcıların kuruluşunuzun verilerine erişen uygulamalara izin verme yeteneğini devre dışı bırakmak veya etkinleştirmek için Azure portal kullanabilirsiniz:

1. [Azure Portal](https://portal.azure.com) [genel yönetici](../users-groups-roles/directory-assign-admin-roles.md#global-administrator--company-administrator)olarak oturum açın.
2. **Azure Active Directory**, ardından **Kurumsal uygulamalar**, sonra **Kullanıcı ayarları**' nı seçin.
3. **Kullanıcıları adına şirket verilerine erişen uygulamalara izin verebilmeleri için**Kullanıcı onayı 'nı etkinleştirin veya devre dışı bırakın.
4. Seçim Bir uygulamaya izin verilmeyen kullanıcıların onay isteyebilmesine olanak sağlamak için [yönetici onayı isteği iş akışını](configure-admin-consent-workflow.md) yapılandırın.

> [!TIP]
> Kullanıcıların, kullanıcının izin vermediği bir uygulamayı (örneğin, kullanıcı onayı devre dışı bırakıldığı veya uygulama kullanıcının izin vermediği izinleri istemesi nedeniyle) bir yöneticinin gözden geçirmesine izin vermek için [yönetici onayı iş akışını yapılandırmayı](configure-admin-consent-workflow.md)düşünün.

### <a name="disable-or-enable-user-consent-using-powershell"></a>PowerShell kullanarak Kullanıcı onayını devre dışı bırakma veya etkinleştirme

Kullanıcıların kuruluşunuzun verilerine erişen uygulamalara izin verme yeteneğini etkinleştirmek veya devre dışı bırakmak için Azure AD PowerShell v1 modülünü ([MSOnline](https://docs.microsoft.com/powershell/module/msonline/?view=azureadps-1.0)) kullanabilirsiniz.

1. Şu cmdlet 'i çalıştırarak kuruluşunuzda oturum açın:

    ```powershell
    Connect-MsolService
    ```

2. Bu cmdlet 'i çalıştırarak Kullanıcı izninin etkinleştirilip etkinleştirilmediğini denetleyin:

    ```powershell
    Get-MsolCompanyInformation | Format-List UsersPermissionToUserConsentToAppEnabled
    ```

3. Kullanıcı onayını etkinleştirin veya devre dışı bırakın. Örneğin, Kullanıcı onayını devre dışı bırakmak için şu cmdlet 'i çalıştırın:

    ```powershell
    Set-MsolCompanySettings -UsersPermissionToUserConsentToAppEnabled $false
    ```

## <a name="configure-group-owner-consent-to-apps-accessing-group-data"></a>Grup verilerine erişen uygulamalar için Grup sahibi onayını yapılandırma

> [!IMPORTANT]
> Aşağıdaki bilgiler, Grup sahiplerinin uygulamalara ait verilerine erişim izni vermesini sağlayacak gelecek bir özellik içindir. Bu yetenek serbest bırakıldığında varsayılan olarak etkinleştirilir. Bu özellik henüz yaygın olarak yayınlanmasa da, bu yönergeleri kullanarak sürümünün sürümü için özelliği devre dışı bırakabilirsiniz.

Grup sahipleri, kuruluşunuzun bir grupla ilişkili verilerinize erişebilmesi için uygulamaları (örneğin, üçüncü taraf satıcılar tarafından yayımlanan uygulamalar) yetkilendirebilirler. Örneğin, takım sahibi (ekip için Office 365 grubunun sahibi olan), bir uygulamanın ekipteki tüm ekip iletilerini okumasına veya bir grubun üyelerinin temel profilini listeliğine izin verebilir.

> [!NOTE]
> Bu ayardan bağımsız olarak, Grup sahibinin her zaman başka Kullanıcı veya uygulamaları doğrudan Grup sahipleri olarak eklemesine izin verilir.

### <a name="configure-group-owner-consent-using-powershell"></a>PowerShell kullanarak Grup sahibi onayını yapılandırma

Grup sahiplerini, sahip oldukları gruplar için kuruluşunuzun verilerine erişen uygulamalara izin verme özelliğini etkinleştirmek veya devre dışı bırakmak için Azure AD PowerShell önizleme modülünü ([Azureadpreview](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0-preview)) kullanabilirsiniz.

1. [Azureadpreview](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0-preview) modülünü kullandığınızdan emin olun (hem [azuread](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0) modülünü hem de [azureadpreview](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0-preview) modülünü yüklediyseniz bu adım önemlidir).

    ```powershell
    Remove-Module AzureAD
    Import-Module AzureADPreview
    ```

2. Azure AD PowerShell 'e bağlanın.

   ```powershell
   Connect-AzureAD
   ```

3. Kiracınızdaki *Izin Ilkesi ayarları* dizin ayarları için geçerli değeri alın. Bunun için, bu özelliğin Dizin ayarlarının oluşturulup oluşturulmadığından ve ilgili dizin ayarları şablonundaki değerler kullanılarak denetlenmesi gerekir.

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

4. Ayar değerlerini anlayın. Bir uygulamanın grubun verilerine erişmesine izin veren kullanıcıları tanımlayan iki ayar değeri vardır:

    | Ayar       | Tür         | Açıklama  |
    | ------------- | ------------ | ------------ |
    | _Enablegroupspecificonayı_   | Boole |  Grup sahiplerinin gruba özel izinler verip vermeyeceğini belirten bayrak. |
    | _ConstrainGroupSpecificConsentToMembersOfGroupId_ | Guid | _Enablegroupspecificonay_ değeri "true" olarak ayarlanırsa ve bu değer bir grubun nesne kimliği olarak ayarlandıysa, tanımlanan grubun üyelerinin sahip oldukları gruplara gruba özel izinler verme yetkisine sahip olur. |

5. İstenen yapılandırma için ayar değerlerini güncelleştir:

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

6. Ayarları kaydedin.

    ```powershell
    if ($settings.Id) {
        # Update an existing directory settings
        Set-AzureADDirectorySetting -Id $settings.Id -DirectorySetting $settings
    } else {
        # Create a new directory settings to override the default setting 
        New-AzureADDirectorySetting -DirectorySetting $settings
    }
    ```

## <a name="next-steps"></a>Sonraki adımlar

[Yönetici onayı iş akışını yapılandırma](configure-admin-consent-workflow.md)

[Bir uygulamaya kiracı genelinde yönetici onayı verme](grant-admin-consent.md)

[Microsoft Identity platformunda izinler ve onay](../develop/active-directory-v2-scopes.md)

[StackOverflow üzerinde Azure AD](https://stackoverflow.com/questions/tagged/azure-active-directory)
