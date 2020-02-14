---
title: Özel ilkede kendi kendine onaylanan teknik profil tanımlama
titleSuffix: Azure AD B2C
description: Azure Active Directory B2C bir özel ilkede kendi kendine onaylanan bir teknik profil tanımlayın.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 02/13/2020
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: a16fb1184de5b545b3ef527b1a66ffb7b68d1ef4
ms.sourcegitcommit: 333af18fa9e4c2b376fa9aeb8f7941f1b331c11d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2020
ms.locfileid: "77197927"
---
# <a name="define-a-self-asserted-technical-profile-in-an-azure-active-directory-b2c-custom-policy"></a>Azure Active Directory B2C özel ilkesinde kendi kendine onaylanan teknik profil tanımlama

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Kullanıcının giriş sağlaması beklenen Azure Active Directory B2C (Azure AD B2C) içindeki tüm etkileşimler kendi kendine onaylanan teknik profillerdir. Örneğin, bir kaydolma sayfası, oturum açma sayfası veya parola sıfırlama sayfası.

## <a name="protocol"></a>Protokol

**Protokol** öğesinin **Name** özniteliğinin `Proprietary`olarak ayarlanması gerekir. **Handler** özniteliği, kendi kendine onaylanan Azure AD B2C tarafından kullanılan protokol işleyici derlemesinin tam adını içermelidir: `Web.TPEngine.Providers.SelfAssertedAttributeProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null`

Aşağıdaki örnek, e-posta kaydı için kendi kendine onaylanan bir teknik profili göstermektedir:

```XML
<TechnicalProfile Id="LocalAccountSignUpWithLogonEmail">
  <DisplayName>Email signup</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.SelfAssertedAttributeProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
```

## <a name="input-claims"></a>Giriş talepleri

Kendi kendine onaylanan bir teknik profilde, otomatik olarak onaylanan sayfada görünen taleplerin değerini önceden doldurmak için **inputclaim** ve **ınputclaimstransformations** öğelerini kullanabilirsiniz (talepleri görüntüle). Örneğin, profil düzenleme ilkesinde Kullanıcı yolculuğu ilk olarak Azure AD B2C dizin hizmetinden kullanıcı profilini okur, ardından kendi kendini onaylanan teknik profil, giriş taleplerini Kullanıcı profilinde depolanan kullanıcı verileriyle ayarlar. Bu talepler Kullanıcı profilinden toplanır ve sonra var olan verileri düzenleyebilen kullanıcıya sunulur.

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

## <a name="display-claims"></a>Talepleri görüntüle

Görüntüleme talepleri özelliği şu anda **önizlemededir**.

**DisplayClaim** öğesi, kullanıcıdan veri toplamaya yönelik ekranda sunulacak talepler listesini içerir. Çıkış taleplerinin değerlerini önceden doldurmak için, daha önce açıklanan giriş taleplerini kullanın. Öğesi de varsayılan bir değer içerebilir.

**DisplayClaim** 'teki taleplerin sırası, Azure AD B2C ekranda talepleri işleyen sırayı belirtir. Kullanıcıyı belirli bir talep için bir değer sağlamaya zorlamak için, **DisplayClaim** öğesinin **gerekli** özniteliğini `true`olarak ayarlayın.

**DisplayClaim** koleksiyonundaki **ClaimType** öğesi, **userınputtype** öğesini Azure AD B2C tarafından desteklenen herhangi bir kullanıcı giriş türüne ayarlamış olmalıdır. Örneğin, `TextBox` veya `DropdownSingleSelect`.

### <a name="add-a-reference-to-a-displaycontrol"></a>DisplayControl öğesine başvuru ekleme

Görüntüleme talepleri koleksiyonunda, oluşturduğunuz bir [DisplayControl](display-controls.md) öğesine başvuru ekleyebilirsiniz. Görüntüleme denetimi, özel işlevleri olan ve Azure AD B2C arka uç hizmetiyle etkileşime sahip bir kullanıcı arabirimi öğesidir. Kullanıcının arka uçta doğrulama teknik profilini çağıran sayfada eylemler gerçekleştirmesine olanak sağlar. Örneğin, bir e-posta adresi, telefon numarası veya müşteri bağlılık programı numarası doğrulanıyor.

Aşağıdaki örnek `TechnicalProfile` ekran denetimleriyle görüntüleme taleplerinin kullanımını gösterir.

* İlk görüntüleme talebi, e-posta adresini toplayıp doğrulayan `emailVerificationControl` görüntü denetimine bir başvuru yapar.
* Beşinci görüntüleme talebi, bir telefon numarasını toplayan ve doğrulayan `phoneVerificationControl` görüntü denetimine bir başvuru yapar.
* Diğer görüntüleme talepleri, kullanıcıdan toplanacak ClaimTypes türleridir.

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

Belirtildiği gibi, bir görüntü denetimi başvurusu olan bir görüntüleme talebi kendi doğrulamasını çalıştırabilir, örneğin e-posta adresini doğrulayan. Ayrıca, otomatik olarak onaylanan sayfa, bir sonraki düzenleme adımına geçmeden önce herhangi bir kullanıcı girişi (talep türleri veya görüntüleme denetimleri) dahil olmak üzere tüm sayfayı doğrulamak için bir doğrulama teknik profili kullanmayı destekler.

### <a name="combine-usage-of-display-claims-and-output-claims-carefully"></a>Görüntüleme taleplerinin ve çıkış taleplerinin kullanımını dikkatle birleştirin

Kendi kendine onaylanan bir teknik profilde bir veya daha fazla **DisplayClaim** öğesi belirtirseniz, ekranda görüntülemek ve kullanıcıdan toplamak istediğiniz *her* talep için bir DisplayClaim kullanmanız gerekir. En az bir görüntüleme talebi içeren, kendi kendine onaylanan bir teknik profille hiçbir çıkış talebi gösterilmiyor.

Bir `age` talebinin bir temel ilkede **Çıkış** talebi olarak tanımlandığı aşağıdaki örneği göz önünde bulundurun. Kendi kendini onaylanan teknik profile herhangi bir görüntüleme talebi eklemeden önce, `age` talep kullanıcıdan veri toplamaya ilişkin ekranda görüntülenir:

```XML
<TechnicalProfile Id="id">
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="age" />
  </OutputClaims>
</TechnicalProfile>
```

O temeli devralan bir yaprak ilke daha sonra bir **görüntüleme** talebi olarak `officeNumber` belirtiyorsa:

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

Temel ilkedeki `age` talebi artık ekranda kullanıcıya sunulmadı; bu, etkin bir "gizli". `age` talebi görüntülemek ve kullanıcıdan yaş değerini toplamak için, bir `age` **DisplayClaim**eklemeniz gerekir.

## <a name="output-claims"></a>Çıkış talepleri

**Outputclaim** öğesi, sonraki düzenleme adımına döndürülecek talepler listesini içerir. **DefaultValue** özniteliği yalnızca talep ayarlanmamışsa devreye girer. Önceki bir düzenleme adımında ayarlandıysa, Kullanıcı değeri boş bırakırsa bile varsayılan değer geçerli olmaz. Varsayılan bir değerin kullanımını zorlamak için, **Alwaysusedefaultvalue** özniteliğini `true`olarak ayarlayın.

> [!NOTE]
> Kimlik deneyimi çerçevesinin (ıEF) önceki sürümlerinde, çıkış talepleri kullanıcıdan veri toplamak için kullanıldı. Kullanıcıdan veri toplamak için bunun yerine bir **DisplayClaim** koleksiyonu kullanın.

**Outputclaimstransformations** öğesi, çıkış taleplerini değiştirmek veya yenilerini oluşturmak için kullanılan bir **outputclaimstransreference** öğeleri koleksiyonu içerebilir.

### <a name="when-you-should-use-output-claims"></a>Çıkış taleplerini kullanmanız gerektiğinde

Kendi kendine onaylanan bir teknik profilde, çıkış talep koleksiyonu, talepleri sonraki düzenleme adımına döndürür.

Şu durumlarda çıkış taleplerini kullanmanız gerekir:

- **Talepler çıkış talebi dönüşümüne göre çıktılardır**.
- Kullanıcıdan veri toplanmadan veya doğrulama teknik profilindeki verileri döndürmeden **bir çıkış talebinde varsayılan değer ayarlama** . `LocalAccountSignUpWithLogonEmail` kendi kendini onaylanan teknik profili, **yürütülmüş-SelfAsserted-Input** talebini `true`olarak ayarlar.
- **Bir doğrulama teknik profili, çıkış taleplerini döndürür** . Teknik profiliniz, bazı talepler döndüren bir doğrulama teknik profili çağırabilir. Talepleri kabarcığa eklemek ve bunları Kullanıcı yolculuğunda sonraki düzenleme adımlarına döndürmek isteyebilirsiniz. Örneğin, yerel bir hesapla oturum açarken, `SelfAsserted-LocalAccountSignin-Email` adlı kendi kendini onaylanan teknik profil, `login-NonInteractive`adlı doğrulama teknik profilini çağırır. Bu teknik profil, Kullanıcı kimlik bilgilerini doğrular ve kullanıcı profilini de döndürür. ' UserPrincipalName ', ' displayName ', ', ' ve ' soyadı ' gibi.
- **Bir görüntü denetimi çıkış taleplerini döndürür** . Teknik profiliniz bir [görüntü denetimine](display-controls.md)yönelik bir başvuruya sahip olabilir. Görünen denetim, doğrulanmış e-posta adresi gibi bazı talepler döndürür. Talepleri kabarcığa eklemek ve bunları Kullanıcı yolculuğunda sonraki düzenleme adımlarına döndürmek isteyebilirsiniz. Görüntüleme denetimi özelliği şu anda **önizlemededir**.

Aşağıdaki örnek, hem görüntüleme taleplerini hem de çıkış taleplerini kullanan kendi kendine onaylanan bir teknik profilin kullanımını gösterir.

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

## <a name="persist-claims"></a>Kalıcı talepler

**PersistedClaims** öğesi yoksa, kendi kendini onaylanan teknik profil Azure AD B2C verileri kalıcı hale etmez. Bunun yerine, verileri kalıcı hale getirmekten sorumlu bir doğrulama teknik profiline çağrı yapılır. Örneğin kaydolma ilkesi, yeni kullanıcı profilini toplamak için `LocalAccountSignUpWithLogonEmail` kendi kendine onaylanan teknik profilini kullanır. `LocalAccountSignUpWithLogonEmail` teknik profili, hesabı Azure AD B2C oluşturmak için doğrulama teknik profilini çağırır.

## <a name="validation-technical-profiles"></a>Doğrulama teknik profilleri

Bir doğrulama teknik profili, başvuru yapan teknik profilin bazı veya tüm çıkış taleplerini doğrulamak için kullanılır. Doğrulama teknik profilinin giriş talepleri, kendi kendini onaylanan teknik profilin çıkış taleplerinde görünmelidir. Doğrulama teknik profili, Kullanıcı girişini doğrular ve kullanıcıya bir hata döndürebilir.

Doğrulama teknik profili, ilkede [Azure Active Directory](active-directory-technical-profile.md) veya [REST API](restful-technical-profile.md) teknik profilleri gibi herhangi bir teknik profil olabilir. Önceki örnekte `LocalAccountSignUpWithLogonEmail` teknik profili, Signınname 'in dizinde mevcut olmadığını doğrular. Aksi takdirde, doğrulama teknik profili yerel bir hesap oluşturur ve objectID, authenticationSource, newUser öğesini döndürür. `SelfAsserted-LocalAccountSignin-Email` teknik profili, Kullanıcı kimlik bilgilerini doğrulamak için `login-NonInteractive` doğrulama teknik profilini çağırır.

Ayrıca kurumsal iş kolu uygulamasıyla daha fazla tümleştirme yaparak iş mantığınızla birlikte REST API teknik bir profil çağırabilir, giriş taleplerinin üzerine yazabilir veya zenginleştirme Kullanıcı verileri sağlayabilirsiniz. Daha fazla bilgi için bkz. [doğrulama teknik profili](validation-technical-profile.md)

## <a name="metadata"></a>Meta Veriler

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| Setting. operatingMode | Hayır | Oturum açma sayfası için bu özellik, giriş doğrulaması ve hata iletileri gibi Kullanıcı adı alanının davranışını denetler. Beklenen değerler: `Username` veya `Email`. |
| AllowGenerationOfClaimsWithNullValues| Hayır| Null değerli bir talep oluşturmaya izin verin. Örneğin, bir durumda Kullanıcı bir onay kutusu seçmez.|
| Contentdefinitionreferenceıd | Evet | Bu teknik profille ilişkili [içerik tanımının](contentdefinitions.md) tanımlayıcısı. |
| Enforceemaildoğrulaması | Hayır | Kaydolma veya profil düzenleme için, e-posta doğrulamasını uygular. Olası değerler: `true` (varsayılan) veya `false`. |
| . retryLimit ayarlanıyor | Hayır | Bir kullanıcının bir doğrulama teknik profiline karşı denetlenen verileri sağlamaya kaç kez denendiğini denetler. Örneğin, bir kullanıcı zaten var olan bir hesapla kaydolmaya çalışır ve sınıra ulaşılana kadar denemeye devam eder.
| SignUpTarget | Hayır | Kaydolma hedefi değişim tanımlayıcısı. Kullanıcı kaydolma düğmesine tıkladığında, Azure AD B2C belirtilen Exchange tanımlayıcısını yürütür. |
| Setting. showCancelButton | Hayır | İptal düğmesini görüntüler. Olası değerler: `true` (varsayılan) veya `false` |
| Setting. Showdevam düğmesi | Hayır | Devam düğmesini görüntüler. Olası değerler: `true` (varsayılan) veya `false` |
| . Showsignyukarı ayarlanıyor | Hayır | Kaydolma düğmesini görüntüler. Olası değerler: `true` (varsayılan) veya `false` |
| Setting. forgotPasswordLinkLocation| Hayır| Parolayı unuttum bağlantısını görüntüler. Olası değerler: `AfterInput` (varsayılan) bağlantı sayfanın alt kısmında görüntülenir `None` veya parola unutulan bağlantıyı kaldırır.| 
| IncludeClaimResolvingInClaimsHandling  | Hayır | Giriş ve çıkış talepleri için, [talep çözümlemenin](claim-resolver-overview.md) teknik profile dahil edilip edilmeyeceğini belirtir. Olası değerler: `true`veya `false` (varsayılan). Teknik profilde bir talep çözümleyici kullanmak istiyorsanız bunu `true`olarak ayarlayın. |

## <a name="cryptographic-keys"></a>Şifreleme anahtarları

**Cryptographickeys** öğesi kullanılmıyor.
