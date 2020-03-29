---
title: Azure AD B2C (MSAL Android) | Azure
titleSuffix: Microsoft identity platform
description: Android için Microsoft Kimlik Doğrulama Kitaplığı (MSAL) ile Azure AD B2C kullanırken belirli hususlar hakkında bilgi edinin. Android)
services: active-directory
author: brianmel
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 9/18/2019
ms.author: brianmel
ms.reviewer: rapong
ms.custom: aaddev
ms.openlocfilehash: 0998bb04b0dfc69db4696f2e390cfe259eba6718
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76696530"
---
# <a name="use-msal-for-android-with-b2c"></a>B2C'li Android için MSAL kullanın

Microsoft Kimlik Doğrulama Kitaplığı (MSAL), uygulama geliştiricilerin [Azure Active Directory B2C (Azure AD B2C)](https://docs.microsoft.com/azure/active-directory-b2c/)kullanarak kullanıcıların sosyal ve yerel kimliklerini doğrulamalarını sağlar. Azure AD B2C bir kimlik yönetimi hizmetidir. Müşterilerin uygulamalarınızı kullanırken profillerini nasıl kaydolduklarını, kaydolduklarını ve yöneteceklerini özelleştirmek ve denetlemek için kullanın.

## <a name="configure-known-authorities-and-redirect-uri"></a>Bilinen yetkilileri yapılandırın ve URI'yi yeniden yönlendirin

Android için MSAL'da, B2C politikaları (kullanıcı yolculukları) bireysel otoriteler olarak yapılandırılır.

İki ilke olan bir B2C uygulaması göz önüne alındığında:
- Kayıt / Kayıt
    * Denilen`B2C_1_SISOPolicy`
- Profili Düzenle
    * Denilen`B2C_1_EditProfile`

Uygulama için yapılandırma dosyası iki `authorities`bildirir. Her poliçe için bir tane. Her `type` makamın `B2C`özelliği.

### `app/src/main/res/raw/msal_config.json`
```json
{
    "client_id": "<your_client_id_here>",
    "redirect_uri": "<your_redirect_uri_here>",
    "authorities": [{
            "type": "B2C",
            "authority_url": "https://contoso.b2clogin.com/tfp/contoso.onmicrosoft.com/B2C_1_SISOPolicy/",
            "default": true
        },
        {
            "type": "B2C",
            "authority_url": "https://contoso.b2clogin.com/tfp/contoso.onmicrosoft.com/B2C_1_EditProfile/"
        }
    ]
}
```

Uygulama `redirect_uri` yapılandırmasına ve yetkilendirme kodu hibe `AndroidManifest.xml` [akışı](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-reference-oauth-code)sırasında yeniden yönlendirmeyi desteklemek için kaydedilmesi gerekir.

## <a name="initialize-ipublicclientapplication"></a>IPublicClientApplication'ı Başlatma

`IPublicClientApplication`uygulama yapılandırmasının eşit olarak ayrıştırılmasına olanak sağlayacak bir fabrika yöntemi yle oluşturulur.

```java
PublicClientApplication.createMultipleAccountPublicClientApplication(
    context, // Your application Context
    R.raw.msal_config, // Id of app JSON config
    new IPublicClientApplication.ApplicationCreatedListener() {
        @Override
        public void onCreated(IMultipleAccountPublicClientApplication pca) {
            // Application has been initialized.
        }

        @Override
        public void onError(MsalException exception) {
            // Application could not be created.
            // Check Exception message for details.
        }
    }
);
```

## <a name="interactively-acquire-a-token"></a>Etkileşimli olarak bir belirteç edinin

MSAL ile etkileşimli olarak bir belirteç elde etmek için bir `AcquireTokenParameters` örnek oluşturun ve metoduna tedarik edin. `acquireToken` Aşağıdaki belirteç isteği `default` yetkisini kullanır.

```java
IMultipleAccountPublicClientApplication pca = ...; // Initialization not shown

AcquireTokenParameters parameters = new AcquireTokenParameters.Builder()
    .startAuthorizationFromActivity(activity)
    .withScopes(Arrays.asList("https://contoso.onmicrosoft.com/contosob2c/read")) // Provide your registered scope here
    .withPrompt(Prompt.LOGIN)
    .callback(new AuthenticationCallback() {
        @Override
        public void onSuccess(IAuthenticationResult authenticationResult) {
            // Token request was successful, inspect the result
        }

        @Override
        public void onError(MsalException exception) {
            // Token request was unsuccessful, inspect the exception
        }

        @Override
        public void onCancel() {
            // The user cancelled the flow
        }
    }).build();

pca.acquireToken(parameters);
```

## <a name="silently-renew-a-token"></a>Bir belirteci sessizce yenileme

MSAL ile sessizce bir belirteç `AcquireTokenSilentParameters` elde etmek için, bir örnek oluşturun ve yönteme tedarik edin. `acquireTokenSilentAsync` Yöntemin `acquireToken` aksine, `authority` sessizce bir belirteç elde etmek için belirtilmelidir.

```java
IMultilpeAccountPublicClientApplication pca = ...; // Initialization not shown
AcquireTokenSilentParameters parameters = new AcquireTokenSilentParameters.Builder()
    .withScopes(Arrays.asList("https://contoso.onmicrosoft.com/contosob2c/read")) // Provide your registered scope here
    .forAccount(account)
    // Select a configured authority (policy), mandatory for silent auth requests
    .fromAuthority("https://contoso.b2clogin.com/tfp/contoso.onmicrosoft.com/B2C_1_SISOPolicy/")
    .callback(new SilentAuthenticationCallback() {
        @Override
        public void onSuccess(IAuthenticationResult authenticationResult) {
            // Token request was successful, inspect the result
        }

        @Override
        public void onError(MsalException exception) {
            // Token request was unsuccesful, inspect the exception
        }
    })
    .build();

pca.acquireTokenSilentAsync(parameters);
```

## <a name="specify-a-policy"></a>Bir ilke belirtin

B2C'deki ilkeler ayrı yetkililer olarak temsil edildiğinden, varsayılandan başka bir ilke `fromAuthority` çağırmak, `acquireToken` oluşturma `acquireTokenSilent` veya parametreler de belirtilmek suretiyle bir yan tümce belirtilmek suretiyle elde edilir.  Örnek:

```java
AcquireTokenParameters parameters = new AcquireTokenParameters.Builder()
    .startAuthorizationFromActivity(activity)
    .withScopes(Arrays.asList("https://contoso.onmicrosoft.com/contosob2c/read")) // Provide your registered scope here
    .withPrompt(Prompt.LOGIN)
    .callback(...) // provide callback here
    .fromAuthority("<url_of_policy_defined_in_configuration_json>")
    .build();
```

## <a name="handle-password-change-policies"></a>Parola değişikliği ilkelerini işleme

Yerel hesap kayıt veya oturum açma kullanıcı akışı '**Şifremi unuttum?**' Bağlantı. Bu bağlantıyı tıklattığınızda otomatik olarak bir parola sıfırlama kullanıcı akışını tetiklemez.

Bunun yerine, `AADB2C90118` hata kodu uygulamanıza döndürülür. Uygulamanız, parolayı sıfırlayan belirli bir kullanıcı akışını çalıştırarak bu hata kodunu işlemelidir.

Parola sıfırlama hata kodunu yakalamak için aşağıdaki uygulama aşağıdaki `AuthenticationCallback`uygulama için kullanılabilir:

```java
new AuthenticationCallback() {

    @Override
    public void onSuccess(IAuthenticationResult authenticationResult) {
        // ..
    }

    @Override
    public void onError(MsalException exception) {
        final String B2C_PASSWORD_CHANGE = "AADB2C90118";

        if (exception.getMessage().contains(B2C_PASSWORD_CHANGE)) {
            // invoke password reset flow
        }
    }

    @Override
    public void onCancel() {
        // ..
    }
}
```

## <a name="use-iauthenticationresult"></a>IAuthenticationResult'ı kullanma

Başarılı bir belirteç `IAuthenticationResult` edinimi bir nesneyle sonuçlanır. Erişim belirteci, kullanıcı talepleri ve meta verileri içerir.

### <a name="get-the-access-token-and-related-properties"></a>Erişim jetonu ve ilgili özellikleri alın

```java
// Get the raw bearer token
String accessToken = authenticationResult.getAccessToken();

// Get the scopes included in the access token
String[] accessTokenScopes = authenticationResult.getScope();

// Gets the access token's expiry
Date expiry = authenticationResult.getExpiresOn();

// Get the tenant for which this access token was issued
String tenantId = authenticationResult.getTenantId();
```

### <a name="get-the-authorized-account"></a>Yetkili hesabı alın

```java
// Get the account from the result
IAccount account = authenticationResult.getAccount();

// Get the id of this account - note for B2C, the policy name is a part of the id
String id = account.getId();

// Get the IdToken Claims
//
// For more information about B2C token claims, see reference documentation
// https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-reference-tokens
Map<String, ?> claims = account.getClaims();

// Get the 'preferred_username' claim through a convenience function
String username = account.getUsername();

// Get the tenant id (tid) claim through a convenience function
String tenantId = account.getTenantId();
```

### <a name="idtoken-claims"></a>IdToken talepleri

IdToken'da iade edilen talepler MSAL tarafından değil, Güvenlik Belirteci Servisi (STS) tarafından doldurulur. Kullanılan kimlik sağlayıcısına (IdP) bağlı olarak, bazı talepler yok olabilir. Bazı IDP'ler şu `preferred_username` anda hak talebinde bulunmuyor. Çünkü bu talep MSAL tarafından önbelleğe almak `MISSING FROM THE TOKEN RESPONSE`için kullanılır, bir yer tutucu değeri, onun yerine kullanılır. B2C IdToken talepleri hakkında daha fazla bilgi için Azure [Etkin Dizini B2C'deki belirteçlere Genel Bakış](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-reference-tokens#claims)bölümüne bakın.

## <a name="managing-accounts-and-policies"></a>Hesapları ve ilkeleri yönetme

B2C her ilkeyi ayrı bir otorite olarak ele alar. Bu nedenle, her ilkeden döndürülen erişim belirteçleri, yenileme belirteçleri ve kimlik belirteçleri birbirinin yerine çıkarılamaz. Bu, her ilkenin belirteçleri diğer ilkeleri çağırmak için kullanılamayan ayrı `IAccount` bir nesne döndürdİğİ anlamına gelir.

Her ilke, her kullanıcı için önbelleğe bir ekspertme `IAccount` ekler. Bir kullanıcı bir uygulamayı imzalar ve iki ilke çağırırsa, iki `IAccount`s'leri olur. Bu kullanıcıyı önbellekten kaldırmak için `removeAccount()` her ilke için aramanız gerekir.

Bir iipolitikasının belirteçlerini `acquireTokenSilent`yenilediğinizde, ilkenin önceki çağrılarından döndürülen inanca `IAccount` `AcquireTokenSilentParameters`da aynı sıyrık. Başka bir ilke tarafından döndürülen bir hesap sağlamak bir hataya neden olur.

## <a name="next-steps"></a>Sonraki adımlar

Azure Active Directory B2C (Azure AD B2C) hakkında daha fazla bilgi edinin, [Azure Active Directory B2C nedir?](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-overview)
