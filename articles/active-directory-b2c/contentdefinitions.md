---
title: ContentDefinitions
titleSuffix: Azure AD B2C
description: Azure Active Directory B2C'de özel bir ilkenin ContentDefinitions öğesini belirtin.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 02/20/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 074a0a39090e22a29f778fc1c99060848c6bfd99
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80051502"
---
# <a name="contentdefinitions"></a>ContentDefinitions

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Herhangi bir kendini ileri teknik [profil](self-asserted-technical-profile.md)görünümünü ve hissediyorum özelleştirebilirsiniz. Azure Active Directory B2C (Azure AD B2C), müşterinizin tarayıcısında kod çalıştırır ve Çapraz Kaynak Paylaşımı (CORS) adı verilen modern bir yaklaşım kullanır.

Kullanıcı arabirimini özelleştirmek için **ContentDefinition** öğesinde özelleştirilmiş HTML içeriğine sahip bir URL belirtirsiniz. Kendi kendine öne çıkan teknik profil veya **OrchestrationStep'te,** bu içerik tanımı tanımlayıcısını işaret edersiniz. İçerik tanımı, yüklenmesi gereken yerelleştirilmiş kaynakların listesini belirten bir **Yerelleştirilmiş Kaynaklar Başvuruları** öğesi içerebilir. Azure AD B2C, kullanıcı arabirimi öğelerini URL'nizden yüklenen HTML içeriğiyle birleştirir ve sayfayı kullanıcıya görüntüler.

**ContentDefinitions** öğesi, kullanıcı yolculuğunda kullanılabilecek HTML5 şablonlarına URL'ler içerir. HTML5 sayfası URI, belirtilen bir kullanıcı arabirimi adımı için kullanılır. Örneğin, oturum açma veya kaydolma, parola sıfırlama veya hata sayfaları. HTML5 dosyası için LoadUri'yi geçersiz kılarak görünüm ve his değiştirebilirsiniz. İhtiyaçlarınıza göre yeni içerik tanımları oluşturabilirsiniz. Bu öğe, [Yerelleştirme](localization.md) öğesinde belirtilen yerelleştirme tanımlayıcısına yerelleştirilmiş kaynaklar başvurusu içerebilir.

Aşağıdaki örnekte içerik tanım tanımlayıcısı ve yerelleştirilmiş kaynakların tanımı gösterilmektedir:

```XML
<ContentDefinition Id="api.localaccountsignup">
  <LoadUri>~/tenant/default/selfAsserted.cshtml</LoadUri>
  <RecoveryUri>~/common/default_page_error.html</RecoveryUri>
  <DataUri>urn:com:microsoft:aad:b2c:elements:selfasserted:1.1.0</DataUri>
  <Metadata>
    <Item Key="DisplayName">Local account sign up page</Item>
  </Metadata>
  <LocalizedResourcesReferences MergeBehavior="Prepend">
    <LocalizedResourcesReference Language="en" LocalizedResourcesReferenceId="api.localaccountsignup.en" />
    <LocalizedResourcesReference Language="es" LocalizedResourcesReferenceId="api.localaccountsignup.es" />
    ...
```

**LocalAccountSignWithLogonEmail** kendi kendine ileri teknik profil meta verileri içerik tanımı tanımlayıcı **İçerikDefinitionReferenceId** için ayarlanmış içerir`api.localaccountsignup`

```XML
<TechnicalProfile Id="LocalAccountSignUpWithLogonEmail">
  <DisplayName>Email signup</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.SelfAssertedAttributeProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  <Metadata>
    <Item Key="ContentDefinitionReferenceId">api.localaccountsignup</Item>
    ...
  </Metadata>
  ...
```

## <a name="contentdefinition"></a>Contentdefinition

**ContentDefinition** öğesi aşağıdaki özniteliği içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| Kimlik | Evet | İçerik tanımı için tanımlayıcı. Değer, daha sonra bu sayfada **İçerik tanımı disleri** bölümünde belirtilen bir değerdir. |

**ContentDefinition** öğesi aşağıdaki öğeleri içerir:

| Öğe | Oluşum | Açıklama |
| ------- | ----------- | ----------- |
| Loaduri | 1:1 | İçerik tanımı için HTML5 sayfasının URL'sini içeren dize. |
| Kurtarma Uri | 1:1 | İçerik tanımıyla ilgili bir hata görüntülemek için HTML sayfasının URL'sini içeren dize. Şu anda kullanılmaz, `~/common/default_page_error.html`değeri olmalıdır. |
| Datauri | 1:1 | Kullanıcı deneyiminin adım için çağrıda bulunmasını sağlayan bir HTML dosyasının göreli URL'sini içeren dize. |
| Meta Veriler | 0:1 | İçerik tanımıtarafından kullanılan meta verileri içeren anahtar/değer çiftleri topluluğu. |
| Yerelleştirilmiş KaynaklarReferanslar | 0:1 | Yerelleştirilmiş kaynak başvuruları koleksiyonu. Kullanıcı arabiriminin ve talep özniteliğinin yerelleştirilmesini özelleştirmek için bu öğeyi kullanın. |

### <a name="datauri"></a>Datauri

**DataUri** öğesi sayfa tanımlayıcısını belirtmek için kullanılır. Azure AD B2C, Web-üfe öğelerini ve istemci tarafı JavaScript'i yüklemek ve başlatmak için sayfa tanımlayıcısını kullanır. Değerin biçimi `urn:com:microsoft:aad:b2c:elements:page-name:version`. Aşağıdaki tabloda kullanabileceğiniz sayfa tanımlayıcıları listelenizdir.

| Sayfa tanımlayıcısı | Açıklama |
| ----- | ----------- |
| `globalexception` | Bir özel durum veya bir hatayla karşılaşıldığında bir hata sayfası görüntüler. |
| `providerselection`, `idpselection` | Oturum açma sırasında kullanıcıların aralarından seçim yapabilecekleri kimlik sağlayıcılarını listeler.  |
| `unifiedssp` | E-posta adresini veya kullanıcı adını temel alan yerel bir hesapla oturum açma formu görüntüler. Bu değer aynı zamanda "oturum açmamı sağla" ve "Parolanızı unuttunuz mu?" Bağlantı. |
| `unifiedssd` | E-posta adresini veya kullanıcı adını temel alan yerel bir hesapla oturum açma formu görüntüler. |
| `multifactor` | Kaydolma veya oturum açma sırasında metin veya ses kullanarak telefon numaralarını doğrular. |
| `selfasserted` | Kullanıcıdan veri toplamak için bir form görüntüler. Örneğin, kullanıcıların profillerini oluşturmalarına veya güncelleştirmelerine olanak tanır. |

### <a name="select-a-page-layout"></a>Sayfa düzeni seçme

[JavaScript istemci tarafı kodunu,](javascript-samples.md) sayfa türü `contract` `elements` yle arasına ekleyerek etkinleştirebilirsiniz. Örneğin, `urn:com:microsoft:aad:b2c:elements:contract:page-name:version`.

[!INCLUDE [active-directory-b2c-public-preview](../../includes/active-directory-b2c-public-preview.md)]

Sürüm [version](page-layout.md) bölümü, `DataUri` ilkenizdeki kullanıcı arabirimi öğeleri için HTML, CSS ve JavaScript içeren içerik paketini belirtir. JavaScript istemci tarafı kodunu etkinleştirmek istiyorsanız, JavaScript'inizi temel aldığınız öğeler değişmez olmalıdır. Değişmez değillerse, herhangi bir değişiklik kullanıcı sayfalarınızda beklenmeyen davranışlara neden olabilir. Bu sorunları önlemek için, bir sayfa düzeni kullanımını zorleyin ve bir sayfa düzeni sürümü belirtin. Bunu yapmak, JavaScript'inizi temel aldığınız tüm içerik tanımlarının değişmez olmasını sağlar. JavaScript'i etkinleştirmek istemeseniz bile, sayfalarınızın sayfa düzeni sürümünü belirtmeniz gerekir.

Aşağıdaki örnek, sürümün `1.2.0` `selfasserted` **DataUri'sini** gösterir:

```xml
<ContentDefinition Id="api.localaccountpasswordreset">
<LoadUri>~/tenant/templates/AzureBlue/selfAsserted.cshtml</LoadUri>
<RecoveryUri>~/common/default_page_error.html</RecoveryUri>
<DataUri>urn:com:microsoft:aad:b2c:elements:contract:selfasserted:1.2.0</DataUri>
<Metadata>
    <Item Key="DisplayName">Local account change password page</Item>
</Metadata>
</ContentDefinition>
```

#### <a name="migrating-to-page-layout"></a>Sayfa düzenine geçiş

Değerin biçimi : `contract` _urn:com:microsoft:aad:b2c:elements:**sözleşme**:page-name:version_. Eski bir **DataUri** değeri kullanan özel ilkelerinizde bir sayfa düzeni belirtmek için, yeni biçime geçiş yapmak için aşağıdaki tabloyu kullanın.

| Eski DataUri değeri | Yeni DataUri değeri |
| ----------------- | ----------------- |
| `urn:com:microsoft:aad:b2c:elements:globalexception:1.0.0` | `urn:com:microsoft:aad:b2c:elements:contract:globalexception:1.2.0` |
| `urn:com:microsoft:aad:b2c:elements:globalexception:1.1.0` | `urn:com:microsoft:aad:b2c:elements:contract:globalexception:1.2.0` |
| `urn:com:microsoft:aad:b2c:elements:idpselection:1.0.0` | `urn:com:microsoft:aad:b2c:elements:contract:providerselection:1.2.0` |
| `urn:com:microsoft:aad:b2c:elements:multifactor:1.0.0` | `urn:com:microsoft:aad:b2c:elements:contract:multifactor:1.2.0` |
| `urn:com:microsoft:aad:b2c:elements:multifactor:1.1.0` | `urn:com:microsoft:aad:b2c:elements:contract:multifactor:1.2.0` |
| `urn:com:microsoft:aad:b2c:elements:selfasserted:1.0.0` | `urn:com:microsoft:aad:b2c:elements:contract:selfasserted:1.2.0` |
| `urn:com:microsoft:aad:b2c:elements:selfasserted:1.1.0` | `urn:com:microsoft:aad:b2c:elements:contract:selfasserted:1.2.0` |
| `urn:com:microsoft:aad:b2c:elements:unifiedssd:1.0.0` | `urn:com:microsoft:aad:b2c:elements:contract:unifiedssd:1.2.0` |
| `urn:com:microsoft:aad:b2c:elements:unifiedssp:1.0.0` | `urn:com:microsoft:aad:b2c:elements:contract:unifiedssp:1.2.0` |
| `urn:com:microsoft:aad:b2c:elements:unifiedssp:1.1.0` | `urn:com:microsoft:aad:b2c:elements:contract:unifiedssp:1.2.0` |


### <a name="metadata"></a>Meta Veriler

**Meta veri** öğesi aşağıdaki öğeleri içerir:

| Öğe | Oluşum | Açıklama |
| ------- | ----------- | ----------- |
| Öğe | 0:n | İçerik tanımıyla ilgili meta veriler. |

**Meta veri** öğesinin **Öğe** öğesi aşağıdaki öznitelikleri içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| Anahtar | Evet | Meta veri anahtarı.  |

#### <a name="metadata-keys"></a>Meta veri anahtarları

İçerik tanımı aşağıdaki meta veri öğelerini destekler:

| Anahtar | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| DisplayName | Hayır | İçerik tanımının adını içeren bir dize. |

### <a name="localizedresourcesreferences"></a>Yerelleştirilmiş KaynaklarReferanslar

**LocalizedResourcesReferences** öğesi aşağıdaki öğeleri içerir:

| Öğe | Oluşum | Açıklama |
| ------- | ----------- | ----------- |
| Yerelleştirilmiş KaynaklarReferans | 1:n | İçerik tanımı için yerelleştirilmiş kaynak başvurularının listesi. |

**LocalizedResourcesReference** öğesi aşağıdaki öznitelikleri içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| Dil | Evet | RFC 5646 başına ilke için desteklenen bir dil içeren bir dize - Dilleri Tanımlamak için etiketler. |
| YerelleştirilmişKaynaklarReferenceId | Evet | **Yerelleştirilmiş Kaynaklar** öğesinin tanımlayıcısı. |

Aşağıdaki örnekte, İngilizce, Fransızca ve İspanyolca için yerelleştirme ye atıfta bulunan bir kaydolma veya oturum açma içerik tanımı gösterilmektedir:

```XML
<ContentDefinition Id="api.signuporsignin">
  <LoadUri>~/tenant/default/unified.cshtml</LoadUri>
  <RecoveryUri>~/common/default_page_error.html</RecoveryUri>
  <DataUri>urn:com:microsoft:aad:b2c:elements:unifiedssp:1.0.0</DataUri>
  <Metadata>
    <Item Key="DisplayName">Signin and Signup</Item>
  </Metadata>
  <LocalizedResourcesReferences MergeBehavior="Prepend">
    <LocalizedResourcesReference Language="en" LocalizedResourcesReferenceId="api.signuporsignin.en" />
    <LocalizedResourcesReference Language="fr" LocalizedResourcesReferenceId="api.signuporsignin.rf" />
    <LocalizedResourcesReference Language="es" LocalizedResourcesReferenceId="api.signuporsignin.es" />
</LocalizedResourcesReferences>
</ContentDefinition>
```

İçerik tanımlarınıza yerelleştirme desteği eklemeyi öğrenmek için [bkz.](localization.md)

## <a name="content-definition-ids"></a>İçerik tanımı tünaları

**ContentDefinition** öğesinin kimlik özniteliği, içerik tanımıyla ilgili sayfa türünü belirtir. Öğe, özel bir HTML5/CSS şablonunun uygulayacağı bağlamı tanımlar. Aşağıdaki tabloda, Kimlik Deneyimi Çerçevesi tarafından tanınan içerik tanımı kimlikleri kümesi ve bunlarla ilgili sayfa türleri açıklanmaktadır. Rasgele bir kimlikle kendi içerik tanımlarınızı oluşturabilirsiniz.

| Kimlik | Varsayılan şablon | Açıklama |
| -- | ---------------- | ----------- |
| **api.error** | [özel durum.cshtml](https://login.microsoftonline.com/static/tenant/default/exception.cshtml) | **Hata sayfası** - Bir özel durum veya bir hatayla karşılaşıldığında bir hata sayfası görüntüler. |
| **api.idpselections** | [idpSelector.cshtml](https://login.microsoftonline.com/static/tenant/default/idpSelector.cshtml) | **Kimlik sağlayıcı seçim sayfası** - Oturum açma sırasında kullanıcıların aralarından seçim yapabilecekleri kimlik sağlayıcılarını listeler. Seçenekler genellikle kurumsal kimlik sağlayıcıları, Facebook ve Google+ gibi sosyal kimlik sağlayıcıları veya yerel hesaplardır. |
| **api.idpselections.signup** | [idpSelector.cshtml](https://login.microsoftonline.com/static/tenant/default/idpSelector.cshtml) | **Kaydolmak için kimlik sağlayıcı seçimi** - Kullanıcıların kayıt sırasında aralarından seçim yapabilecekleri kimlik sağlayıcılarını listeler. Seçenekler genellikle kurumsal kimlik sağlayıcıları, Facebook ve Google+ gibi sosyal kimlik sağlayıcıları veya yerel hesaplardır. |
| **api.localaccountpasswordreset** | [selfasserted.cshtml](https://login.microsoftonline.com/static/tenant/default/selfAsserted.cshtml) | **Parola sayfasını unuttum** - Parola sıfırlama işlemini başlatmak için kullanıcıların tamamlaması gereken bir form görüntüler. |
| **api.localaccountsignin** | [selfasserted.cshtml](https://login.microsoftonline.com/static/tenant/default/selfAsserted.cshtml) | **Yerel hesap oturum açma sayfası** - E-posta adresini veya kullanıcı adını temel alan yerel bir hesapla oturum açma formu görüntüler. Form, metin giriş kutusu ve parola giriş kutusu içerebilir. |
| **api.localaccountsignup** | [selfasserted.cshtml](https://login.microsoftonline.com/static/tenant/default/selfAsserted.cshtml) | **Yerel hesap kayıt sayfası** - E-posta adresini veya kullanıcı adını temel alan yerel bir hesaba kaydolmak için bir form görüntüler. Form, metin giriş kutusu, parola giriş kutusu, radyo düğmesi, tek seçimli açılır bırakma kutuları ve çok seçimli onay kutuları gibi çeşitli giriş denetimleri içerebilir. |
| **api.phonefactor** | [çok faktörlü-1.0.0.cshtml](https://login.microsoftonline.com/static/tenant/default/multifactor-1.0.0.cshtml) | **Çok faktörlü kimlik doğrulama sayfası** - Kaydolma veya kaydolma sırasında metin veya ses kullanarak telefon numaralarını doğrular. |
| **api.selfasserted** | [selfasserted.cshtml](https://login.microsoftonline.com/static/tenant/default/selfAsserted.cshtml) | **Sosyal hesap kayıt sayfası** - Sosyal kimlik sağlayıcısından gelen mevcut bir hesabı kullanarak kullanıcıların kaydolurken tamamlamaları gereken bir form görüntüler. Bu sayfa, parola giriş alanları dışında, önceki sosyal hesap kayıt sayfasına benzer. |
| **api.selfasserted.profileupdate** | [updateprofile.cshtml](https://login.microsoftonline.com/static/tenant/default/updateProfile.cshtml) | **Profil güncelleme sayfası** - Kullanıcıların profillerini güncellemek için erişebileceği bir form görüntüler. Bu sayfa, parola giriş alanları dışında sosyal hesap kayıt sayfasına benzer. |
| **api.signuporsignin** | [unified.cshtml](https://login.microsoftonline.com/static/tenant/default/unified.cshtml) | **Birleşik kayıt veya oturum açma sayfası** - Kullanıcı kayıt ve oturum açma işlemini işler. Kullanıcılar kurumsal kimlik sağlayıcılarını, Facebook veya Google+ gibi sosyal kimlik sağlayıcılarını veya yerel hesapları kullanabilir. |

## <a name="next-steps"></a>Sonraki adımlar

İçerik tanımlarını kullanarak kullanıcı arabirimini özelleştirme örneği için bkz:

[Özel bir ilke kullanarak uygulamanızın kullanıcı arabirimini özelleştirin](custom-policy-ui-customization.md)
