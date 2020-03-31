---
title: Azure AD'yi kullanarak son kullanıcıların uygulamalara nasıl onay verme lerini yapılandırın
description: Kullanıcıların kuruluşunuzun verilerine erişebilecek uygulamalara nasıl ve ne zaman onay verebileceğini nasıl yöneteceğinizi öğrenin.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75443403"
---
# <a name="configure-how-end-users-consent-to-applications"></a>Son kullanıcıların uygulamalara nasıl onay verme sini yapılandırın

Uygulamalar, kullanıcıların Azure Active Directory'deki (Azure AD) iş veya okul hesaplarını kullanarak oturum açmalarına ve zengin verilere dayalı deneyimler sunmak için kuruluşunuzun verilerine erişmelerine olanak sağlamak için Microsoft Identity platformuyla tümleşebilir. Farklı izinler, uygulamanın kullanıcılarınızın ve kuruluşunuzun verilerine farklı düzeyde erişim sağlamasına olanak sağlar.

Varsayılan olarak, kullanıcılar yalnızca bazı izinler için de olsa kuruluşunuzun verilerine erişen uygulamalara izin verebilir. Örneğin, varsayılan olarak bir kullanıcı, bir uygulamanın kendi posta kutusuna veya kullanıcının sahip olduğu bir takımın Takım konuşmalarına erişmesine izin verebilir, ancak bir uygulamanın katılımsız erişiminin kuruluşunuzdaki tüm SharePoint sitelerine okuma ve yazma izni vermeyi kabul edemez. Kullanıcıların kendi rızalarına izin vermek, kullanıcıların Microsoft 365, Azure ve diğer hizmetlerle tümletkikolan yararlı uygulamaları kolayca edinmelerine olanak sağlarken, kullanılmadığı ve dikkatle izlenmediği takdirde risk teşkil edebilir.

Microsoft, yüzey alanınızı azaltmaya ve bu riski azaltmaya yardımcı olmak için gelecekteki kullanıcı onayı işlemlerini devre dışı bırakmanızı önerir. Kullanıcı onayı devre dışı bırakılırsa, önceki izin hibeleri yine de onurlandırılır, ancak gelecekteki tüm onay işlemleri bir yönetici tarafından gerçekleştirilmelidir. Kiracı çapında yönetici onayı, tümleşik bir [yönetici onayı isteği iş akışı](configure-admin-consent-workflow.md) veya kendi destek süreçleriniz aracılığıyla kullanıcılardan talep edilebilir. Daha fazla ayrıntı [için kimlik altyapınızı güvence altına almak](../../security/fundamentals/steps-secure-identity.md) için beş adıma bakın.

## <a name="configure-user-consent-to-applications"></a>Kullanıcı onaylarını uygulamalara yapılandırma
### <a name="disable-or-enable-user-consent-from-the-azure-portal"></a>Azure portalından kullanıcı onayını devre dışı kılabilir veya etkinleştirin

Azure portalını, kullanıcıların kuruluşunuzun verilerine erişen uygulamalara izin verme yeteneğini devre dışı kalmak veya etkinleştirmek için kullanabilirsiniz:

1. Azure [portalında](https://portal.azure.com) [Global Administrator](../users-groups-roles/directory-assign-admin-roles.md#global-administrator--company-administrator)olarak oturum açın.
2. **Azure Etkin Dizini'ni,** ardından **Kurumsal uygulamaları,** ardından **Kullanıcı ayarlarını**seçin.
3. Kullanıcı onayını etkinleştirin veya etiketlenmiş denetimle devre dışı **kullanabilirsiniz Kullanıcılar, şirket verilerine kendi adlarına erişen uygulamalara izin verebilir.**
4. (İsteğe bağlı) Bir [uygulamaya](configure-admin-consent-workflow.md) izin verilmeyen kullanıcıların onay isteyebilmesini sağlamak için yönetici onayı isteğini yapılandırın.

> [!TIP]
> Kullanıcıların, kullanıcının izin vermesine izin verilmeyen bir uygulamanın yönetici tarafından incelenmesini istemesine izin vermek için (örneğin, kullanıcı onayı devre dışı bırakıldığıiçin veya uygulama kullanıcının vermesine izin verilmeyen izinler istediğinden), [yönetici onayı iş akışını yapılandırmayı](configure-admin-consent-workflow.md)düşünün.

### <a name="disable-or-enable-user-consent-using-powershell"></a>PowerShell'i kullanarak kullanıcı onayLarını devre dışı kılabilir veya etkinleştirin

Azure AD PowerShell v1 modüllerini[(MSOnline)](https://docs.microsoft.com/powershell/module/msonline/?view=azureadps-1.0)kullanarak kullanıcıların kuruluşunuzun verilerine erişen uygulamalara izin verme lerini sağlayabilir veya devre dışı kullanabilirsiniz.

1. Bu cmdlet çalıştırarak kuruluşunuz için oturum açın:

    ```powershell
    Connect-MsolService
    ```

2. Bu cmdlet çalıştırılarak kullanıcı onayı etkinleştirilip etkinleştirilemeip etkinleştirilemeyince kontrol edin:

    ```powershell
    Get-MsolCompanyInformation | Format-List UsersPermissionToUserConsentToAppEnabled
    ```

3. Kullanıcı onayLarını etkinleştirin veya devre dışı edin. Örneğin, kullanıcı onayLarını devre dışı bırakmak için şu cmdlet'i çalıştırın:

    ```powershell
    Set-MsolCompanySettings -UsersPermissionToUserConsentToAppEnabled $false
    ```

## <a name="configure-group-owner-consent-to-apps-accessing-group-data"></a>Grup verilerine erişen uygulamalara grup sahibi iznini yapılandırma

> [!IMPORTANT]
> Aşağıdaki bilgiler, grup sahiplerinin uygulamalara gruplarının verilerine erişim izni vermesini sağlayacak yaklaşan bir özellik içindir. Bu özellik yayımlandığında, varsayılan olarak etkinleştirilir. Bu özellik henüz yaygın olarak yayımlanmasa da, bu yönergeleri yayımlanmasından önce özelliği devre dışı bırakmak için kullanabilirsiniz.

Grup sahipleri, kuruluşunuzun bir grupla ilişkili verilerine erişmeleri için uygulamalara (örneğin, üçüncü taraf satıcılar tarafından yayınlanan uygulamalar) yetki verebilir. Örneğin, bir takım sahibi (takım için Office 365 Grubu'nun sahibi) bir uygulamanın takımdaki tüm Takım iletilerini okumasına veya bir grubun üyelerinin temel profilini listelemesine izin verebilir.

> [!NOTE]
> Bu ayardan bağımsız olarak, bir grup sahibinin diğer kullanıcıları veya uygulamaları doğrudan grup sahibi olarak eklemesine izin verilir.

### <a name="configure-group-owner-consent-using-powershell"></a>PowerShell kullanarak grup sahibinin onay Belgesini yapılandırma

Grup sahiplerinin sahip oldukları gruplar için kuruluşunuz verilerine erişen uygulamalara izin verme lerini etkinleştirmek veya devre dışı kalmak için Azure AD PowerShell Preview modüllerini[(AzureADPreview)](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0-preview)kullanabilirsiniz.

1. [AzureADPreview](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0-preview) modüllerini kullandığınızdan emin olun (hem AzureAD modüllerini hem de [AzureADPreview](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0) modüllerini yüklediyseniz bu adım önemlidir). [AzureADPreview](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0-preview)

    ```powershell
    Remove-Module AzureAD
    Import-Module AzureADPreview
    ```

2. Azure AD PowerShell'e bağlanın.

   ```powershell
   Connect-AzureAD
   ```

3. *Kiracınızdaki İzin İlkesi Ayarları* dizininin geçerli değerini alın. Bu, bu özelliğin dizin ayarlarının oluşturulup oluşturulmadığını ve oluşturulmazsa, ilgili dizin ayarları şablonundaki değerleri kullanmayı gerektirir.

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

4. Ayar değerlerini anlayın. Hangi kullanıcıların bir uygulamanın kendi grubunun verilerine erişmesine izin verileceğini tanımlayan iki ayar değeri vardır:

    | Ayar       | Tür         | Açıklama  |
    | ------------- | ------------ | ------------ |
    | _EtkinleştirmeGroupSpecificConsent_   | Boole |  Grup sahiplerinin gruba özel izinler vermesine izin verilip verilmediğini belirten bayrak. |
    | _ConstrainGroupSpecificConsentToMembersGroupId_ | Guid | _EnableGroupSpecificConsent_ "True" olarak ayarlanırsa ve bu değer bir grubun nesne kimliğine ayarlanırsa, tanımlanan grup üyeleri sahip oldukları gruplara gruba özel izinler verme yetkisine sahip olur. |

5. İstenilen yapılandırma için ayarlar değerlerini güncelleştirme:

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

[Kiracı genelinde yönetici onayı verme](grant-admin-consent.md)

[Microsoft kimlik platformunda izinler ve onay](../develop/active-directory-v2-scopes.md)

[StackOverflow'da Azure REKLAM](https://stackoverflow.com/questions/tagged/azure-active-directory)
