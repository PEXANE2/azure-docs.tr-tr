---
title: Özel ilkelerde çözümleyicileri talep edin
titleSuffix: Azure AD B2C
description: Azure Active Directory B2C'de özel bir ilkedeki talep çözümleyicilerini nasıl kullanacağınızı öğrenin.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 04/21/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 0bdede482b79c82e6e05b1429cb7c17399bc2277
ms.sourcegitcommit: 31e9f369e5ff4dd4dda6cf05edf71046b33164d3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81756616"
---
# <a name="about-claim-resolvers-in-azure-active-directory-b2c-custom-policies"></a>Azure Active Directory B2C özel ilkelerindeki talep çözümleyicileri hakkında

Azure Active Directory B2C (Azure AD B2C) özel ilkelerindeki talep [çözümleyicileri,](custom-policy-overview.md) ilke adı, istek korelasyon kimliği, kullanıcı arabirimi dili ve daha fazlası gibi bir yetkilendirme isteği hakkında bağlam bilgileri sağlar.

Bir giriş veya çıktı talebinde bir talep çözümleyicisi kullanmak için, [ClaimsSchema](claimsschema.md) öğesi altında bir string **ClaimType**tanımlarsınız ve sonra **varsayılan değeri** giriş veya çıktı talep öğesindeki talep çözümleyicisine ayarlarsınız. Azure AD B2C, talep çözümleyicisinin değerini okur ve teknik profildeki değeri kullanır.

Aşağıdaki örnekte, adlı `correlationId` bir talep türü Veri `string` **Türü** ile tanımlanır.

```XML
<ClaimType Id="correlationId">
  <DisplayName>correlationId</DisplayName>
  <DataType>string</DataType>
  <UserHelpText>Request correlation Id</UserHelpText>
</ClaimType>
```

Teknik profilde, talep çözümleyicisini talep türüyle eşle. Azure AD B2C, talep çözümleyicisinin `{Context:CorrelationId}` değerini `correlationId` talepte yer le doldurur ve talebi teknik profile gönderir.

```XML
<InputClaim ClaimTypeReferenceId="correlationId" DefaultValue="{Context:CorrelationId}" />
```

## <a name="claim-resolver-types"></a>Talep çözümleyici türleri

Aşağıdaki bölümlerde kullanılabilir talep çözümleyicileri listelenmektedir.

### <a name="culture"></a>Kültür

| İste | Açıklama | Örnek |
| ----- | ----------- | --------|
| {Kültür:Dil Adı} | Dil için iki harfLI ISO kodu. | tr |
| {Kültür:LCID}   | Dil kodunun LCID'si. | 1033 |
| {Kültür:Bölge Adı} | Bölge için iki harfLI ISO kodu. | ABD |
| {Kültür:RFC5646} | RFC5646 dil kodu. | tr-TR |

### <a name="policy"></a>İlke

| İste | Açıklama | Örnek |
| ----- | ----------- | --------|
| {İlke:PolicyId} | Güvenen parti ilkesi adı. | B2C_1A_signup_signin |
| {İlke:RelyingPartyTenantId} | Güvenilen parti ilkesinin kiracı kimliği. | your-tenant.onmicrosoft.com |
| {İlke:TenantObjectid} | Kiracı, güvenilen parti ilkesinin kimliğine itiraz etti. | 00000000-0000-0000-0000-000000000000 |
| {İlke:TrustFrameworkTenantid} | Güven çerçevesinin kiracı kimliği. | your-tenant.onmicrosoft.com |

### <a name="openid-connect"></a>OpenID Connect

| İste | Açıklama | Örnek |
| ----- | ----------- | --------|
| {OIDC:Kimlik DoğrulamaBağlam Başvuruları} |Sorgu `acr_values` dize parametresi. | Yok |
| {OIDC:ClientId} |Sorgu `client_id` dize parametresi. | 00000000-0000-0000-0000-000000000000 |
| {OIDC:DomainHint} |Sorgu `domain_hint` dize parametresi. | facebook.com |
| {OIDC:LoginHint} |  Sorgu `login_hint` dize parametresi. | someone@contoso.com |
| {OIDC:Maxage} | `max_age`. | Yok |
| {OIDC:Nonce} |Sorgu `Nonce` dize parametresi. | defaultNonce |
| {OIDC:Şifre}| Kaynak sahibi nin parola kimlik bilgileri kullanıcının parolasını [akar.](ropc-custom.md)| şifre1| 
| {OIDC:İstem} | Sorgu `prompt` dize parametresi. | oturum aç |
| {OIDC:RedirectUri} |Sorgu `redirect_uri` dize parametresi. | https://jwt.ms |
| {OIDC:Kaynak} |Sorgu `resource` dize parametresi. | Yok |
| {OIDC:Kapsam} |Sorgu `scope` dize parametresi. | Openıd |
| {OIDC:Kullanıcı adı}| Kaynak sahibi nin parola kimlik bilgileri kullanıcının kullanıcı adı [akışı.](ropc-custom.md)| emily@contoso.com| 

### <a name="context"></a>Bağlam

| İste | Açıklama | Örnek |
| ----- | ----------- | --------|
| {Bağlam:Yapı Numarası} | Kimlik Deneyimi Çerçevesi sürümü (yapı numarası).  | 1.0.507.0 |
| {Bağlam:BağınlaşimiD} | Korelasyon kimliği.  | 00000000-0000-0000-0000-000000000000 |
| {Bağlam:DateTimeInUtc} |UTC'deki tarih saati.  | 10.10.2018 12:00:00 |
| {Bağlam:DeploymentMode} |İlke dağıtım modu.  | Üretim |
| {Bağlam:IPAddress} | Kullanıcı IP adresi. | 11.111.111.11 |
| {Bağlam:KMSI} | Beni beklet onay kutusunda [oturum açıp tutmanın](custom-policy-keep-me-signed-in.md) seçili olup olmadığını gösterir. |  true |

### <a name="claims"></a>Talepler 

| İste | Açıklama | Örnek |
| ----- | ----------- | --------|
| {Talep:talep türü} | İlke dosyasındaki veya üst ilke dosyasındaki ClaimsSchema bölümünde zaten tanımlanmış bir talep türünün tanımlayıcısı.  Örneğin: `{Claim:displayName}`, `{Claim:objectId}`veya . | Talep türü değeri.|


### <a name="oauth2-key-value-parameters"></a>OAuth2 anahtar değer parametreleri

OIDC veya OAuth2 isteğinin bir parçası olarak yer alan herhangi bir parametre adı, kullanıcı yolculuğundaki bir hak talebine eşlenebilir. Örneğin, uygulamadan gelen `app_session`istek, bir adı olan , veya `loyalty_number`herhangi bir özel sorgu dizesi içeren bir sorgu dize parametresi içerebilir.

| İste | Açıklama | Örnek |
| ----- | ----------------------- | --------|
| {OAUTH-KV:campaignId} | Sorgu dize parametresi. | Hawaii |
| {OAUTH-KV:app_session} | Sorgu dize parametresi. | A3C5R |
| {OAUTH-KV:loyalty_number} | Sorgu dize parametresi. | 1234 |
| {OAUTH-KV:herhangi bir özel sorgu dizesi} | Sorgu dize parametresi. | Yok |

### <a name="oauth2"></a>OAuth2

| İste | Açıklama | Örnek |
| ----- | ----------------------- | --------|
| {oauth2:access_token} | Erişim jetonu. | Yok |


### <a name="saml"></a>SAML

| İste | Açıklama | Örnek |
| ----- | ----------- | --------|
| {SAML:AuthnContextClassReferences} | SAML `AuthnContextClassRef` isteğinden öğe değeri. | vazo:vaha:isimler:tc:SAML:2.0:ac:sınıflar:PasswordProtectedTransport |
| {SAML:NameIdPolicyFormat} | SAML `Format` isteğiöğesinden `NameIDPolicy` öznitelik. | urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress |
| {SAML:Veren} |  SAML `Issuer` isteğinin SAML öğesi değeri.| `https://contoso.com` |
| {SAML:AllowCreate} | SAML isteğinin `AllowCreate` `NameIDPolicy` öğesinden öznitelik değeri. | True |
| {SAML:ForceAuthn} | SAML isteğinin `ForceAuthN` `AuthnRequest` öğesinden öznitelik değeri. | True |
| {SAML:Sağlayıcı Adı} | SAML isteğinin `ProviderName` `AuthnRequest` öğesinden öznitelik değeri.| Contoso.com |
| {SAML:RelayState} | Sorgu `RelayState` dize parametresi.| 

## <a name="using-claim-resolvers"></a>Talep çözümleyicilerini kullanma

Aşağıdaki öğeleri kullanarak talep çözümleyicileri kullanabilirsiniz:

| Öğe | Öğe | Ayarlar |
| ----- | ----------------------- | --------|
|Uygulama Öngörüleri teknik profili |`InputClaim` | |
|[Azure Active Directory](active-directory-technical-profile.md) teknik profili| `InputClaim`, `OutputClaim`| 1, 2|
|[OAuth2](oauth2-technical-profile.md) teknik profili| `InputClaim`, `OutputClaim`| 1, 2|
|[OpenID Connect](openid-connect-technical-profile.md) teknik profili| `InputClaim`, `OutputClaim`| 1, 2|
|[Talep dönüşümü](claims-transformation-technical-profile.md) teknik profili| `InputClaim`, `OutputClaim`| 1, 2|
|[RESTful sağlayıcı](restful-technical-profile.md) teknik profili| `InputClaim`| 1, 2|
|[SAML2](saml-technical-profile.md) teknik profili| `OutputClaim`| 1, 2|
|[Kendinden Sorumlu](self-asserted-technical-profile.md) Teknik Profil| `InputClaim`, `OutputClaim`| 1, 2|
|[Contentdefinition](contentdefinitions.md)| `LoadUri`| |
|[İçerikTanım Parametreleri](relyingparty.md#contentdefinitionparameters)| `Parameter` | |
|[RelyingParty](relyingparty.md#technicalprofile) teknik profili| `OutputClaim`| 2 |

Ayarlar:
1. Meta `IncludeClaimResolvingInClaimsHandling` veriler ' e `true`ayarlanmalıdır.
1. Giriş veya çıktı talepleri `AlwaysUseDefaultValue` özniteliği ' `true`ne ayarlanmalıdır.

## <a name="claim-resolvers-samples"></a>Çözümleyiciler örnekleri talep edin

### <a name="restful-technical-profile"></a>RESTful teknik profili

Yeniden [kullanımlı](restful-technical-profile.md) bir teknik profilde, kullanıcı dilini, ilke adını, kapsamı ve istemci kimliğini göndermek isteyebilirsiniz. İddiaları temel alan REST API özel iş mantığı çalıştırabilir ve gerekirse yerelleştirilmiş bir hata iletisi yükseltir.

Aşağıdaki örnekte, bu senaryoya sahip yeni bir teknik profil gösterilmektedir:

```XML
<TechnicalProfile Id="REST">
  <DisplayName>Validate user input data and return loyaltyNumber claim</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  <Metadata>
    <Item Key="ServiceUrl">https://your-app.azurewebsites.net/api/identity</Item>
    <Item Key="AuthenticationType">None</Item>
    <Item Key="SendClaimsIn">Body</Item>
    <Item Key="IncludeClaimResolvingInClaimsHandling">true</Item>
  </Metadata>
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="userLanguage" DefaultValue="{Culture:LCID}" AlwaysUseDefaultValue="true" />
    <InputClaim ClaimTypeReferenceId="policyName" DefaultValue="{Policy:PolicyId}" AlwaysUseDefaultValue="true" />
    <InputClaim ClaimTypeReferenceId="scope" DefaultValue="{OIDC:Scope}" AlwaysUseDefaultValue="true" />
    <InputClaim ClaimTypeReferenceId="clientId" DefaultValue="{OIDC:ClientId}" AlwaysUseDefaultValue="true" />
  </InputClaims>
  <UseTechnicalProfileForSessionManagement ReferenceId="SM-Noop" />
</TechnicalProfile>
```

### <a name="direct-sign-in"></a>Doğrudan oturum açma

Talep çözümleyicilerini kullanarak, oturum açma adını veya doğrudan oturum açma oturum açmayı Facebook, LinkedIn veya Microsoft hesabı gibi belirli bir sosyal kimlik sağlayıcısına önceden doldurabilirsiniz. Daha fazla bilgi için bkz: [Azure Active Directory B2C'yi kullanarak doğrudan oturum açma'yı ayarla.](direct-signin.md)

### <a name="dynamic-ui-customization"></a>Dinamik Kullanıcı Bira Özelleştirme

Azure AD B2C, sayfa içeriğini dinamik olarak işlemek için sorgu dize parametrelerini HTML içerik tanımı uç noktalarınıza geçirmenizi sağlar. Örneğin, bu özellik, web veya mobil uygulamanızdan geçtiğiniz özel bir parametreye göre Azure AD B2C kaydolma veya kaydolma sayfasındaki arka plan görüntüsünü değiştirme olanağı sağlar. Daha fazla bilgi için bkz: [Azure Etkin Dizin I2C'de özel ilkeler kullanarak Kullanıcı Arabirimi'ni dinamik olarak yapılandırın.](custom-policy-ui-customization.md#configure-dynamic-custom-page-content-uri) Ayrıca HTML sayfanızı bir dil parametresini temel alınca yerelleştirebilir veya istemci kimliğine göre içeriği değiştirebilirsiniz.

Aşağıdaki örnek, **campaignId** adlı sorgu dize parametresinde `Hawaii`, `en-US`bir **dil** kodu ve istemci kimliğini temsil eden **uygulama** değeriyle geçer:

```XML
<UserJourneyBehaviors>
  <ContentDefinitionParameters>
    <Parameter Name="campaignId">{OAUTH-KV:campaignId}</Parameter>
    <Parameter Name="language">{Culture:RFC5646}</Parameter>
    <Parameter Name="app">{OIDC:ClientId}</Parameter>
  </ContentDefinitionParameters>
</UserJourneyBehaviors>
```

Sonuç olarak, Azure AD B2C yukarıdaki parametreleri HTML içerik sayfasına gönderir:

```
/selfAsserted.aspx?campaignId=hawaii&language=en-US&app=0239a9cc-309c-4d41-87f1-31288feb2e82
```

### <a name="content-definition"></a>İçerik tanımı

[ContentDefinition'da,](contentdefinitions.md) `LoadUri`kullanılan parametrelere bağlı olarak içeriği farklı yerlerden çekmek için talep çözümleyicileri gönderebilirsiniz.

```XML
<ContentDefinition Id="api.signuporsignin">
  <LoadUri>https://contoso.blob.core.windows.net/{Culture:LanguageName}/myHTML/unified.html</LoadUri>
  ...
</ContentDefinition>
```

### <a name="application-insights-technical-profile"></a>Uygulama Öngörüleri teknik profili

Azure Uygulama Öngörüleri ve talep çözümleyicileri ile kullanıcı davranışı hakkında öngörüler edinebilirsiniz. Application Insights teknik profilinde, Azure Application Insights'a kalıcı giriş talepleri gönderirsiniz. Daha fazla bilgi için Bkz. [Uygulama Öngörüleri'ni kullanarak Azure AD B2C yolculuklarında kullanıcı davranışını izleyin.](analytics-with-application-insights.md) Aşağıdaki örnekte, ilke kimliği, korelasyon kimliği, dil ve istemci kimliği Azure Uygulama Öngörüleri'ne gönderilmektedir.

```XML
<TechnicalProfile Id="AzureInsights-Common">
  <DisplayName>Alternate Email</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.Insights.AzureApplicationInsightsProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  ...
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="PolicyId" PartnerClaimType="{property:Policy}" DefaultValue="{Policy:PolicyId}" />
    <InputClaim ClaimTypeReferenceId="CorrelationId" PartnerClaimType="{property:CorrelationId}" DefaultValue="{Context:CorrelationId}" />
    <InputClaim ClaimTypeReferenceId="language" PartnerClaimType="{property:language}" DefaultValue="{Culture:RFC5646}" />
    <InputClaim ClaimTypeReferenceId="AppId" PartnerClaimType="{property:App}" DefaultValue="{OIDC:ClientId}" />
  </InputClaims>
</TechnicalProfile>
```

### <a name="relying-party-policy"></a>Parti ilkesine güvenme

[Güvenilen taraf](relyingparty.md) ilkesi teknik profilinde, kiracı kimliğini veya jwt içindeki bağlı taraf uygulamasına ilişkin korelasyon kimliğini göndermek isteyebilirsiniz.

```XML
<RelyingParty>
    <DefaultUserJourney ReferenceId="SignUpOrSignIn" />
    <TechnicalProfile Id="PolicyProfile">
      <DisplayName>PolicyProfile</DisplayName>
      <Protocol Name="OpenIdConnect" />
      <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="displayName" />
        <OutputClaim ClaimTypeReferenceId="givenName" />
        <OutputClaim ClaimTypeReferenceId="surname" />
        <OutputClaim ClaimTypeReferenceId="email" />
        <OutputClaim ClaimTypeReferenceId="objectId" PartnerClaimType="sub"/>
        <OutputClaim ClaimTypeReferenceId="identityProvider" />
        <OutputClaim ClaimTypeReferenceId="tenantId" AlwaysUseDefaultValue="true" DefaultValue="{Policy:TenantObjectId}" />
        <OutputClaim ClaimTypeReferenceId="correlationId" AlwaysUseDefaultValue="true" DefaultValue="{Context:CorrelationId}" />
      </OutputClaims>
      <SubjectNamingInfo ClaimType="sub" />
    </TechnicalProfile>
  </RelyingParty>
```
