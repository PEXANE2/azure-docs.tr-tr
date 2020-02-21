---
title: Korumalı Web API uygulamalarını yapılandırma | Mavisi
titleSuffix: Microsoft identity platform
description: Korumalı bir Web API 'SI oluşturmayı ve uygulamanızın kodunu yapılandırmayı öğrenin.
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
ms.openlocfilehash: b05eefb2a0e516772390f898c22e723b08973338
ms.sourcegitcommit: 98a5a6765da081e7f294d3cb19c1357d10ca333f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/20/2020
ms.locfileid: "77484460"
---
# <a name="protected-web-api-code-configuration"></a>Korumalı Web API 'SI: kod yapılandırması

Korunan Web API 'niz için kodu yapılandırmak üzere şunları anlamanız gerekir:

- API 'Leri korumalı olarak tanımlar.
- Bir taşıyıcı belirtecini yapılandırma.
- Belirteci doğrulama.

## <a name="what-defines-aspnet-and-aspnet-core-apis-as-protected"></a>ASP.NET ve ASP.NET Core API 'Leri korunan olarak tanımlar?

Web Apps gibi, ASP.NET ve ASP.NET Core Web API 'Leri, denetleyici eylemlerine **[Yetkilendir]** özniteliği ön eki eklendiği için korunur. Denetleyici eylemleri yalnızca API bir yetkili kimlikle çağrılırsa çağrılabilir.

Aşağıdaki soruları göz önünde bulundurun:

- Yalnızca bir uygulama, bir Web API 'sini çağırabilir. API bu uygulamayı çağıran uygulamanın kimliğini nasıl bilir?
- Uygulama bir kullanıcı adına API 'yi çağırırsa, kullanıcının kimliği nedir?

## <a name="bearer-token"></a>Taşıyıcı belirteci

Uygulama çağrıldığında üst bilgide ayarlanan taşıyıcı belirteç, uygulama kimliği hakkında bilgi içerir. Web uygulaması, bir Daemon uygulamasından hizmetten hizmete çağrılar kabul etmediği takdirde, kullanıcı hakkındaki bilgileri de barındırır.

Aşağıda, .NET C# Için Microsoft kimlik doğrulama kitaplığı ile belirteç aldıktan sonra API 'yi çağıran bir istemciyi gösteren bir kod örneği (msal.net):

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
> İstemci uygulaması, *Web API 'si Için*Microsoft Identity platform uç noktasına taşıyıcı belirtecini ister. Web API 'SI, belirtecin doğrulanması ve içerdiği talepleri görüntülemesi gereken tek uygulamadır. İstemci uygulamaları, belirteçlerdeki talepleri incelemeye asla denememelidir.
>
> Gelecekte, Web API 'SI belirtecin şifrelenmesini gerektirebilir. Bu gereksinim, erişim belirteçlerini görüntüleyebilen istemci uygulamalarına erişimi engeller.

## <a name="jwtbearer-configuration"></a>Jwttaşıyıcı yapılandırması

Bu bölümde, bir taşıyıcı belirtecinin nasıl yapılandırılacağı açıklanmaktadır.

### <a name="config-file"></a>Yapılandırma dosyası

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

Bir uygulama bir **[Yetkilendir]** özniteliği tutan bir denetleyici eyleminde çağrıldığında, ASP.NET ve ASP.NET Core erişim belirtecini yetkilendirme üstbilgisinin taşıyıcı belirtecinden ayıklayın. Daha sonra erişim belirteci, .NET için Microsoft IdentityModel uzantıları 'nı çağıran Jwttaşıyıcı ara yazılıma iletilir.

ASP.NET Core, bu ara yazılım Startup.cs dosyasında başlatılır.

```csharp
using Microsoft.AspNetCore.Authentication.JwtBearer;
```

Bu yönerge, ara yazılım Web API 'sine eklenir:

```csharp
 services.AddAuthentication(AzureADDefaults.JwtBearerAuthenticationScheme)
         .AddAzureAdBearer(options => Configuration.Bind("AzureAd", options));
```

 Şu anda ASP.NET Core şablonları, kuruluşunuzdaki veya herhangi bir kuruluşun içindeki kullanıcıların oturum açmasını sağlayan Azure Active Directory (Azure AD) Web API 'Leri oluşturur. Kişisel hesaplarla kullanıcıları oturum açtıklarında oturum açabilirler. Ancak, bu kodu Startup.cs 'e ekleyerek şablonları Microsoft Identity platform uç noktasını kullanacak şekilde değiştirebilirsiniz:

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

Yukarıdaki kod parçacığı, [Microsoft. Identity. Web/WebApiServiceCollectionExtensions. cs # L50-L63](https://github.com/Azure-Samples/active-directory-dotnet-native-aspnetcore-v2/blob/154282843da2fc2958fad151e2a11e521e358d42/Microsoft.Identity.Web/WebApiServiceCollectionExtensions.cs#L50-L63)' deki ASP.NET Core Web API artımlı öğreticiden ayıklanır. Kod parçacığının gösterdiği daha fazla olan **Addprotectedwebapi** yöntemi, Startup.cs adresinden çağırılır.

## <a name="token-validation"></a>Belirteç doğrulama

Yukarıdaki kod parçacığında, Web Apps 'teki OpenID Connect ara yazılımı gibi Jwttaşıyıcı ara yazılımı, `TokenValidationParameters`değerine göre belirteci doğrular. Gerektiğinde belirtecin şifresi çözülür, talepler ayıklanır ve imza doğrulanır. Bu durumda, ara yazılım bu verileri denetleyerek belirteci doğrular:

- Hedef kitle: belirteç Web API 'sine yöneliktir.
- Sub: Web API 'sini çağırmaya izin verilen bir uygulama için verildi.
- Veren: bir güvenilen güvenlik belirteci hizmeti (STS) tarafından verilmiş.
- Süre sonu: ömrü Aralık içinde.
- İmza: ile oynanmadı.

Özel doğrulamalar de olabilir. Örneğin, bir belirteçte gömülü olduğunda imzalama anahtarlarının güvenilir olduğunu ve belirtecin yeniden çalınmayacağını doğrulamak mümkündür. Son olarak, bazı protokoller belirli doğrulamalar gerektirir.

### <a name="validators"></a>Metninin

Doğrulama adımları, .NET açık kaynak kitaplığı [Için Microsoft IdentityModel uzantıları](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet) tarafından belirtilen doğrulayıcılar içinde yakalanır. Doğrulayıcılar, [Microsoft. IdentityModel. Tokens/Doğrulayıcı. cs](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet/blob/master/src/Microsoft.IdentityModel.Tokens/Validators.cs)kitaplık kaynak dosyasında tanımlanmıştır.

Bu tabloda doğrulayıcılar açıklanmaktadır:

| Doğrulayıcı | Açıklama |
|---------|---------|
| **ValidateAudience** | Belirtecin sizin için belirteci doğrulayan uygulamanın olduğundan emin olur. |
| **Validateıssuer** | Belirtecin güvenilir bir STS tarafından verildiğini ve güvendiğiniz bir kişiden geldiğini sağlar. |
| **Validateıssuersigningkey** | Belirteç, belirteci imzalamak için kullanılan anahtara güvendiğini belirten uygulamayı sağlar. Anahtarın belirtece gömülü olduğu özel bir durum vardır. Ancak bu durum genellikle ortaya çıkar. |
| **ValidateLifetime** | Belirtecin hala veya zaten geçerli olduğundan emin olur. Doğrulayıcı, belirtecin kullanım ömrünün **NotBefore** ve **Expires** talepleri tarafından belirtilen aralıkta olup olmadığını denetler. |
| **ValidateSignature** | Belirtecin oynanmamasını sağlar. |
| **ValidateTokenReplay** | Belirtecin yeniden çalınmamasını sağlar. Bazı Onetime kullanım protokolleri için özel bir durum vardır. |

Doğrulayıcılar **Tokenvalidationparameters** sınıfının özellikleriyle ilişkilendirilir. Özellikler, ASP.NET ve ASP.NET Core yapılandırmasından başlatılır.

Çoğu durumda, parametreleri değiştirmeniz gerekmez. Tek kiracılar olmayan uygulamalar özel durumlardır. Bu Web Apps kullanıcıları herhangi bir kuruluştan veya kişisel Microsoft hesaplarından kabul eder. Bu durumda verenler doğrulanması gerekir.

## <a name="token-validation-in-azure-functions"></a>Azure Işlevlerinde belirteç doğrulama

Ayrıca, Azure Işlevlerinde gelen erişim belirteçlerini doğrulayabilirsiniz. [Microsoft .net](https://github.com/Azure-Samples/ms-identity-dotnet-webapi-azurefunctions), [NodeJS](https://github.com/Azure-Samples/ms-identity-nodejs-webapi-azurefunctions)ve [Python](https://github.com/Azure-Samples/ms-identity-python-webapi-azurefunctions)'da bu doğrulamanın örneklerini bulabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Kodunuzda kapsamları ve uygulama rollerini doğrulama](scenario-protected-web-api-verification-scope-app-roles.md)
