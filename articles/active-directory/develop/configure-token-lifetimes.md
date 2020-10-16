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
ms.date: 09/29/2020
ms.author: ryanwi
ms.custom: aaddev, content-perf, FY21Q1
ms.reviewer: hirsin, jlu, annaba
ms.openlocfilehash: f70a11165f6433e580fd857f2d5a620deb6640c0
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91604343"
---
# <a name="configure-token-lifetime-policies-preview"></a>Belirteç ömür ilkelerini yapılandırma (Önizleme)
Uygulamalar, hizmet sorumluları ve genel kuruluşunuz için belirteç yaşam sürelerini oluşturup yönetebilmeniz için Azure AD 'de birçok senaryo mümkündür. Daha fazla bilgi edinmek için [Microsoft Identity platformunda yapılandırılabilir belirteç yaşam sürelerini](active-directory-configurable-token-lifetimes.md)okuyun. 

> [!IMPORTANT]
> Önizleme sırasında müşterilerden bir duyduktan sonra Azure AD koşullu erişim 'de [kimlik doğrulama oturumu yönetimi özelliklerini](../conditional-access/howto-conditional-access-session-lifetime.md) uyguladık. Bu yeni özelliği, oturum açma sıklığını ayarlayarak yenileme belirteci yaşam sürelerini yapılandırmak için kullanabilirsiniz. 30 Mayıs 2020 ' den sonra, yeni bir kiracı oturumu yapılandırmak ve belirteçleri yenilemek için yapılandırılabilir belirteç ömür ilkesi kullanamaz. Kullanımdan kaldırma, bu tarihten sonra birkaç ay içinde gerçekleşecektir. Bu, mevcut oturum ve belirteçleri yenileme belirteçleri ilkelerini durdurduğumuz anlamına gelir. Kullanımdan kaldırıldıktan sonra erişim belirteci yaşam sürelerini yapılandırabilirsiniz.


Bu bölümde, için yeni kurallar sungetirmenize yardımcı olabilecek birkaç ortak ilke senaryosundan ilerliyoruz:

* Belirteç ömrü
* Belirteç en fazla etkin değil süresi
* En fazla belirteç yaşı

Örneklerde, şunları nasıl yapacağınızı öğrenebilirsiniz:

* Kuruluşun varsayılan ilkesini yönetme
* Web 'de oturum açma ilkesi oluşturma
* Web API 'sini çağıran yerel uygulama için bir ilke oluşturma
* Gelişmiş bir ilkeyi yönetme

## <a name="prerequisites"></a>Ön koşullar
Aşağıdaki örneklerde uygulamalar, hizmet sorumluları ve genel kurumunuzun ilkelerini oluşturur, güncelleştirir, bağlar ve silebilirsiniz. Azure AD 'de yeni başladıysanız, bu örneklere geçmeden önce [bir Azure AD kiracısı alma](quickstart-create-new-tenant.md) hakkında bilgi almanızı öneririz.  

Başlamak için aşağıdaki adımları uygulayın:

1. En son [Azure AD PowerShell modülü genel önizleme sürümünü](https://www.powershellgallery.com/packages/AzureADPreview)indirin.
2. `Connect`Azure AD yönetici hesabınızda oturum açmak için komutunu çalıştırın. Her yeni oturumu başlattığınızda bu komutu çalıştırın.

    ```powershell
    Connect-AzureAD -Confirm
    ```

3. Kuruluşunuzda oluşturulan tüm ilkeleri görmek için aşağıdaki komutu çalıştırın. Aşağıdaki senaryolarda işlemlerden en çok sonra bu komutu çalıştırın. Komutu çalıştırmak, ilkeleriniz için * * * * * * * * * * * * * *.

    ```powershell
    Get-AzureADPolicy
    ```

## <a name="manage-an-organizations-default-policy"></a>Kuruluşun varsayılan ilkesini yönetme
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

## <a name="create-a-policy-for-a-native-app-that-calls-a-web-api"></a>Web API 'sini çağıran yerel uygulama için bir ilke oluşturma
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

## <a name="manage-an-advanced-policy"></a>Gelişmiş bir ilkeyi yönetme
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