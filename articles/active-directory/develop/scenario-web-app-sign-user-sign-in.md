---
title: Kullanıcılara oturum açan Web uygulaması (oturum açma)-Microsoft Identity platform
description: Kullanıcıları oturum açan bir Web uygulaması oluşturmayı öğrenin (oturum açın)
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: be7801515355452306cd5e7afa709a0681c7c314
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68562197"
---
# <a name="web-app-that-signs-in-users---sign-in"></a>Kullanıcılar oturum açtığında oturum açan Web uygulaması

Kullanıcıları oturum açan Web uygulamanız için koda oturum açma eklemeyi öğrenin.

## <a name="sign-in"></a>Oturum açma

Önceki makale [uygulamasının kod yapılandırmasında](scenario-web-app-sign-user-app-configuration.md) , oturum kapatma uygulamanız için gereken kod. Kullanıcı uygulamanıza oturum açtıktan sonra muhtemelen oturumu kapatmalarını sağlamak isteyeceksiniz. ASP.NET çekirdek, oturumunuzu sizin için gerçekleştirir.

## <a name="what-sign-out-involves"></a>Oturum kapatma neleri içerir?

Bir Web uygulamasından oturum açmak, Web uygulamasının durumundan oturum açmış hesap hakkındaki bilgileri kaldırmasından daha fazla.
Web uygulaması oturumu kapatmak için kullanıcıyı Microsoft Identity Platform `logout` uç noktasına yönlendirmelidir. Web uygulamanız kullanıcıyı `logout` uç noktaya yönlendirirse, bu uç nokta kullanıcının oturumunu tarayıcıdan temizler. Uygulamanız `logout` uç noktaya gitmediyse, Kullanıcı Microsoft Identity platform uç noktası ile geçerli bir çoklu oturum açma oturumuna sahip olduklarından, kimlik bilgilerini tekrar girmeden uygulamanızı yeniden doğrulayacak.

Daha fazla bilgi edinmek için [Microsoft Identity platform ve OpenID Connect Protocol](v2-protocols-oidc.md) kavramsal belgelerindeki [oturum açma isteği gönderme](v2-protocols-oidc.md#send-a-sign-out-request) bölümüne bakın.

## <a name="application-registration"></a>Uygulama kaydı

Uygulama kaydı sırasında, bir **Post Logout URI 'si**kaydettiniz. Öğreticimizde, **kimlik doğrulama** sayfasındaki `https://localhost:44321/signout-oidc` **Gelişmiş ayarlar** bölümünün **oturum kapatma URL 'si** alanına kaydolduysanız. Ayrıntılar için bkz [. WebApp uygulamasını kaydetme](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/1-WebApp-OIDC/1-1-MyOrg#register-the-webapp-app-webapp)

## <a name="aspnet-core-code"></a>ASP.NET Core kodu

### <a name="signout-button"></a>SignOut düğmesi

Oturumu Kapat düğmesi ' de `Views\Shared\_LoginPartial.cshtml` gösterilir ve yalnızca kimliği doğrulanmış bir hesap (kullanıcının daha önce oturum açması durumunda) görüntülenir.

```html
@using Microsoft.Identity.Web
@if (User.Identity.IsAuthenticated)
{
    <ul class="nav navbar-nav navbar-right">
        <li class="navbar-text">Hello @User.GetDisplayName()!</li>
        <li><a asp-area="AzureAD" asp-controller="Account" asp-action="SignOut">Sign out</a></li>
    </ul>
}
else
{
    <ul class="nav navbar-nav navbar-right">
        <li><a asp-area="AzureAD" asp-controller="Account" asp-action="SignIn">Sign in</a></li>
    </ul>
}
```

### <a name="signout-action-of-the-accountcontroller"></a>`Signout()`öğesinin eylemi`AccountController`

Web uygulamasındaki **oturumu** Kapat düğmesine basıldığında `SignOut` `Account` denetleyicideki eylem tetiklenir. ASP.NET Core şablonlarının önceki sürümlerinde, `Account` denetleyici Web uygulamasıyla katıştırıldı, ancak artık ASP.NET Core çerçevesinin bir parçası olduğu için bu durum artık böyle değildir. 

İçin `AccountController` kodu, [accountcontroller.cs](https://github.com/aspnet/AspNetCore/blob/master/src/Azure/AzureAD/Authentication.AzureAD.UI/src/Areas/AzureAD/Controllers/AccountController.cs)adresinden ASP.NET Core deposundan kullanılabilir. Hesap denetimi:

- Azure AD oturumu kapatma işlemini gerçekleştirdiyse `/Account/SignedOut` denetleyicinin geri çağrılması için bir OpenID yeniden yönlendirme URI 'si olarak ayarlar
- Openıdconnect ara yazılımların Microsoft Identity Platform `Signout()` `logout` uç noktasıyla iletişim kurmasına izin veren çağrılar:

  - Oturum tanımlama bilgisini tarayıcıdan temizler ve
  - Çağıran, son olarak **oturum kapatma URL**'sini geri çağırır. Bu, varsayılan olarak, ' ın ASP.NET Core bir parçası olarak da sağlanmış olan imzalanmış görünüm sayfası [signeout. html](https://github.com/aspnet/AspNetCore/blob/master/src/Azure/AzureAD/Authentication.AzureAD.UI/src/Areas/AzureAD/Pages/Account/SignedOut.cshtml) ' i görüntüler.

### <a name="intercepting-the-call-to-the-logout-endpoint"></a>`logout` Uç nokta çağrısını kesintiye uğratan

ASP.NET Core openıdconnect ara yazılımı, uygulamanızın adlı `logout` `OnRedirectToIdentityProviderForSignOut`bir openıdconnect olayı sağlayarak Microsoft Identity platform uç noktası çağrısını ele almasını sağlar. Web uygulaması, oturumu kapatma sırasında kullanıcıya sunulacak hesabı Seç iletişim kutusunu kullanmaktan kaçınmaya çalışacak şekilde kullanır. Bu yakama, `AddAzureAdV2Authentication` `Microsoft.Identity.Web` yeniden kullanılabilir kitaplıkta yapılır. Bkz. [StartupHelpers.cs L58-L66](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/blob/b87a1d859ff9f9a4a98eb7b701e6a1128d802ec5/Microsoft.Identity.Web/StartupHelpers.cs#L58-L66)

```CSharp
public static IServiceCollection AddAzureAdV2Authentication(this IServiceCollection services,
                                                            IConfiguration configuration)
{
    ...
    services.Configure<OpenIdConnectOptions>(AzureADDefaults.OpenIdScheme, options =>
    {
        ...
        options.Authority = options.Authority + "/v2.0/";
        ...
        // Attempt to avoid displaying the select account dialog when signing out
        options.Events.OnRedirectToIdentityProviderForSignOut = async context =>
        {
            var user = context.HttpContext.User;
            context.ProtocolMessage.LoginHint = user.GetLoginHint();
            context.ProtocolMessage.DomainHint = user.GetDomainHint();
            await Task.FromResult(0);
        };
    }
}
```

## <a name="aspnet-code"></a>ASP.NET kodu

ASP.NET ' de, oturum kapatma bir denetleyicideki SignOut () yönteminden tetiklenir (örneğin AccountController). Bu yöntem, ASP.NET Framework 'ün bir parçası değildir (ASP.NET Core olduğu gibi) ve zaman uyumsuz olarak kullanmaz ve bu nedenle şunlar olabilir:

- bir OpenID oturum kapatma sınaması gönderir
- önbelleği temizler
- istediği sayfaya yeniden yönlendirir

```CSharp
/// <summary>
/// Send an OpenID Connect sign-out request.
/// </summary>
public void SignOut()
{
 HttpContext.GetOwinContext()
            .Authentication
            .SignOut(CookieAuthenticationDefaults.AuthenticationType);
 MsalAppBuilder.ClearUserTokenCache();
 Response.Redirect("/");
}
```

## <a name="protocol"></a>Protocol

ASP.NET Core veya ASP.NET kullanmak istemiyorsanız, [Açık kimlik Connect](./v2-protocols-oidc.md)'ten erişilebilen protokol belgelerine bakabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Üretime taşı](scenario-web-app-sign-user-production.md)
