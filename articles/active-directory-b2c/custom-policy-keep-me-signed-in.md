---
title: Oturumumu Açık tut Azure Active Directory B2C
description: Azure Active Directory B2C 'da Oturumumu Açık tut (KMSI) ayarlamayı öğrenin.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 02/27/2020
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 84ba68c97f69872e39121915a6edf23aa029fa75
ms.sourcegitcommit: 1f738a94b16f61e5dad0b29c98a6d355f724a2c7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2020
ms.locfileid: "78161695"
---
# <a name="enable-keep-me-signed-in-kmsi-in-azure-active-directory-b2c"></a>Azure Active Directory B2C 'da Oturumumu Açık tut (KMSI) özelliğini etkinleştir

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Web kullanıcılarınızın kullanıcıları ve Azure Active Directory B2C (Azure AD B2C) dizininizde yerel hesaplara sahip yerel uygulamalarınız için Oturumumu Açık tut (KMSı) işlevini etkinleştirebilirsiniz. Bu özellik, Kullanıcı adını ve parolasını yeniden girmesi istenmeden, uygulamanıza geri dönen kullanıcılara erişim izni verir. Kullanıcı oturumu kapattığında bu erişim iptal edilir.

Kullanıcılar bu seçeneği genel bilgisayarlarda etkinleştirmemelidir.

![Oturumumu Açık tut onay kutusunun gösterildiği örnek kaydolma, oturum açma sayfası](./media/custom-policy-keep-me-signed-in/kmsi.PNG)

## <a name="prerequisites"></a>Önkoşullar

- Yerel hesap oturum açmaya izin verecek şekilde yapılandırılmış bir Azure AD B2C kiracısı. KMSı, dış kimlik sağlayıcısı hesaplarında desteklenmez.
- [Özel ilkelerle çalışmaya başlama](custom-policy-get-started.md)bölümündeki adımları uygulayın.

## <a name="configure-the-page-identifier"></a>Sayfa tanımlayıcısını yapılandırma 

KMSI 'yi etkinleştirmek için, içerik tanımı `DataUri` öğesini [sayfa tanımlayıcısı](contentdefinitions.md#datauri) `unifiedssp` ve [sayfa sürümü](page-layout.md) *1.1.0* veya üzeri olarak ayarlayın.

1. İlkenizin uzantısı dosyasını açın. Örneğin,  <em> **`TrustFrameworkExtensions.xml`** `SocialAndLocalAccounts/`</em>. Bu uzantı dosyası, önkoşul içinde elde etmeniz gereken özel ilke başlangıç paketine dahil olan ilke dosyalarından biridir, [özel ilkeleri](custom-policy-get-started.md)kullanmaya başlayın.
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
    
1. Uzantı dosyasını kaydedin.



## <a name="configure-a-relying-party-file"></a>Bağlı olan taraf dosyası yapılandırma

Oluşturduğunuz Kullanıcı yolculuğunu başlatan bağlı olan taraf (RP) dosyasını güncelleştirin.

1. Özel ilke dosyanızı açın. Örneğin, *Signuporsignın. xml*.
1. Zaten mevcut değilse, `<RelyingParty>` düğümüne bir `<UserJourneyBehaviors>` alt düğümü ekleyin. `<DefaultUserJourney ReferenceId="User journey Id" />`hemen sonra yerleştirilmelidir, örneğin: `<DefaultUserJourney ReferenceId="SignUpOrSignIn" />`.
1. Aşağıdaki düğümü `<UserJourneyBehaviors>` öğesinin bir alt öğesi olarak ekleyin.

    ```XML
    <UserJourneyBehaviors>
      <SingleSignOn Scope="Tenant" KeepAliveInDays="30" />
      <SessionExpiryType>Absolute</SessionExpiryType>
      <SessionExpiryInSeconds>1200</SessionExpiryInSeconds>
    </UserJourneyBehaviors>
    ```

    - **Sessionexpiryıtype** -oturumun `SessionExpiryInSeconds` ve `KeepAliveInDays`belirtilen zamana göre nasıl uzatıldığını gösterir. `Rolling` değeri (varsayılan), kullanıcının kimlik doğrulaması gerçekleştirdiği her seferinde oturumun genişletildiğini gösterir. `Absolute` değeri, kullanıcının belirtilen süre geçtikten sonra yeniden kimlik doğrulaması zorlaması gerektiğini belirtir.
 
    - **Sessionexpirınınseconds** - *Oturumumu Açık bırak* etkin olmadığında oturum tanımlama bilgilerinin ömrü etkinleştirilmemiştir veya bir Kullanıcı Oturumumu *Açık bırak*seçeneğini seçmiyor. Oturumun süresi `SessionExpiryInSeconds` geçtikten sonra veya tarayıcı kapatıldıktan sonra dolar.
 
    - **Keepaliveındays** - *Oturumumu Açık bırak* etkin olduğunda oturum tanımlama bilgilerinin yaşam süresi etkindir ve Kullanıcı Oturumumu *açık tut '* i seçer.  `KeepAliveInDays` değeri `SessionExpiryInSeconds` değerden önceliklidir ve oturum sona erme süresini belirler. Bir kullanıcı tarayıcıyı kapatır ve daha sonra yeniden açarsa, bu kullanıcılar yine de Keepaliveındays zaman diliminde olduğu sürece sessizce oturum açabilir.
    
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

4. Değişikliklerinizi kaydettikten sonra dosyayı karşıya yükleyin.
5. Karşıya yüklediğiniz özel ilkeyi test etmek için, Azure portal, ilke sayfasına gidin ve **Şimdi Çalıştır**' ı seçin.

Örnek ilkeyi [burada](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/keep%20me%20signed%20in)bulabilirsiniz.
