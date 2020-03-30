---
title: Uygulama Öngörüleri ile özel ilkeleri sorun giderme
titleSuffix: Azure AD B2C
description: Özel ilkelerinizin yürütülmesini izlemek için Uygulama Öngörüleri nasıl ayarlayınız?
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 11/04/2019
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 403dbe6106cb7a1d277ba672112d2bc45dbc2987
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78186276"
---
# <a name="collect-azure-active-directory-b2c-logs-with-application-insights"></a>Uygulama Öngörüleri ile Azure Active Directory B2C günlüklerini topla

Bu makalede, özel ilkelerinizle ilgili sorunları tanılayabilmeniz için Active Directory B2C'den (Azure AD B2C) günlükleri toplamak için adımlar sağlanır. Uygulama Öngörüleri, özel durumları tanılamak ve uygulama performansı sorunlarını görselleştirmek için bir yol sağlar. Azure AD B2C, Uygulama Öngörüleri'ne veri gönderme özelliği içerir.

Burada açıklanan ayrıntılı etkinlik günlükleri **yalnızca** özel ilkelerinizin geliştirilmesi sırasında etkinleştirilmelidir.

> [!WARNING]
> Üretimde geliştirme modunu etkinleştirme. Günlükler, kimlik sağlayıcılara gönderilen ve kimlik sağlayıcılardan gönderilen tüm talepleri toplar. Geliştirici olarak, Application Insights günlüklerinizde toplanan kişisel verilerin sorumluluğunu siz üstlenebilirsiniz. Bu ayrıntılı günlükler yalnızca ilke DEVELOPER **MODE'a**yerleştirildiğinde toplanır.

## <a name="set-up-application-insights"></a>Uygulama Öngörülerini Ayarlama

Zaten bir tane yoksa, aboneliğinizde Uygulama Öngörüleri'nin bir örneğini oluşturun.

1. [Azure portalında](https://portal.azure.com)oturum açın.
1. Üst menüdeki **Dizin + abonelik** filtresini seçin ve ardından Azure aboneliğinizi içeren dizin 'i (Azure AD B2C dizininiz değil) seçin.
1. Sol navigasyon menüsünde **kaynak oluştur'u** seçin.
1. **Uygulama Öngörülerini**arayın ve seçin, ardından **Oluştur'u**seçin.
1. Formu doldurun, **Gözden Geçir + oluştur'u**seçin ve sonra **Oluştur'u**seçin.
1. Dağıtım tamamlandıktan sonra **kaynağa git'i**seçin.
1. Uygulama Öngörüleri menüsünde **Yapılandırışla** menüsünde **Özellikler'i**seçin.
1. Daha sonraki bir adımda kullanılmak üzere **INSTRUMENTATION KEY'i** kaydedin.

## <a name="configure-the-custom-policy"></a>Özel ilkeyi yapılandırma

1. Bağlı taraf (RP) dosyasını açın, örneğin *SignUpOrSignin.xml*.
1. `<TrustFrameworkPolicy>` Öğeye aşağıdaki öznitelikleri ekleyin:

   ```XML
   DeploymentMode="Development"
   UserJourneyRecorderEndpoint="urn:journeyrecorder:applicationinsights"
   ```

1. Zaten yoksa, `<UserJourneyBehaviors>` `<RelyingParty>` düğüme bir alt düğüm ekleyin. Hemen sonra `<DefaultUserJourney ReferenceId="UserJourney Id" from your extensions policy, or equivalent (for example:SignUpOrSigninWithAAD" />`bulunmalıdır.
1. `<UserJourneyBehaviors>` Öğenin bir alt öğesi olarak aşağıdaki düğümü ekleyin. Daha önce `{Your Application Insights Key}` kaydettiğiniz Application Insights **Instrumentation Key** ile değiştirdiğinizi unutmayın.

    ```XML
    <JourneyInsights TelemetryEngine="ApplicationInsights" InstrumentationKey="{Your Application Insights Key}" DeveloperMode="true" ClientEnabled="false" ServerEnabled="true" TelemetryVersion="1.0.0" />
    ```

    * `DeveloperMode="true"`işlem boru hattı üzerinden telemetri hızlandırmak için ApplicationInsights söyler. Geliştirme için iyi, ama yüksek hacimlerde kısıtlı.
    * `ClientEnabled="true"`sayfa görünümünü ve istemci tarafı hatalarını izlemek için ApplicationInsights istemci tarafı komut dosyasını gönderir. Bunları Application Insights portalındaki **tarayıcıZamanlamatablosunda** görüntüleyebilirsiniz. Ayar `ClientEnabled= "true"`yaparak, sayfa komut dosyasına Uygulama Öngörüleri eklersiniz ve sayfa yüklerinin ve AJAX çağrılarının zamanlamalarını, sayımları, tarayıcı özel durumlarının ayrıntılarını ve AJAX hatalarını ve kullanıcı ve oturum sayılarını alırsınız. Bu alan **isteğe bağlıdır** `false` ve varsayılan olarak ayarlanır.
    * `ServerEnabled="true"`mevcut UserJourneyRecorder JSON'ı Özel bir etkinlik olarak Uygulama Öngörüleri'ne gönderir.

    Örnek:

    ```XML
    <TrustFrameworkPolicy
      ...
      TenantId="fabrikamb2c.onmicrosoft.com"
      PolicyId="SignUpOrSignInWithAAD"
      DeploymentMode="Development"
      UserJourneyRecorderEndpoint="urn:journeyrecorder:applicationinsights"
    >
    ...
    <RelyingParty>
      <DefaultUserJourney ReferenceId="UserJourney ID from your extensions policy, or equivalent (for example: SignUpOrSigninWithAzureAD)" />
      <UserJourneyBehaviors>
        <JourneyInsights TelemetryEngine="ApplicationInsights" InstrumentationKey="{Your Application Insights Key}" DeveloperMode="true" ClientEnabled="false" ServerEnabled="true" TelemetryVersion="1.0.0" />
      </UserJourneyBehaviors>
      ...
    </TrustFrameworkPolicy>
    ```

1. İlkeyi yükleyin.

## <a name="see-the-logs-in-application-insights"></a>Uygulama İstatistikleri'ndeki günlüklere bakın

Uygulama Öngörüleri'nde yeni günlükleri göremeden önce genellikle beş dakikadan kısa bir gecikme vardır.

1. [Azure portalında](https://portal.azure.com)oluşturduğunuz Uygulama Öngörüleri kaynağını açın.
1. Genel **Bakış** menüsünde **Analytics'i**seçin.
1. Uygulama Öngörüleri'nde yeni bir sekme açın.

Günlükleri görmek için kullanabileceğiniz sorguların listesi aşağıda veda edebilirsiniz:

| Sorgu | Açıklama |
|---------------------|--------------------|
`traces` | Azure AD B2C tarafından oluşturulan tüm günlükleri görün |
`traces | where timestamp > ago(1d)` | Azure AD B2C tarafından son gün için oluşturulan tüm günlükleri görün

Girişler uzun olabilir. Daha yakından bakmak için CSV'ye dışa aktarın.

Sorgulama hakkında daha fazla bilgi için [Azure Monitor'daki günlük sorgularına genel bakış](../azure-monitor/log-query/log-query-overview.md)bölümüne bakın.

## <a name="next-steps"></a>Sonraki adımlar

Topluluk, kimlik geliştiricilerine yardımcı olmak için bir kullanıcı yolculuğu görüntüleyicisi geliştirmiştir. Uygulama Öngörüleri örneğinden okur ve kullanıcı yolculuğu olaylarının iyi yapılandırılmış bir görünümünü sağlar. Kaynak kodunu elde ve kendi çözüm dağıtmak.

Kullanıcı yolculuğu oynatıcısı Microsoft tarafından desteklenmez ve olduğu gibi kullanıma sunulmuştur.

GitHub'daki Application Insights'taki olayları okuyan görüntüleyici sürümünü burada bulabilirsiniz:

[Azure-Örnekler/etkin-dizini-b2c-gelişmiş ilkeler](https://github.com/Azure-Samples/active-directory-b2c-advanced-policies/tree/master/wingtipgamesb2c/src/WingTipUserJourneyPlayerWebApplication)
