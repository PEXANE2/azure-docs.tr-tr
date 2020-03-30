---
title: Azure Akış Analizi işlerinde ölçekleme ve çıkış
description: Bu makalede, giriş verilerini bölümlere alarak, sorguyu ayarlayarak ve iş akışı birimlerini ayarlayarak Akış Analizi işini nasıl ölçeklendirebilirsiniz.
author: JSeb225
ms.author: jeanb
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 06/22/2017
ms.openlocfilehash: d828103bef8e57f5d0cdfe6c243c52e2d0526663
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80257555"
---
# <a name="scale-an-azure-stream-analytics-job-to-increase-throughput"></a>İş verisini artırmak için Azure Akış Analizi işini ölçeklendirin
Bu makalede, Akış Analizi işlerinin iş akışını artırmak için bir Akış Analizi sorgusunun nasıl ayarlıtaklış edilebilmektedir. Daha yüksek yükü işlemek ve daha fazla sistem kaynağından yararlanmak için işinizi ölçeklendirmek için aşağıdaki kılavuzu kullanabilirsiniz (daha fazla bant genişliği, daha fazla CPU kaynağı, daha fazla bellek gibi).
Ön koşul olarak, aşağıdaki makaleleri okumanız gerekebilir:
-   [Akış Birimlerini anlama ve ayarlama](stream-analytics-streaming-unit-consumption.md)
-   [Paralelleştirilebilir işler oluşturma](stream-analytics-parallelization.md)

## <a name="case-1--your-query-is-inherently-fully-parallelizable-across-input-partitions"></a>Örnek 1 – Sorgunuz giriş bölümleri arasında doğal olarak tamamen paralellik lenebilir
Sorgunuz giriş bölümleri arasında doğal olarak tamamen paralellik sağlayabilirse, aşağıdaki adımları izleyebilirsiniz:
1.  Partition **BY** anahtar sözcük kullanarak sorgunuzu utanç verici bir şekilde paralel olacak şekilde yazar. [Bu sayfadaki](stream-analytics-parallelization.md)Utanç Verici paralel işler bölümünde daha fazla ayrıntıya bakın.
2.  Sorgunuzda kullanılan çıktı türlerine bağlı olarak, bazı çıktılar paralelleştirilebilir olmayabilir veya utanç verici derecede paralel olması için daha fazla yapılandırma gerekebilir. Örneğin, PowerBI çıkışı paralelleştirilebilir değildir. Çıktılar her zaman çıktı lavaboya göndermeden önce birleştirilir. Blobs, Tablolar, ADLS, Hizmet Veri Veri Ve Azure İşveri otomatik olarak paralel. SQL ve SQL DW çıkışlarıparalelleştirme için bir seçenek var. Olay Hub PartitionKey yapılandırma alanı (genellikle PartitionId) **tarafından PARTITION** by ile eşleşecek şekilde ayarlanmış olması gerekir. Olay Hub'ı için, bölümler arasında çapraz geçişten kaçınmak için tüm girişler ve tüm çıktılar için bölüm sayısını eşleştirmeye de ekstra dikkat edin. 
3.  En fazla ulaşılabilir iş gücünü ölçmek için **sorgunuzu 6 SU** (tek bir bilgisayar düğümünün tam kapasitesi) ile çalıştırın ve **GROUP BY**kullanıyorsanız, işin işleyebilir kaç grup (kardinallik) ölçebileceğini ölçün. Sistemin kaynak limitlerini vuran işin genel belirtileri şunlardır.
    - SU % kullanım ölçümü %80'in üzerindedir. Bu, bellek kullanımının yüksek olduğunu gösterir. Bu metrik artışa katkıda bulunan faktörler [burada](stream-analytics-streaming-unit-consumption.md)açıklanmıştır. 
    -   Çıkış zaman damgası duvar saati süresi ile ilgili olarak geride kalıyor. Sorgu mantığınıza bağlı olarak, çıkış zaman damgasının duvar saati saatinden bir mantık dengelemesi olabilir. Ancak, kabaca aynı oranda ilerleme olmalıdır. Çıktı zaman damgası giderek daha da geriden düşüyorsa, bu sistemin aşırı çalıştığının bir göstergesidir. Bu downstream çıkış lavabo azaltma veya yüksek CPU kullanımı nın bir sonucu olabilir. Şu anda CPU kullanım ölçümü sağlamadığımız için ikisini birbirinden ayırmak zor olabilir.
        - Sorun batması nedeniyle, çıktı bölüm sayısını artırmak (ve ayrıca iş tamamen paralelleştirilebilir tutmak için giriş bölümleri) veya lavabo nun kaynak miktarını artırmak (Örneğin CosmosDB için İstek Birimleri sayısı) gerekebilir.
    - İş diyagramında, her giriş için bölüm başına biriktirme listesi olay ölçümü vardır. Biriktirme listesi olay ölçümü artmaya devam ederse, sistem kaynağının kısıtlandığı (çıktı batması veya yüksek CPU nedeniyle) bir göstergesidir.
4.  6 SU işinin ulaşabileceği sınırları belirledikten sonra, belirli bir bölümü "sıcak" yapan herhangi bir veri eğriliği olmadığını varsayarak, daha fazla SUs ekledikçe işin işleme kapasitesini doğrusal olarak tahmin edebilirsiniz.

> [!NOTE]
> Doğru Akış Birimi sayısını seçin: Akış Analizi eklenen her 6 SU için bir işleme düğümü oluşturduğundan, bölümlerin düğümler arasında eşit olarak dağıtılaabilmesi için düğüm sayısını giriş bölümsayısının böleni yapmak en iyisidir.
> Örneğin, 6 SU işinizin 4 MB/s işleme hızına ulaşabileceğini ölçtünüz ve giriş bölüm sayınız 4'dür. Yaklaşık 8 MB/sişlem hızı elde etmek için işinizi 12 SU veya 16 MB/selde etmek için 24 SU ile çalıştırmayı seçebilirsiniz. Daha sonra, giriş oranınızın bir fonksiyonu olarak, iş için SU numarasını ne değere yükselteceğine karar verebilirsiniz.


## <a name="case-2---if-your-query-is-not-embarrassingly-parallel"></a>Örnek 2 - Sorgunuz utanç verici bir şekilde paralel değilse.
Sorgunuz utanç verici derecede paralel değilse, aşağıdaki adımları izleyebilirsiniz.
1.  **Bölümleme** karmaşıklığını önlemek için önce BÖLÜMSÜZ olmayan bir sorguyla başlayın ve sorgunuzu 6 SU ile çalıştırarak [büyük/küçük harf 1'deki](#case-1--your-query-is-inherently-fully-parallelizable-across-input-partitions)gibi maksimum yükü ölçün.
2.  Eğer iş vadede beklenen yükü elde edebilirsiniz, işiniz biter. Alternatif olarak, senaryonuz için çalışan en az SU sayısını bulmak için 3 SU ve 1 SU'da çalışan aynı işi ölçmeyi seçebilirsiniz.
3.  İstenilen iş elde edamıyorsanız, sorgunuzu mümkünse birden çok adıma ayırmayı deneyin, zaten birden çok adım yoksa ve sorgudaki her adım için en fazla 6 SU ayırın. Örneğin 3 adımın varsa, "Ölçekle" seçeneğine 18 SU ayırın.
4.  Böyle bir işi çalıştırırken, Stream Analytics her adımı özel 6 SU kaynağıyla kendi düğümüne koyar. 
5.  Yük hedefinize hala ulaşamadıysanız, girişe daha yakın adımlardan **başlayarak PARTITION'ı** kullanmayı deneyebilirsiniz. **Group BY** işleci için, doğal olarak bölümlenebilir olmayabilir, bir bölümlenmiş GRUP BY ardından bir bölümlenmiş **GRUP** gerçekleştirmek için yerel / küresel toplam desen kullanabilirsiniz. **GROUP BY** Örneğin, her 3 dakikada bir gişeden kaç aracın geçtiğini saymak isterseniz ve verilerin hacmi 6 SU tarafından işlenebileceklerin ötesindedir.

Sorgu:

 ```SQL
 WITH Step1 AS (
 SELECT COUNT(*) AS Count, TollBoothId, PartitionId
 FROM Input1 Partition By PartitionId
 GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
 )
 SELECT SUM(Count) AS Count, TollBoothId
 FROM Step1
 GROUP BY TumblingWindow(minute, 3), TollBoothId
 ```
Yukarıdaki sorguda, bölüm başına gişe başına araç sayıyor ve tüm bölümlerden sayımı birlikte ekliyorsunuz.

Bir kez bölümlenmiş, adımın her bölümü için, 6 SU kadar ayırın, 6 SU olan her bölüm maksimum, böylece her bölüm kendi işleme düğümü üzerine yerleştirilebilir.

> [!Note]
> Sorgunuz bölümlenemiyorsa, çok adımlı bir sorguda ek SU eklemek her zaman iş ortası iyileşmeyebilir. Performans kazanmanın bir yolu, yukarıda adım 5'te açıklandığı gibi, yerel/küresel toplam deseni kullanarak ilk adımlardaki hacmi azaltmaktır.

## <a name="case-3---you-are-running-lots-of-independent-queries-in-a-job"></a>Örnek 3 - Bir işte çok sayıda bağımsız sorgu çalıştırın.
Her kiracı için ayrı giriş ve çıktılar kullanarak, tek bir işteki birden çok kiracıdan gelen verileri işlemenin daha uygun maliyetli olduğu belirli ISV kullanım örnekleri için, tek bir işte oldukça az (örneğin 20) bağımsız sorgu çalıştırabilirsiniz. Varsayım, bu tür alt sorgunun yükü nispeten küçüktür. Bu durumda, aşağıdaki adımları izleyebilirsiniz.
1.  Bu durumda, sorguda **PARTITION BY'yi** kullanmayın
2.  Olay Hub'ını kullanıyorsanız, giriş bölümü sayısını mümkün olan en düşük 2 değerine düşürün.
3.  Sorguyu 6 SU ile çalıştırın. Her alt sorgu için beklenen yükle, iş sistem kaynak sınırlarını vurana kadar mümkün olduğunca çok alt sorgu ekleyin. Bu durumda belirtiler için [Örnek 1'e](#case-1--your-query-is-inherently-fully-parallelizable-across-input-partitions) bakın.
4.  Yukarıda ölçülen alt sorgu sınırına ulaştıktan sonra, alt sorguyu yeni bir işe eklemeye başlayın. Bağımsız sorgu sayısının bir işlevi olarak çalıştıracak iş sayısı, herhangi bir yük eğriliğiniz olmadığını varsayarak oldukça doğrusal olmalıdır. Daha sonra, hizmet vermek istediğiniz kiracı sayısının bir fonksiyonu olarak kaç tane 6 SU işi çalıştırmanız gerektiğini tahmin edebilirsiniz.
5.  Başvuru verilerini kullanırken, aynı başvuru verileriyle katılmadan önce girişleri bir araya getirin. Sonra, gerekirse olayları bölün. Aksi takdirde, her başvuru veri birleştirme bellekte başvuru verilerinin bir kopyasını tutar, büyük olasılıkla gereksiz yere bellek kullanımını havaya uçurma.

> [!Note] 
> Her işe kaç kiracı koyacağız?
> Bu sorgu deseni genellikle çok sayıda alt sorguya sahiptir ve çok büyük ve karmaşık topolojiyle sonuçlanır. İşin denetleyicisi bu kadar büyük bir topolojiyi kaldıramayabilir. Başparmak bir kural olarak, 1 SU iş için 40 kiracı altında kalmak ve 3 SU ve 6 SU işleri için 60 kiracı. Denetleyicinin kapasitesini aştığınızda, iş başarılı bir şekilde başlamaz.



## <a name="get-help"></a>Yardım alın
Daha fazla yardım için [Azure Akışı Analizi forumumuzu](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics)deneyin.

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Akış Analizine Giriş](stream-analytics-introduction.md)
* [Azure Akış Analizi'ni kullanmaya başlama](stream-analytics-real-time-fraud-detection.md)
* [Azure Akış Analizi Sorgu Dili Başvurusu](https://docs.microsoft.com/stream-analytics-query/stream-analytics-query-language-reference)
* [Azure Akış Analizi Yönetimi REST API'si Başvurusu](https://msdn.microsoft.com/library/azure/dn835031.aspx)

<!--Image references-->

[img.stream.analytics.monitor.job]: ./media/stream-analytics-scale-jobs/StreamAnalytics.job.monitor-NewPortal.png
[img.stream.analytics.configure.scale]: ./media/stream-analytics-scale-jobs/StreamAnalytics.configure.scale.png
[img.stream.analytics.perfgraph]: ./media/stream-analytics-scale-jobs/perf.png
[img.stream.analytics.streaming.units.scale]: ./media/stream-analytics-scale-jobs/StreamAnalyticsStreamingUnitsExample.jpg
[img.stream.analytics.preview.portal.settings.scale]: ./media/stream-analytics-scale-jobs/StreamAnalyticsPreviewPortalJobSettings-NewPortal.png   

<!--Link references-->

[microsoft.support]: https://support.microsoft.com
[azure.event.hubs.developer.guide]: https://msdn.microsoft.com/library/azure/dn789972.aspx

[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-real-time-fraud-detection.md
[stream.analytics.query.language.reference]: https://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: https://go.microsoft.com/fwlink/?LinkId=517301

