---
title: Azure Stream Analytics çözüm desenleri
description: Azure Akış Analizi için panolama, olay mesajlaşması, veri depoları, referans veri zenginleştirme ve izleme gibi yaygın çözüm modelleri hakkında bilgi edinin.
author: mamccrea
ms.author: mamccrea
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 06/21/2019
ms.openlocfilehash: 3b95863c1ae53bd0642aec356f55aba1faf8ef09
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79535791"
---
# <a name="azure-stream-analytics-solution-patterns"></a>Azure Stream Analytics çözüm desenleri

Azure'daki diğer birçok hizmet gibi, Akış Analizi de daha büyük bir uçtan uca çözüm oluşturmak için diğer hizmetlerle birlikte en iyi şekilde kullanılır. Bu makalede, basit Azure Akışı Analizi çözümleri ve çeşitli mimari desenler açıklanmaktadır. Daha karmaşık çözümler geliştirmek için bu desenler üzerine inşa edebilirsiniz. Bu makalede açıklanan desenler çok çeşitli senaryolarda kullanılabilir. Senaryoya özgü desenlere örnek olarak [Azure çözüm mimarilerinde](https://azure.microsoft.com/solutions/architecture/?product=stream-analytics)kullanılabilir.

## <a name="create-a-stream-analytics-job-to-power-real-time-dashboarding-experience"></a>Gerçek zamanlı pano deneyimi sağlamak için bir Stream Analytics işi oluşturun

Azure Akış Analizi ile gerçek zamanlı panoları ve uyarıları hızlı bir şekilde ayağa kaldırabilirsiniz. Basit bir çözüm Olay Hub'larından veya IoT Hub'ından olayları yutarak [Power BI panosunu akışlı veri kümesiyle besler.](/power-bi/service-real-time-streaming) Daha fazla bilgi için ayrıntılı öğretici Stream Analytics ile telefon görüşmesi verilerini analiz edin [ve Power BI panosundaki sonuçları görselleştirin.](stream-analytics-manage-job.md)

![ASA Güç BI panosu](media/stream-analytics-solution-patterns/pbidashboard.png)

Bu çözüm, Azure portalından sadece birkaç dakika içinde oluşturulabilir. Kapsamlı bir kodlama söz konusu değildir ve SQL dili iş mantığını ifade etmek için kullanılır.

Bu çözüm deseni, olay kaynağından tarayıcıdaki Power BI panosuna kadar en düşük gecikme süresini sunar. Azure Akış Analizi, bu yerleşik yeteneğe sahip tek Azure hizmetidir.

## <a name="use-sql-for-dashboard"></a>Pano için SQL'i kullanma

Power BI panosu düşük gecikme gecikmesi sunar, ancak tam teşekküllü Güç BI raporları üretmek için kullanılamaz. Ortak bir raporlama deseni, verilerinizi önce bir SQL veritabanına çıkarmaktır. Ardından, en son veriler için SQL'i sorgulamak için Power BI'nin SQL konektörünü kullanın.

![ASA SQL panosu](media/stream-analytics-solution-patterns/sqldashboard.png)

SQL veritabanını kullanmak size daha fazla esneklik sağlar, ancak biraz daha yüksek gecikme giderme pahasına. Bu çözüm, gecikme gereksinimleri bir saniyeden büyük olan işler için en uygun çözümdür. Bu yöntemle, raporlar için verileri ve çok daha fazla görselleştirme seçeneğini daha fazla dilimlemek ve zarlamak için Güç BI özelliklerini en üst düzeye çıkarabilirsiniz. Ayrıca Tableau gibi diğer pano çözümlerini kullanma esnekliği kazanırsınız.

SQL yüksek bir iş kaynağı veri deposu değildir. Azure Stream Analytics'ten bir SQL veritabanına maksimum iş akışı şu anda 24 MB/s civarındadır. Çözümünüzdeki olay kaynakları daha yüksek bir oranda veri üretiyorsa, çıktı oranını SQL'e düşürmek için Akış Analizi'nde işlem mantığını kullanmanız gerekir. Filtreleme, pencereli toplamalar, zamansal birleştirmelerle desen eşleştirme ve analitik işlevler gibi teknikler kullanılabilir. SQL'e çıkış oranı, Azure Akış Analizi çıktısında Azure [SQL Veritabanı'nda](stream-analytics-sql-output-perf.md)açıklanan teknikler kullanılarak daha da optimize edilebilir.

## <a name="incorporate-real-time-insights-into-your-application-with-event-messaging"></a>Etkinlik iletisi ile uygulamanız için gerçek zamanlı öngörüler birleştirin

Stream Analytics'in ikinci en popüler kullanımı gerçek zamanlı uyarılar oluşturmaktır. Bu çözüm deseninde, Akış Analizi'ndeki iş mantığı [zamansal ve uzamsal desenleri](stream-analytics-geospatial-functions.md) veya [anormallikleri](stream-analytics-machine-learning-anomaly-detection.md)algılamak, ardından uyarı sinyalleri üretmek için kullanılabilir. Ancak, Stream Analytics'in Power BI'yi tercih edilen bir bitiş noktası olarak kullandığı pano çözümünün aksine, bir dizi ara veri lavabosu kullanılabilir. Bu lavabolar, Olay Hub'larını, Hizmet Veri Veri Toslarını ve Azure İşlerini içerir. Uygulama oluşturucu olarak, hangi veri lavabonun senaryonuz için en iyi şekilde çalıştığına karar vermeniz gerekir.

Varolan iş akışınızda uyarılar oluşturmak için akış aşağı olay tüketici mantığının uygulanması gerekir. Azure İşlevlerinde özel mantık uygulayabileceğinizden, Azure İşlevleri bu tümleştirmeyi gerçekleştirmenin en hızlı yoludur. Azure İşi'ni Bir Akış Analizi işi için çıktı olarak kullanmak için bir [öğretici, Azure Akış Analizi işlerinden Azure İşlerini Çalıştır'da](stream-analytics-with-azure-functions.md)bulunabilir. Azure İşlevleri, metin ve e-posta gibi çeşitli bildirim türlerini de destekler. Logic App, Akış Analizi ve Mantık Uygulaması arasındaki Olay Hub'ları ile bu tür tümleştirmeler için de kullanılabilir.

![ASA olay mesajlaşma uygulaması](media/stream-analytics-solution-patterns/eventmessagingapp.png)

Olay Hub'ları ise en esnek entegrasyon noktasını sunar. Azure Veri Gezgini ve Zaman Serisi Öngörüleri gibi diğer birçok hizmet Olay Hub'larından olayları tüketebilir. Hizmetler, çözümü tamamlamak için Azure Stream Analytics'in Etkinlik Hub'larına doğrudan bağlanabilir. Olay Hub'ları, bu tür tümleştirme senaryoları için Azure'da bulunan en yüksek iş hasılatı ileti aracısidır.

## <a name="dynamic-applications-and-websites"></a>Dinamik uygulamalar ve web siteleri

Azure Akış Analitiği ve Azure SinyalR Hizmeti'ni kullanarak pano veya harita görselleştirmesi gibi özel gerçek zamanlı görselleştirmeler oluşturabilirsiniz. SignalR kullanarak, web istemcileri güncellenebilir ve gerçek zamanlı olarak dinamik içerik gösterebilir.

![ASA dinamik uygulaması](media/stream-analytics-solution-patterns/dynamicapp.png)

## <a name="incorporate-real-time-insights-into-your-application-through-data-stores"></a>Veri depoları aracılığıyla uygulamanız için gerçek zamanlı öngörüler birleştirin

Günümüzde çoğu web hizmeti ve web uygulaması, sunum katmanına hizmet etmek için bir istek/yanıt deseni kullanır. İstek/yanıt deseni oluşturmak kolaydır ve Cosmos DB gibi stateless frontend ve ölçeklenebilir mağazalar kullanılarak düşük yanıt süresi ile kolayca ölçeklenebilir.

Yüksek veri hacmi genellikle CRUD tabanlı bir sistemde performans darboğazları oluşturur. [Olay kaynak çözüm deseni](/azure/architecture/patterns/event-sourcing) performans darboğazları gidermek için kullanılır. Zamansal desenler ve öngörüler de zor ve verimsiz geleneksel bir veri deposundan ayıklamak için vardır. Modern yüksek hacimli veri odaklı uygulamalar genellikle veri akışı tabanlı bir mimari benimser. Hareket halindeki veriler için bilgi işlem motoru olarak Azure Stream Analytics, bu mimarinin temel taşıdır.

![ASA olay kaynak uygulaması](media/stream-analytics-solution-patterns/eventsourcingapp.png)

Bu çözüm deseninde, olaylar Azure Stream Analytics tarafından işlenir ve veri depolarına toplanır. Uygulama katmanı, geleneksel istek/yanıt deseni kullanarak veri depolarıyla etkileşime girmektedir. Stream Analytics'in çok sayıda olayı gerçek zamanlı olarak işleyebilme özelliği sayesinde, uygulama veri deposu katmanını toplu hale getirmek zorunda kalmadan yüksek oranda ölçeklenebilir. Veri deposu katmanı aslında sistemde maddeleştirilmiş bir görünümdür. [Azure Cosmos DB'ye azure Akış Analizi çıkışı,](stream-analytics-documentdb-output.md) Cosmos DB'nin Akış Analizi çıktısı olarak nasıl kullanıldığını açıklar.

İşleme mantığının karmaşık olduğu ve mantığın belirli bölümlerini bağımsız olarak yükseltme ihtiyacının olduğu gerçek uygulamalarda, birden çok Akış Analizi işi, aracı etkinlik aracısı olarak Event Hub'ları ile birlikte oluşturulabilir.

![ASA karmaşık olay kaynak uygulaması](media/stream-analytics-solution-patterns/eventsourcingapp2.png)

Bu desen, sistemin esnekliğini ve yönetilebilirliğini artırır. Ancak, Stream Analytics tam olarak bir kez işleme garantisi verse de, yinelenen olayların aracı Etkinlik Hub'larına inme olasılığı düşüktür. Akış Akışı Analizi işinin, geçmişe dönüş penceresinde mantık tuşlarını kullanarak olayları dedupe etmesi önemlidir. Etkinlik teslimi hakkında daha fazla bilgi için [Olay Teslim Garantileri](/stream-analytics-query/event-delivery-guarantees-azure-stream-analytics) referansına bakın.

## <a name="use-reference-data-for-application-customization"></a>Uygulama özelleştirmeiçin başvuru verilerini kullanma

Azure Akışı Analizi başvuru veri özelliği, eşik uyarı, işleme kuralları ve coğrafi çitler gibi son kullanıcı [özelleştirmesi](geospatial-scenarios.md)için özel olarak tasarlanmıştır. Uygulama katmanı parametre değişikliklerini kabul edebilir ve bunları bir SQL veritabanında depolayabilir. Akış Analizi işi, veritabanındaki değişiklikleri düzenli olarak sorgular ve özelleştirme parametrelerini başvuru verileri birleştirme yoluyla erişilebilir hale getirir. Uygulama özelleştirmeiçin başvuru verilerinin nasıl kullanılacağı hakkında daha fazla bilgi için [SQL başvuru verileri](sql-reference-data.md) ve başvuru [verileri birleştirme'ye](/stream-analytics-query/reference-data-join-azure-stream-analytics)bakın.

Bu desen, başvuru verilerinden kuralların eşiklerinin tanımlandığı bir kural altyapısı uygulamak için de kullanılabilir. Kurallar hakkında daha fazla bilgi için [Azure Akış Analizi'nde Yapılabilen Eşik Tabanlı İşlem kurallarına](stream-analytics-threshold-based-rules.md)bakın.

![ASA referans veri uygulaması](media/stream-analytics-solution-patterns/refdataapp.png)

## <a name="add-machine-learning-to-your-real-time-insights"></a>Gerçek zamanlı öngörülerinize Makine Öğrenimi ekleyin

Azure Akış Analizi'nin yerleşik [Anomali Algılama modeli,](stream-analytics-machine-learning-anomaly-detection.md) Machine Learning'i gerçek zamanlı uygulamanızla tanıştırmak için kullanışlı bir yoldur. Daha geniş bir Makine Öğrenimi gereksinimleri için Azure [Akış Analizi'nin Azure Machine Learning'in puanlama hizmetiyle tümleştirmesini](stream-analytics-machine-learning-integration-tutorial.md)görün.

Çevrimiçi eğitim ve puanlamayı aynı Stream Analytics ardışık hattına dahil etmek isteyen gelişmiş kullanıcılar için, [doğrusal regresyon](stream-analytics-high-frequency-trading.md)ile bunu nasıl yaptığına ilişkin bu örneğe bakın.

![ASA Makine Öğrenimi uygulaması](media/stream-analytics-solution-patterns/mlapp.png)

## <a name="near-real-time-data-warehousing"></a>Gerçek zamanlı veri depolamanın yakınında

Başka bir yaygın desen gerçek zamanlı veri depolama, ayrıca veri ambarı akışı denir. Uygulamanızdan Event Hub'larına ve IoT Hub'a gelen etkinliklere ek olarak, [IoT Edge'de çalışan Azure Akış Analizi,](stream-analytics-edge.md) veri temizleme, veri azaltma ve veri depolama ve iletme gereksinimlerini karşılamak için kullanılabilir. IoT Edge'de çalışan Stream Analytics, sistemdeki bant genişliği sınırlamasını ve bağlantı sorunlarını zarif bir şekilde işleyebilir. SQL çıkış bağdaştırıcısı, SQL Veri Ambarı'na çıkış için kullanılabilir; ancak, maksimum verim 10 MB/sn ile sınırlıdır.

![ASA Veri Depolama](media/stream-analytics-solution-patterns/datawarehousing.png)

Bazı gecikme tradeoff ile iş kısmını iyileştirmenin bir yolu, olayları Azure Blob depolama alanına arşivlemek ve bunları [Polybase ile SQL Veri Ambarı'na aktarmaktır.](../synapse-analytics/sql-data-warehouse/load-data-from-azure-blob-storage-using-polybase.md) [Verileri zaman damgasına göre arşivleyerek](stream-analytics-custom-path-patterns-blob-storage-output.md) ve düzenli aralıklarla içe aktararak Stream Analytics'ten blob depolamasına ve blob depolamadan SQL Veri Ambarı'na girişe kadar çıktıyı el ile birleştirmelisiniz.

Bu kullanım deseninde, Azure Akış Analizi neredeyse gerçek zamanlı ETL motoru olarak kullanılır. Yeni gelen etkinlikler sürekli olarak dönüştürülür ve akış aşağı analiz hizmeti tüketimi için saklanır.

![ASA yüksek iş kaynağı Veri Depolama](media/stream-analytics-solution-patterns/datawarehousing2.png)

## <a name="archiving-real-time-data-for-analytics"></a>Analitik için gerçek zamanlı verileri arşivleme

Çoğu veri bilimi ve analiz aktivitesi hala çevrimdışı gerçekleşir. Veriler Azure Veri Gölü Deposu Gen2 çıktısı ve Parke çıktısı biçimleri aracılığıyla Azure Stream Analytics tarafından arşivlenebilir. Bu özellik, verileri doğrudan Azure Veri Gölü Analizi, Azure Veri Tuğlaları ve Azure HDInsight'a beslemek için sürtünmeyi ortadan kaldırır. Azure Akış Analizi bu çözümde neredeyse gerçek zamanlı ETL motoru olarak kullanılır. Çeşitli bilgi işlem altyapılarını kullanarak Veri Gölü'nde arşivlenmiş verileri keşfedebilirsiniz.

![ASA çevrimdışı analitik](media/stream-analytics-solution-patterns/offlineanalytics.png)

## <a name="use-reference-data-for-enrichment"></a>Zenginleştirme için referans verilerini kullanma

Veri zenginleştirme genellikle ETL motorları için bir gerekliliktir. Azure Akış Analizi, hem SQL veritabanından hem de Azure Blob depolamasından [gelen referans verilerle](stream-analytics-use-reference-data.md) veri zenginleştirmeyi destekler. Hem Azure Veri Gölü'ne hem de SQL Veri Ambarına veri kiniş için veri zenginleştirme yapılabilir.

![Veri zenginleştirme ile ASA çevrimdışı analitik](media/stream-analytics-solution-patterns/offlineanalytics.png)

## <a name="operationalize-insights-from-archived-data"></a>Arşivlenmiş verilerden öngörüleri operasyonel hale

Çevrimdışı analitik deseni yakın gerçek zamanlı uygulama deseniyle birleştirirseniz, bir geri bildirim döngüsü oluşturabilirsiniz. Geri bildirim döngüsü, uygulamanın verilerdeki desenleri değiştirmek için otomatik olarak ayarlamasını sağlar. Bu geri bildirim döngüsü, uyarı için eşik değerini değiştirmek kadar basit veya Machine Learning modellerini yeniden eğitmek kadar karmaşık olabilir. Aynı çözüm mimarisi hem bulutta çalışan ASA işlerine hem de IoT Edge'de uygulanabilir.

![ASA operasyonelleştirme öngörüleri](media/stream-analytics-solution-patterns/insightsoperationalization.png)

## <a name="how-to-monitor-asa-jobs"></a>ASA işleri nasıl izlenir?

Bir Azure Akışı Analizi işi, gelen olayları gerçek zamanlı olarak sürekli olarak işlemek için 7/24 çalıştırılabilir. Çalışma süresi garantisi, genel uygulamanın sağlığı için çok önemlidir. Stream Analytics, sektörde [%99,9 kullanılabilirlik garantisi](https://azure.microsoft.com/support/legal/sla/stream-analytics/v1_0/)sunan tek akış analizi hizmeti olsa da, yine de bir miktar kapalı kalma süresine maruz kalabilirsiniz. Yıllar içinde Stream Analytics, işlerin durumunu yansıtacak ölçümler, günlükler ve iş durumları getirmiştir. Bunların tümü Azure Monitor hizmeti aracılığıyla su yüzüne çıkar ve OMS'ye daha fazla aktarılabilir. Daha fazla bilgi için [Bkz. Akış Analizi iş izlemeyi ve sorguları nasıl izleyebilirsiniz.](stream-analytics-monitoring.md)

![ASA izleme](media/stream-analytics-solution-patterns/monitoring.png)

İzlenmeli iki önemli şey vardır:

- [İş başarısız durum](job-states.md)

    Her şeyden önce, işin devam edeceğinden emin olmalısın. Çalışan durumda iş olmadan, yeni ölçümler veya günlükler oluşturulur. İşler, yüksek SU kullanım düzeyine (yani kaynakların tükenmesi) sahip olmak gibi çeşitli nedenlerle başarısız bir duruma dönüşebilir.

- [Filigran gecikme ölçümleri](https://azure.microsoft.com/blog/new-metric-in-azure-stream-analytics-tracks-latency-of-your-streaming-pipeline/)

    Bu metrik, işlem ardışık nızın duvar saati (saniye) ne kadar gerisinde olduğunu yansıtır. Gecikmenin bir kısmı doğal işleme mantığına atfedilir. Sonuç olarak, artan eğilimi izlemek mutlak değeri izlemekten çok daha önemlidir. Sabit durum gecikmesi, izleme veya uyarılarla değil, uygulama tasarımınızla giderilmelidir.

Başarısız olduktan sonra, etkinlik günlükleri ve [tanılama günlükleri](stream-analytics-job-diagnostic-logs.md) hataları aramaya başlamak için en iyi yerlerdir.

## <a name="build-resilient-and-mission-critical-applications"></a>Esnek ve görev açısından kritik uygulamalar oluşturun

Azure Akış Analizi'nin SLA garantisi ne olursa olsun ve uçtan uca uygulamanızı ne kadar dikkatli çalıştırdığınıza bakılmaksızın kesintiler gerçekleşir. Başvurunuz kritik bir görevse, zarif bir şekilde iyileşmek için kesintilere hazırlıklı olmanız gerekir.

Uygulamaları uyarmak için en önemli şey bir sonraki uyarıyı algılamaktır. Kurtarma yaparken, geçmiş uyarıları yoksayarak işi geçerli zamanda yeniden başlatmayı seçebilirsiniz. İş başlangıç saati semantik ilk çıkış zamanı değil, ilk giriş zamanı vardır. Giriş, belirtilen zamanda ilk çıktının tam ve doğru olduğunu garanti etmek için uygun bir süre geriye doğru olarak yeniden sarılır. Sonuç olarak kısmi toplamlar almaz ve beklenmedik şekilde uyarıları tetiklersiniz.

Ayrıca, geçmişte bir süre çıktı başlatmak için seçebilirsiniz. Hem Olay Hub'ları hem de IoT Hub'ın bekletme ilkeleri, geçmişten gelen işleme izin vermek için makul miktarda veri tutar. Bunun ne kadar hızlı bir şekilde geçerli saate yetişip zamanında yeni uyarılar oluşturmaya başlabildiğinizdir. Veriler zaman içinde değerini hızla kaybeder, bu nedenle geçerli zamana hızlı bir şekilde yetişmek önemlidir. Hızlı bir şekilde yetişmenin iki yolu vardır:

- Yetişerken daha fazla kaynak (SU) sağlama.
- Geçerli andan itibaren yeniden başlatın.

Geçerliden yeniden başlatma işlemi, işleme sırasında bir boşluk bırakmanın dengelenmesi yle yapmak kolaydır. Bu şekilde yeniden başlatma senaryoları uyarmak için tamam olabilir, ancak pano senaryoları için sorunlu olabilir ve arşivleme ve veri depolama senaryoları için başlatıcı olmayan bir başlangıç.

Daha fazla kaynak sağlanması işlemi hızlandırabilir, ancak işlem hızında dalgalanma olmasının etkisi karmaşıktır.

- İşinizin daha fazla sayıda SUs'a ölçeklenebilir olduğunu test edin. Tüm sorgular ölçeklenebilir değildir. Sorgunuzun [paralel leştirilmiş](stream-analytics-parallelization.md)olduğundan emin olmanız gerekir.

- Akış yukarı Olay Hub'larında veya IoT Hub'ında, giriş çıktısını ölçeklendirmek için daha fazla ThroughPut Birimi (TUs) ekleyebileceğiniz yeterli bölüm olduğundan emin olun. Unutmayın, her Olay Hub'ı TU 2 MB/s'lik bir çıkış hızıyla maksimuma ulaşır.

- Çıktı lavabolarında (örneğin, SQL Database, Cosmos DB) yeterli kaynak sağladığınızdan emin olun, böylece çıktılardaki dalgalanmayı azaltmaz, bu da bazen sistemin kilitlenmesine neden olabilir.

En önemli şey, işlem hızı değişikliğini tahmin etmek, üretime geçmeden önce bu senaryoları test etmek ve hata kurtarma süresi boyunca işlemeyi doğru şekilde ölçeklendirmeye hazır olmaktır.

Gelen olayların tüminin geciktiğine dair aşırı senaryoda, işinize geç gelen bir pencere [uyguladıysanız, tüm gecikmiş olayların düşmesi mümkündür.](stream-analytics-time-handling.md) Olayların düşmesi başlangıçta gizemli bir davranış gibi görünebilir; Ancak, Stream Analytics'in gerçek zamanlı bir işlem motoru olduğu göz önüne alındığında, gelen olayların duvar saati zamanına yakın olmasını bekler. Bu kısıtlamaları ihlal eden olayları bırakmak zorunda.

### <a name="lambda-architectures-or-backfill-process"></a>Lambda Mimariler veya Yedek doldurma işlemi

Neyse ki, önceki veri arşivleme deseni bu geç olayları incelikle işlemek için kullanılabilir. Fikir, arşivleme iş süreçleri gelen olaylar varış zamanında ve arşivler olaylar zaman ları ile Azure Blob veya Azure Data Lake Store doğru zaman kova içine olaylar. Bir olayın ne kadar geç geldiği önemli değil, asla bırakılmayacak. Her zaman doğru zamanda kova inecek. Kurtarma sırasında, arşivlenmiş olayları yeniden işlemek ve sonuçları tercih edilen depoya geri doldurmak mümkündür. Bu lambda desenleri nasıl uygulandığına benzer.

![ASA yedek doldurma](media/stream-analytics-solution-patterns/backfill.png)

Geri doldurma işlemi, büyük olasılıkla Azure Akış Analizi'nden farklı bir programlama modeline sahip çevrimdışı toplu işlem sistemiyle yapılmalıdır. Bu, tüm işleme mantığını yeniden uygulamanız anlamına gelir.

Geri doldurma için, sabit durum işleme gereksinimlerinden daha yüksek iş verimi işlemek için çıktı lavabolarına en azından geçici olarak daha fazla kaynak sağlamak yine de önemlidir.

|Senaryolar  |Artık yalnızca yeniden başlatın  |Son durdurulan zamandan yeniden başlat |Arşivlenmiş etkinliklerle şimdi + geri doldurmayı yeniden başlatın|
|---------|---------|---------|---------|
|**Pano Lama**   |Boşluk oluşturur    |Kısa kesinti için Tamam    |Uzun kesinti için kullanın |
|**Uyarı**   |Kabul edilebilir |Kısa kesinti için Tamam    |Gerekli değil |
|**Olay kaynak uygulaması** |Kabul edilebilir |Kısa kesinti için Tamam    |Uzun kesinti için kullanın |
|**Veri ambarlama**   |Veri kaybı  |Kabul edilebilir |Gerekli değil |
|**Çevrimdışı analitik**  |Veri kaybı  |Kabul edilebilir |Gerekli değil|

## <a name="putting-it-all-together"></a>Hepsini bir araya getirme

Yukarıda belirtilen tüm çözüm kalıplarının karmaşık bir uçtan uca sistemde bir araya getirilemeyeceğini hayal etmek zor değildir. Birleştirilmiş sistem panolar, uyarı, olay kaynak uygulaması, veri depolama ve çevrimdışı analiz yeteneklerini içerebilir.

Anahtar, sisteminizi birleştirilebilir desenlerhalinde tasarlamaktır, böylece her alt sistem oluşturulabilir, test edilebilir, yükseltilebilir ve bağımsız olarak kurtarılabilir.

## <a name="next-steps"></a>Sonraki adımlar

Azure Akış Analizi'ni kullanarak çeşitli çözüm desenleri gördünüz. Bundan sonra derinlere inerek ilk Stream Analytics işinizi oluşturabilirsiniz:

* [Azure portalını kullanarak Stream Analytics işi oluşturma](stream-analytics-quick-create-portal.md).
* [Azure PowerShell kullanarak Stream Analytics işi oluşturma](stream-analytics-quick-create-powershell.md).
* [Visual Studio'u kullanarak Bir Stream Analytics işi oluşturun.](stream-analytics-quick-create-vs.md)
