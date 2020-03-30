---
title: Özel bir ilkede Uygulama Öngörüleri teknik profilini tanımlama
titleSuffix: Azure AD B2C
description: Azure Active Directory B2C'de özel bir ilkede Uygulama Öngörüleri teknik profilini tanımlayın.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80108581"
---
# <a name="define-an-application-insights-technical-profile-in-an-azure-ad-b2c-custom-policy"></a>Azure AD B2C özel ilkesinde Uygulama Öngörüleri teknik profilini tanımlayın

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Azure Active Directory B2C (Azure AD B2C), Azure AD B2C'ye sağlanan enstrümantasyon anahtarını kullanarak etkinlik verilerinin doğrudan [Uygulama Öngörüleri'ne](../azure-monitor/app/app-insights-overview.md) gönderilmesini destekler.  Application Insights teknik profili ile, kullanıcı yolculuklarınız için ayrıntılı ve özelleştirilmiş etkinlik günlükleri alabilirsiniz:

* Kullanıcı davranışları hakkında bilgi edinin.
* Geliştirme veya üretimde kendi ilkelerinizi giderin.
* Performansı ölçün.
* Uygulama Öngörüleri'nden bildirimler oluşturun.


## <a name="protocol"></a>Protokol

**Protokol** öğesinin `Proprietary` **Ad** özniteliğinin . **İşleyici** özniteliği, Uygulama Öngörüleri için Azure AD B2C tarafından kullanılan protokol işleyicisi derlemesinin tam nitelikli adını içermelidir:`Web.TPEngine.Providers.AzureApplicationInsightsProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null`

Aşağıdaki örnekte, ortak Application Insights teknik profili gösterilmektedir. Diğer Application Insights teknik profilleri, yapılandırmadan yararlanmak için AzureInsights-Common'ı içerir.  

```xml
<TechnicalProfile Id="AzureInsights-Common">
  <DisplayName>Azure Insights Common</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.Insights.AzureApplicationInsightsProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
</TechnicalProfile>
```

## <a name="input-claims"></a>Giriş talepleri

**InputClaims** öğesi, Uygulama Öngörüleri'ne gönderilecek taleplerin listesini içerir. Ayrıca, uygulama öngörülerinizde görünmeyi tercih ettiğiniz bir ada sahip olduğunuz hakkın adını eşleyebilirsiniz. Aşağıdaki örnek, Uygulama Öngörüleri'ne telemetries nasıl gönderilen gösterir. Bir olayın özellikleri sözdizimi `{property:NAME}`aracılığıyla eklenir , name özelliği olaya ekleniyor. DefaultValue statik bir değer veya desteklenen [talep çözümleyicilerinden](claim-resolver-overview.md)biri tarafından çözülmüş bir değer olabilir.

```XML
<InputClaims>
  <InputClaim ClaimTypeReferenceId="PolicyId" PartnerClaimType="{property:Policy}" DefaultValue="{Policy:PolicyId}" />
  <InputClaim ClaimTypeReferenceId="CorrelationId" PartnerClaimType="{property:JourneyId}" DefaultValue="{Context:CorrelationId}" />
  <InputClaim ClaimTypeReferenceId="Culture" PartnerClaimType="{property:Culture}" DefaultValue="{Culture:RFC5646}" />
  <InputClaim ClaimTypeReferenceId="objectId" PartnerClaimType="{property:objectId}"  />
</InputClaims>
```

**InputClaimsTransformations** öğesi, Uygulama Öngörüleri'ne göndermeden önce giriş taleplerini değiştirmek veya yenilerini oluşturmak için kullanılan **InputClaimsTransformation** öğeleri koleksiyonunu içerebilir.

## <a name="persist-claims"></a>Devam talepleri

PersistedClaims öğesi kullanılmaz.

## <a name="output-claims"></a>Çıktı talepleri

OutputClaims ve OutputClaimsTransformations öğeleri kullanılmaz.

## <a name="cryptographic-keys"></a>Şifreleme tuşları

CryptographicKeys öğesi kullanılmaz.


## <a name="metadata"></a>Meta Veriler

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| EnstrümantasyonAnahtar| Evet | Olayları günlüğe kaydetmek için kullanılacak Olan Uygulama Öngörüleri [enstrümantasyon anahtarı.](../azure-monitor/app/create-new-resource.md#copy-the-instrumentation-key) | 
| Geliştirici Modu| Hayır | Geliştirici modunun etkin olup olmadığını gösteren bir Boolean. Olası `true` değerler: `false` veya (varsayılan). Bu meta veri, olayların arabelleğe nasıl çıkarılbildiğini denetler. En az olay hacmine sahip bir geliştirme ortamında, geliştirici modunun olayların hemen Application Insights'a gönderilmesine neden olur.|  
|Devre dışı Telemetri |Hayır |Telemetrinin etkin olup olmadığını gösteren bir Boolean. Olası `true` değerler: `false` veya (varsayılan).| 


## <a name="next-steps"></a>Sonraki adımlar

- [Uygulama Öngörüleri kaynağı oluşturma](../azure-monitor/app/create-new-resource.md)
- [Uygulama Öngörülerini kullanarak Azure Active Directory B2C'de kullanıcı davranışını](analytics-with-application-insights.md) nasıl izleyeceğinizi öğrenin
