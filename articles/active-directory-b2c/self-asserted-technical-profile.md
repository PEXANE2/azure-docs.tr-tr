---
title: Özel bir ilkede kendi kendini öne süren bir teknik profil tanımlama
titleSuffix: Azure AD B2C
description: Azure Active Directory B2C'de özel bir ilkede kendi kendini öne süren bir teknik profil tanımlayın.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 03/26/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 2b29b8b0975639e5c5315a55e1382794d7662665
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80332500"
---
# <a name="define-a-self-asserted-technical-profile-in-an-azure-active-directory-b2c-custom-policy"></a>Azure Active Directory B2C özel ilkesinde kendi kendini öne süren bir teknik profil tanımlama

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Kullanıcının girdi sağlamasıbeklenen Azure Active Directory B2C'deki (Azure AD B2C) tüm etkileşimler, kendi kendine öne sürülen teknik profillerdir. Örneğin, bir kayıt sayfası, oturum açma sayfası veya parola sıfırlama sayfası.

## <a name="protocol"></a>Protokol

**Protokol** öğesinin `Proprietary` **Ad** özniteliğinin . **Işleyici** özniteliği, azure AD B2C tarafından kullanılan protokol işleyicisi derlemesinin tam nitelikli adını içermelidir:`Web.TPEngine.Providers.SelfAssertedAttributeProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null`

Aşağıdaki örnek, e-posta kaydolmak için kendi kendini öne süren bir teknik profili gösterir:

```XML
<TechnicalProfile Id="LocalAccountSignUpWithLogonEmail">
  <DisplayName>Email signup</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.SelfAssertedAttributeProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
```

## <a name="input-claims"></a>Giriş talepleri

Kendi kendini öne süren bir teknik profilde, kendi kendini öne süren sayfada görünen iddiaların değerini önceden doldurmak için Giriş Talepleri ve **InputClaimsTransformations** öğelerini kullanabilirsiniz (talepleri görüntüler). **InputClaims** Örneğin, profil oluşturma ilkesinde, kullanıcı yolculuğu önce Azure AD B2C dizin hizmetinden kullanıcı profilini okur, ardından kendi kendini öne süren teknik profil, kullanıcı profilinde depolanan kullanıcı verileriyle giriş taleplerini ayarlar. Bu talepler kullanıcı profilinden toplanır ve daha sonra varolan verileri yeniden atabilecek kullanıcıya sunulur.

```XML
<TechnicalProfile Id="SelfAsserted-ProfileUpdate">
...
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="alternativeSecurityId" />
    <InputClaim ClaimTypeReferenceId="userPrincipalName" />
    <InputClaim ClaimTypeReferenceId="givenName" />
    <InputClaim ClaimTypeReferenceId="surname" />
  </InputClaims>
```

## <a name="display-claims"></a>Talepleri görüntüleme

Ekran talepleri özelliği şu anda **önizlemede.**

**DisplayClaims** öğesi, kullanıcıdan veri toplamak için ekranda sunulacak taleplerin bir listesini içerir. Görüntü taleplerinin değerlerini önceden doldurmak için, daha önce açıklanan giriş taleplerini kullanın. Öğe varsayılan bir değer de içerebilir.

**DisplayClaims'teki** taleplerin sırası, Azure AD B2C'nin talepleri ekranda işleme sırasını belirtir. Kullanıcıyı belirli bir talep için bir değer sağlamaya zorlamak için **DisplayClaim** `true`öğesinin **gerekli** özniteliğini .

**DisplayClaims** koleksiyonundaki **ClaimType** öğesinin **UserInputType** öğesini Azure AD B2C tarafından desteklenen herhangi bir kullanıcı giriş türüne ayarlaması gerekir. Örneğin `TextBox` veya `DropdownSingleSelect` olabilir.

### <a name="add-a-reference-to-a-displaycontrol"></a>DisplayControl'e başvuru ekleme

Görüntü talepleri koleksiyonuna, oluşturduğunuz [DisplayControl'e](display-controls.md) bir başvuru ekleyebilirsiniz. Ekran denetimi, özel işlevsellik olan ve Azure AD B2C arka uç hizmetiyle etkileşim edebilen bir kullanıcı arabirimi öğesidir. Kullanıcının sayfada arka uçta doğrulama teknik profili çağıran eylemleri gerçekleştirmesine olanak tanır. Örneğin, bir e-posta adresini, telefon numarasını veya müşteri sadakat numarasını doğrulama.

Aşağıdaki örnekte, `TechnicalProfile` görüntü denetimleri ile görüntü taleplerinin kullanımı gösteriş verilmiştir.

* İlk görüntüleme talebi, e-posta adresini toplayan ve doğrulayan `emailVerificationControl` ekran denetimine başvuruda bulunarak, bu denetimi sağlar.
* Beşinci ekran talebi, bir `phoneVerificationControl` telefon numarasını toplayan ve doğrulayan ekran denetimine başvuruda bulunarak, bu denetime başvuru yapar.
* Diğer görüntü talepleri, kullanıcıdan toplanacak ClaimTypes'dır.

```XML
<TechnicalProfile Id="Id">
  <DisplayClaims>
    <DisplayClaim DisplayControlReferenceId="emailVerificationControl" />
    <DisplayClaim ClaimTypeReferenceId="displayName" Required="true" />
    <DisplayClaim ClaimTypeReferenceId="givenName" Required="true" />
    <DisplayClaim ClaimTypeReferenceId="surName" Required="true" />
    <DisplayClaim DisplayControlReferenceId="phoneVerificationControl" />
    <DisplayClaim ClaimTypeReferenceId="newPassword" Required="true" />
    <DisplayClaim ClaimTypeReferenceId="reenterPassword" Required="true" />
  </DisplayClaims>
</TechnicalProfile>
```

Belirtildiği gibi, bir görüntü denetimine başvuru içeren bir görüntüleme talebi, örneğin e-posta adresini doğrulamak için kendi doğrulamasını çalıştırabilir. Buna ek olarak, kendi kendini öne süren sayfa, bir sonraki düzenleme adımına geçmeden önce kullanıcı girişi (talep türleri veya görüntü denetimleri) dahil olmak üzere tüm sayfayı doğrulamak için bir doğrulama teknik profili kullanılmasını destekler.

### <a name="combine-usage-of-display-claims-and-output-claims-carefully"></a>Ekran taleplerinin ve çıktı taleplerinin kullanımını dikkatle birleştirin

Kendi kendine öne çıkan bir teknik profilde bir veya daha fazla **DisplayClaim** öğesi belirtirseniz, ekranda görüntülemek ve kullanıcıdan toplamak istediğiniz *her* talep için bir DisplayClaim kullanmanız gerekir. Hiçbir çıktı talebi, en az bir görüntü talebi içeren kendi kendini öne süren teknik bir profil tarafından görüntülenmez.

Bir `age` talebin temel ilkedeki **çıktı** talebi olarak tanımlandığı aşağıdaki örneği göz önünde bulundurun. Herhangi bir görüntü iddiasını kendi kendine öne süren `age` teknik profile eklemeden önce, talep kullanıcıdan veri toplama için ekranda görüntülenir:

```XML
<TechnicalProfile Id="id">
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="age" />
  </OutputClaims>
</TechnicalProfile>
```

Bu temeli devralan bir yaprak ilkesi `officeNumber` daha sonra **bir görüntü** talebi olarak belirtirse:

```XML
<TechnicalProfile Id="id">
  <DisplayClaims>
    <DisplayClaim ClaimTypeReferenceId="officeNumber" />
  </DisplayClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="officeNumber" />
  </OutputClaims>
</TechnicalProfile>
```

Temel `age` ilkedeki talep artık ekranda kullanıcıya sunulmadı - etkili bir şekilde "gizlidir." `age` Talebi görüntülemek ve kullanıcıdan yaş değerini toplamak için `age` bir **DisplayClaim**eklemeniz gerekir.

## <a name="output-claims"></a>Çıktı talepleri

**OutputClaims** öğesi, bir sonraki düzenleme adımına döndürülecek taleplerin listesini içerir. **Varsayılan Değer** özniteliği yalnızca talep hiç ayarlanmadıysa etkinolur. Önceki bir düzenleme adımında ayarlanmışsa, kullanıcı değeri boş bıraksa bile varsayılan değer etkili olmaz. Varsayılan değerin kullanımını zorlamak `true` **için, AlwaysUseDefaultValue** özniteliğini .

Güvenlik nedenleriyle, parola talep`UserInputType` değeri `Password`(ayarlanmış) yalnızca kendi kendini öne süren teknik profilin doğrulama teknik profillerinde kullanılabilir. Sonraki düzenleme adımlarında parola iddiasını kullanamazsınız. 

> [!NOTE]
> Kimlik Deneyimi Çerçevesi'nin (IEF) önceki sürümlerinde, çıktı talepleri kullanıcıdan veri toplamak için kullanılmıştır. Kullanıcıdan veri toplamak için **displayclaims** koleksiyonunu kullanın.

**OutputClaimsTransformations** öğesi, çıktı taleplerini değiştirmek veya yenilerini oluşturmak için kullanılan **OutputClaimsTransformation** öğelerikoleksiyonunu içerebilir.

### <a name="when-you-should-use-output-claims"></a>Çıktı taleplerini kullanmanız gerektiğinde

Kendi kendine ileri süren teknik profilde, çıktı talepleri toplama talepleri bir sonraki düzenleme adımına döndürür.

Çıktı taleplerini şu anda kullanın:

- **Talepler çıktı talepleri dönüşümü ile çıktıdır.**
- Kullanıcıdan veri toplamadan veya doğrulama teknik profilinden verileri döndürmeden **çıktı talebinde varsayılan değer ayarlama.** Kendi `LocalAccountSignUpWithLogonEmail` kendini ileri süren teknik **profil, yürütülen-SelfAsserted-Giriş** iddiasını `true`ayarlar.
- **Doğrulama teknik profili çıktı taleplerini döndürür** - Teknik profiliniz bazı talepleri döndüren bir doğrulama teknik profili çağırabilir. İddiaları kabarcıklamak ve kullanıcı yolculuğunda bir sonraki düzenleme adımlarına döndürmek isteyebilirsiniz. Örneğin, yerel bir hesapla oturum açken, kendi kendini `SelfAsserted-LocalAccountSignin-Email` öne süren teknik `login-NonInteractive`profil adlı doğrulama teknik profilini çağırır. Bu teknik profil kullanıcı kimlik bilgilerini doğrular ve kullanıcı profilini de döndürür. 'userPrincipalName', 'displayName', 'givenName' ve 'surname' gibi.
- **Görüntü denetimi çıktı taleplerini döndürür** - Teknik profilinizin [ekran denetimine](display-controls.md)bir başvurusu olabilir. Görüntü denetimi, doğrulanmış e-posta adresi gibi bazı talepleri döndürür. İddiaları kabarcıklamak ve kullanıcı yolculuğunda bir sonraki düzenleme adımlarına döndürmek isteyebilirsiniz. Ekran denetimi özelliği şu anda **önizlemede.**

Aşağıdaki örnek, hem görüntü talepleri hem de çıktı talepleri kullanan kendini ileri süren bir teknik profilin kullanımını göstermektedir.

```XML
<TechnicalProfile Id="LocalAccountSignUpWithLogonEmail">
  <DisplayName>Email signup</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.SelfAssertedAttributeProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  <Metadata>
    <Item Key="IpAddressClaimReferenceId">IpAddress</Item>
    <Item Key="ContentDefinitionReferenceId">api.localaccountsignup</Item>
    <Item Key="language.button_continue">Create</Item>
  </Metadata>
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="email" />
  </InputClaims>
  <DisplayClaims>
    <DisplayClaim DisplayControlReferenceId="emailVerificationControl" />
    <DisplayClaim DisplayControlReferenceId="SecondaryEmailVerificationControl" />
    <DisplayClaim ClaimTypeReferenceId="displayName" Required="true" />
    <DisplayClaim ClaimTypeReferenceId="givenName" Required="true" />
    <DisplayClaim ClaimTypeReferenceId="surName" Required="true" />
    <DisplayClaim ClaimTypeReferenceId="newPassword" Required="true" />
    <DisplayClaim ClaimTypeReferenceId="reenterPassword" Required="true" />
  </DisplayClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="email" Required="true" />
    <OutputClaim ClaimTypeReferenceId="objectId" />
    <OutputClaim ClaimTypeReferenceId="executed-SelfAsserted-Input" DefaultValue="true" />
    <OutputClaim ClaimTypeReferenceId="authenticationSource" />
    <OutputClaim ClaimTypeReferenceId="newUser" />
  </OutputClaims>
  <ValidationTechnicalProfiles>
    <ValidationTechnicalProfile ReferenceId="AAD-UserWriteUsingLogonEmail" />
  </ValidationTechnicalProfiles>
  <UseTechnicalProfileForSessionManagement ReferenceId="SM-AAD" />
</TechnicalProfile>
```

## <a name="persist-claims"></a>Devam talepleri

PersistedClaims öğesi kullanılmaz. Kendi kendini öne süren teknik profil, verileri Azure AD B2C'ye kadar devam etmez. Bunun yerine, verileri kalıcı olarak sağlamakla sorumlu bir doğrulama teknik profiline arama yapılır. Örneğin, kaydolma ilkesi, yeni `LocalAccountSignUpWithLogonEmail` kullanıcı profilini toplamak için kendi kendini öne süren teknik profili kullanır. Teknik `LocalAccountSignUpWithLogonEmail` profil, hesabı Azure AD B2C'de oluşturmak için doğrulama teknik profilini çağırır.

## <a name="validation-technical-profiles"></a>Doğrulama teknik profilleri

Doğrulama teknik profili, başvuru teknik profilinin çıktı taleplerinin bir kısmını veya tamamını doğrulamak için kullanılır. Doğrulama teknik profilinin giriş talepleri, kendi kendine ileri sürülen teknik profilin çıktı taleplerinde görünmelidir. Doğrulama teknik profili kullanıcı girişini doğrular ve kullanıcıya bir hata döndürebilir.

Doğrulama teknik profili, ilkedeki [Azure Etkin Dizini](active-directory-technical-profile.md) veya REST [API](restful-technical-profile.md) teknik profilleri gibi herhangi bir teknik profil olabilir. Önceki örnekte, `LocalAccountSignUpWithLogonEmail` teknik profil signinName dizinde yok olduğunu doğrular. Değilse, doğrulama teknik profili yerel bir hesap oluşturur ve objectId, authenticationSource, newUser döndürür. Teknik `SelfAsserted-LocalAccountSignin-Email` profil, `login-NonInteractive` kullanıcı kimlik bilgilerini doğrulamak için doğrulama teknik profilini çağırır.

Ayrıca, iş mantığınızla birlikte REST API teknik profilini arayabilir, giriş taleplerinin üzerine yazabilir veya kurumsal iş yeri uygulamasıyla daha fazla entegre ederek kullanıcı verilerini zenginleştirebilirsiniz. Daha fazla bilgi için Doğrulama [teknik profiline](validation-technical-profile.md) bakın

## <a name="metadata"></a>Meta Veriler

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| setting.operatingMod <sup>1</sup>| Hayır | Oturum açma sayfası için bu özellik, giriş doğrulama ve hata iletileri gibi kullanıcı adı alanının davranışını denetler. Beklenen `Username` değerler: `Email`veya .  |
| İzin GenerationofclaimsWithNullvalues| Hayır| Null değeri olan bir talep oluşturmamaya izin verin. Örneğin, bir durumda kullanıcı bir onay kutusu seçmez.|
| İçerikDefinitionReferenceId | Evet | Bu teknik profille ilişkili [içerik tanımının](contentdefinitions.md) tanımlayıcısı. |
| E-posta Doğrulamayı Uygulayın | Hayır | Kaydolma veya profil düzeltme için e-posta doğrulamasını zorlar. Olası değerler: `true` (varsayılan) veya `false`. |
| setting.retryLimit | Hayır | Bir kullanıcının doğrulama teknik profiliyle karşısına çıkan verileri sağlamayı kaç kez deneyebileceğini denetler. Örneğin, bir kullanıcı zaten var olan bir hesapla kaydolmaya çalışır ve sınırulaşınana kadar denemeye devam eder.
| Kayıt Hedef <sup>1</sup>| Hayır | Kayıt hedef değişim tanımlayıcısı. Kullanıcı kaydol düğmesini tıklattığında, Azure AD B2C belirtilen değişim tanımlayıcısını çalıştırır. |
| setting.showCancelButton | Hayır | İptal düğmesini görüntüler. Olası değerler: `true` (varsayılan) veya`false` |
| setting.showContinueButton | Hayır | Continue düğmesini görüntüler. Olası değerler: `true` (varsayılan) veya`false` |
| setting.showSignupLink <sup>2</sup>| Hayır | Kaydol düğmesini görüntüler. Olası değerler: `true` (varsayılan) veya`false` |
| ayarı.forgotPasswordLinkLocation <sup>2</sup>| Hayır| Unutulan parola bağlantısını görüntüler. Olası değerler: `AfterInput` (varsayılan) bağlantı sayfanın alt kısmında `None` görüntülenir veya unutulan parola bağlantısını kaldırır.|
| ayarı.enableRememberMe <sup>2</sup>| Hayır| Beni [tut imzalı](custom-policy-keep-me-signed-in.md) onay kutusunu görüntüler. Olası değerler: `true` `false` , veya (varsayılan). |
| IncludeClaimResolvingInClaimsHandling  | Hayır | Giriş ve çıktı talepleri [için, talep çözümlübir](claim-resolver-overview.md) çözümünü teknik profilde bulunup bulunmayacağını belirtir. Olası değerler: `true` `false`  , veya (varsayılan). Teknik profilde bir talep çözümleyicisi kullanmak istiyorsanız, bunu ' ya `true`göre ayarlayın. |

Notlar:
1. İçerik tanımı [dataUri](contentdefinitions.md#datauri) türü `unifiedssp`için `unifiedssd`kullanılabilir , veya .
1. İçerik tanımı [dataUri](contentdefinitions.md#datauri) türü `unifiedssp`için `unifiedssd`kullanılabilir , veya . [Sayfa düzeni sürüm](page-layout.md) 1.1.0 ve üzeri.

## <a name="cryptographic-keys"></a>Şifreleme tuşları

**CryptographicKeys** öğesi kullanılmaz.
