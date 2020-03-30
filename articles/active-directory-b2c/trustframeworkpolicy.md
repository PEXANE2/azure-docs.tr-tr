---
title: TrustFrameworkPolicy - Azure Active Directory B2C | Microsoft Dokümanlar
description: Azure Active Directory B2C'de özel bir ilkenin TrustFrameworkPolicy öğesini belirtin.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 01/31/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: c964a7bde0b7db9357c73fc79d2df3170075fcc1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78186395"
---
# <a name="trustframeworkpolicy"></a>TrustFrameworkPolicy

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Özel bir ilke, hiyerarşik bir zincirde birbirine başvuran bir veya daha fazla XML biçimli dosya olarak temsil edilir. XML öğeleri, talep şeması, talep dönüşümleri, içerik tanımları, talep sağlayıcılar, teknik profiller, kullanıcı yolculuğu ve düzenleme adımları gibi ilkenin öğelerini tanımlar. Her ilke dosyası, bir ilke dosyasının üst düzey **TrustFrameworkPolicy** öğesi içinde tanımlanır.

```XML
<TrustFrameworkPolicy
  xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance"
  xmlns:xsd="https://www.w3.org/2001/XMLSchema"
  xmlns="http://schemas.microsoft.com/online/cpim/schemas/2013/06"
  PolicySchemaVersion="0.3.0.0"
  TenantId="mytenant.onmicrosoft.com"
  PolicyId="B2C_1A_TrustFrameworkBase"
  PublicPolicyUri="http://mytenant.onmicrosoft.com/B2C_1A_TrustFrameworkBase">
  ...
```


**TrustFrameworkPolicy** öğesi aşağıdaki öznitelikleri içerir:

| Öznitelik | Gerekli | Açıklama |
|---------- | -------- | ----------- |
| PolicySchemaVersion | Evet | İlke yürütmek için kullanılacak şema sürümü. Değer,`0.3.0.0` |
| KiracıObjectId | Hayır | Azure Active Directory B2C (Azure AD B2C) kiracısının benzersiz nesne tanımlayıcısı. |
| TenantId | Evet | Bu iipolitikasın ait olduğu kiracının benzersiz tanımlayıcısı. |
| PolicyId | Evet | İlke için benzersiz tanımlayıcı. Bu *tanımlayıcı, B2C_1A_* |
| PublicPolicyUri | Evet | Kiracı kimliği ve ilke kimliği nin birleşimi olan ilke için URI. |
| Dağıtım Modu | Hayır | Olası değerler: `Production` `Development`, veya . `Production` varsayılan değerdir. İlkenizin hata sını ayıklamak için bu özelliği kullanın. Daha fazla bilgi için, [Toplama Günlükleri'ne](troubleshoot-with-application-insights.md)bakın. |
| KullanıcıJourneyRecorderEndpoint | Hayır | **DeploymentMode** ' a ayarlandığında kullanılan `Development`bitiş noktası. Değer. `urn:journeyrecorder:applicationinsights` Daha fazla bilgi için, [Toplama Günlükleri'ne](troubleshoot-with-application-insights.md)bakın. |


Aşağıdaki örnekte **TrustFrameworkPolicy** öğesinin nasıl belirtilen gösterilmektedir:

``` XML
<TrustFrameworkPolicy
   xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance"
   xmlns:xsd="https://www.w3.org/2001/XMLSchema"
   xmlns="http://schemas.microsoft.com/online/cpim/schemas/2013/06"
   PolicySchemaVersion="0.3.0.0"
   TenantId="mytenant.onmicrosoft.com"
   PolicyId="B2C_1A_TrustFrameworkBase"
   PublicPolicyUri="http://mytenant.onmicrosoft.com/B2C_1A_TrustFrameworkBase">
```

## <a name="inheritance-model"></a>Kalıtım modeli

Bu tür ilke dosyaları genellikle bir kullanıcı yolculuğunda kullanılır:

- Tanımların çoğunu içeren **bir Temel** dosya. Sorun giderme ve ilkelerinizin uzun süreli bakımına yardımcı olmak için, bu dosyada en az sayıda değişiklik yapmanız önerilir.
- Kiracınız için benzersiz yapılandırma değişikliklerini tutan bir **Uzantılar** dosyası. Bu ilke dosyası Temel dosyadan türetilmiştir. Yeni işlevler eklemek veya varolan işlevselliği geçersiz kılmak için bu dosyayı kullanın. Örneğin, yeni kimlik sağlayıcıları ile fetorate için bu dosyayı kullanın.
- Web, mobil veya masaüstü uygulamalarınız gibi güvenen taraf uygulaması tarafından doğrudan çağrılan tek görev odaklı dosya olan **Güvenen Taraf (RP)** dosyasıdır. Kaydolma veya kaydolma, parola sıfırlama veya profil ayarlama gibi her benzersiz görev, kendi RP ilke dosyası gerektirir. Bu ilke dosyası Uzantılar dosyasından türetilmiştir.

Güvenen bir taraf uygulaması, belirli bir görevi yürütmek için RP ilke dosyasını çağırır. Örneğin, oturum açma akışını başlatmak için. Azure AD B2C'deki Kimlik Deneyimi Çerçevesi, geçerli ilkeyi birleştirmek için önce Temel dosyadan, sonra Uzanlar dosyasından ve son olarak da RP ilkesi dosyasından tüm öğeleri ekler. RP dosyasındaki aynı tür ve ada daki öğeler Uzantılar'daki bu öğeleri geçersiz kılar ve Uzantılar Tabanı geçersiz kılar. Aşağıdaki diyagram, ilke dosyaları ile güvenilen taraf uygulamaları arasındaki ilişkiyi gösterir.

![Güven çerçevesi ilkesi devralma modelini gösteren diyagram](./media/trustframeworkpolicy/custom-policy-Inheritance-model.png)

Kalıtım modeli aşağıdaki gibidir:

- Üst politika ve alt ilkesi aynı şema vardır.
- Herhangi bir düzeydeki alt ilke, üst politikadan devralabilir ve yeni öğeler ekleyerek bu politikayı genişletebilir.
- Düzey sayısında bir sınır yoktur.

Daha fazla bilgi için [bkz.](custom-policy-get-started.md)

## <a name="base-policy"></a>Temel ilke

Bir ilkeyi başka bir ilkeden devralmak için, **bir BasePolicy** öğesinin ilke dosyasının **TrustFrameworkPolicy** öğesi altında bildirilmesi gerekir. **BasePolicy** öğesi, bu ilkenin türetildiği temel ilke için bir başvurudur.

**BasePolicy** öğesi aşağıdaki öğeleri içerir:

| Öğe | Oluşum | Açıklama |
| ------- | ----------- | --------|
| TenantId | 1:1 | Azure AD B2C kiracınızın tanımlayıcısı. |
| PolicyId | 1:1 | Ana politikanın tanımlayıcısı. |


Aşağıdaki örnekte, temel ilke nasıl belirtililir gösterilmektedir. Bu **B2C_1A_TrustFrameworkExtensions** ilkesi **B2C_1A_TrustFrameworkBase** ilkesinden türetilmiştir.

``` XML
<TrustFrameworkPolicy
   xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance"
   xmlns:xsd="https://www.w3.org/2001/XMLSchema"
   xmlns="http://schemas.microsoft.com/online/cpim/schemas/2013/06"
   PolicySchemaVersion="0.3.0.0"
   TenantId="mytenant.onmicrosoft.com"
   PolicyId="B2C_1A_TrustFrameworkExtensions"
   PublicPolicyUri="http://mytenant.onmicrosoft.com/B2C_1A_TrustFrameworkExtensions">

  <BasePolicy>
    <TenantId>yourtenant.onmicrosoft.com</TenantId>
    <PolicyId>B2C_1A_TrustFrameworkBase</PolicyId>
  </BasePolicy>
  ...
</TrustFrameworkPolicy>
```

## <a name="policy-execution"></a>İlke yürütme

Web, mobil veya masaüstü uygulaması gibi güvenilen bir taraf uygulaması, [güvenen tarafı (RP) ilkesini](relyingparty.md)çağırır. RP ilke dosyası oturum açma, parolayı sıfırlama veya profili düzenleme gibi belirli bir görevi yürütür. RP ilkesi, bağlı taraf uygulamasının verilen belirteç lerin bir parçası olarak aldığı taleplerin listesini yapılandırır. Birden çok uygulama aynı ilkeyi kullanabilir. Tüm uygulamalar talepleri ile aynı belirteci alır ve kullanıcı aynı kullanıcı yolculuğundan geçer. Tek bir uygulama birden çok ilke kullanabilir.

RP ilke dosyasının içinde, [UserJourney'yi](userjourneys.md)işaret eden **DefaultUserJourney** öğesini belirtirsiniz. Kullanıcı yolculuğu genellikle Temel veya Uzantılar ilkesinde tanımlanır.

B2C_1A_signup_signin politikası:

```XML
<RelyingParty>
  <DefaultUserJourney ReferenceId="SignUpOrSignIn">
  ...
```

B2C_1A_TrustFrameWorkBase veya B2C_1A_TrustFrameworkExtensionPolicy:

```XML
<UserJourneys>
  <UserJourney Id="SignUpOrSignIn">
  ...
```

Kullanıcı yolculuğu, bir kullanıcının neler yaşadığının iş mantığını tanımlar. Her kullanıcı yolculuğu, kimlik doğrulama ve bilgi toplama açısından sırayla bir dizi eylem gerçekleştiren bir dizi düzenleme adımı kümesidir.

[Başlangıç paketindeki](custom-policy-get-started.md#custom-policy-starter-pack) **SocialAndLocalAccounts** politika dosyası SignUpOrSignIn, ProfileEdit, PasswordReset kullanıcı yolculuklarını içerir. E-posta adresini değiştirme veya bir sosyal hesabı bağlama ve bağlantısını kaldırma gibi diğer senaryolar için daha fazla kullanıcı yolculuğu ekleyebilirsiniz.

Orkestrasyon adımları [Teknik Profil'i](technicalprofiles.md)çağırabilir. Teknik profil, farklı türde taraflarla iletişim kurmak için yerleşik bir mekanizmaiçeren bir çerçeve sağlar. Örneğin, teknik bir profil diğerleri arasında bu eylemleri gerçekleştirebilir:

- Bir kullanıcı deneyimi oluşturma.
- Kullanıcıların Facebook, Microsoft hesabı, Google, Salesforce veya başka bir kimlik sağlayıcısı gibi sosyal veya kurumsal bir hesapla oturum açmalarına izin verin.
- MFA için telefon doğrulamasını ayarlayın.
- Azure AD B2C kimlik deposuna ve adresinden veri okuma ve yazma.
- Özel bir Dinlendirici API hizmetini arayın.

![İlke yürütme akışını gösteren diyagram](./media/trustframeworkpolicy/custom-policy-execution.png)

 **TrustFrameworkPolicy** öğesi aşağıdaki öğeleri içerir:

- Yukarıda belirtildiği gibi BasePolicy
- [BuildingBlocks](buildingblocks.md)
- [ClaimsProviders](claimsproviders.md)
- [UserJourneys](userjourneys.md)
- [RelyingParty](relyingparty.md)
