---
title: Azure Stream Analytics'te sorgu paralelleştirme ve ölçek kullanın
description: Bu makalede, giriş bölümlerini yapılandırma, Sorgu tanımını ayarlayarak ve akış birimleri iş ayarlama Stream Analytics işlerini ölçeklendirme açıklar.
author: JSeb225
ms.author: jeanb
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 05/07/2018
ms.openlocfilehash: d1afb6037b5fc290de93faba405982ebd1fb68ea
ms.sourcegitcommit: 509b39e73b5cbf670c8d231b4af1e6cfafa82e5a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78364589"
---
# <a name="leverage-query-parallelization-in-azure-stream-analytics"></a>Azure Stream analytics'te sorgu paralelleştirmesinden
Bu makalede, Azure Stream Analytics'te paralelleştirme yararlanmak işlemini göstermektedir. Giriş bölümlerini yapılandırma ve analytics Sorgu tanımını ayarlayarak Stream Analytics işlerini ölçeklendirmeyi öğrenin.
Bir önkoşul olarak, [akış birimlerinin anlaşılması ve ayarlanması](stream-analytics-streaming-unit-consumption.md)bölümünde açıklanan akış birimi kavramı hakkında bilgi sahibi olmak isteyebilirsiniz.

## <a name="what-are-the-parts-of-a-stream-analytics-job"></a>Bir Stream Analytics işi bölümlerini nelerdir?
Stream Analytics iş tanımı girişleri, sorgu ve çıkış içerir. Burada işi veri akışından okur girişlerdir. Veri giriş akışını dönüştürmek için kullanılan sorgu ve iş için iş sonuçlarını göndereceği yeri çıkış alınır.

Bir iş akış verileri için en az bir giriş kaynağı gerektirir. Veri akışı giriş kaynağı, bir Azure olay hub'ı veya Azure blob depolama alanında depolanabilir. Daha fazla bilgi için bkz. [Azure Stream Analytics giriş](stream-analytics-introduction.md) ve [Azure Stream Analytics kullanmaya başlama](stream-analytics-real-time-fraud-detection.md).

## <a name="partitions-in-sources-and-sinks"></a>Kaynaklar ve havuzlar bölümler
Bir Stream Analytics işi ölçeklendirme, giriş veya çıkış bölümlerinde avantajlarından yararlanır. Verileri bölüm anahtarına göre alt kümelerini böler sağlar bölümleme. Verileri (örneğin, bir akış analizi işi) kullanan bir işlem kullanabilir ve aktarım hızını artıran paralel olarak, farklı bölümler yazma. 

### <a name="inputs"></a>Girişler
Tüm Azure Stream Analytics giriş bölümleme yararlanabilirsiniz:
-   EventHub (bölüm anahtarı PARTITION BY anahtar sözcüğü ile açıkça ayarlamak için gereklidir)
-   IOT hub'ı (bölüm anahtarı PARTITION BY anahtar sözcüğü ile açıkça ayarlamak için gereklidir)
-   Blob depolama

### <a name="outputs"></a>Çıkışlar

Stream Analytics ile çalışırken, yapılandırma çıkışları bölümleme yararlanabilirsiniz:
-   Azure Data Lake Storage
-   Azure İşlevleri
-   Azure Tablosu
-   Blob depolama (bölüm anahtarı, açıkça ayarlayabilirsiniz)
-   Cosmos DB (Bölüm anahtarını açıkça ayarlamanız gerekir)
-   Event Hubs (Bölüm anahtarını açıkça ayarlamanız gerekir)
-   IOT hub'ı (bölüm anahtarı açıkça ayarlamak için gereklidir)
-   Service Bus
- İsteğe bağlı bölümlendirme ile SQL ve SQL veri ambarı: [Azure SQL veritabanı 'Na çıkış](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-sql-output-perf)hakkında daha fazla bilgi için bkz.

Power BI Bölümlendirmeyi desteklemiyor. Ancak, [Bu bölümde](#multi-step-query-with-different-partition-by-values) açıklandığı gibi girişi yine de bölümleyebilirsiniz 

Bölümleri hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

* [Event Hubs özelliklerine genel bakış](../event-hubs/event-hubs-features.md#partitions)
* [Veri bölümleme](https://docs.microsoft.com/azure/architecture/best-practices/data-partitioning)


## <a name="embarrassingly-parallel-jobs"></a>Utandırıcı derecede paralel işleri
Azure Stream Analytics olduğumuz en ölçeklenebilir senaryo, *emantik bir paralel* iş. Bu çıkış bir bölüm için bir bölüm bir örnek sorgu girişi bağlanır. Bu paralellik aşağıdaki gereksinimlere sahiptir:

1. Sorgu mantığınızı aynı sorgu örneği tarafından işlenmekte olan aynı anahtara bağlı olduğu durumlarda olayları girişlerinizin aynı bölüme gideceği emin olmanız gerekir. Event Hubs veya IoT Hub için, bu, olay verilerinin **Partitionkey** değer kümesine sahip olması gerektiği anlamına gelir. Alternatif olarak, bölümlenmiş Gönderenler kullanabilirsiniz. BLOB Depolama için bu olaylar aynı bölüm klasörüne gönderilir anlamına gelir. Sorgu mantığınızı aynı sorgu örneği tarafından işlenmek üzere aynı anahtar gerektirmiyorsa, bu gereksinim yoksayabilirsiniz. Bu mantık örneği basit bir select proje filtresi sorgusu olacaktır.  

2. Veri giriş tarafında düzenlendiğini sonra sorgunuzu bölümlenen emin olmanız gerekir. Bu, tüm adımlarda **bölüm** kullanmanızı gerektirir. Birden çok adım izin verilir, ancak bunların tümü aynı anahtarla bölümlenmesi gerekir. Uyumluluk düzeyi 1,0 ve 1,1 altında, işin tam olarak paralel olması için bölümleme anahtarı **PartitionID** olarak ayarlanmalıdır. Uyumluluk düzeyi 1,2 ve üzeri olan işler için, özel sütun giriş ayarlarında bölüm anahtarı olarak belirtilebilir ve iş, Bölüm BY yan tümcesi olmadan bile otomatik olarak paralellized olacaktır. Olay Hub 'ı çıkışı için "bölüm anahtarı sütunu" özelliği "PartitionID" kullanılacak şekilde ayarlanmalıdır.

3. Bizim çıkış çoğunu bölümleme avantajından yararlanmak, bir çıkış türü kullanıyorsanız, bölümleme desteklemez ancak işinizi tam olarak paralel olmayacaktır. Daha fazla ayrıntı için [çıkış bölümüne](#outputs) bakın.

4. Giriş bölüm sayısı, çıkış bölüm sayısına eşit olmalıdır. BLOB Depolama çıkışı bölümler destekleyebilir ve Yukarı Akış sorgunun bölümleme düzeni devralır. Blob Depolama belirtilmişse, veri için bölüm anahtarı giriş bölümünü bölümlenmiş, bu nedenle yine de tam olarak paralel sonucudur. Tam olarak paralel bir iş çalıştırılmasına izin bölüm değerlerini örnekleri aşağıda verilmiştir:

   * Bölüm 8 olay hub'ı giriş bölümleri ve 8 olay hub'ı çıkışı
   * 8 olay hub'ı giriş bölümleri ve blob depolama çıkışı
   * 8 olay hub'ı giriş bölümleri ve özel bir alana göre rastgele kardinaliteyle bölümlenmiş blob depolama çıkışı
   * 8 blob depolama giriş bölüm ve blob depolama çıkışı
   * Depolama giriş bölümleri ve 8 olay hub'ı çıkış bölüm 8 blob

Aşağıdaki bölümlerde utandırıcı derecede paralel bazı örnek senaryolar açıklanmaktadır.

### <a name="simple-query"></a>Basit sorgu

* Giriş: 8 bölüm olay hub'ı
* Çıkış: 8 bölümlü Olay Hub 'ı ("bölüm anahtarı sütunu" "PartitionID" kullanacak şekilde ayarlanmalıdır)

Sorgu:

```SQL
    SELECT TollBoothId
    FROM Input1 Partition By PartitionId
    WHERE TollBoothId > 100
```

Bu sorguyu basit bir filtredir. Bu nedenle, olay hub'ına gönderilen giriş bölümlendirme hakkında endişe etmeniz gerekmez. Uyumluluk düzeyi 1,2 ' dan önce **bölüm PartitionID** yan tümcesini içermelidir, bu nedenle daha önce gereksinim #2 gereksinimini karşılar. Çıktı için, Bölüm anahtarının **PartitionID**olarak ayarlanması için, işteki Olay Hub 'ı çıkışını yapılandırmamız gerekir. Bir son onay giriş bölüm sayısı çıkış bölüm sayısına eşit olduğundan emin olmaktır.

### <a name="query-with-a-grouping-key"></a>Bir gruplandırma anahtar ile sorgulama

* Giriş: 8 bölüm olay hub'ı
* Çıkış: Blob Depolama

Sorgu:

```SQL
    SELECT COUNT(*) AS Count, TollBoothId
    FROM Input1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
```

Bu sorgu, bir gruplandırma anahtarına sahiptir. Bu nedenle, gruplandırılmış olayları olay hub'ı aynı bölüme gönderilmesi gerekir. Bu örnekte biz tarafından TollBoothID grubunda olduğundan, biz olayları olay Hub'ına gönderildiğinde TollBoothID bölüm anahtarı olarak kullanıldığından emin olmalıdır. Ardından, bu bölüm düzeninden devralması ve tam paralelleştirme özelliğini etkinleştirmek için, bu bölüm **tarafından PartitionID Ile bölüm** kullanabiliriz. Çıktı blob depolama olduğundan, gereksinim #4 ilişkin bir bölüm anahtarı değerini yapılandırma hakkında endişe etmeniz gerekmez.

## <a name="example-of-scenarios-that-are-not-embarrassingly-parallel"></a>Emsanki paralel *olmayan* senaryolara örnek

Önceki bölümde, biz utandırıcı derecede paralel bazı senaryolar gösterilmiştir. Bu bölümde, utandırıcı derecede paralel olarak tüm gereksinimlerini senaryoları ele alır. 

### <a name="mismatched-partition-count"></a>Uyuşmayan bölüm sayısı
* Giriş: 8 bölüm olay hub'ı
* Çıkış: Olay hub'ı ile 32 bölümlü

Bu durumda, sorgu nedir önemi yoktur. Giriş bölüm sayısı olan çıkış bölüm sayısı eşleşmezse, topoloji utandırıcı derecede değil paralel. + ancak biz yine de bazı düzeyi veya paralelleştirme elde edebilirsiniz.

### <a name="query-using-non-partitioned-output"></a>Bölümlenmemiş çıkış kullanarak sorgulama
* Giriş: 8 bölüm olay hub'ı
* Çıkış: Power BI

Power BI çıktısı şu anda Bölümlendirmeyi desteklemiyor. Bu nedenle, bu senaryo utandırıcı derecede paralel değil.

### <a name="multi-step-query-with-different-partition-by-values"></a>Çok adımlı sorgunun PARTITION BY farklı değerlerle
* Giriş: 8 bölüm olay hub'ı
* Çıkış: Olay hub'ı 8 bölüm ile

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

Gördüğünüz gibi, ikinci adım bölümlendirme anahtarı olarak **Tollboothıd** kullanır. Bu adım ilk adım ile aynı değildir ve bu nedenle bize bir karışık yapmanız gerekir. 

Önceki örneklerde utandırıcı derecede paralel bir topoloji uygun (veya yok) bazı Stream Analytics işleri gösterir. Uygun, en yüksek ölçek olası sahiptirler. Bir kılavuz ölçeklendirme, bu profillerin uygun olmayan işler gelecekte kullanıma sunulacak yönelik güncelleştirmeler. Şu an için genel kılavuz aşağıdaki bölümlerde kullanın.

### <a name="compatibility-level-12---multi-step-query-with-different-partition-by-values"></a>Uyumluluk düzeyi 1,2-değerlere göre farklı BÖLÜMLERI olan çok adımlı sorgu 
* Giriş: 8 bölüm olay hub'ı
* Çıkış: 8 bölümlü ("bölüm anahtarı sütunu") Olay Hub 'ı "Tollboothıd" kullanılacak şekilde ayarlanmalıdır

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

Uyumluluk düzeyi 1,2, paralel sorgu yürütmeyi varsayılan olarak sunar. Örneğin, önceki bölümden sorgu, "Tollboothıd" sütunu giriş bölümü anahtarı olarak ayarlandığı sürece parteklenecektir. Parttionıd yan tümcesine göre bölüm gerekli değil.

## <a name="calculate-the-maximum-streaming-units-of-a-job"></a>Akış birimleri, bir işin en büyük hesaplayın
Stream Analytics işi tarafından kullanılan akış birimlerinin toplam sayısı, iş ve her adım için bölüm sayısı için tanımlanan sorgu, adım sayısı bağlıdır.

### <a name="steps-in-a-query"></a>Sorgu adımları
Bir sorgu, bir veya daha fazla adım olabilir. Her adım **WITH** anahtar sözcüğü tarafından tanımlanan bir alt sorgu olur. WITH anahtar sözcüğü dışında (yalnızca bir sorgu) sorgu, aşağıdaki sorgudaki **Select** ifadesiyle **birlikte** bir adım olarak da sayılır:

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

Bu sorgu, iki adımı vardır.

> [!NOTE]
> Bu sorgu, makalenin ilerleyen bölümlerinde daha ayrıntılı olarak ele alınmıştır.
>  

### <a name="partition-a-step"></a>Bir adım bölümleme
Bir adım bölümleme aşağıdaki koşulları gerektirir:

* Giriş kaynağı bölümlenmiş olması gerekir. 
* Sorgunun **Select** ifadesinin bölümlenmiş bir giriş kaynağından okuması gerekir.
* Adım içindeki sorgunun **bölüm** anahtar sözcüğüne sahip olması gerekir.

Bir sorgu bölümlenmiş, işlenen ve toplu olarak ayrı bölüm grupları giriş olaylardır ve olayları çıkışlar grupların her biri için oluşturulur. Birleşik toplama istiyorsanız, bölümlenmemiş bir ikinci adım oluşturmalısınız toplanacak.

### <a name="calculate-the-max-streaming-units-for-a-job"></a>Akış birimleri, bir iş için en fazla Hesapla
Tüm adımları bölümlenmemiş en fazla altı akış birimleri (su) için bir Stream Analytics işi birlikte ölçeklendirebilirsiniz. Buna ek olarak, bölümlenmiş bir adımda her bölüm 6 SUs ekleyebilirsiniz.
Aşağıdaki tabloda bazı **örnekleri** görebilirsiniz.

| Sorgu                                               | İş için en çok SUs |
| --------------------------------------------------- | ------------------- |
| <ul><li>Sorgu bir adım içerir.</li><li>Adım bölümlenmiş değil.</li></ul> | 6 |
| <ul><li>Giriş veri akışını 16 göre bölümlenir.</li><li>Sorgu bir adım içerir.</li><li>Adım bölümlenir.</li></ul> | 96 (6 * 16 bölümler) |
| <ul><li>Sorgu iki adımı içeriyor.</li><li>Adımları hiçbiri bölümlenir.</li></ul> | 6 |
| <ul><li>Giriş veri akışını 3 göre bölümlenir.</li><li>Sorgu iki adımı içeriyor. Giriş adım bölümlenmiş ve ikinci adım değil.</li><li><strong>Select</strong> deyimleri bölümlenmiş girişten okur.</li></ul> | 24 (bölümlenmiş adımları 18 + bölümlenmemiş adımları için 6 |

### <a name="examples-of-scaling"></a>Ölçeklendirme örnekleri

Aşağıdaki sorgu, üç tollbooths sahip Ücretli istasyonu giderek bir üç dakikalık penceresi içinde otomobiller sayısını hesaplar. Bu sorgu, altı SUs kadar ölçeklendirilebilir.

```SQL
    SELECT COUNT(*) AS Count, TollBoothId
    FROM Input1
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
```

Sorgu için daha fazla SUs kullanmak için giriş veri akışını hem de sorgu bölümlenmiş olması gerekir. Veri akışı bölüm 3 olarak ayarlandığından, aşağıdaki değiştirilen sorguyu en fazla 18 SUs ölçeklendirilebilir:

```SQL
    SELECT COUNT(*) AS Count, TollBoothId
    FROM Input1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
```

Bir sorgu bölümlenmiş, giriş olayları işlenir ve ayrı bölüm grupları içinde toplanır. Çıkış olayları da grupların her biri için oluşturulur. **Gruplandırma** alanı, giriş veri akışındaki bölüm anahtarı olmadığında bölümlendirme bazı beklenmedik sonuçlara neden olabilir. Örneğin, önceki sorgudaki **Tollboothıd** alanı **Input1**bölüm anahtarı değildir. Birden çok bölüm gişe #1 verilerden yayılabilen sonucudur.

**Input1** bölümlerinin her biri Stream Analytics tarafından ayrı olarak işlenir. Sonuç olarak, araba sayısı aynı atlayan pencere içinde aynı gişe için birden çok kayıt oluşturulur. Giriş bölüm anahtarı değiştirilemez, aşağıdaki örnekte olduğu gibi bölümler arasında toplama değerleri için bir bölüm olmayan adım ekleyerek bu sorun düzeltilebilir:

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

Bu sorgu için 24 SUs ölçeklendirilebilir.

> [!NOTE]
> İki birleştirilecekse, akışları birleştirmeler oluşturmak için kullandığınız bir sütun bölüm anahtarı bölümlenir emin olun. Ayrıca her iki akış bölümleri aynı sayıda sahip olduğunuzdan emin olun.
> 
> 

## <a name="achieving-higher-throughputs-at-scale"></a>Daha yüksek olan Through, ölçeğe göre elde

Büyük bir [paralel](#embarrassingly-parallel-jobs) iş gereklidir, ancak ölçeğe göre daha yüksek bir üretilen işi sürdürmek için yeterli değildir. Her depolama sistemi ve buna karşılık gelen Stream Analytics çıktısı, olası en iyi yazma verimini elde etme hakkında değişimlere sahiptir. Her türlü ölçeklendirme senaryosunda olduğu gibi, doğru yapılandırma kullanılarak çözülebilecek bazı zorluklar vardır. Bu bölümde, birkaç ortak çıkış için yapılandırma ele alınmaktadır ve saniye başına 1 k, 5K ve 10.000 olay alma fiyatları için örnekler sağlanmaktadır.

Aşağıdaki gözlemler, Olay Hub 'ı, Azure SQL DB 'ye veya Cosmos DB yazan temel bir JavaScript UDF 'i olan durum bilgisiz (PASSTHROUGH) sorgusuyla bir Stream Analytics işi kullanır.

#### <a name="event-hub"></a>Olay Hub'ı

|Alım oranı (saniye başına olay) | Akış birimleri | Çıkış kaynakları  |
|--------|---------|---------|
| 1000     |    1    |  2 TU   |
| 5K     |    6    |  6 TU   |
| Kesme    |    12   |  10 TU  |

[Olay Hub 'ı](https://github.com/Azure-Samples/streaming-at-scale/tree/master/eventhubs-streamanalytics-eventhubs) çözümü, akış BIRIMLERI (su) ve verimlilik açısından doğrusal bir şekilde ölçeklendirirken, verileri Stream Analytics çözümlemek ve veri akışı yapmak için en verimli ve performanslı bir yoldur. İşler 192 SU 'e kadar ölçeklendirilebilir ve bu da günde en fazla 200 MB/sn veya 19.000.000.000.000 olay olarak çeviri yapılabilir.

#### <a name="azure-sql"></a>Azure SQL
|Alım oranı (saniye başına olay) | Akış birimleri | Çıkış kaynakları  |
|---------|------|-------|
|    1000   |   3  |  S3   |
|    5K   |   18 |  P4   |
|    Kesme  |   36 |  P6   |

[Azure SQL](https://github.com/Azure-Samples/streaming-at-scale/tree/master/eventhubs-streamanalytics-azuresql) , yazmayı devralma adlı paralel olarak yazmayı destekler, ancak varsayılan olarak etkinleştirilmemiştir. Ancak, tümüyle paralel bir sorgu ile birlikte Bölümlendirmeyi etkinleştirmek, daha yüksek bir yük devretmede elde etmek için yeterli olmayabilir. SQL Write Through, SQL Azure veritabanı yapılandırmanıza ve tablo şemanıza önemli ölçüde bağlıdır. [SQL çıkış performansı](./stream-analytics-sql-output-perf.md) makalesi, yazma aktarım hızını en üst düzeye çıkarabileceğiniz parametreler hakkında daha fazla ayrıntı içerir. Bu çözüm, [Azure SQL veritabanı 'na Azure Stream Analytics çıkışı](./stream-analytics-sql-output-perf.md#azure-stream-analytics) bölümünde belirtildiği gibi, 8 bölümden daha büyük bir paralel işlem hattı olarak doğrusal şekilde ölçeklendirmez ve SQL çıktısından önce yeniden bölümlenmesi gerekebilir ( [bkz.](https://docs.microsoft.com/stream-analytics-query/into-azure-stream-analytics#into-shard-count)). Premium SKU 'Lar, çok sayıda dakikada bir oluşan günlük yedeklerinden gelen ek yükün yanı sıra yüksek GÇ ücretleri için de gereklidir.

#### <a name="cosmos-db"></a>Cosmos DB
|Alım oranı (saniye başına olay) | Akış birimleri | Çıkış kaynakları  |
|-------|-------|---------|
|  1000   |  3    | 20K RU  |
|  5K   |  24   | 60K RU  |
|  Kesme  |  48   | 120K RU |

Stream Analytics çıkış [Cosmos DB](https://github.com/Azure-Samples/streaming-at-scale/tree/master/eventhubs-streamanalytics-cosmosdb) , [Uyumluluk düzeyi 1,2](./stream-analytics-documentdb-output.md#improved-throughput-with-compatibility-level-12)altında yerel tümleştirmeyi kullanacak şekilde güncelleştirilmiştir. Uyumluluk düzeyi 1,2, önemli ölçüde daha yüksek aktarım hızı sağlar ve yeni işler için varsayılan uyumluluk düzeyi olan 1,1 ile karşılaştırıldığında RU tüketimini azaltır. Çözüm,/DeviceID üzerinde bölümlenmiş CosmosDB kapsayıcılarını kullanır ve geri kalan çözüm de aynı şekilde yapılandırılmıştır.

[Azure örnekleri ölçeğinde tüm akışlar](https://github.com/Azure-Samples/streaming-at-scale) , test istemcilerinin giriş olarak benzetimini yaparak yük tarafından beslenen bir olay hub 'ı kullanır. Her giriş olayı bir 1 KB JSON belgesidir. Bu, yapılandırılan Alım tarifelerinin hızını işleme oranlarına (1MB/s, 5 MB/s ve 10 GB/sn) kolayca dönüştürür. Olaylar, en fazla 1K cihaz için aşağıdaki JSON verilerini (kısaltılmış bir biçimde) gönderen bir IoT cihazının benzetimini yapar:

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
> Yapılandırmalarda kullanılan çeşitli bileşenler nedeniyle yapılandırmaların değiştirilmesi tabidir. Daha doğru bir tahmin için, örnekleri senaryonuza uyacak şekilde özelleştirin.

### <a name="identifying-bottlenecks"></a>Performans sorunlarını belirleme

İşlem hattınızdaki performans sorunlarını belirlemek için Azure Stream Analytics işinizin ölçümler bölmesini kullanın. İşin giriş oranına sahip olup olmadığını görmek için üretilen iş ve ["filigran gecikmesi"](https://azure.microsoft.com/blog/new-metric-in-azure-stream-analytics-tracks-latency-of-your-streaming-pipeline/) veya **biriktirme listesi olayları** Için **giriş/çıkış olaylarını** gözden geçirin. Olay Hub 'ı ölçümleri için, **Kısıtlanmış istekleri** bulun ve eşik birimlerini uygun şekilde ayarlayın. Cosmos DB ölçümleri için, bölüm anahtarı aralıklarınızın her bir şekilde tüketildiğinden emin olmak için üretilen Iş birimi **anahtar aralığı başına en fazla ru/sn** 'yi gözden geçirin. Azure SQL DB için **günlük GÇ** ve **CPU 'yu**izleyin.

## <a name="get-help"></a>Yardım alın

Daha fazla yardım için [Azure Stream Analytics Forumumuzu](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics)deneyin.

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Stream Analytics giriş](stream-analytics-introduction.md)
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

