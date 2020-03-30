---
title: REST API talep değişimleri - Azure Active Directory B2C
description: Active Directory B2C'deki özel ilkelere REST API talepleri değişimi ekleyin.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 03/26/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 6316165ba08d055be1186995e2fe2ad5a0079fb7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80330713"
---
# <a name="walkthrough-add-rest-api-claims-exchanges-to-custom-policies-in-azure-active-directory-b2c"></a>Walkthrough: Azure Active Directory B2C'deki özel ilkelere REST API talep alışverişi ekleme

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Azure Active Directory B2C (Azure AD B2C), kimlik geliştiricilerin kullanıcı yolculuğunda YENIDEN Bir API ile etkileşimi tümleştirmelerine olanak tanır. Bu iznin sonunda, [RESTful hizmetleriyle](custom-policy-rest-api-intro.md)etkileşimedebilen bir Azure AD B2C kullanıcı yolculuğu oluşturabilirsiniz.

Bu senaryoda, kurumsal bir iş akışıyla tümleştirerek kullanıcının belirteç verilerini zenginleştiririz. Azure AD B2C, yerel veya federe hesapla kaydolma veya kaydolma sırasında, kullanıcının genişletilmiş profil verilerini uzak bir veri kaynağından almak için bir REST API'si çağırır. Bu örnekte, Azure AD B2C kullanıcının benzersiz tanımlayıcısını, objectId'yi gönderir. REST API'si daha sonra kullanıcının hesap bakiyesini (rasgele bir sayı) döndürür. Kendi CRM sisteminizle, pazarlama veritabanınızla veya herhangi bir iş akışıyla tümleştirmek için bu örneği başlangıç noktası olarak kullanın.

Etkileşimi doğrulama teknik profili olarak da tasarlayabilirsiniz. Bu, REST API'si ekrandaki verileri doğrularken ve talepleri iade ederken uygundur. Daha fazla bilgi için [Walkthrough: Kullanıcı girişini doğrulamak için Azure AD B2C kullanıcı yolculuğunuzdaki REST API talep alışverişini tümleştirin.](custom-policy-rest-api-claims-validation.md)

## <a name="prerequisites"></a>Ön koşullar

- [Özel ilkelerle başlayın](custom-policy-get-started.md)adımlarını tamamlayın. Yerel hesaplarla kaydolmak ve oturum açmak için çalışan bir özel politikanız olmalıdır.
- [REST API talep alışverişini Azure AD B2C özel politikanızda](custom-policy-rest-api-intro.md)nasıl entegre edebilirsiniz öğrenin.

## <a name="prepare-a-rest-api-endpoint"></a>REST API bitiş noktası hazırlama

Bu izbin için, bir kullanıcının Azure AD B2C objectId'inin arka uç sisteminizde kayıtlı olup olmadığını doğrulayan bir REST API'niz olmalıdır. Kaydedilmişse, REST API kullanıcı hesap bakiyesini döndürür. Aksi takdirde, REST API yeni hesabı dizine kaydeder ve `50.00`başlangıç bakiyesini döndürür.

Aşağıdaki JSON kodu, Azure AD B2C'nin REST API bitiş noktanıza göndereceği verileri gösterir. 

```json
{
    "objectId": "User objectId",
    "language": "Current UI language"
}
```

REST API'niz verileri doğruladıktan sonra, aşağıdaki JSON verileriyle birlikte bir HTTP 200 (Ok) döndürmesi gerekir:

```json
{
    "balance": "760.50"
}
```

REST API bitiş noktasının kurulumu bu makalenin kapsamı dışındadır. Bir Azure [İşlevler](https://docs.microsoft.com/azure/azure-functions/functions-reference) örneği oluşturduk. Tüm Azure işlev koduna [GitHub'dan](https://github.com/azure-ad-b2c/rest-api/tree/master/source-code/azure-function)erişebilirsiniz.

## <a name="define-claims"></a>Talepleri tanımlama

Talep, Azure AD B2C ilke yürütmesi sırasında verilerin geçici olarak depolanmasını sağlar. [İddialar şeması](claimsschema.md) bölümünde ki talepleri beyan edebilirsiniz. 

1. İlkinizin uzantılar dosyasını açın. Örneğin, <em> `SocialAndLocalAccounts/` </em>.
1. [BuildingBlocks](buildingblocks.md) öğesini arayın. Öğe yoksa, ekleyin.
1. [ClaimsSchema](claimsschema.md) öğesini bulun. Öğe yoksa, ekleyin.
1. Aşağıdaki iddiaları **ClaimsSchema** öğesine ekleyin.  

```xml
<ClaimType Id="balance">
  <DisplayName>Your Balance</DisplayName>
  <DataType>string</DataType>
</ClaimType>
<ClaimType Id="userLanguage">
  <DisplayName>User UI language (used by REST API to return localized error messages)</DisplayName>
  <DataType>string</DataType>
</ClaimType>
```

## <a name="configure-the-restful-api-technical-profile"></a>RESTful API teknik profilini yapılandırma 

[Dinlendirici teknik profil,](restful-technical-profile.md) kendi RESTful hizmetinizle birlikte yüz yüze çalışma desteği sağlar. Azure AD B2C, bir `InputClaims` koleksiyondaki RESTful hizmetine veri `OutputClaims` gönderir ve bir koleksiyondaki verileri geri alır. Dosyanızdaki **ClaimsProviders** <em>**`TrustFrameworkExtensions.xml`**</em> Talep Sağlayıcıları öğesini bulun ve aşağıdaki gibi yeni bir talep sağlayıcısı ekleyin:

```xml
<ClaimsProvider>
  <DisplayName>REST APIs</DisplayName>
  <TechnicalProfiles>
    <TechnicalProfile Id="REST-GetProfile">
      <DisplayName>Get user extended profile Azure Function web hook</DisplayName>
      <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
      <Metadata>
        <Item Key="ServiceUrl">https://your-account.azurewebsites.net/api/GetProfile?code=your-code</Item>
        <Item Key="SendClaimsIn">Body</Item>
        <!-- Set AuthenticationType to Basic or ClientCertificate in production environments -->
        <Item Key="AuthenticationType">None</Item>
        <!-- REMOVE the following line in production environments -->
        <Item Key="AllowInsecureAuthInProduction">true</Item>
      </Metadata>
      <InputClaims>
        <!-- Claims sent to your REST API -->
        <InputClaim ClaimTypeReferenceId="objectId" />
        <InputClaim ClaimTypeReferenceId="userLanguage" PartnerClaimType="lang" DefaultValue="{Culture:LCID}" AlwaysUseDefaultValue="true" />
      </InputClaims>
      <OutputClaims>
        <!-- Claims parsed from your REST API -->
        <OutputClaim ClaimTypeReferenceId="balance" />
      </OutputClaims>
      <UseTechnicalProfileForSessionManagement ReferenceId="SM-Noop" />
    </TechnicalProfile>
  </TechnicalProfiles>
</ClaimsProvider>
```

Bu örnekte, `userLanguage` JSON yükü içinde `lang` olduğu gibi REST hizmetine gönderilecektir. Talebin `userLanguage` değeri geçerli kullanıcı dili kimliğini içerir. Daha fazla bilgi [için, talep çözümleyicisi](claim-resolver-overview.md)bakın.

Yukarıdaki `AuthenticationType` açıklamaları `AllowInsecureAuthInProduction` belirtin ve bir üretim ortamına taşınırken yaptığınız değişiklikleri belirtin. Yeniden üretim için yeniden ifl eksalarınızı nasıl güvenli hale erdireceğimize yönelik bilgi için Secure [RESTful API'ye](secure-rest-api.md)bakın.

## <a name="add-an-orchestration-step"></a>Orkestrasyon adımı ekleme

[Kullanıcı yolculukları,](userjourneys.md) bir ilkenin, bir kullanıcı için istenen talepleri elde etmesine güvenen bir taraf uygulamasının izin verdiği açık yolları belirtir. Kullanıcı yolculuğu, başarılı bir işlem için izlenmesi gereken bir orkestrasyon dizisi olarak gösterilir. Düzenleme adımları ekleyebilir veya çıkarabilirsiniz. Bu durumda, REST API çağrısı ile kullanıcı kaydolduktan veya oturum açtıktan sonra uygulamaya sağlanan bilgileri artırmak için kullanılan yeni bir düzenleme adımı eklersiniz.

1. İlkinizin temel dosyasını açın. Örneğin, <em> `SocialAndLocalAccounts/` </em>.
1. Öğeyi `<UserJourneys>` arayın. Öğenin tamamını kopyalayın ve silin.
1. İlkinizin uzantılar dosyasını açın. Örneğin, <em> `SocialAndLocalAccounts/` </em>.
1. `<ClaimsProviders>` Öğenin `<UserJourneys>` kapatılmasından sonra uzantıları dosyasına yapıştırın.
1. 'yi `<UserJourney Id="SignUpOrSignIn">`bulun ve sonuncusundan önce aşağıdaki orkestrasyon adımını ekleyin.

    ```XML
    <OrchestrationStep Order="7" Type="ClaimsExchange">
      <ClaimsExchanges>
        <ClaimsExchange Id="RESTGetProfile" TechnicalProfileReferenceId="REST-GetProfile" />
      </ClaimsExchanges>
    </OrchestrationStep>
    ```

1. Refactor son orkestrasyon `Order` adımı `8`değiştirerek . Son iki düzenleme adımınız aşağıdaki gibi görünmelidir:

    ```XML
    <OrchestrationStep Order="7" Type="ClaimsExchange">
      <ClaimsExchanges>
        <ClaimsExchange Id="RESTGetProfile" TechnicalProfileReferenceId="REST-GetProfile" />
      </ClaimsExchanges>
    </OrchestrationStep>

    <OrchestrationStep Order="8" Type="SendClaims" CpimIssuerTechnicalProfileReferenceId="JwtIssuer" />
    ```

1. **ProfileEdit** ve **PasswordReset** kullanıcı yolculukları için son iki adımı yineleyin.


## <a name="include-a-claim-in-the-token"></a>Belirteci bir talep ekleme 

`balance` Talebi güvenen taraf uygulamasına geri döndürmek <em> `SocialAndLocalAccounts/` </em> için, dosyaya bir çıktı talebi ekleyin. Çıktı talebi eklemek, başarılı bir kullanıcı yolculuğundan sonra talebi belirteci içine alacaktır ve uygulamaya gönderilir. Çıktı talebi olarak eklemek `balance` için güvenilen taraf bölümündeki teknik profil öğesini değiştirin.
 
```xml
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
      <OutputClaim ClaimTypeReferenceId="balance" DefaultValue="" />
    </OutputClaims>
    <SubjectNamingInfo ClaimType="sub" />
  </TechnicalProfile>
</RelyingParty>
```

**ProfileEdit.xml**ve **PasswordReset.xml** kullanıcı yolculukları için bu adımı tekrarlayın.

Değiştirdiğiniz dosyaları kaydedin: *TrustFrameworkBase.xml*, ve *TrustFrameworkExtensions.xml*, *SignUpOrSignin.xml*, *ProfileEdit.xml*, ve *PasswordReset.xml*. 

## <a name="test-the-custom-policy"></a>Özel ilkeyi test edin

1. [Azure portalında](https://portal.azure.com)oturum açın.
1. Üst menüdeki **Dizin + abonelik** filtresini seçerek ve Azure AD kiracınızı içeren dizin seçerek Azure AD kiracınızı içeren dizini kullandığınızdan emin olun.
1. Azure portalının sol üst köşesindeki **tüm hizmetleri** seçin ve ardından **Uygulama kayıtlarını**arayın ve seçin.
1. **Kimlik Deneyimi Çerçevesi'ni**seçin.
1. **Özel Politika Yükle'yi**seçin ve sonra değiştirdiğiniz politika dosyalarını yükleyin: *TrustFrameworkBase.xml*ve *TrustFrameworkExtensions.xml*, *SignUpOrSignin.xml*, *ProfileEdit.xml*, ve *PasswordReset.xml*. 
1. Yüklediğiniz kaydolma veya kaydolma ilkesini seçin ve **Şimdi Çalıştır** düğmesini tıklatın.
1. Bir e-posta adresi veya Facebook hesabı kullanarak kaydolabilirsiniz.
1. Başvurunuza geri gönderilen `balance` belirteç, talebi içerir.

```json
{
  "typ": "JWT",
  "alg": "RS256",
  "kid": "X5eXk4xyojNFum1kl2Ytv8dlNP4-c57dO6QGTVBwaNk"
}.{
  "exp": 1584961516,
  "nbf": 1584957916,
  "ver": "1.0",
  "iss": "https://contoso.b2clogin.com/f06c2fe8-709f-4030-85dc-38a4bfd9e82d/v2.0/",
  "aud": "e1d2612f-c2bc-4599-8e7b-d874eaca1ee1",
  "acr": "b2c_1a_signup_signin",
  "nonce": "defaultNonce",
  "iat": 1584957916,
  "auth_time": 1584957916,
  "name": "Emily Smith",
  "email": "emily@outlook.com",
  "given_name": "Emily",
  "family_name": "Smith",
  "balance": "202.75"
  ...
}
```

## <a name="next-steps"></a>Sonraki adımlar


## <a name="next-steps"></a>Sonraki adımlar

API'lerinizi nasıl güvene aldığınızı öğrenmek için aşağıdaki makalelere bakın:

- [Walkthrough: Azure AD B2C kullanıcı yolculuğunuzdaki REST API taleplerini bir orkestrasyon adımı olarak tümleştirin](custom-policy-rest-api-claims-exchange.md)
- [RESTful API'nizi emniyete alasınız](secure-rest-api.md)
- [Referans: RESTful teknik profili](restful-technical-profile.md)
