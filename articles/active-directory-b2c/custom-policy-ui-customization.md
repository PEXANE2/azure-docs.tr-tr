---
title: Özel ilkeyle uygulamanızın kullanıcı arabirimini özelleştirme
titleSuffix: Azure AD B2C
description: Azure Active Directory B2C özel bir ilke kullanarak Kullanıcı arabirimini özelleştirme hakkında bilgi edinin.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 03/16/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: e50d6d0623e87dfa68a7cc9744c3f595ff0179c6
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80396374"
---
# <a name="customize-the-user-interface-of-your-application-using-a-custom-policy-in-azure-active-directory-b2c"></a>Azure Active Directory B2C özel bir ilke kullanarak uygulamanızın kullanıcı arabirimini özelleştirme

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Bu makaledeki adımları tamamlayarak, marka ve görünümünüzdeki bir kaydolma ve oturum açma özel ilkesi oluşturacaksınız. Azure Active Directory B2C (Azure AD B2C) sayesinde, kullanıcılara sunulan HTML ve CSS içeriğinin neredeyse tam denetimini alırsınız. Özel bir ilke kullandığınızda, Azure portal denetimleri kullanmak yerine, XML 'de UI özelleştirmesini yapılandırırsınız.

## <a name="prerequisites"></a>Ön koşullar

[Özel ilkelerle çalışmaya başlama](custom-policy-get-started.md)bölümündeki adımları uygulayın. Kaydolma ve yerel hesaplarla oturum açma için çalışan bir özel ilkenize sahip olmanız gerekir.

[!INCLUDE [active-directory-b2c-html-how-to](../../includes/active-directory-b2c-html-how-to.md)]

### <a name="4-modify-the-extensions-file"></a>4. uzantılar dosyasını değiştirme

UI özelleştirmesini yapılandırmak için, **Içerik tanımını** ve alt öğelerini temel dosyadan uzantılar dosyasına kopyalayın.

1. İlkenizin temel dosyasını açın. Örneğin, <em> `SocialAndLocalAccounts/` </em>. Bu temel dosya, önkoşul içinde elde etmeniz gereken özel ilke başlangıç paketine dahil olan ilke dosyalarından biridir, [özel ilkeleri](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-get-started-custom)kullanmaya başlayın.
1. **ContentDefinitions** öğesinin tüm içeriğini arayın ve kopyalayın.
1. Uzantı dosyasını açın. Örneğin, *TrustFrameworkExtensions. xml*. **Buildingblocks** öğesi için arama yapın. Öğe yoksa, ekleyin.
1. **Buildingblocks** öğesinin bir alt öğesi olarak kopyaladığınız **ContentDefinitions** öğesinin tüm içeriğini yapıştırın.
1. Kopyaladığınız XML içinde içeren `Id="api.signuporsignin"` **ContentDefinition** öğesini arayın.
1. **Loaduri** değerini, depolama ALANıNA yüklediğiniz HTML dosyasının URL 'si ile değiştirin. Örneğin, `https://your-storage-account.blob.core.windows.net/your-container/customize-ui.html`.

    Özel ilkeniz aşağıdaki kod parçacığı gibi görünmelidir:

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

1. Uzantı dosyasını kaydedin.

### <a name="5-upload-and-test-your-updated-custom-policy"></a>5. güncelleştirilmiş özel ilkenizi karşıya yükleyin ve test edin

#### <a name="51-upload-the-custom-policy"></a>5,1 özel ilkeyi karşıya yükleme

1. Üst menüdeki **Dizin + abonelik** filtresini seçip kiracınızı içeren dizini seçerek Azure AD B2C kiracınızı içeren dizini kullandığınızdan emin olun.
1. Arama yapın ve **Azure AD B2C**seçin.
1. **İlkeler**altında **kimlik deneyimi çerçevesi**' ni seçin.
1. **Özel Ilkeyi karşıya yükle**' yi seçin.
1. Daha önce değiştirdiğiniz uzantıları dosyasını karşıya yükleyin.

#### <a name="52-test-the-custom-policy-by-using-run-now"></a>5,2 **Şimdi Çalıştır** 'ı kullanarak özel ilkeyi test etme

1. Karşıya yüklediğiniz ilkeyi seçin ve **Şimdi Çalıştır**' ı seçin.
1. Bir e-posta adresi kullanarak kaydolabilirsiniz.

[!INCLUDE [active-directory-b2c-html-templates](../../includes/active-directory-b2c-html-templates.md)]

## <a name="configure-dynamic-custom-page-content-uri"></a>Dinamik özel sayfa içerik URI 'sini Yapılandır

Azure AD B2C özel ilke kullanarak, URL yolunda veya bir sorgu dizesinde bir parametre gönderebilirsiniz. Parametreyi HTML uç noktanıza ileterek sayfa içeriğini dinamik olarak değiştirebilirsiniz. Örneğin web veya mobil uygulamanızdan ilettiğiniz bir parametreye göre Azure AD B2C kaydolma veya oturum açma sayfanızdaki arka plan görüntüsünü değiştirebilirsiniz. Parametresi, uygulama KIMLIĞI, dil KIMLIĞI veya gibi özel sorgu dizesi parametresi gibi herhangi bir [talep Çözümleyicisi](claim-resolver-overview.md)olabilir `campaignId`.

### <a name="sending-query-string-parameters"></a>Sorgu dizesi parametreleri gönderiliyor

Sorgu dizesi parametreleri göndermek için [bağlı olan taraf ilkesinde](relyingparty.md)aşağıda gösterildiği gibi bir `ContentDefinitionParameters` öğe ekleyin.

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

İçerik tanımınızda değerini `LoadUri` olarak `https://<app_name>.azurewebsites.net/home/unified`değiştirin. Özel ilkeniz `ContentDefinition` aşağıdaki kod parçacığı gibi görünmelidir:

```XML
<ContentDefinition Id="api.signuporsignin">
  <LoadUri>https://<app_name>.azurewebsites.net/home/unified</LoadUri>
  ...
</ContentDefinition>
```

Azure AD B2C sayfayı yüklediğinde, Web sunucusu uç noktanıza bir çağrı yapar:

```http
https://<app_name>.azurewebsites.net/home/unified?campaignId=123&lang=fr&appId=f893d6d3-3b6d-480d-a330-1707bf80ebea
```

### <a name="dynamic-page-content-uri"></a>Dinamik sayfa içerik URI 'SI

İçerik, kullanılan parametrelere göre farklı konumlardan çekeklenebilir. CORS etkin uç noktanıza, ana içerik için bir klasör yapısı ayarlayın. Örneğin, içeriği aşağıdaki yapıda düzenleyebilirsiniz. *Dil/HTML dosyalarınıza göre kök klasör/klasör*. Örneğin, özel sayfa URI 'niz şöyle görünebilir:

```XML
<ContentDefinition Id="api.signuporsignin">
  <LoadUri>https://contoso.blob.core.windows.net/{Culture:LanguageName}/myHTML/unified.html</LoadUri>
  ...
</ContentDefinition>
```

Azure AD B2C, `fr` Fransızca için dil için ıkı harfli ISO kodu gönderir:

```http
https://contoso.blob.core.windows.net/fr/myHTML/unified.html
```

## <a name="next-steps"></a>Sonraki adımlar

Özelleştirilebilecek kullanıcı ARABIRIMI öğeleri hakkında daha fazla bilgi için bkz. [Kullanıcı akışları IÇIN UI özelleştirmesi başvuru kılavuzu](customize-ui-overview.md).
