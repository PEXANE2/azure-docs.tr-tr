---
title: Azure Active Directory B2C bir özel ilkede bir OpenID Connect Teknik profili tanımlayın | Microsoft Docs
description: Azure Active Directory B2C bir özel ilkede OpenID Connect Teknik profili tanımlayın.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: b1262d34f93ecbcdb71586fd551d28fde477f92a
ms.sourcegitcommit: f209d0dd13f533aadab8e15ac66389de802c581b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/17/2019
ms.locfileid: "71063936"
---
# <a name="define-an-openid-connect-technical-profile-in-an-azure-active-directory-b2c-custom-policy"></a>Azure Active Directory B2C özel ilkesinde OpenID Connect Teknik profili tanımlama

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Azure Active Directory B2C (Azure AD B2C), [OpenID Connect](https://openid.net/2015/04/17/openid-connect-certification-program/) protokol kimlik sağlayıcısı için destek sağlar. OpenID Connect 1,0, OAuth 2,0 üzerinde bir kimlik katmanını tanımlar ve modern kimlik doğrulama protokollerinde bulunan resimlerin durumunu temsil eder. OpenID Connect Teknik profiliyle birlikte, OpenID Connect tabanlı bir kimlik sağlayıcısıyla (örneğin, Azure AD) federasyona bağlayabilirsiniz. Bir kimlik sağlayıcısı ile federasyona eklemek, kullanıcıların mevcut sosyal veya kurumsal kimliklerinde oturum açmasına olanak tanır.

## <a name="protocol"></a>Protocol

**Protokol** öğesinin `OpenIdConnect` **Name** özniteliğinin olarak ayarlanması gerekir. Örneğin, **MSA-OıDC** Technical profile `OpenIdConnect`için protokol:

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
| client_id | Evet | Kimlik sağlayıcısının uygulama tanımlayıcısı. |
| Idtokenaudience | Hayır | İd_token kitlesi. Belirtilmişse Azure AD B2C, belirtecin kimlik sağlayıcısı tarafından döndürülen bir talep içinde olup olmadığını denetler ve belirtilen değere eşittir. |
| VERIYI | Evet | OpenID Connect bulgu belirtimine göre biçimlendirilmiş bir JSON yapılandırma belgesine işaret eden, iyi bilinen bir OpenID yapılandırma uç noktası olarak da bilinen bir URL. |
| Adı | Hayır | Kimlik sağlayıcısının adı. |
| response_types | Hayır | OpenID Connect Core 1,0 belirtimine göre yanıt türü. Olası değerler: `id_token`, `code`, veya `token`. |
| response_mode | Hayır | Kimlik sağlayıcısının sonucu Azure AD B2C geri göndermek için kullandığı yöntem. Olası değerler: `query`, `form_post` (varsayılan) veya `fragment`. |
| scope | Hayır | OpenID Connect Core 1,0 belirtimine göre tanımlanan isteğin kapsamı. `openid` ,`profile`Ve gibi`email`. |
| HttpBinding | Hayır | Erişim belirtecine ve talep belirteci uç noktalarına beklenen HTTP bağlaması. Olası değerler: `GET` veya `POST`.  |
| Validtokenıssueröneklerini | Hayır | Azure Active Directory gibi çok kiracılı bir kimlik sağlayıcısı kullanırken kiracıların her birinde oturum açmak için kullanılabilen bir anahtar. |
| Usepolicınredirecturi | Hayır | Yeniden yönlendirme URI 'SI oluşturulurken bir ilke kullanılıp kullanılmayacağını belirtir. Uygulamanızı kimlik sağlayıcısında yapılandırdığınızda, yeniden yönlendirme URI 'sini belirtmeniz gerekir. Yeniden yönlendirme URI 'si Azure AD B2C, ( `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` Login.microsoftonline.com, Your-Tenant-Name.b2clogin.com ile değiştirilebilir).  Belirtirseniz `false`, kullandığınız her ilke için bir yeniden yönlendirme URI 'si eklemeniz gerekir. Örneğin: `https://login.microsoftonline.com/te/{tenant}/{policy}/oauth2/authresp` |
| MarkAsFailureOnStatusCode5xx | Hayır | Http durum kodu 5xx aralığalıyorsa, bir dış hizmete yönelik isteğin hata olarak işaretlenip işaretlenmeyeceğini gösterir. Varsayılan, `false` değeridir. |
| Discovermetadatabyıtokenıssuer | Hayır | OıDC meta verilerinin JWT belirtecindeki veren kullanılarak bulunup bulunmadığını gösterir. |

## <a name="cryptographic-keys"></a>Şifreleme anahtarları

**Cryptographickeys** öğesi aşağıdaki özniteliği içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| client_secret | Evet | Kimlik sağlayıcısı uygulamasının istemci gizli anahtarı. Şifreleme anahtarı yalnızca **response_types** meta verileri olarak `code`ayarlandıysa gereklidir. Bu durumda Azure AD B2C, bir erişim belirtecinin yetkilendirme kodunu Exchange için başka bir çağrı yapar. Meta veriler olarak `id_token` ayarlandıysa, şifreleme anahtarını atlayabilirsiniz.  |

## <a name="redirect-uri"></a>Yeniden yönlendirme URI 'Si

Kimlik sağlayıcınızın yeniden yönlendirme URI 'sini yapılandırdığınızda, girin `https://login.microsoftonline.com/te/tenant/oauth2/authresp`. **Kiracıyı** kiracınızın adı (örneğin, contosob2c.onmicrosoft.com) veya KIRACıNıN kimliği ile değiştirdiğinizden emin olun. Yeniden yönlendirme URI 'sinin tamamen küçük harfle olması gerekir.

**Login.microsoftonline.com** yerine **b2clogin.com** etki alanını kullanıyorsanız Login.microsoftonline.com yerine b2clogin.com kullandığınızdan emin olun.

Örnekler:

- [Özel ilkeler kullanarak bir kimlik sağlayıcısı olarak Microsoft hesabı (MSA) ekleme](active-directory-b2c-custom-setup-msa-idp.md)
- [Azure AD hesaplarını kullanarak oturum açın](active-directory-b2c-setup-aad-custom.md)
- [Kullanıcıların özel ilkeleri kullanarak çok kiracılı bir Azure AD kimlik sağlayıcısında oturum açmalarına izin ver](active-directory-b2c-setup-commonaad-custom.md)

 














