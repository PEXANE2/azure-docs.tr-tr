---
title: Application Insights ile Kullanıcı davranışını izleme
titleSuffix: Azure AD B2C
description: Özel ilkeler kullanarak Azure AD B2C Kullanıcı ilerinizden Application Insights olay günlüklerini nasıl etkinleştirebileceğinizi öğrenin.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.topic: how-to
ms.workload: identity
ms.date: 04/05/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 67ea7324419d86fa5b5c23a2f0aa5f8c057495d1
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85385986"
---
# <a name="track-user-behavior-in-azure-active-directory-b2c-using-application-insights"></a>Application Insights kullanarak Azure Active Directory B2C Kullanıcı davranışını izleme

[!INCLUDE [active-directory-b2c-public-preview](../../includes/active-directory-b2c-public-preview.md)]

Azure Active Directory B2C (Azure AD B2C), Azure AD B2C için sunulan izleme anahtarını kullanarak doğrudan olay verilerini [Application Insights](../azure-monitor/app/app-insights-overview.md) göndermeyi destekler.  Application Insights teknik bir profille, Kullanıcı yolculukları için ayrıntılı ve özelleştirilmiş olay günlükleri edinebilirsiniz:

* Kullanıcı davranışında Öngörüler elde edin.
* Geliştirme veya üretimde kendi ilkelerinizin sorunlarını giderin.
* Ölçüm performansı.
* Application Insights bildirimler oluşturun.

## <a name="how-it-works"></a>Nasıl çalışır?

[Application Insights](application-insights-technical-profile.md) teknik profili Azure AD B2C bir olay tanımlar. Profil, olayın adını, kaydedilen talepleri ve izleme anahtarını belirtir. Bir olay göndermek için, teknik profil [Kullanıcı yolculuğunda](userjourneys.md)bir düzenleme adımı olarak eklenir.

Application Insights, bir Kullanıcı oturumu kaydetmek için bir bağıntı KIMLIĞI kullanarak olayları birleştirebilirsiniz. Application Insights, olay ve oturumu Saniyeler içinde kullanılabilir hale getirir ve birçok görselleştirme, dışarı aktarma ve analitik araç sunar.

## <a name="prerequisites"></a>Ön koşullar

[Özel ilkelerle çalışmaya başlama](custom-policy-get-started.md)bölümündeki adımları uygulayın. Kaydolma ve yerel hesaplarla oturum açma için çalışan bir özel ilkenize sahip olmanız gerekir.

## <a name="create-an-application-insights-resource"></a>Application Insights kaynağı oluşturma

Azure AD B2C Application Insights kullanırken, tek yapmanız gereken bir kaynak oluşturur ve izleme anahtarını alır. Bilgi için bkz. [Application Insights kaynak oluşturma](../azure-monitor/app/create-new-resource.md)

1. [Azure portalında](https://portal.azure.com/) oturum açın.
2. Üst menüdeki **Dizin + abonelik** filtresini seçip aboneliğinizi içeren dizini seçerek Azure aboneliğinizi içeren dizini kullandığınızdan emin olun. Bu kiracı Azure AD B2C kiracınız değil.
3. Azure portal, sol üst köşedeki **kaynak oluştur** ' u seçin ve **Application Insights**arayıp seçin.
4. **Oluştur**'a tıklayın.
5. Kaynak için bir **ad** girin.
6. **Uygulama türü**için **ASP.NET Web uygulaması**' nı seçin.
7. **Kaynak grubu**için mevcut bir grubu seçin veya yeni bir grup için bir ad girin.
8. **Oluştur**'a tıklayın.
4. Application Insights kaynağını oluşturduktan sonra açın, **temel**bileşenler ' i genişletin ve izleme anahtarını kopyalayın.

![Application Insights genel bakış ve Izleme anahtarı](./media/analytics-with-application-insights/app-insights.png)

## <a name="define-claims"></a>Talepleri tanımlama

Bir talep, Azure AD B2C ilkesi yürütmesi sırasında verilerin geçici bir şekilde depolanmasını sağlar. [Talep şeması](claimsschema.md) , taleplerinizi bildirdiğiniz yerdir.

1. İlkenizin uzantıları dosyasını açın. Örneğin, <em>`SocialAndLocalAccounts/`**`TrustFrameworkExtensions.xml`**</em> .
1. [Buildingblocks](buildingblocks.md) öğesi için arama yapın. Öğe yoksa, ekleyin.
1. [Claimsschema](claimsschema.md) öğesini bulun. Öğe yoksa, ekleyin.
1. Aşağıdaki talepleri **Claimsschema** öğesine ekleyin. 

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

## <a name="add-new-technical-profiles"></a>Yeni teknik profiller ekleyin

Teknik profiller, Azure AD B2C kimlik deneyimi çerçevesinde işlevler olarak kabul edilebilir. Bu tabloda, bir oturumu açmak ve olayları göndermek için kullanılan teknik profiller tanımlanmaktadır.

| Teknik profil | Görev |
| ----------------- | -----|
| Appınsights-ortak | Tüm Azure Insights teknik profillerine dahil edilecek ortak parametre kümesi. |
| Appınsights-Signınrequest | Bir `SignInRequest` oturum açma isteği alındığında bir talep kümesiyle olay kaydeder. |
| Appınsights-UserSignUp | `UserSignUp`Kullanıcı kaydolma/oturum açma yolculuğunda oturum açma seçeneğini tetiklediğinde bir olayı kaydeder. |
| Appınsights-Signcomplete | `SignInComplete`Bağlı olan taraf uygulamasına bir belirteç gönderildiğinde bir kimlik doğrulamasının başarıyla tamamlanmasında bir olay kaydeder. |

Profilleri başlangıç paketinden *TrustFrameworkExtensions.xml* dosyasına ekleyin. Bu öğeleri **Claimsproviders** öğesine ekleyin:

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
> `AppInsights-Common`Teknik profildeki izleme anahtarını Application Insights kaynağınızın SAĞLADıĞı GUID ile değiştirin.

## <a name="add-the-technical-profiles-as-orchestration-steps"></a>Teknik profilleri düzenleme adımları olarak ekleme

`AppInsights-SignInRequest`Bir oturum açma/kaydolma isteğinin alındığını izlemek için düzenleme adımı 2 olarak çağırın:

```xml
<!-- Track that we have received a sign in request -->
<OrchestrationStep Order="2" Type="ClaimsExchange">
  <ClaimsExchanges>
    <ClaimsExchange Id="TrackSignInRequest" TechnicalProfileReferenceId="AppInsights-SignInRequest" />
  </ClaimsExchanges>
</OrchestrationStep>
```

*before* `SendClaims` Düzenleme adımından hemen önce, çağıran yeni bir adım ekleyin `AppInsights-UserSignup` . Kullanıcı kaydolma/oturum açma yolculuğunda oturum açma düğmesini seçtiğinde tetiklenir.

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

`SendClaims`Düzenleme adımından hemen sonra öğesini çağırın `AppInsights-SignInComplete` . Bu adımda başarıyla tamamlanan bir yolculuğun bulunduğu gösterilmektedir.

```xml
<!-- Track that we have successfully sent a token -->
<OrchestrationStep Order="10" Type="ClaimsExchange">
  <ClaimsExchanges>
    <ClaimsExchange Id="TrackSignInComplete" TechnicalProfileReferenceId="AppInsights-SignInComplete" />
  </ClaimsExchanges>
</OrchestrationStep>
```

> [!IMPORTANT]
> Yeni düzenleme adımlarını ekledikten sonra, 1 ile N arasında bir tamsayı atlamadan adımları ardışık olarak yeniden numaralandırın.


## <a name="upload-your-file-run-the-policy-and-view-events"></a>Dosyanızı karşıya yükleyin, ilkeyi çalıştırın ve olayları görüntüleyin

*TrustFrameworkExtensions.xml* dosyasını kaydedin ve karşıya yükleyin. Ardından, bağlı olan taraf ilkesini uygulamanızdan çağırın veya Azure portal **Şimdi Çalıştır** ' ı kullanın. Saniyeler içinde, olaylarınız Application Insights kullanılabilir.

1. Azure Active Directory kiracınızda **Application Insights** kaynağını açın.
2. **Kullanım**  >  **olaylarını**seçin.
3. **Son saate** **ve** **3 dakikaya** **kadar ayarlayın.**  Sonuçları görüntülemek için **Yenile** ' yi seçmeniz gerekebilir.

![Application Insights kullanımı-etkinlik öncesi](./media/analytics-with-application-insights/app-ins-graphic.png)

## <a name="optional-collect-more-data"></a>Seçim Daha fazla veri toplayın

Gereksinimlerinize uyacak şekilde Kullanıcı yolculuğuna talep türleri ve olaylar ekleyin. [Talep çözümleyicilerine](claim-resolver-overview.md) veya herhangi bir dize talep türüne, Application Insights olayına bir **giriş talep** öğesi ekleyerek veya appınsights-Common Technical profile ' i kullanarak talepler ekleyebilirsiniz.

- **ClaimTypeReferenceId** , bir talep türüne başvurudur.
- **Partnerclaimtype** , Azure Insights 'ta görünen özelliğin adıdır. Öğesinin sözdizimini kullanın; `{property:NAME}` burada, `NAME` olaya bir özellik eklenir.
- **DefaultValue** herhangi bir dize değeri veya talep çözümleyici kullanır.

```xml
<InputClaim ClaimTypeReferenceId="app_session" PartnerClaimType="{property:app_session}" DefaultValue="{OAUTH-KV:app_session}" />
<InputClaim ClaimTypeReferenceId="loyalty_number" PartnerClaimType="{property:loyalty_number}" DefaultValue="{OAUTH-KV:loyalty_number}" />
<InputClaim ClaimTypeReferenceId="language" PartnerClaimType="{property:language}" DefaultValue="{Culture:RFC5646}" />
```

## <a name="next-steps"></a>Sonraki adımlar

- IEF başvurusunda [Application Insights](application-insights-technical-profile.md) teknik profil hakkında daha fazla bilgi edinin. 
