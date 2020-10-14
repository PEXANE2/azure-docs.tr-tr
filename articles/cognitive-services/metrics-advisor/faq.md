---
title: Ölçüm Danışmanı sık sorulan sorular
titleSuffix: Azure Cognitive Services
description: Ölçüm Danışmanı hizmeti hakkında sık sorulan sorular.
services: cognitive-services
author: mrbullwinkle
manager: nitinme
ms.service: cognitive-services
ms.subservice: metrics-advisor
ms.topic: conceptual
ms.date: 09/30/2020
ms.author: mbullwin
ms.openlocfilehash: 42b23876761afa213b07f07b3a61e125dcf0824b
ms.sourcegitcommit: 2e72661f4853cd42bb4f0b2ded4271b22dc10a52
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/14/2020
ms.locfileid: "92046817"
---
# <a name="metrics-advisor-frequently-asked-questions"></a>Ölçüm Danışmanı sık sorulan sorular

### <a name="what-is-the-cost-of-my-instance"></a>Örneðimin maliyeti nedir?

Şu anda, önizleme sırasında örneğinizi kullanma maliyeti yoktur.

### <a name="why-is-the-demo-website-readonly"></a>Tanıtım Web sitesi neden salt okunur?

[Tanıtım Web sitesi](https://anomaly-detector.azurewebsites.net/) genel kullanıma sunulmuştur. Bu örnek, verileri yanlışlıkla karşıya yüklemeyi engellemek için salt okunurdur.

### <a name="why-cant-i-create-the-resource-the-pricing-tier-is-unavailable-and-it-says-you-have-already-created-1-s0-for-this-subscription"></a>Kaynağı neden oluşturamıyorum? "Fiyatlandırma katmanı" kullanılamıyor ve "Bu abonelik için zaten 1 S0 oluşturdunuz" diyor musunuz?

:::image type="content" source="media/pricing.png" alt-text="Bir F0 kaynağı zaten varsa ileti":::

Genel Önizleme sırasında, bir bölgede bir abonelik altında yalnızca bir ölçüm Danışmanı örneğine izin verilir.

Aynı bölgede aynı abonelik kullanılarak oluşturulmuş bir örneğiniz zaten varsa, yeni bir örnek oluşturmak için farklı bir bölge veya farklı bir abonelik deneyebilirsiniz. Ayrıca, yeni bir örnek oluşturmak için var olan bir örneği silebilirsiniz.

Mevcut örneği zaten sildiyseniz ancak hatayı görmeye devam ederseniz, yeni bir örnek oluşturmadan önce lütfen kaynak silme sonrasında yaklaşık 20 dakika bekleyin.

## <a name="basic-concepts"></a>Temel kavramlar

### <a name="what-is-multi-dimensional-time-series-data"></a>Çok boyutlu zaman serisi verileri nedir?

Sözlükteki [çok boyutlu ölçüm](glossary.md#multi-dimensional-metric)  tanımına bakın.

### <a name="how-much-data-is-needed-for-metrics-advisor-to-start-anomaly-detection"></a>Ölçüm Danışmanı 'nın anomali algılamayı başlatması için ne kadar veri gerekiyor?

En azından, bir veri noktası anomali algılamayı tetikleyebilir. Bununla birlikte en iyi doğruluk getirmez. Hizmet, veri akışı oluşturma sırasında "Fill-Gap" kuralı olarak belirttiğiniz değeri kullanarak önceki veri noktalarının bir penceresini varsayacaktır.

Üzerinde algılamayı yapmak istediğiniz zaman damgasından önce bazı veriler olmasını öneririz.
Verilerinizin ayrıntı düzeyi temelinde, önerilen veri miktarı aşağıda belirtildiği gibi değişir.

| Ayrıntı düzeyi | Algılama için önerilen veri miktarı |
| ----------- | ------------------------------------- |
| 5 dakikadan az | 4 gün veri |
| 5 dakika ila 1 gün | 28 gün veri |
| 1 günden fazla, 31 güne kadar | 4 yıllık veri |
| 31 günden fazla | 48 yıllık veri |

### <a name="why-metrics-advisor-doesnt-detect-anomalies-from-historical-data"></a>Ölçüm Danışmanı neden geçmiş verilerden bozukluklar algılamıyor?

Ölçüm Danışmanı canlı akış verilerini algılamak için tasarlanmıştır. Hizmetin geri bakması ve anomali algılama çalıştırması için geçmiş verilerin maksimum uzunluğuna yönelik bir sınırlama vardır. Bu, yalnızca belirli bir en erken zaman damgasına ait veri noktalarının anomali algılama sonuçlarına sahip olacağını gösterir. Bu en erken zaman damgası, verilerinizin ayrıntı düzeyine bağlıdır.

Verilerinizin ayrıntı düzeyi temelinde, anomali algılama sonuçlarına sahip olacak geçmiş verilerin uzunlukları aşağıda verilmiştir.

| Ayrıntı düzeyi | Anomali algılama için maksimum geçmiş verileri uzunluğu |
| ----------- | ------------------------------------- |
| 5 dakikadan az | Ekleme saati-13 saat |
| 1 saatten az 5 dakika | Ekleme zamanı-4 gün  |
| 1 günden daha az 1 saat | Ekleme saati-14 gün  |
| 1 gün | Ekleme saati-28 gün  |
| 1 günden fazla, 31 günden daha az | Ekleme saati-2 yıl  |
| 31 günden fazla | Ekleme saati-24 yıl   |

### <a name="more-concepts-and-technical-terms"></a>Daha fazla kavram ve teknik koşullar

Daha fazla bilgi için bkz. [Sözlük](glossary.md) .

###  <a name="how-do-i-write-a-valid-query-for-ingesting-my-data"></a>Nasıl yaparım? verileri almak için geçerli bir sorgu yazmak mı istiyorsunuz?  

Ölçüm Danışmanı 'nın verilerinizi almak için tek bir zaman damgasında verilerinizin boyutlarını döndüren bir sorgu oluşturmanız gerekir. Ölçüm Danışmanı, her zaman damgasından verileri almak için bu sorguyu birden çok kez çalıştıracaktır. 

Sorgunun belirli bir zaman damgasında her bir boyut birleşimi için en çok bir kayıt döndürmesi gerektiğini unutmayın. Döndürülen tüm kayıtlar aynı zaman damgasına sahip olmalıdır. Sorgu tarafından döndürülen bir yinelenen kayıt olmamalıdır.

Örneğin, günlük ölçüm için aşağıdaki sorguyu oluşturduğunuzu varsayalım: 
 
`select timestamp, city, category, revenue from sampledata where Timestamp >= @StartTime and Timestamp < dateadd(DAY, 1, @StartTime)`

Zaman seriniz için doğru ayrıntı düzeyini kullandığınızdan emin olun. Saatlik ölçüm için şunu kullanabilirsiniz: 

`select timestamp, city, category, revenue from sampledata where Timestamp >= @StartTime and Timestamp < dateadd(hour, 1, @StartTime)`

Bu sorguların yalnızca tek bir zaman damgasında veri döndürdüğüne ve ölçüm Danışmanı tarafından alınacak tüm boyut birleşimlerini içerdiğini unutmayın. 

:::image type="content" source="media/query-result.png" alt-text="Bir F0 kaynağı zaten varsa ileti" lightbox="media/query-result.png":::


### <a name="how-do-i-detect-spikes--dips-as-anomalies"></a>Nasıl yaparım?, anormal bir şekilde & ileri 'leri algılar mi?

Önceden tanımlanmış sabit eşikler varsa, [anomali algılama yapılandırmalarında](how-tos/configure-metrics.md#anomaly-detection-methods)"Sabit eşik" i el ile ayarlayabilirsiniz.
Hiçbir eşik yoksa AI tarafından desteklenen "akıllı algılama" kullanabilirsiniz. Ayrıntılar için lütfen [yapılandırmayı algılama](how-tos/configure-metrics.md#tune-the-detecting-configuration) bölümüne bakın.

### <a name="how-do-i-detect-inconformity-with-regular-seasonal-patterns-as-anomalies"></a>Nasıl yaparım?, düzenli (mevsimlik) desenlerle anormal olarak indiksel olarak algılama mi?

"Akıllı algılama" mevsimsel desenler dahil verilerinizin desenini öğreniyor. Daha sonra, düzenli desenlerle aynı şekilde uyumlu olmayan bu veri noktalarını algılar. Ayrıntılar için lütfen [yapılandırmayı algılama](how-tos/configure-metrics.md#tune-the-detecting-configuration) bölümüne bakın.

### <a name="how-do-i-detect-flat-lines-as-anomalies"></a>Düz satırları anomali olarak Nasıl yaparım? mi?

Verileriniz normalde çok kararsız ve büyük bir ölçüde dalgalandığı ve çok kararlı ve hatta düz bir çizgi haline geldiğinde uyarılmak istiyorsanız, değişiklik çok küçük olduğunda bu tür veri noktalarını algılamak için "değişiklik eşiği" yapılandırılabilir.
Ayrıntılar için lütfen [anomali algılama yapılandırmalarına](how-tos/configure-metrics.md#anomaly-detection-methods) bakın.

## <a name="next-steps"></a>Sonraki Adımlar
- [Ölçüm danışmanına genel bakış](overview.md)
- [Demo sitesini deneyin](quickstarts/explore-demo.md)
- [Web portalını kullanma](quickstarts/web-portal.md)