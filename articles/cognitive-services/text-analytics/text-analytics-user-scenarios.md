---
title: Metin Analizi API'si için örnek kullanıcı senaryoları
titleSuffix: Azure Cognitive Services
description: Metin Analizi API'si hizmetlerinizle ve süreçlerinizle tümleştirmeyle ilgili bazı yaygın senaryoları görmek için bu makaleyi kullanın.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: overview
ms.date: 02/25/2020
ms.author: aahi
ms.openlocfilehash: 560f34d21a68cecc4950db5dcceb9c8ef7b05a49
ms.sourcegitcommit: f15f548aaead27b76f64d73224e8f6a1a0fc2262
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/26/2020
ms.locfileid: "77614422"
---
# <a name="example-user-scenarios-for-the-text-analytics-api"></a>Metin Analizi API'si için örnek kullanıcı senaryoları

Metin Analizi API'si, metin üzerinde gelişmiş doğal dil işleme sağlayan bulut tabanlı bir hizmettir. Bu makalede, API 'YI iş çözümlerinizi ve süreçlerinizle tümleştirmek için bazı örnek kullanım durumları açıklanmaktadır. 

## <a name="analyze-survey-results"></a>Anket sonuçlarını çözümle

Yaklaşım Analizi kullanarak ham metin yanıtlarını işleyerek müşteri ve çalışan anketi sonuçlarından Öngörüler çizin. Analiz için bulguları toplayın, izleyin ve görüşmeleri yapın.

![Müşteri ve çalışan anketlerde yaklaşım analizinin nasıl gerçekleştirileceğini açıklayan bir görüntü.](media/use-cases/survey-results.svg)

## <a name="analyze-recorded-inbound-customer-calls"></a>Kayıtlı gelen müşteri çağrılarını çözümle

Metin Okuma, Yaklaşım Analizi ve Anahtar İfade Ayıklama kullanarak müşteri hizmetleri çağrılarından Öngörüler ayıklayın. Müşterileri daha iyi anlamak, müşteri hizmeti eğilimlerini vurgulamak ve müşteri katılımı sağlamak için Power BI panoda veya portalda sonuçları görüntüleyin. API isteklerini raporlama için bir toplu iş olarak veya müdahale için gerçek zamanlı olarak gönderin. [GitHub 'daki](https://github.com/rlagh2/callcenteranalytics)örnek koda bakın.

![Yaklaşım analizini kullanarak müşteri hizmetleri çağrılarından Öngörüler elde etme işlemini otomatik hale getirmeyi açıklayan bir görüntü](media/use-cases/azure-inbound.svg)

## <a name="process-and-categorize-support-incidents"></a>Destek olaylarını işleme ve kategorilere ayırma

Yapılandırılmamış metinsel biçimde gönderilen destek isteklerini işlemek için Anahtar İfade Ayıklama ve varlık tanıma özelliğini kullanın. Kaynak planlama ve eğilim analizi isteklerini sınıflandırmak için ayıklanan tümcecikleri ve varlıkları kullanın.

![Olay raporlarını ve eğilimleri kategorilere ayırmak için anahtar tümceciği ayıklama ve varlık tanımayı nasıl kullanacağınızı açıklayan bir görüntü](media/use-cases/support-incidents.svg)

## <a name="monitor-your-products-social-media-feeds"></a>Ürününüzün sosyal medya akışlarını izleyin

Ürününüzün Twitter veya Facebook sayfasında Kullanıcı ürün geri bildirimini izleyin. Yeni ürünlere yönelik müşteri yaklaşımını çözümlemek, Özellikler ve özellik istekleri hakkında önemli tümceleri ayıklamak veya müşteri şikayetlerini olduğu gibi çözmek için verileri kullanın. Örnek [Microsoft Flow şablonu](https://flow.microsoft.com/galleries/public/templates/2680d2227d074c4d901e36c66e68f6f9/run-sentiment-analysis-on-tweets-and-push-results-to-a-power-bi-dataset/)' na bakın.

![Anahtar tümceciği ayıklama kullanarak ürün ve şirket geri bildiriminizi sosyal medya hakkında nasıl izleyeceğinizi açıklayan bir görüntü](media/use-cases/social-feed.svg)

## <a name="classify-and-redact-documents-that-have-sensitive-information"></a>Hassas bilgiler içeren belgeleri sınıflandırın ve redaksiyonlar

Belgelerde kişisel ve hassas bilgileri tanımlamak için adlandırılmış varlık tanımayı kullanın. Belgeleri sınıflandırmak veya Redaksiyonları kullanarak güvenli bir şekilde paylaşılabilmesi için verileri kullanın.

![Kişisel bilgileri algılamak ve belgeleri sınıflandırmak ve Redaksiyonları belirlemek için NER 'in nasıl kullanılacağını açıklayan bir görüntü](media/use-cases/sensitive-docs.jpg)

## <a name="next-steps"></a>Sonraki adımlar

* [Metin Analizi API'si nedir?](overview.md)
* [İstemci kitaplığını kullanarak Metin Analizi API'si istek gönderme](quickstarts/text-analytics-sdk.md)
