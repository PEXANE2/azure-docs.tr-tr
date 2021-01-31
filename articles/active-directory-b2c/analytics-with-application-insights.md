---
title: Application Insights ile Kullanıcı davranışını izleme
titleSuffix: Azure AD B2C
description: Application Insights Azure AD B2C Kullanıcı ilerinizden olay günlüklerini nasıl etkinleştirebileceğinizi öğrenin.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.topic: how-to
ms.workload: identity
ms.date: 01/29/2021
ms.author: mimart
ms.subservice: B2C
zone_pivot_groups: b2c-policy-type
ms.openlocfilehash: ce80e3376482ef44b466757cf7e345c4bcf186ad
ms.sourcegitcommit: 54e1d4cdff28c2fd88eca949c2190da1b09dca91
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2021
ms.locfileid: "99218562"
---
# <a name="track-user-behavior-in-azure-active-directory-b2c-using-application-insights"></a>Application Insights kullanarak Azure Active Directory B2C Kullanıcı davranışını izleme

[!INCLUDE [active-directory-b2c-choose-user-flow-or-custom-policy](../../includes/active-directory-b2c-choose-user-flow-or-custom-policy.md)]

::: zone pivot="b2c-user-flow"

[!INCLUDE [active-directory-b2c-limited-to-custom-policy](../../includes/active-directory-b2c-limited-to-custom-policy.md)]

::: zone-end

::: zone pivot="b2c-custom-policy"

Azure Active Directory B2C (Azure AD B2C), Azure AD B2C için sunulan izleme anahtarını kullanarak doğrudan olay verilerini [Application Insights](../azure-monitor/app/app-insights-overview.md) göndermeyi destekler. Application Insights teknik bir profille, Kullanıcı yolculukları için ayrıntılı ve özelleştirilmiş olay günlükleri edinebilirsiniz:

* Kullanıcı davranışında Öngörüler elde edin.
* Geliştirme veya üretimde kendi ilkelerinizin sorunlarını giderin.
* Ölçüm performansı.
* Application Insights bildirimler oluşturun.

## <a name="overview"></a>Genel Bakış

Özel olay günlüklerini etkinleştirmek için bir Application Insights teknik profili eklersiniz. Teknik profilde, Application Insights izleme anahtarını, olay adını ve kaydedilecek talepleri tanımlarsınız. Bir olay göndermek için, teknik profil [Kullanıcı yolculuğunda](userjourneys.md)bir düzenleme adımı olarak eklenir.

Application Insights kullanırken, aşağıdakileri göz önünde bulundurun:

- Application Insights yeni günlüklerin kullanılabilmesi için genellikle beş dakikadan kısa bir gecikme olur.
- Azure AD B2C, kaydedilecek talepleri seçmenizi sağlar. Kişisel verilerle talepler eklemeyin.
- Bir Kullanıcı oturumu kaydetmek için olaylar bir bağıntı KIMLIĞI kullanılarak birleştirilmiş olabilir. 
- Application Insights teknik profilini doğrudan bir [Kullanıcı yolculuğu](userjourneys.md) veya bir [alt gündekden](subjourneys.md)çağırın. Application Insights teknik profilini [doğrulama teknik profili](validation-technical-profile.md)olarak kullanmayın.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [active-directory-b2c-customization-prerequisites-custom-policy](../../includes/active-directory-b2c-customization-prerequisites-custom-policy.md)]

## <a name="create-an-application-insights-resource"></a>Application Insights kaynağı oluşturma

Azure AD B2C Application Insights kullanırken, tek yapmanız gereken bir kaynak oluşturur ve izleme anahtarını alır. Bilgi için bkz. [Application Insights kaynak oluşturma](../azure-monitor/app/create-new-resource.md)

1. [Azure portalında](https://portal.azure.com/) oturum açın.
2. Üst menüdeki **Dizin + abonelik** filtresini seçip aboneliğinizi içeren dizini seçerek Azure aboneliğinizi içeren dizini kullandığınızdan emin olun. Bu kiracı Azure AD B2C kiracınız değil.
3. Azure portal, sol üst köşedeki **kaynak oluştur** ' u seçin ve **Application Insights** arayıp seçin.
4. **Oluştur**’a tıklayın.
5. Kaynak için bir **ad** girin.
6. **Uygulama türü** için **ASP.NET Web uygulaması**' nı seçin.
7. **Kaynak grubu** için mevcut bir grubu seçin veya yeni bir grup için bir ad girin.
8. **Oluştur**’a tıklayın.
4. Application Insights kaynağını oluşturduktan sonra açın, **temel** bileşenler ' i genişletin ve izleme anahtarını kopyalayın.

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

Teknik profiller, özel ilkedeki işlevleri kabul edebilir. Bu tabloda, bir oturumu açmak ve olayları göndermek için kullanılan teknik profiller tanımlanmaktadır. Çözüm, [Teknik profil ekleme](technicalprofiles.md#include-technical-profile) yaklaşımını kullanır. Teknik bir profil, ayarları değiştirmek veya yeni işlevsellik eklemek için başka bir teknik profil içerir.

| Teknik profil | Görev |
| ----------------- | -----|
| AppInsights-Common | Ortak yapılandırma kümesiyle ortak teknik profil. Application Insights izleme anahtarı, kaydedilecek talepler koleksiyonu ve geliştirici modu dahildir. Aşağıdaki teknik profiller ortak teknik profili içerir ve olay adı gibi daha fazla talep ekler. |
| AppInsights-SignInRequest | Bir `SignInRequest` oturum açma isteği alındığında bir talep kümesiyle olay kaydeder. |
| AppInsights-UserSignUp | `UserSignUp`Kullanıcı kaydolma/oturum açma yolculuğunda oturum açma seçeneğini tetiklediğinde bir olayı kaydeder. |
| AppInsights-SignInComplete | `SignInComplete`Bağlı olan taraf uygulamasına bir belirteç gönderildiğinde bir kimlik doğrulamasının başarıyla tamamlanmasında bir olay kaydeder. |

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
        <InputClaim ClaimTypeReferenceId="tenantId" PartnerClaimType="{property:TenantId}" DefaultValue="{Policy:TrustFrameworkTenantId}" />
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
        <InputClaim ClaimTypeReferenceId="identityProvider" PartnerClaimType="{property:IDP}" DefaultValue="Local" />
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

 `SendClaims` Düzenleme adımından hemen önce, çağıran yeni bir adım ekleyin `AppInsights-UserSignup` . Kullanıcı kaydolma/oturum açma yolculuğunda oturum açma düğmesini seçtiğinde tetiklenir.

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

*TrustFrameworkExtensions.xml* dosyasını kaydedin ve karşıya yükleyin. Ardından, bağlı olan taraf ilkesini uygulamanızdan çağırın veya Azure portal **Şimdi Çalıştır** ' ı kullanın. Bir dakika bekleyin ve olaylarınız Application Insights kullanılabilir.

1. Azure Active Directory kiracınızda **Application Insights** kaynağını açın.
2. **Kullanım**' i seçin ve ardından **Olaylar**' ı seçin.
3. **Son saate** **ve** **3 dakikaya** **kadar ayarlayın.**  Sonuçları görüntülemek için **Yenile** ' yi seçmeniz gerekebilir.

![Application Insights USAGE-Events Blağı](./media/analytics-with-application-insights/app-ins-graphic.png)

## <a name="collect-more-data"></a>Daha fazla veri toplayın

İşletmenizin ihtiyaçlarını karşılamak için daha fazla talep kaydetmek isteyebilirsiniz. Talep eklemek için, önce [bir talep tanımlayın](#define-claims), sonra talep giriş talep koleksiyonuna ekleyin. *Appınsights-ortak* teknik profiline eklediğiniz talepler tüm olaylarda görünür. Belirli bir teknik profile eklediğiniz talepler yalnızca o olayda görünür. Giriş talep öğesi aşağıdaki öznitelikleri içerir:

- **ClaimTypeReferenceId** -bir talep türüne başvurudur. 
- **Partnerclaimtype** -Azure Insights 'ta görünen özelliğin adıdır. Öğesinin sözdizimini kullanın; `{property:NAME}` burada, `NAME` olaya bir özellik eklenir.
- **DefaultValue** -olay adı gibi, kaydedilecek önceden tanımlanmış bir değer. Kimlik sağlayıcısı adı gibi Kullanıcı yolculuğunda kullanılan bir talep. Talep boşsa, varsayılan değer kullanılacaktır. Örneğin, `identityProvider` talep Facebook gibi Federasyon teknik profilleri tarafından ayarlanır. Talep boşsa, kullanıcının bir yerel hesapla oturum açmasını gösterir. Bu nedenle, varsayılan değer *Yerel* olarak ayarlanır. Ayrıca, uygulama KIMLIĞI veya Kullanıcı IP adresi gibi bağlamsal bir değere sahip bir [talep çözümleyicilerine](claim-resolver-overview.md) de kaydedebilirsiniz.

### <a name="manipulating-claims"></a>Talepleri düzenleme

Giriş taleplerini değiştirmek veya Application Insights göndermeden önce yeni bir tane oluşturmak için [giriş talep dönüşümlerini](custom-policy-trust-frameworks.md#manipulating-your-claims) kullanabilirsiniz. Aşağıdaki örnekte, teknik profil *Checkısadmin* giriş talepleri dönüşümünü içerir. 

```xml
<TechnicalProfile Id="AppInsights-SignInComplete">
  <InputClaimsTransformations>  
    <InputClaimsTransformation ReferenceId="CheckIsAdmin" />
  </InputClaimsTransformations>
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="isAdmin" PartnerClaimType="{property:IsAdmin}"  />
    ...
  </InputClaims>
  <IncludeTechnicalProfile ReferenceId="AppInsights-Common" />
</TechnicalProfile>
```

### <a name="add-events"></a>Olay Ekle

Bir olay eklemek için *Appınsights-ortak* teknik profilini içeren yeni bir teknik profil oluşturun. Ardından [Kullanıcı yolculuğuna](custom-policy-trust-frameworks.md#orchestration-steps)düzenleme adımı olarak teknik profilini ekleyin. İstediğiniz zaman olayı tetiklemek için [önkoşulu](userjourneys.md#preconditions) kullanın. Örneğin, yalnızca kullanıcılar MFA aracılığıyla çalıştırıldığında olayı rapor edin.

```xml
<TechnicalProfile Id="AppInsights-MFA-Completed">
  <InputClaims>
     <InputClaim ClaimTypeReferenceId="EventType" PartnerClaimType="eventName" DefaultValue="MFA-Completed" />
  </InputClaims>
  <IncludeTechnicalProfile ReferenceId="AppInsights-Common" />
</TechnicalProfile>
```

Artık teknik bir profiliniz olduğuna göre, olayı Kullanıcı yolculuğuna ekleyin. Ardından, 1 ile N arasında bir tamsayı atlamadan adımları ardışık olarak yeniden numaralandırın.

```xml
<OrchestrationStep Order="8" Type="ClaimsExchange">
  <Precondition Type="ClaimsExist" ExecuteActionsIf="true">
    <Value>isActiveMFASession</Value>
    <Action>SkipThisOrchestrationStep</Action>
    </Precondition>
  </Preconditions>
  <ClaimsExchanges>
    <ClaimsExchange Id="TrackUserMfaCompleted" TechnicalProfileReferenceId="AppInsights-MFA-Completed" />
  </ClaimsExchanges>
</OrchestrationStep>
```

## <a name="enable-developer-mode"></a>Geliştirici modunu etkinleştir

Olayları tanımlamak için Application Insights kullanırken, geliştirici modunun etkin olup olmadığını belirtebilirsiniz. Geliştirici modu olayların nasıl arabelleğe alınacağını denetler. En az olay hacmi olan bir geliştirme ortamında, geliştirici modunun etkinleştirilmesi, Application Insights ' a anında gönderilen olaylarda sonuçlanır. `false` varsayılan değerdir. Üretim ortamlarında Geliştirici modunu etkinleştirmeyin.

Geliştirici modunu etkinleştirmek için *Appınsights-ortak* teknik profilinde `DeveloperMode` meta verileri şu şekilde değiştirin `true` : 

```xml
<TechnicalProfile Id="AppInsights-Common">
  <Metadata>
    ...
    <Item Key="DeveloperMode">true</Item>
  </Metadata>
</TechnicalProfile>
```

## <a name="disable-telemetry"></a>Telemetrisi devre dışı bırak

Uygulama Insight günlüklerini devre dışı bırakmak için *Appınsights-ortak* teknik profilinde `DisableTelemetry` meta verileri şu şekilde değiştirin `true` : 

```xml
<TechnicalProfile Id="AppInsights-Common">
  <Metadata>
    ...
    <Item Key="DisableTelemetry">true</Item>
  </Metadata>
</TechnicalProfile>
```

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Application Insights kullanarak özel KPI panoları oluşturmayı](../azure-monitor/learn/tutorial-app-dashboards.md)öğrenin. 

::: zone-end