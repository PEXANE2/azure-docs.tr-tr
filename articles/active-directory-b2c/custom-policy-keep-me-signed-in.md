---
title: Oturumumu Açık tut Azure Active Directory B2C
description: Azure Active Directory B2C 'da Oturumumu Açık tut (KMSI) ayarlamayı öğrenin.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 03/26/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 041fb8d881307b52fb170a11618f930debc522a4
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80803169"
---
# <a name="enable-keep-me-signed-in-kmsi-in-azure-active-directory-b2c"></a>Azure Active Directory B2C 'da Oturumumu Açık tut (KMSI) özelliğini etkinleştir

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Web kullanıcılarınızın kullanıcıları ve Azure Active Directory B2C (Azure AD B2C) dizininizde yerel hesaplara sahip yerel uygulamalarınız için Oturumumu Açık tut (KMSı) işlevini etkinleştirebilirsiniz. Bu özellik, Kullanıcı adını ve parolasını yeniden girmesi istenmeden, uygulamanıza geri dönen kullanıcılara erişim izni verir. Kullanıcı oturumu kapattığında bu erişim iptal edilir.

Kullanıcılar bu seçeneği genel bilgisayarlarda etkinleştirmemelidir.

![Oturumumu Açık tut onay kutusunun gösterildiği örnek kaydolma, oturum açma sayfası](./media/custom-policy-keep-me-signed-in/kmsi.PNG)

## <a name="prerequisites"></a>Ön koşullar

- Yerel hesap oturum açmaya izin verecek şekilde yapılandırılmış bir Azure AD B2C kiracısı. KMSı, dış kimlik sağlayıcısı hesaplarında desteklenmez.
- [Özel ilkelerle çalışmaya başlama](custom-policy-get-started.md)bölümündeki adımları uygulayın.

## <a name="configure-the-page-identifier"></a>Sayfa tanımlayıcısını yapılandırma

KMsi 'yi etkinleştirmek için, içerik `DataUri` tanımı öğesini [sayfa tanımlayıcısı](contentdefinitions.md#datauri) `unifiedssp` ve [sayfa sürümü](page-layout.md) *1.1.0* veya üzeri olarak ayarlayın.

1. İlkenizin uzantısı dosyasını açın. Örneğin, <em> `SocialAndLocalAccounts/` </em>. Bu uzantı dosyası, önkoşul içinde elde etmeniz gereken özel ilke başlangıç paketine dahil olan ilke dosyalarından biridir, [özel ilkeleri](custom-policy-get-started.md)kullanmaya başlayın.
1. **Buildingblocks** öğesi için arama yapın. Öğe yoksa, ekleyin.
1. **ContentDefinitions** öğesini Ilkenin **buildingblocks** öğesine ekleyin.

    Özel ilkeniz aşağıdaki kod parçacığı gibi görünmelidir:

    ```xml
    <BuildingBlocks>
      <ContentDefinitions>
        <ContentDefinition Id="api.signuporsignin">
          <DataUri>urn:com:microsoft:aad:b2c:elements:unifiedssp:1.1.0</DataUri>
        </ContentDefinition>
      </ContentDefinitions>
    </BuildingBlocks>
    ```

## <a name="add-the-metadata-to-the-self-asserted-technical-profile"></a>Otomatik olarak onaylanan teknik profile meta verileri ekleyin

Kayıt ve oturum açma sayfasına KMSI onay kutusunu eklemek için `setting.enableRememberMe` meta verileri doğru olarak ayarlayın. Uzantı dosyasındaki SelfAsserted-Localaccountsignın-email teknik profillerini geçersiz kılın.

1. ClaimsProviders öğesini bulun. Öğe yoksa, ekleyin.
1. Aşağıdaki talep sağlayıcısını ClaimsProviders öğesine ekleyin:

```XML
<ClaimsProvider>
  <DisplayName>Local Account</DisplayName>
  <TechnicalProfiles>
    <TechnicalProfile Id="SelfAsserted-LocalAccountSignin-Email">
      <Metadata>
        <Item Key="setting.enableRememberMe">True</Item>
      </Metadata>
    </TechnicalProfile>
  </TechnicalProfiles>
</ClaimsProvider>
```

1. Uzantı dosyasını kaydedin.

## <a name="configure-a-relying-party-file"></a>Bağlı olan taraf dosyası yapılandırma

Oluşturduğunuz Kullanıcı yolculuğunu başlatan bağlı olan taraf (RP) dosyasını güncelleştirin.

1. Özel ilke dosyanızı açın. Örneğin, *Signuporsignın. xml*.
1. Zaten mevcut değilse, `<UserJourneyBehaviors>` `<RelyingParty>` düğüme bir alt düğüm ekleyin. Hemen sonra `<DefaultUserJourney ReferenceId="User journey Id" />`yerleştirilmelidir, örneğin: `<DefaultUserJourney ReferenceId="SignUpOrSignIn" />`.
1. Aşağıdaki düğümü `<UserJourneyBehaviors>` öğesinin alt öğesi olarak ekleyin.

    ```XML
    <UserJourneyBehaviors>
      <SingleSignOn Scope="Tenant" KeepAliveInDays="30" />
      <SessionExpiryType>Absolute</SessionExpiryType>
      <SessionExpiryInSeconds>1200</SessionExpiryInSeconds>
    </UserJourneyBehaviors>
    ```

    - **Sessionexpiryıtype** -oturumun ve `SessionExpiryInSeconds` `KeepAliveInDays`' de belirtilen zamana göre nasıl uzatıldığını gösterir. `Rolling` Değer (varsayılan), kullanıcının kimlik doğrulaması gerçekleştirdiği her seferinde oturumun genişletildiğini gösterir. `Absolute` Değer, kullanıcının belirtilen süre sonunda yeniden kimlik doğrulaması zorlaması gerektiğini gösterir.

    - **Sessionexpirınınseconds** - *Oturumumu Açık bırak* etkin olmadığında oturum tanımlama bilgilerinin ömrü etkinleştirilmemiştir veya bir Kullanıcı Oturumumu *Açık bırak*seçeneğini seçmiyor. Oturumun süresi geçtiğinde veya `SessionExpiryInSeconds` tarayıcı kapatıldıktan sonra sona erer.

    - **Keepaliveındays** - *Oturumumu Açık bırak* etkin olduğunda oturum tanımlama bilgilerinin yaşam süresi etkindir ve Kullanıcı Oturumumu *açık tut '* i seçer.  Değeri `KeepAliveInDays` `SessionExpiryInSeconds` değerin üzerine gelir ve oturum sona erme süresini belirler. Bir kullanıcı tarayıcıyı kapatır ve daha sonra yeniden açarsa, bu kullanıcılar yine de Keepaliveındays zaman diliminde olduğu sürece sessizce oturum açabilir.

    Daha fazla bilgi için bkz. [Kullanıcı yolculuğu davranışları](relyingparty.md#userjourneybehaviors).

Aşağıdaki örnekte gösterildiği gibi, Sessionexpirınseconds değerini kısa bir süre (1200 saniye) olarak ayarlamanızı öneririz, ancak Keepaliveındays değeri görece uzun bir döneme (30 gün) ayarlanabilir:

```XML
<RelyingParty>
  <DefaultUserJourney ReferenceId="SignUpOrSignIn" />
  <UserJourneyBehaviors>
    <SingleSignOn Scope="Tenant" KeepAliveInDays="30" />
    <SessionExpiryType>Absolute</SessionExpiryType>
    <SessionExpiryInSeconds>1200</SessionExpiryInSeconds>
  </UserJourneyBehaviors>
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
    </OutputClaims>
    <SubjectNamingInfo ClaimType="sub" />
  </TechnicalProfile>
</RelyingParty>
```

## <a name="test-your-policy"></a>İlkenizi test etme

1. Değişikliklerinizi kaydedin ve sonra dosyayı karşıya yükleyin.
1. Karşıya yüklediğiniz özel ilkeyi test etmek için, Azure portal ilke sayfasına gidin ve **Şimdi Çalıştır**' ı seçin.
1. **Kullanıcı adınızı** ve **parolanızı**yazın, **Oturumumu Açık bırak**' ı seçin ve **oturum aç**' a tıklayın.
1. Azure portalına geri dönün. İlke sayfasına gidin ve ardından oturum açma URL 'sini kopyalamak için **Kopyala** ' yı seçin.
1. Tarayıcı adres çubuğunda, kullanıcıyı bu istek üzerine `&prompt=login` kimlik bilgilerini girmeye zorlayan sorgu dizesi parametresini kaldırın.
1. Tarayıcıda **Git**' e tıklayın. Artık Azure AD B2C, yeniden oturum açmanızı istemeden bir erişim belirteci yayımlayacak. 

## <a name="next-steps"></a>Sonraki adımlar

Örnek ilkeyi [burada](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/keep%20me%20signed%20in)bulabilirsiniz.
