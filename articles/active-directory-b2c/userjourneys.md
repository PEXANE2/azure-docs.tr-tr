---
title: Kullanıcı neys | Microsoft Docs
description: Azure Active Directory B2C bir özel ilkenin User, neys öğesini belirtin.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 03/04/2021
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 05307fe2ad9e0a59fa11c30f2dc7154ba5076603
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102174674"
---
# <a name="userjourneys"></a>UserJourneys

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Kullanıcı yolculukları, bağlı olan taraf uygulamasının kullanıcı için istenen talepleri elde etmesine izin veren bir ilkenin kullandığı belirgin yolları belirtir. Kullanıcı, bağlı olan tarafa sunulacak talepleri almak için bu yolları izler. Diğer bir deyişle, Kullanıcı yolculukları, Azure AD B2C kimlik deneyimi çerçevesi, isteği işleyen bir son kullanıcının iş mantığını tanımlar.

Bu Kullanıcı ilgisi, ilgili topluluğun çeşitli bağlı tarafları için temel gereksinimi karşılamak üzere kullanılabilir şablonlar olarak düşünülebilir. Kullanıcı, bir ilkenin bağlı olan taraf bölümünün tanımını kolaylaştırır. Bir ilke, birden çok kullanıcı bağlantısı tanımlayabilir. Her Kullanıcı yolculuğu bir düzenleme adımları dizisidir.

İlke tarafından desteklenen kullanıcıyı tanımlamak için, ilke dosyasının en üst düzey öğesinin altına bir **userbir neys** öğesi eklenir.

**Userınewys** öğesi aşağıdaki öğeyi içerir:

| Öğe | Öğeleri | Description |
| ------- | ----------- | ----------- |
| Kullanıcı yolculuğu | 1: n | Tüm Kullanıcı akışı için gereken tüm yapıları tanımlayan bir Kullanıcı yolculuğu. |

**Useryolculuney** öğesi aşağıdaki özniteliği içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| Id | Yes | İlkedeki diğer öğelerden başvurmak için kullanılabilen bir Kullanıcı yolculuğunun tanımlayıcısı. [Bağlı olan taraf Ilkesinin](relyingparty.md) **Defaultuseryolculuney** öğesi bu özniteliğe işaret eder. |

**Useryolculuney** öğesi aşağıdaki öğeleri içerir:

| Öğe | Öğeleri | Description |
| ------- | ----------- | ----------- |
| Authorizationcealprofiles | 0:1 | Yetkilendirme teknik profillerinin listesi. | 
| OrchestrationSteps | 1: n | Başarılı bir işlem için izlenmesi gereken bir düzenleme sırası. Her Kullanıcı yolculuğu, sırayla yürütülen bir düzenleme adımları sıralı listesinden oluşur. Herhangi bir adım başarısız olursa, işlem başarısız olur. |

## <a name="authorizationtechnicalprofiles"></a>Authorizationcealprofiles

Bir kullanıcının bir Kullanıcı yolculuğunu tamamladığını ve bir erişim ya da KIMLIK belirteci edindiğini varsayalım. [UserInfo uç noktası](userinfo-endpoint.md)gibi ek kaynakları yönetmek için, kullanıcının tanımlanması gerekir. Bu işlemi başlatmak için, kullanıcının ilk olarak geçerli bir Azure AD B2C ilkesi tarafından doğrulandığını kanıtlayarak, daha önce verilen erişim belirtecini sağlaması gerekir. Kullanıcının bu isteği yapmasına izin verildiğinden emin olmak için bu işlem sırasında Kullanıcı için geçerli bir belirteç bulunmalıdır. Yetkilendirme teknik profilleri, gelen belirteci doğrular ve belirteçteki talepleri ayıklar.

**Authorizationteknisyen** öğesi aşağıdaki öğeyi içerir:

| Öğe | Öğeleri | Description |
| ------- | ----------- | ----------- |
| Authorizationteknisyen Alprofıle | 0:1 | Yetkilendirme teknik profillerinin listesi. | 

**Authorizationcealprofile** öğesi aşağıdaki özniteliği içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| TechnicalProfileReferenceId | Yes | Yürütülecek teknik profilin tanımlayıcısı. |

Aşağıdaki örnekte, yetkilendirme teknik profillerine sahip bir Kullanıcı yolculuğu öğesi gösterilmektedir:

```xml
<UserJourney Id="UserInfoJourney" DefaultCpimIssuerTechnicalProfileReferenceId="UserInfoIssuer">
  <Authorization>
    <AuthorizationTechnicalProfiles>
      <AuthorizationTechnicalProfile ReferenceId="UserInfoAuthorization" />
    </AuthorizationTechnicalProfiles>
  </Authorization>
  <OrchestrationSteps>
    <OrchestrationStep Order="1" Type="ClaimsExchange">
     ...
```

## <a name="orchestrationsteps"></a>OrchestrationSteps

Kullanıcı yolculuğu, başarılı bir işlem için izlenmesi gereken bir düzenleme sırası olarak temsil edilir. Herhangi bir adım başarısız olursa, işlem başarısız olur. Bu düzenleme adımlarında, ilke dosyasında izin verilen yapı taşları ve talep sağlayıcılarının her ikisi de başvuru yapılır. Bir kullanıcı deneyimini gösterme veya işleme sorumlu olan herhangi bir düzenleme adımının karşılık gelen içerik tanımı tanımlayıcısına de bir başvurusu vardır.

Orchestration adımları, Orchestration Step öğesinde tanımlanan önkoşullara göre koşullu olarak çalıştırılabilir. Örneğin, yalnızca belirli bir talep varsa veya bir talebin belirtilen değere eşit veya daha fazla olması halinde bir düzenleme adımı gerçekleştirmeyi denetleyebilirsiniz.

Düzenleme adımlarının sıralı listesini belirtmek için, ilkenin bir parçası olarak bir **Orchestrationsteps** öğesi eklenir. Bu öğe gereklidir.

**Orchestrationsteps** öğesi aşağıdaki öğeyi içerir:

| Öğe | Öğeleri | Description |
| ------- | ----------- | ----------- |
| OrchestrationStep | 1: n | Sıralı düzenleme adımı. |

**Orchestrationstep** öğesi aşağıdaki öznitelikleri içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| `Order` | Evet | Düzenleme adımlarının sırası. |
| `Type` | Yes | Düzenleme adımının türü. Olası değerler: <ul><li>**Claimsproviderselection** -Orchestration adımının, kullanıcıya bir tane seçmek üzere çeşitli talep sağlayıcıları sunmadığını gösterir.</li><li>**CombinedSignInAndSignUp** -düzenleme adımının birleştirilmiş bir sosyal sağlayıcı oturum açma ve yerel hesap kaydolma sayfası sunmadığını gösterir.</li><li>**Claimsexchange** -Orchestration adımının bir talep sağlayıcısıyla talepler olduğunu gösterir.</li><li>**Getclaim** -düzenleme adımının, bağlı olan tarafın yapılandırması aracılığıyla Azure AD B2C gönderilen talep verilerini işlemesi gerektiğini belirtir `InputClaims` .</li><li>**InvokeSubJourney** -düzenleme adımının, bir [alt yolculuğa](subjourneys.md) sahip talepler (genel önizlemede) olduğunu gösterir.</li><li>**Sendclaim** -düzenleme adımının talepleri, talep veren tarafından verilen bir belirteç ile bağlı olan tarafa göndereceğini gösterir.</li></ul> |
| Contentdefinitionreferenceıd | No | Bu düzenleme adımı ile ilişkili [içerik tanımının](contentdefinitions.md) tanımlayıcısı. Genellikle içerik tanımı başvuru tanımlayıcısı, kendi kendine onaylanan teknik profilde tanımlanmıştır. Ancak, Azure AD B2C teknik bir profil olmadan bir şeyi görüntülemesi gerektiğinde bazı durumlar vardır. İki örnek vardır: düzenleme adımının türü aşağıdakilerden biri ise, `ClaimsProviderSelection`  `CombinedSignInAndSignUp` Azure AD B2C teknik bir profil olmadan kimlik sağlayıcısı seçimini görüntülemesi gerekir. |
| CpimIssuerTechnicalProfileReferenceId | No | Orchestration adımının türü `SendClaims` . Bu özellik, bağlı olan taraf için belirteci veren talep sağlayıcısının teknik profil tanımlayıcısını tanımlar.  Yoksa, bağlı olan taraf belirteci oluşturulmaz. |

**Orchestrationstep** öğesi aşağıdaki öğeleri içerebilir:

| Öğe | Öğeleri | Description |
| ------- | ----------- | ----------- |
| Üstbilgisinde | 0: n | Düzenleme adımının yürütülmesi için karşılanması gereken önkoşulların bir listesi. |
| Claimsproviderseçimleri | 0: n | Düzenleme adımı için bir talep sağlayıcı seçimleri listesi. |
| ClaimsExchanges | 0: n | Düzenleme adımı için talep değişimlerinin listesi. |
| Bağlantı listesi | 0:1 | Düzenleme adımının alt yolculuğu adayların listesi. |

### <a name="preconditions"></a>Üstbilgisinde

Ön **koşullar** öğesi aşağıdaki öğeyi içerir:

| Öğe | Öğeleri | Description |
| ------- | ----------- | ----------- |
| Ön koşul | 1: n | Kullanılan teknik profile bağlı olarak, istemciyi talep sağlayıcısı seçimine göre yeniden yönlendirir veya Exchange taleplerini bir sunucu çağrısı yapar. |


#### <a name="precondition"></a>Ön koşul

Düzenleme adımları düzenleme adımında tanımlanan önkoşullara göre koşullu olarak çalıştırılabilir. İki tür ön koşullar vardır:
 
- **Talepler var** -belirtilen talepler kullanıcının geçerli talep paketinde mevcutsa eylemlerin gerçekleştirilmesi gerektiğini belirtir.
- **Talep eşittir** -belirtilen talep varsa ve değeri belirtilen değere eşitse eylemlerin gerçekleştirilmesi gerektiğini belirtir. Denetim, büyük/küçük harfe duyarlı bir sıra karşılaştırması gerçekleştirir. Boole talep türünü denetlerken, veya kullanın `True` `False` .

**Önkoşul** öğesi aşağıdaki öznitelikleri içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| `Type` | Evet | Bu ön koşul için gerçekleştirilecek denetim veya sorgu türü. Bu değer, belirtilen talepler varsa ve bu değerin belirtilen değere eşit olması halinde, eylemlerin gerçekleştirilmesi gerektiğini belirten, eylemlerin, kullanıcının geçerli talep kümesinde bulunması veya **Claımequals** olması gerektiğini belirten **claimsexist** olabilir. |
| `ExecuteActionsIf` | Yes | `true` `false` Önkoşulun içindeki eylemlerin gerçekleştirilip gerçekleştirilmeyeceğine karar vermek için bir veya test kullanın. |

**Önkoşul** öğeleri aşağıdaki öğeleri içerir:

| Öğe | Öğeleri | Description |
| ------- | ----------- | ----------- |
| Değer | 1:2 | Bir talep türünün tanımlayıcısı. Talep, ilke dosyası veya üst ilke dosyasındaki talep şeması bölümünde zaten tanımlanmış. Önkoşul türü olduğunda `ClaimEquals` , ikinci `Value` öğe denetlenecek değeri içerir. |
| Eylem | 1:1 | Bir düzenleme adımı içinde önkoşul denetimi doğru ise gerçekleştirilmesi gereken eylem. Öğesinin değeri `Action` olarak ayarlandıysa `SkipThisOrchestrationStep` , ilişkili öğesinin `OrchestrationStep` yürütülmemelidir. |

#### <a name="preconditions-examples"></a>Ön koşullar örnekleri

Aşağıdaki Önkoşullar, kullanıcının ObjectID 'nin var olup olmadığını denetler. Kullanıcı yolculuğunda Kullanıcı, yerel hesabı kullanarak oturum açmayı seçti. ObjectID varsa, bu düzenleme adımını atlayın.

```xml
<OrchestrationStep Order="2" Type="ClaimsExchange">
  <Preconditions>
    <Precondition Type="ClaimsExist" ExecuteActionsIf="true">
      <Value>objectId</Value>
      <Action>SkipThisOrchestrationStep</Action>
    </Precondition>
  </Preconditions>
  <ClaimsExchanges>
    <ClaimsExchange Id="FacebookExchange" TechnicalProfileReferenceId="Facebook-OAUTH" />
    <ClaimsExchange Id="SignUpWithLogonEmailExchange" TechnicalProfileReferenceId="LocalAccountSignUpWithLogonEmail" />
  </ClaimsExchanges>
</OrchestrationStep>
```

Aşağıdaki ön koşullar kullanıcının bir sosyal hesapla oturum açmış olup olmadığını denetler. Dizinde Kullanıcı hesabını bulmak için bir girişimde bulunuldu. Kullanıcı oturum açtığında veya yerel bir hesapla kaydolduğunda, bu düzenleme adımını atlayın.

```xml
<OrchestrationStep Order="3" Type="ClaimsExchange">
  <Preconditions>
    <Precondition Type="ClaimEquals" ExecuteActionsIf="true">
      <Value>authenticationSource</Value>
      <Value>localAccountAuthentication</Value>
      <Action>SkipThisOrchestrationStep</Action>
    </Precondition>
  </Preconditions>
  <ClaimsExchanges>
    <ClaimsExchange Id="AADUserReadUsingAlternativeSecurityId" TechnicalProfileReferenceId="AAD-UserReadUsingAlternativeSecurityId-NoError" />
  </ClaimsExchanges>
</OrchestrationStep>
```

Ön koşullar birden çok önkoşulları denetleyebilir. Aşağıdaki örnek ' ObjectID ' veya ' email ' olup olmadığını denetler. İlk koşul doğru ise, yolculuğa bir sonraki düzenleme adımına atlar.

```xml
<OrchestrationStep Order="4" Type="ClaimsExchange">
  <Preconditions>
    <Precondition Type="ClaimsExist" ExecuteActionsIf="true">
      <Value>objectId</Value>
      <Action>SkipThisOrchestrationStep</Action>
    </Precondition>
    <Precondition Type="ClaimsExist" ExecuteActionsIf="true">
      <Value>email</Value>
      <Action>SkipThisOrchestrationStep</Action>
    </Precondition>
  </Preconditions>
  <ClaimsExchanges>
    <ClaimsExchange Id="SelfAsserted-SocialEmail" TechnicalProfileReferenceId="SelfAsserted-SocialEmail" />
  </ClaimsExchanges>
</OrchestrationStep>
```

## <a name="claims-provider-selection"></a>Talep sağlayıcı seçimi

Kimlik sağlayıcısı seçimi, kullanıcıların bir seçenek listesinden bir eylem seçmesini sağlar. Kimlik sağlayıcısı seçimi iki düzenleme adımından oluşur: 

1. **Düğmeler** -bu, türüyle başlar `ClaimsProviderSelection` veya `CombinedSignInAndSignUp` bir kullanıcının aralarından seçim yapabileceğiniz seçeneklerin bir listesini içerir. Öğesi içindeki seçeneklerin sırası, `ClaimsProviderSelections` kullanıcıya sunulan düğmelerin sırasını denetler.
2. **Eylemler** -öğesinden sonra türü `ClaimsExchange` . ClaimsExchange, eylemlerin listesini içerir. Eylem, [OAuth2](oauth2-technical-profile.md), [OpenID Connect](openid-connect-technical-profile.md), [talep dönüştürme](claims-transformation-technical-profile.md)veya [kendiliğinden onaylanan](self-asserted-technical-profile.md)gibi bir teknik profile başvurudur. Bir Kullanıcı düğmelerden birine tıkladığında ilgili eylem yürütülür.

**Claimsproviderseçimlere** öğesi aşağıdaki öğeyi içerir:

| Öğe | Öğeleri | Description |
| ------- | ----------- | ----------- |
| ClaimsProviderSelection | 1: n | Seçilebilirler talep sağlayıcılarının listesini sağlar.|

**Claimsproviderseçimlere** öğesi aşağıdaki öznitelikleri içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| DisplayOption| No | Tek bir talep sağlayıcı seçiminin kullanılabildiği bir durumun davranışını denetler. Olası değerler: `DoNotShowSingleProvider` (varsayılan), kullanıcı federe kimlik sağlayıcısına anında yönlendirilir. Veya `ShowSingleProvider` Azure AD B2C, oturum açma sayfasını tek kimlik sağlayıcı seçimiyle gösterir. Bu özniteliği kullanmak için, [İçerik tanımı sürümü](page-layout.md) `urn:com:microsoft:aad:b2c:elements:contract:providerselection:1.0.0` ve üzeri olmalıdır.|

**Claimsproviderselection** öğesi aşağıdaki öznitelikleri içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| Targetclaimsexchangeıd | No | Talep sağlayıcısı seçiminin sonraki düzenleme adımında yürütülen talep değişim tanıtıcısı. Bu öznitelik veya Validationclaimsexchangeıd özniteliği belirtilmelidir, ancak her ikisi birden belirtilmemelidir. |
| Validationclaimsexchangeıd | No | Talep sağlayıcı seçimini doğrulamak için geçerli düzenleme adımında yürütülen talep alışverişi tanıtıcısı. Bu öznitelik veya Targetclaimsexchangeıd özniteliği belirtilmelidir, ancak her ikisi birden belirtilmemelidir. |

### <a name="claims-provider-selection-example"></a>Talep sağlayıcı seçimi örneği

Aşağıdaki düzenleme adımında, Kullanıcı Facebook, LinkedIn, Twitter, Google veya yerel bir hesap ile oturum açmayı tercih edebilir. Kullanıcı sosyal kimlik sağlayıcılarından birini seçerse ikinci düzenleme adımı özniteliğinde belirtilen seçili talep değiş tokuşu ile yürütülür `TargetClaimsExchangeId` . İkinci düzenleme adımı, oturum açma işlemini gerçekleştirmek için kullanıcıyı sosyal kimlik sağlayıcısına yönlendirir. Kullanıcı yerel hesapla oturum açmayı seçerse Azure AD B2C aynı düzenleme adımında kalır (aynı kayıt sayfası veya oturum açma sayfası) ve ikinci düzenleme adımını atlar.

```xml
<OrchestrationStep Order="1" Type="CombinedSignInAndSignUp" ContentDefinitionReferenceId="api.signuporsignin">
  <ClaimsProviderSelections>
    <ClaimsProviderSelection TargetClaimsExchangeId="FacebookExchange" />
    <ClaimsProviderSelection TargetClaimsExchangeId="LinkedInExchange" />
    <ClaimsProviderSelection TargetClaimsExchangeId="TwitterExchange" />
    <ClaimsProviderSelection TargetClaimsExchangeId="GoogleExchange" />
    <ClaimsProviderSelection ValidationClaimsExchangeId="LocalAccountSigninEmailExchange" />
  </ClaimsProviderSelections>
  <ClaimsExchanges>
  <ClaimsExchange Id="LocalAccountSigninEmailExchange"
        TechnicalProfileReferenceId="SelfAsserted-LocalAccountSignin-Email" />
  </ClaimsExchanges>
</OrchestrationStep>


<OrchestrationStep Order="2" Type="ClaimsExchange">
  <Preconditions>
    <Precondition Type="ClaimsExist" ExecuteActionsIf="true">
      <Value>objectId</Value>
      <Action>SkipThisOrchestrationStep</Action>
    </Precondition>
  </Preconditions>
  <ClaimsExchanges>
    <ClaimsExchange Id="FacebookExchange" TechnicalProfileReferenceId="Facebook-OAUTH" />
    <ClaimsExchange Id="SignUpWithLogonEmailExchange" TechnicalProfileReferenceId="LocalAccountSignUpWithLogonEmail" />
    <ClaimsExchange Id="GoogleExchange" TechnicalProfileReferenceId="Google-OAUTH" />
    <ClaimsExchange Id="LinkedInExchange" TechnicalProfileReferenceId="LinkedIn-OAUTH" />
    <ClaimsExchange Id="TwitterExchange" TechnicalProfileReferenceId="Twitter-OAUTH1" />
  </ClaimsExchanges>
</OrchestrationStep>
```

## <a name="claimsexchanges"></a>ClaimsExchanges

**Claimsexchanges** öğesi aşağıdaki öğeyi içerir:

| Öğe | Öğeleri | Description |
| ------- | ----------- | ----------- |
| ClaimsExchange | 1: n | Kullanılan teknik profile bağlı olarak, istemciyi seçili olan ClaimsProviderSelection öğesine göre yeniden yönlendirir veya Exchange taleplerini bir sunucu çağrısı yapar. |

**Claimsexchange** öğesi aşağıdaki öznitelikleri içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| Id | Yes | Talep değişimi adımının tanıtıcısı. Tanımlayıcı, ilkedeki bir talep sağlayıcı seçimi adımından talep değişimine başvurmak için kullanılır. |
| TechnicalProfileReferenceId | Yes | Yürütülecek teknik profilin tanımlayıcısı. |

## <a name="journeylist"></a>Bağlantı listesi

Giden **Neylist** öğesi aşağıdaki öğeyi içerir:

| Öğe | Öğeleri | Description |
| ------- | ----------- | ----------- |
| Aday | 1:1 | Çağrılacak bir alt yolculuğa başvuru. |

### <a name="candidate"></a>Aday

**Aday** öğesi aşağıdaki öznitelikleri içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| Alt bağlantı Neyımreferenceıd | Yes | Yürütülecek [alt yolculuğun](subjourneys.md) tanımlayıcısı. |
