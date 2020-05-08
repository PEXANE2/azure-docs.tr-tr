---
title: Azure Stream Analytics 'de iş izlemeyi anlama
description: Bu makalede, Azure portal Azure Stream Analytics işlerin nasıl izleneceği açıklanır.
author: mamccrea
ms.author: mamccrea
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 06/21/2018
ms.custom: seodec18
ms.openlocfilehash: 54bff88e9650240a3703e18d583f603cafeb3022
ms.sourcegitcommit: 50ef5c2798da04cf746181fbfa3253fca366feaa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2020
ms.locfileid: "82611900"
---
# <a name="understand-stream-analytics-job-monitoring-and-how-to-monitor-queries"></a>Stream Analytics iş izlemeyi ve sorguların nasıl izleneceğini anlayın

## <a name="introduction-the-monitor-page"></a>Giriş: izleyici sayfası
Azure portal, sorgunuzu ve iş performansınızı izlemek ve sorunlarını gidermek için kullanılabilen temel performans ölçümlerini yüzeyler. Bu ölçümleri görmek için ölçümler ' i görmek ve Genel Bakış sayfasındaki **izleme** bölümünü görüntülemek için ilgilendiğiniz Stream Analytics işe göz atın.  

![Stream Analytics iş izleme bağlantısı](./media/stream-analytics-monitoring/02-stream-analytics-monitoring-block.png)

Bu pencere gösterildiği gibi görünür:

![Stream Analytics iş izleme panosu](./media/stream-analytics-monitoring/01-stream-analytics-monitoring.png)  

## <a name="metrics-available-for-stream-analytics"></a>Stream Analytics için kullanılabilir ölçümler
| Ölçüm                 | Tanım                               |
| ---------------------- | ---------------------------------------- |
| Biriktirme listesindeki giriş olayları       | Biriktirme listesindeki giriş olaylarının sayısı. Bu ölçüm için sıfır olmayan bir değer, işinizin gelen olay sayısıyla devam edemediğinden emin olmak anlamına gelir. Bu değer yavaş bir şekilde artarak veya sürekli olarak sıfır değilse, işinizi ölçeklendirmelisiniz. Daha fazla bilgi edinmek için öğrenme [ve akış birimlerini ayarlayın](stream-analytics-streaming-unit-consumption.md). |
| Veri dönüştürme hataları | Beklenen çıkış şemasına dönüştürülemeyen çıkış olaylarının sayısı. Hata ilkesi, bu senaryoya ilişkin olayları bırakmak için ' Drop ' olarak değiştirilebilir. |
| Erken giriş olayları       | Uygulama zaman damgası, 5 dakikadan uzun bir süre içinde olan olay zaman damgasından daha önce. |
| Başarısız Işlev Istekleri | Başarısız Azure Machine Learning işlev çağrılarının sayısı (varsa). |
| İşlev olayları        | Azure Machine Learning işlevine gönderilen olay sayısı (varsa). |
| İşlev Istekleri      | Azure Machine Learning işlevine yapılan çağrıların sayısı (varsa). |
| Giriş seri kaldırma hataları       | Seri durumdan çıkarılabilen giriş olaylarının sayısı.  |
| Olay baytlarını gir      | Stream Analytics işi tarafından bayt cinsinden alınan veri miktarı. Bu, olayların giriş kaynağına gönderildiğini doğrulamak için kullanılabilir. |
| Giriş olayları           | Giriş olaylarından Serisi kaldırılan kayıt sayısı. Bu sayı, hata serisini kaldırma ile sonuçlanan gelen olayları içermez. Aynı olaylar, iç kurtarmalar ve kendinden birleşimler gibi senaryolarda birden çok kez Stream Analytics alınabilir. Bu nedenle, işinizin basit bir ' pass-through ' sorgusu varsa giriş olaylarının ve çıkış olayları ölçümlerinin eşleşmesi beklenmemelidir. |
| Alınan giriş kaynakları       | İş tarafından alınan ileti sayısı. Olay Hub 'ı için, bir ileti tek bir EventData ' dır. Blob için, bir ileti tek bir Blobun olur. Giriş kaynaklarının seri durumundan önce sayıldığını lütfen unutmayın. Seri kaldırma hataları varsa, giriş kaynakları giriş olaylarından daha büyük olabilir. Aksi halde, her ileti birden çok olay içerebileceğinden, bu, giriş olayından daha az veya eşit olabilir. |
| Geç giriş olayları      | Yapılandırılan geç gelme toleransı penceresinden daha sonra gelen olaylar. [Azure Stream Analytics olay sırası konuları](stream-analytics-out-of-order-and-late-events.md) hakkında daha fazla bilgi edinin. |
| Sıra dışı olaylar    | Olay sıralama Ilkesine bağlı olarak, ayarlanmış bir zaman damgasına bırakılan veya verilen zaman damgası dışında alınan olay sayısı. Bu, sipariş dışı tolerans penceresi ayarının yapılandırmasından etkilenebilir. |
| Çıkış olayları          | Stream Analytics işi tarafından çıktı hedefine, olay sayısında gönderilen veri miktarı. |
| Çalışma zamanı hataları         | Sorgu işlemeyle ilgili toplam hata sayısı (olayları alırken veya sonuçları yazarken bulunan hatalar hariç) |
| SU kullanımı yüzdesi       | İşin ölçek sekmesinden bir işe atanan akış birimlerinin kullanımı. Bu gösterge %80 veya üzeri bir duruma ulaşılmalıdır, olay işlemenin ilerleme durumunu geciktirebileceği veya durmuş olabileceği büyük bir olasılık vardır. |
| Filigran gecikmesi       | İş içindeki tüm çıkışların tüm bölümlerinde en fazla eşik gecikmesi. |

[Stream Analytics işinizin performansını izlemek](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-set-up-alerts#scenarios-to-monitor)için bu ölçümleri kullanabilirsiniz. 

## <a name="customizing-monitoring-in-the-azure-portal"></a>Azure portal Izlemeyi özelleştirme
Grafik türünü, gösterilen ölçümleri ve zaman aralığını düzenleme grafiği ayarları ' nda ayarlayabilirsiniz. Ayrıntılar için bkz. [Izlemeyi özelleştirme](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md).

  ![Sorgu İzleyicisi zaman grafiğini Stream Analytics](./media/stream-analytics-monitoring/08-stream-analytics-monitoring.png)  


## <a name="latest-output"></a>En son çıkış
İşinizi izlemeye yönelik başka bir ilgi çekici veri noktası, Genel Bakış sayfasında gösterilen son çıkışın zamandır.
Bu süre, işinizin en son çıktısının uygulama zamanı (örneğin, olay verilerinden zaman damgası kullanılarak).

## <a name="get-help"></a>Yardım alın
Daha fazla yardım için [Azure Stream Analytics forumumuzu](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics) deneyin.

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Stream Analytics giriş](stream-analytics-introduction.md)
* [Azure Akış Analizi'ni kullanmaya başlama](stream-analytics-real-time-fraud-detection.md)
* [Azure Akış Analizi işlerini ölçeklendirme](stream-analytics-scale-jobs.md)
* [Azure Akış Analizi Sorgu Dili Başvurusu](https://docs.microsoft.com/stream-analytics-query/stream-analytics-query-language-reference)
* [Azure Akış Analizi Yönetimi REST API'si Başvurusu](https://msdn.microsoft.com/library/azure/dn835031.aspx)
