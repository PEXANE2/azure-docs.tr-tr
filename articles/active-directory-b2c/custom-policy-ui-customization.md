---
title: Uygulamanızın kullanıcı arabirimini özel bir ilke yle özelleştirin
titleSuffix: Azure AD B2C
description: Azure Active Directory B2C'de özel bir ilke kullanarak kullanıcı arabirimini özelleştirme hakkında bilgi edinin.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 03/16/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 87c3a3a904705b9fcb702c4745c4c80a4b447e69
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79476731"
---
# <a name="customize-the-user-interface-of-your-application-using-a-custom-policy-in-azure-active-directory-b2c"></a>Azure Active Directory B2C'de özel bir ilke kullanarak uygulamanızın kullanıcı arabirimini özelleştirin

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Bu makaledeki adımları tamamlayarak, markanız ve görünümünüz ile bir kaydolma ve kaydolma özel ilkesi oluşturursunuz. Azure Active Directory B2C (Azure AD B2C) ile kullanıcılara sunulan HTML ve CSS içeriğinin neredeyse tam denetimini elde edersiniz. Özel bir ilke kullandığınızda, Azure portalındaki denetimleri kullanmak yerine XML'de Kullanıcı Arabirimi özelleştirmesini yapılandırırsınız.

## <a name="prerequisites"></a>Ön koşullar

[Özel ilkelerle başlayın](custom-policy-get-started.md)adımlarını tamamlayın. Yerel hesaplarla kaydolmak ve oturum açmak için çalışan bir özel politikanız olmalıdır.

[!INCLUDE [active-directory-b2c-html-how-to](../../includes/active-directory-b2c-html-how-to.md)]

## <a name="4-modify-the-extensions-file"></a>4. Uzantılar dosyasını değiştirin

Kullanıcı Bilgisi özelleştirmesini yapılandırmak için **ContentDefinition** ve alt öğelerini temel dosyadan uzantılar dosyasına kopyalayın.

1. İlkinizin temel dosyasını açın. Örneğin, <em> `SocialAndLocalAccounts/` </em>. Bu temel dosya, ön koşulda elde etmiş olması gereken özel ilke başlangıç paketinde yer alan ilke dosyalarından biridir, [özel ilkeler ile başlayın.](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-get-started-custom)
1. **ContentDefinitions** öğesinin tüm içeriğini arayın ve kopyalayın.
1. Uzantı dosyasını açın. Örneğin, *TrustFrameworkExtensions.xml*. **BuildingBlocks** öğesini arayın. Öğe yoksa, ekleyin.
1. **BuildingBlocks** öğesinin alt öğesi olarak kopyaladığınız **ContentDefinitions** öğesinin tüm içeriğini yapıştırın.
1. Kopyaladığınız XML'de `Id="api.signuporsignin"` bulunan **ContentDefinition** öğesini arayın.
1. **LoadUri'nin** değerini depolama alanına yüklediğiniz HTML dosyasının URL'si ile değiştirin. Örneğin, `https://your-storage-account.blob.core.windows.net/your-container/customize-ui.html`.

    Özel politikanız aşağıdaki kod parçacığı gibi görünmelidir:

    ```xml
    <BuildingBlocks>
      <ContentDefinitions>
        <ContentDefinition Id="api.signuporsignin">
          <LoadUri>https://your-storage-account.blob.core.windows.net/your-container/customize-ui.html</LoadUri>
          <RecoveryUri>~/common/default_page_error.html</RecoveryUri>
          <DataUri>urn:com:microsoft:aad:b2c:elements:unifiedssp:1.0.0</DataUri>
          <Metadata>
            <Item Key="DisplayName">Signin and Signup</Item>
          </Metadata>
        </ContentDefinition>
      </ContentDefinitions>
    </BuildingBlocks>
    ```

1. Uzantılar dosyasını kaydedin.

## <a name="5-upload-and-test-your-updated-custom-policy"></a>5. Güncellenmiş özel politikanızı yükleyin ve test edin

### <a name="51-upload-the-custom-policy"></a>5.1 Özel politikayı yükleme

1. Üst menüdeki **Dizin + abonelik** filtresini seçerek ve kiracınızı içeren dizin seçerek Azure AD B2C kiracınızı içeren dizini kullandığınızdan emin olun.
1. Azure AD **B2C'yi**arayın ve seçin.
1. **İlkeler**altında Kimlik **Deneyimi Çerçevesi'ni**seçin.
1. **Özel politika yükle'yi**seçin.
1. Daha önce değiştirdiğiniz uzantılar dosyasını yükleyin.

### <a name="52-test-the-custom-policy-by-using-run-now"></a>5.2 Şimdi Çalıştır'ı kullanarak özel ilkeyi test **edin**

1. Yüklediğiniz ilkeyi seçin ve ardından **Şimdi Çalıştır'ı**seçin.
1. Bir e-posta adresi kullanarak kaydolmalısınız.

[!INCLUDE [active-directory-b2c-html-templates](../../includes/active-directory-b2c-html-templates.md)]

## <a name="configure-dynamic-custom-page-content-uri"></a>Dinamik özel sayfa içeriği URI yapılandırma

Azure AD B2C özel ilkelerini kullanarak, URL yoluna bir parametre veya sorgu dizesi gönderebilirsiniz. Parametreyi HTML uç noktanıza ileterek sayfa içeriğini dinamik olarak değiştirebilirsiniz. Örneğin web veya mobil uygulamanızdan ilettiğiniz bir parametreye göre Azure AD B2C kaydolma veya oturum açma sayfanızdaki arka plan görüntüsünü değiştirebilirsiniz. Parametre, uygulama kimliği, dil kimliği veya `campaignId`özel sorgu dize parametresi gibi herhangi bir talep [çözümleyicisi](claim-resolver-overview.md)olabilir.

### <a name="sending-query-string-parameters"></a>Sorgu dize parametrelerini gönderme

Sorgu dize parametrelerini göndermek için, bağlı `ContentDefinitionParameters` taraf [ilkesinde,](relyingparty.md)aşağıda gösterildiği gibi bir öğe ekleyin.

```XML
<RelyingParty>
    <DefaultUserJourney ReferenceId="SignUpOrSignIn" />
    <UserJourneyBehaviors>
    <ContentDefinitionParameters>
        <Parameter Name="campaignId">{OAUTH-KV:campaignId}</Parameter>
        <Parameter Name="lang">{Culture:LanguageName}</Parameter>
        <Parameter Name="appId">{OIDC:ClientId}</Parameter>
    </ContentDefinitionParameters>
    </UserJourneyBehaviors>
    ...
</RelyingParty>
```

İçerik tanımınızda, 'nin `LoadUri` değerini `https://<app_name>.azurewebsites.net/home/unified`' ye göre değiştirin. Özel politikanız `ContentDefinition` aşağıdaki kod parçacığı gibi görünmelidir:

```XML
<ContentDefinition Id="api.signuporsignin">
  <LoadUri>https://<app_name>.azurewebsites.net/home/unified</LoadUri>
  ...
</ContentDefinition>
```

Azure AD B2C sayfayı yüklendiğinde, web sunucusu bitiş noktanıza bir çağrı yapar:

```http
https://<app_name>.azurewebsites.net/home/unified?campaignId=123&lang=fr&appId=f893d6d3-3b6d-480d-a330-1707bf80ebea
```

### <a name="dynamic-page-content-uri"></a>Dinamik sayfa içeriği URI

İçerik, kullanılan parametrelere göre farklı yerlerden çekilebilir. CORS özellikli bitiş noktanızda, içeriği barındıracak bir klasör yapısı ayarlayın. Örneğin, içeriği aşağıdaki yapıda düzenleyebilirsiniz. *Dil/html dosyaları başına kök klasör/klasör*. Örneğin, özel sayfanız URI şu şekilde görünebilir:

```XML
<ContentDefinition Id="api.signuporsignin">
  <LoadUri>https://contoso.blob.core.windows.net/{Culture:LanguageName}/myHTML/unified.html</LoadUri>
  ...
</ContentDefinition>
```

Azure AD B2C, dil için Iki harfli ISO kodunu Fransızca `fr` olarak gönderir:

```http
https://contoso.blob.core.windows.net/fr/myHTML/unified.html
```

## <a name="next-steps"></a>Sonraki adımlar

Özelleştirilebilir Kullanıcı Arabirimi öğeleri hakkında daha fazla bilgi için, [kullanıcı akışları için Kullanıcı Arabirimi özelleştirmeiçin başvuru kılavuzuna](customize-ui-overview.md)bakın.
