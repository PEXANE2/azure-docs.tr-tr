---
title: Azure Akış Analizinde Akış Birimleri
description: Bu makalede, Akış Birimleri ayarı ve Azure Akış Analizi'nde performansı etkileyen diğer etkenler açıklanmaktadır.
author: JSeb225
ms.author: jeanb
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 02/27/2020
ms.openlocfilehash: 397e455c8b6a1097e2a32473036e1acd2bbdf2eb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79267358"
---
# <a name="understand-and-adjust-streaming-units"></a>Akış Birimlerini anlama ve ayarlama

Akış Birimleri (SUS), Bir Akış Analizi işini yürütmek için ayrılan bilgi işlem kaynaklarını temsil eder. SU sayısı ne kadar büyük olursa işe ayrılan CPU ve bellek kaynakları o kadar fazla olur. Bu kapasite sorgu mantığına odaklanmanızı sağlar ve Akış Analizi işinizi zamanında çalıştırmak için donanımı yönetme gereksinimini özetler.

Düşük gecikme süreli akış işlemeyi başarabilmek için, Azure Stream Analytics işleri tüm işlemi bellekte gerçekleştirir. Belleği tükendiğinde, akış işi başarısız olur. Sonuç olarak, bir üretim işi için, akışlı bir işin kaynak kullanımını izlemek ve işlerin 7/24 çalışmasını sağlamak için yeterli kaynak ayrıldıklarından emin olmak önemlidir.

%0 ile %100 arasında değişen SU % kullanım ölçümü, iş yükünüzün bellek tüketimini açıklar. En az ayak izine sahip bir akış işi için bu metrik genellikle %10 ile %20 arasındadır. SU% kullanımı düşükse ve giriş olayları birikmişse, iş yükünüz büyük olasılıkla daha fazla bilgi işlem kaynağı gerektirir ve bu da SU sayısını artırmanızı gerektirir. Su metrik altında tutmak en iyisidir 80% zaman zaman ani artışlar için hesap. Microsoft, kaynak yorgunluğunu önlemek için %80 SU Kullanımı ölçümü nde bir uyarı ayarlamanızı önerir. Daha fazla bilgi için [Bkz. Öğretici: Azure Akış Analizi işleri için uyarılar ayarlayın.](stream-analytics-set-up-alerts.md)

## <a name="configure-stream-analytics-streaming-units-sus"></a>Akış Analizi Akış Birimlerini (SUs) Yapılandır
1. [Azure portalında](https://portal.azure.com/) oturum açma

2. Kaynaklar listesinde, ölçeklendirmek istediğiniz Akış Analizi işini bulun ve ardından açın. 

3. İş sayfasında, **Yapıla** başlığı altında **Ölçek'i**seçin. 

    ![Azure portalı Stream Analytics iş yapılandırması][img.stream.analytics.preview.portal.settings.scale]
    
4. İş için SUs'ları ayarlamak için kaydırıcıyı kullanın. Belirli SU ayarlarıyla sınırlı olduğunuza dikkat edin. 
5. Çalışırken bile işinize atanan SUs sayısını değiştirebilirsiniz. İşiniz [bölümlenmemiş](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-parallelization#query-using-non-partitioned-output) bir çıktı kullanıyorsa veya [farklı PARTITION BY değerlerine sahip çok adımlı bir sorguvarsa](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-parallelization#multi-step-query-with-different-partition-by-values)bu mümkün değildir. İş çalışırken su değerleri kümesiarasından seçim yapmakla sınırlı olabilir. 

## <a name="monitor-job-performance"></a>İş performansını izleme
Azure portalını kullanarak, bir işin iş kısmını izleyebilirsiniz:

![Azure Akış Analizi işleri izler][img.stream.analytics.monitor.job]

İş yükünün beklenen iş yükünü hesaplayın. İş bölümü beklenenden azsa, giriş bölmesini ayarlayın, sorguyu ayarlayın ve işinize SUs ekleyin.

## <a name="how-many-sus-are-required-for-a-job"></a>Bir iş için kaç SU gerekir?

Belirli bir iş için gerekli SUs sayısını seçmek, girdilerin bölüm yapılandırmasına ve iş içinde tanımlanan sorguya bağlıdır. **Ölçek** sayfası doğru sayıda SUs ayarlamanızı sağlar. Gerektiğinden daha fazla SUs ayırmak en iyi uygulamadır. Stream Analytics işleme motoru, ek bellek ayırma pahasına gecikme ve iş hacmi için optimize eder.

Genel olarak, en iyi uygulama **BÖLÜM BY**kullanmayan sorgular için 6 SUs ile başlamaktır. Ardından, temsili veri tutarlarını geçtikten ve SU% Kullanım ölçümünü inceledikten sonra SU sayısını değiştirdiğiniz bir deneme yanılma yöntemi ni kullanarak tatlı noktayı belirleyin. Bir Akış Analizi işi tarafından kullanılabilecek en fazla akış birimi sayısı, iş için tanımlanan sorgudaki adım sayısına ve her adımdaki bölüm sayısına bağlıdır. [Burada](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-parallelization#calculate-the-maximum-streaming-units-of-a-job)sınırları hakkında daha fazla bilgi edinebilirsiniz.

Doğru SUs sayısını seçme hakkında daha fazla bilgi için şu sayfaya bakın: [İş akışını artırmak için Azure Akış Analizi işlerini ölçeklendirin](stream-analytics-scale-jobs.md)

> [!Note]
> Belirli bir iş için kaç SUs'un gerekli olduğunu seçmek, girişlerin bölüm yapılandırmasına ve iş için tanımlanan sorguya bağlıdır. Bir iş için SUs'ta kotanızı seçebilirsiniz. Varsayılan olarak, her Azure aboneliği, belirli bir bölgedeki tüm analiz işleri için en fazla 500 SUs kotasına sahiptir. Abonelikleriniz için bu kotanın ötesinde sus'ları artırmak için [Microsoft Destek'e](https://support.microsoft.com)başvurun. İş başına SUs için geçerli değerler 1, 3, 6 ve yukarı 6'dır.

## <a name="factors-that-increase-su-utilization"></a>SU kullanım yüzdesini artıran faktörler 

Zamansal (zaman yönelimli) sorgu öğeleri, Stream Analytics tarafından sağlanan durum lu işleçlerin temel kümesidir. Stream Analytics, bellek tüketimini yöneterek, esneklik için denetim noktasını oluşturarak ve hizmet yükseltmeleri sırasında durum kurtarma işlemlerini kullanıcı adına dahili olarak yönetir. Stream Analytics durumları tam olarak yönetse de, kullanıcıların göz önünde bulundurması gereken bir dizi en iyi uygulama önerisi vardır.

Karmaşık sorgu mantığına sahip bir işin, sürekli giriş olayları almasa bile yüksek SU% kullanımına sahip olabileceğini unutmayın. Bu giriş ve çıkış olaylarında ani bir ani ani ani bir artış sonra gerçekleşebilir. Sorgu karmaşıksa, iş bellekte durumu korumaya devam edebilir.

SU% kullanımı aniden beklenen seviyelere geri gelmeden önce kısa bir süre için 0 düşebilir. Bu geçici hatalar veya sistem yükseltmeleri başlatılan nedeniyle olur. Bir iş için akış birimlerinin artan sayıda su% Kullanımını azaltmayabilir sorgunuz [tam olarak paralel](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-parallelization)değilse.

Bir süre boyunca kullanımı karşılaştırırken, [olay hızı ölçümlerini](stream-analytics-monitoring.md)kullanın. GirişOlaylar ve ÇıktılarOlaylar ölçümleri kaç olayın okunduğunu ve işlenmeyi gösterir. Deserialization hataları gibi hata olaylarının sayısını gösteren ölçümler de vardır. Zaman birimi başına olay sayısı arttığında, çoğu durumda SU% artar.

## <a name="stateful-query-logicin-temporal-elements"></a>Zamansal öğelerde durumlu sorgu mantığı
Azure Akış Analizi işinin benzersiz yeteneklerinden biri, pencereli toplamalar, zamansal birleştirmeler ve zamansal analitik işlevler gibi durum sallanabilir işlemler gerçekleştirmektir. Bu işleçlerin her biri durum bilgilerini tutar.Bu sorgu öğeleri için en büyük pencere boyutu yedi gündür. 

Zamansal pencere kavramı birkaç Akış Analizi sorgu öğesinde görünür:
1. Pencereli agregalar: YUVARLAnma, Atlamalı ve Sürgülü pencerelerden OLUŞAN GROUP BY

2. Zamansal birleştirmeler: DATEDIFF fonksiyonu ile BIRLEŞTIRME

3. Zamansal analitik fonksiyonlar: LIMIT SÜRELİ ISFIRST, LAST ve LAG

Aşağıdaki etkenler, Stream Analytics işleri tarafından kullanılan belleği (akış birimleri ölçümünün bir parçası) etkiler:

## <a name="windowed-aggregates"></a>Pencereli toplamlar
Pencereli bir agrega için tüketilen bellek (durum boyutu) her zaman pencere boyutuyla doğru orantılı değildir. Bunun yerine, tüketilen bellek verilerin kardinalliği veya her zaman penceresindeki grup sayısı ile orantılıdır.


Örneğin, aşağıdaki sorguda, ilişkili sayı `clusterid` sorgunun kardinalidir. 

   ```sql
   SELECT count(*)
   FROM input 
   GROUP BY  clusterid, tumblingwindow (minutes, 5)
   ```

Önceki sorguda yüksek kardinallik nedeniyle herhangi bir sorunları azaltmak için, olay Hub tarafından `clusterid`bölümlenmiş olaylar gönderebilir ve sistemin aşağıdaki örnekte gösterildiği gibi **PARTITION BY** kullanarak ayrı ayrı her giriş bölümü işlemek için izin vererek sorgu ölçeklendirmek:

   ```sql
   SELECT count(*) 
   FROM input PARTITION BY PartitionId
   GROUP BY PartitionId, clusterid, tumblingwindow (minutes, 5)
   ```

Sorgu bölümlere ayrıldıktan sonra birden çok düğüme dağıtılır. Sonuç olarak, her `clusterid` düğüme gelen değer sayısı azaltılır ve böylece operatör tarafından grubun kardinalliği azalır. 

Olay Hub bölümleri bir azaltma adımı gerekönlemek için gruplandırma anahtarı ile bölümlenmiş olmalıdır. Daha fazla bilgi için [Olay Hub'larına genel bakış adabına](../event-hubs/event-hubs-what-is-event-hubs.md)bakın. 

## <a name="temporal-joins"></a>Zamansal birleştirmeler
Zamansal bir birleştirme nin tüketilen bellek (durum boyutu), birleştirmenin zamansal kıpırdanma odasındaki olayların sayısıyla orantılıdır, bu da olay giriş hızının kıpırdanma odası boyutuyla çarpılmasıdır. Başka bir deyişle, birleştirmeler tarafından tüketilen bellek, ortalama olay hızıyla çarpılarak DateDiff zaman aralığıyla orantılıdır.

Birleştirmedeki eşleşmemiş olayların sayısı sorgunun bellek kullanımını etkiler. Aşağıdaki sorgu, tıklama oluşturan reklam görüntüleme sayısını bulmayı amaçlamaktadır:

   ```sql
   SELECT clicks.id
   FROM clicks 
   INNER JOIN impressions ON impressions.id = clicks.id AND DATEDIFF(hour, impressions, clicks) between 0 AND 10.
   ```

Bu örnekte, çok sayıda reklamın gösterilmesi ve çok az kişinin tıklaması mümkündür ve tüm olayları zaman penceresinde tutmak gerekir. Tüketilen bellek miktarı pencere boyutu ve olay hızıyla doğru orantılıdır. 

Bunu düzeltmek için, birleştirme anahtarlarıyla bölümlenmiş Olay Hub'ına olayları gönderin (bu durumda kimlik) ve sistemin her giriş bölümüne gösterildiği gibi PARTITION BY'yi kullanarak ayrı ayrı işlemesine izin vererek sorguyu **ölçeklendirin:**

   ```sql
   SELECT clicks.id
   FROM clicks PARTITION BY PartitionId
   INNER JOIN impressions PARTITION BY PartitionId 
   ON impression.PartitionId = clicks.PartitionId AND impressions.id = clicks.id AND DATEDIFF(hour, impressions, clicks) between 0 AND 10 
   ```

Sorgu bölümlere ayrıldıktan sonra birden çok düğüme dağıtılır. Sonuç olarak, her düğüme gelen olayların sayısı azaltılır ve böylece birleştirme penceresinde tutulan durum boyutunu küçültür. 

## <a name="temporal-analytic-functions"></a>Zamansal analitik fonksiyonlar
Zamansal analitik fonksiyonun tüketilen bellek (durum boyutu) olay hızı nın süreyle çarpımıyla orantılıdır.Analitik işlevler tarafından tüketilen bellek pencere boyutuyla orantılı değil, her zaman penceresinde bölüm sayısıdır.

Düzeltme zamansal birleştirme benzer. **PARTITION BY'yi**kullanarak sorguyu ölçeklendirebilirsiniz. 

## <a name="out-of-order-buffer"></a>Sipariş dışı arabellek 
Kullanıcı, Olay Sırası yapılandırma bölmesinde sıra dışı arabellek boyutunu yapılandırabilir. Arabellek, girişleri pencere süresince tutmak ve yeniden sıralamak için kullanılır. Arabelleğein boyutu, olay giriş hızıyla, sıra dışı pencere boyutuyla çarpımıyla orantılıdır. Varsayılan pencere boyutu 0'dır. 

Sıra dışı arabelleği taşma düzeltmek için, PARTITION **BY**kullanarak sorgu ölçeklendirmek. Sorgu bölümlere ayrıldıktan sonra birden çok düğüme dağıtılır. Sonuç olarak, her düğüme gelen olayların sayısı azaltılır ve böylece her yeniden sipariş arabelleğindeki olayların sayısı azalır. 

## <a name="input-partition-count"></a>Giriş bölümü sayısı 
Bir iş girişinin her giriş bölümü bir arabellek vardır. Giriş bölümleri daha fazla sayıda, iş daha fazla kaynak tüketir. Azure Akış Analizi, her akış birimi için yaklaşık 1 MB/s'lik giriş işleyebilir. Bu nedenle, Akış Analizi akış birimlerinin sayısını Olay Hub'ınızdaki bölüm sayısıyla eşleştirerek en iyi duruma getirebilirsiniz. 

Genellikle, bir akış birimiyle yapılandırılan bir iş, iki bölümü olan bir Olay Hub'ı için yeterlidir (olay hub'ı için en az olan iş). Olay Hub'ı daha fazla bölüme sahipse, Akış Analizi işiniz daha fazla kaynak tüketir, ancak Olay Hub'ı tarafından sağlanan ekstra iş fazlasını kullanması gerekmez. 

6 akış birimine sahip bir iş için Olay Hub'ından 4 veya 8 bölüm gerekebilir. Ancak, aşırı kaynak kullanımına neden olduğundan çok fazla gereksiz bölümkaçının. Örneğin, 1 akış birimi olan bir Akış Analizi işinde 16 veya daha büyük bölümleri olan bir Olay Hub'ı. 

## <a name="reference-data"></a>Referans verileri 
ASA'daki başvuru verileri hızlı arama için belleğe yüklenir. Geçerli uygulamayla, aynı başvuru verileriyle birden çok kez katılsanız bile, başvuru verileriyle yapılan her birleştirme işlemi, başvuru verilerinin bir kopyasını bellekte tutar. **PARTITION BY**ile sorgular için, her bölüm başvuru verilerinin bir kopyasına sahiptir, bu nedenle bölümler tamamen ayrılmıştır. Çarpan efekti ile, birden çok bölümile referans verileri birden çok kez katılırsanız bellek kullanımı hızla çok yüksek alabilirsiniz.  

### <a name="use-of-udf-functions"></a>UDF işlevlerinin kullanımı
Bir UDF işlevi eklediğinizde, Azure Akış Analizi JavaScript çalışma zamanını belleğe yükler. Bu SU% etkileyecektir.

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Akış Analitiği'nde paralelleştirilebilir sorgular oluşturma](stream-analytics-parallelization.md)
* [İş verisini artırmak için Azure Akış Analizi işlerini ölçeklendirin](stream-analytics-scale-jobs.md)

<!--Image references-->

[img.stream.analytics.monitor.job]: ./media/stream-analytics-scale-jobs/StreamAnalytics.job.monitor-NewPortal.png
[img.stream.analytics.configure.scale]: ./media/stream-analytics-scale-jobs/StreamAnalytics.configure.scale.png
[img.stream.analytics.perfgraph]: ./media/stream-analytics-scale-jobs/perf.png
[img.stream.analytics.streaming.units.scale]: ./media/stream-analytics-scale-jobs/StreamAnalyticsStreamingUnitsExample.jpg
[img.stream.analytics.preview.portal.settings.scale]: ./media/stream-analytics-scale-jobs/StreamAnalyticsPreviewPortalJobSettings-NewPortal.png   
