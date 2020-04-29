---
title: Azure Stream Analytics 'de anomali algılama
description: Bu makalede, Azure Stream Analytics ve Azure Machine Learning nasıl kullanılacağı açıklanır.
author: mamccrea
ms.author: mamccrea
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 06/21/2019
ms.openlocfilehash: 51b9c827d453eef2e2e75e1aa5222204eaa38d0e
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "77525541"
---
# <a name="anomaly-detection-in-azure-stream-analytics"></a>Azure Stream Analytics 'de anomali algılama

Hem bulutta hem de Azure IoT Edge kullanılabilir Azure Stream Analytics, en yaygın olarak karşılaşılan iki anormali izlemek için kullanılabilecek yerleşik makine öğrenimi algılama özellikleri sunar: geçici ve kalıcı. **AnomalyDetection_SpikeAndDip** ve **AnomalyDetection_ChangePoint** işlevleriyle, doğrudan Stream Analytics işinizdeki anomali algılama gerçekleştirebilirsiniz.

Makine öğrenimi modelleri, tek bir örneklenmiş zaman serisini kabul eder. Zaman serisi Tekdüzen değilse, anomali algılama çağrısı yapmadan önce, atlayan bir pencereyle birlikte toplama adımı ekleyebilirsiniz.

Machine Learning işlemleri, şu anda mevsimsellik eğilimlerini veya çok değişkenli bir bağıntıları desteklemez.

## <a name="anomaly-detection-using-machine-learning-in-azure-stream-analytics"></a>Azure Stream Analytics makine öğrenimi kullanarak anomali algılama

Aşağıdaki videoda, Azure Stream Analytics ' deki makine öğrenimi işlevlerini kullanarak bir anomali 'in gerçek zamanlı olarak nasıl algılanacağı gösterilmektedir. 

> [!VIDEO https://channel9.msdn.com/Shows/Internet-of-Things-Show/Real-Time-ML-Based-Anomaly-Detection-In-Azure-Stream-Analytics/player]

## <a name="model-behavior"></a>Model davranışı

Genellikle, modelin doğruluğu, kayan pencerede daha fazla veri ile artar. Belirtilen kayan penceredeki veriler, bu zaman çerçevesi için normal değer aralığının bir parçası olarak değerlendirilir. Model, geçerli olayın anormal olup olmadığını denetlemek için yalnızca kayan pencere üzerinde olay geçmişini dikkate alır. Kayan pencere hareket ederken, eski değerler modelin eğitimine çıkarılmakta.

İşlevleri, şimdiye kadar gördüklerine göre belirli bir normal kurarak çalışır. Aykırı değerler, güvenilirlik düzeyi içinde, normal oluşturulan normalin karşılaştırılmasıyla tanımlanır. Pencere boyutu, bir anomali gerçekleştiğinde bunu tanıyabilmesi için modeli normal davranış için eğitme gereken en düşük olayları temel almalıdır.

Modelin yanıt süresi, daha yüksek bir geçmiş olay ile karşılaştırılabilmesi gerektiğinden geçmiş boyutuyla artar. Daha iyi performans için yalnızca gerekli sayıda olayı içermesi önerilir.

Zaman serisinde boşluklar, modelin belirli noktalarda olayları almamaya neden olabilir. Bu durum, imputation Logic kullanılarak Stream Analytics tarafından işlenir. Geçmiş boyutunun ve zaman süresinin yanı sıra, olayların gelmesi beklenen ortalama oranı hesaplamak için de aynı kayan pencere kullanılır.

[Burada](https://aka.ms/asaanomalygenerator) bulunan bir anomali Oluşturucu, farklı anomali desenlerine sahip verilerle bir IoT Hub 'ının akışını yapmak için kullanılabilir. Bu IoT Hub 'ından okumak ve anormallikleri algılamak için bu anomali algılama işlevleriyle bir ASA işi ayarlanabilir.

## <a name="spike-and-dip"></a>Ani ve DIP

Zaman serisi olay akışındaki geçici bozukluklar, ani artışlar ve DIB 'ler olarak bilinir. Ani artışlar ve DIB 'ler, [AnomalyDetection_SpikeAndDip](https://docs.microsoft.com/stream-analytics-query/anomalydetection-spikeanddip-azure-stream-analytics
)Machine Learning tabanlı işleç kullanılarak izlenebilir.

![Ani artış ve DIP anomali örneği](./media/stream-analytics-machine-learning-anomaly-detection/anomaly-detection-spike-dip.png)

Aynı kayan pencerede ikinci bir ani artış birinciden küçükse, daha küçük bir artış için hesaplanan puan büyük olasılıkla belirtilen güven düzeyindeki ilk ani puanla karşılaştırıldığında yeterince önemli değildir. Bu tür bozukluklar tespit etmek için modelin güven düzeyini azaltmayı deneyebilirsiniz. Ancak, çok fazla uyarı almaya başladıysanız daha yüksek güvenirlik aralığı kullanabilirsiniz.

Aşağıdaki örnek sorgu, 120 olay geçmişiyle 2 dakikalık bir kayan pencerede saniye başına bir etkinliğin tek bir giriş hızını varsayar. Son SELECT ifadesiyle puan ve anomali durumu, %95 güven düzeyine sahip ve çıktı veriyor.

```SQL
WITH AnomalyDetectionStep AS
(
    SELECT
        EVENTENQUEUEDUTCTIME AS time,
        CAST(temperature AS float) AS temp,
        AnomalyDetection_SpikeAndDip(CAST(temperature AS float), 95, 120, 'spikesanddips')
            OVER(LIMIT DURATION(second, 120)) AS SpikeAndDipScores
    FROM input
)
SELECT
    time,
    temp,
    CAST(GetRecordPropertyValue(SpikeAndDipScores, 'Score') AS float) AS
    SpikeAndDipScore,
    CAST(GetRecordPropertyValue(SpikeAndDipScores, 'IsAnomaly') AS bigint) AS
    IsSpikeAndDipAnomaly
INTO output
FROM AnomalyDetectionStep
```

## <a name="change-point"></a>Değişiklik noktası

Zaman serisi olay akışındaki kalıcı bozukluklar, olay akışındaki değerlerin dağıtımında, düzey değişiklikler ve eğilimler gibi değişiklikler. Stream Analytics, bu tür bozukluklar Machine Learning tabanlı [AnomalyDetection_ChangePoint](https://docs.microsoft.com/stream-analytics-query/anomalydetection-changepoint-azure-stream-analytics) işleci kullanılarak algılanır.

Kalıcı değişiklikler ani ve dallara göre çok daha uzun ve çok zararlı olay (ler) belirtebilir. Kalıcı değişiklikler genellikle çıplak göz için görünür değildir, ancak **AnomalyDetection_ChangePoint** işleci ile algılanabilir.

Aşağıdaki görüntü, bir düzey değişikliği örneğidir:

![Düzey değişim anomali örneği](./media/stream-analytics-machine-learning-anomaly-detection/anomaly-detection-level-change.png)

Aşağıdaki görüntü, bir eğilim değişikliği örneğidir:

![Eğilim değişikliği anomali örneği](./media/stream-analytics-machine-learning-anomaly-detection/anomaly-detection-trend-change.png)

Aşağıdaki örnek sorgu, 1200 olaylarının geçmiş boyutuyla 20 dakikalık bir kayan pencerede saniye başına bir olay için tek bir olayın Tekdüzen giriş hızını varsayar. Son SELECT ifadesiyle puan ve anomali durumu, %80 güven düzeyine sahip ve çıktı veriyor.

```SQL
WITH AnomalyDetectionStep AS
(
    SELECT
        EVENTENQUEUEDUTCTIME AS time,
        CAST(temperature AS float) AS temp,
        AnomalyDetection_ChangePoint(CAST(temperature AS float), 80, 1200) 
        OVER(LIMIT DURATION(minute, 20)) AS ChangePointScores
    FROM input
)
SELECT
    time,
    temp,
    CAST(GetRecordPropertyValue(ChangePointScores, 'Score') AS float) AS
    ChangePointScore,
    CAST(GetRecordPropertyValue(ChangePointScores, 'IsAnomaly') AS bigint) AS
    IsChangePointAnomaly
INTO output
FROM AnomalyDetectionStep

```

## <a name="performance-characteristics"></a>Performans özellikleri

Bu modellerin performansı, geçmiş boyutuna, pencere süresine, olay yüküne ve işlev düzeyi bölümlemenin kullanılıp kullanılmadığını belirtir. Bu bölümde bu yapılandırma açıklanmaktadır ve saniyede 1K, 5K ve 10.000 olay alma hızlarının nasıl ele alınacağını gösteren örnekler sağlanmaktadır.

* **Geçmiş boyutu** -bu modeller **Geçmiş boyutuyla**linerken gerçekleştirir. Geçmiş boyutu arttıkça, modeller yeni bir olayı puan etmek için daha uzun sürer. Bunun nedeni, modellerin yeni olayı geçmiş arabelleğindeki geçmiş olayların her biriyle karşılaştırmaktır.
* **Pencere süresi** - **pencere süresi** , geçmiş boyutuyla belirtilen sayıda olayı almak için geçen süreyi yansıtmalıdır. Pencerede bu çok sayıda olay olmadan, Azure Stream Analytics eksik değerleri bir değere sahip olur. Bu nedenle, CPU tüketimi geçmiş boyutunun bir işlevidir.
* **Olay yükü** -daha fazla **olay yükü**, model tarafından gerçekleştirilen ve CPU tüketimini etkileyen daha fazla çalışma. İş mantığı daha fazla giriş bölümü kullanmanın mantıklı olduğunu varsayarak, iş bu kadar paralel olarak genişletilebilir hale gelir.
* **İşlev düzeyi bölümleme** - **işlev düzeyi bölümlendirme** , anomali algılama ```PARTITION BY``` işlevi çağrısı içinde kullanılarak yapılır. Aynı anda birden çok modelde durum tutulması gerektiğinden, bu tür bölümleme bir ek yük ekler. İşlev düzeyi bölümleme, cihaz düzeyi bölümlendirme gibi senaryolarda kullanılır.

### <a name="relationship"></a>İlişki
Geçmiş boyutu, pencere süresi ve toplam olay yükü aşağıdaki şekilde ilişkilidir:

windowDuration (MS) = 1000 * geçmişini/(Toplam giriş olayı/sn/giriş bölümü sayısı)

İşlev DeviceID ile bölümlenirken, anomali algılama işlev çağrısına "PARTITION BY DeviceID" ekleyin.

### <a name="observations"></a>Lemin
Aşağıdaki tablo, bölümlenmemiş durum için tek bir düğüm (6 SU) için üretilen iş gözlemlerini içerir:

| Geçmiş boyutu (olaylar) | Pencere süresi (MS) | Saniyede toplam giriş olayı sayısı |
| --------------------- | -------------------- | -------------------------- |
| 60 | 55 | 2.200 |
| 600 | 728 | 1.650 |
| 6.000 | 10.910 | 1.100 |

Aşağıdaki tablo, bölümlenmiş durum için tek bir düğüm (6 SU) için üretilen iş gözlemlerini içerir:

| Geçmiş boyutu (olaylar) | Pencere süresi (MS) | Saniyede toplam giriş olayı sayısı | Cihaz sayımı |
| --------------------- | -------------------- | -------------------------- | ------------ |
| 60 | 1.091 | 1.100 | 10 |
| 600 | 10.910 | 1.100 | 10 |
| 6.000 | 218.182 | <550 | 10 |
| 60 | 21.819 | 550 | 100 |
| 600 | 218.182 | 550 | 100 |
| 6.000 | 2.181.819 | <550 | 100 |

Yukarıdaki bölümlenmemiş yapılandırmaların çalıştırıldığı örnek kod, Azure örneklerinin [Ölçek deposunda akışta](https://github.com/Azure-Samples/streaming-at-scale/blob/f3e66fa9d8c344df77a222812f89a99b7c27ef22/eventhubs-streamanalytics-eventhubs/anomalydetection/create-solution.sh) bulunur. Kod, giriş ve çıkış olarak olay hub 'ını kullanan işlev düzeyi bölümlendirme olmadan bir Stream Analytics işi oluşturur. Giriş yükü test istemcileri kullanılarak oluşturulur. Her giriş olayı bir 1KB JSON belgesidir. Olaylar, JSON verileri gönderen bir IoT cihazının benzetimini yapar (en fazla 1K cihaz için). Geçmiş boyutu, pencere süresi ve toplam olay yükü 2 ' den fazla giriş bölümü üzerinde farklılık gösterir.

> [!Note]
> Daha doğru bir tahmin için, örnekleri senaryonuza uyacak şekilde özelleştirin.

### <a name="identifying-bottlenecks"></a>Performans sorunlarını belirleme
İşlem hattınızdaki performans sorunlarını belirlemek için Azure Stream Analytics işinizin ölçümler bölmesini kullanın. İşin giriş oranına sahip olup olmadığını görmek için üretilen iş ve ["filigran gecikmesi"](https://azure.microsoft.com/blog/new-metric-in-azure-stream-analytics-tracks-latency-of-your-streaming-pipeline/) veya **biriktirme listesi olayları** Için **giriş/çıkış olaylarını** gözden geçirin. Olay Hub 'ı ölçümleri için, **Kısıtlanmış istekleri** bulun ve eşik birimlerini uygun şekilde ayarlayın. Cosmos DB ölçümleri için, bölüm anahtarı aralıklarınızın her bir şekilde tüketildiğinden emin olmak için üretilen Iş birimi **anahtar aralığı başına en fazla ru/sn** 'yi gözden geçirin. Azure SQL DB için **günlük GÇ** ve **CPU 'yu**izleyin.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Stream Analytics giriş](stream-analytics-introduction.md)
* [Azure Akış Analizi'ni kullanmaya başlama](stream-analytics-real-time-fraud-detection.md)
* [Azure Akış Analizi işlerini ölçeklendirme](stream-analytics-scale-jobs.md)
* [Azure Akış Analizi Sorgu Dili Başvurusu](https://docs.microsoft.com/stream-analytics-query/stream-analytics-query-language-reference)
* [Azure Akış Analizi Yönetimi REST API'si Başvurusu](https://msdn.microsoft.com/library/azure/dn835031.aspx)

