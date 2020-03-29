---
title: Metin Analizi API'si için örnek kullanıcı senaryoları
titleSuffix: Azure Cognitive Services
description: Metin Analizi API'sini hizmetlerinize ve süreçlerinize entegre etmek için bazı yaygın senaryoları görmek için bu makaleyi kullanın.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: conceptual
ms.date: 03/04/2020
ms.author: aahi
ms.openlocfilehash: 6847059de2a8685a56719f07a041a40456f2aa06
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79219252"
---
# <a name="example-user-scenarios-for-the-text-analytics-api"></a>Metin Analizi API'si için örnek kullanıcı senaryoları

Text Analytics API, metin üzerinden gelişmiş doğal dil işleme sağlayan bulut tabanlı bir hizmettir. Bu makalede, API'yi iş çözümlerinize ve süreçlerinize entegre etmek için bazı örnek kullanım örnekleri açıklanmaktadır. 

## <a name="analyze-survey-results"></a>Anket sonuçlarını analiz et

Duygu Analizi'ni kullanarak ham metin yanıtlarını işleyerek müşteri ve çalışan anket sonuçlarından öngörüler çizin. Analiz, takip ve sürüş nişanları için bulguları toplar.

![Müşteri ve çalışan anketlerinde duyarlılık analizinin nasıl yapılacağını açıklayan bir resim.](media/use-cases/survey-results.svg)

## <a name="analyze-recorded-inbound-customer-calls"></a>Kaydedilen gelen müşteri çağrılarını analiz edin

Metinden Konuşmaya, Duygu Analizine ve Anahtar Tümcecik Çıkarma'yı kullanarak müşteri hizmetleri aramalarından öngörüler ayıklayın. Müşterileri daha iyi anlamak, müşteri hizmetleri eğilimlerini vurgulamak ve müşteri katılımını artırmak için sonuçları Power BI panosunda veya portalda görüntüleyin. API isteklerini raporlama için toplu olarak veya müdahale için gerçek zamanlı olarak gönderin. [GitHub'daki](https://github.com/rlagh2/callcenteranalytics)örnek kodu görün.

![Duyarlılık analizini kullanarak müşteri hizmetleri aramalarından öngörü almanın nasıl otomatikleştirilebildiğini açıklayan bir resim](media/use-cases/azure-inbound.svg)

## <a name="process-and-categorize-support-incidents"></a>Destek olaylarını işleme ve kategorilere ayırma

Yapılandırılmamış metin biçiminde gönderilen destek isteklerini işlemek için Anahtar Tümceme Çıkarma ve Varlık Tanıma'yı kullanın. Kaynak planlama ve eğilim çözümlemesi isteklerini kategorilere ayırmak için ayıklanan ifadeleri ve varlıkları kullanın.

![Olay raporlarını ve eğilimlerini kategorilere ayırmak için anahtar tümcecik çıkarma ve varlık tanımanın nasıl kullanılacağını açıklayan bir resim](media/use-cases/support-incidents.svg)

## <a name="monitor-your-products-social-media-feeds"></a>Ürününüzün sosyal medya akışlarını izleme

Ürününüzün twitter veya Facebook sayfasındaki kullanıcı ürün geri bildirimlerini izleyin. Yeni ürün lansmanlarına yönelik müşteri duyarlılığını analiz etmek, özellikler ve özellik istekleri yle ilgili anahtar ifadeleri ayıklamak veya müşteri şikayetlerini olduğu gibi ele almak için verileri kullanın. Örnek [Microsoft Flow şablonuna](https://flow.microsoft.com/galleries/public/templates/2680d2227d074c4d901e36c66e68f6f9/run-sentiment-analysis-on-tweets-and-push-results-to-a-power-bi-dataset/)bakın.

![Anahtar ifade çıkarma kullanarak sosyal medyadaki ürün ve şirket geri bildirimlerinizi nasıl izleyeceğinizi açıklayan bir resim](media/use-cases/social-feed.svg)

## <a name="classify-and-redact-documents-that-have-sensitive-information"></a>Hassas bilgilere sahip belgeleri sınıflandırma ve redakte etmek

Belgelerdeki kişisel ve hassas bilgileri tanımlamak için Adlandırılmış Varlık Tanıma'yı kullanın. Verileri, güvenli bir şekilde paylaşılabilmeleri için belgeleri sınıflandırmak veya yeniden canlandırmak için kullanın.

![Kişisel bilgileri algılamak ve belgeleri sınıflandırmak ve redact için NER'in nasıl kullanılacağını açıklayan bir resim](media/use-cases/sensitive-docs.jpg)

## <a name="next-steps"></a>Sonraki adımlar

* [Metin Analizi API'si nedir?](overview.md)
* [İstemci kitaplığını kullanarak Metin Analizi API'sine istek gönderme](quickstarts/text-analytics-sdk.md)
