---
title: RelyingParty - Azure Active Directory B2C | Microsoft Dokümanlar
description: Azure Active Directory B2C'de özel bir ilkenin RelyingParty öğesini belirtin.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 02/24/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 90ac6f35cafbe63e8c6cdb77450089d00c0e3099
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79264355"
---
# <a name="relyingparty"></a>RelyingParty

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

**RelyingParty** öğesi, Azure Active Directory B2C (Azure AD B2C) için geçerli istek için zorlamak için kullanıcı yolculuğunu belirtir. Ayrıca, verilen belirteç bir parçası olarak güvenen taraf (RP) uygulamasının ihtiyaç duyduğu iddiaların listesini belirtir. Web, mobil veya masaüstü uygulaması gibi bir RP uygulaması RP ilkesi dosyasını çağırır. RP ilke dosyası oturum açma, parolayı sıfırlama veya profili düzenleme gibi belirli bir görevi yürütür. Birden çok uygulama aynı RP ilkesini kullanabilir ve tek bir uygulama birden çok ilke kullanabilir. Tüm RP uygulamaları talepleri ile aynı belirteci alır ve kullanıcı aynı kullanıcı yolculuğundan geçer.

Aşağıdaki örnek, *B2C_1A_signup_signin* ilkesi dosyasında bir **RelyingParty** öğesini gösterir:

```XML
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<TrustFrameworkPolicy
  xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance"
  xmlns:xsd="https://www.w3.org/2001/XMLSchema"
  xmlns="http://schemas.microsoft.com/online/cpim/schemas/2013/06"
  PolicySchemaVersion="0.3.0.0"
  TenantId="your-tenant.onmicrosoft.com"
  PolicyId="B2C_1A_signup_signin"
  PublicPolicyUri="http://your-tenant.onmicrosoft.com/B2C_1A_signup_signin">

  <BasePolicy>
    <TenantId>your-tenant.onmicrosoft.com</TenantId>
    <PolicyId>B2C_1A_TrustFrameworkExtensions</PolicyId>
  </BasePolicy>

  <RelyingParty>
    <DefaultUserJourney ReferenceId="SignUpOrSignIn" />
    <UserJourneyBehaviors>
      <SingleSignOn Scope="TrustFramework" KeepAliveInDays="7"/>
      <SessionExpiryType>Rolling</SessionExpiryType>
      <SessionExpiryInSeconds>300</SessionExpiryInSeconds>
      <JourneyInsights TelemetryEngine="ApplicationInsights" InstrumentationKey="your-application-insights-key" DeveloperMode="true" ClientEnabled="false" ServerEnabled="true" TelemetryVersion="1.0.0" />
      <ContentDefinitionParameters>
        <Parameter Name="campaignId">{OAUTH-KV:campaignId}</Parameter>
      </ContentDefinitionParameters>
    </UserJourneyBehaviors>
    <TechnicalProfile Id="PolicyProfile">
      <DisplayName>PolicyProfile</DisplayName>
      <Description>The policy profile</Description>
      <Protocol Name="OpenIdConnect" />
      <Metadata>collection of key/value pairs of data</Metadata>
      <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="displayName" />
        <OutputClaim ClaimTypeReferenceId="givenName" />
        <OutputClaim ClaimTypeReferenceId="surname" />
        <OutputClaim ClaimTypeReferenceId="email" />
        <OutputClaim ClaimTypeReferenceId="objectId" PartnerClaimType="sub"/>
        <OutputClaim ClaimTypeReferenceId="identityProvider" />
        <OutputClaim ClaimTypeReferenceId="loyaltyNumber" />
      </OutputClaims>
      <SubjectNamingInfo ClaimType="sub" />
    </TechnicalProfile>
  </RelyingParty>
  ...
```

İsteğe bağlı **RelyingParty** öğesi aşağıdaki öğeleri içerir:

| Öğe | Oluşum | Açıklama |
| ------- | ----------- | ----------- |
| Varsayılan Kullanıcı Yolculuğu | 1:1 | RP uygulaması için varsayılan kullanıcı yolculuğu. |
| UserJourneyBehaviors | 0:1 | Kullanıcı yolculuğu davranışlarının kapsamı. |
| Teknik Profil | 1:1 | RP uygulaması tarafından desteklenen teknik bir profil. Teknik profil, RP uygulamasının Azure AD B2C ile iletişim kurmasını sağlar. |

## <a name="defaultuserjourney"></a>Varsayılan Kullanıcı Yolculuğu

Öğe, `DefaultUserJourney` genellikle Temel veya Uzantılar ilkesinde tanımlanan kullanıcı yolculuğunun tanımlayıcısına bir başvuru belirtir. Aşağıdaki örnekler, **RelyingParty** öğesinde belirtilen kaydolma veya kaydolma kullanıcı yolculuğunu gösterir:

*B2C_1A_signup_signin* politikası:

```XML
<RelyingParty>
  <DefaultUserJourney ReferenceId="SignUpOrSignIn">
  ...
```

*B2C_1A_TrustFrameWorkBase* veya *B2C_1A_TrustFrameworkExtensionPolicy*:

```XML
<UserJourneys>
  <UserJourney Id="SignUpOrSignIn">
  ...
```

**DefaultUserJourney** öğesi aşağıdaki özniteliği içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| ReferenceId | Evet | İlkedeki kullanıcı yolculuğunun tanımlayıcısı. Daha fazla bilgi için [kullanıcı yolculuklarına](userjourneys.md) bakın |

## <a name="userjourneybehaviors"></a>UserJourneyBehaviors

**UserJourneyBehaviors** öğesi aşağıdaki öğeleri içerir:

| Öğe | Oluşum | Açıklama |
| ------- | ----------- | ----------- |
| SingleSignon | 0:1 | Bir kullanıcı yolculuğunun tek oturum (SSO) oturumu davranışının kapsamı. |
| SessionExpiryType |0:1 | Oturumun kimlik doğrulama davranışı. Olası `Rolling` değerler: `Absolute`veya . `Rolling` Değer (varsayılan), kullanıcı uygulamada sürekli olarak etkin olduğu sürece kullanıcının oturum açmış olarak kaldığını gösterir. Değer, `Absolute` kullanıcının uygulama oturumu boyunca belirtilen süre sonunda yeniden kimlik doğrulamaya zorladığını gösterir. |
| SessionExpiryInSeconds | 0:1 | Azure AD B2C'nin oturum çerezinin kullanım ömrü, başarılı kimlik doğrulaması sırasında kullanıcının tarayıcısında depolanan bir tamsayı olarak belirtilir. |
| JourneyInsights | 0:1 | Kullanılacak Azure Application Insights enstrümantasyon anahtarı. |
| İçerikTanım Parametreleri | 0:1 | İçerik tanımına eklenecek anahtar değer çiftleri listesi URI yükleyin. |
|Komut Dosyası Yürütme| 0:1| Desteklenen [JavaScript](javascript-samples.md) yürütme modları. Olası `Allow` değerler: `Disallow` veya (varsayılan).

### <a name="singlesignon"></a>SingleSignon

**SingleSignOn** öğesi aşağıdaki öznitelikte içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| Kapsam | Evet | Tek oturum açma davranışının kapsamı. Olası `Suppressed`değerler: `Tenant` `Application`, `Policy`, , veya . Değer, `Suppressed` davranışın bastırılmış olduğunu gösterir. Örneğin, tek bir oturum açma oturumu söz konusu olduğunda, kullanıcı için oturum yapılmaz ve kullanıcıdan her zaman bir kimlik sağlayıcı seçimi istenir. Değer, `TrustFramework` davranışın güven çerçevesindeki tüm ilkeler için uygulandığını gösterir. Örneğin, bir güven çerçevesi için iki ilke yolculuğunda gezinen bir kullanıcıdan kimlik sağlayıcı seçimi istenmez. Değer, `Tenant` davranışın kiracıdaki tüm ilkelere uygulandığını gösterir. Örneğin, bir kiracı için iki ilke yolculuğu boyunca gezinen bir kullanıcı, kimlik sağlayıcı seçimi için istenmez. Değer, `Application` davranışın isteği oluşturan uygulama için tüm ilkelere uygulandığını gösterir. Örneğin, bir uygulama için iki ilke yolculuğunda gezinen bir kullanıcı, kimlik sağlayıcı seçimi için istenmez. Değer, `Policy` davranışın yalnızca bir ilke için geçerli olduğunu gösterir. Örneğin, bir güven çerçevesi için iki ilke yolculuğunda gezinen bir kullanıcı, ilkeler arasında geçiş yaparken kimlik sağlayıcı seçimi için istenir. |
| KeepAliveInDays | Evet | Kullanıcının oturum da ne kadar süreyle oturum açmış olduğunu denetler. Değeri 0 olarak ayarlamak KMSI işlevselliğini kapatır. Daha fazla bilgi için [bkz.](custom-policy-keep-me-signed-in.md) |
|EnforceIdTokenHintOnLogout| Hayır|  Son kullanıcının istemciyle geçerli kimlik doğrulama oturumu hakkında ipucu olarak oturum sonu noktasına daha önce verilmiş bir kimlik belirteci geçmeye zorlar. Olası değerler: `false` (varsayılan) veya `true`. Daha fazla bilgi için [OpenID Connect ile Web oturum açma'ya](openid-connect.md)bakın.  |


## <a name="journeyinsights"></a>JourneyInsights

**JourneyInsights** öğesi aşağıdaki öznitelikleri içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| TelemetriMotoru | Evet | Değer. `ApplicationInsights` |
| EnstrümantasyonAnahtar | Evet | Uygulama öngörüleri öğesi için enstrümantasyon anahtarını içeren dize. |
| Geliştirici Modu | Evet | Olası `true` değerler: `false`veya . Eğer `true`, Uygulama Öngörüleri işlem ardışık aracılığıyla telemetri hızlandırır. Bu ayar geliştirme için iyidir, ancak yüksek hacimlerde sınırlandırılmıştır Ayrıntılı etkinlik günlükleri yalnızca özel ilkelerin geliştirilmesine yardımcı olmak üzere tasarlanmıştır. Üretimde geliştirme modunu kullanmayın. Günlükler, geliştirme sırasında kimlik sağlayıcılarına gönderilen ve kimlik sağlayıcılardan gönderilen tüm talepleri toplar. Üretimde kullanılırsa, geliştirici sahip oldukları App Insights günlüğünde toplanan KIŞISEL Bilgiler (Özel Tanımlanabilir Bilgiler) için sorumluluk üstlenir. Bu ayrıntılı günlükler yalnızca bu değer `true`.|
| İstemci Etkin | Evet | Olası `true` değerler: `false`veya . , `true`sayfa görünümünü ve istemci tarafı hatalarını izlemek için Application Insights istemci tarafı komut dosyasını gönderirse. |
| Sunucu Etkin | Evet | Olası `true` değerler: `false`veya . Mevcut `true`UserJourneyRecorder JSON'ı özel bir olay olarak Uygulama Öngörüleri'ne gönderirse. |
| TelemetryVersiyon | Evet | Değer. `1.0.0` |

Daha fazla bilgi için [bkz.](troubleshoot-with-application-insights.md)

## <a name="contentdefinitionparameters"></a>İçerikTanım Parametreleri

Azure AD B2C'de özel ilkeler kullanarak, sorgu dizesi içinde bir parametre gönderebilirsiniz. Parametreyi HTML uç noktanıza ileterek sayfa içeriğini dinamik olarak değiştirebilirsiniz. Örneğin web veya mobil uygulamanızdan ilettiğiniz bir parametreye göre Azure AD B2C kaydolma veya oturum açma sayfanızdaki arka plan görüntüsünü değiştirebilirsiniz. Azure AD B2C sorgu dize parametrelerini aspx dosyası gibi dinamik HTML dosyanıza aktarır.

Aşağıdaki örnek, sorgu dizesinde `campaignId` `hawaii` değeri olan bir parametreyi geçer:

`https://login.microsoft.com/contoso.onmicrosoft.com/oauth2/v2.0/authorize?pB2C_1A_signup_signin&client_id=a415078a-0402-4ce3-a9c6-ec1947fcfb3f&nonce=defaultNonce&redirect_uri=http%3A%2F%2Fjwt.io%2F&scope=openid&response_type=id_token&prompt=login&campaignId=hawaii`

**ContentDefinitionParameters** öğesi aşağıdaki öğeyi içerir:

| Öğe | Oluşum | Açıklama |
| ------- | ----------- | ----------- |
| İçerikDefinitionParameter | 0:n | Bir içerik tanımı yükü URI'nin sorgu dizesine eklenen anahtar değer çiftini içeren bir dize. |

**ContentDefinitionParameter** öğesi aşağıdaki özniteliği içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| Adı | Evet | Anahtar değer çiftinin adı. |

Daha fazla bilgi için bkz: [Özel ilkeler kullanarak kullanıcı arabirimi'ni dinamik içerikle yapılandır](custom-policy-ui-customization.md#configure-dynamic-custom-page-content-uri)

## <a name="technicalprofile"></a>Teknik Profil

**TechnicalProfile** öğesi aşağıdaki özniteliği içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| Kimlik | Evet | Değer. `PolicyProfile` |

**TechnicalProfile** aşağıdaki öğeleri içerir:

| Öğe | Oluşum | Açıklama |
| ------- | ----------- | ----------- |
| DisplayName | 1:1 | Teknik profilin adını içeren dize. |
| Açıklama | 0:1 | Teknik profilin açıklamasını içeren dize. |
| Protokol | 1:1 | Federasyon için kullanılan protokol. |
| Meta Veriler | 0:1 | Bir işlem sırasında son nokta ile iletişim kurmak için protokol tarafından kullanılan anahtar/değer çiftleri *Öğesinin* toplanması, güvenen taraf ve diğer topluluk katılımcıları arasındaki etkileşimi yapılandırmak için. |
| Çıktılar Talepleri | 1:1 | Teknik profilde çıktı olarak alınan talep türlerinin listesi. Bu öğelerin her **biri, ClaimsSchema** bölümünde veya bu ilke dosyasının devraldığı bir ilkede zaten tanımlanmış bir **ClaimType'a** başvuru içerir. |
| Konu NamingInfo | 1:1 | Belirteçlerde kullanılan özne adı. |

**Protokol** öğesi aşağıdaki öznitelik içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| Adı | Evet | Teknik profilin bir parçası olarak kullanılan Azure AD B2C tarafından desteklenen geçerli bir protokolün adı. Olası `OpenIdConnect` değerler: `SAML2`veya . Değer, `OpenIdConnect` OpenID temel belirtimine göre OpenID Connect 1.0 protokol standardını temsil eder. OASIS `SAML2` belirtimi uyarınca SAML 2.0 protokol standardını temsil eder. Üretimde SAML belirteci kullanmayın. |

## <a name="outputclaims"></a>Çıktılar Talepleri

**OutputClaims** öğesi aşağıdaki öğeyi içerir:

| Öğe | Oluşum | Açıklama |
| ------- | ----------- | ----------- |
| ÇıktılarTalep | 0:n | Güvenilen tarafın abone olduğu ilke için desteklenen listede beklenen bir talep türünün adı. Bu talep teknik profil için bir çıktı olarak hizmet vermektedir. |

**OutputClaim** öğesi aşağıdaki öznitelikleri içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| ClaimTypeReferenceId | Evet | İlke dosyasındaki **ClaimsSchema** bölümünde zaten tanımlanmış bir **ClaimType'a** başvuru. |
| Varsayılan Değer | Hayır | Talep değeri boşsa kullanılabilecek varsayılan değer. |
| OrtakClaimType | Hayır | Talebi ClaimType tanımında yapılandırıldığı şekilde farklı bir adla gönderir. |

### <a name="subjectnaminginfo"></a>Konu NamingInfo

**SubjectNameingInfo** öğesi ile belirteç öznesinin değerini denetlersiniz:
- **JWT belirteç** - `sub` iddia. Bu, belirteç bir uygulamanın kullanıcısı gibi bilgileri ileri saldığı bir anamüdürdür. Bu değer değişmez dir ve yeniden atanamaz veya yeniden kullanılamaz. Belirteç bir kaynağa erişmek için ne zaman kullanıldığı gibi güvenli yetkilendirme denetimleri gerçekleştirmek için kullanılabilir. Varsayılan olarak, özne talebi dizindeki kullanıcının nesne kimliğiyle doldurulur. Daha fazla bilgi için [Belirteç, oturum ve tek oturum yapılandırmasına](session-behavior.md)bakın.
- **SAML belirteci** - özne öğesini tanımlayan `<Subject><NameID>` öğe.

**SubjectNamingInfo** öğesi aşağıdaki öznitelik içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| Claimtype | Evet | Çıktı talebinin **PartnerClaimType'ına**başvuru. Çıktı talepleri, güvenilen parti ilkesi **Çıktı Talepleri** koleksiyonunda tanımlanmalıdır. |

Aşağıdaki örnek, openid connect relying party nasıl tanımlanılacağını gösterir. Konu adı bilgisi aşağıdaki gibi `objectId`yapılandırılır:

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
    </OutputClaims>
    <SubjectNamingInfo ClaimType="sub" />
  </TechnicalProfile>
</RelyingParty>
```
JWT belirteci, `sub` kullanıcı objectId'i ile ilgili talebi içerir:

```JSON
{
  ...
  "sub": "6fbbd70d-262b-4b50-804c-257ae1706ef2",
  ...
}
```
