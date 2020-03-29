---
title: Hizmet yapılandırması - QnA Maker
description: Kaynakların nasıl ve nerede yapılandırılabildiğini anlayın.
ms.topic: reference
ms.date: 02/21/2020
ms.openlocfilehash: 8ef6fecbfeb119d0c68ec5bc3bbc90ec449dbb7d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77651866"
---
# <a name="service-configuration"></a>Hizmet yapılandırması

QnA Maker, Bilişsel Arama, Uygulama Hizmeti, Uygulama Hizmeti Planı ve Uygulama Öngörüleri dahil olmak üzere çeşitli Azure kaynaklarını (hizmetleri) kullanır.

QnA Maker tarafından desteklenen bu ayarlara yapılan tüm özelleştirmeler aşağıda listelenmiştir.

## <a name="app-service"></a>App Service

QnA [Maker, oluşturan Yanıt API'sı](https://docs.microsoft.com/rest/api/cognitiveservices/qnamakerruntime/runtime/generateanswer)tarafından kullanılan sorgu çalışma süresini sağlamak için Uygulama Hizmetini kullanır.


Bu ayarlar, Uygulama Hizmeti için Azure portalında kullanılabilir. Ayarlar Ayarlar **seçerek**kullanılabilir , sonra **Yapılandırma**.

Uygulama Ayarları listesinde n ayrı bir ayar ayarlayabilir veya **Gelişmiş düzenleme'yi**seçerek birkaç ayarı değiştirebilirsiniz.

|Kaynak|Ayar|
|--|--|
|AzureSearchAdminKey|Bilişsel Arama - QnA set depolama ve Ranker #1 için kullanılır|
|AzureSearchName|Bilişsel Arama - QnA set depolama ve Ranker #1 için kullanılır|
|Varsayılan Yanıt|Eşleşme bulunamediğinde yanıt metni|
|UserAppInsightsAppId|Sohbet günlüğü ve telemetri|
|UserAppInsightsKey|Sohbet günlüğü ve telemetri|
|UserAppInsightsName|Sohbet günlüğü ve telemetri|

[Bilişsel Arama hizmetinizi nasıl değiştireceğinizi](./how-to/set-up-qnamaker-service-azure.md#configure-qna-maker-to-use-different-cognitive-search-resource) öğrenin.

Değişiklik yapmayı bitirdikten sonra hizmeti Azure portalının **Genel Bakış** sayfasından **yeniden başlatmanız** gerekir.

## <a name="qna-maker-service"></a>QnA Maker Servisi

QnA Maker hizmeti, aşağıdaki kullanıcıların tek bir QnA Maker hizmeti ve tüm bilgi tabanları üzerinde işbirliği yapmaları için yapılandırma sağlar.

Hizmetinize [ortak çalışanlar eklemeyi](./how-to/collaborate-knowledge-base.md) öğrenin.

## <a name="application-insights"></a>Application Insights

Application Insights'ın QnA Maker'a özgü yapılandırma ayarları yoktur.

## <a name="app-service-plan"></a>App Service Planı

Uygulama Hizmet Planı'nda QnA Maker'a özgü yapılandırma ayarları yoktur.

## <a name="next-steps"></a>Sonraki adımlar

Bilgi tabanına almak istediğiniz belgelerin ve URL'lerin [biçimleri](reference-document-format-guidelines.md) hakkında daha fazla bilgi edinin.