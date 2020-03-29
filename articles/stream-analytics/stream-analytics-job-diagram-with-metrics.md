---
title: Azure Akış Analizi'nde veri tabanlı hata ayıklama
description: Bu makalede, Azure portalındaki iş diyagramını ve ölçümleri kullanarak Azure Akış Analizi işinizi nasıl gideriniz açıklanmaktadır.
author: jseb225
ms.author: jeanb
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 05/01/2017
ms.openlocfilehash: 3c0c29e1793e56efae8d13cb01d57faf257d8805
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75426067"
---
# <a name="data-driven-debugging-by-using-the-job-diagram"></a>İş diyagramını kullanarak veri odaklı hata ayıklama

Azure portalındaki **İzleme** bıçağındaki iş diyagramı, iş ardınızı görselleştirmenize yardımcı olabilir. Girişleri, çıkışları ve sorgu adımlarını gösterir. İş diyagramını kullanarak her adımda ölçümleri inceleyebilir, sorunları giderirken sorunun kaynağını daha hızlı yalıtabilirsiniz.

## <a name="using-the-job-diagram"></a>İş diyagramını kullanma

Azure portalında, Bir Akış Analizi işinde, **SUPPORT + TROUBLESHOOTING**altında, **İş diyagramını**seçin:

![Ölçümleri ile iş diyagramı - konum](./media/stream-analytics-job-diagram-with-metrics/stream-analytics-job-diagram-with-metrics-portal-1.png)

Sorgu düzenleme bölmesinde ilgili bölümü görmek için her sorgu adımını seçin. Adım için bir metrik grafik sayfadaki alt bölmede görüntülenir.

![Ölçümleri ile iş diyagramı - temel iş](./media/stream-analytics-job-diagram-with-metrics/stream-analytics-job-diagram-with-metrics-portal-2.png)

Azure Etkinlik Hub'ları girişinin bölümlerini görmek için **. . .** Bağlam menüsü görüntülenir. Giriş birleşmesi de görebilirsiniz.

![Ölçümlerle iş diyagramı - bölümü genişletme](./media/stream-analytics-job-diagram-with-metrics/stream-analytics-job-diagram-with-metrics-portal-3.png)

Yalnızca tek bir bölüm için metrik grafiği görmek için bölüm düğümünü seçin. Ölçümler sayfanın alt kısmında gösterilir.

![Ölçümlerle iş diyagramı - daha fazla ölçüm](./media/stream-analytics-job-diagram-with-metrics/stream-analytics-job-diagram-with-metrics-portal-4.png)

Birbirlemenin ölçüm tablosunu görmek için birleşme düğümünü seçin. Aşağıdaki grafik, hiçbir olayın düşürülmediğini veya ayarlanmadığını gösterir.

![Ölçümleri ile iş diyagramı - ızgara](./media/stream-analytics-job-diagram-with-metrics/stream-analytics-job-diagram-with-metrics-portal-5.png)

Metrik değer ve zamanın ayrıntılarını görmek için grafiğe işaret edin.

![Ölçümleri ile iş diyagramı - hover](./media/stream-analytics-job-diagram-with-metrics/stream-analytics-job-diagram-with-metrics-portal-6.png)

## <a name="troubleshoot-by-using-metrics"></a>Ölçümleri kullanarak sorun giderme

**QueryLastProcessedTime** ölçümü, belirli bir adımın ne zaman veri aldığını gösterir. Topolojiye bakarak, hangi adımın veri almadığını görmek için çıktı işlemcisinden geriye doğru çalışabilirsiniz. Bir adım veri almuyorsa, ondan hemen önce sorgu adımına gidin. Önceki sorgu adımının bir zaman penceresi olup olmadığını ve çıktı verileri için yeterli zaman geçip geçmediğini denetleyin. (Zaman pencerelerinin saate tutturulmuş olduğunu unutmayın.)
 
Önceki sorgu adımı bir giriş işlemcisiyse, aşağıdaki hedeflenen soruları yanıtlamaya yardımcı olmak için giriş ölçümlerini kullanın. Bunlar, bir işin giriş kaynaklarından veri alıp almadığını belirlemenize yardımcı olabilir. Sorgu bölümlendirilmişse her bir bölümü inceleyin.
 
### <a name="how-much-data-is-being-read"></a>Ne kadar veri okunuyor?

*   **InputEventsSourcesTotal** okunan veri birimlerinin sayısıdır. Örneğin, lekelerin sayısı.
*   **InputEventsTotal** okunan olay sayısıdır. Bu ölçüm her bölüm için kullanılabilir.
*   **InputEventsInBytesTotal** okunan bayt sayısıdır.
*   **InputEventsLastArrivalTime** alınan her etkinliğin enqueued zamanı ile güncellenir.
 
### <a name="is-time-moving-forward-if-actual-events-are-read-punctuation-might-not-be-issued"></a>Zaman ilerliyor mu? Gerçek olaylar okunuyorsa noktalama işaretleri verilmeyebilir.

*   **InputEventsLastPunctuationTime**, zamanın ilerlemesini sağlamak için bir noktalama işaretinin ne zaman verildiğini gösterir. Noktalama işaretleri verilmezse, veri akışı engellenebilir.
 
### <a name="are-there-any-errors-in-the-input"></a>Girişte herhangi bir hata var mı?

*   **InputEventsEventDataNullTotal,** null veriye sahip olayların sayısıdır.
*   **InputEventsSerializerErrorsTotal,** doğru şekilde deserialediyonu yapılamayan olayların sayısıdır.
*   **InputEventsDegradedTotal,** deserialization dışında bir sorunu olan olayların sayısıdır.
 
### <a name="are-events-being-dropped-or-adjusted"></a>Olaylar bırakılıyor mu veya ayarlanıyor mu?

*   **InputEventsEarlyTotal,** yüksek filigrandan önce uygulama zaman damgası olan olayların sayısıdır.
*   **InputEventsLateTotal,** yüksek filigrandan sonra uygulama zaman damgası olan olayların sayısıdır.
*   **InputEventsDroppedBeforeApplicationStartTimeTotal,** iş başlangıç saatinden önce bırakılan sayı olaylarıdır.
 
### <a name="are-we-falling-behind-in-reading-data"></a>Verileri okumada geride mi kalıyoruz?

*   **Giriş Olayları Backlogged (Toplam),** Olay Hub'ları ve Azure IoT Hub girişleri için daha kaç iletinin okunması gerektiğini söyler. Bu sayı 0'dan büyükolduğunda, işinizin verileri geldiği kadar hızlı işleyebileceği anlamına gelir. Bu durumda Akış Birimi sayısını artırmanız ve/veya işinizin paralelleştirilediğinden emin olmanız gerekebilir. Bu konuda daha fazla bilgi sorgu [paralelleştirme sayfasında](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-parallelization)görebilirsiniz. 


## <a name="get-help"></a>Yardım alın
Ek yardım için [Azure Akışı Analizi forumumuzu](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics)deneyin. 

## <a name="next-steps"></a>Sonraki adımlar
* [Akış Analizine Giriş](stream-analytics-introduction.md)
* [Akış Analizi ile çalışmaya başlama](stream-analytics-real-time-fraud-detection.md)
* [Stream Analytics işlerini ölçeklendirme](stream-analytics-scale-jobs.md)
* [Akış Analizi sorgu dili başvurusu](https://docs.microsoft.com/stream-analytics-query/stream-analytics-query-language-reference)
* [Stream Analytics yönetimi REST API referansı](https://msdn.microsoft.com/library/azure/dn835031.aspx)
