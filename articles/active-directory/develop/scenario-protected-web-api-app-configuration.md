---
title: Korumalı web API uygulamalarını yapılandırma | Azure
titleSuffix: Microsoft identity platform
description: Korumalı bir web API'sini nasıl oluşturup uygulamanızın kodunu nasıl yapılandırıştırmayı öğrenin.
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 3f07105c14d4dafeb689eaaf7d679f93e5f235fe
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79262522"
---
# <a name="protected-web-api-code-configuration"></a>Korumalı web API: Kod yapılandırması

Korumalı web API'nizin kodunu yapılandırmak için şunları anlamanız gerekir:

- API'leri korumalı olarak tanımlayan şey.
- Taşıyıcı belirteci nasıl yapılandırılatır.
- Belirteci nasıl doğrulanır.

## <a name="what-defines-aspnet-and-aspnet-core-apis-as-protected"></a>ASP.NET ve ASP.NET Çekirdek API'leri korumalı olarak tanımlar?

Web uygulamaları gibi, ASP.NET ve ASP.NET Core web API'leri de denetleyici eylemleri **[Authorize]** özelliğiyle önceden belirlenmiş olduğundan korunur. Denetleyici eylemleri yalnızca API yetkili bir kimlikle çağrıldığında çağrılabilir.

Aşağıdaki soruları göz önünde bulundurun:

- Yalnızca bir uygulama web API'sını arayabilir. API, onu çağıran uygulamanın kimliğini nasıl biliyor?
- Uygulama bir kullanıcı adına API'yi arıyorsa, kullanıcının kimliği nedir?

## <a name="bearer-token"></a>Taşıyıcı belirteci

Uygulama çağrıldığında üstbilgide ayarlanan taşıyıcı belirteci, uygulama kimliği hakkında bilgi tutar. Ayrıca, web uygulaması bir daemon uygulamasından servise yapılan çağrıları kabul etmediği sürece kullanıcı hakkında bilgi tutar.

Aşağıda, .NET (MSAL.NET) için Microsoft Kimlik Doğrulama Kitaplığı ile bir belirteç edindikten sonra API çağıran bir istemcinin api'yi aradığını gösteren bir C# kodu örneği verilmiştir:

```csharp
var scopes = new[] {$"api://.../access_as_user"};
var result = await app.AcquireToken(scopes)
                      .ExecuteAsync();

httpClient = new HttpClient();
httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);

// Call the web API.
HttpResponseMessage response = await _httpClient.GetAsync(apiUri);
```

> [!IMPORTANT]
> İstemci uygulaması, taşıyıcının *web API'si için*Microsoft kimlik platformu bitiş noktasına belirteci istemesini ister. Web API, belirteci doğrulaması ve içerdiği iddiaları görüntülemesi gereken tek uygulamadır. İstemci uygulamaları hiçbir zaman jetondaki talepleri incelemeye çalışmamalıdır.
>
> Gelecekte, web API belirteci şifrelenmiş olması gerekebilir. Bu gereksinim, erişim belirteçlerini görüntüleyebilen istemci uygulamalarına erişimi engeller.

## <a name="jwtbearer-configuration"></a>JwtBearer yapılandırma

Bu bölümde taşıyıcı belirteci nasıl yapılandırılabildiğini açıklar.

### <a name="config-file"></a>Config dosyası

```Json
{
  "AzureAd": {
    "Instance": "https://login.microsoftonline.com/",
    "ClientId": "[Client_id-of-web-api-eg-2ec40e65-ba09-4853-bcde-bcb60029e596]",
    /*
      You need specify the TenantId only if you want to accept access tokens from a single tenant
     (line-of-business app).
      Otherwise, you can leave them set to common.
      This can be:
      - A GUID (Tenant ID = Directory ID)
      - 'common' (any organization and personal accounts)
      - 'organizations' (any organization)
      - 'consumers' (Microsoft personal accounts)
    */
    "TenantId": "common"
  },
  "Logging": {
    "LogLevel": {
      "Default": "Warning"
    }
  },
  "AllowedHosts": "*"
}
```

### <a name="code-initialization"></a>Kod başlatma

Bir uygulama **,[Authorize]** özniteliği tutan bir denetleyici eylemine çağrıldığında, ASP.NET ve ASP.NET Core, Yetkilendirme üstbilginin taşıyıcı belirtecinden erişim belirteci ayıklayın. Erişim belirteci daha sonra .NET için Microsoft IdentityModel Uzantıları çağıran JwtBearer ara yazılımına iletilir.

ASP.NET Core'da, bu ara yazılım Startup.cs dosyasında başharflere işlenir.

```csharp
using Microsoft.AspNetCore.Authentication.JwtBearer;
```

Ara yazılım bu talimatla web API'sine eklenir:

```csharp
 services.AddAuthentication(AzureADDefaults.JwtBearerAuthenticationScheme)
         .AddAzureADBearer(options => Configuration.Bind("AzureAd", options));
```

 Şu anda, ASP.NET Core şablonları, kuruluşunuzdaki veya herhangi bir kuruluştaki kullanıcıları oturum açan Azure Etkin Dizin (Azure AD) web API'leri oluşturur. Kişisel hesapları olan kullanıcılaroturum açmaz. Ancak, bu kodu Startup.cs ekleyerek Microsoft kimlik platformu bitiş noktasını kullanmak üzere şablonları değiştirebilirsiniz:

```csharp
services.Configure<JwtBearerOptions>(AzureADDefaults.JwtBearerAuthenticationScheme, options =>
{
    // This is a Microsoft identity platform web API.
    options.Authority += "/v2.0";

    // The web API accepts as audiences both the Client ID (options.Audience) and api://{ClientID}.
    options.TokenValidationParameters.ValidAudiences = new []
    {
     options.Audience,
     $"api://{options.Audience}"
    };

    // Instead of using the default validation (validating against a single tenant,
    // as we do in line-of-business apps),
    // we inject our own multitenant validation logic (which even accepts both v1 and v2 tokens).
    options.TokenValidationParameters.IssuerValidator = AadIssuerValidator.GetIssuerValidator(options.Authority).Validate;;
});
```

Önceki kod parçacığı [Microsoft.Identity.Web/WebApiServiceCollectionExtensions.cs#L50-L63](https://github.com/Azure-Samples/active-directory-dotnet-native-aspnetcore-v2/blob/154282843da2fc2958fad151e2a11e521e358d42/Microsoft.Identity.Web/WebApiServiceCollectionExtensions.cs#L50-L63)ASP.NET Core web API artımlı öğretici ayıklanır. Snippet'in gösterdiğinden daha fazlasını yapan **AddProtectedWebApi** yöntemi Startup.cs' dan çağrılır.

## <a name="token-validation"></a>Belirteç doğrulama

Önceki snippet, JwtBearer ara yazılım, web uygulamalarında OpenID Connect ara ware gibi, değerine `TokenValidationParameters`göre belirteç doğrular. Belirteç gerektiğinde şifresi çözülür, talepler ayıklanır ve imza doğrulanır. Ara yazılım daha sonra bu verileri denetleyerek belirteci doğrular:

- Hedef kitle: Belirteç web API'sını hedeflenebilmiştir.
- Alt: Web API'sını aramasına izin verilen bir uygulama için yayımlandı.
- İhraççı: Güvenilir bir güvenlik belirteci hizmeti (STS) tarafından verildi.
- Son kullanma tarihi: Ömrü aralıktadır.
- İmza: Kurcalanmadı.

Özel doğrulamalar da olabilir. Örneğin, bir belirteç gömülü olduğunda imza anahtarları güvenilir olduğunu ve belirteç yeniden oynatılmıyor doğrulamak mümkündür. Son olarak, bazı protokoller belirli doğrulamalar gerektirir.

### <a name="validators"></a>Doğrulayıcıları

Doğrulama adımları, .NET açık kaynak kitaplığı [için Microsoft IdentityModel Uzantıları](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet) tarafından sağlanan doğrulayıcılarda yakalanır. Doğrulayıcılar [microsoft.identitymodel.Tokens/Validators.cs](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet/blob/master/src/Microsoft.IdentityModel.Tokens/Validators.cs)kitaplık kaynak dosyasında tanımlanır.

Bu tablo doğrulayıcıları açıklar:

| Doğrulayıcı | Açıklama |
|---------|---------|
| **Doğrulayan Kitle** | Belirteci sizin için belirteç doğrulayan uygulama için olduğundan emin olun. |
| **Doğrulayıcı** | Belirteci güvenilir bir STS tarafından verildiğinden emin olur, yani güvendiğiniz birinden gelir. |
| **DoğrulamaIssuerSigningKey** | Belirteç güvenleri doğrulayan uygulama sağlar belirteci imzalamak için kullanılan anahtar. Anahtarın jetonun içine gömülü olduğu özel bir durum var. Ama bu dava genelde ortaya çıkmaz. |
| **Ömür Boyu Doğrulama** | Belirteci hala veya zaten geçerli olduğundan emin olur. Geçerlilik, belirteç ömrü önceki tarafından belirtilen aralıkta **notbefore** olup olmadığını ve **süresinin dolup olmadığını denetler.** |
| **İmzayı Doğrula** | Jetonun kurcalanmadığından emin olmak için. |
| **TokenReplay'i Doğrulama** | Belirteç yeniden oynatını sağlar. Bazı tek kullanımlık protokoller için özel bir durum var. |

Doğrulayıcılar, **TokenValidationParametreler** sınıfının özellikleriyle ilişkilidir. Özellikler ASP.NET ve ASP.NET Core yapılandırmasından başharfe alınır.

Çoğu durumda, parametreleri değiştirmeniz gerekmez. Tek kiracı olmayan uygulamalar istisnadır. Bu web uygulamaları, herhangi bir kuruluştan veya kişisel Microsoft hesaplarından kullanıcıları kabul eder. Bu durumda verenler doğrulanmalıdır.

## <a name="token-validation-in-azure-functions"></a>Azure İşlevlerinde Belirteç doğrulaması

Azure İşlevler'de gelen erişim belirteçlerini de doğrulayabilirsiniz. [Microsoft .NET](https://github.com/Azure-Samples/ms-identity-dotnet-webapi-azurefunctions), [NodeJS](https://github.com/Azure-Samples/ms-identity-nodejs-webapi-azurefunctions)ve [Python'da](https://github.com/Azure-Samples/ms-identity-python-webapi-azurefunctions)bu tür doğrulama örnekleri bulabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Kodunuzda kapsamları ve uygulama rollerini doğrulama](scenario-protected-web-api-verification-scope-app-roles.md)
