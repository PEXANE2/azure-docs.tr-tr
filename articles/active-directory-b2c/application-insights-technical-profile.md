---
title: Özel ilkede Application Insights teknik profili tanımlama
titleSuffix: Azure AD B2C
description: Azure Active Directory B2C bir özel ilkede Application Insights teknik profili tanımlayın.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 03/20/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: f50373b0841b7626bc405f121015c15ae1587a97
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80108581"
---
# <a name="define-an-application-insights-technical-profile-in-an-azure-ad-b2c-custom-policy"></a>Azure AD B2C özel ilkesinde Application Insights teknik profil tanımlama

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Azure Active Directory B2C (Azure AD B2C), Azure AD B2C için sunulan izleme anahtarını kullanarak doğrudan olay verilerini [Application Insights](../azure-monitor/app/app-insights-overview.md) göndermeyi destekler.  Application Insights teknik bir profille, Kullanıcı yolculukları için ayrıntılı ve özelleştirilmiş olay günlükleri edinebilirsiniz:

* Kullanıcı davranışında Öngörüler elde edin.
* Geliştirme veya üretimde kendi ilkelerinizin sorunlarını giderin.
* Ölçüm performansı.
* Application Insights bildirimler oluşturun.


## <a name="protocol"></a>Protokol

**Protokol** öğesinin `Proprietary` **Name** özniteliğinin olarak ayarlanması gerekir. **Handler** özniteliği, Application Insights için Azure AD B2C tarafından kullanılan protokol işleyici derlemesinin tam adını içermelidir:`Web.TPEngine.Providers.AzureApplicationInsightsProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null`

Aşağıdaki örnek, ortak Application Insights teknik profilini gösterir. Diğer Application Insights teknik profiller, yapılandırmasından yararlanmak için AzureInsights-Common ' i içerir.  

```xml
<TechnicalProfile Id="AzureInsights-Common">
  <DisplayName>Azure Insights Common</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.Insights.AzureApplicationInsightsProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
</TechnicalProfile>
```

## <a name="input-claims"></a>Giriş talepleri

**Inputclaim** öğesi Application Insights gönderileceği talepler listesini içerir. Ayrıca, talebin adını Application Insights görünmesini istediğiniz bir adla eşleyebilirsiniz. Aşağıdaki örnek, Application Insights için nasıl Telemetriler gönderileceğini gösterir. Bir olayın özellikleri, olaya eklenen özelliğin sözdizimi `{property:NAME}`aracılığıyla eklenir. DefaultValue, bir statik değer veya desteklenen [talep çözümleyicilerine](claim-resolver-overview.md)göre çözümlenen bir değer olabilir.

```XML
<InputClaims>
  <InputClaim ClaimTypeReferenceId="PolicyId" PartnerClaimType="{property:Policy}" DefaultValue="{Policy:PolicyId}" />
  <InputClaim ClaimTypeReferenceId="CorrelationId" PartnerClaimType="{property:JourneyId}" DefaultValue="{Context:CorrelationId}" />
  <InputClaim ClaimTypeReferenceId="Culture" PartnerClaimType="{property:Culture}" DefaultValue="{Culture:RFC5646}" />
  <InputClaim ClaimTypeReferenceId="objectId" PartnerClaimType="{property:objectId}"  />
</InputClaims>
```

**Inputclaimstransformations** öğesi, giriş taleplerini değiştirmek veya Application Insights göndermeden önce yenilerini oluşturmak Için kullanılan **inputclaimstransreference** öğelerinin bir koleksiyonunu içerebilir.

## <a name="persist-claims"></a>Kalıcı talepler

PersistedClaims öğesi kullanılmıyor.

## <a name="output-claims"></a>Çıkış talepleri

Outputclaim ve OutputClaimsTransformations öğeleri kullanılmaz.

## <a name="cryptographic-keys"></a>Şifreleme anahtarları

CryptographicKeys öğesi kullanılmıyor.


## <a name="metadata"></a>Meta Veriler

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| Instrumentationkey| Yes | Olayları günlüğe kaydetmek için kullanılacak Application Insights [izleme anahtarı](../azure-monitor/app/create-new-resource.md#copy-the-instrumentation-key). | 
| DeveloperMode| Hayır | Geliştirici modunun etkin olup olmadığını gösteren bir Boole değeri. Olası değerler: `true` veya `false` (varsayılan). Bu meta veriler, olayların nasıl arabelleğe alınacağını denetler. En az olay hacmi olan bir geliştirme ortamında, geliştirici modunun etkinleştirilmesi, Application Insights ' a anında gönderilen olaylarda sonuçlanır.|  
|Disabletelemetri |Hayır |Telemetrinin etkin olup olmadığını gösteren bir Boole değeri. Olası değerler: `true` veya `false` (varsayılan).| 


## <a name="next-steps"></a>Sonraki adımlar

- [Application Insights kaynağı oluşturma](../azure-monitor/app/create-new-resource.md)
- [Azure Active Directory B2C Application Insights kullanarak Kullanıcı davranışının nasıl izleneceğini](analytics-with-application-insights.md) öğrenin
