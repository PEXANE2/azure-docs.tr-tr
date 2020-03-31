---
title: Azure Akış Analizi'nde sorgu paralelleştirmeve ölçek kullanma
description: Bu makalede, giriş bölümlerini yapılandırarak, sorgu tanımını ayarlayarak ve iş akışı birimleri ayarlayarak Akış Analizi işlerinin nasıl ölçeklendirileni açıklanmaktadır.
author: JSeb225
ms.author: jeanb
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 05/07/2018
ms.openlocfilehash: 31ac43ec796d305b8a8f4b62ea09481e262b6b3f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80256989"
---
# <a name="leverage-query-parallelization-in-azure-stream-analytics"></a>Azure Akış Analitiği'nde sorgu paralellemi'ni kinle
Bu makalede, Azure Akış Analizi'nde paralelleştirmeden nasıl yararlanabileceğiniz gösterilmektedir. Giriş bölümlerini yapılandırarak ve analitik sorgu tanımını ayarlayarak Akış Analizi işlerini nasıl ölçeklendireceklerini öğrenirsiniz.
Ön koşul olarak, [Akış Birimlerini Anlayın ve ayarlayın'da](stream-analytics-streaming-unit-consumption.md)açıklanan Akış Birimi kavramına aşina olmak isteyebilirsiniz.

## <a name="what-are-the-parts-of-a-stream-analytics-job"></a>Akış Analizi işinin bölümleri nelerdir?
Akış Analizi iş tanımı girişleri, sorguve çıktıiçerir. Girişler, işin veri akışını okuduğu yerdir. Sorgu, veri giriş akışını dönüştürmek için kullanılır ve çıktı işin iş sonuçlarını gönderdiği yerdir.

Bir iş veri akışı için en az bir giriş kaynağı gerektirir. Veri akışı giriş kaynağı bir Azure olay hub'ında veya Azure blob depolama alanında depolanabilir. Daha fazla bilgi için azure [akış analizine giriş](stream-analytics-introduction.md) ve [Azure Akış Analitiği'ni kullanmaya başlayın'](stream-analytics-real-time-fraud-detection.md)a bakın.

## <a name="partitions-in-sources-and-sinks"></a>Kaynaklardaki ve lavabolarda bölümler
Bir Akış Analizi işini ölçekleme, giriş veya çıktıdaki bölümlerden yararlanır. Bölümleme, verileri bir bölüm anahtarına dayalı alt kümelere bölmenizi sağlar. Verileri tüketen bir işlem (Akış Analizi işi gibi) farklı bölümleri paralel olarak tüketebilir ve yazabilir ve bu da iş akışını artırır. 

### <a name="inputs"></a>Girişler
Tüm Azure Akış Analizi girişi bölümleme avantajlarından yararlanabilir:
-   EventHub (partition by anahtar kelime ile bölüm anahtarını açıkça ayarlamanız gerekir)
-   IoT Hub (partition by anahtar kelime ile açıkça bölüm anahtarını ayarlamanız gerekir)
-   Blob depolama

### <a name="outputs"></a>Çıkışlar

Stream Analytics ile çalışırken, çıktılardaki bölümleme avantajlarından yararlanabilirsiniz:
-   Azure Data Lake Storage
-   Azure İşlevleri
-   Azure Tablosu
-   Blob depolama (bölüm anahtarını açıkça ayarlayabilir)
-   Cosmos DB (bölüm anahtarını açıkça ayarlamanız gerekir)
-   Olay Hub'ları (bölüm anahtarını açıkça ayarlamanız gerekir)
-   IoT Hub (bölüm anahtarını açıkça ayarlamanız gerekir)
-   Service Bus
- İsteğe bağlı bölümleme ile SQL ve SQL Veri Ambarı: [Azure SQL Veritabanı na Çıktı sayfasında](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-sql-output-perf)daha fazla bilgi edinin.

Power BI bölümleme desteklemez. Ancak yine de [bu bölümde](#multi-step-query-with-different-partition-by-values) açıklandığı gibi giriş bölümleyebilirsiniz 

Bölümler hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

* [Event Hubs özelliklerine genel bakış](../event-hubs/event-hubs-features.md#partitions)
* [Veri bölümleme](https://docs.microsoft.com/azure/architecture/best-practices/data-partitioning)


## <a name="embarrassingly-parallel-jobs"></a>Utanç verici paralel işler
*Utanç verici derecede paralel* bir iş, Azure Akış Analizi'nde sahip olduğumuz en ölçeklenebilir senaryodur. Girişin bir bölümü, sorgunun bir örneğine çıktının bir bölümüne bağlanır. Bu paralellik aşağıdaki gereksinimlere sahiptir:

1. Sorgu mantığınız aynı anahtara aynı sorgu örneği tarafından işlenen bağlıdırsa, olayların girişinizin aynı bölümüne gittiğine emin olmalısınız. Olay Hub'ları veya IoT Hub'ı için bu, olay verilerinin **PartitionKey** değer kümesine sahip olması gerektiği anlamına gelir. Alternatif olarak, bölümlenmiş gönderenler kullanabilirsiniz. Blob depolama için bu, olayların aynı bölüm klasörüne gönderildiği anlamına gelir. Sorgu mantığınız aynı anahtaraynı sorgu örneği tarafından işlenmesini gerektirmiyorsa, bu gereksinimi yok sayabilirsiniz. Bu mantığa örnek basit bir select-project-filter sorgusu olacaktır.  

2. Veriler giriş tarafında ortaya konduktan sonra, sorgunuzun bölümlenmiş olduğundan emin olmalısınız. Bu, tüm adımlarda **PARTITION BY'yi** kullanmanızı gerektirir. Birden çok adıma izin verilir, ancak bunların tümü aynı anahtarla bölümlere ayrılmıştır. Uyumluluk düzeyi 1.0 ve 1.1 altında, işin tamamen paralel olabilmesi için bölümleme anahtarı bölümleme anahtarının **PartitionId** olarak ayarlanması gerekir. Uyumluluk düzeyi 1.2 ve üzeri olan işleriçin, giriş ayarlarında özel sütun Bölüm Anahtarı olarak belirtilebilir ve iş BÖLÜM BY yan tümcesi olmadan bile otomatik olarak paralellenir. Olay hub çıkışı için "Partition key column" özelliği "PartitionId" kullanmak üzere ayarlanmalıdır.

3. Çıktımızın çoğu bölümlemeden yararlanabilir, ancak işinizi bölmeyi desteklemeyen bir çıktı türü kullanırsanız tam olarak paralel olmayacaktır. Olay Hub çıktıları **için, Bölüm tuşu sütununsorgu** bölümü anahtarıyla aynı şekilde ayarlandığından emin olun. Daha fazla ayrıntı için [çıktı bölümüne](#outputs) bakın.

4. Giriş bölümlerinin sayısı çıktı bölüm sayısına eşit olmalıdır. Blob depolama çıkışı bölümleri destekleyebilir ve yukarı akış sorgusunun bölümleme düzenini devralır. Blob depolama için bir bölüm anahtarı belirtildiğinde, veri giriş bölümü başına bölümlenir, böylece sonuç hala tamamen paraleldir. Burada tam olarak paralel bir iş izin bölüm değerleri örnekleri şunlardır:

   * 8 olay hub giriş bölümleri ve 8 olay hub çıkış bölümleri
   * 8 olay hub giriş bölümleri ve blob depolama çıkışı
   * Rasgele kardinallik ile özel bir alan tarafından bölümlenmiş 8 olay hub giriş bölümleri ve blob depolama çıkışı
   * 8 blob depolama giriş bölümleri ve blob depolama çıkışı
   * 8 blob depolama giriş bölümleri ve 8 olay hub çıkış bölümleri

Aşağıdaki bölümlerde utanç verici paralel bazı örnek senaryolar tartışılmaktadır.

### <a name="simple-query"></a>Basit sorgu

* Giriş: 8 bölümlü olay merkezi
* Çıktı: 8 bölümlü olay hub'ı ("Partition tuşu sütun" "PartitionId" kullanmak üzere ayarlanmalıdır)

Sorgu:

```SQL
    SELECT TollBoothId
    FROM Input1 Partition By PartitionId
    WHERE TollBoothId > 100
```

Bu sorgu basit bir filtredir. Bu nedenle, olay hub'ına gönderilen girişi bölümleme konusunda endişelenmemize gerek yoktur. 1.2'den önce uyumluluk düzeyine sahip işlerin **Partition BY PartitionId** yan tümcesi içermesi gerektiğine dikkat edin, bu nedenle daha önceki #2 gereksinimini yerine getirmektedir. Çıktı için, **bölüm anahtarının PartitionId'e**ayarlamıştırılmak için işteki olay hub çıktısını yapılandırmamız gerekir. Son bir denetim, giriş bölümleri sayısının çıktı bölüm sayısına eşit olduğundan emin olmaktır.

### <a name="query-with-a-grouping-key"></a>Gruplandırma anahtarıyla sorgula

* Giriş: 8 bölümlü olay merkezi
* Çıktı: Blob depolama

Sorgu:

```SQL
    SELECT COUNT(*) AS Count, TollBoothId
    FROM Input1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
```

Bu sorgunun bir gruplandırma anahtarı vardır. Bu nedenle, birlikte gruplanan olaylar aynı Olay Hub bölümüne gönderilmelidir. Bu örnekte TollBoothID tarafından gruplanınca, olaylar Olay Hub'ına gönderildiğinde TollBoothID'nin bölüm anahtarı olarak kullanıldığından emin olmalıyız. Daha sonra ASA'da, bu bölüm düzeninden devralmak ve tam paralelleştirmeyi etkinleştirmek için **PARTITION BY PartitionId'i** kullanabiliriz. Çıktı blob depolama olduğundan, gereksinim #4 göre, bir bölüm anahtar değeri yapılandırma konusunda endişelenmenize gerek yok.

## <a name="example-of-scenarios-that-are-not-embarrassingly-parallel"></a>Utanç verici paralel *olmayan* senaryoörneği

Bir önceki bölümde, bazı utanç verici paralel senaryolar gösterdi. Bu bölümde, utanç verici bir şekilde paralel olmak için tüm gereksinimleri karşılamayan senaryoları tartışıyoruz. 

### <a name="mismatched-partition-count"></a>Eşleşmemiş bölüm sayısı
* Giriş: 8 bölümlü olay merkezi
* Çıktı: 32 bölümlü olay merkezi

Bu durumda, sorgunun ne olduğu önemli değildir. Giriş bölümü sayısı çıktı bölüm sayısıyla eşleşmiyorsa, topoloji utanç verici bir şekilde paralel değildir.+ Ancak yine de bir seviye veya paralelleştirme elde edebiliriz.

### <a name="query-using-non-partitioned-output"></a>Bölümlenmemiş çıktıyı kullanarak sorgula
* Giriş: 8 bölümlü olay merkezi
* Çıkış: Güç BI

Power BI çıkışı şu anda bölümleme desteklemez. Bu nedenle, bu senaryo utanç verici paralel değildir.

### <a name="multi-step-query-with-different-partition-by-values"></a>Farklı PARTITION BY değerleri ile çok adımlı sorgu
* Giriş: 8 bölümlü olay merkezi
* Çıktı: 8 bölümlü olay merkezi

Sorgu:

```SQL
    WITH Step1 AS (
    SELECT COUNT(*) AS Count, TollBoothId, PartitionId
    FROM Input1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
    )

    SELECT SUM(Count) AS Count, TollBoothId
    FROM Step1 Partition By TollBoothId
    GROUP BY TumblingWindow(minute, 3), TollBoothId
```

Gördüğünüz gibi, ikinci adım bölümleme anahtarı olarak **TollBoothId** kullanır. Bu adım ilk adımla aynı değildir ve bu nedenle bir karıştırma yapmamızı gerektirir. 

Önceki örnekler, utanç verici derecede paralel bir topolojiye uyan (veya uymayan) bazı Akış Analizi işlerini göstermektedir. Eğer uygun yaparlarsa, maksimum ölçek potansiyeline sahip olurlar. Bu profillerden birine uymayan işlerde ölçekleme kılavuzu gelecekteki güncelleştirmelerde kullanılabilir. Şimdilik, aşağıdaki bölümlerde genel kılavuzu kullanın.

### <a name="compatibility-level-12---multi-step-query-with-different-partition-by-values"></a>Uyumluluk düzeyi 1.2 - Farklı PARTITION BY değerleri ile çok adımlı sorgu 
* Giriş: 8 bölümlü olay merkezi
* Çıktı: 8 bölümlü olay hub'ı ("Partition anahtar sütunu" "TollBoothId"i kullanacak şekilde ayarlanmalıdır)

Sorgu:

```SQL
    WITH Step1 AS (
    SELECT COUNT(*) AS Count, TollBoothId
    FROM Input1
    GROUP BY TumblingWindow(minute, 3), TollBoothId
    )

    SELECT SUM(Count) AS Count, TollBoothId
    FROM Step1
    GROUP BY TumblingWindow(minute, 3), TollBoothId
```

Uyumluluk düzeyi 1.2 varsayılan olarak paralel sorgu yürütme sağlar. Örneğin, "TollBoothId" sütunu giriş Bölümü Anahtarı olarak ayarlanakadar önceki bölümdeki sorgu bölümlenir. PartitionId yan tümcesi ile BÖLÜMLEME gerekli değildir.

## <a name="calculate-the-maximum-streaming-units-of-a-job"></a>Bir işin maksimum akış birimlerini hesaplama
Bir Akış Analizi işi tarafından kullanılabilecek toplam akış birimi sayısı, iş için tanımlanan sorgudaki adım sayısına ve her adım için bölüm sayısına bağlıdır.

### <a name="steps-in-a-query"></a>Sorgudaki adımlar
Bir sorgunun bir veya çok adımı olabilir. Her **adım, WITH** anahtar sözcüğü tarafından tanımlanan bir alt sorgudur. **WITH** anahtar sözcüğünün dışında olan sorgu (yalnızca bir sorgu) da aşağıdaki sorgudaki **SELECT** deyimi gibi bir adım olarak sayılır:

Sorgu:

```SQL
    WITH Step1 AS (
        SELECT COUNT(*) AS Count, TollBoothId
        FROM Input1 Partition By PartitionId
        GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
    )
    SELECT SUM(Count) AS Count, TollBoothId
    FROM Step1
    GROUP BY TumblingWindow(minute,3), TollBoothId
```

Bu sorgunun iki adımı vardır.

> [!NOTE]
> Bu sorgu daha sonra makalede daha ayrıntılı olarak ele alınmıştır.
>  

### <a name="partition-a-step"></a>Bölüm bir adım
Bir adımı bölümleme için aşağıdaki koşullar aranır:

* Giriş kaynağı bölümlenmiş olmalıdır. 
* Sorgunun **SELECT** deyimi bölümlenmiş bir giriş kaynağından okunmalıdır.
* Adım içindeki sorguda ANAHTAR **SÖZCÜK BÖLÜMLÜ** olması gerekir.

Sorgu bölümlendiğinde, giriş olayları ayrı bölüm gruplarında işlenir ve toplanır ve çıktı olayları her grup için oluşturulur. Birleşik bir toplam istiyorsanız, toplamak için ikinci bir bölümlenmemiş adım oluşturmanız gerekir.

### <a name="calculate-the-max-streaming-units-for-a-job"></a>Bir iş için maksimum akış birimlerini hesaplama
Bölümlenmemiş tüm adımlar, Bir Stream Analytics işi için altı akış birimine (SUS) kadar ölçeklenebilir. Buna ek olarak, bölümlenmiş bir adımda her bölüm için 6 SUs ekleyebilirsiniz.
Aşağıdaki tabloda bazı **örnekler** görebilirsiniz.

| Sorgu                                               | Max SUs iş için |
| --------------------------------------------------- | ------------------- |
| <ul><li>Sorgu bir adım içerir.</li><li>Adım bölümlenmiyor.</li></ul> | 6 |
| <ul><li>Giriş veri akışı 16 ile bölümlere ayrılmıştır.</li><li>Sorgu bir adım içerir.</li><li>Adım bölümlenmiş.</li></ul> | 96 (6 * 16 bölüm) |
| <ul><li>Sorgu iki adım içerir.</li><li>Adımların hiçbiri bölümlenmiş değil.</li></ul> | 6 |
| <ul><li>Giriş veri akışı 3 ile bölümlere ayrılmıştır.</li><li>Sorgu iki adım içerir. Giriş adımı bölümlenir ve ikinci adım değildir.</li><li><strong>SELECT</strong> deyimi bölümlenmiş girişten okur.</li></ul> | 24 (Bölümlenmiş adımlar için 18 + 6 bölümsüz adımlar için |

### <a name="examples-of-scaling"></a>Ölçekleme örnekleri

Aşağıdaki sorgu, üç gişesi olan bir gişeden geçen üç dakikalık bir penceredeki araç sayısını hesaplar. Bu sorgu altı SUs'a kadar ölçeklendirilebilir.

```SQL
    SELECT COUNT(*) AS Count, TollBoothId
    FROM Input1
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
```

Sorgu için daha fazla SUs kullanmak için hem giriş veri akışı hem de sorgu bölümlenmiş olmalıdır. Veri akışı bölümü 3 olarak ayarlandığından, aşağıdaki değiştirilmiş sorgu 18 SUs'a kadar ölçeklendirilebilir:

```SQL
    SELECT COUNT(*) AS Count, TollBoothId
    FROM Input1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
```

Sorgu bölümlendiğinde, giriş olayları işlenir ve ayrı bölüm gruplarında toplanır. Çıktı olayları da her grup için oluşturulur. **GROUP BY** alanı giriş veri akışındaki bölüm anahtarı olmadığında bölümleme bazı beklenmeyen sonuçlara neden olabilir. Örneğin, önceki sorgudaki **TollBoothId** alanı **Giriş1'in**bölüm anahtarı değildir. Sonuç olarak, TollBooth #1 gelen veriler birden çok bölüme yayılabilir.

**Input1** bölümlerinin her biri Stream Analytics tarafından ayrı ayrı işlenir. Sonuç olarak, aynı Takla penceresinde aynı gişe için araç sayısının birden fazla kaydı oluşturulur. Giriş bölümü anahtarı değiştirilemezse, aşağıdaki örnekte olduğu gibi, bölümler arasında değerleri toplamak için bölümolmayan bir adım eklenerek bu sorun giderilebilir:

```SQL
    WITH Step1 AS (
        SELECT COUNT(*) AS Count, TollBoothId
        FROM Input1 Partition By PartitionId
        GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
    )

    SELECT SUM(Count) AS Count, TollBoothId
    FROM Step1
    GROUP BY TumblingWindow(minute, 3), TollBoothId
```

Bu sorgu 24 SUs'a ölçeklenebilir.

> [!NOTE]
> İki akışa katılıyorsanız, akışların birleştirmeleri oluşturmak için kullandığınız sütunun bölümleme anahtarıyla bölümlere katıldığından emin olun. Ayrıca, her iki akışta da aynı sayıda bölümolduğundan emin olun.
> 
> 

## <a name="achieving-higher-throughputs-at-scale"></a>Ölçekte daha yüksek iş elde etme

Utanç [verici derecede paralel](#embarrassingly-parallel-jobs) bir iş gereklidir, ancak ölçekte daha yüksek bir iş elde etmek için yeterli değildir. Her depolama sistemi ve buna karşılık gelen Stream Analytics çıktısı, mümkün olan en iyi yazma verime nasıl elde edilene ilişkin varyasyonlara sahiptir. Herhangi bir ölçekte senaryoda olduğu gibi, doğru yapılandırmaları kullanarak çözülebilecek bazı zorluklar vardır. Bu bölümde, birkaç ortak çıktı için yapılandırmalar açıklanır ve saniyede 1K, 5K ve 10K olaylarının sürekli olarak yutulma oranları için örnekler sağlar.

Aşağıdaki gözlemler, Event Hub, Azure SQL DB veya Cosmos DB'ye yazan temel bir JavaScript UDF olan stateless (geçiş) sorgusuna sahip bir Akış Analizi işi kullanır.

#### <a name="event-hub"></a>Olay Hub'ı

|Yutma Oranı (saniyedeki olaylar) | Akış Birimleri | Çıktı Kaynakları  |
|--------|---------|---------|
| 1K     |    1    |  2 TU   |
| 5K     |    6    |  6 TU   |
| 10K    |    12   |  10 TU  |

[Olay Hub](https://github.com/Azure-Samples/streaming-at-scale/tree/master/eventhubs-streamanalytics-eventhubs) çözümü akış birimleri (SU) ve iş hacmi açısından doğrusal olarak ölçeklendirerek, verileri Akış Analizi'nden analiz etmenin ve akışla geçirmenin en verimli ve en etkili yolu haline getirir. İşler 192 SU'ya kadar büyütülebilir ve bu da kabaca günde 200 MB/s'ye veya günde 19 trilyon olaya kadar işlenme anlamına gelir.

#### <a name="azure-sql"></a>Azure SQL
|Yutma Oranı (saniyedeki olaylar) | Akış Birimleri | Çıktı Kaynakları  |
|---------|------|-------|
|    1K   |   3  |  S3   |
|    5K   |   18 |  P4   |
|    10K  |   36 |  P6   |

[Azure SQL,](https://github.com/Azure-Samples/streaming-at-scale/tree/master/eventhubs-streamanalytics-azuresql) Devralma Bölümü adı verilen paralel yazmayı destekler, ancak varsayılan olarak etkinleştirmez. Ancak, Devralma Bölümlemesini etkinleştirmek, tamamen paralel bir sorguyla birlikte, daha yüksek iş girdilerine ulaşmak için yeterli olmayabilir. SQL yazma iş girişleri önemli ölçüde SQL Azure veritabanı yapılandırmanıza ve tablo şemanıza bağlıdır. [SQL Output Performance](./stream-analytics-sql-output-perf.md) makalesi, yazma iş parçacığınızı en üst düzeye çıkarabilecek parametreler hakkında daha fazla ayrıntıya sahiptir. Azure Akış [Analizi çıktısında azure SQL Veritabanı makalesinde](./stream-analytics-sql-output-perf.md#azure-stream-analytics) belirtildiği gibi, bu çözüm doğrusal olarak 8 bölümün ötesinde tam paralel bir ardışık işlem olarak ölçeklendirilmez ve SQL çıkışından önce yeniden bölümlenmesi gerekebilir (bkz. [INTO).](https://docs.microsoft.com/stream-analytics-query/into-azure-stream-analytics#into-shard-count) Premium SK'lar, birkaç dakikada bir gerçekleşen günlük yedeklemelerinden gelen genel merkezlerle birlikte yüksek IO oranlarını sürdürmek için gereklidir.

#### <a name="cosmos-db"></a>Cosmos DB
|Yutma Oranı (saniyedeki olaylar) | Akış Birimleri | Çıktı Kaynakları  |
|-------|-------|---------|
|  1K   |  3    | 20K RU  |
|  5K   |  24   | 60K RU  |
|  10K  |  48   | 120K RU |

Stream Analytics'in [Cosmos DB](https://github.com/Azure-Samples/streaming-at-scale/tree/master/eventhubs-streamanalytics-cosmosdb) çıktısı, [uyumluluk düzeyi 1.2](./stream-analytics-documentdb-output.md#improved-throughput-with-compatibility-level-12)altında yerel tümleştirmeyi kullanacak şekilde güncelleştirildi. Uyumluluk düzeyi 1.2 önemli ölçüde daha yüksek iş elde edilmesini sağlar ve yeni işler için varsayılan uyumluluk düzeyi olan 1.1'e kıyasla RU tüketimini azaltır. Çözüm , /deviceId'de bölümlenmiş CosmosDB kapları kullanır ve çözümün geri kalanı aynı şekilde yapılandırılmıştır.

[Ölçekte Tüm Akış azure örnekleri,](https://github.com/Azure-Samples/streaming-at-scale) test istemcilerini girdi olarak yükleyerek beslenen bir Olay Hub'ı kullanır. Her giriş olayı, yapılandırılmış alım oranlarını kolayca işlenme hızlarına (1MB/s, 5MB/sve 10MB ve 10MB/s) kolayca çeviren bir 1KB JSON belgesidir. Olaylar, 1K'ya kadar aygıtlar için aşağıdaki JSON verilerini (kısaltılmış biçimde) gönderen bir IoT aygıtını simüle eder:

```
{
    "eventId": "b81d241f-5187-40b0-ab2a-940faf9757c0",
    "complexData": {
        "moreData0": 51.3068118685458,
        "moreData22": 45.34076957651598
    },
    "value": 49.02278128887753,
    "deviceId": "contoso://device-id-1554",
    "type": "CO2",
    "createdAt": "2019-05-16T17:16:40.000003Z"
}
```

> [!NOTE]
> Yapılandırmalar, çözümde kullanılan çeşitli bileşenler nedeniyle değişebilir. Daha doğru bir tahmin için, örnekleri senaryonuza uyacak şekilde özelleştirin.

### <a name="identifying-bottlenecks"></a>Darboğazları Tanımlama

Ardışık hattınızdaki darboğazları belirlemek için Azure Akış Analizi işinizdeki Ölçümler bölmesini kullanın. İş giriş hızına ayak uydurup uymadın mı görmek için **giriş/Çıkış Olaylarını** ve ["Filigran Gecikmesi"](https://azure.microsoft.com/blog/new-metric-in-azure-stream-analytics-tracks-latency-of-your-streaming-pipeline/) veya **Backlogged Olayları** gözden geçirin. Olay Hub ölçümleri **için, Daraltılmış İstekleri** arayın ve Eşik Birimlerini buna göre ayarlayın. Cosmos DB ölçümleri için, bölüm anahtar aralıklarınızın eşit şekilde tüketilmesini sağlamak için İşlem altında **bölüm başına RU/s tüketilen Max'i** gözden geçirin. Azure SQL DB için **Log IO** ve **CPU'yı**izleyin.

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

