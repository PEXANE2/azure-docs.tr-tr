---
title: Uygulamaları ve API'leri b2clogin.com geçirin
titleSuffix: Azure AD B2C
description: Azure Active Directory B2C için yönlendirme URL'lerinizde b2clogin.com kullanma hakkında bilgi edinin.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 12/04/2019
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 64b440054795670b99a22e37dec7188f3e1cd74c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78189999"
---
# <a name="set-redirect-urls-to-b2clogincom-for-azure-active-directory-b2c"></a>Azure Active Directory B2C için b2clogin.com yönlendirme URL'lerini ayarlama

Azure Active Directory B2C (Azure AD B2C) uygulamanızda kaydolmak ve oturum açmak için bir kimlik sağlayıcısı ayarladığınızda, yeniden yönlendirme URL'si belirtmeniz gerekir. Artık uygulamalarınızda ve API'lerinizde *login.microsoftonline.com* başvurmamalısınız. Bunun yerine, tüm yeni uygulamalar için *b2clogin.com* kullanın ve varolan uygulamaları *login.microsoftonline.com'den b2clogin.com'a*geçirin. *login.microsoftonline.com*

## <a name="deprecation-of-loginmicrosoftonlinecom"></a>login.microsoftonline.com amortismanı

04 Aralık 2019 tarihinde, **04 Aralık 2020**tarihinde Azure AD B2C'de login.microsoftonline.com desteğinin planlanan emekliliğini duyurduk:

[Azure Active Directory B2C login.microsoftonline.com](https://azure.microsoft.com/updates/b2c-deprecate-msol/)

login.microsoftonline.com amortismanı 04 Aralık 2020'de tüm Azure AD B2C kiracıları için yürürlüğe girerek mevcut kiracılara b2clogin.com'a geçiş için bir (1) yıl sağlar. 04 Aralık 2019 tarihinden sonra oluşturulan yeni kiracılar login.microsoftonline.com gelen talepleri kabul etmeyecektir. Tüm işlevler b2clogin.com bitiş noktasında aynı kalır.

login.microsoftonline.com amortismanı Azure Etkin Dizin kiracılarını etkilemez. Bu değişiklikten yalnızca Azure Etkin Dizin B2C kiracıları etkilenir.

## <a name="benefits-of-b2clogincom"></a>b2clogin.com Faydaları

*b2clogin.com* yönlendirme URL'niz olarak kullandığınızda:

* Microsoft hizmetleri tarafından çerez üstbilgisinde tüketilen alan azalır.
* Yönlendirme URL'lerinizin artık Microsoft'a başvuruda bulunmalarına gerek yoktur.
* JavaScript istemci tarafı kodu özelleştirilmiş sayfalarda (şu anda [önizlemede)](user-flow-javascript-overview.md)desteklenir. Güvenlik kısıtlamaları nedeniyle, *login.microsoftonline.com*kullanıyorsanız JavaScript kodu ve HTML form öğeleri özel sayfalardan kaldırılır.

## <a name="overview-of-required-changes"></a>Gerekli değişikliklere genel bakış

Uygulamalarınızı *b2clogin.com*geçirmek için yapmanız gereken birkaç değişiklik vardır:

* Kimlik sağlayıcınızın uygulamalarındaki yönlendirme URL'sini *başvuru b2clogin.com*olarak değiştirin.
* Kullanıcı akışında ve belirteç uç nokta başvurularında *b2clogin.com* kullanmak için Azure AD B2C uygulamalarınızı güncelleştirin.
* [Kullanıcı arabirimi özelleştirmesi](custom-policy-ui-customization.md)için CORS ayarlarında tanımladığınız **İzin Verilen Kökenleri** güncelleştirin.

## <a name="change-identity-provider-redirect-urls"></a>Kimlik sağlayıcısının URL'leri yeniden yönlendirmesi değiştirme

Bir uygulama oluşturduğunuz her kimlik sağlayıcısının web sitesinde, güvenilen tüm URL'leri `your-tenant-name.b2clogin.com` *login.microsoftonline.com*yerine yeniden yönlendirmek için değiştirin.

URL'leri yeniden yönlendirmek için b2clogin.com kullanabileceğiniz iki biçim vardır. Bunlardan ilki, kiracı alan adınızın yerine Kiracı Kimliği'ni (GUID) kullanarak URL'nin herhangi bir yerinde "Microsoft"un görünmesini sağlamama avantajı sağlar:

```
https://{your-tenant-name}.b2clogin.com/{your-tenant-id}/oauth2/authresp
```

İkinci seçenek, kiracı etki alanı adınızı `your-tenant-name.onmicrosoft.com`. Örnek:

```
https://{your-tenant-name}.b2clogin.com/{your-tenant-name}.onmicrosoft.com/oauth2/authresp
```

Her iki biçim için:

* Azure `{your-tenant-name}` AD B2C kiracınızın adıyla değiştirin.
* URL'de varsa kaldırın. `/te`

## <a name="update-your-applications-and-apis"></a>Uygulamalarınızı ve API'lerinizi güncelleştirin

Azure AD B2C özellikli uygulamalarınızdaki ve API'lerinizdeki kod çeşitli yerlerde ifade `login.microsoftonline.com` edilebilir. Örneğin, kodunuzda kullanıcı akışlarına ve belirteç uç noktalarına başvurular olabilir. Bunun yerine referans `your-tenant-name.b2clogin.com`için aşağıdaki güncelleştirme:

* Yetkilendirme bitiş noktası
* Belirteç bitiş noktası
* Belirteç veren

Örneğin, Contoso'nun kaydolma/kaydolma ilkesinin yetki bitiş noktası şu olacaktır:

```
https://contosob2c.b2clogin.com/00000000-0000-0000-0000-000000000000/B2C_1_signupsignin1
```

OWIN tabanlı web uygulamalarını b2clogin.com geçirme hakkında bilgi için, [b2clogin.com için OWIN tabanlı bir web API'sini geçir'e](multiple-token-endpoints.md)bakın.

Azure AD B2C tarafından korunan Azure API Yönetimi API'lerini geçirmek için, [Azure AD B2C ile güvenli bir Azure API Yönetimi API'sinin](secure-api-management.md)b2clogin.com [bölümüne](secure-api-management.md#migrate-to-b2clogincom) geçiş bölümüne bakın.

## <a name="microsoft-authentication-library-msal"></a>Microsoft Authentication Library (MSAL)

### <a name="validateauthority-property"></a>DoğrulamaYetki özelliği

[V2][msal-dotnet] veya daha önce MSAL.NET kullanıyorsanız, yönlendirmelerin `false` *b2clogin.com'a*yönlendirilmesine izin vermek için Doğrulama **Özelliğini** istemci anında ayarlayın. Bu ayar MSAL.NET v3 ve üzeri için gerekli değildir.

```csharp
ConfidentialClientApplication client = new ConfidentialClientApplication(...); // Can also be PublicClientApplication
client.ValidateAuthority = false; // MSAL.NET v2 and earlier **ONLY**
```

[JavaScript için MSAL][msal-js]kullanıyorsanız:

```JavaScript
this.clientApplication = new UserAgentApplication(
  env.auth.clientId,
  env.auth.loginAuthority,
  this.authCallback.bind(this),
  {
    validateAuthority: false
  }
);
```

## <a name="next-steps"></a>Sonraki adımlar

OWIN tabanlı web uygulamalarını b2clogin.com geçirme hakkında bilgi için, [b2clogin.com için OWIN tabanlı bir web API'sini geçir'e](multiple-token-endpoints.md)bakın.

Azure AD B2C tarafından korunan Azure API Yönetimi API'lerini geçirmek için, [Azure AD B2C ile güvenli bir Azure API Yönetimi API'sinin](secure-api-management.md)b2clogin.com [bölümüne](secure-api-management.md#migrate-to-b2clogincom) geçiş bölümüne bakın.

<!-- LINKS - External -->
[msal-dotnet]: https://github.com/AzureAD/microsoft-authentication-library-for-dotnet
[msal-dotnet-b2c]: https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/AAD-B2C-specifics
[msal-js]: https://github.com/AzureAD/microsoft-authentication-library-for-js
[msal-js-b2c]: ../active-directory/develop/msal-b2c-overview.md
