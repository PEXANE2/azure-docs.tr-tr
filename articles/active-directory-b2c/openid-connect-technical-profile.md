---
title: Özel ilkede bir OpenID Connect Teknik profili tanımlama
titleSuffix: Azure AD B2C
description: Azure Active Directory B2C bir özel ilkede OpenID Connect Teknik profili tanımlayın.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 03/26/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: e259a57a9cd6b24362862ffd6cb738157ca912d5
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80332757"
---
# <a name="define-an-openid-connect-technical-profile-in-an-azure-active-directory-b2c-custom-policy"></a>Azure Active Directory B2C özel ilkesinde OpenID Connect Teknik profili tanımlama

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Azure Active Directory B2C (Azure AD B2C), [OpenID Connect](https://openid.net/2015/04/17/openid-connect-certification-program/) protokol kimlik sağlayıcısı için destek sağlar. OpenID Connect 1,0, OAuth 2,0 üzerinde bir kimlik katmanını tanımlar ve modern kimlik doğrulama protokollerinde bulunan resimlerin durumunu temsil eder. OpenID Connect Teknik profiliyle birlikte, OpenID Connect tabanlı bir kimlik sağlayıcısıyla (örneğin, Azure AD) federasyona bağlayabilirsiniz. Bir kimlik sağlayıcısı ile federasyona eklemek, kullanıcıların mevcut sosyal veya kurumsal kimliklerinde oturum açmasına olanak tanır.

## <a name="protocol"></a>Protokol

**Protokol** öğesinin `OpenIdConnect` **Name** özniteliğinin olarak ayarlanması gerekir. Örneğin, **MSA-OıDC** Technical profile için protokol `OpenIdConnect`:

```XML
<TechnicalProfile Id="MSA-OIDC">
  <DisplayName>Microsoft Account</DisplayName>
  <Protocol Name="OpenIdConnect" />
  ...
```

## <a name="input-claims"></a>Giriş talepleri

**Inputclaim** ve **ınputclaimstransformations** öğeleri gerekli değildir. Ancak kimlik sağlayıcınıza ek parametreler göndermek isteyebilirsiniz. Aşağıdaki örnek, değerini `contoso.com` yetkilendirme isteğine **domain_hint** sorgu dizesi parametresini ekler.

```XML
<InputClaims>
  <InputClaim ClaimTypeReferenceId="domain_hint" DefaultValue="contoso.com" />
</InputClaims>
```

## <a name="output-claims"></a>Çıkış talepleri

**Outputclaim** öğesi, OpenID Connect kimlik sağlayıcısı tarafından döndürülen taleplerin bir listesini içerir. İlkenizde tanımlanan talebin adını kimlik sağlayıcısında tanımlanan adla eşlemeniz gerekebilir. `DefaultValue` Özniteliği ayarladığınız sürece, kimlik sağlayıcısı tarafından döndürülmeyen talepleri de ekleyebilirsiniz.

**Outputclaimstransformations** öğesi, çıkış taleplerini değiştirmek veya yenilerini oluşturmak için kullanılan bir **outputclaimstransreference** öğeleri koleksiyonu içerebilir.

Aşağıdaki örnekte, Microsoft hesabı kimlik sağlayıcısı tarafından döndürülen talepler gösterilmektedir:

- **Issueruserıd** talebine eşlenen **Sub** talebi.
- **DisplayName** talebine eşlenen **ad** talebi.
- Ad eşleştirmesi olmayan **e-posta** .

Teknik profil, kimlik sağlayıcısı tarafından döndürülmeyen talepleri de döndürür:

- Kimlik sağlayıcısının adını içeren **IdentityProvider** talebi.
- Varsayılan bir **Socialidpauthentication**değeri olan **authenticationsource** talebi.

```xml
<OutputClaims>
  <OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="live.com" />
  <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="socialIdpAuthentication" />
  <OutputClaim ClaimTypeReferenceId="issuerUserId" PartnerClaimType="sub" />
  <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="name" />
  <OutputClaim ClaimTypeReferenceId="email" />
</OutputClaims>
```

## <a name="metadata"></a>Meta Veriler

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| client_id | Yes | Kimlik sağlayıcısının uygulama tanımlayıcısı. |
| Idtokenaudience | Hayır | İd_token kitlesi. Belirtilmişse, kimlik sağlayıcısı tarafından döndürülen bir `aud` belirteçte talebin ıdtokenaudience meta verilerinde belirtilen değere eşit olup olmadığını denetler Azure AD B2C.  |
| VERIYI | Yes | OpenID ile bilinen yapılandırma uç noktası olarak da bilinen bir OpenID Connect kimlik sağlayıcısı yapılandırma belgesine işaret eden bir URL. URL, kiracı adıyla değiştirilmiş `{tenant}` olan ifadesini içerebilir.  |
| authorization_endpoint | Hayır | Bir OpenID Connect kimlik sağlayıcısı yapılandırma yetkilendirmesi uç noktasına işaret eden bir URL. Authorization_endpoint meta veri değeri, OpenID iyi bilinen `authorization_endpoint` yapılandırma uç noktasında belirtilen değere göre önceliklidir. URL, kiracı adıyla değiştirilmiş `{tenant}` olan ifadesini içerebilir. |
| yayınlayan | Hayır | Bir OpenID Connect kimlik sağlayıcısının benzersiz tanımlayıcısı. Verenin meta verilerinin değeri, OpenID iyi bilinen `issuer` yapılandırma uç noktasında belirtilen değere göre önceliklidir.  Belirtilmişse, kimlik sağlayıcısı tarafından döndürülen bir `iss` belirteçte talebin, verenin meta verilerinde belirtilen değere eşit olup olmadığını denetler Azure AD B2C. |
| Adı | Hayır | Kimlik sağlayıcısının adı.  |
| response_types | Hayır | OpenID Connect Core 1,0 belirtimine göre yanıt türü. Olası değerler: `id_token`, `code`, veya `token`. |
| response_mode | Hayır | Kimlik sağlayıcısının sonucu Azure AD B2C geri göndermek için kullandığı yöntem. Olası değerler: `query`, `form_post` (varsayılan) veya `fragment`. |
| scope | Hayır | OpenID Connect Core 1,0 belirtimine göre tanımlanan isteğin kapsamı. `openid`, `profile`Ve `email`gibi. |
| HttpBinding | Hayır | Erişim belirtecine ve talep belirteci uç noktalarına beklenen HTTP bağlaması. Olası değerler: `GET` veya `POST`.  |
| Validtokenıssueröneklerini | Hayır | Azure Active Directory gibi çok kiracılı bir kimlik sağlayıcısı kullanırken kiracıların her birinde oturum açmak için kullanılabilen bir anahtar. |
| Usepolicınredirecturi | Hayır | Yeniden yönlendirme URI 'SI oluşturulurken bir ilke kullanılıp kullanılmayacağını belirtir. Uygulamanızı kimlik sağlayıcısında yapılandırdığınızda, yeniden yönlendirme URI 'sini belirtmeniz gerekir. Yeniden yönlendirme URI 'SI Azure AD B2C, ' `https://{your-tenant-name}.b2clogin.com/{your-tenant-name}.onmicrosoft.com/oauth2/authresp`a işaret eder.  Belirtirseniz `false`, kullandığınız her ilke için bir yeniden yönlendirme URI 'si eklemeniz gerekir. Örneğin: `https://{your-tenant-name}.b2clogin.com/{your-tenant-name}.onmicrosoft.com/{policy-name}/oauth2/authresp`. |
| MarkAsFailureOnStatusCode5xx | Hayır | Http durum kodu 5xx aralığalıyorsa, bir dış hizmete yönelik isteğin hata olarak işaretlenip işaretlenmeyeceğini gösterir. Varsayılan değer: `false`. |
| Discovermetadatabyıtokenıssuer | Hayır | OıDC meta verilerinin JWT belirtecindeki veren kullanılarak bulunup bulunmadığını gösterir. |
| IncludeClaimResolvingInClaimsHandling  | Hayır | Giriş ve çıkış talepleri için, [talep çözümlemenin](claim-resolver-overview.md) teknik profile dahil edilip edilmeyeceğini belirtir. Olası değerler: `true`, veya `false`  (varsayılan). Teknik profilde bir talep çözümleyici kullanmak istiyorsanız, bunu olarak `true`ayarlayın. |

### <a name="ui-elements"></a>Kullanıcı arabirimi öğeleri
 
Hata durumunda görüntülenecek hata iletisini yapılandırmak için aşağıdaki ayarlar kullanılabilir. Meta verilerin OpenID Connect Teknik profilinde yapılandırılması gerekir. Hata iletileri [yerelleştirilebilecek](localization-string-ids.md#sign-up-or-sign-in-error-messages).

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| UserMessageIfClaimsPrincipalDoesNotExist | Hayır | Dizinde, girilen kullanıcı adına sahip bir hesap bulunmazsa kullanıcıya görüntülenecek ileti. |
| Usermessageifınvalidpassword | Hayır | Parola yanlışsa kullanıcıya görüntülenecek ileti. |
| UserMessageIfOldPasswordUsed| Hayır |  Eski bir parola kullanılıyorsa kullanıcıya görüntülenecek ileti.|

## <a name="cryptographic-keys"></a>Şifreleme anahtarları

**Cryptographickeys** öğesi aşağıdaki özniteliği içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| client_secret | Yes | Kimlik sağlayıcısı uygulamasının istemci gizli anahtarı. Şifreleme anahtarı yalnızca **response_types** meta verileri olarak `code`ayarlanmışsa gereklidir. Bu durumda Azure AD B2C, bir erişim belirtecinin yetkilendirme kodunu Exchange için başka bir çağrı yapar. Meta veriler olarak `id_token` ayarlandıysa, şifreleme anahtarını atlayabilirsiniz.  |

## <a name="redirect-uri"></a>Yeniden yönlendirme URI 'Si

Kimlik sağlayıcınızın yeniden yönlendirme URI 'sini yapılandırdığınızda, girin `https://{your-tenant-name}.b2clogin.com/{your-tenant-name}.onmicrosoft.com/oauth2/authresp`. Öğesini kiracınızın adıyla `{your-tenant-name}` değiştirdiğinizden emin olun. Yeniden yönlendirme URI 'sinin tamamen küçük harfle olması gerekir.

Örnekler:

- [Özel ilkeler kullanarak bir kimlik sağlayıcısı olarak Microsoft hesabı (MSA) ekleme](identity-provider-microsoft-account-custom.md)
- [Azure AD hesaplarını kullanarak oturum açın](identity-provider-azure-ad-single-tenant-custom.md)
- [Kullanıcıların özel ilkeleri kullanarak çok kiracılı bir Azure AD kimlik sağlayıcısında oturum açmalarına izin ver](identity-provider-azure-ad-multi-tenant-custom.md)
