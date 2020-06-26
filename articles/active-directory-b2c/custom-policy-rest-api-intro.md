---
title: B2C özel ilkesinde talep değişimlerinin REST API
titleSuffix: Azure AD B2C
description: Yeniden çalışan hizmetlerle etkileşim kuran Azure AD B2C Kullanıcı yolculuğu oluşturmaya giriş.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 05/18/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: fe328de9460efb743037f697c7f564e2c628278d
ms.sourcegitcommit: b56226271541e1393a4b85d23c07fd495a4f644d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/26/2020
ms.locfileid: "85388944"
---
# <a name="integrate-rest-api-claims-exchanges-in-your-azure-ad-b2c-custom-policy"></a>Azure AD B2C özel ilkenizde REST API talep alışverişlerini tümleştirin

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Azure Active Directory B2C (Azure AD B2C) olan kimlik deneyimi çerçevesi, Kullanıcı yolculuğu içindeki yeniden kullanılabilir API 'lerle tümleştirilebilir. Bu makalede, daha fazla [Teknik profil](https://identitydivision.visualstudio.com/defaultcollection/Identity%20CXP/_git/GTP?path=%2Fyoelh%2Fdocs%2Frest-api%2Frestful-technical-profile.md&version=GBmaster)kullanarak bir rehirsiz hizmetle etkileşim kuran bir Kullanıcı yolculuğu nasıl oluşturacağınız gösterilmektedir.

Azure AD B2C kullanarak, kendi iş mantığınızı Kullanıcı yolculuğuna ekleyerek kendi Reişsiz hizmetinizi ekleyebilirsiniz. Kimlik deneyimi çerçevesi, talepler alışverişi için yeniden elde edilebilir hizmetten veri gönderip alabilir. Örneğin, şunları yapabilirsiniz:

- **Kullanıcı giriş verilerini doğrulayın**. Örneğin, Kullanıcı tarafından girilen e-posta adresinin müşterinizin veritabanında var olduğunu ve bir hata sunkullanılmadığını doğrulayabilirsiniz.
- **Talep işleme**. Bir Kullanıcı ilk adını küçük harfle veya tüm büyük harflerde girerse, REST API adı yalnızca ilk harfi büyük harfle biçimlendirebilir ve Azure AD B2C olarak döndürebilir.
- **Şirket iş kolu uygulamalarıyla daha fazla tümleştirerek Kullanıcı verilerini zenginleştirin**. Yeniden deneme hizmetiniz, kullanıcının e-posta adresini alabilir, müşterinin veritabanını sorgulayabilir ve Azure AD B2C için kullanıcının bağlılık programı numarasını döndürebilir. Ardından, dönüş talepleri kullanıcının Azure AD hesabında depolanabilir, sonraki düzenleme adımlarında değerlendirilir veya erişim belirtecine dahil edilir.
- **Özel iş mantığını çalıştırın**. Anında iletme bildirimleri gönderebilir, kurumsal veritabanlarını güncelleştirebilir, bir kullanıcı geçiş işlemi çalıştırabilir, izinleri yönetebilir, veritabanlarını denetleyebilir ve diğer iş akışlarını gerçekleştirebilirsiniz.

![Yeniden takip eden bir hizmet talebi değişimi diyagramı](media/custom-policy-rest-api-intro/restful-service-claims-exchange.png)

> [!NOTE]
> Yeniden Azure AD B2C için kalan hizmetten yavaş veya hiç yanıt yoksa, zaman aşımı 30 saniyedir ve yeniden deneme sayısı 2 kez olur (toplamda 3 deneme olur). Zaman aşımı ve yeniden deneme sayısı ayarları şu anda yapılandırılamaz.

## <a name="calling-a-restful-service"></a>Yeniden bir hizmet çağırma

Etkileşim, REST API talepler ve Azure AD B2C arasında bilgi talebi alışverişi içerir. Aşağıdaki yollarla, yeniden kullanılabilen hizmetlerle tümleştirmeyi tasarlayabilirsiniz:

- **Doğrulama teknik profili**. Yeniden gerçekleşen hizmete yapılan çağrı, belirtilen [otomatik olarak onaylanan teknik profilin](self-asserted-technical-profile.md)bir [doğrulama teknik profilinde](validation-technical-profile.md) veya bir [görüntü denetiminin](display-controls.md) [doğrulama görüntüleme denetiminden](display-control-verification.md) oluşur. Doğrulama teknik profili, Kullanıcı yolculuğu ileründen önce Kullanıcı tarafından belirtilen verileri doğrular. Doğrulama teknik profiliyle şunları yapabilirsiniz:

  - Talepler REST API gönderin.
  - Talepleri doğrulayın ve kullanıcıya görüntülenen özel hata iletileri oluşturun.
  - Talepleri REST API sonraki düzenleme adımlarına geri gönderin.

- **Talep değişimi**. Doğrudan talep alışverişi, bir [Kullanıcı yolculuğunun](userjourneys.md)düzenleme adımından doğrudan bir REST API teknik profil çağırarak yapılandırılabilir. Bu tanım şu şekilde sınırlıdır:

  - Talepler REST API gönderin.
  - Talepleri doğrulayın ve uygulamaya döndürülen özel hata iletilerini oluşturun.
  - Talepleri REST API sonraki düzenleme adımlarına geri gönderin.

Özel bir ilke tarafından tanımlanan Kullanıcı yolculuğu içindeki herhangi bir adımda REST API çağrısı ekleyebilirsiniz. Örneğin, bir REST API çağırabilirsiniz:

- Oturum açma sırasında, Azure AD B2C kimlik bilgilerini doğrulamak yeterlidir.
- Oturum açma işleminden hemen sonra.
- Azure AD B2C önce, dizinde yeni bir hesap oluşturmadan önce.
- Azure AD B2C sonra dizinde yeni bir hesap oluşturur.
- Azure AD B2C önce, bir erişim belirteci vermeden önce.

![Doğrulama teknik profili koleksiyonu](media/custom-policy-rest-api-intro/validation-technical-profile.png)

## <a name="sending-data"></a>Veriler gönderiliyor

Daha fazla [Teknik profilde](restful-technical-profile.md), `InputClaims` öğesi, yeniden takip eden hizmetinize göndermek için taleplerin bir listesini içerir. Talebin adını, Restity hizmetinde tanımlanan adla eşleyebilir, varsayılan bir değer ayarlayabilir ve [talep çözücüler](claim-resolver-overview.md)kullanabilirsiniz.

Giriş taleplerinin, SendClaimsIn özniteliğini kullanarak yeniden gönderilmiş talepler sağlayıcısına nasıl gönderileceğini yapılandırabilirsiniz. Olası değerler şunlardır:

- **Gövde**, JSON BIÇIMINDE http post istek gövdesinde gönderilir.
- HTTP POST istek gövdesinde, ve ' & ' ayrılmış anahtar değeri biçiminde gönderilen **form**.
- HTTP GET istek üst bilgisinde gönderilen **üst bilgi**.
- HTTP GET isteği sorgu dizesinde gönderilen **QueryString**.

**Gövde** seçeneği yapılandırıldığında REST API teknik profili, bir uç noktaya karmaşık bir JSON yükü göndermenizi sağlar. Daha fazla bilgi için bkz. [JSON yükü gönderme](restful-technical-profile.md#send-a-json-payload).

## <a name="receiving-data"></a>Veri alma

`OutputClaims` [Restlen teknik profilinin](restful-technical-profile.md) öğesi, REST API tarafından döndürülen taleplerin bir listesini içerir. İlkenizde tanımlanan talebin adını REST API tanımlı adla eşlemeniz gerekebilir. DefaultValue özniteliğini ayarladığınız sürece REST API Identity sağlayıcısı tarafından döndürülmeyen talepleri de ekleyebilirsiniz.

Yeniden takip eden talepler sağlayıcısı tarafından ayrıştırılmış çıkış talepleri her zaman düz bir JSON gövde yanıtı ayrıştırmayı bekler, örneğin:

```json
{
  "name": "Emily Smith",
  "email": "emily@outlook.com",
  "loyaltyNumber":  1234
}
```

Çıkış talepleri aşağıdaki gibi görünmelidir:

```xml
<OutputClaims>
  <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="name" />
  <OutputClaim ClaimTypeReferenceId="email" />
  <OutputClaim ClaimTypeReferenceId="loyaltyNumber" />
</OutputClaims>
```

İç içe geçmiş bir JSON gövdesi yanıtını ayrıştırmak için, ResolveJsonPathsInJsonTokens meta verilerini true olarak ayarlayın. Çıkış talebinde, PartnerClaimType ' ı çıkış yapmak istediğiniz JSON yolu öğesi olarak ayarlayın.

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


Çıkış talepleri aşağıdaki gibi görünmelidir:

```xml
<OutputClaims>
  <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="contacts.0.person.name" />
  <OutputClaim ClaimTypeReferenceId="email" PartnerClaimType="contacts.0.person.emails.0.email" />
  <OutputClaim ClaimTypeReferenceId="loyaltyNumber" PartnerClaimType="contacts.0.person.loyaltyNumber" />
</OutputClaims>
```

## <a name="security-considerations"></a>Güvenlikle ilgili dikkat edilmesi gerekenler

Yalnızca kimliği doğrulanmış istemcilerin sizinle iletişim kurabilmesi için REST API uç noktanızı korumanız gerekir. REST API bir HTTPS uç noktası kullanmalıdır. AuthenticationType meta verilerini aşağıdaki kimlik doğrulama yöntemlerinden biri olarak ayarlayın:

- **İstemci sertifikası** , istemci sertifikası kimlik doğrulaması kullanarak erişimi kısıtlar. Yalnızca uygun sertifikalara sahip olan hizmetler, API 'nize erişebilir. İstemci sertifikasını bir Azure AD B2C Ilkesi anahtarında depoladığınızda. [İstemci sertifikalarını kullanarak, daha fazla hizmetin güvenliğini sağlama](secure-rest-api.md#https-client-certificate-authentication)hakkında daha fazla bilgi edinin.
- **Temel** , http temel kimlik doğrulamasıyla REST API güvenliğini sağlar. Yalnızca doğrulanmış kullanıcılar, Azure AD B2C dahil, API 'nize erişebilir. Kullanıcı adı ve parola Azure AD B2C ilke anahtarlarında depolanır. [Http temel kimlik doğrulaması kullanarak, yeniden yapılan hizmetlerinizin güvenliğini sağlama](secure-rest-api.md#http-basic-authentication)hakkında bilgi edinin.
- **Taşıyıcı** , bir istemci OAuth2 erişim belirteci kullanarak erişimi kısıtlar. Erişim belirteci Azure AD B2C ilke anahtarında depolanır. Daha fazla bilgi için, [taşıyıcı belirtecini kullanarak, Restsize hizmetinin güvenliğini sağlama](secure-rest-api.md#oauth2-bearer-authentication)hakkında daha fazla bilgi edinin.

## <a name="rest-api-platform"></a>REST API platform
REST API, tüm platformları temel alabilir ve her türlü programlama dilinde yazılabilir ve bu, güvenli olduğu sürece, daha fazla [Teknik profilde](restful-technical-profile.md)belirtildiği gibi talepler gönderebilir ve alabilir.

## <a name="localize-the-rest-api"></a>REST API yerelleştirin
Teknik bir teknik profilde, geçerli oturumun dilini/yerel ayarını göndermek ve gerekirse, yerelleştirilmiş bir hata iletisi vermek isteyebilirsiniz. [Talep çözümleyici](claim-resolver-overview.md)'yi kullanarak, Kullanıcı dili gibi bağlamsal bir talep gönderebilirsiniz. Aşağıdaki örnekte, bu senaryoyu gösteren, teknik bir teknik profil gösterilmektedir.

```xml
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

REST API, "kullanıcının CRM sisteminde bulunamamıştır" gibi bir hata iletisi döndürmesi gerekebilir. Bir hata oluşursa REST API bir HTTP 409 hata iletisi (çakışma yanıtı durum kodu) döndürmelidir. Daha fazla bilgi için bkz. [Restsize teknik profil](restful-technical-profile.md#returning-validation-error-message).

Bu yalnızca bir doğrulama teknik profilinden REST API teknik bir profil çağırarak elde edilebilir. Bu, kullanıcının sayfadaki verileri düzeltmesini ve sayfa gönderimi sırasında doğrulamayı yeniden çalıştırmasını sağlar.

Bu düzenleme adımında sonraki tüm doğrulama teknik profillerinin işlenmesini engellemek için bir HTTP 409 yanıtı gereklidir.

Bir Kullanıcı yolculuğuna doğrudan bir REST API teknik profile başvurmanız durumunda, Kullanıcı bağlı olan taraf uygulamasına ilgili hata iletisiyle yeniden yönlendirilir.

## <a name="publishing-your-rest-api"></a>REST API yayımlama

REST API hizmetinize yönelik istek Azure AD B2C sunuculardan gelir. REST API hizmeti, herkese açık bir HTTPS uç noktasına yayımlanmalıdır. REST API çağrıları bir Azure veri merkezi IP adresinden gelir.

REST API hizmetinizi ve temel alınan bileşenlerini (veritabanı ve dosya sistemi gibi) yüksek oranda kullanılabilir olacak şekilde tasarlayın.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla teknik profil kullanma örnekleri için aşağıdaki makalelere bakın:

- [İzlenecek yol: Kullanıcı girişinin doğrulanması sırasında Azure AD B2C Kullanıcı yolculuğunda REST API talep alışverişlerinde tümleştirin](custom-policy-rest-api-claims-validation.md)
- [İzlenecek yol: Azure Active Directory B2C içindeki özel ilkelere REST API talep alışverişi ekleme](custom-policy-rest-api-claims-validation.md)
- [REST API hizmetlerinizin güvenliğini sağlama](secure-rest-api.md)
- [Başvuru: Restuz teknik profili](restful-technical-profile.md)
