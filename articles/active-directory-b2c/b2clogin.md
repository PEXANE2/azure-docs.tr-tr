---
title: Uygulamaları ve API 'Leri b2clogin.com 'e geçirme
titleSuffix: Azure AD B2C
description: Azure Active Directory B2C için yeniden yönlendirme URL 'Lerinde b2clogin.com kullanma hakkında bilgi edinin.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 12/04/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: e4ebde6295c3f3467dd615b58d2140302aa86a61
ms.sourcegitcommit: 5b9287976617f51d7ff9f8693c30f468b47c2141
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/09/2019
ms.locfileid: "74949992"
---
# <a name="set-redirect-urls-to-b2clogincom-for-azure-active-directory-b2c"></a>Azure Active Directory B2C için yeniden yönlendirme URL 'Lerini b2clogin.com olarak ayarlayın

Azure Active Directory B2C (Azure AD B2C) uygulamanızda kaydolma ve oturum açma için bir kimlik sağlayıcısı ayarladığınızda, bir yeniden yönlendirme URL 'SI belirtmeniz gerekir. Artık uygulamalarınızda ve API 'lerinize *login.microsoftonline.com* başvurmamalıdır. Bunun yerine, tüm yeni uygulamalar için *b2clogin.com* kullanın ve mevcut uygulamaları *login.microsoftonline.com* ' den *b2clogin.com*' ye geçirin.

## <a name="deprecation-of-loginmicrosoftonlinecom"></a>Login.microsoftonline.com kullanımdan kaldırma

04 Aralık 2019 ' de, login.microsoftonline.com desteğinin zamanlanan emekliliğinizi Azure AD B2C **04 aralık 2020**tarihinde duyurduk:

[Azure Active Directory B2C kullanımdan kalklogin.microsoftonline.com](https://azure.microsoft.com/updates/b2c-deprecate-msol/)

Login.microsoftonline.com 'ın kullanımdan kaldırılması, 04 Aralık 2020 ' deki tüm Azure AD B2C kiracılar için geçerli olur ve bu da b2clogin.com 'e geçiş için bir (1) yıl. 04 Aralık 2019 ' den sonra oluşturulan yeni kiracılar, login.microsoftonline.com 'dan gelen istekleri kabul etmez. Tüm işlevler, b2clogin.com uç noktasında aynı kalır.

Login.microsoftonline.com 'nin kullanımdan kaldırılması Azure Active Directory kiracıları etkilemez. Bu değişiklikten yalnızca Azure Active Directory B2C kiracılar etkilenir.

## <a name="benefits-of-b2clogincom"></a>B2clogin.com avantajları

Yeniden yönlendirme URL 'SI olarak *b2clogin.com* kullandığınızda:

* Microsoft Hizmetleri tarafından kullanılan tanımlama bilgisi üstbilgisinde tüketilen alan azalır.
* Yeniden yönlendirme URL 'Lerinin artık Microsoft 'a bir başvuru eklemesi gerekmez.
* JavaScript istemci tarafı kodu özelleştirilmiş sayfalarda desteklenir (Şu anda [önizlemede](user-flow-javascript-overview.md)). Güvenlik kısıtlamaları nedeniyle, *login.microsoftonline.com*kullanıyorsanız, JavaScript kodu ve HTML form öğeleri özel sayfalardan kaldırılır.

## <a name="overview-of-required-changes"></a>Gerekli değişikliklere genel bakış

Uygulamalarınızı *b2clogin.com*'e geçirmek için yapmanız gerekebilecek birkaç değişiklik vardır:

* Kimlik sağlayıcınızın uygulamalarındaki yeniden yönlendirme URL 'sini *b2clogin.com*başvurusuna değiştirin.
* Azure AD B2C uygulamalarınızı, Kullanıcı akışında ve belirteç uç noktası başvurularında *b2clogin.com* kullanacak şekilde güncelleştirin.
* [Kullanıcı arabirimi özelleştirmesi](active-directory-b2c-ui-customization-custom-dynamic.md)için CORS ayarlarında tanımladığınız tüm **izin verilen kaynakları** güncelleştirin.

## <a name="change-identity-provider-redirect-urls"></a>Kimlik sağlayıcısı yeniden yönlendirme URL 'Lerini Değiştir

Bir uygulamayı oluşturduğunuz her bir kimlik sağlayıcısının Web sitesinde, *login.microsoftonline.com*yerine `your-tenant-name.b2clogin.com` yeniden yönlendirmek için tüm güvenilen URL 'leri değiştirin.

B2clogin.com yeniden yönlendirme URL 'Leri için kullanabileceğiniz iki biçim vardır. Birincisi, kiracı etki alanı adınızın yerine kiracı KIMLIĞINI (GUID) kullanarak URL 'de herhangi bir yerde "Microsoft" görünmeme avantajını sağlar:

```
https://{your-tenant-name}.b2clogin.com/{your-tenant-id}/oauth2/authresp
```

İkinci seçenek, `your-tenant-name.onmicrosoft.com`biçiminde kiracı etki alanı adınızı kullanır. Örnek:

```
https://{your-tenant-name}.b2clogin.com/{your-tenant-name}.onmicrosoft.com/oauth2/authresp
```

Her iki biçim için:

* Değiştirin `{your-tenant-name}` Azure AD B2C kiracınızın adı.
* URL 'de varsa `/te` kaldırın.

## <a name="update-your-applications-and-apis"></a>Uygulamalarınızı ve API 'lerinizi güncelleştirme

Azure AD B2C özellikli uygulamalardaki ve API 'lerinizde bulunan kod, birkaç yerde `login.microsoftonline.com` başvurabilir. Örneğin, kodunuzun Kullanıcı akışlarına ve belirteç uç noktalarına başvuruları olabilir. `your-tenant-name.b2clogin.com`başvurusunu bunun yerine aşağıdaki şekilde güncelleştirin:

* Yetkilendirme uç noktası
* belirteç uç noktası
* Belirteç veren

Örneğin, contoso kaydolma/oturum açma ilkesi için yetkili uç noktası şu şekilde olacaktır:

```
https://contosob2c.b2clogin.com/00000000-0000-0000-0000-000000000000/B2C_1_signupsignin1
```

OWIN tabanlı Web uygulamalarını b2clogin.com 'e geçirme hakkında bilgi için bkz. [OWIN tabanlı Web API 'sini b2clogin.com 'ye geçirme](multiple-token-endpoints.md).

Azure AD B2C tarafından korunan Azure API Management API ['leri geçirmek için](secure-api-management.md#migrate-to-b2clogincom) , [Azure API Management API 'Sinin Azure AD B2C Ile güvenli hale getirme](secure-api-management.md)bölümüne bakın.

## <a name="microsoft-authentication-library-msal"></a>Microsoft Authentication Library (MSAL)

### <a name="validateauthority-property"></a>ValidateAuthority özelliği

[Msal.net][msal-dotnet] v2 veya daha önceki bir sürümünü kullanıyorsanız, *b2clogin.com*'e yeniden yönlendirmeye Izin vermek için **validateauthority** özelliğini istemci örneklemede `false` olarak ayarlayın. Bu ayar MSAL.NET v3 ve üzeri için gerekli değildir.

```CSharp
ConfidentialClientApplication client = new ConfidentialClientApplication(...); // Can also be PublicClientApplication
client.ValidateAuthority = false; // MSAL.NET v2 and earlier **ONLY**
```

[JavaScript Için msal][msal-js]kullanıyorsanız:

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

OWIN tabanlı Web uygulamalarını b2clogin.com 'e geçirme hakkında bilgi için bkz. [OWIN tabanlı Web API 'sini b2clogin.com 'ye geçirme](multiple-token-endpoints.md).

Azure AD B2C tarafından korunan Azure API Management API ['leri geçirmek için](secure-api-management.md#migrate-to-b2clogincom) , [Azure API Management API 'Sinin Azure AD B2C Ile güvenli hale getirme](secure-api-management.md)bölümüne bakın.

<!-- LINKS - External -->
[msal-dotnet]: https://github.com/AzureAD/microsoft-authentication-library-for-dotnet
[msal-dotnet-b2c]: https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/AAD-B2C-specifics
[msal-js]: https://github.com/AzureAD/microsoft-authentication-library-for-js
[msal-js-b2c]: ../active-directory/develop/msal-b2c-overview.md