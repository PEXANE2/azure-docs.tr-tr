---
title: Application Insights ile kullanıcı davranışını izleme
titleSuffix: Azure AD B2C
description: Azure AD B2C kullanıcı yolculuklarından Uygulama Öngörüleri'nde etkinlik günlüklerini özel ilkeler kullanarak nasıl etkinleştirmeyi öğrenin.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.date: 04/05/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 25e62e7c6865f91daa242a33a0f491f8015be41a
ms.sourcegitcommit: b129186667a696134d3b93363f8f92d175d51475
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80672529"
---
# <a name="track-user-behavior-in-azure-active-directory-b2c-using-application-insights"></a>Uygulama Öngörülerini kullanarak Azure Active Directory B2C'deki kullanıcı davranışını izleme

[!INCLUDE [active-directory-b2c-public-preview](../../includes/active-directory-b2c-public-preview.md)]

Azure Active Directory B2C (Azure AD B2C), Azure AD B2C'ye sağlanan enstrümantasyon anahtarını kullanarak etkinlik verilerinin doğrudan [Uygulama Öngörüleri'ne](../azure-monitor/app/app-insights-overview.md) gönderilmesini destekler.  Application Insights teknik profili ile, kullanıcı yolculuklarınız için ayrıntılı ve özelleştirilmiş etkinlik günlükleri alabilirsiniz:

* Kullanıcı davranışları hakkında bilgi edinin.
* Geliştirme veya üretimde kendi ilkelerinizi giderin.
* Performansı ölçün.
* Uygulama Öngörüleri'nden bildirimler oluşturun.

## <a name="how-it-works"></a>Nasıl çalışır?

[Application Insights](application-insights-technical-profile.md) teknik profili, Azure AD B2C'den bir olayı tanımlar. Profil, olayın adını, kaydedilen talepleri ve enstrümantasyon anahtarını belirtir. Bir olayı göndermek için teknik [profil, kullanıcı yolculuğunda](userjourneys.md)bir düzenleme adımı olarak eklenir.

Uygulama Öngörüleri, bir kullanıcı oturumunu kaydetmek için bir korelasyon kimliği kullanarak olayları birlağan abilir. Application Insights, etkinliği ve oturumu saniyeler içinde kullanıma sunar ve birçok görselleştirme, dışa aktarma ve analitik araç sunar.

## <a name="prerequisites"></a>Ön koşullar

[Özel ilkelerle başlayın](custom-policy-get-started.md)adımlarını tamamlayın. Yerel hesaplarla kaydolmak ve oturum açmak için çalışan bir özel politikanız olmalıdır.

## <a name="create-an-application-insights-resource"></a>Application Insights kaynağı oluşturma

Azure AD B2C ile Uygulama Öngörüleri'ni kullanırken tek yapmanız gereken bir kaynak oluşturmak ve enstrümantasyon anahtarını almaktır. Bilgi için [bkz.](../azure-monitor/app/create-new-resource.md)

1. [Azure portalında](https://portal.azure.com/)oturum açın.
2. Üst menüdeki **Dizin + abonelik** filtresini seçerek ve aboneliğinizi içeren dizin seçerek Azure aboneliğinizi içeren dizini kullandığınızdan emin olun. Bu kiracı, Azure AD B2C kiracınız değildir.
3. Azure portalının sol üst köşesinde **kaynak oluştur'u** seçin ve ardından **Application Insights'ı**arayın ve seçin.
4. **Oluştur'u**tıklatın.
5. Kaynak için bir **Ad** girin.
6. **Uygulama Türü**için, web **uygulaması ASP.NET**seçin.
7. **Kaynak Grubu**için varolan bir grup seçin veya yeni bir grup için bir ad girin.
8. **Oluştur'u**tıklatın.
4. Application Insights kaynağını oluşturduktan sonra, bu kaynağı açın, **Essentials'ı**genişletin ve enstrümantasyon anahtarını kopyalayın.

![Uygulama Öngörüleri Genel Bakış ve Enstrümantasyon Anahtarı](./media/analytics-with-application-insights/app-insights.png)

## <a name="define-claims"></a>Talepleri tanımlama

Talep, Azure AD B2C ilke yürütmesi sırasında geçici bir veri depolama sağlar. [İddia şeması,](claimsschema.md) iddialarınızı beyan ettiğiniz yerdir.

1. İlkinizin uzantılar dosyasını açın. Örneğin, <em> `SocialAndLocalAccounts/` </em>.
1. [BuildingBlocks](buildingblocks.md) öğesini arayın. Öğe yoksa, ekleyin.
1. [ClaimsSchema](claimsschema.md) öğesini bulun. Öğe yoksa, ekleyin.
1. Aşağıdaki iddiaları **ClaimsSchema** öğesine ekleyin. 

```xml
<ClaimType Id="EventType">
  <DisplayName>Event type</DisplayName>
  <DataType>string</DataType>
</ClaimType>
<ClaimType Id="EventTimestamp">
  <DisplayName>Event timestamp</DisplayName>
  <DataType>string</DataType>
</ClaimType>
<ClaimType Id="PolicyId">
  <DisplayName>Policy Id</DisplayName>
  <DataType>string</DataType>
</ClaimType>
<ClaimType Id="Culture">
  <DisplayName>Culture ID</DisplayName>
  <DataType>string</DataType>
</ClaimType>
<ClaimType Id="CorrelationId">
  <DisplayName>Correlation Id</DisplayName>
  <DataType>string</DataType>
</ClaimType>
<ClaimType Id="federatedUser">
  <DisplayName>Federated user</DisplayName>
  <DataType>boolean</DataType>
</ClaimType>
<ClaimType Id="parsedDomain">
  <DisplayName>Domain name</DisplayName>
  <DataType>string</DataType>
  <UserHelpText>The domain portion of the email address.</UserHelpText>
</ClaimType>
<ClaimType Id="userInLocalDirectory">
  <DisplayName>userInLocalDirectory</DisplayName>
  <DataType>boolean</DataType>
</ClaimType>
```

## <a name="add-new-technical-profiles"></a>Yeni teknik profiller ekleme

Teknik profiller, Azure AD B2C'nin Kimlik Deneyimi Çerçevesi'nde işlev olarak kabul edilebilir. Bu tablo, bir oturum açmak ve olayları göndermek için kullanılan teknik profilleri tanımlar.

| Teknik Profil | Görev |
| ----------------- | -----|
| AppInsights-Ortak | Tüm Azure İstatistikleri teknik profillerine eklenecek ortak parametre kümesi. |
| AppInsights-SignInRequest | Oturum `SignInRequest` açma isteği alındığı zaman bir olayı bir dizi taleple kaydeder. |
| AppInsights-UserSignUp | Kullanıcı `UserSignUp` kaydolma/kaydolma yolculuğunda kaydolma seçeneğini tetiklediğinde bir olayı kaydeder. |
| AppInsights-SignInComplete | Bir `SignInComplete` belirteç güvenen taraf uygulamasına gönderildiğinde, kimlik doğrulamasının başarıyla tamamlanmasıyla ilgili bir olayı kaydeder. |

Profilleri başlangıç paketinden *TrustFrameworkExtensions.xml* dosyasına ekleyin. Bu öğeleri **ClaimsProviders** öğesine ekleyin:

```xml
<ClaimsProvider>
  <DisplayName>Application Insights</DisplayName>
  <TechnicalProfiles>
    <TechnicalProfile Id="AppInsights-Common">
      <DisplayName>Application Insights</DisplayName>
      <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.Insights.AzureApplicationInsightsProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
      <Metadata>
        <!-- The ApplicationInsights instrumentation key which will be used for logging the events -->
        <Item Key="InstrumentationKey">xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx</Item>
        <Item Key="DeveloperMode">false</Item>
        <Item Key="DisableTelemetry ">false</Item>
      </Metadata>
      <InputClaims>
        <!-- Properties of an event are added through the syntax {property:NAME}, where NAME is property being added to the event. DefaultValue can be either a static value or a value that's resolved by one of the supported DefaultClaimResolvers. -->
        <InputClaim ClaimTypeReferenceId="EventTimestamp" PartnerClaimType="{property:EventTimestamp}" DefaultValue="{Context:DateTimeInUtc}" />
        <InputClaim ClaimTypeReferenceId="PolicyId" PartnerClaimType="{property:Policy}" DefaultValue="{Policy:PolicyId}" />
        <InputClaim ClaimTypeReferenceId="CorrelationId" PartnerClaimType="{property:CorrelationId}" DefaultValue="{Context:CorrelationId}" />
        <InputClaim ClaimTypeReferenceId="Culture" PartnerClaimType="{property:Culture}" DefaultValue="{Culture:RFC5646}" />
      </InputClaims>
    </TechnicalProfile>

    <TechnicalProfile Id="AppInsights-SignInRequest">
      <InputClaims>
        <!-- An input claim with a PartnerClaimType="eventName" is required. This is used by the AzureApplicationInsightsProvider to create an event with the specified value. -->
        <InputClaim ClaimTypeReferenceId="EventType" PartnerClaimType="eventName" DefaultValue="SignInRequest" />
      </InputClaims>
      <IncludeTechnicalProfile ReferenceId="AppInsights-Common" />
    </TechnicalProfile>

    <TechnicalProfile Id="AppInsights-UserSignUp">
      <InputClaims>
        <InputClaim ClaimTypeReferenceId="EventType" PartnerClaimType="eventName" DefaultValue="UserSignUp" />
      </InputClaims>
      <IncludeTechnicalProfile ReferenceId="AppInsights-Common" />
    </TechnicalProfile>
    
    <TechnicalProfile Id="AppInsights-SignInComplete">
      <InputClaims>
        <InputClaim ClaimTypeReferenceId="EventType" PartnerClaimType="eventName" DefaultValue="SignInComplete" />
        <InputClaim ClaimTypeReferenceId="federatedUser" PartnerClaimType="{property:FederatedUser}" DefaultValue="false" />
        <InputClaim ClaimTypeReferenceId="parsedDomain" PartnerClaimType="{property:FederationPartner}" DefaultValue="Not Applicable" />
      </InputClaims>
      <IncludeTechnicalProfile ReferenceId="AppInsights-Common" />
    </TechnicalProfile>
  </TechnicalProfiles>
</ClaimsProvider>
```

> [!IMPORTANT]
> Teknik profildeki enstrümantasyon anahtarını, `AppInsights-Common` Application Insights kaynağınızın sağladığı GUID ile değiştirin.

## <a name="add-the-technical-profiles-as-orchestration-steps"></a>Teknik profilleri orkestrasyon adımları olarak ekleme

Oturum `AppInsights-SignInRequest` açma/kaydolma isteğinin alındığını izlemek için 2 numaralı orkestrasyon adımı olarak arayın:

```xml
<!-- Track that we have received a sign in request -->
<OrchestrationStep Order="1" Type="ClaimsExchange">
  <ClaimsExchanges>
    <ClaimsExchange Id="TrackSignInRequest" TechnicalProfileReferenceId="AppInsights-SignInRequest" />
  </ClaimsExchanges>
</OrchestrationStep>
```

Orkestrasyon adımından `SendClaims` hemen *önce,* `AppInsights-UserSignup`çağıran yeni bir adım ekleyin. Kullanıcı kaydolma/kaydolma yolculuğunda kaydolma düğmesini seçtiğinde tetiklenir.

```xml
<!-- Handles the user clicking the sign up link in the local account sign in page -->
<OrchestrationStep Order="8" Type="ClaimsExchange">
  <Preconditions>
    <Precondition Type="ClaimsExist" ExecuteActionsIf="false">
      <Value>newUser</Value>
      <Action>SkipThisOrchestrationStep</Action>
    </Precondition>
    <Precondition Type="ClaimEquals" ExecuteActionsIf="true">
      <Value>newUser</Value>
      <Value>false</Value>
      <Action>SkipThisOrchestrationStep</Action>
    </Precondition>
  </Preconditions>
  <ClaimsExchanges>
    <ClaimsExchange Id="TrackUserSignUp" TechnicalProfileReferenceId="AppInsights-UserSignup" />
  </ClaimsExchanges>
</OrchestrationStep>
```

Orkestrasyon adımından `SendClaims` `AppInsights-SignInComplete`hemen sonra, . Bu adım, başarıyla tamamlanmış bir yolculuğu gösterir.

```xml
<!-- Track that we have successfully sent a token -->
<OrchestrationStep Order="10" Type="ClaimsExchange">
  <ClaimsExchanges>
    <ClaimsExchange Id="TrackSignInComplete" TechnicalProfileReferenceId="AppInsights-SignInComplete" />
  </ClaimsExchanges>
</OrchestrationStep>
```

> [!IMPORTANT]
> Yeni düzenleme adımlarını ekledikten sonra, 1'den N'ye herhangi bir tümsedo'yu atlamadan adımları sırayla yeniden numaralandırın.


## <a name="upload-your-file-run-the-policy-and-view-events"></a>Dosyanızı yükleyin, ilkeyi çalıştırın ve olayları görüntüleyin

*TrustFrameworkExtensions.xml* dosyasını kaydedin ve yükleyin. Ardından, uygulamanızdan güvenen taraf ilkesini arayın veya Azure portalında **Şimdi Çalıştır'ı** kullanın. Saniyeler içinde, etkinlikleriniz Application Insights'ta kullanılabilir.

1. Azure Etkin Dizin kiracınızda **Uygulama Öngörüleri** kaynağını açın.
2. **Kullanım** > **Etkinlikleri'ni**seçin.
3. **Son saat** **boyunca** **ve** **3 dakika**ya kadar ayarlayın.  Sonuçları görüntülemek için **Yenile'yi** seçmeniz gerekebilir.

![Uygulama Öngörüleri KULLANIM-Olaylar Blase](./media/analytics-with-application-insights/app-ins-graphic.png)

## <a name="optional-collect-more-data"></a>[İsteğe bağlı] Daha fazla veri toplama

İhtiyaçlarınıza uyacak şekilde kullanıcı yolculuğunuza talep türleri ve etkinlikler ekleyin. [Talep çözümleyicilerini](claim-resolver-overview.md) veya herhangi bir dize talep türünü kullanabilir, Uygulama Öngörüleri etkinliğine veya AppInsights-Common teknik profiline bir **Giriş Talebi** öğesi ekleyerek talepleri ekleyebilirsiniz.

- **ClaimTypeReferenceId** bir talep türüne başvurudur.
- **OrtakClaimType,** Azure Öngörüleri'nde görünen özelliğin adıdır. Olaya özelliğin `{property:NAME}`eklendiği `NAME` yerin sözdizimini kullanın.
- **DefaultValue** herhangi bir dize değeri veya talep çözümleyicisi kullanın.

```XML
<InputClaim ClaimTypeReferenceId="app_session" PartnerClaimType="{property:app_session}" DefaultValue="{OAUTH-KV:app_session}" />
<InputClaim ClaimTypeReferenceId="loyalty_number" PartnerClaimType="{property:loyalty_number}" DefaultValue="{OAUTH-KV:loyalty_number}" />
<InputClaim ClaimTypeReferenceId="language" PartnerClaimType="{property:language}" DefaultValue="{Culture:RFC5646}" />
```

## <a name="next-steps"></a>Sonraki adımlar

- IEF başvurusunda [Application Insights](application-insights-technical-profile.md) teknik profili hakkında daha fazla bilgi edinin. 
