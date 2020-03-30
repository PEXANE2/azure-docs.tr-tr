---
title: Azure Akış Analizi'nde anormallik algılaması
description: Bu makalede, anormallikleri algılamak için Azure Akış Analizi ve Azure Machine Learning'in birlikte nasıl kullanılacağı açıklanmaktadır.
author: mamccrea
ms.author: mamccrea
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 06/21/2019
ms.openlocfilehash: 51b9c827d453eef2e2e75e1aa5222204eaa38d0e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77525541"
---
# <a name="anomaly-detection-in-azure-stream-analytics"></a>Azure Akış Analizi'nde anormallik algılaması

Hem bulutta hem de Azure IoT Edge'de kullanılabilen Azure Stream Analytics, en sık görülen iki anomaliyi izlemek için kullanılabilecek yerleşik makine öğrenimi tabanlı anormallik algılama yetenekleri sunar: geçici ve kalıcı. **AnomalyDetection_SpikeAndDip** ve **AnomalyDetection_ChangePoint** işlevleri yle, doğrudan Stream Analytics işinizde anormallik algılama gerçekleştirebilirsiniz.

Makine öğrenme modelleri düzgün örneklenmiş zaman serisi varsayar. Zaman serisi düzgün değilse, anomali algılamayı çağırmadan önce yuvarlanan pencereye bir toplama adımı ekleyebilirsiniz.

Makine öğrenimi işlemleri şu anda mevsimsellik eğilimlerini veya çok değişkenli korelasyonları desteklemez.

## <a name="anomaly-detection-using-machine-learning-in-azure-stream-analytics"></a>Azure Akış Analizi'nde makine öğrenimini kullanarak anormallik algılama

Aşağıdaki video, Azure Akış Analizi'nde makine öğrenimi işlevlerini kullanarak bir anomalinin gerçek zamanlı olarak nasıl algılanış edilebildiğini göstermektedir. 

> [!VIDEO https://channel9.msdn.com/Shows/Internet-of-Things-Show/Real-Time-ML-Based-Anomaly-Detection-In-Azure-Stream-Analytics/player]

## <a name="model-behavior"></a>Model davranışı

Genellikle, modelin doğruluğu kayar pencerede daha fazla veri ile geliştirir. Belirtilen kayar penceredeki veriler, o zaman dilimi için normal değer aralığının bir parçası olarak kabul edilir. Model, geçerli olayın anormal olup olmadığını kontrol etmek için yalnızca kayan pencere üzerindeki olay geçmişini dikkate alır. Kayan pencere hareket ettikçe, eski değerler modelin eğitiminden çıkarılır.

Fonksiyonlar, şimdiye kadar gördüklerine göre belirli bir normal oluşturarak çalışır. Outliers, güven düzeyi içinde, belirlenen normalle karşılaştırılarak tanımlanır. Pencere boyutu, modeli normal davranış için eğitmek için gereken minimum olaylara dayanmalıdır, böylece bir anormallik meydana geldiğinde, modeli tanıyabilir.

Modelin yanıt süresi geçmiş olaylarla karşılaştırıldığında gerektiğinden geçmiş boyutuyla artar. Yalnızca daha iyi performans için gerekli sayıda olayın dahil olması önerilir.

Zaman serisindeki boşluklar, modelin belirli noktalarda zaman içinde olay alamadığının bir sonucu olabilir. Bu durum, akış analizi mantığı kullanılarak ele alınır. Aynı kayar pencere için geçmiş boyutu ve bir zaman süresi, olayların gelmesi beklenen ortalama oranı hesaplamak için kullanılır.

[Burada](https://aka.ms/asaanomalygenerator) bulunan bir anomali jeneratörü, iot hub'ı farklı anomali desenli verilerle beslemek için kullanılabilir. Bu Iot Hub'dan okumak ve anomalileri tespit etmek için bu anomali algılama işlevleri ile bir ASA işi kurulabilir.

## <a name="spike-and-dip"></a>Başak ve daldırma

Bir zaman serisi olay akışındaki geçici anomaliler ani artışlar ve dipler olarak bilinir. Ani artışlar ve dips Makine Öğrenme tabanlı operatör, [AnomalyDetection_SpikeAndDip](https://docs.microsoft.com/stream-analytics-query/anomalydetection-spikeanddip-azure-stream-analytics
)kullanılarak izlenebilir.

![Başak ve dip anomalisi örneği](./media/stream-analytics-machine-learning-anomaly-detection/anomaly-detection-spike-dip.png)

Aynı kayar pencerede, ikinci bir başak ilkinden daha küçükse, daha küçük başak için hesaplanan skor muhtemelen belirtilen güven düzeyi içindeki ilk başak skoruna göre yeterince önemli değildir. Bu tür anormallikleri tespit etmek için modelin güven düzeyini düşürmeyi deneyebilirsiniz. Ancak, çok fazla uyarı almaya başlarsanız, daha yüksek bir güven aralığı kullanabilirsiniz.

Aşağıdaki örnek sorgu, 120 olay geçmişi olan 2 dakikalık bir kayar pencerede saniyede bir olayın tek düze giriş hızını varsayar. Son SELECT deyimi% 95 güven düzeyi ile puan ve anomali durumu ayıklar ve çıktıları.

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

## <a name="change-point"></a>Değiştirme noktası

Bir zaman serisi olay akışındaki kalıcı anomaliler, düzey değişiklikleri ve eğilimler gibi olay akışındaki değerlerin dağılımındaki değişikliklerdir. Stream Analytics'te bu tür anomaliler Machine Learning tabanlı [AnomalyDetection_ChangePoint](https://docs.microsoft.com/stream-analytics-query/anomalydetection-changepoint-azure-stream-analytics) operatörü kullanılarak tespit edilir.

Kalıcı değişiklikler ani ve diplerden çok daha uzun sürer ve yıkıcı bir olay(lar) gösterebilir. Kalıcı değişiklikler genellikle çıplak gözle görülemez, ancak **AnomalyDetection_ChangePoint** işleci ile algılanabilir.

Aşağıdaki resim bir düzey değişikliğine örnektir:

![Düzey değişikliği anomalisi örneği](./media/stream-analytics-machine-learning-anomaly-detection/anomaly-detection-level-change.png)

Aşağıdaki resim bir eğilim değişikliğine örnektir:

![Eğilim değişikliği anomalisi örneği](./media/stream-analytics-machine-learning-anomaly-detection/anomaly-detection-trend-change.png)

Aşağıdaki örnek sorgu, 1200 olay geçmişi olan 20 dakikalık bir kayar pencerede saniyede bir olayın tek düze giriş hızını varsayar. Son SELECT deyimi% 80 güven düzeyi ile puan ve anomali durumunu ayıklar ve çıktıları.

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

Bu modellerin performansı geçmiş boyutuna, pencere süresine, olay yüküne ve işlev düzeyi bölümlemenin kullanılıp kullanılmadığına bağlıdır. Bu bölümde bu yapılandırmalar açıklanır ve saniyede 1K, 5K ve 10K olaylarının alım oranlarını nasıl sürdüreceklerine yönelik örnekler sağlar.

* **Geçmiş boyutu** - Bu modeller **tarih boyutu**ile doğrusal gerçekleştirmek. Geçmiş boyutu ne kadar uzunsa, modellerin yeni bir olay almasına o kadar uzun zaman var. Bunun nedeni, modellerin yeni olayı geçmiş arabellekteki geçmiş olayların her biriyle karşılaştırmasıdır.
* **Pencere süresi** - **Pencere süresi,** geçmiş boyutuna göre belirtildiği kadar çok olay almanın ne kadar sürdüğünü yansıtmalıdır. Pencerede bu kadar çok olay olmadan, Azure Akış Analizi eksik değerleri ortaya koyar. Bu nedenle, CPU tüketimi geçmiş boyutunun bir fonksiyonudur.
* **Olay yükü** - **Olay yükü**ne kadar büyükse, modeller tarafından gerçekleştirilen ve CPU tüketimini etkileyen daha fazla çalışma. İş mantığının daha fazla giriş bölümü kullanmasının mantıklı olduğunu varsayarak, iş utanç verici bir şekilde paralel hale getirilerek iş ölçeklendirilebilir.
* **Fonksiyon düzeyi bölümleme** - **Fonksiyon düzeyi bölümleme** anomali algılama fonksiyonu içinde call kullanılarak ```PARTITION BY``` yapılır. Durum aynı anda birden çok model için tutulması gerektiğinden, bu tür bir bölümleme ek yükü ekler. Aygıt düzeyi bölümleme gibi senaryolarda işlev düzeyi bölümleme kullanılır.

### <a name="relationship"></a>İlişki
Geçmiş boyutu, pencere süresi ve toplam olay yükü aşağıdaki şekilde ilişkilidir:

windowDuration (ms olarak) = 1000 * historySize / (Toplam Giriş Olayları Başına Sec / Giriş Bölüm Sayısı)

İşlevin deviceId ile bölümlenirken, anomali algılama işlevi çağrısına "DEVICEId ILE BÖLÜMLEME" ekleyin.

### <a name="observations"></a>Gözlem
Aşağıdaki tablo, bölümlenmemiş durum için tek bir düğüm (6 SU) için iş ortası gözlemlerini içerir:

| Geçmiş boyutu (olaylar) | Pencere süresi (ms) | Her saniyelik toplam giriş olayları |
| --------------------- | -------------------- | -------------------------- |
| 60 | 55 | 2.200 |
| 600 | 728 | 1,650 |
| 6.000 | 10,910 | 1.100 |

Aşağıdaki tablo, bölümlenmiş servis talebi için tek bir düğüm (6 SU) için iş bölümü gözlemlerini içerir:

| Geçmiş boyutu (olaylar) | Pencere süresi (ms) | Her saniyelik toplam giriş olayları | Cihaz sayımı |
| --------------------- | -------------------- | -------------------------- | ------------ |
| 60 | 1,091 | 1.100 | 10 |
| 600 | 10,910 | 1.100 | 10 |
| 6.000 | 218,182 | <550 | 10 |
| 60 | 21,819 | 550 | 100 |
| 600 | 218,182 | 550 | 100 |
| 6.000 | 2,181,819 | <550 | 100 |

Yukarıdaki bölümlenmemiş yapılandırmaları çalıştırmak için örnek kod, Azure Örneklerinin [Ölçek düzeyinde akış repo'sunda](https://github.com/Azure-Samples/streaming-at-scale/blob/f3e66fa9d8c344df77a222812f89a99b7c27ef22/eventhubs-streamanalytics-eventhubs/anomalydetection/create-solution.sh) bulunur. Kod, olay hub'ını giriş ve çıktı olarak kullanan işlev düzeyi bölümlemi olmayan bir akış analizi işi oluşturur. Giriş yükü test istemcileri kullanılarak oluşturulur. Her giriş olayı bir 1KB json belgesidir. Olaylar, JSON verileri gönderen bir IoT aygıtını simüle eder (1K'ya kadar aygıtlar için). Geçmiş boyutu, pencere süresi ve toplam olay yükü 2 giriş bölümü üzerinde değişir.

> [!Note]
> Daha doğru bir tahmin için, örnekleri senaryonuza uyacak şekilde özelleştirin.

### <a name="identifying-bottlenecks"></a>Darboğazları tanımlama
Ardışık hattınızdaki darboğazları belirlemek için Azure Akış Analizi işinizdeki Ölçümler bölmesini kullanın. İş giriş hızına ayak uydurup uymadın mı görmek için **giriş/Çıkış Olaylarını** ve ["Filigran Gecikmesi"](https://azure.microsoft.com/blog/new-metric-in-azure-stream-analytics-tracks-latency-of-your-streaming-pipeline/) veya **Backlogged Olayları** gözden geçirin. Olay Hub ölçümleri **için, Daraltılmış İstekleri** arayın ve Eşik Birimlerini buna göre ayarlayın. Cosmos DB ölçümleri için, bölüm anahtar aralıklarınızın eşit şekilde tüketilmesini sağlamak için İşlem altında **bölüm başına RU/s tüketilen Max'i** gözden geçirin. Azure SQL DB için **Log IO** ve **CPU'yı**izleyin.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Akış Analizine Giriş](stream-analytics-introduction.md)
* [Azure Akış Analizi'ni kullanmaya başlama](stream-analytics-real-time-fraud-detection.md)
* [Azure Akış Analizi işlerini ölçeklendirme](stream-analytics-scale-jobs.md)
* [Azure Akış Analizi Sorgu Dili Başvurusu](https://docs.microsoft.com/stream-analytics-query/stream-analytics-query-language-reference)
* [Azure Akış Analizi Yönetimi REST API'si Başvurusu](https://msdn.microsoft.com/library/azure/dn835031.aspx)

