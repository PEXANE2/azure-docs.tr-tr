---
title: Yeniden yönlendirme URL 'Lerini b2clogin.com-Azure Active Directory B2C olarak ayarla | Microsoft Docs
description: Azure Active Directory B2C için yeniden yönlendirme URL 'Lerinde b2clogin.com kullanma hakkında bilgi edinin.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 01/28/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 080c1933f88d9e824969a42212de2eacd0f62e14
ms.sourcegitcommit: 13a289ba57cfae728831e6d38b7f82dae165e59d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/09/2019
ms.locfileid: "68927277"
---
# <a name="set-redirect-urls-to-b2clogincom-for-azure-active-directory-b2c"></a>Azure Active Directory B2C için yeniden yönlendirme URL 'Lerini b2clogin.com olarak ayarlayın

Azure Active Directory (Azure AD) B2C uygulamanızda kaydolma ve oturum açma için bir kimlik sağlayıcısı ayarladığınızda, bir yeniden yönlendirme URL 'SI belirtmeniz gerekir. Geçmişte, login.microsoftonline.com kullanıldı, şimdi b2clogin.com kullanıyor olmanız gerekir.

> [!NOTE]
> B2clogin.com içinde JavaScript istemci tarafı kodunu (Şu anda önizleme aşamasında) kullanabilirsiniz. Login.microsoftonline.com kullanıyorsanız, JavaScript kodunuz özel sayfanıza kaldırılır. Özel sayfanıza HTML form öğelerini kaldırma gibi ek güvenlik kısıtlamaları da login.microsoftonline.com ' e uygulanır. 

B2clogin.com kullanmak gibi ek avantajlar sağlar:

- Microsoft Hizmetleri tarafından kullanılan tanımlama bilgisi üstbilgisinde tüketilen alan azalır.
- URL 'niz artık Microsoft 'a bir başvuru içermiyor. Örneğin: `https://your-tenant-name.b2clogin.com/tenant-id/oauth2/authresp`.

> [!NOTE]
> Hem kiracı adını hem de kiracı GUID 'sini aşağıdaki gibi kullanabilirsiniz:
> * `https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com`(hala başvurduğu `onmicrosoft.com`)
> * `https://your-tenant-name.b2clogin.com/your-tenant-guid`(Bu durumda, Microsoft 'a hiçbir başvuru yoktur)
>
> Ancak, Azure Active Directory B2C kiracınız için _özel bir etki alanı_ kullanamazsınız, örn. `https://your-tenant-name.b2clogin.com/your-custom-domain-name` çalışmaz.

B2clogin.com kullanırken değiştirilmesi gerekebilecek şu ayarları göz önünde bulundurun:

- Kimlik sağlayıcısı uygulamalarınızda b2clogin.com kullanmak için yeniden yönlendirme URL 'Lerini ayarlayın. 
- Kullanıcı akış başvuruları ve belirteç uç noktaları için Azure AD B2C uygulamanızı b2clogin.com kullanacak şekilde ayarlayın. 
- MSAL kullanıyorsanız, **Validateauthority** özelliğini olarak `false`ayarlamanız gerekir.
- [Kullanıcı arabirimi özelleştirmesi](active-directory-b2c-ui-customization-custom-dynamic.md)için CORS ayarlarında tanımladığınız **izin verilen kaynakları** değiştirdiğinizden emin olun.  

## <a name="change-redirect-urls"></a>Yeniden yönlendirme URL 'Lerini değiştirme

B2clogin.com 'yi kullanmak için, kimlik sağlayıcısı uygulamanızın ayarlarında, Azure AD B2C yeniden yönlendirileceği güvenilen URL 'Lerin listesini bulup değiştirin.  Şu anda, büyük olasılıkla bazı login.microsoftonline.com sitesine yeniden yönlendirmek üzere ayarlamış olursunuz. 

Yetkilendirilmiş olması için yeniden yönlendirme URL `your-tenant-name.b2clogin.com` 'sini değiştirmeniz gerekir. Azure AD B2C kiracınızın adıyla `your-tenant-name` değiştirdiğinizden emin olun ve URL 'de varsa kaldırın. `/te` Her kimlik sağlayıcısı için bu URL 'nin hafif çeşitlemeleri vardır. bu nedenle, URL 'YI tam olarak almak için ilgili sayfayı kontrol edin.

Aşağıdaki makalelerde kimlik sağlayıcılarının kurulum bilgilerini bulabilirsiniz:

- [Microsoft hesabı](active-directory-b2c-setup-msa-app.md)
- [Facebook](active-directory-b2c-setup-fb-app.md)
- [Google](active-directory-b2c-setup-goog-app.md)
- [Amazon](active-directory-b2c-setup-amzn-app.md)
- [LinkedIn](active-directory-b2c-setup-li-app.md)
- [Twitter](active-directory-b2c-setup-twitter-app.md)
- [GitHub](active-directory-b2c-setup-github-app.md)
- [Weibo](active-directory-b2c-setup-weibo-app.md)
- [QQ](active-directory-b2c-setup-qq-app.md)
- [WeChat](active-directory-b2c-setup-wechat-app.md)
- [Azure AD](active-directory-b2c-setup-oidc-azure-active-directory.md)
- [Özel OıDC](active-directory-b2c-setup-oidc-idp.md)

## <a name="update-your-application"></a>Uygulamanızı güncelleştirme

Azure AD B2C uygulamanız muhtemelen Kullanıcı akışı başvuruları `login.microsoftonline.com` ve belirteç uç noktaları gibi birkaç yerde anlamına gelir.  Yetkilendirme uç noktanızın, belirtecin bitiş noktasının ve verenin kullanılmak `your-tenant-name.b2clogin.com`üzere güncelleştirildiğinden emin olun.  

## <a name="set-the-validateauthority-property"></a>ValidateAuthority özelliğini ayarlama

MSAL kullanıyorsanız, **Validateauthority** özelliğini olarak `false`ayarlayın. **Validateauthority** olarak `false`ayarlandığında, b2clogin.com için yeniden yönlendirmeye izin verilir. 

Aşağıdaki örnek, özelliği nasıl ayarlayabileceğini göstermektedir:

[.Net Için msal](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet)içinde:

```CSharp
 ConfidentialClientApplication client = new ConfidentialClientApplication(...); // can also be PublicClientApplication
 client.ValidateAuthority = false;
```

[JavaScript Için msal](https://github.com/AzureAD/microsoft-authentication-library-for-js)içinde:

```Javascript
this.clientApplication = new UserAgentApplication(
  env.auth.clientId,
  env.auth.loginAuthority,
  this.authCallback.bind(this),
  {
    validateAuthority: false
  }
);
```
