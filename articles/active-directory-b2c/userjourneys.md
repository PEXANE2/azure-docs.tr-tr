---
title: Kullanıcı neys | Microsoft Docs
description: Azure Active Directory B2C bir özel ilkenin User, neys öğesini belirtin.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 02/04/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: d73a1a3ce23817d9d6f742a4a8c730afb58ee0c8
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "78226996"
---
# <a name="userjourneys"></a>UserJourneys

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Kullanıcı yolculukları, bir ilkenin bir kullanıcı için istenen talepleri elde etmesine izin veren bir ilke aracılığıyla açık yollar belirtmektir. Kullanıcı, bağlı olan tarafa sunulacak talepleri almak için bu yollar üzerinden alınır. Diğer bir deyişle, Kullanıcı yolculukları, Azure AD B2C kimlik deneyimi çerçevesi, isteği işleyen bir son kullanıcının iş mantığını tanımlar.

Bu Kullanıcı ilgisi, ilgili topluluğun çeşitli bağlı tarafları için temel gereksinimi karşılamak üzere kullanılabilir şablonlar olarak düşünülebilir. Kullanıcı, bir ilkenin bağlı olan taraf bölümünün tanımını kolaylaştırır. Bir ilke, birden çok kullanıcı bağlantısı tanımlayabilir. Her Kullanıcı yolculuğu bir düzenleme adımları dizisidir.

İlke tarafından desteklenen kullanıcıyı tanımlamak için, ilke dosyasının en üst düzey öğesinin altına bir **userbir neys** öğesi eklenir.

**Userınewys** öğesi aşağıdaki öğeyi içerir:

| Öğe | Öğeleri | Açıklama |
| ------- | ----------- | ----------- |
| Kullanıcı yolculuğu | 1: n | Tüm Kullanıcı akışı için gereken tüm yapıları tanımlayan bir Kullanıcı yolculuğu. |

**Useryolculuney** öğesi aşağıdaki özniteliği içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| Kimlik | Yes | İlkedeki diğer öğelerden başvurmak için kullanılabilen bir Kullanıcı yolculuğunun tanımlayıcısı. [Bağlı olan taraf Ilkesinin](relyingparty.md) **Defaultuseryolculuney** öğesi bu özniteliğe işaret eder. |

**Useryolculuney** öğesi aşağıdaki öğeleri içerir:

| Öğe | Öğeleri | Açıklama |
| ------- | ----------- | ----------- |
| OrchestrationSteps | 1: n | Başarılı bir işlem için izlenmesi gereken bir düzenleme sırası. Her Kullanıcı yolculuğu, sırayla yürütülen bir düzenleme adımları sıralı listesinden oluşur. Herhangi bir adım başarısız olursa, işlem başarısız olur. |

## <a name="orchestrationsteps"></a>OrchestrationSteps

Kullanıcı yolculuğu, başarılı bir işlem için izlenmesi gereken bir düzenleme sırası olarak temsil edilir. Herhangi bir adım başarısız olursa, işlem başarısız olur. Bu düzenleme adımlarında, ilke dosyasında izin verilen yapı taşları ve talep sağlayıcılarının her ikisi de başvuru yapılır. Bir kullanıcı deneyimini gösterme veya işleme sorumlu olan herhangi bir düzenleme adımının karşılık gelen içerik tanımı tanımlayıcısına de bir başvurusu vardır.

Orchestration adımları, Orchestration Step öğesinde tanımlanan önkoşullara göre koşullu olarak çalıştırılabilir. Örneğin, yalnızca belirli talepler varsa veya bir talep belirtilen değere eşitse ya da buna eşit değilse, bir düzenleme adımı gerçekleştirmeyi seçebilirsiniz.

Düzenleme adımlarının sıralı listesini belirtmek için, ilkenin bir parçası olarak bir **Orchestrationsteps** öğesi eklenir. Bu öğe gereklidir.

**Orchestrationsteps** öğesi aşağıdaki öğeyi içerir:

| Öğe | Öğeleri | Açıklama |
| ------- | ----------- | ----------- |
| OrchestrationStep | 1: n | Sıralı düzenleme adımı. |

**Orchestrationstep** öğesi aşağıdaki öznitelikleri içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| `Order` | Yes | Düzenleme adımlarının sırası. |
| `Type` | Yes | Düzenleme adımının türü. Olası değerler: <ul><li>**Claimsproviderselection** -Orchestration adımının, kullanıcıya bir tane seçmek üzere çeşitli talep sağlayıcıları sunmadığını gösterir.</li><li>**CombinedSignInAndSignUp** -düzenleme adımının birleştirilmiş bir sosyal sağlayıcı oturum açma ve yerel hesap kaydolma sayfası sunmadığını gösterir.</li><li>**Claimsexchange** -Orchestration adımının bir talep sağlayıcısıyla talepler olduğunu gösterir.</li><li>**Getclaim** -düzenleme adımının giriş taleplerini okuduğunu gösterir.</li><li>**Sendclaim** -düzenleme adımının talepleri, talep veren tarafından verilen bir belirteç ile bağlı olan tarafa göndereceğini gösterir.</li></ul> |
| Contentdefinitionreferenceıd | Hayır | Bu düzenleme adımı ile ilişkili [içerik tanımının](contentdefinitions.md) tanımlayıcısı. Genellikle içerik tanımı başvuru tanımlayıcısı, kendi kendine onaylanan teknik profilde tanımlanmıştır. Ancak, Azure AD B2C teknik bir profil olmadan bir şeyi görüntülemesi gerektiğinde bazı durumlar vardır. İki örnek vardır: `ClaimsProviderSelection` `CombinedSignInAndSignUp`düzenleme adımının türü aşağıdakilerden biri ise, Azure AD B2C teknik bir profil olmadan kimlik sağlayıcısı seçimini görüntülemesi gerekir. |
| CpimIssuerTechnicalProfileReferenceId | Hayır | Orchestration adımının türü `SendClaims`. Bu özellik, bağlı olan taraf için belirteci veren talep sağlayıcısının teknik profil tanımlayıcısını tanımlar.  Yoksa, bağlı olan taraf belirteci oluşturulmaz. |


**Orchestrationstep** öğesi aşağıdaki öğeleri içerebilir:

| Öğe | Öğeleri | Açıklama |
| ------- | ----------- | ----------- |
| Üstbilgisinde | 0: n | Düzenleme adımının yürütülmesi için karşılanması gereken önkoşulların bir listesi. |
| Claimsproviderseçimleri | 0: n | Düzenleme adımı için bir talep sağlayıcı seçimleri listesi. |
| ClaimsExchanges | 0: n | Düzenleme adımı için talep değişimlerinin listesi. |

### <a name="preconditions"></a>Üstbilgisinde

Ön **koşullar** öğesi aşağıdaki öğeyi içerir:

| Öğe | Öğeleri | Açıklama |
| ------- | ----------- | ----------- |
| Ön koşul | 1: n | Kullanılan teknik profile bağlı olarak, istemciyi talep sağlayıcısı seçimine göre yeniden yönlendirir veya Exchange taleplerini bir sunucu çağrısı yapar. |


#### <a name="precondition"></a>Ön koşul

**Önkoşul** öğesi aşağıdaki öznitelikleri içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| `Type` | Yes | Bu ön koşul için gerçekleştirilecek denetim veya sorgu türü. Bu değer, belirtilen talepler varsa ve bu değerin belirtilen değere eşit olması halinde, eylemlerin gerçekleştirilmesi gerektiğini belirten, eylemlerin, kullanıcının geçerli talep kümesinde bulunması veya **Claımequals**olması gerektiğini belirten **claimsexist**olabilir. |
| `ExecuteActionsIf` | Yes | Önkoşuldaki eylemlerin gerçekleştirilip gerçekleştirilmeyeceğine karar vermek için doğru veya yanlış test kullanın. |

**Önkoşul** öğeleri aşağıdaki öğeleri içerir:

| Öğe | Öğeleri | Açıklama |
| ------- | ----------- | ----------- |
| Değer | 1: n | İçin sorgulanacak bir ClaimTypeReferenceId. Başka bir değer öğesi, denetlenecek değeri içerir.</li></ul>|
| Eylem | 1:1 | Bir düzenleme adımı içinde önkoşul denetimi doğru ise gerçekleştirilmesi gereken eylem. Öğesinin değeri olarak `Action` `SkipThisOrchestrationStep`ayarlandıysa, ilişkili `OrchestrationStep` öğesinin yürütülmemelidir. |

#### <a name="preconditions-examples"></a>Ön koşullar örnekleri

Aşağıdaki Önkoşullar, kullanıcının ObjectID 'nin var olup olmadığını denetler. Kullanıcı yolculuğunda Kullanıcı, yerel hesabı kullanarak oturum açmayı seçti. ObjectID varsa, bu düzenleme adımını atlayın.

```XML
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

```XML
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

```XML
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

## <a name="claimsproviderselection"></a>ClaimsProviderSelection

Türünde `ClaimsProviderSelection` bir düzenleme adımı veya `CombinedSignInAndSignUp` bir kullanıcının oturum açması için bir talep sağlayıcı listesi içerebilir. Öğelerin içindeki `ClaimsProviderSelections` öğelerin sırası, kullanıcıya sunulan kimlik sağlayıcılarının sırasını denetler.

**Claimsproviderseçimlere** öğesi aşağıdaki öğeyi içerir:

| Öğe | Öğeleri | Açıklama |
| ------- | ----------- | ----------- |
| ClaimsProviderSelection | 1: n | Seçilebilirler talep sağlayıcılarının listesini sağlar.|

**Claimsproviderseçimlere** öğesi aşağıdaki öznitelikleri içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| DisplayOption| Hayır | Tek bir talep sağlayıcı seçiminin kullanılabildiği bir durumun davranışını denetler. Olası değerler: `DoNotShowSingleProvider` (varsayılan), kullanıcı federe kimlik sağlayıcısına anında yönlendirilir. Veya `ShowSingleProvider` Azure AD B2C, oturum açma sayfasını tek kimlik sağlayıcı seçimiyle gösterir. Bu özniteliği kullanmak için, [İçerik tanımı sürümü](page-layout.md)  `urn:com:microsoft:aad:b2c:elements:contract:providerselection:1.0.0` ve üzeri olmalıdır.|

**Claimsproviderselection** öğesi aşağıdaki öznitelikleri içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| Targetclaimsexchangeıd | Hayır | Talep sağlayıcısı seçiminin sonraki düzenleme adımında yürütülen talep değişim tanıtıcısı. Bu öznitelik veya Validationclaimsexchangeıd özniteliği belirtilmelidir, ancak her ikisi birden belirtilmemelidir. |
| Validationclaimsexchangeıd | Hayır | Talep sağlayıcı seçimini doğrulamak için geçerli düzenleme adımında yürütülen talep alışverişi tanıtıcısı. Bu öznitelik veya Targetclaimsexchangeıd özniteliği belirtilmelidir, ancak her ikisi birden belirtilmemelidir. |

### <a name="claimsproviderselection-example"></a>ClaimsProviderSelection örneği

Aşağıdaki düzenleme adımında, Kullanıcı Facebook, LinkedIn, Twitter, Google veya yerel bir hesap ile oturum açmayı tercih edebilir. Kullanıcı sosyal kimlik sağlayıcılarından birini seçerse ikinci düzenleme adımı `TargetClaimsExchangeId` özniteliğinde belirtilen seçili talep değiş tokuşu ile yürütülür. İkinci düzenleme adımı, oturum açma işlemini gerçekleştirmek için kullanıcıyı sosyal kimlik sağlayıcısına yönlendirir. Kullanıcı yerel hesapla oturum açmayı seçerse Azure AD B2C aynı düzenleme adımında kalır (aynı kayıt sayfası veya oturum açma sayfası) ve ikinci düzenleme adımını atlar.

```XML
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

| Öğe | Öğeleri | Açıklama |
| ------- | ----------- | ----------- |
| ClaimsExchange | 1: n | Kullanılan teknik profile bağlı olarak, istemciyi seçili olan ClaimsProviderSelection öğesine göre yeniden yönlendirir veya Exchange taleplerini bir sunucu çağrısı yapar. |

**Claimsexchange** öğesi aşağıdaki öznitelikleri içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| Kimlik | Yes | Talep değişimi adımının tanıtıcısı. Tanımlayıcı, ilkedeki bir talep sağlayıcı seçimi adımından talep değişimine başvurmak için kullanılır. |
| TechnicalProfileReferenceId | Yes | Yürütülecek teknik profilin tanımlayıcısı. |
