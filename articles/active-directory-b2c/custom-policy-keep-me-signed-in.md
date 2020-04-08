---
title: Azure Active Directory B2C'de Beni Oturum Aç
description: Azure Active Directory B2C'de Beni İmzala (KMSI) nasıl ayarlayabilirsiniz öğrenin.
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
ms.sourcegitcommit: 6397c1774a1358c79138976071989287f4a81a83
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "80803169"
---
# <a name="enable-keep-me-signed-in-kmsi-in-azure-active-directory-b2c"></a>Azure Active Directory B2C'de Beni Oturum Aç (KMSI) etkinleştirin

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Azure Active Directory B2C (Azure AD B2C) dizininizde yerel hesapları olan web ve yerel uygulamalarınızın kullanıcıları için Beni İmzala (KMSI) Tut işlevini etkinleştirebilirsiniz. Bu özellik, uygulamanız için geri dönen kullanıcılara kullanıcı adlarını ve parolalarını yeniden girmelerini sormadan erişim sağlar. Bir kullanıcı imzaladığında bu erişim iptal edilir.

Kullanıcılar bu seçeneği ortak bilgisayarlarda etkinleştirmemelidir.

![Beni onay kutusunda tut imzalı olarak gösteren örnek kayıt sayfası](./media/custom-policy-keep-me-signed-in/kmsi.PNG)

## <a name="prerequisites"></a>Ön koşullar

- Yerel hesap oturum açmasına izin verecek şekilde yapılandırılan bir Azure AD B2C kiracısı. KMSI dış kimlik sağlayıcı hesapları için desteklenmez.
- [Özel ilkelerle başlayın](custom-policy-get-started.md)adımlarını tamamlayın.

## <a name="configure-the-page-identifier"></a>Sayfa tanımlayıcısını yapılandırma

KMSI'yi etkinleştirmek için `DataUri` içerik tanım öğesini [sayfa tanımlayıcısı](contentdefinitions.md#datauri) `unifiedssp` ve [sayfa sürümü](page-layout.md) *1.1.0* veya üzeri olarak ayarlayın.

1. İlkinizin uzantı dosyasını açın. Örneğin, <em> `SocialAndLocalAccounts/` </em>. Bu uzantı dosyası, ön koşulda elde etmiş olması gereken özel ilke başlangıç paketinde yer alan ilke dosyalarından biridir, [özel ilkeler ile başlayın.](custom-policy-get-started.md)
1. **BuildingBlocks** öğesini arayın. Öğe yoksa, ekleyin.
1. İçerik **Tanımları** öğesini ipolitikasının **BuildingBlocks** öğesine ekleyin.

    Özel politikanız aşağıdaki kod parçacığı gibi görünmelidir:

    ```xml
    <BuildingBlocks>
      <ContentDefinitions>
        <ContentDefinition Id="api.signuporsignin">
          <DataUri>urn:com:microsoft:aad:b2c:elements:unifiedssp:1.1.0</DataUri>
        </ContentDefinition>
      </ContentDefinitions>
    </BuildingBlocks>
    ```

## <a name="add-the-metadata-to-the-self-asserted-technical-profile"></a>Meta verileri kendi kendini öne süren teknik profile ekleme

KMSI onay kutusunu kaydolma ve kaydolma sayfasına eklemek `setting.enableRememberMe` için meta verileri doğru olarak ayarlayın. Uzantı dosyasındaki SelfAsserted-LocalAccountSignin-Email teknik profillerini geçersiz kılın.

1. İddia Sağlayıcıları öğesini bulun. Öğe yoksa, ekleyin.
1. Aşağıdaki talep sağlayıcıyı Talep Sağlayıcıları öğesine ekleyin:

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

1. Uzantılar dosyasını kaydedin.

## <a name="configure-a-relying-party-file"></a>Güvenilen bir parti dosyanı yapılandırma

Oluşturduğunuz kullanıcı yolculuğunu başlatan güvenen taraf (RP) dosyasını güncelleştirin.

1. Özel ilke dosyanızı açın. Örneğin, *SignUpOrSignin.xml*.
1. Zaten yoksa, `<UserJourneyBehaviors>` `<RelyingParty>` düğüme bir alt düğüm ekleyin. Hemen sonra `<DefaultUserJourney ReferenceId="User journey Id" />`yer almalıdır , örneğin: `<DefaultUserJourney ReferenceId="SignUpOrSignIn" />`.
1. `<UserJourneyBehaviors>` Öğenin bir alt öğesi olarak aşağıdaki düğümü ekleyin.

    ```XML
    <UserJourneyBehaviors>
      <SingleSignOn Scope="Tenant" KeepAliveInDays="30" />
      <SessionExpiryType>Absolute</SessionExpiryType>
      <SessionExpiryInSeconds>1200</SessionExpiryInSeconds>
    </UserJourneyBehaviors>
    ```

    - **SessionExpiryType** - Oturumda belirtilen süreye göre `SessionExpiryInSeconds` nasıl `KeepAliveInDays`uzatıldığını gösterir ve . Değer `Rolling` (varsayılan), kullanıcı kimlik doğrulaması her gerçekleştirişinde oturumun uzatıldığını gösterir. Değer, `Absolute` kullanıcının belirtilen süreden sonra yeniden kimlik doğrulaması yapmak zorunda kaldığını gösterir.

    - **SessionExpiryInSeconds** - Oturum çerezleri süresi *beni oturumda tutmak* etkin değil, ya da bir kullanıcı *beni oturumda tut*seçmez. Oturum geçtikten `SessionExpiryInSeconds` sonra sona erer veya tarayıcı kapatılır.

    - **KeepAliveInDays** - Oturum çerezleri ömür boyu *beni oturumda tutmak* etkin dir ve kullanıcı beni *oturumda tutmak*seçer .  Değerin `KeepAliveInDays` değeri, değerden `SessionExpiryInSeconds` önce gelir ve oturum bitiş saatini belirler. Bir kullanıcı tarayıcıyı kapatır ve daha sonra yeniden açarsa, KeepAliveInDays zaman dilimi içinde olduğu sürece sessizce oturum açabilir.

    Daha fazla bilgi için [kullanıcı yolculuğu davranışlarına](relyingparty.md#userjourneybehaviors)bakın.

SessionExpiryInSeconds değerini kısa bir süre (1200 saniye) olarak ayarlamanızı öneririz, KeepAliveInDays değeri aşağıdaki örnekte gösterildiği gibi nispeten uzun bir süre (30 gün) olarak ayarlanabilir:

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

## <a name="test-your-policy"></a>İlkinizi test edin

1. Değişikliklerinizi kaydedin ve ardından dosyayı yükleyin.
1. Azure portalında yüklediğiniz özel ilkeyi sınamak için ilke sayfasına gidin ve ardından **şimdi Çalıştır'ı**seçin.
1. Kullanıcı **adınızı** ve **şifrenizi**yazın, **beni oturum aç'ı**seçin ve oturum **aç'ı**tıklatın.
1. Azure portalına geri dönün. İlke sayfasına gidin ve **Copy** oturum açma URL'sini kopyalamak için Kopyala'yı seçin.
1. Tarayıcı adresi çubuğunda, `&prompt=login` kullanıcıyı bu istek üzerine kimlik bilgilerini girmeye zorlayan sorgu dize parametresini kaldırın.
1. Tarayıcıda **Git'i**tıklatın. Artık Azure AD B2C, yeniden oturum açmanızı istemeden bir erişim jetonu yayınlayacak. 

## <a name="next-steps"></a>Sonraki adımlar

Örnek ilkeyi [burada](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/keep%20me%20signed%20in)bulabilirsiniz.
