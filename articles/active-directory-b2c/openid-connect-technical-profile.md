---
title: Özel bir ilkede OpenID Connect teknik profilini tanımlama
titleSuffix: Azure AD B2C
description: Azure Active Directory B2C'de özel bir ilke de OpenID Connect teknik profilini tanımlayın.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80332757"
---
# <a name="define-an-openid-connect-technical-profile-in-an-azure-active-directory-b2c-custom-policy"></a>Azure Active Directory B2C özel ilkesinde OpenID Connect teknik profilini tanımlama

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Azure Active Directory B2C (Azure AD B2C), [OpenID Connect](https://openid.net/2015/04/17/openid-connect-certification-program/) iletişim kuralı kimlik sağlayıcısı için destek sağlar. OpenID Connect 1.0, OAuth 2.0'ın üstündeki kimlik katmanını tanımlar ve modern kimlik doğrulama protokollerinde en son teknolojiyi temsil eder. OpenID Connect teknik profiliyle, Azure AD gibi OpenID Connect tabanlı bir kimlik sağlayıcısıyla federate yapabilirsiniz. Bir kimlik sağlayıcısıyla federating kullanıcıların mevcut sosyal veya kurumsal kimlikleri ile oturum açmalarına olanak tanır.

## <a name="protocol"></a>Protokol

**Protokol** öğesinin `OpenIdConnect` **Ad** özniteliğinin . Örneğin, **MSA-OIDC** teknik profili için `OpenIdConnect`protokol:

```XML
<TechnicalProfile Id="MSA-OIDC">
  <DisplayName>Microsoft Account</DisplayName>
  <Protocol Name="OpenIdConnect" />
  ...
```

## <a name="input-claims"></a>Giriş talepleri

**Giriş Talepleri** ve **GirişTalepleri Dönüşümleri** öğeleri gerekli değildir. Ancak, kimlik sağlayıcınıza ek parametreler göndermek isteyebilirsiniz. Aşağıdaki örnek, **domain_hint** yetkilendirme isteğine değeri `contoso.com` yle birlikte domain_hint sorgu dize parametresi ekler.

```XML
<InputClaims>
  <InputClaim ClaimTypeReferenceId="domain_hint" DefaultValue="contoso.com" />
</InputClaims>
```

## <a name="output-claims"></a>Çıktı talepleri

**Çıktılar İçeriği** öğesi, OpenID Connect kimlik sağlayıcısı tarafından döndürülen taleplerin listesini içerir. İlkenizde tanımlanan talep adını kimlik sağlayıcısında tanımlanan adla eşlemeniz gerekebilir. Özniteliği ayarladığınız `DefaultValue` sürece kimlik sağlayıcısı tarafından döndürülen talepleri de ekleyebilirsiniz.

**OutputClaimsTransformations** öğesi, çıktı taleplerini değiştirmek veya yenilerini oluşturmak için kullanılan **OutputClaimsTransformation** öğelerikoleksiyonunu içerebilir.

Aşağıdaki örnek, Microsoft Hesabı kimlik sağlayıcısı tarafından iade edilen talepleri gösterir:

- **İhraççıUserId** iddiasına eşlenen **alt** talep.
- **DisplayName** claim'ineşe eşlenen **ad** iddiası.
- Ad eşlemesiz **e-posta.**

Teknik profil, kimlik sağlayıcısı tarafından döndürülen talepleri de döndürür:

- Kimlik **Sağlayıcı,** kimlik sağlayıcısının adını içeren talepte bulunun.
- **SocialIdpAuthentication** varsayılan değeri **socialIdpAuthentication**ile kimlik doğrulamaKaynak iddiası .

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
| client_id | Evet | Kimlik sağlayıcısının uygulama tanımlayıcısı. |
| IdTokenAudience | Hayır | id_token seyircisi. Belirtilirse, Azure AD B2C, kimlik sağlayıcısı tarafından döndürülen bir belirteçteki `aud` talebin IdTokenAudience meta verilerinde belirtilene eşit olup olmadığını denetler.  |
| Meta veri | Evet | OpenID Connect kimlik sağlayıcısı yapılandırma belgesine işaret eden ve OpenID olarak bilinen yapılandırma bitiş noktası olarak da bilinen URL. URL, kiracı `{tenant}` adı ile değiştirilen ifadeyi içerebilir.  |
| authorization_endpoint | Hayır | OpenID Connect kimlik sağlayıcısı yapılandırma yetkilendirme bitiş noktasını gösteren bir URL. authorization_endpoint meta verilerin değeri, OpenID `authorization_endpoint` iyi bilinen yapılandırma bitiş noktasında belirtilenden önce gelir. URL, kiracı `{tenant}` adı ile değiştirilen ifadeyi içerebilir. |
| yayınlayan | Hayır | OpenID Connect kimlik sağlayıcısının benzersiz tanımlayıcısı. Veren meta verilerin değeri, OpenID `issuer` iyi bilinen yapılandırma bitiş noktasında belirtilenden önce gelir.  Belirtilirse, Azure AD B2C, kimlik sağlayıcısı tarafından döndürülen bir belirteçteki `iss` talebin veren meta verilerinde belirtilene eşit olup olmadığını denetler. |
| Sağlayıcı Adı | Hayır | Kimlik sağlayıcısının adı.  |
| response_types | Hayır | OpenID Connect Core 1.0 belirtimine göre yanıt türü. Olası `id_token`değerler: `code`, `token`, veya . |
| response_mode | Hayır | Kimlik sağlayıcısının sonucu Azure AD B2C'ye geri göndermek için kullandığı yöntem. Olası `query`değerler: `form_post` , (varsayılan) veya `fragment`. |
| scope | Hayır | OpenID Connect Core 1.0 belirtimine göre tanımlanan isteğin kapsamı. Gibi `openid`, `profile`, `email`ve . |
| httpBinding | Hayır | Beklenen HTTP erişim belirteci ve iddia belirteç bitiş noktaları için bağlayıcı. Olası `GET` değerler: `POST`veya .  |
| ValidTokenIssuerÖnefixes | Hayır | Azure Etkin Dizini gibi çok kiracılı bir kimlik sağlayıcısı kullanırken kiracıların her birinde oturum açmada kullanılabilecek bir anahtar. |
| UsePolicyInRedirectUri | Hayır | Yeniden yönlendirme URI'yi yaparken bir ilke kullanıp kullanmayacağını gösterir. Uygulamanızı kimlik sağlayıcısında yapılandırdığınızda, uri yönlendirmesini belirtmeniz gerekir. URI'yi Azure AD B2C'ye yönlendiren noktalar, `https://{your-tenant-name}.b2clogin.com/{your-tenant-name}.onmicrosoft.com/oauth2/authresp`.  Belirtirseniz, `false`kullandığınız her ilke için yeniden yönlendirme URI eklemeniz gerekir. Örneğin: `https://{your-tenant-name}.b2clogin.com/{your-tenant-name}.onmicrosoft.com/{policy-name}/oauth2/authresp`. |
| MarkAsFailureOnStatusCode5xx | Hayır | Http durum kodu 5xx aralığındaysa, harici bir hizmete yapılan isteğin hata olarak işaretlenip işaretlenmemesi gerektiğini gösterir. Varsayılan değer: `false`. |
| DiscoverMetadataByKenIssuer | Hayır | OIDC meta verilerinin JWT belirtecindeki veren kullanılarak bulunup bulunmayacağını gösterir. |
| IncludeClaimResolvingInClaimsHandling  | Hayır | Giriş ve çıktı talepleri [için, talep çözümlübir](claim-resolver-overview.md) çözümünü teknik profilde bulunup bulunmayacağını belirtir. Olası değerler: `true` `false`  , veya (varsayılan). Teknik profilde bir talep çözümleyicisi kullanmak istiyorsanız, bunu ' ya `true`göre ayarlayın. |

### <a name="ui-elements"></a>Kullanıcı arabirimi öğeleri
 
Aşağıdaki ayarlar, hata üzerine görüntülenen hata iletisini yapılandırmak için kullanılabilir. Meta veriler OpenID Connect teknik profilinde yapılandırılmalıdır. Hata iletileri [yerelleştirilebilir.](localization-string-ids.md#sign-up-or-sign-in-error-messages)

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| UserMessageIfClaimsPrincipalDoesExist | Hayır | Sağlanan kullanıcı adı ile bir hesap dizinde bulunamadıysa kullanıcıya görüntülenecek ileti. |
| UserMessageIfGeçersizŞifre | Hayır | Parola yanlışsa kullanıcıya görüntülenecek ileti. |
| UserMessageIfOldPasswordUsed| Hayır |  Eski bir parola kullanılıyorsa kullanıcıya görüntülenecek ileti.|

## <a name="cryptographic-keys"></a>Şifreleme tuşları

**CryptographicKeys** öğesi aşağıdaki özniteliği içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| client_secret | Evet | Kimlik sağlayıcı uygulamasının istemci sırrı. Şifreleme anahtarı yalnızca **response_types** meta veri sayar. `code` Bu durumda, Azure AD B2C, yetkilendirme kodunu bir erişim jetonuyla değiştirmek için başka bir arama yapar. Meta veriler ayarlanmışsa, `id_token` şifreleme anahtarını atlayabilirsiniz.  |

## <a name="redirect-uri"></a>Uri'yi Yönlendirme

Kimlik sağlayıcınızın yeniden yönlendirme URI'sini yapılandırdığınızda, `https://{your-tenant-name}.b2clogin.com/{your-tenant-name}.onmicrosoft.com/oauth2/authresp`girin. Kiracınızın adını `{your-tenant-name}` değiştirdiğinizden emin olun. Yeniden yönlendirme URI tüm küçük olması gerekir.

Örnekler:

- [Özel ilkeler kullanarak microsoft hesabını (MSA) kimlik sağlayıcısı olarak ekleme](identity-provider-microsoft-account-custom.md)
- [Azure AD hesaplarını kullanarak oturum açın](identity-provider-azure-ad-single-tenant-custom.md)
- [Kullanıcıların özel ilkeler kullanarak çok kiracılı bir Azure AD kimlik sağlayıcısında oturum açmalarına izin verme](identity-provider-azure-ad-multi-tenant-custom.md)
