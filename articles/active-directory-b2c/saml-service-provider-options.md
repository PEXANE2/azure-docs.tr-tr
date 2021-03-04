---
title: SAML hizmeti sağlayıcısı seçeneklerini yapılandırın
title-suffix: Azure Active Directory B2C
description: Azure Active Directory B2C SAML hizmeti sağlayıcısı seçeneklerini yapılandırma
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 03/03/2021
ms.author: mimart
ms.subservice: B2C
ms.custom: fasttrack-edit
zone_pivot_groups: b2c-policy-type
ms.openlocfilehash: b9a491b639cd1b960ffe3b7164a0940770792148
ms.sourcegitcommit: 4b7a53cca4197db8166874831b9f93f716e38e30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/04/2021
ms.locfileid: "102107780"
---
# <a name="options-for-registering-a-saml-application-in-azure-ad-b2c"></a>Azure AD B2C SAML uygulaması kaydetme seçenekleri

Bu makalede, SAML uygulamanızla Azure Active Directory (Azure AD B2C) bağlanırken kullanılabilen yapılandırma seçenekleri açıklanmaktadır.

[!INCLUDE [active-directory-b2c-choose-user-flow-or-custom-policy](../../includes/active-directory-b2c-choose-user-flow-or-custom-policy.md)]

::: zone pivot="b2c-user-flow"

[!INCLUDE [active-directory-b2c-limited-to-custom-policy](../../includes/active-directory-b2c-limited-to-custom-policy.md)]

::: zone-end

::: zone pivot="b2c-custom-policy"

## <a name="encrypted-saml-assertions"></a>Şifrelenmiş SAML onayları

Uygulamanız SAML onayları 'nin şifreli bir biçimde olmasını bekliyorsa, Azure AD B2C ilkesinde şifrelemenin etkinleştirildiğinden emin olun.

Azure AD B2C, SAML onaylama işlemi şifrelemek için hizmet sağlayıcısının ortak anahtar sertifikasını kullanır. Ortak anahtar, aşağıdaki örnekte gösterildiği gibi, ' Use ' anahtar tanımlayıcısı ' Encryption ' olarak ayarlanmış olan SAML uygulamasının meta veri uç noktasında bulunmalıdır:

```xml
<KeyDescriptor use="encryption">
  <KeyInfo xmlns="https://www.w3.org/2000/09/xmldsig#">
    <X509Data>
      <X509Certificate>valid certificate</X509Certificate>
    </X509Data>
  </KeyInfo>
</KeyDescriptor>
```

Azure AD B2C şifreli onaylar göndermek üzere etkinleştirmek için, **WantsEncryptedAssertion** meta veri öğesini `true` [bağlı olan taraf teknik profilinde](relyingparty.md#technicalprofile)olarak ayarlayın. SAML onayını şifrelemek için kullanılan algoritmayı da yapılandırabilirsiniz.

```xml
<RelyingParty>
  <DefaultUserJourney ReferenceId="SignUpOrSignIn" />
  <TechnicalProfile Id="PolicyProfile">
    <DisplayName>PolicyProfile</DisplayName>
    <Protocol Name="SAML2"/>
    <Metadata>
      <Item Key="WantsEncryptedAssertions">true</Item>
    </Metadata>
   ..
  </TechnicalProfile>
</RelyingParty>
```

## <a name="identity-provider-initiated-flow"></a>Kimlik sağlayıcısı-başlatılan akış

Uygulamanız, kimlik sağlayıcısına bir SAML AuthN isteği göndermeden önce bir SAML onayı almayı bekliyorsa, kimlik sağlayıcısı tarafından başlatılan Flow Azure AD B2C yapılandırmanız gerekir.

Kimlik sağlayıcısı tarafından başlatılan akışta, oturum açma işlemi, hizmet sağlayıcısına (bağlı olan taraf uygulamanız) istenmeyen bir SAML yanıtı gönderen kimlik sağlayıcısı tarafından başlatılır (Azure AD B2C).

Şu anda, başlatma kimlik sağlayıcısı 'nın Azure AD B2C, örneğin [AD-FS](identity-provider-adfs.md)veya [Salesforce](identity-provider-salesforce-saml.md)gibi Federasyon federal bir dış kimlik sağlayıcısı olduğu senaryoları desteklemiyoruz. Yalnızca Azure AD B2C yerel hesap kimlik doğrulaması için desteklenir.

Kimlik sağlayıcısı tarafından başlatılan akışı etkinleştirmek için, **ıdpınitiated Profileenabled** meta veri öğesini `true` [bağlı olan taraf teknik profilinde](relyingparty.md#technicalprofile)olarak ayarlayın.

```xml
<RelyingParty>
  <DefaultUserJourney ReferenceId="SignUpOrSignIn" />
  <TechnicalProfile Id="PolicyProfile">
    <DisplayName>PolicyProfile</DisplayName>
    <Protocol Name="SAML2"/>
    <Metadata>
      <Item Key="IdpInitiatedProfileEnabled">true</Item>
    </Metadata>
   ..
  </TechnicalProfile>
</RelyingParty>
```

Kimlik sağlayıcısı tarafından başlatılan Flow aracılığıyla oturum açmak veya Kullanıcı kaydolmak için aşağıdaki URL 'YI kullanın:

```
https://<tenant-name>.b2clogin.com/<tenant-name>.onmicrosoft.com/<policy-name>/generic/login?EntityId=app-identifier-uri 
```

Aşağıdaki değerleri değiştirin:

* Kiracı adınızla kiracı **adı**
* **ilke-** SAML bağlı olan taraf ilkesi adınızla adlandırın
* **App-Identifier-** `identifierUris` meta veri dosyasında ile URI `https://contoso.onmicrosoft.com/app-name`

### <a name="sample-policy"></a>Örnek ilke

SAML test uygulaması ile test etmek için kullanabileceğiniz, örnek bir ilke sağlıyoruz.

1. [SAML-SP tarafından başlatılan oturum açma örnek ilkesini](https://github.com/azure-ad-b2c/saml-sp/tree/master/policy/SAML-SP-Initiated)indirin.
1. `TenantId`Kiracı adınızla eşleşecek şekilde güncelleştirin, örneğin *contoso.b2clogin.com*.
1. İlke adını *B2C_1A_signup_signin_saml* tutun.

## <a name="saml-response-signature-algorithm"></a>SAML yanıtı imza algoritması

SAML onaylama işlemi imzalamak için kullanılan imza algoritmasını yapılandırabilirsiniz. Olası değerler şunlardır,, `Sha256` `Sha384` `Sha512` veya `Sha1` . Teknik profilin ve uygulamanın aynı imza algoritmasını kullanmasından emin olun. Yalnızca sertifikanızın desteklediği algoritmayı kullanın.

`XmlSignatureAlgorithm`RelyingParty Metadata düğümündeki Metadata anahtarını kullanarak imza algoritmasını yapılandırın.

```xml
<RelyingParty>
  <DefaultUserJourney ReferenceId="SignUpOrSignIn" />
  <TechnicalProfile Id="PolicyProfile">
    <DisplayName>PolicyProfile</DisplayName>
    <Protocol Name="SAML2"/>
    <Metadata>
      <Item Key="XmlSignatureAlgorithm">Sha256</Item>
    </Metadata>
   ..
  </TechnicalProfile>
</RelyingParty>
```

## <a name="saml-response-lifetime"></a>SAML yanıtı ömrü

SAML yanıtının geçerli kaldığı sürenin uzunluğunu yapılandırabilirsiniz. `TokenLifeTimeInSeconds`SAML belirteci verenin teknik profili içindeki meta veri öğesini kullanarak yaşam süresini ayarlayın. Bu değer, `NotBefore` belirteç verme sırasında hesaplanan zaman damgasından geçebilecek saniye sayısıdır. Otomatik olarak, bu için çekilen zaman geçerli zaman. Varsayılan yaşam süresi 300 saniyedir (5 dakika).

```xml
<ClaimsProvider>
  <DisplayName>Token Issuer</DisplayName>
  <TechnicalProfiles>
    <TechnicalProfile Id="Saml2AssertionIssuer">
      <DisplayName>Token Issuer</DisplayName>
      <Protocol Name="SAML2"/>
      <OutputTokenFormat>SAML2</OutputTokenFormat>
      <Metadata>
        <Item Key="TokenLifeTimeInSeconds">400</Item>
      </Metadata>
      ...
    </TechnicalProfile>
```

## <a name="saml-response-valid-from-skew"></a>SAML yanıtı, eğriltiden geçerli

SAML yanıt zaman damgasına uygulanan zaman çarpıklığı yapılandırabilirsiniz `NotBefore` . Bu yapılandırma, iki platformdaki süreler eşitlenmemişse, bu zaman eğriliği dahilinde SAML onaylama 'nın hala geçerli kabul edilir olmasını sağlar.

`TokenNotBeforeSkewInSeconds`SAML belirteci verenin teknik profili içindeki meta veri öğesini kullanarak zaman eğriliğini ayarlayın. Eğriliği değeri saniye cinsinden varsayılan değeri 0 ile verilir. En büyük değer 3600 ' dir (bir saat).

Örneğin, `TokenNotBeforeSkewInSeconds` saniye olarak ayarlandığında `120` :

- Belirteç 13:05:10 UTC 'de verilir
- Belirteç 13:03:10 UTC 'den geçerli

```xml
<ClaimsProvider>
  <DisplayName>Token Issuer</DisplayName>
  <TechnicalProfiles>
    <TechnicalProfile Id="Saml2AssertionIssuer">
      <DisplayName>Token Issuer</DisplayName>
      <Protocol Name="SAML2"/>
      <OutputTokenFormat>SAML2</OutputTokenFormat>
      <Metadata>
        <Item Key="TokenNotBeforeSkewInSeconds">120</Item>
      </Metadata>
      ...
    </TechnicalProfile>
```

## <a name="azure-ad-b2c-issuer-id"></a>Azure AD B2C veren KIMLIĞI

Farklı değerlere bağlı birden çok SAML uygulamanız varsa `entityID` , `issueruri` bağlı olan taraf dosyanızdaki değeri geçersiz kılabilirsiniz. Veren URI 'yi geçersiz kılmak için, teknik profili temel dosyadaki "Saml2AssertionIssuer" KIMLIĞIYLE kopyalayın ve değeri geçersiz kılın `issueruri` .

> [!TIP]
> `<ClaimsProviders>`Temel öğeden bölümü kopyalayın ve bu öğeleri talep sağlayıcısı içinde koruyun: `<DisplayName>Token Issuer</DisplayName>` , `<TechnicalProfile Id="Saml2AssertionIssuer">` ve `<DisplayName>Token Issuer</DisplayName>` .
 
Örnek:

```xml
   <ClaimsProviders>   
    <ClaimsProvider>
      <DisplayName>Token Issuer</DisplayName>
      <TechnicalProfiles>
        <TechnicalProfile Id="Saml2AssertionIssuer">
          <DisplayName>Token Issuer</DisplayName>
          <Metadata>
            <Item Key="IssuerUri">customURI</Item>
          </Metadata>
        </TechnicalProfile>
      </TechnicalProfiles>
    </ClaimsProvider>
  </ClaimsProviders>
  <RelyingParty>
    <DefaultUserJourney ReferenceId="SignUpInSAML" />
    <TechnicalProfile Id="PolicyProfile">
      <DisplayName>PolicyProfile</DisplayName>
      <Protocol Name="SAML2" />
      <Metadata>
     …
```

## <a name="session-management"></a>Oturum yönetimi

`UseTechnicalProfileForSessionManagement`Öğesini ve [Samlssosessionprovider](custom-policy-reference-sso.md#samlssosessionprovider)öğesini kullanarak Azure AD B2C ve SAML bağlı olan taraf uygulaması arasındaki oturumu yönetebilirsiniz.

## <a name="debug-the-saml-protocol"></a>SAML protokolünde hata ayıklama

Hizmet sağlayıcınızda tümleştirmeyi yapılandırmaya ve hata ayıklamanıza yardımcı olmak için, SAML protokolü için bir tarayıcı uzantısı kullanabilirsiniz; Örneğin, Chrome için [SAML DevTools uzantısı](https://chrome.google.com/webstore/detail/saml-devtools-extension/jndllhgbinhiiddokbeoeepbppdnhhio) , Firefox için [SAML izleyici](https://addons.mozilla.org/es/firefox/addon/saml-tracer/) veya [Edge ya da IE geliştirici araçları](https://techcommunity.microsoft.com/t5/microsoft-sharepoint-blog/gathering-a-saml-token-using-edge-or-ie-developer-tools/ba-p/320957).

Bu araçları kullanarak, uygulamanız ve Azure AD B2C arasındaki tümleştirmeyi kontrol edebilirsiniz. Örnek:

* SAML isteğinin imza içerip içermediğini denetleyin ve yetkilendirme isteğinde oturum açmak için hangi algoritmanın kullanılacağını saptayın.
* Azure AD B2C bir hata iletisi döndürdüğünden emin olun.
* Onaylama bölümünün şifrelendiğini kontrol edin.

## <a name="next-steps"></a>Sonraki adımlar

- [OASSıS Web sitesinde SAML Protokolü](https://www.oasis-open.org/)hakkında daha fazla bilgi edinin.
- [Azure AD B2C GitHub topluluk DEPOSUNDAN](https://github.com/azure-ad-b2c/saml-sp-tester)SAML test Web uygulamasını alın.

<!-- LINKS - External -->
[samltest]: https://aka.ms/samltestapp

::: zone-end
