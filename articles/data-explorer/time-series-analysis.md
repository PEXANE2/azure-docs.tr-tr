---
title: Azure Veri Gezgini kullanarak zaman serisi verilerini çözümleme
description: Azure Veri Gezgini kullanarak bulutta zaman serisi verilerini çözümlemeyi öğrenin.
author: orspod
ms.author: orspodek
ms.reviewer: adieldar
ms.service: data-explorer
ms.topic: conceptual
ms.date: 04/07/2019
ms.openlocfilehash: 3873b25394f91ce1c1601c348de2098198ba7fdd
ms.sourcegitcommit: 6bb98654e97d213c549b23ebb161bda4468a1997
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2019
ms.locfileid: "74765492"
---
# <a name="time-series-analysis-in-azure-data-explorer"></a>Azure Veri Gezgini zaman serisi analizi

Azure Veri Gezgini (ADX), bulut hizmetlerinden veya IoT cihazlarından Telemetri verilerinin üzerine toplanmasını gerçekleştirir. Bu veriler, Service Health, fiziksel üretim süreçlerini ve kullanım eğilimlerini izleme gibi çeşitli Öngörüler için analiz edilebilir. Seçili ölçümlerin zaman serisinde, genel taban çizgisi düzeniyle karşılaştırıldığında, düzende bir sapma bulmak için analiz yapılır.
ADX birden çok zaman dizisinin oluşturulması, işlenmesi ve çözümlenmesi için yerel destek içerir. Bu konuda, ADX **'in binlerce zaman serisini**, yaklaşık gerçek zamanlı izleme çözümlerini ve iş akışlarına olanak sağlayan şekilde nasıl oluşturup analiz edeceğinizi öğrenin.

## <a name="time-series-creation"></a>Zaman serisi oluşturma

Bu bölümde, büyük bir normal zaman serisi kümesi oluşturacağız ve `make-series` işlecini kullanarak ve eksik değerleri gerektiği gibi doldurabilirsiniz.
Zaman serisi analizinin ilk adımı, orijinal telemetri tablosunu bölümleyip bir zaman serisi kümesine dönüştürmeye yönelik. Tablo genellikle zaman damgası sütunu, bağlamsal Boyutlar ve isteğe bağlı ölçümler içerir. Boyutlar, verileri bölümlemek için kullanılır. Amaç, düzenli zaman aralıklarıyla bölüm başına binlerce zaman serisi oluşturmaktır.

*Demo_make_series1* giriş tablosu, rastgele Web hizmeti trafiğinin 600k kayıtlarını içerir. 10 kayıt örneklemek için aşağıdaki komutu kullanın:

[**sorguyu çalıştırmak için\[tıklayın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAAA0tJzc2Pz03MTo0vTi3KTC02VKhRKAFyFQwNADOyzKUbAAAA) **\]**

```kusto
demo_make_series1 | take 10 
```

Elde edilen tablo bir zaman damgası sütunu, üç bağlamsal boyutlar sütunu ve ölçüm içermez:

|   |   |   |   |   |
| --- | --- | --- | --- | --- |
|   | Ilişkin | BrowserVer | OsVer | Ülke/Bölge |
|   | 2016-08-25 09:12:35.4020000 | Chrome 51,0 | Windows 7 | Birleşik Krallık |
|   | 2016-08-25 09:12:41.1120000 | Chrome 52,0 | Windows 10 |   |
|   | 2016-08-25 09:12:46.2300000 | Chrome 52,0 | Windows 7 | Birleşik Krallık |
|   | 2016-08-25 09:12:46.5100000 | Chrome 52,0 | Windows 10 | Birleşik Krallık |
|   | 2016-08-25 09:12:46.5570000 | Chrome 52,0 | Windows 10 | Litvanya Cumhuriyeti |
|   | 2016-08-25 09:12:47.0470000 | Chrome 52,0 | Windows 8.1 | Hindistan |
|   | 2016-08-25 09:12:51.3600000 | Chrome 52,0 | Windows 10 | Birleşik Krallık |
|   | 2016-08-25 09:12:51.6930000 | Chrome 52,0 | Windows 7 | Hollanda |
|   | 2016-08-25 09:12:56.4240000 | Chrome 52,0 | Windows 10 | Birleşik Krallık |
|   | 2016-08-25 09:13:08.7230000 | Chrome 52,0 | Windows 10 | Hindistan |

Ölçüm olmadığından yalnızca trafik sayısını temsil eden bir zaman serisi kümesi oluşturabilir ve aşağıdaki sorguyu kullanarak işletim sistemi tarafından bölümlenebilir:

[**sorguyu çalıştırmak için\[tıklayın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAAA5XPwQrCMBAE0Hu/Yo4NVLBn6Td4ULyWtV1tMJtIsoEq/XhbC4J48jgw+5h1rBDrW0UDDakjR7HsWUIrdOM2cbScakxIWYSiffJSL49W+KAkd2N2hVsMGv8yaPw2furFhCVu1gifpelC9loa9Hyh7LTZInh8FFiPSP7K5fufap1UoR4Mzg/s04njjEb2PUfofNYNFPUFtJiguAEBAAA=) **\]**

```kusto
let min_t = toscalar(demo_make_series1 | summarize min(TimeStamp));
let max_t = toscalar(demo_make_series1 | summarize max(TimeStamp));
demo_make_series1
| make-series num=count() default=0 on TimeStamp in range(min_t, max_t, 1h) by OsVer
| render timechart 
```

- Üç zaman serisi kümesi oluşturmak için [`make-series`](/azure/kusto/query/make-seriesoperator) işlecini kullanın; burada:
    - `num=count()`: trafik dizisi
    - `range(min_t, max_t, 1h)`: zaman serisi zaman aralığında (en eski ve en yeni tarih damgalarının tablo kayıtlarının) 1 saat içinde oluşturulur.
    - `default=0`: normal zaman serisi oluşturmak için eksik depo gözleri için Fill metodunu belirtin. Alternatif olarak, değişiklikler için [`series_fill_const()`](/azure/kusto/query/series-fill-constfunction), [`series_fill_forward()`](/azure/kusto/query/series-fill-forwardfunction), [`series_fill_backward()`](/azure/kusto/query/series-fill-backwardfunction) ve [`series_fill_linear()`](/azure/kusto/query/series-fill-linearfunction) kullanın
    - `byOsVer`: işletim sistemine göre Bölüm
- Gerçek zamanlı seri veri yapısı, her zaman bin başına toplam değerin sayısal dizisidir. Görselleştirme için `render timechart` kullanırız.

Yukarıdaki tabloda üç bölüm vardır. Grafikte görülen her bir işletim sistemi sürümü için ayrı bir zaman serisi: Windows 10 (kırmızı), 7 (mavi) ve 8,1 (yeşil) oluşturabilirsiniz:

![Zaman serisi bölümü](media/time-series-analysis/time-series-partition.png)

## <a name="time-series-analysis-functions"></a>Zaman serisi analiz işlevleri

Bu bölümde, tipik seri işleme işlevleri uygulayacağız.
Bir zaman serisi kümesi oluşturulduktan sonra, ADX, [zaman serisi belgelerinde](/azure/kusto/query/machine-learning-and-tsa)bulabileceğiniz, bunları işlemek ve analiz etmek için büyüyen bir işlev listesini destekler. Zaman serisini işlemek ve çözümlemek için bazı temsili işlevleri anlayacağız.

### <a name="filtering"></a>Filtreleme

Filtreleme, sinyal işleme ve zaman serisi işleme görevleri için (örneğin, gürültülü bir sinyal Düzgünleştir, değişiklik algılama) faydalı bir uygulamadır.
- İki genel filtreleme işlevi vardır:
    - [`series_fir()`](/azure/kusto/query/series-firfunction): köknar filtresi uygulanıyor. Değişiklik algılama için zaman serisinin hareketli ortalama ve farklılaşmasıyla ilgili basit hesaplama için kullanılır.
    - [`series_iir()`](/azure/kusto/query/series-iirfunction): IIR filtresi uygulanıyor. Üstel yumuşatma ve birikmeli toplam için kullanılır.
- sorguya yeni bir hareketli ortalama serisi Boyut 5 bölme ( *ma_num*) ekleyerek zaman serisi kümesi `Extend`:

[**sorguyu çalıştırmak için\[tıklayın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAAA5WPQavCMBCE7/6KOSYQ4fXgSfobPDx517C2q4bXpLLZQBV/vKkFQTx5WRh25tvZgRUxJK9ooWPuaCAxPcfRR/pnn1kC5wZ35BIjSbjxbDf7EPlXKV6s3a6GmUHTVwya3hkf9tUds1wvEqnEthtLUmPR85HKoO0PxoQXBSFBKJ3YPP9xSyWH5mxxuGKX/1gqlCfl1Neln5EL3R+DmCodhC9MahqHjXVQKbxMW5NScyzQerA7k+gDa1tswzsBAAA=) **\]**

```kusto
let min_t = toscalar(demo_make_series1 | summarize min(TimeStamp));
let max_t = toscalar(demo_make_series1 | summarize max(TimeStamp));
demo_make_series1
| make-series num=count() default=0 on TimeStamp in range(min_t, max_t, 1h) by OsVer
| extend ma_num=series_fir(num, repeat(1, 5), true, true)
| render timechart
```

![Zaman serisi filtreleme](media/time-series-analysis/time-series-filtering.png)

### <a name="regression-analysis"></a>Regresyon analizi

ADX, zaman serisinin eğilimini tahmin etmek için kesimli doğrusal regresyon analizini destekler.
- Genel eğilim algılaması için en iyi satırı bir zaman serisine uydurmak için [series_fit_line ()](/azure/kusto/query/series-fit-linefunction) kullanın.
- İzleme senaryolarında yararlı olan, taban çizgisine göre eğilim değişikliklerini algılamak için [series_fit_2lines ()](/azure/kusto/query/series-fit-2linesfunction) kullanın.

Bir zaman serisi sorgusunda `series_fit_line()` ve `series_fit_2lines()` işlevleri örneği:

[**sorguyu çalıştırmak için\[tıklayın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAAA0tJzc2PL04tykwtNuKqUUitKEnNS1GACMSnZZbEG+Vk5qUWa1Rq6iCLggSBYkAdRUD1qUUKIIHkjMSiEoXyzJIMjYrk/JzS3DzbCk0AUIIJ02EAAAA=) **\]**

```kusto
demo_series2
| extend series_fit_2lines(y), series_fit_line(y)
| render linechart with(xcolumn=x)
```

![Zaman serisi gerileme](media/time-series-analysis/time-series-regression.png)

- Mavi: özgün zaman serisi
- Yeşil: monte çizgili
- Kırmızı: iki sığdırılmış çizgi

> [!NOTE]
> İşlev, sıçrama (düzey değişiklik) noktasını doğru bir şekilde algıladı.

### <a name="seasonality-detection"></a>Mevsimsellik algılama

Birçok ölçüm dönemsel (düzenli) desenleri izler. Bulut hizmetlerinin Kullanıcı trafiği genellikle, iş günü ve gece yarısı ve hafta sonu boyunca en yüksek olan günlük ve haftalık desenler içerir. Düzenli aralıklarla IoT algılayıcı ölçer. Sıcaklık, basınç veya nem gibi fiziksel ölçümler de mevsimsel davranışları gösterebilir.

Aşağıdaki örnek, bir Web hizmetinin (2 saatlik sepetler) bir aylık trafiği için mevsimsellik algılamayı uygular:

[**sorguyu çalıştırmak için\[tıklayın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAAA0tJzc2PL04tykwtNuaqUShKzUtJLVIoycxNTc5ILCoBAHrjE80fAAAA) **\]**

```kusto
demo_series3
| render timechart 
```

![Zaman serisi mevsimsellik](media/time-series-analysis/time-series-seasonality.png)

- Zaman serisindeki dönemleri otomatik olarak algılamak için [series_periods_detect ()](/azure/kusto/query/series-periods-detectfunction) kullanın. 
- Bir metriğin belirli ayrı süreleri olması gerektiğini ve bunların mevcut olduğunu doğrulamak istiyoruz [series_periods_validate ()](/azure/kusto/query/series-periods-validatefunction) kullanın.

> [!NOTE]
> Belirli ayrı dönemler yoksa bu bir anomali olur

[**sorguyu çalıştırmak için\[tıklayın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAAA12OwQ6CMBBE737FHKmpVtAr39IguwkYyzZ0IZj48TZSLx533szOEAfxieeR0/XwRpzlwb2iilkSShapl5mTQYvd5QvxxJqd1bQEi8vZor6RawaLxsA5FewcOjBKBOP0PXUMXL7lyrCeeIvdRPjrzIw35Qyoe6W2GY4qJMv9yb91xtX0AS7N323BAAAA) **\]**

```kusto
demo_series3
| project (periods, scores) = series_periods_detect(num, 0., 14d/2h, 2) //to detect the periods in the time series
| mv-expand periods, scores
| extend days=2h*todouble(periods)/1d
```

|   |   |   |   |
| --- | --- | --- | --- |
|   | dönemin | puanları | miş |
|   | 84 | 0.820622786055595 | 7 |
|   | 12 | 0.764601405803502 | 1 |

İşlev günlük ve haftalık mevsimsellik algılar. Hafta sonu günleri, haftanın haftalarından farklı olduğundan günlük puanlar haftalık olarak küçüktür.

### <a name="element-wise-functions"></a>Öğe temelinde işlevler

Aritmetik ve mantıksal işlemler, bir zaman serisinde yapılabilir. [Series_subtract ()](/azure/kusto/query/series-subtractfunction) kullanarak, bir fazlalık zaman serisini hesaplayabilir, diğer bir deyişle, özgün ham ölçüm ve düzleştirilmiş bir değer arasındaki fark ve fazlalık sinyalinde aykırımları arayabilirsiniz:

[**sorguyu çalıştırmak için\[tıklayın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAAA5WQQU/DMAyF7/sVT5waqWjrgRPqb+AAgmPltR6LSNLJcdhA+/G4izRAnLhEerbfl2cHVkSfBkUPnfNIgaSZOM5DpDceMovn3OGMXGIk8Z+8jDdPPvKjUjw4d78KC4NO/2LQ6Tfjz/jqjEXeVolUYj/OJWnjMPGOStB+gznhSoFPEEqv3Fz2aWukFt3eYfuBh/zMYlA+KafJmsOCrPRh56Ux2UL4wKRN1+LOtVApXF/37RTOfioUfvpz2arQqBVS2Q7rtc6wa4wlkPLVCLXIqE7DHvcsXOOh73Hz4tM0HzO6zQ1gDOx8UOvZrtayst0Y7z4babkkYQxMyQbGPYnCiGIxTS/fXGpfwk+n7uQBAAA=) **\]**

```kusto
let min_t = toscalar(demo_make_series1 | summarize min(TimeStamp));
let max_t = toscalar(demo_make_series1 | summarize max(TimeStamp));
demo_make_series1
| make-series num=count() default=0 on TimeStamp in range(min_t, max_t, 1h) by OsVer
| extend ma_num=series_fir(num, repeat(1, 5), true, true)
| extend residual_num=series_subtract(num, ma_num) //to calculate residual time series
| where OsVer == "Windows 10"   // filter on Win 10 to visualize a cleaner chart 
| render timechart
```

![Zaman serisi işlemleri](media/time-series-analysis/time-series-operations.png)

- Mavi: özgün zaman serisi
- Kırmızı: düzleştirilmiş zaman serisi
- Yeşil: kalan zaman serisi

## <a name="time-series-workflow-at-scale"></a>Ölçekte zaman serisi iş akışı

Aşağıdaki örnekte, bu işlevlerin, anomali algılama için saniye cinsinden binlerce zaman serisinde ölçeklendirerek nasıl çalıştırılabilmesini gösterilmektedir. Bir DB hizmetinin okuma sayısı ölçüsünün dört gün içinde birkaç örnek telemetri kaydını görmek için aşağıdaki sorguyu çalıştırın:

[**sorguyu çalıştırmak için\[tıklayın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAAA0tJzc2Pz03Mq4wvTi3KTC025KpRKEnMTlUwAQArfAiiGgAAAA==) **\]**

```kusto
demo_many_series1
| take 4 
```

|   |   |   |   |   |   |
| --- | --- | --- | --- | --- | --- |
|   | ILIŞKIN | Çerçeve | anonOp | VERITABANı | DataRead |
|   | 2016-09-11 21:00:00.0000000 | Loc 9 | 5117853934049630089 | 262 | 0 |
|   | 2016-09-11 21:00:00.0000000 | Loc 9 | 5117853934049630089 | 241 | 0 |
|   | 2016-09-11 21:00:00.0000000 | Loc 9 | -865998331941149874 | 262 | 279862 |
|   | 2016-09-11 21:00:00.0000000 | Loc 9 | 371921734563783410 | 255 | 0 |

Ve basit İstatistikler:

[**sorguyu çalıştırmak için\[tıklayın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAAA0tJzc2Pz03Mq4wvTi3KTC025KpRKC7NzU0syqxKVcgrzbVNzi/NK9HQ1FHIzcyLL7EFkhohnr6uwSGOvgEg0cQKkGhiBZIoAEq2dK9VAAAA) **\]**

```kusto
demo_many_series1
| summarize num=count(), min_t=min(TIMESTAMP), max_t=max(TIMESTAMP) 
```

|   |   |   |   |
| --- | --- | --- | --- |
|   | numaraları | Min\_t | en fazla\_t |
|   | 2177472 | 2016-09-08 00:00:00.0000000 | 2016-09-11 23:00:00.0000000 |

Okuma ölçüsünün 1 saatlik depo gözlerinde zaman serisi oluşturma (Toplam dört gün * 24 saat = 96 punto), normal kalıp dalgalanmasına neden olur:

[**sorguyu çalıştırmak için\[tıklayın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAAA5WPMQvCMBSE9/6KGxOoYGfpIOjgUBDtXh7twwabFF6ittIfb2rBQSfHg+8+7joOsMZVATlC72vqSFTDtq8subHyLIZ9hgn+Zi2JefKMq/JQ7M/ltjhqvQGSbrbQ8JeFhm/LTyGZInbl1RIhTI3P6X5ROwp0ikmjd/hYYByE3IXV+1G6TEqRtTqahF3DgmAs1y1JwMOEVo0Rzdf6BbBH5FAHAQAA) **\]**

```kusto
let min_t = toscalar(demo_many_series1 | summarize min(TIMESTAMP));  
let max_t = toscalar(demo_many_series1 | summarize max(TIMESTAMP));  
demo_many_series1
| make-series reads=avg(DataRead) on TIMESTAMP in range(min_t, max_t, 1h)
| render timechart with(ymin=0) 
```

![Ölçekte zaman serisi](media/time-series-analysis/time-series-at-scale.png)

Yukarıdaki davranış yanıltıcıdır, çünkü tek normal zaman serisi anormal desenleri olabilecek binlerce farklı örnek tarafından toplanır. Bu nedenle, örnek başına bir zaman serisi oluşturacağız. Örnek, Loc (konum), anonOp (işlem) ve DB (belirli makine) tarafından tanımlanır.

Kaç zaman serisi oluşturuyoruz?

[**sorguyu çalıştırmak için\[tıklayın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAAA0tJzc2Pz03Mq4wvTi3KTC025KpRKC7NzU0syqxKVUiqVPDJT9ZR8C/QUXBxAkol55fmlQAAWEsFxjQAAAA=) **\]**

```kusto
demo_many_series1
| summarize by Loc, Op, DB
| count
```

|   |   |
| --- | --- |
|   | Sayı |
|   | 18339 |

Şimdi, okuma sayısı ölçümünün 18339 zaman serisi bir kümesini oluşturacağız. `by` yan tümcesini Make-Series ifadesine ekler, doğrusal regresyon uygular ve en önemli azalan eğilimi olan en yüksek iki zaman serisini seçer:

[**sorguyu çalıştırmak için\[tıklayın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAAA5WPsU7DQBBE+3zFdLmTTGHSgFAKUCiQiIKIe2u5rJ0T9l3YWwcH5eO5JBIFVJSzmnmz07Gi96FWzKExOepIzIb7WPcUDnVi8ZxKHJGGvifxX3yym+pp+biu7pcv1t4Bk+5EofFfFBp/U/4EJsdse+eri4QwbdKc9q1ZkNJrVhYx4IcCHyAUWjbnRcXlpQLl1uLtgOfoCqx2BRYPGcyjctjASPoYSLhA6uKObR5waasbr3XnA5tzrc0RjTtcn0hnKyg55KtkDAvU9+y2JIpPr1ujXjueT9cse+8YlVDTeIfVoNQymiiZ5ENSCi4vM3FQxAblzWx2a6f2G2UcBRyWAQAA) **\]**

```kusto
let min_t = toscalar(demo_many_series1 | summarize min(TIMESTAMP));  
let max_t = toscalar(demo_many_series1 | summarize max(TIMESTAMP));  
demo_many_series1
| make-series reads=avg(DataRead) on TIMESTAMP in range(min_t, max_t, 1h) by Loc, Op, DB
| extend (rsquare, slope) = series_fit_line(reads)
| top 2 by slope asc 
| render timechart with(title='Service Traffic Outage for 2 instances (out of 18339)')
```

![Zaman serisi üst iki](media/time-series-analysis/time-series-top-2.png)

Örnekleri görüntüle:

[**sorguyu çalıştırmak için\[tıklayın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAAA5WPvW4CMRCEe55iSlsyBWkjChApIoESAb21udsQg38O26AD8fDx3SEUJVXKWc18s2M5wxmvM6bIIVVkKYqaXdCO/EUnjobTBDekk3MUzZU7u9i+rl4229nqXcpnYGQ7CrX/olD7m/InMLoV24HHg0RkqtOUzjuxoEzroiSCx4MC4xHJ71j0i9TwksLkS+LjgmWoFN4ahcW8gLnN7GuImI4niqyQbGhYlgFDm/40WVvjWfS1skRyaPDUkXorKFXl2MSw5yr/pN9Z31SyxuhbAQAA) **\]**

```kusto
let min_t = toscalar(demo_many_series1 | summarize min(TIMESTAMP));  
let max_t = toscalar(demo_many_series1 | summarize max(TIMESTAMP));  
demo_many_series1
| make-series reads=avg(DataRead) on TIMESTAMP in range(min_t, max_t, 1h) by Loc, Op, DB
| extend (rsquare, slope) = series_fit_line(reads)
| top 2 by slope asc
| project Loc, Op, DB, slope 
```

|   |   |   |   |   |
| --- | --- | --- | --- | --- |
|   | Çerçeve | Üs | VERITABANı | eğilmiş |
|   | Loc 15 | 37 | 1151 | -102743,910227889 |
|   | Loc 13 | 37 | 1249 | -86303.2334644601 |

İki dakikadan kısa bir süre içinde, ADX 20.000 zaman serisine yakın şekilde çözümlendiğinden, okuma sayısının aniden bırakılmakta olan iki olağandışı zaman serisi tespit edildi.

ADX hızlı performans ile birlikte sunulan bu gelişmiş yetenekler, zaman serisi analizine yönelik benzersiz ve güçlü bir çözüm sağlar.

## <a name="next-steps"></a>Sonraki adımlar

* Azure Veri Gezgini 'da [zaman serisi anomali algılama ve tahmin](/azure/data-explorer/anomaly-detection) hakkında bilgi edinin.
* Azure Veri Gezgini 'de [makine öğrenimi özellikleri](/azure/data-explorer/machine-learning-clustering) hakkında bilgi edinin.