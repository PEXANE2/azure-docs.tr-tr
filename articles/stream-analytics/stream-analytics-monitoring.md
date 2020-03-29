---
title: Azure Akış Analitiği'nde iş izlemeyi anlama
description: Bu makalede, Azure portalındaki Azure Akış Analizi işlerinin nasıl izlendiği açıklanmaktadır.
author: mamccrea
ms.author: mamccrea
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 06/21/2018
ms.custom: seodec18
ms.openlocfilehash: 4e9f90035816269d2d41781be34d0d8080628b12
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75431646"
---
# <a name="understand-stream-analytics-job-monitoring-and-how-to-monitor-queries"></a>Akış Analizi iş izlemeyi ve sorguları nasıl izleyebilirsiniz anlayın

## <a name="introduction-the-monitor-page"></a>Giriş: Monitör sayfası
Azure portalı, sorgunuzu ve iş performansınızı izlemek ve sorun gidermek için kullanılabilecek önemli performans ölçümlerini yüzeyler. Bu ölçümleri görmek için, Özet sayfada İzleme **bölümünü** görmek istediğiniz Akış Analizi işine göz atın ve ölçümleri görüntüleyin.  

![Stream Analytics iş izleme bağlantısı](./media/stream-analytics-monitoring/02-stream-analytics-monitoring-block.png)

Pencere gösterildiği gibi görünür:

![Stream Analytics iş izleme panosu](./media/stream-analytics-monitoring/01-stream-analytics-monitoring.png)  

## <a name="metrics-available-for-stream-analytics"></a>Akış Analizi için kullanılabilir ölçümler
| Ölçüm                 | Tanım                               |
| ---------------------- | ---------------------------------------- |
| Backlogged Giriş Olayları       | Backlogged giriş olaylarının sayısı. Bu ölçüm için sıfır olmayan bir değer, işinizin gelen olayların sayısına ayak uyduramadığını gösterir. Bu değer yavaş yavaş artıyorsa veya sürekli olarak sıfıra aykırıysa, işinizi ölçeklendirmeniz gerekir. [Akış Birimlerini Anla'yı](stream-analytics-streaming-unit-consumption.md)ziyaret ederek daha fazla bilgi edinebilir ve ayarlayabilirsiniz. |
| Veri Dönüştürme Hataları | Beklenen çıktı şemasına dönüştürülmeyen çıktı olaylarının sayısı. Hata ilkesi, bu senaryoyla karşılaşan olayları bırakmak için 'Bırak' olarak değiştirilebilir. |
| Erken Giriş Olayları       | Başvuru süresi 5 dakikadan daha erken olan etkinlikler. |
| Başarısız Fonksiyon İstekleri | Başarısız Azure Machine Learning işlev çağrılarının sayısı (varsa). |
| Fonksiyon Etkinlikleri        | Azure Machine Learning işlevine gönderilen olay sayısı (varsa). |
| Fonksiyon İstekleri      | Azure Machine Learning işlevine yapılan çağrı sayısı (varsa). |
| Giriş Deserialization Hataları       | Seri olarak deserialize edilemeyen giriş olaylarının sayısı.  |
| Giriş Olay Bayt      | Akış Analizi işi tarafından alınan veri miktarı, baytlar halinde. Bu, olayların giriş kaynağına gönderildiğini doğrulamak için kullanılabilir. |
| Giriş Olayları           | Giriş olaylarından deserialized kayıtların sayısı. Bu sayı, deserialization hataları neden gelen olayları içermez. |
| Alınan Giriş Kaynakları       | İş tarafından alınan ileti sayısı. Olay Hub'ı için ileti tek bir EventData'dır. Blob için ileti tek bir damladır. Giriş Kaynaklarının deserialization'dan önce sayıldığını lütfen unutmayın. Deserialization hataları varsa, giriş kaynakları giriş olayları daha büyük olabilir. Aksi takdirde, her ileti birden çok olay içerebildiği için giriş olaylarını daha az veya eşit olabilir. |
| Geç Giriş Olayları      | Yapılandırılan geç varış tolerans penceresinden daha sonra gelen olaylar. [Azure Akışı Analizi etkinlik siparişi hususları](stream-analytics-out-of-order-and-late-events.md) hakkında daha fazla bilgi edinin. |
| Sıra dışı Etkinlikler    | Olay Sipariş İlkesi'ni temel alan, sıra dışında alınan veya ayarlanmış bir zaman damgası verilen olay sayısı. Bu, Sıra Dışı Tolerans Penceresi ayarınınin yapılandırması tarafından etkilenebilir. |
| Çıktı Olayları          | Akış Analizi işi tarafından çıktı hedefine gönderilen veri miktarı, olay sayısı. |
| Çalışma Zamanı Hataları         | Sorgu işlemeyle ilgili toplam hata sayısı (olayları sindirirken veya sonuç çıkarırken bulunan hatalar hariç) |
| SU % Kullanım       | Bir işe atanan Akış Birimi(ler)in kullanımı, işin Ölçek sekmesinden. Bu göstergenin %80 veya üzerinde olması durumunda, olay işlemenin gecikme veya ilerleme nin durdurulma olasılığı yüksektir. |
| Filigran Gecikmesi       | İşdeki tüm çıktıların tüm bölümleri arasında maksimum filigran gecikmesi. |

[Bu ölçümleri, Akış Analizi işinizin performansını izlemek](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-set-up-alerts#scenarios-to-monitor)için kullanabilirsiniz. 

## <a name="customizing-monitoring-in-the-azure-portal"></a>Azure portalında İzlemeyi Özelleştirme
Grafik türünü, gösterilen ölçümleri ve zaman aralığını Grafik Ayarları'nı Edit ayarlarında ayarlayabilirsiniz. Ayrıntılar [için, İzlemeyi Özelleştirme ye bakın.](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md)

  ![Akış Analizi sorgu izleme zaman grafiği](./media/stream-analytics-monitoring/08-stream-analytics-monitoring.png)  


## <a name="latest-output"></a>En son çıktı
İşinizi izlemek için bir diğer ilginç veri noktası da Genel Bakış sayfasında gösterilen son çıktının zamanıdır.
Bu süre, işinizin en son çıktısının uygulama süresidir (yani olay verilerinden zaman damgasını kullanma süresi).

## <a name="get-help"></a>Yardım alın
Daha fazla yardım için [Azure Stream Analytics forumumuzu](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics) deneyin.

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Akış Analizine Giriş](stream-analytics-introduction.md)
* [Azure Akış Analizi'ni kullanmaya başlama](stream-analytics-real-time-fraud-detection.md)
* [Azure Akış Analizi işlerini ölçeklendirme](stream-analytics-scale-jobs.md)
* [Azure Akış Analizi Sorgu Dili Başvurusu](https://docs.microsoft.com/stream-analytics-query/stream-analytics-query-language-reference)
* [Azure Akış Analizi Yönetimi REST API'si Başvurusu](https://msdn.microsoft.com/library/azure/dn835031.aspx)
