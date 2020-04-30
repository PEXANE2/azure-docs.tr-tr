---
title: Özel ilkelerdeki talep çözücüler
titleSuffix: Azure AD B2C
description: Azure Active Directory B2C bir özel ilkede talep çözümleyicilerine nasıl kullanacağınızı öğrenin.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 04/21/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 83e1e11fe38a21bbd7c44139fac562342bcab866
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82229655"
---
# <a name="about-claim-resolvers-in-azure-active-directory-b2c-custom-policies"></a>Azure Active Directory B2C özel ilkelerde talep çözücüler hakkında

Azure Active Directory B2C (Azure AD B2C) [özel ilkelerindeki](custom-policy-overview.md) talep çözücüler, ilke adı, Istek bağıntı kimliği, Kullanıcı arabirimi dili ve daha fazlası gibi bir yetkilendirme isteğiyle ilgili bağlam bilgilerini sağlar.

Bir giriş veya çıkış talebinde bir talep çözümleyici kullanmak için, [Claimsschema](claimsschema.md) öğesi altında bir String **ClaimType**tanımlar ve ardından **DefaultValue** değerini giriş veya çıkış talebi öğesinde talep çözümleyici olarak ayarlarsınız. Azure AD B2C, talep Çözümleyicisinin değerini okur ve teknik profildeki değeri kullanır.

Aşağıdaki örnekte, adlı `correlationId` bir talep türü, bir **veri türü** ile tanımlanmıştır. `string`

```XML
<ClaimType Id="correlationId">
  <DisplayName>correlationId</DisplayName>
  <DataType>string</DataType>
  <UserHelpText>Request correlation Id</UserHelpText>
</ClaimType>
```

Teknik profilde talep çözümleyicisini talep türü ile eşleyin. Azure AD B2C talep Çözümleyicisinin `{Context:CorrelationId}` değerini talebe `correlationId` göre doldurur ve talebi teknik profile gönderir.

```XML
<InputClaim ClaimTypeReferenceId="correlationId" DefaultValue="{Context:CorrelationId}" />
```

## <a name="claim-resolver-types"></a>Talep çözümleyici türleri

Aşağıdaki bölümlerde, kullanılabilir talep çözücüler listelenmektedir.

### <a name="culture"></a>Culture (Kültür)

| İste | Açıklama | Örnek |
| ----- | ----------- | --------|
| {Culture: LanguageName} | Dil için iki harfli ISO kodu. | tr |
| {Culture: LCıD}   | Dil kodunun LCıD 'SI. | 1033 |
| {Culture: RegionName} | Bölgenin iki harfli ISO kodu. | ABD |
| {Culture: RFC5646} | RFC5646 dil kodu. | tr-TR |

### <a name="policy"></a>İlke

| İste | Açıklama | Örnek |
| ----- | ----------- | --------|
| {Policy: PolicyId} | Bağlı olan taraf ilkesi adı. | B2C_1A_signup_signin |
| {Policy: RelyingPartyTenantId} | Bağlı olan taraf ilkesinin kiracı KIMLIĞI. | your-tenant.onmicrosoft.com |
| {Policy: Tenantobjectıd} | Bağlı olan taraf ilkesinin kiracı nesne KIMLIĞI. | 00000000-0000-0000-0000-000000000000 |
| {Policy: TrustFrameworkTenantId} | Güven çerçevesinin kiracı KIMLIĞI. | your-tenant.onmicrosoft.com |

### <a name="openid-connect"></a>OpenID Connect

| İste | Açıklama | Örnek |
| ----- | ----------- | --------|
| {OıDC: AuthenticationContextReferences} |`acr_values` Sorgu dizesi parametresi. | Yok |
| {OıDC: ClientID} |`client_id` Sorgu dizesi parametresi. | 00000000-0000-0000-0000-000000000000 |
| {OıDC: Domainipucuyla} |`domain_hint` Sorgu dizesi parametresi. | facebook.com |
| {OıDC: Loginipucu} |  `login_hint` Sorgu dizesi parametresi. | someone@contoso.com |
| {OıDC: MaxAge} | `max_age`. | Yok |
| {OıDC: nonce} |`Nonce` Sorgu dizesi parametresi. | defaultNonce |
| {OıDC: Password}| [Kaynak sahibi parola kimlik bilgileri](ropc-custom.md) kullanıcının parolasını akışa.| Parola1| 
| {OıDC: Prompt} | `prompt` Sorgu dizesi parametresi. | oturum aç |
| {OıDC: RedirectUri} |`redirect_uri` Sorgu dizesi parametresi. | https://jwt.ms |
| {OıDC: kaynak} |`resource` Sorgu dizesi parametresi. | Yok |
| {OıDC: scope} |`scope` Sorgu dizesi parametresi. | OpenID |
| {OıDC: username}| [Kaynak sahibi parola kimlik bilgileri](ropc-custom.md) kullanıcının Kullanıcı adını Flow.| emily@contoso.com| 

### <a name="context"></a>Bağlam

| İste | Açıklama | Örnek |
| ----- | ----------- | --------|
| {Context: BuildNumber} | Kimlik deneyimi çerçevesi sürümü (derleme numarası).  | 1.0.507.0 |
| {Context: CorrelationId} | Bağıntı KIMLIĞI.  | 00000000-0000-0000-0000-000000000000 |
| {Context: Datetimeınutc} |UTC olarak tarih saat.  | 10/10/2018 12:00:00 PM |
| {Context: DeploymentMode} |İlke dağıtım modu.  | Üretim |
| {Context: IPAddress} | Kullanıcı IP adresi. | 11.111.111.11 |
| {Context: KMSI} | Oturumumu [açık tut](custom-policy-keep-me-signed-in.md) onay kutusunun seçili olup olmadığını gösterir. |  true |

### <a name="claims"></a>Talepler 

| İste | Açıklama | Örnek |
| ----- | ----------- | --------|
| {Talep: talep türü} | İlke dosyası veya üst ilke dosyasında ClaimsSchema bölümünde zaten tanımlanmış olan bir talep türünün tanımlayıcısı.  Örneğin: `{Claim:displayName}`, veya `{Claim:objectId}`. | Bir talep türü değeri.|


### <a name="oauth2-key-value-parameters"></a>OAuth2 anahtar-değer parametreleri

OıDC veya OAuth2 isteğinin bir parçası olarak dahil edilen herhangi bir parametre adı, Kullanıcı yolculuğunda bir talebe eşleştirilebilir. Örneğin, uygulamadaki istek, veya bir özel sorgu dizesi adına `app_session` `loyalty_number`sahip bir sorgu dizesi parametresi içerebilir.

| İste | Açıklama | Örnek |
| ----- | ----------------------- | --------|
| {OAUTH-KV: kampanya Nkimliği} | Sorgu dizesi parametresi. | Hawaii |
| {OAUTH-KV: app_session} | Sorgu dizesi parametresi. | A3C5R |
| {OAUTH-KV: loyalty_number} | Sorgu dizesi parametresi. | 1234 |
| {OAUTH-KV: herhangi bir özel sorgu dizesi} | Sorgu dizesi parametresi. | Yok |

### <a name="oauth2"></a>OAuth2

| İste | Açıklama | Örnek |
| ----- | ----------------------- | --------|
| {OAuth2: access_token} | Erişim belirteci. | Yok |


### <a name="saml"></a>SAML

| İste | Açıklama | Örnek |
| ----- | ----------- | --------|
| {SAML: AuthnContextClassReferences} | SAML `AuthnContextClassRef` isteğinden öğe değeri. | urn: oassıs: adlar: TC: SAML: 2.0: AC: sınıflar: PasswordProtectedTransport |
| {SAML: Nameıdpolicyformat} | SAML `Format` isteğinin `NameIDPolicy` öğesinden özniteliği. | urn: oassıs: adlar: TC: SAML: 1.1: NameID-Format: Emapostaadı |
| {SAML: Issuer} |  SAML isteğinin `Issuer` SAML öğesi değeri.| `https://contoso.com` |
| {SAML: AllowCreate} | SAML `AllowCreate` isteğinin `NameIDPolicy` öğesinden öznitelik değeri. | True |
| {SAML: ForceAuthn} | SAML `ForceAuthN` isteğinin `AuthnRequest` öğesinden öznitelik değeri. | True |
| {SAML: ProviderName} | SAML `ProviderName` isteğinin `AuthnRequest` öğesinden öznitelik değeri.| Contoso.com |
| {SAML: RelayState} | `RelayState` Sorgu dizesi parametresi.| 

## <a name="using-claim-resolvers"></a>Talep çözücüler kullanma

Talep çözümleyicilerine aşağıdaki öğelerle birlikte kullanabilirsiniz:

| Öğe | Öğe | Ayarlar |
| ----- | ----------------------- | --------|
|Application Insights teknik profili |`InputClaim` | |
|[Azure Active Directory](active-directory-technical-profile.md) teknik profili| `InputClaim`, `OutputClaim`| 1, 2|
|[OAuth2](oauth2-technical-profile.md) teknik profili| `InputClaim`, `OutputClaim`| 1, 2|
|[OpenID Connect](openid-connect-technical-profile.md) teknik profili| `InputClaim`, `OutputClaim`| 1, 2|
|[Talep dönüştürme](claims-transformation-technical-profile.md) teknik profili| `InputClaim`, `OutputClaim`| 1, 2|
|[Restsize sağlayıcı](restful-technical-profile.md) teknik profili| `InputClaim`| 1, 2|
|[SAML kimlik sağlayıcısı](saml-identity-provider-technical-profile.md) teknik profili| `OutputClaim`| 1, 2|
|[Kendi kendine onaylanan](self-asserted-technical-profile.md) teknik profil| `InputClaim`, `OutputClaim`| 1, 2|
|[ContentDefinition](contentdefinitions.md)| `LoadUri`| |
|[ContentDefinitionParameters](relyingparty.md#contentdefinitionparameters)| `Parameter` | |
|[RelyingParty](relyingparty.md#technicalprofile) teknik profili| `OutputClaim`| 2 |

Ayarlar:
1. `IncludeClaimResolvingInClaimsHandling` Meta verilerin olarak `true`ayarlanması gerekir.
1. Giriş veya çıkış talepleri özniteliği `AlwaysUseDefaultValue` olarak `true`ayarlanmalıdır.

## <a name="claim-resolvers-samples"></a>Talep çözücüler örnekleri

### <a name="restful-technical-profile"></a>Yeniden teknik profil

[Bir teknik](restful-technical-profile.md) profilde, Kullanıcı dilini, ilke adını, kapsamı ve istemci kimliğini göndermek isteyebilirsiniz. REST API, özel iş mantığını çalıştırabilmelidir ve gerekirse yerelleştirilmiş bir hata iletisi oluşturabilir.

Aşağıdaki örnek, bu senaryoya sahip bir teknik profili göstermektedir:

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

Talep çözümleyicilerine göre, oturum açma adını veya Facebook, LinkedIn veya Microsoft hesabı gibi belirli bir sosyal kimlik sağlayıcısına doğrudan oturum açmayı önceden girebilirsiniz. Daha fazla bilgi için bkz. [Azure Active Directory B2C kullanarak doğrudan oturum açma ayarlama](direct-signin.md).

### <a name="dynamic-ui-customization"></a>Dinamik UI özelleştirmesi

Azure AD B2C, sayfa içeriğini dinamik olarak oluşturmak için sorgu dizesi parametrelerini HTML içerik tanım uç noktalarınıza geçirmenize olanak sağlar. Örneğin, bu özellik, Web veya mobil uygulamanızdan geçirdiğiniz özel bir parametreye göre Azure AD B2C kaydolma veya oturum açma sayfasındaki arka plan görüntüsünü değiştirebilme olanağı sağlar. Daha fazla bilgi için bkz. [Azure Active Directory B2C içindeki özel ilkeleri kullanarak Kullanıcı arabirimini dinamik olarak yapılandırma](custom-policy-ui-customization.md#configure-dynamic-custom-page-content-uri). Ayrıca, HTML sayfanızı bir dil parametresine göre yerelleştirebilirsiniz veya içeriği istemci KIMLIĞINE göre değiştirebilirsiniz.

Aşağıdaki örnek, `Hawaii`bir değeri, bir **dil** kodu `en-US`ve istemci kimliğini temsil eden bir **uygulama** olan **Kampanya NID** adlı sorgu dizesi parametresinde geçirilir:

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

Bir [ContentDefinition](contentdefinitions.md) `LoadUri`'da, kullanılan parametrelere göre farklı yerlerden çekme içerikleri için talep çözücüler gönderebilirsiniz.

```XML
<ContentDefinition Id="api.signuporsignin">
  <LoadUri>https://contoso.blob.core.windows.net/{Culture:LanguageName}/myHTML/unified.html</LoadUri>
  ...
</ContentDefinition>
```

### <a name="application-insights-technical-profile"></a>Application Insights teknik profili

Azure Application Insights ve talep Çözümleyenler sayesinde Kullanıcı davranışında Öngörüler elde edebilirsiniz. Application Insights teknik profilinde, Azure Application Insights kalıcı olan giriş taleplerini gönderirsiniz. Daha fazla bilgi için bkz. [Application Insights kullanarak Azure AD B2C yolculuğa yönelik kullanıcı davranışını izleme](analytics-with-application-insights.md). Aşağıdaki örnek, ilke KIMLIĞI, bağıntı KIMLIĞI, dil ve istemci KIMLIĞINI Azure Application Insights gönderir.

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

### <a name="relying-party-policy"></a>Bağlı olan taraf ilkesi

[Bağlı olan taraf](relyingparty.md) ilkesi teknik profilinde, Kiracı kimliğini veya BAĞıNTı kimliğini JWT içindeki bağlı olan taraf uygulamasına göndermek isteyebilirsiniz.

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
