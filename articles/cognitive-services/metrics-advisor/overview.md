---
title: Ölçüm Danışmanı hizmeti nedir?
titleSuffix: Azure Cognitive Services
description: Ölçüm Danışmanı nedir?
services: cognitive-services
author: mrbullwinkle
manager: nitinme
ms.service: cognitive-services
ms.subservice: metrics-advisor
ms.topic: overview
ms.date: 09/14/2020
ms.author: mbullwin
ms.openlocfilehash: 901d86b5569be61f89178dac460b8750bce9ea73
ms.sourcegitcommit: 73d80a95e28618f5dfd719647ff37a8ab157a668
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2021
ms.locfileid: "105605544"
---
# <a name="what-is-metrics-advisor-preview"></a>Ölçüm Danışmanı (Önizleme) nedir? 

Ölçüm Danışmanı, zaman serisi verilerinde veri izleme ve anomali algılama işlemleri gerçekleştirmek için AI kullanan Azure bilişsel hizmetler 'in bir parçasıdır. Hizmet, verilerinize model uygulama sürecini otomatikleştirir ve makine öğrenimine gerek kalmadan veri alımı, anomali algılama ve Tanılama için bir dizi API ve Web tabanlı çalışma alanı sağlar. Geliştiriciler, hizmetin en üstünde Aıops, tahmine dayalı bakım ve iş izleme uygulamaları oluşturabilir. Ölçüm Danışmanı 'nı kullanarak şunları yapın:

* Birden çok veri kaynağından çok boyutlu verileri analiz etme
* Anormallikleri tanımla ve bağıntı
* Verilerinizde kullanılan anomali algılama modelini yapılandırın ve hassas ayarlar yapın
* Kök neden analizi ile ilgili bozukluklar ve yardım

:::image type="content" source="media/metrics-advisor-overview.png" alt-text="Ölçüm danışmanına genel bakış":::

## <a name="connect-to-a-variety-of-data-sources"></a>Çeşitli veri kaynaklarına bağlanma

Ölçüm Danışmanı, SQL Server, Azure Blob depolama, MongoDB gibi birçok veri mağazasında bulunan [çok boyutlu ölçüm](how-tos/onboard-your-data.md) verilerine bağlanabilir ve bunları alabilir.

## <a name="easy-to-use-and-customizable-anomaly-detection"></a>Kullanımı kolay ve özelleştirilebilir anomali algılama

* Ölçüm Danışmanı, herhangi bir makine öğrenimine gerek kalmadan verileriniz için en iyi modeli otomatik olarak seçer.
* [Çok boyutlu ölçümler](glossary.md#multi-dimensional-metric)içindeki her bir zaman serisini otomatik olarak izleyin.
* Verilerinize uygulanan modeli ve gelecekteki anomali algılama sonuçlarını özelleştirmek için [parametre ayarlama](how-tos/configure-metrics.md) ve [etkileşimli geri bildirimleri](how-tos/anomaly-feedback.md) kullanın.

## <a name="real-time-alerts-through-multiple-channels"></a>Birden çok kanal aracılığıyla gerçek zamanlı uyarılar

Anomali algılandığında, ölçüm Danışmanı, e-posta kancaları, Web kancaları ve Azure DevOps kancaları gibi kancaları kullanarak [gerçek zamanlı uyarıları](how-tos/alerts.md) birden çok kanal aracılığıyla gönderebilir. Esnek uyarı kuralları hangi uyarıların gönderileceğini ve bunların hedefini özelleştirmenizi sağlar.

## <a name="smart-diagnostic-insights-by-analyzing-anomalies"></a>Anormallikleri çözümleyerek akıllı tanılama öngörüleri

Çok boyutlu ölçümler üzerinde algılanan anormallikleri çözümleyin ve en olası temel neden, tanılama ağaçları, ölçüm detaya gitme ve daha fazlasını içeren [akıllı tanılama öngörüleri](how-tos/diagnose-incident.md) oluşturun. [Ölçüm grafiğini](how-tos/metrics-graph.md)yapılandırarak, olayları görselleştirmenize yardımcı olmak için çapraz ölçüm Analizi etkinleştirilebilir.


## <a name="typical-workflow"></a>Tipik iş akışı

İş akışı basittir: verilerinizi ekledikten sonra anomali algılamayı hassas şekilde ayarlayabilir ve senaryonuza uyacak şekilde yapılandırma oluşturabilirsiniz.

1. Ölçüm Danışmanı için [bir Azure kaynağı oluşturun](https://go.microsoft.com/fwlink/?linkid=2142156) . 
2. Web portalını kullanarak ilk monitörünüzü oluşturun.
    1. Verilerinizi ekleme
    2. Anomali algılama için ince ayar yapma
    3. Uyarılara abone olma
    4. Tanılama öngörülerini görüntüleme
3. Örneğinizi özelleştirmek için REST API kullanın.

## <a name="next-steps"></a>Sonraki adımlar

* Hızlı başlangıcı keşfet: [Web 'de ilk ölçümünüzün izlenmesi](quickstarts/web-portal.md).
* Hızlı başlangıcı keşfet: [çözümünüzü özelleştirmek IÇIN REST API 'Lerini kullanın](./quickstarts/rest-api-and-client-library.md).
