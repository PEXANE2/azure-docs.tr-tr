---
title: Sorgu paralelleştirme ve ölçek kullanın Azure Stream Analytics
description: Bu makalede, giriş bölümlerini yapılandırma, sorgu tanımını ayarlama ve iş akışı birimlerinin ayarlanmasıyla Stream Analytics işlerin nasıl ölçeklendiriyapılacağı açıklanır.
author: JSeb225
ms.author: jeanb
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 05/04/2020
ms.openlocfilehash: ead0041e26b5dff5cfd81b6fa02b7efff6e6e9d1
ms.sourcegitcommit: 0b80a5802343ea769a91f91a8cdbdf1b67a932d3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/25/2020
ms.locfileid: "83831204"
---
# <a name="leverage-query-parallelization-in-azure-stream-analytics"></a>Azure Stream Analytics sorgu paralelleştirme özelliğinden yararlanın
Bu makalede, Azure Stream Analytics paralelleştirme avantajlarından nasıl yararlanabilmeniz gösterilmektedir. Giriş bölümlerini yapılandırarak ve analiz sorgu tanımını ayarlayarak Stream Analytics işlerinin nasıl ölçeklendirileyeceğinizi öğrenirsiniz.
Bir önkoşul olarak, [akış birimlerinin anlaşılması ve ayarlanması](stream-analytics-streaming-unit-consumption.md)bölümünde açıklanan akış birimi kavramı hakkında bilgi sahibi olmak isteyebilirsiniz.

## <a name="what-are-the-parts-of-a-stream-analytics-job"></a>Stream Analytics işinin parçaları nelerdir?
Stream Analytics iş tanımı en az bir akış girişi, bir sorgu ve çıkış içerir. Girişler, işin veri akışını okuduğu yerdir. Sorgu, veri girişi akışını dönüştürmek için kullanılır ve çıktı işin iş sonuçlarını göndereceği yerdir.

## <a name="partitions-in-inputs-and-outputs"></a>Giriş ve çıkışdaki bölümler
Bölümlendirme, verileri [bölüm anahtarına](https://docs.microsoft.com/azure/event-hubs/event-hubs-scalability#partitions)göre alt kümelere bölmenizi sağlar. Giriş (örneğin Event Hubs) bir anahtarla bölümlense, Stream Analytics işinize giriş eklenirken Bu bölüm anahtarını belirtmeniz kesinlikle önerilir. Stream Analytics işi ölçeklendirme, giriş ve çıkışdaki bölümlerden yararlanır. Stream Analytics bir iş, farklı bölümleri paralel olarak kullanabilir ve yazabilir, bu da üretilen işi artırır. 

### <a name="inputs"></a>Girişler
Tüm Azure Stream Analytics girişi bölümlemenin avantajlarından yararlanabilir:
-   EventHub (uyumluluk düzeyi 1,1 veya altı kullanılıyorsa bölüm anahtarı ile PARTITION anahtar sözcüğü ile açık olarak ayarlamanız gerekir)
-   IoT Hub (uyumluluk düzeyi 1,1 veya altı kullanılıyorsa bölüm anahtarı ile PARTITION BY anahtar sözcüğüyle açık olarak ayarlamanız gerekir)
-   Blob depolama

### <a name="outputs"></a>Çıkışlar

Stream Analytics ile çalışırken çıktılardan bölümlemeden yararlanabilirsiniz:
-   Azure Data Lake Storage
-   Azure İşlevleri
-   Azure Tablosu
-   BLOB depolama (Bölüm anahtarını açıkça ayarlayabilir)
-   Cosmos DB (Bölüm anahtarını açıkça ayarlamanız gerekir)
-   Event Hubs (Bölüm anahtarını açıkça ayarlamanız gerekir)
-   IoT Hub (Bölüm anahtarını açıkça ayarlamanız gerekir)
-   Service Bus
- İsteğe bağlı bölümlendirme ile SQL ve SQL veri ambarı: [Azure SQL veritabanı 'Na çıkış](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-sql-output-perf)hakkında daha fazla bilgi için bkz..

Power BI Bölümlendirmeyi desteklemiyor. Ancak, [Bu bölümde](#multi-step-query-with-different-partition-by-values) açıklandığı gibi girişi yine de bölümleyebilirsiniz 

Bölümler hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

* [Event Hubs özelliklerine genel bakış](../event-hubs/event-hubs-features.md#partitions)
* [Veri bölümleme](https://docs.microsoft.com/azure/architecture/best-practices/data-partitioning)


## <a name="embarrassingly-parallel-jobs"></a>Embarmsski paralel işler
Azure Stream Analytics ' deki en ölçeklenebilir senaryo, *embarsanki paralel* bir iş. Girişin bir bölümünü bir sorgunun bir bölümüne, çıktının bir bölümüne bağlar. Bu paralellik aşağıdaki gereksinimlere sahiptir:

1. Sorgu mantığınızın aynı sorgu örneği tarafından işlendiği aynı anahtara bağlı olması durumunda, olayların girişin aynı bölümüne gitdiğinizden emin olmanız gerekir. Event Hubs veya IoT Hub için, bu, olay verilerinin **Partitionkey** değer kümesine sahip olması gerektiği anlamına gelir. Alternatif olarak, bölümlenmiş Gönderenler kullanabilirsiniz. BLOB depolama için bu, olayların aynı bölüm klasörüne gönderildiği anlamına gelir. Örnek, giriş Olay Hub 'ı bölüm anahtarı olarak kullanıcı kimliğine göre bölümlenen Kullanıcı adı başına verileri toplayan bir sorgu örneği olabilir. Ancak, sorgu mantığınızın aynı anahtar aynı sorgu örneği tarafından işlenmesini gerektirmiyorsa, bu gereksinimi yoksayabilirsiniz. Bu mantığın bir örneği basit bir Select-Project-Filter sorgusu olacaktır.  

2. Sonraki adım sorgunuzu bölümleyip bölümlendirilmelidir. Uyumluluk düzeyi 1,2 veya üzeri (önerilir) olan işler için, özel sütun giriş ayarlarında bölüm anahtarı olarak belirtilebilir ve iş otomatik olarak paralellized olacaktır. Uyumluluk düzeyi 1,0 veya 1,1 olan işler, sorgunuzun tüm adımlarında **bölüm, PARTITIONıD tarafından** kullanmanız gerekir. Birden çok adıma izin verilir, ancak tümünün aynı anahtarla bölümlenmesi gerekir. 

3. Stream Analytics desteklenen çıktıların çoğu bölümlemenin avantajlarından yararlanabilir. İşi Bölümlendirmeyi desteklemeyen bir çıkış türü kullanırsanız, işiniz *farkında*olmaz. Olay Hub 'ı çıktıları için **bölüm anahtarı sütununun** sorguda kullanılan aynı bölüm anahtarına ayarlandığından emin olun. Daha fazla ayrıntı için [çıkış bölümüne](#outputs) bakın.

4. Giriş bölümlerinin sayısı, çıkış bölümlerinin sayısına eşit olmalıdır. BLOB depolama çıktısı, bölümleri destekleyebilir ve yukarı akış sorgusunun bölümleme şemasını devralır. BLOB depolama için bir bölüm anahtarı belirtildiğinde, veriler giriş bölümü başına bölümlenir, bu nedenle sonuç hala tamamen paraleldir. Tam paralel bir işe izin veren bölüm değerlerinin örnekleri aşağıda verilmiştir:

   * 8 Olay Hub 'ı giriş bölümleri ve 8 Olay Hub 'ı çıkış bölümleri
   * 8 Olay Hub 'ı giriş bölümleri ve BLOB depolama çıkışı
   * Rastgele kardinalite ile özel bir alan tarafından bölümlenmiş 8 Olay Hub 'ı giriş bölümü ve BLOB depolama çıkışı
   * 8 BLOB depolama giriş bölümleri ve BLOB depolama çıkışı
   * 8 BLOB depolama giriş bölümleri ve 8 Olay Hub 'ı çıkış bölümleri

Aşağıdaki bölümlerde, emsanki paralel paralel bazı örnek senaryolar ele alınmaktadır.

### <a name="simple-query"></a>Basit sorgu

* Giriş: 8 bölümlü Olay Hub 'ı
* Çıkış: 8 bölümlü Olay Hub 'ı ("bölüm anahtarı sütunu" "PartitionID" kullanacak şekilde ayarlanmalıdır)

Sorgu:

```SQL
    --Using compatibility level 1.2 or above
    SELECT TollBoothId
    FROM Input1
    WHERE TollBoothId > 100
    
    --Using compatibility level 1.0 or 1.1
    SELECT TollBoothId
    FROM Input1 PARTITION BY PartitionId
    WHERE TollBoothId > 100
```

Bu sorgu basit bir filtredir. Bu nedenle, Olay Hub 'ına gönderilen girişi bölümlemek için endişelenmemiz gerekmiyor. Uyumluluk düzeyi 1,2 ' dan önce **bölüm PartitionID** yan tümcesini içermelidir, bu nedenle daha önce gereksinim #2 gereksinimini karşılar. Çıktı için, Bölüm anahtarının **PartitionID**olarak ayarlanması için, işteki Olay Hub 'ı çıkışını yapılandırmamız gerekir. Son denetim, giriş bölümlerinin sayısının, çıkış bölümlerinin sayısına eşit olduğundan emin olmanızı sağlar.

### <a name="query-with-a-grouping-key"></a>Gruplandırma anahtarı olan sorgu

* Giriş: 8 bölümlü Olay Hub 'ı
* Çıkış: BLOB depolama

Sorgu:

```SQL
    --Using compatibility level 1.2 or above
    SELECT COUNT(*) AS Count, TollBoothId
    FROM Input1
    GROUP BY TumblingWindow(minute, 3), TollBoothId
    
    --Using compatibility level 1.0 or 1.1
    SELECT COUNT(*) AS Count, TollBoothId
    FROM Input1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
```

Bu sorguda bir gruplandırma anahtarı vardır. Bu nedenle, birlikte gruplanmış olayların aynı Olay Hub 'ı bölümüne gönderilmesi gerekir. Bu örnekte, Tollboothıd tarafından gruplandırdığımız için, olaylar Olay Hub 'ına gönderildiğinde, bölüm anahtarı olarak Tollboothıd 'nin kullanıldığından emin olmanız gerekir. Ardından, bu bölüm düzeninden devralması ve tam paralelleştirme özelliğini etkinleştirmek için, bu bölüm **tarafından PartitionID Ile bölüm** kullanabiliriz. Çıkış blob depolaması olduğundan, gereksinim #4 göre bölüm anahtarı değerini yapılandırma konusunda endişelenmenize gerek kalmaz.

## <a name="example-of-scenarios-that-are-not-embarrassingly-parallel"></a>Emsanki paralel *olmayan* senaryolara örnek

Önceki bölümde bazı emantik paralel senaryolar gösterildi. Bu bölümde, tüm gereksinimleri karşılamayan, emstik paralel olması gereken senaryoları tartıştık. 

### <a name="mismatched-partition-count"></a>Eşleşmeyen bölüm sayısı
* Giriş: 8 bölümlü Olay Hub 'ı
* Çıkış: 32 bölümlü Olay Hub 'ı

Giriş bölümü sayısı, çıkış bölümü sayısıyla eşleşmiyorsa, sorgu ne olursa olsun, topoloji emson olarak paralel değildir. Ancak yine de bir düzey veya paralelleştirme elde edebilirsiniz.

### <a name="query-using-non-partitioned-output"></a>Bölümlenmemiş çıkış kullanan sorgu
* Giriş: 8 bölümlü Olay Hub 'ı
* Çıkış: Power BI

Power BI çıktısı şu anda Bölümlendirmeyi desteklemiyor. Bu nedenle, bu senaryo emanmaz paralel değildir.

### <a name="multi-step-query-with-different-partition-by-values"></a>Değerlere göre farklı BÖLÜMLERI olan çok adımlı sorgu
* Giriş: 8 bölümlü Olay Hub 'ı
* Çıkış: 8 bölümlü Olay Hub 'ı
* Uyumluluk düzeyi: 1,0 veya 1,1

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

Gördüğünüz gibi, ikinci adım bölümlendirme anahtarı olarak **Tollboothıd** kullanır. Bu adım ilk adımla aynı değildir ve bu nedenle bir karışık hale getirmemizi gerektirir. 

### <a name="multi-step-query-with-different-partition-by-values"></a>Değerlere göre farklı BÖLÜMLERI olan çok adımlı sorgu
* Giriş: 8 bölümlü Olay Hub 'ı
* Çıkış: 8 bölümlü ("bölüm anahtarı sütunu") Olay Hub 'ı "Tollboothıd" kullanılacak şekilde ayarlanmalıdır
* Uyumluluk düzeyi-1,2 veya üzeri

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

Uyumluluk düzeyi 1,2 veya üzeri, varsayılan olarak paralel sorgu yürütmeyi mümkün bir şekilde sunar. Örneğin, önceki bölümdeki sorgu "Tollboothıd" sütunu giriş bölümü anahtarı olarak ayarlandığı sürece bölümlenecek. PARTITION BY PartitionID yan tümcesi gerekli değildir.

## <a name="calculate-the-maximum-streaming-units-of-a-job"></a>Bir işin en fazla akış birimi sayısını hesaplama
Bir Stream Analytics işi tarafından kullanılabilecek toplam akış birimi sayısı, iş için tanımlanan sorgudaki adım sayısına ve her adımın bölüm sayısına bağlıdır.

### <a name="steps-in-a-query"></a>Sorgudaki adımlar
Sorguda bir veya daha fazla adım olabilir. Her adım **WITH** anahtar sözcüğü tarafından tanımlanan bir alt sorgu olur. WITH anahtar sözcüğü dışında (yalnızca bir sorgu) sorgu, aşağıdaki sorgudaki **Select** ifadesiyle **birlikte** bir adım olarak da sayılır:

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

Bu sorguda iki adım vardır.

> [!NOTE]
> Bu sorgu, makalenin ilerleyen bölümlerinde daha ayrıntılı bir şekilde ele alınmıştır.
>  

### <a name="partition-a-step"></a>Bir adımı bölümle
Bir adımın bölümlenmesi için aşağıdaki koşullar gereklidir:

* Giriş kaynağının bölümlenmiş olması gerekir. 
* Sorgunun **Select** ifadesinin bölümlenmiş bir giriş kaynağından okuması gerekir.
* Adım içindeki sorgunun **bölüm** anahtar sözcüğüne sahip olması gerekir.

Bir sorgu bölümlenmiş olduğunda, giriş olayları ayrı bölüm gruplarında işlenir ve toplanır ve her bir grup için çıkış olayları oluşturulur. Birleşik bir toplama isterseniz, toplanacak ikinci bölümlendirilmemiş bir adım oluşturmanız gerekir.

### <a name="calculate-the-max-streaming-units-for-a-job"></a>Bir iş için en fazla akış birimini hesapla
Tüm bölümlenmemiş adımlar, bir Stream Analytics işi için altı akış birimi (SUs) kadar ölçeklendirebilir. Buna ek olarak, bölümlenmiş bir adımda her bölüm için 6 SUs ekleyebilirsiniz.
Aşağıdaki tabloda bazı **örnekleri** görebilirsiniz.

| Sorgu                                               | İş için en fazla SUs |
| --------------------------------------------------- | ------------------- |
| <ul><li>Sorgu bir adım içerir.</li><li>Adım bölümlenmemiş.</li></ul> | 6 |
| <ul><li>Giriş veri akışı 16 ile bölümlenir.</li><li>Sorgu bir adım içerir.</li><li>Adım bölümlenmiş.</li></ul> | 96 (6 * 16 Bölüm) |
| <ul><li>Sorgu iki adım içerir.</li><li>Adımların hiçbiri bölümlenmemiş.</li></ul> | 6 |
| <ul><li>Giriş veri akışı 3 ' te bölümlenir.</li><li>Sorgu iki adım içerir. Giriş adımı bölümlenmiş ve ikinci adım değil.</li><li><strong>Select</strong> deyimleri bölümlenmiş girişten okur.</li></ul> | 24 (bölümlenmiş adımlarla ilgili olarak 16 bölümlenmemiş adımlar için 6 |

### <a name="examples-of-scaling"></a>Ölçeklendirme örnekleri

Aşağıdaki sorgu, üç dakikalık bir pencere içindeki otomobillerin sayısını üç tollya sahip olan ücretli bir istasyondan hesaplar. Bu sorgu, altı adede kadar SUs 'e kadar ölçeklendirilebilir.

```SQL
    SELECT COUNT(*) AS Count, TollBoothId
    FROM Input1
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
```

Sorgu için daha fazla SUs kullanmak istiyorsanız, hem giriş veri akışı hem de sorgu bölümlenmiş olmalıdır. Veri akışı bölümü 3 olarak ayarlandığı için, aşağıdaki değiştirilen sorgu 18 adede kadar ölçeklendirilebilir:

```SQL
    SELECT COUNT(*) AS Count, TollBoothId
    FROM Input1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
```

Bir sorgu bölümlenmiş olduğunda, giriş olayları işlenir ve ayrı bölüm gruplarında toplanır. Her grup için çıkış olayları da oluşturulur. **Gruplandırma** alanı, giriş veri akışındaki bölüm anahtarı olmadığında bölümlendirme bazı beklenmedik sonuçlara neden olabilir. Örneğin, önceki sorgudaki **Tollboothıd** alanı **Input1**bölüm anahtarı değildir. Sonuç olarak, Tollstand #1 verileri birden çok bölüme yayılabilecek.

**Input1** bölümlerinin her biri Stream Analytics tarafından ayrı olarak işlenir. Sonuç olarak, aynı çıkış penceresinde aynı tollstand için araba sayısı birden çok kaydı oluşturulacaktır. Giriş bölümü anahtarı değiştirilemediğinde, aşağıdaki örnekte olduğu gibi, bölümler arasında değerleri toplamak için bölüm dışı bir adım eklenerek bu sorun düzeltilebilir:

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

Bu sorgu, 24 SUs 'e ölçeklendirilebilir.

> [!NOTE]
> İki akışı birleştiriyorsanız, akışların birleştirmeleri oluşturmak için kullandığınız sütunun bölüm anahtarına göre bölümlendiğinden emin olun. Ayrıca, her iki akışda aynı sayıda bölüm olduğundan emin olun.
> 
> 

## <a name="achieving-higher-throughputs-at-scale"></a>Daha yüksek olan Through, ölçeğe göre elde

Büyük bir [paralel](#embarrassingly-parallel-jobs) iş gereklidir, ancak ölçeğe göre daha yüksek bir üretilen işi sürdürmek için yeterli değildir. Her depolama sistemi ve buna karşılık gelen Stream Analytics çıktısı, olası en iyi yazma verimini elde etme hakkında değişimlere sahiptir. Her türlü ölçeklendirme senaryosunda olduğu gibi, doğru yapılandırma kullanılarak çözülebilecek bazı zorluklar vardır. Bu bölümde, birkaç ortak çıkış için yapılandırma ele alınmaktadır ve saniye başına 1 k, 5K ve 10.000 olay alma fiyatları için örnekler sağlanmaktadır.

Aşağıdaki gözlemler, Olay Hub 'ı, Azure SQL DB 'ye veya Cosmos DB yazan temel bir JavaScript UDF 'i olan durum bilgisiz (PASSTHROUGH) sorgusuyla bir Stream Analytics işi kullanır.

#### <a name="event-hub"></a>Olay Hub'ı

|Alım oranı (saniye başına olay) | Akış birimleri | Çıkış kaynakları  |
|--------|---------|---------|
| 1K     |    1    |  2 TU   |
| 5K     |    6    |  6 TU   |
| 10K    |    12   |  10 TU  |

[Olay Hub 'ı](https://github.com/Azure-Samples/streaming-at-scale/tree/master/eventhubs-streamanalytics-eventhubs) çözümü, akış BIRIMLERI (su) ve verimlilik açısından doğrusal bir şekilde ölçeklendirirken, verileri Stream Analytics çözümlemek ve veri akışı yapmak için en verimli ve performanslı bir yoldur. İşler 192 SU 'e kadar ölçeklendirilebilir ve bu da günde en fazla 200 MB/sn veya 19.000.000.000.000 olay olarak çeviri yapılabilir.

#### <a name="azure-sql"></a>Azure SQL
|Alım oranı (saniye başına olay) | Akış birimleri | Çıkış kaynakları  |
|---------|------|-------|
|    1K   |   3  |  S3   |
|    5K   |   18 |  P4   |
|    10K  |   36 |  P6   |

[Azure SQL](https://github.com/Azure-Samples/streaming-at-scale/tree/master/eventhubs-streamanalytics-azuresql) , yazmayı devralma adlı paralel olarak yazmayı destekler, ancak varsayılan olarak etkinleştirilmemiştir. Ancak, tümüyle paralel bir sorgu ile birlikte Bölümlendirmeyi etkinleştirmek, daha yüksek bir yük devretmede elde etmek için yeterli olmayabilir. SQL Write Through, SQL Azure veritabanı yapılandırmanıza ve tablo şemanıza önemli ölçüde bağlıdır. [SQL çıkış performansı](./stream-analytics-sql-output-perf.md) makalesi, yazma aktarım hızını en üst düzeye çıkarabileceğiniz parametreler hakkında daha fazla ayrıntı içerir. Bu çözüm, [Azure SQL veritabanı 'na Azure Stream Analytics çıkışı](./stream-analytics-sql-output-perf.md#azure-stream-analytics) bölümünde belirtildiği gibi, 8 bölümden daha büyük bir paralel işlem hattı olarak doğrusal şekilde ölçeklendirmez ve SQL çıktısından önce yeniden bölümlenmesi gerekebilir ( [bkz.](https://docs.microsoft.com/stream-analytics-query/into-azure-stream-analytics#into-shard-count)). Premium SKU 'Lar, çok sayıda dakikada bir oluşan günlük yedeklerinden gelen ek yükün yanı sıra yüksek GÇ ücretleri için de gereklidir.

#### <a name="cosmos-db"></a>Cosmos DB
|Alım oranı (saniye başına olay) | Akış birimleri | Çıkış kaynakları  |
|-------|-------|---------|
|  1K   |  3    | 20K RU  |
|  5K   |  24   | 60K RU  |
|  10K  |  48   | 120K RU |

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

Daha fazla yardım için, [Azure Stream Analytics Için Microsoft Q&soru sayfasını](https://docs.microsoft.com/answers/topics/azure-stream-analytics.html)deneyin.

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

