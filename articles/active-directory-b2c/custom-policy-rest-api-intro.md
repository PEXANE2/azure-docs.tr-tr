---
title: REST API, B2C özel politikasındaki değişimleri talep ediyor
titleSuffix: Azure AD B2C
description: RESTful hizmetleriyle etkileşimedebilen bir Azure AD B2C kullanıcı yolculuğu oluşturmaya giriş.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 03/23/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 6a6cc8e5931f3e29c242f51a6e062441953228ad
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80337421"
---
# <a name="integrate-rest-api-claims-exchanges-in-your-azure-ad-b2c-custom-policy"></a>REST API talep alışverişini Azure AD B2C özel politikanızda tümleştirin

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Azure Active Directory B2C 'nin (Azure AD B2C) altında yatan Kimlik Deneyimi Çerçevesi, kullanıcı yolculuğu içinde YENIDEN Kullanılabilir API'lerle tümleşebilir. Bu makalede, [restful teknik profili](https://identitydivision.visualstudio.com/defaultcollection/Identity%20CXP/_git/GTP?path=%2Fyoelh%2Fdocs%2Frest-api%2Frestful-technical-profile.md&version=GBmaster)kullanarak bir RESTful hizmeti ile etkileşim edebilen bir kullanıcı yolculuğu oluşturmak için nasıl gösterir.

Azure AD B2C'yi kullanarak, kendi RESTful hizmetinizi arayarak kullanıcı yolculuğuna kendi iş mantığınızı ekleyebilirsiniz. Kimlik Deneyimi Çerçevesi, talep alışverişinde bulunmak için restful hizmetinizden veri gönderebilir ve alabilir. Örneğin, şunları yapabilirsiniz:

- **Kullanıcı giriş verilerini doğrulayın.** Örneğin, kullanıcı tarafından sağlanan e-posta adresinin müşterinizin veritabanında olduğunu doğrulayabilir ve değilse bir hata sunabilirsiniz.
- **İşlem talepleri**. Bir kullanıcı ilk adını tüm küçük veya büyük harflerle girerse, REST API'niz adı yalnızca ilk harfle biçimlendirebilir ve Azure AD B2C'ye döndürebilir.
- **Kurumsal iş yeri uygulamalarıyla daha fazla entegre ederek kullanıcı verilerini zenginleştirin.** RESTful hizmetiniz kullanıcının e-posta adresini alabilir, müşterinin veritabanını sorgulayabilir ve kullanıcının sadakat numarasını Azure AD B2C'ye döndürebilir. Daha sonra iade talepleri kullanıcının Azure REKLAM hesabında depolanabilir, sonraki düzenleme adımlarında değerlendirilebilir veya erişim belirtecine dahil edilebilir.
- **Özel iş mantığı çalıştırın.** Anında iletme bildirimleri gönderebilir, şirket veritabanlarını güncelleştirebilir, kullanıcı geçiş işlemini çalıştırabilir, izinleri yönetebilir, veritabanlarını denetleyebilir ve diğer iş akışlarını gerçekleştirebilirsiniz.

![RESTful hizmet talep değişimi diyagramı](media/custom-policy-rest-api-intro/restful-service-claims-exchange.png)

## <a name="calling-a-restful-service"></a>RESTful hizmeti arama

Etkileşim, REST API talepleri ile Azure AD B2C arasında bilgi alışverişi taleplerini içerir. RESTful hizmetleriyle tümleştirmeyi aşağıdaki yollarla tasarlayabilirsiniz:

- **Doğrulama teknik profili**. RESTful hizmetine çağrı, belirtilen kendi kendini ileri [süren teknik profilin](self-asserted-technical-profile.md) [doğrulama teknik profili](validation-technical-profile.md) veya bir ekran [denetiminin](display-controls.md) [doğrulama ekran denetimi](display-control-verification.md) içinde gerçekleşir. Doğrulama teknik profili, kullanıcı yolculuğu ilerlemeden önce kullanıcı tarafından sağlanan verileri doğrular. Doğrulama teknik profili ile şunları yapabilirsiniz:

  - İddiaları REST API'nize gönderin.
  - Talepleri doğrulayın ve kullanıcıya görüntülenen özel hata iletileri atın.
  - REST API'deki talepleri sonraki orkestrasyon adımlarına geri gönderin.

- **İddia değişimi**. Doğrudan talep değişimi, doğrudan bir [kullanıcı yolculuğunun](userjourneys.md)bir orkestrasyon adımından BIR REST API teknik profilini arayarak yapılandırılabilir. Bu tanım aşağıdakiile sınırlıdır:

  - İddiaları REST API'nize gönderin.
  - Talepleri doğrulayın ve uygulamaya döndürülen özel hata iletileri atın.
  - REST API'deki talepleri sonraki orkestrasyon adımlarına geri gönderin.

Özel bir ilke ile tanımlanan kullanıcı yolculuğundaki herhangi bir adımda bir REST API çağrısı ekleyebilirsiniz. Örneğin, REST API'yi arayabilirsiniz:

- Oturum açma sırasında, Azure AD B2C kimlik bilgilerini doğrulamadan hemen önce.
- Oturum açmadan hemen sonra.
- Azure AD B2C dizinde yeni bir hesap oluşturmadan önce.
- Azure AD B2C dizinde yeni bir hesap oluşturduktan sonra.
- Azure AD B2C erişim jetonu vermeden önce.

![Doğrulama teknik profil koleksiyonu](media/custom-policy-rest-api-intro/validation-technical-profile.png)

## <a name="sending-data"></a>Veri gönderme

[RESTful teknik](restful-technical-profile.md)profilinde, `InputClaims` öğe, restful hizmetinize göndermek için taleplerin bir listesini içerir. Talebinizin adını, YENIDEN kullanım hizmetinde tanımlanan ada eşleyebilir, varsayılan değer belirleyebilir ve [talep çözümleyicilerini](claim-resolver-overview.md)kullanabilirsiniz.

SendClaimsIn özniteliğini kullanarak giriş taleplerinin geri alınabilir talepler sağlayıcısına nasıl gönderilmiş olduğunu yapılandırabilirsiniz. Olası değerler şunlardır:

- **Gövde**, JSON formatında HTTP POST istek gövdesi gönderildi.
- **Form**, http POST istek gövdesinde bir ampersand '&' ayrılmış anahtar değeri biçiminde gönderildi.
- **Üstbilgi**, HTTP GET istek üstbilgisinde gönderilir.
- **QueryString**, HTTP GET istek sorgu dizesi gönderildi.

**Gövde** seçeneği yapılandırıldığında, REST API teknik profili karmaşık bir JSON yükünü bitiş noktasına göndermenize olanak tanır. Daha fazla bilgi için [json yükü gönder'e](restful-technical-profile.md#send-a-json-payload)bakın.

## <a name="receiving-data"></a>Veri alma

`OutputClaims` [RESTful teknik profilinin](restful-technical-profile.md) öğesi, REST API tarafından döndürülen taleplerin bir listesini içerir. İlkenizde tanımlanan talebin adını REST API'sinde tanımlanan adla eşlemeniz gerekebilir. Varsayılan Değer özniteliğini ayarladığınızda, REST API kimlik sağlayıcısı tarafından döndürülen talepleri de ekleyebilirsiniz.

RESTful claims sağlayıcısı tarafından ayrıştırılan çıktı talepleri her zaman aşağıdakiler gibi düz bir JSON Body yanıtını ayrıştırmayı bekler:

```json
{
  "name": "Emily Smith",
  "email": "emily@outlook.com",
  "loyaltyNumber":  1234
}
```

Çıktı talepleri aşağıdaki gibi görünmelidir:

```xml
<OutputClaims>
  <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="name" />
  <OutputClaim ClaimTypeReferenceId="email" />
  <OutputClaim ClaimTypeReferenceId="loyaltyNumber" />
</OutputClaims>
```

İç içe json vücut yanıtını ayrıştırmak için ResolveJsonPathsInJsonTokens meta verilerini doğru olarak ayarlayın. Çıktı talebinde, PartnerClaimType'ı çıktıalmak istediğiniz JSON yol öğesine ayarlayın.

```json
"contacts": [
  {
    "id": "MAINCONTACT_1",
    "person": {
      "name": "Emily Smith",
      "loyaltyNumber":  1234,
      "emails": [
        {
          "id": "EMAIL_1",
          "type": "WORK",
          "email": "email@domain.com"
        }
      ]
    }
  }
],
```


Çıktı talepleri aşağıdaki gibi görünmelidir:

```xml
<OutputClaims>
  <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="contacts.0.person.name" />
  <OutputClaim ClaimTypeReferenceId="email" PartnerClaimType="contacts.0.person.emails.0.email" />
  <OutputClaim ClaimTypeReferenceId="loyaltyNumber" PartnerClaimType="contacts.0.person.loyaltyNumber" />
</OutputClaims>
```

## <a name="security-considerations"></a>Güvenlik konuları

Yalnızca kimlik doğrulaması verilen istemcilerin onunla iletişim kurabilmeleri için REST API bitiş noktanızı korumanız gerekir. REST API bir HTTPS bitiş noktası kullanmalıdır. Kimlik Doğrulama Türü meta verilerini aşağıdaki kimlik doğrulama yöntemlerinden birine ayarlayın:

- **İstemci sertifikası** istemci sertifikası kimlik doğrulamasını kullanarak erişimi kısıtlar. Yalnızca uygun sertifikalara sahip hizmetler API'nize erişebilir. İstemci sertifikasını Azure AD B2C İlke Anahtarı'nda saklarsınız. [İstemci sertifikalarını kullanarak RESTful hizmetinizi nasıl güvence altına alasınız](secure-rest-api.md#https-client-certificate-authentication)hakkında daha fazla bilgi edinin.
- **Basic,** HTTP temel kimlik doğrulaması ile REST API'sini güvence altına alar. API'nize yalnızca Azure AD B2C de dahil olmak üzere doğrulanmış kullanıcılar erişebilir. Kullanıcı adı ve parola Azure AD B2C ilke anahtarlarında depolanır. [HTTP temel kimlik doğrulamasını kullanarak yeni hizmetlerinizin](secure-rest-api.md#http-basic-authentication)nasıl güvenli hale yapılacağını öğrenin.
- **Taşıyıcı,** istemci OAuth2 erişim jetonunu kullanarak erişimi kısıtlar. Erişim belirteci, Azure AD B2C ilke anahtarında depolanır. [Bearer belirteci'ni kullanarak RESTful hizmetinizi nasıl güvence altına alasınız](secure-rest-api.md#oauth2-bearer-authentication)hakkında daha fazla bilgi edinin.

## <a name="rest-api-platform"></a>REST API platformu
REST API'niz herhangi bir platforma dayalı olabilir ve güvenli olduğu sürece herhangi bir programlama dilinde yazılabilir ve [restful teknik profilinde](restful-technical-profile.md)belirtildiği gibi talepleri gönderip alabilir.

## <a name="localize-the-rest-api"></a>REST API'sini yerelleştirin
Yeniden bir teknik profilde, geçerli oturumun dilini/yerel bağlantısını göndermek ve gerekirse yerelleştirilmiş bir hata iletisi yükseltmek isteyebilirsiniz. Talep [çözümleyicisini](claim-resolver-overview.md)kullanarak, kullanıcı dili gibi bağlamsal bir talep gönderebilirsiniz. Aşağıdaki örnekte, bu senaryoyu gösteren yeni bir teknik profil gösterilmektedir.

```XML
<TechnicalProfile Id="REST-ValidateUserData">
  <DisplayName>Validate user input data</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  <Metadata>
    <Item Key="ServiceUrl">https://your-app.azurewebsites.net/api/identity</Item>
    <Item Key="AuthenticationType">None</Item>
    <Item Key="SendClaimsIn">Body</Item>
    <Item Key="IncludeClaimResolvingInClaimsHandling">true</Item>
  </Metadata>
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="userLanguage" DefaultValue="{Culture:LCID}" AlwaysUseDefaultValue="true" />
    <InputClaim ClaimTypeReferenceId="email" PartnerClaimType="emailAddress" />
  </InputClaims>
  <UseTechnicalProfileForSessionManagement ReferenceId="SM-Noop" />
</TechnicalProfile>
```

## <a name="handling-error-messages"></a>Hata iletilerini işleme

REST API'nizin "Kullanıcı CRM sisteminde bulunamadı" gibi bir hata iletisi döndürmesi gerekebilir. Bir hata oluşursa, REST API bir HTTP 409 hata iletisi (Çakışma yanıt durum kodu) döndürmelidir. Daha fazla bilgi [için, RESTful teknik profiline](https://identitydivision.visualstudio.com/defaultcollection/Identity%20CXP/_git/GTP?path=%2Fyoelh%2Fdocs%2Frest-api%2Frestful-technical-profile.md&version=GBmaster&anchor=returning-error-message)bakın.

Bu, yalnızca doğrulama teknik profilinden REST API teknik profilini arayarak elde edilebilir. Bu, kullanıcının sayfadaki verileri düzeltmesine ve sayfa gönderimi sırasında doğrulamayı yeniden çalıştırmasına olanak tanır.

Bu düzenleme adımı nda sonraki doğrulama teknik profillerinin işlenmesini önlemek için bir HTTP 409 yanıtı gereklidir.

Bir REST API teknik profiline doğrudan bir kullanıcı yolculuğundan başvurursanız, kullanıcı ilgili hata iletisiyle birlikte güvenilen taraf uygulamasına geri yönlendirilir.

## <a name="publishing-your-rest-api"></a>REST API'nizi yayınlama

REST API hizmetinizin isteği Azure AD B2C sunucularından gelir. REST API hizmeti, herkese açık bir HTTPS bitiş noktasında yayınlanmalıdır. REST API çağrıları bir Azure veri merkezi IP adresinden gelecektir.

REST API hizmetinizi ve altta yatan bileşenlerini (veritabanı ve dosya sistemi gibi) son derece kullanılabilir olacak şekilde tasarlayın.

## <a name="next-steps"></a>Sonraki adımlar

ReSTful teknik profil kullanma örnekleri için aşağıdaki makalelere bakın:

- [Walkthrough: Kullanıcı girişinin doğrulanması olarak Azure AD B2C kullanıcı yolculuğunuzdaki REST API talep alışverişini tümleştirin](custom-policy-rest-api-claims-validation.md)
- [Walkthrough: Azure Active Directory B2C'deki özel ilkelere REST API talep alışverişi ekleme](custom-policy-rest-api-claims-validation.md)
- [REST API hizmetlerinizi güvence altına ala](secure-rest-api.md)
- [Referans: RESTful teknik profili](restful-technical-profile.md)