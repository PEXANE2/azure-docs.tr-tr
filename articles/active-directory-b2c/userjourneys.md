---
title: UserJourneys | Microsoft Dokümanlar
description: Azure Active Directory B2C'de özel bir ilkenin UserJourneys öğesini belirtin.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78226996"
---
# <a name="userjourneys"></a>UserJourneys

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Kullanıcı yolculukları, bir ilkenin, bir kullanıcı için istenen talepleri elde etmesine güvenen bir taraf uygulamasının izin verdiği açık yolları belirtir. Kullanıcı, güvenen tarafa sunulacak talepleri almak için bu yollardan alınır. Başka bir deyişle, kullanıcı yolculukları, Azure AD B2C Kimlik Deneyimi Çerçevesi isteği işlerken son kullanıcının neler yaşadığının iş mantığını tanımlar.

Bu kullanıcı yolculukları, ilgi topluluğunun çeşitli güvenen taraflarının temel ihtiyacını karşılamak için kullanılabilir şablonlar olarak kabul edilebilir. Kullanıcı yolculukları, bir politikanın güvenilen taraf kısmının tanımlanmasını kolaylaştırır. Bir ilke, birden çok kullanıcı yolculuğunu tanımlayabilir. Her kullanıcı yolculuğu, orkestrasyon adımlarının bir sırasıdır.

İlke tarafından desteklenen kullanıcı yolculuklarını tanımlamak için, ilke dosyasının üst düzey öğesinin altına bir **UserJourneys** öğesi eklenir.

**UserJourneys** öğesi aşağıdaki öğeyi içerir:

| Öğe | Oluşum | Açıklama |
| ------- | ----------- | ----------- |
| UserJourney | 1:n | Tam bir kullanıcı akışı için gerekli tüm yapıları tanımlayan bir kullanıcı yolculuğu. |

**UserJourney** öğesi aşağıdaki özniteliği içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| Kimlik | Evet | İlkedeki diğer öğelerden başvurmak için kullanılabilecek bir kullanıcı yolculuğunun tanımlayıcısı. [Güvenilen parti ilkesinin](relyingparty.md) **DefaultUserJourney** öğesi bu özniteliğe işaret eder. |

**UserJourney** öğesi aşağıdaki öğeleri içerir:

| Öğe | Oluşum | Açıklama |
| ------- | ----------- | ----------- |
| OrkestrasyonAdımları | 1:n | Başarılı bir işlem için izlenmesi gereken bir orkestrasyon sırası. Her kullanıcı yolculuğu sırayla yürütülen düzenleme adımlarının sıralı bir listesinden oluşur. Herhangi bir adım başarısız olursa, işlem başarısız olur. |

## <a name="orchestrationsteps"></a>OrkestrasyonAdımları

Kullanıcı yolculuğu, başarılı bir işlem için izlenmesi gereken bir orkestrasyon dizisi olarak gösterilir. Herhangi bir adım başarısız olursa, işlem başarısız olur. Bu düzenleme adımları, hem yapı taşlarını hem de ilke dosyasında izin verilen talep sağlayıcılarını başvurur. Bir kullanıcı deneyimini göstermek veya işlemekten sorumlu olan herhangi bir düzenleme adımı, ilgili içerik tanımı tanımlayıcısına da başvurur.

Orkestrasyon adımları, orkestrasyon adımı öğesinde tanımlanan ön koşullara göre koşullu olarak yürütülebilir. Örneğin, yalnızca belirli bir talep varsa veya bir talep belirtilen değere eşit sayılsa da eşit sayılsa da bir düzenleme adımı gerçekleştirebilirsiniz.

Sıralı düzenleme adımları listesini belirtmek için, ilkenin bir parçası olarak bir **OrchestrationSteps** öğesi eklenir. Bu öğe gereklidir.

**OrchestrationSteps** öğesi aşağıdaki öğeyi içerir:

| Öğe | Oluşum | Açıklama |
| ------- | ----------- | ----------- |
| OrkestrasyonAdım | 1:n | Düzenli bir orkestrasyon adımı. |

**OrchestrationStep** öğesi aşağıdaki öznitelikleri içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| `Order` | Evet | Orkestrasyon adımlarının sırası. |
| `Type` | Evet | Orkestrasyon adımının türü. Olası değerler: <ul><li>**ClaimsProviderSelection** - Orkestrasyon adımının kullanıcıya birini seçmek için çeşitli talep sağlayıcılar sunduğunu gösterir.</li><li>**CombinedSignInAndSignUp** - Orkestrasyon adımının birleşik bir sosyal sağlayıcı oturum açma ve yerel hesap kayıt sayfası sunduğunu gösterir.</li><li>**ClaimsExchange** - Orkestrasyon adımının bir talep sağlayıcısıyla talep alışverişinde bulundurdığını gösterir.</li><li>**GetClaims** - Orkestrasyon adımının giriş taleplerini okuduğunu gösterir.</li><li>**SendClaims** - Orkestrasyon adımının talepleri veren tarafça verilen bir belirteçle ilgili tarafa gönderdiğini belirtir.</li></ul> |
| İçerikDefinitionReferenceId | Hayır | Bu düzenleme adımıyla ilişkili [içerik tanımının](contentdefinitions.md) tanımlayıcısı. Genellikle içerik tanımı referans tanımlayıcısı, kendi kendine ileri süren teknik profilde tanımlanır. Ancak, Azure AD B2C'nin teknik profili olmayan bir şeyi görüntülemesi gereken bazı durumlar vardır. İki örnek vardır : düzenleme adımının türü aşağıdakilerden `ClaimsProviderSelection` biriyse: veya `CombinedSignInAndSignUp`Azure AD B2C'nin kimlik sağlayıcı seçimini teknik bir profile sahip olmadan görüntülemesi gerekir. |
| CpimIssuerTechnicalProfileReferenceId | Hayır | Orkestrasyon adımının `SendClaims`türü . Bu özellik, güvenen taraf için belirteç veren talep sağlayıcının teknik profil tanımlayıcısını tanımlar.  Yoksa, güvenen taraf belirteci oluşturulmaz. |


**OrchestrationStep** öğesi aşağıdaki öğeleri içerebilir:

| Öğe | Oluşum | Açıklama |
| ------- | ----------- | ----------- |
| Önkoşulları | 0:n | Yürütme adımının yürütülmesi için yerine getirilmesi gereken ön koşulların listesi. |
| İddia Sağlayıcı seçimleri | 0:n | Düzenleme adımı için talep sağlayıcı seçimlerinin listesi. |
| İddialarBorsalar | 0:n | Orkestrasyon adımı için talep alışverişinin listesi. |

### <a name="preconditions"></a>Önkoşulları

**Önkoşullar** öğesi aşağıdaki öğeyi içerir:

| Öğe | Oluşum | Açıklama |
| ------- | ----------- | ----------- |
| Ön koşul | 1:n | Kullanılan teknik profile bağlı olarak, istemciyi talep sağlayıcı seçimine göre yönlendirir veya talep alışverişinde bulunmak için sunucu çağrısı yapar. |


#### <a name="precondition"></a>Ön koşul

**Önkoşul** öğesi aşağıdaki öznitelikleri içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| `Type` | Evet | Bu ön koşul için gerçekleştirecek denetim veya sorgu türü. Belirtilen talepler kullanıcının geçerli talep kümesinde varsa eylemlerin gerçekleştirilmesi gerektiğini belirten **ClaimsExist**veya Belirtilen talep varsa ve değeri belirtilen değere eşitse eylemlerin gerçekleştirilmesi gerektiğini belirten **ClaimEquals**değeri olabilir. |
| `ExecuteActionsIf` | Evet | Ön koşuldaki eylemlerin gerçekleştirilmesi gerekip gerekeceğine karar vermek için doğru veya yanlış bir sınama kullanın. |

**Önkoşul** öğeleri aşağıdaki öğeleri içerir:

| Öğe | Oluşum | Açıklama |
| ------- | ----------- | ----------- |
| Değer | 1:n | Sorgulanması gereken bir ClaimTypeReferenceId. Başka bir değer öğesi denetlenecek değeri içerir.</li></ul>|
| Eylem | 1:1 | Bir düzenleme adımı içindeki ön koşul denetimi doğruysa yapılması gereken eylem. Değeri `Action` `SkipThisOrchestrationStep`ayarlanmışsa, ilişkili `OrchestrationStep` yürütülmemelidir. |

#### <a name="preconditions-examples"></a>Ön koşul örnekleri

Aşağıdaki ön koşullar, kullanıcının objectId'sinin var olup olmadığını denetler. Kullanıcı yolculuğunda, kullanıcı yerel hesabı kullanarak oturum açmayı seçmiştir. ObjectId varsa, bu düzenleme adımını atlayın.

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

Aşağıdaki ön koşullar, kullanıcının bir sosyal hesapla oturum açıp açmadığını denetler. Dizindeki kullanıcı hesabını bulmak için bir girişimde bulunulr. Kullanıcı yerel bir hesapta kaydolursa veya kaydolursa, bu düzenleme adımını atlayın.

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

Ön koşullar birden fazla ön koşulu kontrol edebilir. Aşağıdaki örnekte 'objectId' veya 'e-posta' olup olmadığını denetler. İlk koşul doğruysa, yolculuk bir sonraki orkestrasyon adımına atlar.

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

## <a name="claimsproviderselection"></a>İddia Sağlayıcı Seçimi

Bir düzenleme adımı `ClaimsProviderSelection` türü `CombinedSignInAndSignUp` veya kullanıcının oturum açabileceği talep sağlayıcılarının listesini içerebilir. `ClaimsProviderSelections` Öğelerin içindeki öğelerin sırası, kullanıcıya sunulan kimlik sağlayıcılarının sırasını denetler.

**ClaimsProviderSelections** öğesi aşağıdaki öğeyi içerir:

| Öğe | Oluşum | Açıklama |
| ------- | ----------- | ----------- |
| İddia Sağlayıcı Seçimi | 1:n | Seçilebilen talep sağlayıcılarının listesini sağlar.|

**ClaimsProviderSelections** öğesi aşağıdaki öznitelikleri içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| DisplayOption| Hayır | Tek bir talep sağlayıcı seçiminin bulunduğu bir servis talebinin davranışını denetler. Olası değerler: `DoNotShowSingleProvider` (varsayılan) , kullanıcı hemen federe kimlik sağlayıcısına yönlendirilir. Veya `ShowSingleProvider` Azure AD B2C oturum açma sayfasını tek kimlik sağlayıcı seçimiyle birlikte sunar. Bu özniteliği kullanmak için içerik tanım `urn:com:microsoft:aad:b2c:elements:contract:providerselection:1.0.0` lı [sürümü](page-layout.md) ve üzerinde olmalıdır.|

**ClaimsProviderSelection** öğesi aşağıdaki öznitelikleri içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| HedefClaimsExchangeid | Hayır | Talep sağlayıcı seçiminin bir sonraki düzenleme adımında yürütülen talep değişiminin tanımlayıcısı. Bu öznitelik veya DoğrulamaClaimsExchangeId özniteliği belirtilmelidir, ancak her ikisi de belirtilmemelidir. |
| DoğrulamaİddialarıExchangeId | Hayır | Talep sağlayıcı seçimini doğrulamak için geçerli düzenleme adımında yürütülen talep değişiminin tanımlayıcısı. Bu öznitelik veya TargetClaimsExchangeId özniteliği belirtilmelidir, ancak her ikisi de belirtilmemelidir. |

### <a name="claimsproviderselection-example"></a>İddialarSağlayıcıSeçimi örneği

Aşağıdaki düzenleme adımında, kullanıcı Facebook, LinkedIn, Twitter, Google veya yerel bir hesapla oturum açmayı seçebilir. Kullanıcı sosyal kimlik sağlayıcılarından birini seçerse, ikinci düzenleme adımı öznitelikte `TargetClaimsExchangeId` belirtilen seçili talep değişimiyle birlikte yürütülür. İkinci düzenleme adımı, oturum açma işlemini tamamlamak için kullanıcıyı sosyal kimlik sağlayıcısına yönlendirir. Kullanıcı yerel hesapla oturum açmayı seçerse, Azure AD B2C aynı düzenleme adımında (aynı kayıt sayfası veya oturum açma sayfası) kalır ve ikinci düzenleme adımını atlar.

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

## <a name="claimsexchanges"></a>İddialarBorsalar

**ClaimsExchanges** öğesi aşağıdaki öğeyi içerir:

| Öğe | Oluşum | Açıklama |
| ------- | ----------- | ----------- |
| İddia Değişimi | 1:n | Kullanılan teknik profile bağlı olarak, istemciyi seçilen ClaimsProviderSelection'a göre yönlendirir veya talep alışverişinde bulunmak için sunucu çağrısı yapar. |

**ClaimsExchange** öğesi aşağıdaki öznitelikleri içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| Kimlik | Evet | Talep değişim adımının tanımlayıcısı. Tanımlayıcı, ilkedeki bir talep sağlayıcı seçim adımından gelen talep değişimine başvurmak için kullanılır. |
| TeknikProfilReferenceId | Evet | Yürütülecek teknik profilin tanımlayıcısı. |
