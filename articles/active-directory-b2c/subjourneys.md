---
title: Azure Active Directory B2C için alt bağlantı | Microsoft Docs
description: Azure Active Directory B2C bir özel ilkenin alt, neys öğesini belirtin.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/15/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 6609dabe9bd507751bd131a4effe24295e2aac04
ms.sourcegitcommit: a2d8acc1b0bf4fba90bfed9241b299dc35753ee6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/12/2020
ms.locfileid: "91952466"
---
# <a name="subjourneys"></a>Alt, neys

Alt işlemler, Kullanıcı yolculuğu içindeki düzenleme adımlarının akışını düzenlemek ve basitleştirmek için kullanılabilir. [Kullanıcı yolculukları](userjourneys.md) , bir ilkenin bir kullanıcı için istenen talepleri elde etmesine izin veren bir ilke aracılığıyla açık yollar belirtmektir. Kullanıcı, bağlı olan tarafa sunulacak talepleri almak için bu yollar üzerinden alınır. Diğer bir deyişle, Kullanıcı yolculukları, Azure AD B2C kimlik deneyimi çerçevesi, isteği işleyen bir son kullanıcının iş mantığını tanımlar. Kullanıcı yolculuğu, başarılı bir işlem için izlenmesi gereken bir düzenleme sırası olarak temsil edilir. Bir düzenleme adımının [Claimsexchange](userjourneys.md#claimsexchanges) öğesi, çalıştıran tek bir [Teknik profile](technical-profiles-overview.md) bağlıdır.

Bir alt yolculukta, Kullanıcı yolculuğu içinde herhangi bir noktada çağrılabilecek bir düzenleme adımları gruplandırmadır. Yeniden kullanılabilir adım dizileri oluşturmak veya iş mantığını daha iyi temsil etmek için dallanmayı uygulamak üzere alt, neys kullanabilirsiniz.

[!INCLUDE [b2c-public-preview-feature](../../includes/active-directory-b2c-public-preview.md)]

## <a name="user-journey-branching"></a>Kullanıcı yolculuğu dallanma

Alt bağlantı, her ikisi de başarılı bir işlem için izlenmesi gereken bir düzenleme sırası olarak temsil edildiği için [Kullanıcı, neys](userjourneys.md)gibi davranır. Kullanıcı, neys kendi başına çağrılabilir ve yürütmek için bir Sendclaim adımı gerektirir. Alt bağlantı, Kullanıcı ve bağımsız olarak çağrılamaz ve bağımsız olarak çağrılamaz ve Kullanıcı yolculuğunda her zaman çağrılır.

Dalın anahtar bileşeni, Kullanıcı yolculuğunda daha iyi iş mantığı işlemeye izin veriydi. Ortak düzenleme adımları ayrı ayrı olarak çağrılacak ayrı parçalar halinde gruplandırılır. Bir alt yolculukta, birden fazla düzenleme adımının birlikte birbirine bağlanmış olduğu bir yolculuğun basitleşmesi (aynı önkoşullara sahip olması). Bir alt yolculuğun yalnızca Kullanıcı yolculuğu aracılığıyla çağrılması, başka bir alt yolculuğa çağrı yapmamamalıdır.

İki tür alt bağlantı vardır:

- **Call** -çağırıcının denetimini döndürür. Alt yolculuğun yürütülmesi ve sonra Denetim, Kullanıcı yolculuğu içinde yürütülmekte olan düzenleme adımına döndürülür.
- **Transfer** -denetimi, alt yolculuğa (geri alınamaz dallanma) aktarır. Alt yolculuğun, talepleri bağlı olan taraf uygulamasına geri döndürmek için bir Sendclaim adımı olması gerekir.

## <a name="example-scenarios"></a>Örnek senaryolar

### <a name="call-subjourney"></a>Alt yolculuğa çağrı

Aşağıdaki senaryolarda bir çağrı alt yolculuğu yararlı olur:

- Yaş aşımları: Yaş aşımları Için Kullanıcı, neys arasında çok sayıda paylaşılan bileşen vardır. Dallanma, ortak öğeleri paylaşılabilir bileşenlere derlemeye izin verir.  
- Ebeveyn onayı: dallandırma, kullanıcının, kullanıcının onay gerektirdiğini bulduktan sonra bir ana izin Kullanıcı yolculuğuna dallayabilmesine olanak tanıyarak, ebeveyn onayı tasarımında kolaylık sağlar. 
- Oturum açmak için kaydolun: bir kullanıcının dizinde zaten varolduğu, ancak aslında bir hesap oluşturduğunu unuttulabileceği bir senaryoyu düşünün. Bu, kullanıcıya girdiğiniz kimlik bilgilerinin zaten var olduğunu ve kullanıcının bu kullanıcı için bir kaydolma akışından oturum açma akışından bir kayıt akışı yapmaya zorlanmasını zorunlu hale getirmek yerine, bu tür bir durumda istenebilir.  

### <a name="transfer-subjourney"></a>Aktarım alt yolculuğu

Aşağıdaki senaryolarda bir aktarım alt yolculuğu yararlı olur:

- Bir blok sayfası gösteriliyor.
- Bir/B testi, isteği bir belirteci yürütmek ve vermek için bir alt yolculuğa yönlendirerek test edin.

## <a name="adding-a-subjourney-element"></a>Subbir ney öğesi ekleme

Aşağıda, `SubJourney` `Call` Denetim Kullanıcı yolculuğuna geri döndüren türünde bir öğe örneği verilmiştir.

```xml
<SubJourneys>
  <SubJourney Id="ConditionalAccess_Evaluation" Type="Call">
    <OrchestrationSteps>
      <OrchestrationStep Order="1" Type="ClaimsExchange">
       <ClaimsExchanges>
        <ClaimsExchange Id="ConditionalAccessEvaluation" TechnicalProfileReferenceId="ConditionalAccessEvaluation" />
       </ClaimsExchanges>
      </OrchestrationStep>
      <OrchestrationStep Order="2" Type="ClaimsExchange">
        <Preconditions>
          <Precondition Type="ClaimsExist" ExecuteActionsIf="false">
            <Value>conditionalAccessClaimCollection</Value>
            <Action>SkipThisOrchestrationStep</Action>
          </Precondition>
        </Preconditions>
        <ClaimsExchanges>
          <ClaimsExchange Id="GenerateCAClaimFlags" TechnicalProfileReferenceId="GenerateCAClaimFlags" />
        </ClaimsExchanges>
      </OrchestrationStep>
    </OrchestrationSteps>
  </SubJourney>
</SubJourneys>
```

Aşağıda, `SubJourney` `Transfer` bağlı olan taraf uygulamasına bir belirteç döndüren türünde bir öğe örneği verilmiştir.

```xml
<SubJourneys>
  <SubJourney Id="B" Type="Transfer">
    <OrchestrationSteps>
      ...
      <OrchestrationStep Order="5" Type="SendClaims">
    </OrchestrationSteps>
  </SubJourney>
</SubJourneys>
```

### <a name="invoke-a-subjourney-step"></a>Bir alt yolculuğu adımını çağırma

Yeni bir düzenleme adımı `InvokeSubJourney` , bir alt yolculuğun yürütülmesi için kullanılır. Aşağıda, bu düzenleme adımının tüm yürütme öğelerini gösteren bir örnek verilmiştir.

```xml
<OrchestrationStep Order="5" Type="InvokeSubJourney">
  <JourneyList>
    <Candidate SubJourneyReferenceId="ConditionalAccess_Evaluation" />
  </JourneyList>
</OrchestrationStep>
```

> [!NOTE]
> Bir alt yolculuğa başka bir alt yolculuğa çağrı mamalıdır.

## <a name="components"></a>Bileşenler

İlke tarafından desteklenen alt bağlantıyı tanımlamak için, ilke dosyasının en üst düzey öğesinin altına bir **subbir neys** öğesi ekleyin.

**Subıneys** öğesi aşağıdaki öğeyi içerir:

| Öğe | Öğeleri | Açıklama |
| ------- | ----------- | ----------- |
| Alt yolculuğa | 1: n | Tüm Kullanıcı akışı için gereken tüm yapıları tanımlayan bir alt yolculuğun. |

**Subıneys** öğesi aşağıdaki öznitelikleri içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| Id | Yes | İlkedeki alt yolculuğa başvurmak için Kullanıcı yolculuğu tarafından kullanılabilen alt yolculuğun tanımlayıcısı. [Aday](userjourneys.md#journeylist) öğesi 'nin **Subbir Newınreferenceıd** öğesi bu özniteliğe işaret ediyor. |
| Tür | Yes | Olası değerler: `Call` , veya `Transfer` . Daha fazla bilgi için bkz. [Kullanıcı yolculuğu dallandırma](#user-journey-branching)|

**Subyolculuney** öğesi aşağıdaki öğeyi içerir:

| Öğe | Öğeleri | Açıklama |
| ------- | ----------- | ----------- |
| OrchestrationSteps | 1: n | Başarılı bir işlem için izlenmesi gereken bir düzenleme sırası. Her Kullanıcı yolculuğu, sırayla yürütülen bir düzenleme adımları sıralı listesinden oluşur. Herhangi bir adım başarısız olursa, işlem başarısız olur. |

## <a name="orchestrationsteps"></a>OrchestrationSteps

Orchestration Step öğelerinin tüm listesi için bkz. [User, neys](userjourneys.md).

## <a name="next-steps"></a>Sonraki adımlar

[UserJourneys](userjourneys.md)