---
title: ADAL-MSAL geçiş kılavuzu (MSAL4j) | Azure
titleSuffix: Microsoft identity platform
description: Azure Active Directory Authentication Library (ADAL) Java uygulamanızı Microsoft Kimlik Doğrulama Kitaplığı'na (MSAL) nasıl geçirebilirsiniz öğrenin.
services: active-directory
author: sangonzal
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.tgt_pltfrm: Java
ms.workload: identity
ms.date: 11/04/2019
ms.author: sagonzal
ms.reviewer: nacanuma, twhitney
ms.custom: aaddev
ms.openlocfilehash: 2929b94a2cb624b96649292714fe93dea09a2085
ms.sourcegitcommit: d187fe0143d7dbaf8d775150453bd3c188087411
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80886509"
---
# <a name="adal-to-msal-migration-guide-for-java"></a>Java için ADAL -MSAL geçiş kılavuzu

Bu makalede, Microsoft Kimlik Doğrulama Kitaplığını (MSAL) kullanmak için Azure Etkin Dizin Kimlik Doğrulama Kitaplığı 'nı (ADAL) kullanan bir uygulamayı geçirmek için yapmanız gereken değişiklikler vurgulanır.

Hem Java için Microsoft Kimlik Doğrulama Kitaplığı (MSAL4J) hem de Java için Azure AD Kimlik Doğrulama Kitaplığı (ADAL4J), Azure AD varlıklarının kimliğini doğrulamak ve Azure AD'den belirteç istemek için kullanılır. Şimdiye kadar çoğu geliştirici, Azure AD Kimlik Doğrulama Kitaplığı 'nı (ADAL) kullanarak belirteçler isteyerek Azure AD kimliklerinin (iş ve okul hesapları) kimliğini doğrulamak için geliştiriciler platformu (v1.0) için Azure AD ile çalışmıştır.

MSAL aşağıdaki avantajları sunar:

- Yeni Microsoft kimlik platformu bitiş noktasını kullandığından, Azure AD kimlikleri, Microsoft hesapları ve azure AD İş'i (B2C) aracılığıyla daha geniş bir Microsoft kimlik kümesini doğrulayabilirsiniz.
- Kullanıcılarınız en iyi tek oturum açma deneyimini yaşar.
- Uygulamanız artımlı onay sağlayabilir ve koşullu erişimi desteklemek daha kolaydır.

Java için MSAL, Microsoft kimlik platformuyla kullanmanızı tavsiye ettiğimiz auth kitaplığıdır. ADAL4J'de yeni özellikler uygulanmayacaktır. İleriye dönük tüm çabalar MSAL'ın iyileştirilmesine odaklanmıştır.

## <a name="differences"></a>Farklılıklar

Geliştiriciler için Azure AD (v1.0) bitiş noktası (ve ADAL4J) ile çalışıyorsanız, [Microsoft kimlik platformu (v2.0) bitiş noktası hakkında farklı ne](https://docs.microsoft.com/azure/active-directory/develop/azure-ad-endpoint-comparison)okumak isteyebilirsiniz?

## <a name="scopes-not-resources"></a>Kapsamlar kaynak değil

ADAL4J kaynaklar için belirteçler satın alarken, Java için MSAL kapsamlar için belirteçler edinir. Java sınıfları için bir dizi MSAL kapsam parametresi gerektirir. Bu parametre, istenen izinleri ve istenen kaynakları bildiren dizelerin listesidir. Örnek kapsamları görmek için [Microsoft Graph'ın kapsamlarına](https://docs.microsoft.com/graph/permissions-reference) bakın.

## <a name="core-classes"></a>Çekirdek sınıflar

ADAL4J'de `AuthenticationContext` sınıf, bir Kurum aracılığıyla Güvenlik Belirteci Hizmeti (STS) veya yetkilendirme sunucusuyla olan bağlantınızı temsil eder. Ancak, Java için MSAL istemci uygulamaları etrafında tasarlanmıştır. İki ayrı sınıf `PublicClientApplication` sağlar: ve `ConfidentialClientApplication` istemci uygulamalarını temsil eder.  İkincisi, `ConfidentialClientApplication`, güvenli bir daemon uygulaması için bir uygulama tanımlayıcısı gibi bir sırrı korumak için tasarlanmış bir uygulamayı temsil eder.

Aşağıdaki tablo, ADAL4J işlevlerinin Java işlevleri için yeni MSAL ile nasıl eşleşiş gösterdiğini gösterir:

| ADAL4J yöntemi| MSAL4J yöntemi|
|------|-------|
|edinme Token(String kaynak, ClientCredential kimlik bilgileri, AuthenticationCallback geri arama) | edinmeToken(ClientCredentialParameters)|
|edinme Token(String kaynak, ClientAssertion iddiası, AuthenticationCallback geri arama)|edinmeToken(ClientCredentialParameters)|
|edinme Token(String kaynak, AsimetrikKeyCredential kimlik bilgileri, AuthenticationCallback geri arama)|edinmeToken(ClientCredentialParameters)|
|edinmeToken(String kaynak, String clientId, String kullanıcı adı, String şifre, AuthenticationCallback geri arama)| edinmeToken(Kullanıcı adıPasswordParameters)|
|acquireToken(String kaynak, String clientId, String kullanıcı adı, String password=null, AuthenticationCallback geri arama)|edinmeToken(IntegratedWindowsAuthenticationParameters)|
|acquireToken(String kaynak, UserAssertion userAssertion, ClientCredential kimlik bilgisi, AuthenticationCallback geri arama)| satın almaToken(OnBehalfOfParametreleri)|
|satın almaTokenByAuthorizationCode() | edinmeToken(AuthorizationCodeParameters) |
| satınDeviceCode() ve satın AlmaTokenByDeviceCode()| edinmeToken(DeviceCodeParameters)|
|satın almaTokenByRefreshToken()| satın AlmaTokenSilently(SilentParameters)|

## <a name="iaccount-instead-of-iuser"></a>IUser yerine IAccount

ADAL4J kullanıcıları manipüle etti. Bir kullanıcı tek bir insanı veya yazılım aracısını temsil etse de, Microsoft kimlik sisteminde bir veya daha fazla hesabı olabilir. Örneğin, bir kullanıcının birkaç Azure REKLAMı, Azure AD B2C veya Microsoft kişisel hesabı olabilir.

Java için `IAccount` MSAL, Hesap kavramını arayüz üzerinden tanımlar. Bu, ADAL4J'den bir kırılma değişikliğidir, ancak aynı kullanıcının birden fazla hesaba ve hatta belki de farklı Azure REKLAM dizinlerinde birden fazla hesaba sahip olabileceği gerçeğini yakaladığından iyi bir değişikliktir. Java için MSAL, ev hesabı bilgileri sağlandığı için konuk senaryolarında daha iyi bilgi sağlar.

## <a name="cache-persistence"></a>Önbellek kalıcılığı

ADAL4J belirteç önbelleği için destek yoktu.
Java için MSAL, süresi dolmuş belirteçleri mümkün olduğunda otomatik olarak yenileyerek ve kullanıcının mümkün olduğunda kimlik bilgilerini sağlaması için gereksiz istemleri önleyerek belirteç kullanım ömürlerini yönetmeyi basitleştirmek için bir [belirteç önbelleği](msal-acquire-cache-tokens.md) ekler.

## <a name="common-authority"></a>Ortak Otorite

V1.0'da, yetkiyi `https://login.microsoftonline.com/common` kullanırsanız, kullanıcılar herhangi bir Azure Active Directory (AAD) hesabıyla (herhangi bir kuruluş için) oturum açabilir.

Yetkiyi `https://login.microsoftonline.com/common` v2.0'da kullanırsanız, kullanıcılar herhangi bir AAD kuruluşuyla, hatta microsoft kişisel hesabıyla (MSA) oturum açabilir. Java için MSAL'da, herhangi bir AAD hesabına girişi kısıtlamak `https://login.microsoftonline.com/organizations` istiyorsanız, yetkiyi kullanmanız gerekir (ADAL4J ile aynı davranıştır). Bir yetkisini belirtmek `authority` için, sınıfınızı `PublicClientApplication` oluştururken [PublicClientApplication.Builder](https://javadoc.io/doc/com.microsoft.azure/msal4j/1.0.0/com/microsoft/aad/msal4j/PublicClientApplication.Builder.html) yöntemindeki parametreyi ayarlayın.

## <a name="v10-and-v20-tokens"></a>v1.0 ve v2.0 belirteçleri

V1.0 uç noktası (ADAL tarafından kullanılan) sadece v1.0 belirteçleri yayan.

V2.0 uç noktası (MSAL tarafından kullanılan) v1.0 ve v2.0 belirteçleri yatabilir. Web API'nin uygulama bildiriminin bir özelliği, geliştiricilerin belirteç tenhangi sürümün kabul edildiğini seçmelerine olanak tanır. Başvuru `accessTokenAcceptedVersion` [bildirimi](https://docs.microsoft.com/azure/active-directory/develop/reference-app-manifest) başvuru belgelerine bakın.

v1.0 ve v2.0 belirteçleri hakkında daha fazla bilgi için [Azure Active Directory erişim belirteçleri](https://docs.microsoft.com/azure/active-directory/develop/access-tokens)bakın.

## <a name="adal-to-msal-migration"></a>ADAL'dan MSAL göçüne

ADAL4J'de, yenileme belirteçleri açığa çıkarıldı ve bu da geliştiricilerin bunları önbelleğe almasına olanak sağladı. Daha sonra, `AcquireTokenByRefreshToken()` kullanıcı artık bağlı olmadığında, kullanıcı adına panoları yenileyen uzun süreli hizmetleri uygulama gibi çözümleri etkinleştirmek için kullanırlar.

Java için MSAL, güvenlik nedenleriyle yenileme belirteçlerini ortaya çıkarmaz. Bunun yerine, MSAL sizin için ferahlatıcı belirteçleri işler.

Java için MSAL, ADAL4j ile edindiğiniz yenileme belirteçlerini Müşteri Uygulamasına geçirmenize olanak tanıyan bir API'ye sahiptir: [satın AlmaToken(RefreshTokenParameters)](https://javadoc.io/static/com.microsoft.azure/msal4j/1.0.0/com/microsoft/aad/msal4j/PublicClientApplication.html#acquireToken-com.microsoft.aad.msal4j.RefreshTokenParameters-). Bu yöntemle, daha önce kullanılan yenileme belirteci ile birlikte istediğiniz kapsamları (kaynaklar) sağlayabilirsiniz. Yenileme belirteci yeni bir belirteç için değiştirilir ve uygulamanız tarafından kullanılmak üzere önbelleğe alınacaktır.

Aşağıdaki kod snippet gizli bir istemci uygulamasında bazı geçiş kodu gösterir:

```java
String rt = GetCachedRefreshTokenForSIgnedInUser(); // Get refresh token from where you have them stored
Set<String> scopes = Collections.singleton("SCOPE_FOR_REFRESH_TOKEN");

RefreshTokenParameters parameters = RefreshTokenParameters.builder(scopes, rt).build();

PublicClientApplication app = PublicClientApplication.builder(CLIENT_ID) // ClientId for your application
                .authority(AUTHORITY)  //plug in your authority
                .build();

IAuthenticationResult result = app.acquireToken(parameters);
```

Yeni `IAuthenticationResult` yenileme belirteciniz önbellekte depolanırken, erişim belirteci ve kimlik belirteci döndürür. Uygulama da şimdi bir IAccount içerecektir:

```java
Set<IAccount> accounts =  app.getAccounts().join();
```

Şu anda önbellekte bulunan belirteçleri kullanmak için aşağıdakileri arayın:

```java
SilentParameters parameters = SilentParameters.builder(scope, accounts.iterator().next()).build(); 
IAuthenticationResult result = app.acquireToken(parameters);
```
