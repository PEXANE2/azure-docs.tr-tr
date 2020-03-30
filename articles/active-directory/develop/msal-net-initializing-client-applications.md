---
title: İstemci uygulamalarını MSAL.NET başlatma | Azure
titleSuffix: Microsoft identity platform
description: .NET (MSAL.NET) için Microsoft Kimlik Doğrulama Kitaplığı'nı kullanarak ortak istemci ve gizli istemci uygulamalarını başlatma hakkında bilgi edinin.
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 04/12/2019
ms.author: marsma
ms.reviewer: saeeda
ms.custom: aaddev
ms.openlocfilehash: 57ce6ab31421cd4016f7e204eeabce82f2f7e6a7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77084001"
---
# <a name="initialize-client-applications-using-msalnet"></a>MSAL.NET kullanarak istemci uygulamalarını başlatma
Bu makalede, .NET (MSAL.NET) için Microsoft Kimlik Doğrulama Kitaplığı kullanarak ortak istemci ve gizli istemci uygulamaları nın başlatılması açıklanmaktadır.  İstemci uygulama türleri ve uygulama yapılandırma seçenekleri hakkında daha fazla bilgi edinmek için [genel bakışı](msal-client-applications.md)okuyun.

MSAL.NET 3.x ile, bir uygulamayı anında kullanmanın önerilen yolu uygulama `PublicClientApplicationBuilder` oluşturucularını kullanmaktır: ve `ConfidentialClientApplicationBuilder`. Uygulamayı koddan veya yapılandırma dosyasından veya hatta her iki yaklaşımı da karıştırarak yapılandırmak için güçlü bir mekanizma sunarlar.

## <a name="prerequisites"></a>Ön koşullar
Bir uygulamayı başlatmadan önce, uygulamanızın Microsoft kimlik platformuyla tümleştirilebilmeleri için [uygulamanızı kaydetmeniz](quickstart-register-app.md) gerekir.  Kayıt olduktan sonra aşağıdaki bilgilere ihtiyacınız olabilir (Azure portalında bulunabilir):

- İstemci kimliği (GUID'i temsil eden bir dize)
- Kimlik sağlayıcı URL (örneğin adı) ve uygulamanız için oturum açma hedef kitlesi. Bu iki parametre topluca otorite olarak bilinir.
- Yalnızca kuruluşunuz için bir iş başvurusu satırı yazıyorsanız kiracı kimliği (tek kiracılı uygulama olarak da adlandırılır).
- Uygulama gizli (istemci gizli dize) veya sertifika (tip X509Certificate2) gizli bir istemci uygulaması ise.
- Web uygulamaları ve bazen kamu istemcisi uygulamaları için (özellikle uygulamanızın bir broker kullanması gerektiğinde), kimlik sağlayıcısının güvenlik belirteçleri ile uygulamanıza geri döneceği redirectUri'yi de ayarlamış olacaksınız.

## <a name="ways-to-initialize-applications"></a>Uygulamaları başlatma yolları
İstemci uygulamalarını anında almanın birçok farklı yolu vardır.

### <a name="initializing-a-public-client-application-from-code"></a>Bir kamu istemcisi uygulamasını koddan başlatma

Aşağıdaki kod, iş ve okul hesapları veya kişisel Microsoft hesaplarıyla birlikte Microsoft Azure genel bulutundaki oturum açma kullanıcıları, ortak istemci uygulaması anında gerçekleşir.

```csharp
IPublicClientApplication app = PublicClientApplicationBuilder.Create(clientId)
    .Build();
```

### <a name="initializing-a-confidential-client-application-from-code"></a>Koddan gizli bir istemci uygulamasını başlatma

Aynı şekilde, aşağıdaki kod, Microsoft Azure genel bulutundaki kullanıcıların `https://myapp.azurewebsites.net`iş ve okul hesapları veya kişisel Microsoft hesaplarıyla birlikte gelen belirteçleri işleyen gizli bir uygulamayı (bir Web uygulamasında bulunan bir Web uygulaması) anında kullanır. Uygulama, bir istemci sırrıpaylaşılarak kimlik sağlayıcısı ile tanımlanır:

```csharp
string redirectUri = "https://myapp.azurewebsites.net";
IConfidentialClientApplication app = ConfidentialClientApplicationBuilder.Create(clientId)
    .WithClientSecret(clientSecret)
    .WithRedirectUri(redirectUri )
    .Build();
```

Bildiğiniz gibi, üretimde istemci sırrı nı kullanmak yerine Azure AD sertifikasıile paylaşmak isteyebilirsiniz. Kod daha sonra aşağıdaki olacaktır:

```csharp
IConfidentialClientApplication app = ConfidentialClientApplicationBuilder.Create(clientId)
    .WithCertificate(certificate)
    .WithRedirectUri(redirectUri )
    .Build();
```

### <a name="initializing-a-public-client-application-from-configuration-options"></a>Yapılandırma seçeneklerinden ortak istemci uygulamasını başlatma

Aşağıdaki kod, programlı olarak doldurulabilen veya yapılandırma dosyasından okunabilen bir yapılandırma nesnesinden ortak istemci uygulamasını anında başlatır:

```csharp
PublicClientApplicationOptions options = GetOptions(); // your own method
IPublicClientApplication app = PublicClientApplicationBuilder.CreateWithApplicationOptions(options)
    .Build();
```

### <a name="initializing-a-confidential-client-application-from-configuration-options"></a>Yapılandırma seçeneklerinden gizli bir istemci uygulamasını başlatma

Aynı tür desen gizli istemci uygulamaları için de geçerlidir. Değiştiriciler (burada `.WithXXX` bir sertifika) kullanarak diğer parametreleri de ekleyebilirsiniz.

```csharp
ConfidentialClientApplicationOptions options = GetOptions(); // your own method
IConfidentialClientApplication app = ConfidentialClientApplicationBuilder.CreateWithApplicationOptions(options)
    .WithCertificate(certificate)
    .Build();
```

## <a name="builder-modifiers"></a>Oluşturucu değiştiriciler

Uygulama oluşturucularını kullanan kod parçacıklarında, bir `.With` dizi yöntem değiştirici olarak uygulanabilir `.WithCertificate` (örneğin, ve). `.WithRedirectUri` 

### <a name="modifiers-common-to-public-and-confidential-client-applications"></a>Genel ve gizli istemci uygulamalarında ortak olan değiştiriciler

Ortak istemci veya gizli istemci uygulama oluşturucu üzerinde ayarlayabilirsiniz değiştiriciler şunlardır:

|Değiştirici | Açıklama|
|--------- | --------- |
|`.WithAuthority()`7 geçersiz kılma | Azure Bulutu, hedef kitle, kiracı (kiracı kimliği veya etki alanı adı) seçme veya doğrudan yetkili URI'yi sağlama olanağıyla uygulama varsayılan yetkisini bir Azure REKLAM yetkilisiolarak ayarlar.|
|`.WithAdfsAuthority(string)` | Uygulama varsayılan yetkisini ADFS yetkilisi olarak ayarlar.|
|`.WithB2CAuthority(string)` | Uygulama varsayılan yetkisini Azure AD B2C yetkilisi olarak ayarlar.|
|`.WithClientId(string)` | İstemci kimliğini geçersiz kılar.|
|`.WithComponent(string)` | MSAL.NET kullanarak kitaplığın adını ayarlar (telemetri nedenleriyle). |
|`.WithDebugLoggingCallback()` | Çağrıldığı takdirde, `Debug.Write` uygulama yalnızca hata ayıklama izlemelerini etkinleştirmeyi çağırır. Daha fazla bilgi için [Günlük'e](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/logging) bakın.|
|`.WithExtraQueryParameters(IDictionary<string,string> eqp)` | Tüm kimlik doğrulama isteğinde gönderilecek uygulama düzeyi ek sorgu parametrelerini ayarlayın. Bu, her belirteç edinme yöntemi düzeyinde (aynı) `.WithExtraQueryParameters pattern`geçersiz kılınabilir.|
|`.WithHttpClientFactory(IMsalHttpClientFactory httpClientFactory)` | BIR HTTP proxy için yapılandırma veya MSAL'ı belirli bir HttpClient kullanmaya zorlamak gibi gelişmiş senaryoları etkinleştiri (örneğin ASP.NET Core web uygulamaları/API'lerinde).|
|`.WithLogging()` | Çağrılsa, uygulama hata ayıklama izleri olan bir geri arama çağırır. Daha fazla bilgi için [Günlük'e](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/logging) bakın.|
|`.WithRedirectUri(string redirectUri)` | Varsayılan yeniden yönlendirme URI geçersiz kılar. Kamu istemcisi uygulamaları söz konusu olduğunda, bu broker içeren senaryolar için yararlı olacaktır.|
|`.WithTelemetry(TelemetryCallback telemetryCallback)` | Telemetri göndermek için kullanılan temsilciyi ayarlar.|
|`.WithTenantId(string tenantId)` | Kiracı kimliğini veya kiracı açıklamasını geçersiz kılar.|

### <a name="modifiers-specific-to-xamarinios-applications"></a>Xamarin.iOS uygulamalarına özel değiştiriciler

Xamarin.iOS'ta bir ortak istemci uygulama oluşturucuüzerinde ayarlayabilirsiniz değiştiriciler şunlardır:

|Değiştirici | Açıklama|
|--------- | --------- |
|`.WithIosKeychainSecurityGroup()` | **Yalnızca Xamarin.iOS**: iOS anahtar zinciri güvenlik grubunu ayarlar (önbellek kalıcılığı için).|

### <a name="modifiers-specific-to-confidential-client-applications"></a>Gizli istemci uygulamalarına özel değiştiriciler

Gizli bir istemci uygulama oluşturucu üzerinde ayarlayabilirsiniz değiştiriciler şunlardır:

|Değiştirici | Açıklama|
|--------- | --------- |
|`.WithCertificate(X509Certificate2 certificate)` | Uygulamayı tanımlayan sertifikayı Azure AD ile ayarlar.|
|`.WithClientSecret(string clientSecret)` | Uygulamayı Azure AD ile tanımlayan istemci sırrını (uygulama parolası) ayarlar.|

Bu değiştiriciler birbirini dışlar. Her ikisini de sağlarsanız, MSAL anlamlı bir özel durum oluşturur.

### <a name="example-of-usage-of-modifiers"></a>Değiştiriciler kullanım örneği

Uygulamanızın yalnızca kuruluşunuz için olan bir iş hattı uygulaması olduğunu varsayalım.  O zaman şöyle yazabilirsin:

```csharp
IPublicClientApplication app;
app = PublicClientApplicationBuilder.Create(clientId)
        .WithAadAuthority(AzureCloudInstance.AzurePublic, tenantId)
        .Build();
```

Burada ilginç hale gelir ulusal bulutlar için programlama şimdi basitleştirilmiş olmasıdır. Uygulamanızın ulusal bir bulutta çok kiracılı bir uygulama olmasını istiyorsanız, örneğin şunları yazabilirsiniz:

```csharp
IPublicClientApplication app;
app = PublicClientApplicationBuilder.Create(clientId)
        .WithAadAuthority(AzureCloudInstance.AzureUsGovernment, AadAuthorityAudience.AzureAdMultipleOrgs)
        .Build();
```

ADFS için bir geçersiz kılma da vardır (ADFS 2019 şu anda desteklenmiyor):
```csharp
IPublicClientApplication app;
app = PublicClientApplicationBuilder.Create(clientId)
        .WithAdfsAuthority("https://consoso.com/adfs")
        .Build();
```

Son olarak, bir Azure AD B2C geliştiricisiyseniz, kiracınızı şu şekilde belirtebilirsiniz:

```csharp
IPublicClientApplication app;
app = PublicClientApplicationBuilder.Create(clientId)
        .WithB2CAuthority("https://fabrikamb2c.b2clogin.com/tfp/{tenant}/{PolicySignInSignUp}")
        .Build();
```
