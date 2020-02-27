---
title: Hizmet yapılandırması-Soru-Cevap Oluşturma
description: Kaynakların nasıl ve nerede yapılandırılacağını anlayın.
ms.topic: reference
ms.date: 02/21/2020
ms.openlocfilehash: 8ef6fecbfeb119d0c68ec5bc3bbc90ec449dbb7d
ms.sourcegitcommit: 96dc60c7eb4f210cacc78de88c9527f302f141a9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2020
ms.locfileid: "77651866"
---
# <a name="service-configuration"></a>Hizmet yapılandırması

Soru-Cevap Oluşturma, Bilişsel Arama, App Service, App Service planı ve Application Insights dahil olmak üzere çeşitli Azure kaynaklarını (Hizmetleri) kullanır.

Soru-Cevap Oluşturma tarafından desteklenen bu ayarlara yapılan tüm özelleştirmeler aşağıda listelenmiştir.

## <a name="app-service"></a>App Service

Soru-Cevap Oluşturma, [Generateanswer API](https://docs.microsoft.com/rest/api/cognitiveservices/qnamakerruntime/runtime/generateanswer)tarafından kullanılan sorgu çalışma zamanını sağlamak için App Service kullanır.


Bu ayarlar, App Service için Azure portal kullanılabilir. **Ayarlar ' ı**ve ardından **yapılandırma**' yı seçerek ayarlar kullanılabilir.

Uygulama Ayarları listesinden ayrı bir ayar ayarlayabilir veya **Gelişmiş Düzenle**' yi seçerek çeşitli ayarları değiştirebilirsiniz.

|Kaynak|Ayar|
|--|--|
|AzureSearchAdminKey|Bilişsel Arama-QnA set Storage ve Ranker #1 için kullanılır|
|AzureSearchName|Bilişsel Arama-QnA set Storage ve Ranker #1 için kullanılır|
|DefaultAnswer|Eşleşme bulunamadığında yanıt metni|
|Userappınsi, Sappıd|Sohbet günlüğü ve telemetrisi|
|Userappınsightskey|Sohbet günlüğü ve telemetrisi|
|Userappınsightsname|Sohbet günlüğü ve telemetrisi|

[Bilişsel arama hizmetinizi hizmetinize nasıl değiştireceğinizi nasıl ekleyeceğinizi](./how-to/set-up-qnamaker-service-azure.md#configure-qna-maker-to-use-different-cognitive-search-resource) öğrenin.

Değişiklikleri yapmayı tamamladıktan sonra, Azure portal **genel bakış** sayfasından hizmeti **yeniden başlatmanız** gerekir.

## <a name="qna-maker-service"></a>Soru-cevap Oluşturucu hizmeti

Soru-Cevap Oluşturma Hizmeti, aşağıdaki kullanıcıların tek bir Soru-Cevap Oluşturma hizmetinde ve tüm bilgi bankalarında işbirliği yapması için yapılandırma sağlar.

Hizmetinizdeki [ortak çalışanları nasıl ekleyeceğinizi](./how-to/collaborate-knowledge-base.md) öğrenin.

## <a name="application-insights"></a>Application Insights

Application Insights Soru-Cevap Oluşturma özgü yapılandırma ayarlarına sahip değil.

## <a name="app-service-plan"></a>App Service Planı

App Service planı Soru-Cevap Oluşturma özgü yapılandırma ayarlarına sahip değil.

## <a name="next-steps"></a>Sonraki adımlar

Bilgi Bankası 'nda içeri aktarmak istediğiniz belge ve URL [biçimleri](reference-document-format-guidelines.md) hakkında daha fazla bilgi edinin.