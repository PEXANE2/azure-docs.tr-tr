---
title: Ölçüm Danışmanı hizmeti nedir?
titleSuffix: Azure Cognitive Services
description: Ölçüm Danışmanı nedir?
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: metrics-advisor
ms.topic: overview
ms.date: 09/14/2020
ms.author: aahi
ms.openlocfilehash: 408bdd948977218d9b39a39bf97391a4141e545c
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90947956"
---
# <a name="what-is-metrics-advisor-preview"></a>Ölçüm Danışmanı (Önizleme) nedir? 

Ölçüm Danışmanı, AI kullanan Azure bilişsel hizmetler 'in, zaman serisi verilerinde veri izleme ve anomali algılama işlemleri gerçekleştirmesini sağlayan bir parçasıdır. Hizmet, verilerinize model uygulama sürecini otomatikleştirir ve makine öğrenimine gerek kalmadan veri alımı, anomali algılama ve Tanılama için bir API web tabanlı çalışma alanı kümesi sağlar. Ölçüm Danışmanı 'nı kullanarak şunları yapın:

* Birden çok veri kaynağından çok boyutlu verileri analiz etme 
* Anormallikleri tanımla ve bağıntı
* Verilerinizde kullanılan anomali algılama modelini yapılandırın ve hassas ayarlar yapın
* Kök neden analiziyle ilgili aykırları ve yardımı tanılayın. 

:::image type="content" source="media/metrics-advisor-overview.png" alt-text="Ölçüm danışmanına genel bakış":::

## <a name="connect-to-a-variety-of-data-sources"></a>Çeşitli veri kaynaklarına bağlanma

Ölçüm Danışmanı, SQL Server, Azure Blob depolama, MongoDB gibi birçok veri mağazasında bulunan [çok boyutlu ölçüm](how-tos/onboard-your-data.md) verilerine bağlanabilir ve bunları alabilir. 

## <a name="easy-to-use-and-customizable-anomaly-detection"></a>Kullanımı kolay ve özelleştirilebilir anomali algılama

* Ölçüm Danışmanı, herhangi bir makine öğrenimine gerek kalmadan verileriniz için en iyi modeli otomatik olarak seçer. 
* [Çok boyutlu ölçümler](glossary.md#multi-dimensional-metric)içindeki her bir zaman serisini otomatik olarak izleyin.
* Verilerinize uygulanan modeli ve gelecekteki anomali algılama sonuçlarını özelleştirmek için [parametre ayarlama](how-tos/configure-metrics.md) ve [etkileşimli geri bildirimleri](how-tos/anomaly-feedback.md) kullanın.


## <a name="real-time-alerts-through-multiple-channels"></a>Birden çok kanal aracılığıyla gerçek zamanlı uyarılar

Anomali algılandığında, ölçüm Danışmanı, e-posta kancaları, Web kancaları ve Azure DevOps kancaları gibi çok sayıda kanal aracılığıyla [gerçek zamanlı uyarılar gönderebilir](how-tos/alerts.md) . Esnek uyarı kuralları hangi uyarıların gönderileceğini ve nerede gönderileceğini özelleştirmenize olanak sağlar.

## <a name="smart-diagnostic-insights-by-analyzing-anomalies"></a>Anormallikleri çözümleyerek akıllı tanılama öngörüleri

Çok boyutlu ölçümler üzerinde algılanan anormallikleri çözümleyin ve en olası temel neden, tanılama ağaçları, ölçüm detaya gitme ve daha fazlasını içeren [akıllı tanılama öngörüleri](how-tos/diagnose-incident.md) oluşturun. [Ölçüm grafiğini](how-tos/metrics-graph.md)yapılandırarak, çapraz ölçümler Analizi olayları görselleştirmenize yardımcı olmak için etkinleştirilebilir.


## <a name="typical-workflow"></a>Tipik iş akışı

İş akışı basittir: verilerinizi ekledikten sonra anomali algılamayı hassas şekilde ayarlayabilir ve senaryonuza uyacak şekilde yapılandırma oluşturabilirsiniz.

1. Ölçüm Danışmanı için [bir Azure kaynağı oluşturun](../cognitive-services-apis-create-account.md) . 
2. Önceden yapılandırılmış örnek verilerle bir örnek ölçüm Danışmanı örneği görmek için demo sitesini deneyin. 
3. Web portalını kullanarak ilk monitörünüzü oluşturun.
    1. Verilerinizi ekleme
    2. Anomali algılama için ince ayar yapma
    3. Uyarılara abone olma
    4. Tanılama öngörülerini görüntüleme
1. Örneğinizi özelleştirmek için REST API kullanın.

## <a name="next-steps"></a>Sonraki adımlar

* [Demo sitesini](quickstarts/explore-demo.md)deneyin.
* Hızlı başlangıcı keşfet: [Web 'de ilk ölçümünüzün izlenmesi](quickstarts/web-portal.md).
* Hızlı başlangıcı keşfet: [çözümünüzü özelleştirmek IÇIN REST API 'Lerini kullanın](quickstarts/rest-api.md).
