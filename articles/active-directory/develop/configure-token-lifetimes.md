---
title: Belirteçler için yaşam sürelerini ayarlama
titleSuffix: Microsoft identity platform
description: Microsoft Identity platform tarafından verilen belirteçler için yaşam sürelerini ayarlamayı öğrenin. Bir kuruluşun varsayılan ilkesini yönetmeyi, Web 'de oturum açma ilkesi oluşturmayı, bir Web API 'SI çağıran yerel uygulama için ilke oluşturmayı ve gelişmiş bir ilkeyi yönetmeyi öğrenin.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.topic: how-to
ms.date: 01/04/2021
ms.author: ryanwi
ms.custom: aaddev, content-perf, FY21Q1
ms.reviewer: hirsin, jlu, annaba
ms.openlocfilehash: 883a06bdffcd0afcbc1be6f2c761d6a1c2c2ea2a
ms.sourcegitcommit: b39cf769ce8e2eb7ea74cfdac6759a17a048b331
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/22/2021
ms.locfileid: "98681884"
---
# <a name="configure-token-lifetime-policies-preview"></a>Belirteç ömür ilkelerini yapılandırma (Önizleme)
Microsoft Identity platform tarafından verilen erişim, SAML veya KIMLIK belirtecinin ömrünü belirtebilirsiniz. Kuruluşunuzdaki tüm uygulamalar, çok kiracılı (çok kuruluşlu) bir uygulama veya belirli bir hizmet sorumlusu için belirteç ömrünü ayarlayabilirsiniz. Daha fazla bilgi için, [yapılandırılabilir belirteç yaşam sürelerini](active-directory-configurable-token-lifetimes.md)okuyun.

Bu bölümde, belirteç ömrü için yeni kurallar almanıza yardımcı olabilecek ortak bir ilke senaryosuna kılavuzluk ederiz. Örnekte, kullanıcıların Web uygulamanızda daha sık kimlik doğrulaması yapmasını gerektiren bir ilke oluşturmayı öğreneceksiniz.

## <a name="get-started"></a>başlarken
Başlamak için aşağıdaki adımları uygulayın:

1. En son [Azure AD PowerShell modülü genel önizleme sürümünü](https://www.powershellgallery.com/packages/AzureADPreview)indirin.
1. `Connect`Azure AD yönetici hesabınızda oturum açmak için komutunu çalıştırın. Her yeni oturumu başlattığınızda bu komutu çalıştırın.

    ```powershell
    Connect-AzureAD -Confirm
    ```

1. Kuruluşunuzda oluşturulan tüm ilkeleri görmek için [Get-AzureADPolicy](/powershell/module/azuread/get-azureadpolicy?view=azureadps-2.0-preview&preserve-view=true) cmdlet 'ini çalıştırın.  Yukarıda listelenen varsayılandan farklı olarak tanımlanmış özellik değerleri olan sonuçlar, emeklilik kapsamıdır.

    ```powershell
    Get-AzureADPolicy -All
    ```

1. Belirlediğiniz belirli bir ilkeye bağlı olan uygulamaları ve hizmet sorumlularını görmek için, aşağıdaki [Get-AzureADPolicyAppliedObject](/powershell/module/azuread/get-azureadpolicyappliedobject?view=azureadps-2.0-preview&preserve-view=true) cmdlet 'ini çalıştırarak **1a37dad8-5dad7-4cc8-87c7-efbc0326cf20** ' i ilke kimliklerinizle değiştirin. Daha sonra koşullu erişim oturum açma sıklığını yapılandırıp yapılandırmamaya veya Azure AD varsayılanları ile kalmaya karar verebilirsiniz.

    ```powershell
    Get-AzureADPolicyAppliedObject -id 1a37dad8-5da7-4cc8-87c7-efbc0326cf20
    ```

Kiracınızda yenileme ve oturum belirteci yapılandırma özellikleri için özel değerler tanımlayan ilkeler varsa, Microsoft bu ilkeleri yukarıda belirtilen Varsayılanları yansıtan değerlere güncelleştirmenizi önerir. Hiçbir değişiklik yapılgerekmediğinden, Azure AD varsayılan değerleri otomatik olarak kabul eder.

## <a name="create-a-policy-for-web-sign-in"></a>Web 'de oturum açma ilkesi oluşturma

Bu örnekte, kullanıcıların Web uygulamanızda daha sık kimlik doğrulaması yapmasını gerektiren bir ilke oluşturacaksınız. Bu ilke, erişim/KIMLIK belirteçlerinin ömrünü ve çok faktörlü bir oturum belirtecinin en fazla yaşını Web uygulamanızın hizmet sorumlusuna ayarlar.

1. Belirteç ömür ilkesi oluşturun.

    Bu ilke, Web 'de oturum açmak için erişim/KIMLIK belirteci ömrünü ve en fazla tek faktörlü oturum belirteci yaşını iki saate ayarlar.

    1. İlkeyi oluşturmak için [New-AzureADPolicy](/powershell/module/azuread/new-azureadpolicy?view=azureadps-2.0-preview&preserve-view=true) cmdlet 'ini çalıştırın:

        ```powershell
        $policy = New-AzureADPolicy -Definition @('{"TokenLifetimePolicy":{"Version":1,"AccessTokenLifetime":"02:00:00","MaxAgeSessionSingleFactor":"02:00:00"}}') -DisplayName "WebPolicyScenario" -IsOrganizationDefault $false -Type "TokenLifetimePolicy"
        ```

    1. Yeni ilkenize bakmak ve ilke **ObjectID**'yi almak için [Get-azureadpolicy](/powershell/module/azuread/get-azureadpolicy?view=azureadps-2.0-preview&preserve-view=true) cmdlet 'ini çalıştırın:

        ```powershell
        Get-AzureADPolicy -Id $policy.Id
        ```

1. İlkeyi hizmet sorumlusuna atayın. Ayrıca hizmet sorumlunun **ObjectID** 'sini almanız gerekir.

    1. Tüm kuruluşunuzun hizmet sorumlularını veya tek bir hizmet sorumlusunu görmek için [Get-AzureADServicePrincipal](/powershell/module/azuread/get-azureadserviceprincipal) cmdlet 'ini kullanın.
        ```powershell
        # Get ID of the service principal
        $sp = Get-AzureADServicePrincipal -Filter "DisplayName eq '<service principal display name>'"
        ```

    1. Hizmet sorumlusu varsa [Add-Azureadservicesprincipalpolicy](/powershell/module/azuread/add-azureadserviceprincipalpolicy?view=azureadps-2.0-preview&preserve-view=true) cmdlet 'ini çalıştırın:
        ```powershell
        # Assign policy to a service principal
        Add-AzureADServicePrincipalPolicy -Id $sp.ObjectId -RefObjectId $policy.Id
        ```

## <a name="create-token-lifetime-policies-for-refresh-and-session-tokens"></a>Yenileme ve oturum belirteçleri için belirteç ömür ilkeleri oluşturma
> [!IMPORTANT]
> Mayıs 2020 itibariyle yeni kiracılar yenileme ve oturum belirteci yaşam sürelerini yapılandıramaz.  Mevcut yapılandırmaya sahip kiracılar, 30 Ocak 2021 ' ye kadar yenileme ve oturum belirteci ilkelerini değiştirebilir.  Azure Active Directory, 30 Ocak 2021 ' den sonra ilkelerde bulunan mevcut yenileme ve oturum belirteci yapılandırmasını durdurur. Kullanımdan sonra erişimi, SAML ve KIMLIK belirteci yaşam sürelerini yapılandırabilirsiniz.
>
> Bir kullanıcının yeniden oturum açması istenmeden önce geçen süreyi tanımlamaya devam etmeniz gerekiyorsa, koşullu erişimde oturum açma sıklığını yapılandırın. Koşullu erişim hakkında daha fazla bilgi edinmek için [koşullu erişimle kimlik doğrulama oturumu yönetimini yapılandırma](../conditional-access/howto-conditional-access-session-lifetime.md)makalesini okuyun.
>
> Kullanımdan kaldırma tarihinden sonra koşullu erişim kullanmak istemiyorsanız, yenileme ve oturum belirteçleriniz bu tarih için [varsayılan yapılandırmaya](active-directory-configurable-token-lifetimes.md#configurable-token-lifetime-properties-after-the-retirement) ayarlanır ve artık yaşam sürelerini değiştiremeyeceksiniz.

### <a name="manage-an-organizations-default-policy"></a>Kuruluşun varsayılan ilkesini yönetme
Bu örnekte, kullanıcılarınızın kuruluşunuzun tamamında daha az sıklıkta oturum açmasını sağlayan bir ilke oluşturacaksınız. Bunu yapmak için, kuruluşunuz genelinde uygulanan tek faktörlü yenileme belirteçleri için bir belirteç ömür ilkesi oluşturun. İlke, kuruluşunuzdaki her uygulamaya ve zaten bir ilke kümesi olmayan her bir hizmet sorumlusuna uygulanır.

1. Belirteç ömür ilkesi oluşturun.

    1. Tek Etmenli yenileme belirtecini "Until-iptal" olarak ayarlayın. Erişim iptal edilene kadar belirtecin kullanım süreleri dolmaz. Aşağıdaki ilke tanımını oluşturun:

        ```powershell
        @('{
            "TokenLifetimePolicy":
            {
                "Version":1,
                "MaxAgeSingleFactor":"until-revoked"
            }
        }')
        ```

    1. İlkeyi oluşturmak için [New-AzureADPolicy](/powershell/module/azuread/new-azureadpolicy?view=azureadps-2.0-preview&preserve-view=true) cmdlet 'ini çalıştırın:

        ```powershell
        $policy = New-AzureADPolicy -Definition @('{"TokenLifetimePolicy":{"Version":1, "MaxAgeSingleFactor":"until-revoked"}}') -DisplayName "OrganizationDefaultPolicyScenario" -IsOrganizationDefault $true -Type "TokenLifetimePolicy"
        ```

    1. Herhangi bir boşluğu kaldırmak için [Get-AzureADPolicy](/powershell/module/azuread/get-azureadpolicy?view=azureadps-2.0-preview&preserve-view=true) cmdlet 'ini çalıştırın:

        ```powershell
        Get-AzureADPolicy -id | set-azureadpolicy -Definition @($((Get-AzureADPolicy -id ).Replace(" ","")))
        ```

    1. Yeni ilkenizi görmek ve ilkenin **ObjectID**'sini almak için aşağıdaki komutu çalıştırın:

        ```powershell
        Get-AzureADPolicy -Id $policy.Id
        ```

1. İlkeyi güncelleştirin.

    Bu örnekte ayarladığınız ilk ilkenin, hizmetiniz için gerekli olan katı olmadığından emin olabilirsiniz. Tek faktörlü yenileme belirtecinizi iki gün içinde dolacak şekilde ayarlamak için aşağıdaki komutu çalıştırın:

    ```powershell
    Set-AzureADPolicy -Id $policy.Id -DisplayName $policy.DisplayName -Definition @('{"TokenLifetimePolicy":{"Version":1,"MaxAgeSingleFactor":"2.00:00:00"}}')
    ```

### <a name="create-a-policy-for-a-native-app-that-calls-a-web-api"></a>Web API 'sini çağıran yerel uygulama için bir ilke oluşturma
Bu örnekte, kullanıcılardan daha az sıklıkta kimlik doğrulaması yapmasını gerektiren bir ilke oluşturursunuz. İlke, Kullanıcı yeniden kimlik doğrulaması yapmadan önce kullanıcının devre dışı kalabileceği süreyi de uzar. İlke, Web API 'sine uygulanır. Yerel uygulama, Web API 'sini bir kaynak olarak istediğinde, bu ilke uygulanır.

1. Belirteç ömür ilkesi oluşturun.

    1. Bir Web API 'SI için katı bir ilke oluşturmak için [New-AzureADPolicy](/powershell/module/azuread/new-azureadpolicy?view=azureadps-2.0-preview&preserve-view=true) cmdlet 'ini çalıştırın:

        ```powershell
        $policy = New-AzureADPolicy -Definition @('{"TokenLifetimePolicy":{"Version":1,"MaxInactiveTime":"30.00:00:00","MaxAgeMultiFactor":"until-revoked","MaxAgeSingleFactor":"180.00:00:00"}}') -DisplayName "WebApiDefaultPolicyScenario" -IsOrganizationDefault $false -Type "TokenLifetimePolicy"
        ```

    1. Yeni ilkenizi görmek için şu komutu çalıştırın:

        ```powershell
        Get-AzureADPolicy -Id $policy.Id
        ```

1. İlkeyi Web API 'nize atayın. Ayrıca uygulamanızın **ObjectID** 'sini almanız gerekir. Uygulamanızın **ObjectID**'sini bulmak için [Get-azureadapplication](/powershell/module/azuread/get-azureadapplication) cmdlet 'ini kullanın veya [Azure Portal](https://portal.azure.com/)kullanın.

    Uygulamanızın **ObjectID** 'sini alın ve ilkeyi atayın:

    ```powershell
    # Get the application
    $app = Get-AzureADApplication -Filter "DisplayName eq 'Fourth Coffee Web API'"

    # Assign the policy to your web API.
    Add-AzureADApplicationPolicy -Id $app.ObjectId -RefObjectId $policy.Id
    ```

### <a name="manage-an-advanced-policy"></a>Gelişmiş bir ilkeyi yönetme
Bu örnekte, öncelik sisteminin nasıl çalıştığını öğrenmek için birkaç ilke oluşturursunuz. Ayrıca, çeşitli nesnelere uygulanan birden çok ilkeyi yönetmeyi öğreneceksiniz.

1. Belirteç ömür ilkesi oluşturun.

    1. Tek faktörlü yenileme belirteci ömrünü 30 güne ayarlayan bir kuruluş varsayılan ilkesi oluşturmak için [New-AzureADPolicy](/powershell/module/azuread/new-azureadpolicy?view=azureadps-2.0-preview&preserve-view=true) cmdlet 'ini çalıştırın:

        ```powershell
        $policy = New-AzureADPolicy -Definition @('{"TokenLifetimePolicy":{"Version":1,"MaxAgeSingleFactor":"30.00:00:00"}}') -DisplayName "ComplexPolicyScenario" -IsOrganizationDefault $true -Type "TokenLifetimePolicy"
        ```

    1. Yeni ilkenizi görmek için [Get-AzureADPolicy](/powershell/module/azuread/get-azureadpolicy?view=azureadps-2.0-preview&preserve-view=true) cmdlet 'ini çalıştırın:

        ```powershell
        Get-AzureADPolicy -Id $policy.Id
        ```

1. İlkeyi bir hizmet sorumlusuna atayın.

    Şimdi, tüm kuruluşa uygulanan bir ilkenize sahipsiniz. Belirli bir hizmet sorumlusu için bu 30 günlük ilkeyi korumak isteyebilirsiniz, ancak kuruluş varsayılan ilkesini "Until-iptal" olarak üst sınıra göre değiştirebilirsiniz.

    1. Tüm kuruluşunuzun hizmet sorumlularını görmek için [Get-AzureADServicePrincipal](/powershell/module/azuread/get-azureadserviceprincipal) cmdlet 'ini kullanırsınız.

    1. Hizmet sorumlusu varsa [Add-Azureadservicesprincipalpolicy](/powershell/module/azuread/add-azureadserviceprincipalpolicy?view=azureadps-2.0-preview&preserve-view=true) cmdlet 'ini çalıştırın:

        ```powershell
        # Get ID of the service principal
        $sp = Get-AzureADServicePrincipal -Filter "DisplayName eq '<service principal display name>'"

        # Assign policy to a service principal
        Add-AzureADServicePrincipalPolicy -Id $sp.ObjectId -RefObjectId $policy.Id
        ```

1. `IsOrganizationDefault`Bayrağı false olarak ayarlayın:

    ```powershell
    Set-AzureADPolicy -Id $policy.Id -DisplayName "ComplexPolicyScenario" -IsOrganizationDefault $false
    ```

1. Yeni bir kuruluş varsayılan ilkesi oluştur:

    ```powershell
    New-AzureADPolicy -Definition @('{"TokenLifetimePolicy":{"Version":1,"MaxAgeSingleFactor":"until-revoked"}}') -DisplayName "ComplexPolicyScenarioTwo" -IsOrganizationDefault $true -Type "TokenLifetimePolicy"
    ```

    Artık özgün ilkeyi hizmet sorumlusuna bağladınız ve yeni ilke, kuruluşunuzun varsayılan ilkesi olarak ayarlanmıştır. Hizmet sorumlularına uygulanan ilkelerin, kuruluş varsayılan ilkelerine göre öncelikli olduğunu unutmamak önemlidir.

## <a name="next-steps"></a>Sonraki adımlar
Azure AD koşullu erişim 'de [kimlik doğrulama oturumu yönetimi özellikleri](../conditional-access/howto-conditional-access-session-lifetime.md) hakkında bilgi edinin.