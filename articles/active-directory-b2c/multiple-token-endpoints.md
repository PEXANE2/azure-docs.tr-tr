---
title: OWIN tabanlı web API'lerini b2clogin.com'a geçirin
titleSuffix: Azure AD B2C
description: Uygulamalarınızı b2clogin.com geçirerken birden çok belirteç veren tarafından verilen belirteçleri desteklemek için bir .NET web API'sini nasıl etkinleştirdiğinizi öğrenin.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 07/31/2019
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 5daf88e746ea803f345c79bd31d656f2615b6754
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78184103"
---
# <a name="migrate-an-owin-based-web-api-to-b2clogincom"></a>b2clogin.com için OWIN tabanlı bir web API'sini geçirin

Bu makalede, [.NET (OWIN) için Açık Web Arabirimi](http://owin.org/)uygulayan web API'lerinde birden çok belirteç verenler için destek etkinleştirmek için bir teknik açıklanmaktadır. Azure Etkin Dizin B2C (Azure AD B2C) API'lerini ve uygulamalarını *login.microsoftonline.com'dan* *b2clogin.com'ye*geçirdiğinizde birden çok belirteç uç noktasını desteklemek yararlıdır.

Hem b2clogin.com hem de login.microsoftonline.com tarafından verilen belirteçleri kabul etmek için API'nize destek ekleyerek, API'den login.microsoftonline.com verilen belirteçler için desteği kaldırmadan önce web uygulamalarınızı aşamalı bir şekilde geçirebilirsiniz.

Aşağıdaki bölümler, [Microsoft OWIN][katana] ara yazılım bileşenlerini (Katana) kullanan bir web API'sında birden çok verenin nasıl etkinleştirilenene ilişkin bir örnek sunar. Kod örnekleri Microsoft OWIN ara yazılımına özgü olsa da, genel teknik diğer OWIN kitaplıkları için geçerli olmalıdır.

> [!NOTE]
> Bu makale, şu anda dağıtılan API'leri ve başvuruyapan `login.microsoftonline.com` uygulamaları olan ve önerilen `b2clogin.com` bitiş noktasına geçiş yapmak isteyen Azure AD B2C müşterileri için tasarlanmıştır. Yeni bir uygulama ayarlıyorsanız, [b2clogin.com](b2clogin.md) yönlendirilmiş olarak kullanın.

## <a name="prerequisites"></a>Ön koşullar

Bu makaledeki adımlara devam etmeden önce aşağıdaki Azure AD B2C kaynaklarına ihtiyacınız var:

* [Kiracınızda](tutorial-create-user-flows.md) oluşturulan kullanıcı akışları veya [özel ilkeler](custom-policy-get-started.md)

## <a name="get-token-issuer-endpoints"></a>Belirteç veren uç noktalarını alın

Öncelikle API'nizde desteklemek istediğiniz her ihraççı için belirteç veren uç noktası URIs'lerini almanız gerekir. Azure AD B2C kiracınız tarafından desteklenen *b2clogin.com* ve *login.microsoftonline.com* uç noktalarını almak için Azure portalında aşağıdaki yordamı kullanın.

Varolan kullanıcı akışlarından birini seçerek başlayın:

1. [Azure portalında](https://portal.azure.com) Azure AD B2C kiracınıza gidin
1. **İlkeler** **altında, Kullanıcı akışlarını (ilkeler)** seçin
1. Varolan bir ilke seçin, örneğin *B2C_1_signupsignin1,* ardından **Kullanıcı akışını çalıştır'ı** seçin
1. Sayfanın üst kısmına yakın **Çalıştır kullanıcı akışı** başlığı altında, kullanıcı akışı için OpenID Connect bulma bitiş noktasına gitmek için köprüseçeneğini seçin.

    ![Azure portalının Çalıştır şimdi sayfasında tanınmış URI köprü](media/multi-token-endpoints/portal-01-policy-link.png)

1. Tarayıcınızda açılan sayfada, örneğin değeri `issuer` kaydedin:

    `https://your-b2c-tenant.b2clogin.com/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/v2.0/`

1. Diğer **etki alanını** seçmek için etki alanını seç'i kullanın, ardından önceki `issuer` iki adımı bir kez daha gerçekleştirin ve değerini kaydedin.

Şimdi benzer iki URI kaydedilmiş olmalıdır:

```
https://login.microsoftonline.com/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/v2.0/
https://your-b2c-tenant.b2clogin.com/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/v2.0/
```

### <a name="custom-policies"></a>Özel ilkeler

Kullanıcı akışları yerine özel ilkeleriniz varsa, veren IU'ları almak için benzer bir işlem kullanabilirsiniz.

1. Azure AD B2C kiracınıza gidin
1. **Kimlik Deneyimi Çerçevesi** Seçin
1. Güvenen parti ilkelerinizden birini seçin( örneğin, *B2C_1A_signup_signin*
1. Bir **etki alanı** seçmek için etki alanı açılır ayinini *seçinyourtenant.b2clogin.com*
1. OpenID Connect bulma bitiş noktası altında görüntülenen **köprüözelliğini** seçin
1. `issuer` Değeri kaydetme
1. Diğer etki alanı için 4-6 adımlarını gerçekleştirin, örneğin *login.microsoftonline.com*

## <a name="get-the-sample-code"></a>Örnek kodunu alma

Artık her iki belirteç uç noktası URIs'iniz olduğuna göre, her iki uç noktanın da geçerli verenler olduğunu belirtmek için kodunuzu güncelleştirmeniz gerekir. Örnek üzerinden yürümek için örnek uygulamayı karşıdan yükleyin veya klonlayın, ardından her iki uç noktayı da geçerli verenler olarak desteklemek için örneği güncelleştirin.

Arşivi [indirin: active-directory-b2c-dotnet-webapp-and-webapi-master.zip][sample-archive]

```
git clone https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi.git
```

## <a name="enable-multiple-issuers-in-web-api"></a>Web API'sinde birden çok vereni etkinleştirme

Bu bölümde, her iki belirteç veren uç noktalarının geçerli olduğunu belirtmek için kodu güncelleştirebilirsiniz.

1. Visual Studio'da **B2C-WebAPI-DotNet.sln** çözümlerini açın
1. **TaskService** projesinde, düzenleyicinizde *TaskService\\\\App_Start **Startup.Auth.cs*** dosyasını açın
1. Dosyanın `using` üst bölümüne aşağıdaki yönergeyi ekleyin:

    `using System.Collections.Generic;`
1. [`ValidIssuers`][validissuers] Özelliği [`TokenValidationParameters`][tokenvalidationparameters] tanıma ekleyin ve önceki bölümde kaydettiğiniz her iki ÜR'leri belirtin:

    ```csharp
    TokenValidationParameters tvps = new TokenValidationParameters
    {
        // Accept only those tokens where the audience of the token is equal to the client ID of this app
        ValidAudience = ClientId,
        AuthenticationType = Startup.DefaultPolicy,
        ValidIssuers = new List<string> {
            "https://login.microsoftonline.com/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/v2.0/",
            "https://{your-b2c-tenant}.b2clogin.com/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/v2.0/"
        }
    };
    ```

`TokenValidationParameters`MSAL.NET tarafından sağlanır ve *Startup.Auth.cs*kodun bir sonraki bölümünde OWIN ara yazılım tarafından tüketilir. Birden çok geçerli veren belirtildiğinde, OWIN uygulama ardışık adı her iki belirteç uç noktasının da geçerli verenler olduğunu niçin onayladığının farkında dır.

```csharp
app.UseOAuthBearerAuthentication(new OAuthBearerAuthenticationOptions
{
    // This SecurityTokenProvider fetches the Azure AD B2C metadata &  from the OpenID Connect metadata endpoint
    AccessTokenFormat = new JwtFormat(tvps, new tCachingSecurityTokenProvider(String.Format(AadInstance, ultPolicy)))
});
```

Daha önce de belirtildiği gibi, diğer OWIN kitaplıkları genellikle birden çok ihraççıyı desteklemek için benzer bir tesis sağlar. Her kitaplık için örnek sağlamak bu makalenin kapsamı dışında olsa da, çoğu kitaplık için benzer bir teknik kullanabilirsiniz.

## <a name="switch-endpoints-in-web-app"></a>Web uygulamasında uç noktaları değiştirme

Artık web API'nız tarafından desteklenen her iki URI ile, artık web uygulamanızı güncellemeniz gerekir, böylece b2clogin.com uç noktasından belirteçleri alır.

Örneğin, *TaskWebApp\\**Web.config ** taskwebapp*projesinin **TaskWebApp** `ida:AadInstance` değerini değiştirerek örnek web uygulamasını yeni bitiş noktasını kullanacak şekilde yapılandırabilirsiniz.

TaskWebApp'ın `ida:AadInstance` *Web.config'indeki* değeri değiştirin, böylece `{your-b2c-tenant-name}.b2clogin.com` başvuru yerine `login.microsoftonline.com`.

Önce:

```xml
<!-- Old value -->
<add key="ida:AadInstance" value="https://login.microsoftonline.com/tfp/{0}/{1}" />
```

Sonra (B2C kiracınızın adı ile değiştirin): `{your-b2c-tenant}`

```xml
<!-- New value -->
<add key="ida:AadInstance" value="https://{your-b2c-tenant}.b2clogin.com/tfp/{0}/{1}" />
```

Uç nokta dizeleri web uygulamasının yürütülmesi sırasında oluşturulduğunda, belirteçleri istediğinde b2clogin.com tabanlı uç noktalar kullanılır.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, birden çok veren uç noktalarından belirteçleri kabul etmek için Microsoft OWIN ara ware (Katana) uygulayan bir web API yapılandırma yöntemi sunulmuştur. Fark edebileceğiniz gibi, *web.Config* dosyalarında hem TaskService hem de TaskWebApp projelerinin bu projeleri kendi kiracınıza karşı oluşturmak ve çalıştırmak istiyorsanız değiştirilmesi gereken birkaç dizeleri vardır. Projeleri iş başında görmek istiyorsanız, projeleri uygun şekilde değiştirebilirsiniz, ancak bu nedenle bu makalenin kapsamı dışında dır.

Azure AD B2C tarafından yayılan farklı güvenlik belirteçleri türleri hakkında daha fazla bilgi için Azure [Etkin Dizin B2C'deki belirteçlere genel bakış](tokens-overview.md)bölümüne bakın.

<!-- LINKS - External -->
[sample-archive]: https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi/archive/master.zip
[sample-repo]: https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi

<!-- LINKS - Internal -->
[katana]: https://docs.microsoft.com/aspnet/aspnet/overview/owin-and-katana/
[validissuers]: https://docs.microsoft.com/dotnet/api/microsoft.identitymodel.tokens.tokenvalidationparameters.validissuers
[tokenvalidationparameters]: https://docs.microsoft.com/dotnet/api/microsoft.identitymodel.tokens.tokenvalidationparameters
