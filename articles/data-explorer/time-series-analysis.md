---
title: Azure Veri Gezgini'ni kullanarak zaman serisi verilerini analiz edin
description: Azure Veri Gezgini'ni kullanarak buluttaki zaman serisi verilerini nasıl analiz edeceğiz öğrenin.
author: orspod
ms.author: orspodek
ms.reviewer: adieldar
ms.service: data-explorer
ms.topic: conceptual
ms.date: 04/07/2019
ms.openlocfilehash: 3873b25394f91ce1c1601c348de2098198ba7fdd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74765492"
---
# <a name="time-series-analysis-in-azure-data-explorer"></a>Azure Veri Gezgini'nde zaman serisi analizi

Azure Veri Gezgini (ADX), bulut hizmetlerinden veya IoT aygıtlarından telemetri verilerinin toplanmasını devam eder. Bu veriler, hizmet durumunun izlenmesi, fiziksel üretim süreçleri ve kullanım eğilimleri gibi çeşitli öngörüler için analiz edilebilir. Analiz, tipik taban çizgisi desenine göre desende bir sapma bulmak için seçilen ölçümlerin zaman serilerinde yapılır.
ADX, birden çok zaman serisinin oluşturulması, manipülasyonu ve analizi için yerel destek içerir. Bu konuda, ADX'in binlerce zaman **serisini saniyeler içinde**oluşturmak ve analiz etmek için nasıl kullanıldığını ve neredeyse gerçek zamanlı izleme çözümleri ve iş akışları sağladığını öğrenin.

## <a name="time-series-creation"></a>Zaman serisi oluşturma

Bu bölümde, `make-series` operatörü kullanarak basit ve sezgisel olarak büyük bir düzenli zaman serisi seti oluşturacağız ve gerektiğinde eksik değerleri dolduracağız.
Zaman serisi analizinde ilk adım, orijinal telemetri tablosunu bir dizi zaman serisine bölmek ve dönüştürmektir. Tablo genellikle bir zaman damgası sütunu, bağlamsal boyutlar ve isteğe bağlı ölçümler içerir. Boyutlar verileri bölmek için kullanılır. Amaç, düzenli zaman aralıklarında bölüm başına binlerce zaman serisi oluşturmaktır.

Giriş tablosu *demo_make_series1* rasgele web hizmeti trafiğinin 600K kayıtlarını içerir. Örnek 10 kayıt için aşağıdaki komutu kullanın:

**\[**[**Sorguyı çalıştırmak için tıklatın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAAA0tJzc2Pz03MTo0vTi3KTC02VKhRKAFyFQwNADOyzKUbAAAA)**\]**

```kusto
demo_make_series1 | take 10 
```

Elde edilen tablo bir zaman damgası sütunu, üç bağlamsal boyut sütunu ve hiçbir ölçüm içerir:

|   |   |   |   |   |
| --- | --- | --- | --- | --- |
|   | Zaman damgası | BrowserVer | Osver | Ülke/Bölge |
|   | 2016-08-25 09:12:35.4020000 | Krom 51.0 | Windows 7 | Birleşik Krallık |
|   | 2016-08-25 09:12:41.1120000 | Krom 52.0 | Windows 10 |   |
|   | 2016-08-25 09:12:46.2300000 | Krom 52.0 | Windows 7 | Birleşik Krallık |
|   | 2016-08-25 09:12:46.5100000 | Krom 52.0 | Windows 10 | Birleşik Krallık |
|   | 2016-08-25 09:12:46.5570000 | Krom 52.0 | Windows 10 | Litvanya Cumhuriyeti |
|   | 2016-08-25 09:12:47.0470000 | Krom 52.0 | Windows 8.1 | Hindistan |
|   | 2016-08-25 09:12:51.3600000 | Krom 52.0 | Windows 10 | Birleşik Krallık |
|   | 2016-08-25 09:12:51.6930000 | Krom 52.0 | Windows 7 | Hollanda |
|   | 2016-08-25 09:12:56.4240000 | Krom 52.0 | Windows 10 | Birleşik Krallık |
|   | 2016-08-25 09:13:08.7230000 | Krom 52.0 | Windows 10 | Hindistan |

Hiçbir ölçüm olmadığından, yalnızca aşağıdaki sorguyu kullanarak işletim sistemi tarafından bölümlenen trafik sayısının kendisini temsil eden bir dizi zaman serisi oluşturabiliriz:

**\[**[**Sorguyı çalıştırmak için tıklatın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAAA5XPwQrCMBAE0Hu/Yo4NVLBn6Td4ULyWtV1tMJtIsoEq/XhbC4J48jgw+5h1rBDrW0UDDakjR7HsWUIrdOM2cbScakxIWYSiffJSL49W+KAkd2N2hVsMGv8yaPw2furFhCVu1gifpelC9loa9Hyh7LTZInh8FFiPSP7K5fufap1UoR4Mzg/s04njjEb2PUfofNYNFPUFtJiguAEBAAA=)**\]**

```kusto
let min_t = toscalar(demo_make_series1 | summarize min(TimeStamp));
let max_t = toscalar(demo_make_series1 | summarize max(TimeStamp));
demo_make_series1
| make-series num=count() default=0 on TimeStamp in range(min_t, max_t, 1h) by OsVer
| render timechart 
```

- Üç [`make-series`](/azure/kusto/query/make-seriesoperator) zaman serisi kümesi oluşturmak için işleci kullanın:
    - `num=count()`: trafik zaman serisi
    - `range(min_t, max_t, 1h)`: zaman serisi zaman aralığında 1 saatlik kutular halinde oluşturulur (tablo kayıtlarının en eski ve en yeni zaman damgaları)
    - `default=0`: normal zaman serileri oluşturmak için eksik kutular için dolgu yöntemini belirtin. Alternatif olarak [`series_fill_const()`](/azure/kusto/query/series-fill-constfunction) [`series_fill_forward()`](/azure/kusto/query/series-fill-forwardfunction), [`series_fill_backward()`](/azure/kusto/query/series-fill-backwardfunction) [`series_fill_linear()`](/azure/kusto/query/series-fill-linearfunction) ve değişiklikler için
    - `byOsVer`: OS tarafından bölüm
- Gerçek zaman serisi veri yapısı, her zaman çöp kutusu başına toplanan değerin sayısal bir dizisidir. Görselleştirme `render timechart` için kullanırız.

Yukarıdaki tabloda, üç bölüm var. Ayrı bir zaman serisi oluşturabiliriz: Grafikte görüldüğü gibi her işletim sistemi sürümü için Windows 10 (kırmızı), 7 (mavi) ve 8,1 (yeşil):

![Zaman serisi bölüm](media/time-series-analysis/time-series-partition.png)

## <a name="time-series-analysis-functions"></a>Zaman serisi analiz fonksiyonları

Bu bölümde, tipik seri işleme işlevleri gerçekleştireceğiz.
Bir dizi zaman serisi oluşturulduktan sonra, ADX [zaman serisi belgelerinde](/azure/kusto/query/machine-learning-and-tsa)bulunabilen işlevleri işlemek ve analiz etmek için büyüyen bir işlevleri destekler. Zaman serilerini işlemek ve analiz etmek için birkaç temsili işlevi açıklayacağız.

### <a name="filtering"></a>Filtreleme

Filtreleme sinyal işleme de yaygın bir uygulamadır ve zaman serisi işleme görevleri için yararlıdır (örneğin, gürültülü bir sinyal, değişiklik algılama sı yetmez).
- İki genel filtreleme işlevi vardır:
    - [`series_fir()`](/azure/kusto/query/series-firfunction): FIR filtresi uygulama. Hareketli ortalamanın basit hesaplanmasında ve değişim algılaması için zaman serisinin farklılaşmasında kullanılır.
    - [`series_iir()`](/azure/kusto/query/series-iirfunction): IIR filtresi uygulanıyor. Üstel yumuşatma ve kümülatif toplam için kullanılır.
- `Extend`sorguya boyut 5 kutular *(ma_num*olarak adlandırılır) yeni bir hareketli ortalama serisi ekleyerek ayarlanan zaman serisi:

**\[**[**Sorguyı çalıştırmak için tıklatın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAAA5WPQavCMBCE7/6KOSYQ4fXgSfobPDx517C2q4bXpLLZQBV/vKkFQTx5WRh25tvZgRUxJK9ooWPuaCAxPcfRR/pnn1kC5wZ35BIjSbjxbDf7EPlXKV6s3a6GmUHTVwya3hkf9tUds1wvEqnEthtLUmPR85HKoO0PxoQXBSFBKJ3YPP9xSyWH5mxxuGKX/1gqlCfl1Neln5EL3R+DmCodhC9MahqHjXVQKbxMW5NScyzQerA7k+gDa1tswzsBAAA=)**\]**

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

ADX, zaman serisinin eğilimini tahmin etmek için parçalı doğrusal regresyon analizini destekler.
- Genel eğilim algılama için bir zaman serisine en iyi satırı sığdırmak için [series_fit_line()](/azure/kusto/query/series-fit-linefunction) kullanın.
- Senaryoları izlemede yararlı olan, taban çizgisine göre eğilim değişikliklerini algılamak için [series_fit_2lines()](/azure/kusto/query/series-fit-2linesfunction) kullanın.

Bir `series_fit_line()` zaman `series_fit_2lines()` serisi sorgusundaki örnek ve işlevler:

**\[**[**Sorguyı çalıştırmak için tıklatın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAAA0tJzc2PL04tykwtNuKqUUitKEnNS1GACMSnZZbEG+Vk5qUWa1Rq6iCLggSBYkAdRUD1qUUKIIHkjMSiEoXyzJIMjYrk/JzS3DzbCk0AUIIJ02EAAAA=)**\]**

```kusto
demo_series2
| extend series_fit_2lines(y), series_fit_line(y)
| render linechart with(xcolumn=x)
```

![Zaman serisi regresyon](media/time-series-analysis/time-series-regression.png)

- Mavi: orijinal zaman serisi
- Yeşil: donatılmış hat
- Kırmızı: iki monte hatları

> [!NOTE]
> İşlev atlama (düzey değişikliği) noktasını doğru bir şekilde algılatmıştır.

### <a name="seasonality-detection"></a>Mevsimsellik tespiti

Birçok ölçüm mevsimsel (periyodik) desenleri izler. Bulut hizmetlerinin kullanıcı trafiği genellikle iş gününün ortasında en yüksek ve gece ve hafta sonu boyunca en düşük günlük ve haftalık desenler içerir. IoT sensörleri periyodik aralıklarla ölçer. Sıcaklık, basınç veya nem gibi fiziksel ölçümler de mevsimsel davranışlar gösterebilir.

Aşağıdaki örnek, bir web hizmetinin bir aylık trafiğinde (2 saatlik kutular) mevsimsellik algılamasını uygular:

**\[**[**Sorguyı çalıştırmak için tıklatın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAAA0tJzc2PL04tykwtNuaqUShKzUtJLVIoycxNTc5ILCoBAHrjE80fAAAA)**\]**

```kusto
demo_series3
| render timechart 
```

![Zaman serisi mevsimsellik](media/time-series-analysis/time-series-seasonality.png)

- Zaman serisindeki dönemleri otomatik olarak algılamak için [series_periods_detect()](/azure/kusto/query/series-periods-detectfunction) kullanın. 
- Bir metrin belirli bir döneme(ler) sahip olması gerektiğini biliyorsak ve bunların var olduğunu doğrulamak istiyorsak [series_periods_validate()](/azure/kusto/query/series-periods-validatefunction) kullanın.

> [!NOTE]
> Belirli belirli dönemler yoksa bu bir anormalliktir.

**\[**[**Sorguyı çalıştırmak için tıklatın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAAA12OwQ6CMBBE737FHKmpVtAr39IguwkYyzZ0IZj48TZSLx533szOEAfxieeR0/XwRpzlwb2iilkSShapl5mTQYvd5QvxxJqd1bQEi8vZor6RawaLxsA5FewcOjBKBOP0PXUMXL7lyrCeeIvdRPjrzIw35Qyoe6W2GY4qJMv9yb91xtX0AS7N323BAAAA)**\]**

```kusto
demo_series3
| project (periods, scores) = series_periods_detect(num, 0., 14d/2h, 2) //to detect the periods in the time series
| mv-expand periods, scores
| extend days=2h*todouble(periods)/1d
```

|   |   |   |   |
| --- | --- | --- | --- |
|   | Dönem | Notalar | gün |
|   | 84 | 0.820622786055595 | 7 |
|   | 12 | 0.764601405803502 | 1 |

İşlev günlük ve haftalık mevsimselliği algılar. Hafta sonu günleri hafta içi farklı olduğundan günlük puanlar haftalık puandan daha azdır.

### <a name="element-wise-functions"></a>Öğe açısından işlevler

Aritmetik ve mantıksal işlemler bir zaman serisi üzerinde yapılabilir. [series_subtract()](/azure/kusto/query/series-subtractfunction) kullanarak, orijinal ham metrik ile düzeltilmiş olan arasındaki farkı, bir artık zaman serisini hesaplayabilir ve artık sinyaldeki anormallikleri bulabiliriz:

**\[**[**Sorguyı çalıştırmak için tıklatın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAAA5WQQU/DMAyF7/sVT5waqWjrgRPqb+AAgmPltR6LSNLJcdhA+/G4izRAnLhEerbfl2cHVkSfBkUPnfNIgaSZOM5DpDceMovn3OGMXGIk8Z+8jDdPPvKjUjw4d78KC4NO/2LQ6Tfjz/jqjEXeVolUYj/OJWnjMPGOStB+gznhSoFPEEqv3Fz2aWukFt3eYfuBh/zMYlA+KafJmsOCrPRh56Ux2UL4wKRN1+LOtVApXF/37RTOfioUfvpz2arQqBVS2Q7rtc6wa4wlkPLVCLXIqE7DHvcsXOOh73Hz4tM0HzO6zQ1gDOx8UOvZrtayst0Y7z4babkkYQxMyQbGPYnCiGIxTS/fXGpfwk+n7uQBAAA=)**\]**

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

- Mavi: orijinal zaman serisi
- Kırmızı: düzeltilmiş zaman serisi
- Yeşil: artık zaman serisi

## <a name="time-series-workflow-at-scale"></a>Ölçekte zaman serisi iş akışı

Aşağıdaki örnek, anomali tespiti için bu işlevlerin binlerce zaman serisinde saniyeler içinde nasıl ölçekte çalıştırılabildiğini göstermektedir. Dört gün içinde bir DB hizmetinin okuma sayısı ölçümünün birkaç örnek telemetri kaydını görmek için aşağıdaki sorguyu çalıştırın:

**\[**[**Sorguyı çalıştırmak için tıklatın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAAA0tJzc2Pz03Mq4wvTi3KTC025KpRKEnMTlUwAQArfAiiGgAAAA==)**\]**

```kusto
demo_many_series1
| take 4 
```

|   |   |   |   |   |   |
| --- | --- | --- | --- | --- | --- |
|   | Zaman damgası | Loc | anonOp | DB | Veri Okuma |
|   | 2016-09-11 21:00:00.0000000 | Loc 9 | 5117853934049630089 | 262 | 0 |
|   | 2016-09-11 21:00:00.0000000 | Loc 9 | 5117853934049630089 | 241 | 0 |
|   | 2016-09-11 21:00:00.0000000 | Loc 9 | -865998331941149874 | 262 | 279862 |
|   | 2016-09-11 21:00:00.0000000 | Loc 9 | 371921734563783410 | 255 | 0 |

Ve basit istatistikler:

**\[**[**Sorguyı çalıştırmak için tıklatın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAAA0tJzc2Pz03Mq4wvTi3KTC025KpRKC7NzU0syqxKVcgrzbVNzi/NK9HQ1FHIzcyLL7EFkhohnr6uwSGOvgEg0cQKkGhiBZIoAEq2dK9VAAAA)**\]**

```kusto
demo_many_series1
| summarize num=count(), min_t=min(TIMESTAMP), max_t=max(TIMESTAMP) 
```

|   |   |   |   |
| --- | --- | --- | --- |
|   | sayı | min\_t | maksimum\_t |
|   | 2177472 | 2016-09-08 00:00:00.0000000 | 2016-09-11 23:00:00.0000000 |

Okuma ölçüsünün 1 saatlik kutu kutularında bir zaman serisi oluşturma (toplam dört gün * 24 saat = 96 puan), normal desen dalgalanması ile sonuçlanır:

**\[**[**Sorguyı çalıştırmak için tıklatın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAAA5WPMQvCMBSE9/6KGxOoYGfpIOjgUBDtXh7twwabFF6ittIfb2rBQSfHg+8+7joOsMZVATlC72vqSFTDtq8subHyLIZ9hgn+Zi2JefKMq/JQ7M/ltjhqvQGSbrbQ8JeFhm/LTyGZInbl1RIhTI3P6X5ROwp0ikmjd/hYYByE3IXV+1G6TEqRtTqahF3DgmAs1y1JwMOEVo0Rzdf6BbBH5FAHAQAA)**\]**

```kusto
let min_t = toscalar(demo_many_series1 | summarize min(TIMESTAMP));  
let max_t = toscalar(demo_many_series1 | summarize max(TIMESTAMP));  
demo_many_series1
| make-series reads=avg(DataRead) on TIMESTAMP in range(min_t, max_t, 1h)
| render timechart with(ymin=0) 
```

![Ölçekte zaman serisi](media/time-series-analysis/time-series-at-scale.png)

Yukarıdaki davranış yanıltıcıdır, çünkü tek normal zaman serisi anormal desenleri olabilecek binlerce farklı örnekten toplanır. Bu nedenle, örnek başına bir zaman serisi oluştururuz. Bir örnek Loc (konum), anonOp (çalışma) ve DB (belirli makine) tarafından tanımlanır.

Kaç tane zaman serisi oluşturabiliriz?

**\[**[**Sorguyı çalıştırmak için tıklatın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAAA0tJzc2Pz03Mq4wvTi3KTC025KpRKC7NzU0syqxKVUiqVPDJT9ZR8C/QUXBxAkol55fmlQAAWEsFxjQAAAA=)**\]**

```kusto
demo_many_series1
| summarize by Loc, Op, DB
| count
```

|   |   |
| --- | --- |
|   | Sayı |
|   | 18339 |

Şimdi, okuma sayısı ölçümünün 18339 zaman serilerini oluşturacağız. Maddeyi `by` make-series deyimine ekliyoruz, doğrusal regresyon uyguluyoruz ve en önemli azalan eğilime sahip en iyi iki zaman serisini seçiyoruz:

**\[**[**Sorguyı çalıştırmak için tıklatın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAAA5WPsU7DQBBE+3zFdLmTTGHSgFAKUCiQiIKIe2u5rJ0T9l3YWwcH5eO5JBIFVJSzmnmz07Gi96FWzKExOepIzIb7WPcUDnVi8ZxKHJGGvifxX3yym+pp+biu7pcv1t4Bk+5EofFfFBp/U/4EJsdse+eri4QwbdKc9q1ZkNJrVhYx4IcCHyAUWjbnRcXlpQLl1uLtgOfoCqx2BRYPGcyjctjASPoYSLhA6uKObR5waasbr3XnA5tzrc0RjTtcn0hnKyg55KtkDAvU9+y2JIpPr1ujXjueT9cse+8YlVDTeIfVoNQymiiZ5ENSCi4vM3FQxAblzWx2a6f2G2UcBRyWAQAA)**\]**

```kusto
let min_t = toscalar(demo_many_series1 | summarize min(TIMESTAMP));  
let max_t = toscalar(demo_many_series1 | summarize max(TIMESTAMP));  
demo_many_series1
| make-series reads=avg(DataRead) on TIMESTAMP in range(min_t, max_t, 1h) by Loc, Op, DB
| extend (rsquare, slope) = series_fit_line(reads)
| top 2 by slope asc 
| render timechart with(title='Service Traffic Outage for 2 instances (out of 18339)')
```

![Zaman serisi ilk iki](media/time-series-analysis/time-series-top-2.png)

Örnekleri görüntüleyin:

**\[**[**Sorguyı çalıştırmak için tıklatın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAAA5WPvW4CMRCEe55iSlsyBWkjChApIoESAb21udsQg38O26AD8fDx3SEUJVXKWc18s2M5wxmvM6bIIVVkKYqaXdCO/EUnjobTBDekk3MUzZU7u9i+rl4229nqXcpnYGQ7CrX/olD7m/InMLoV24HHg0RkqtOUzjuxoEzroiSCx4MC4xHJ71j0i9TwksLkS+LjgmWoFN4ahcW8gLnN7GuImI4niqyQbGhYlgFDm/40WVvjWfS1skRyaPDUkXorKFXl2MSw5yr/pN9Z31SyxuhbAQAA)**\]**

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
|   | Loc | Op | DB | Eğim |
|   | Loc 15 | 37 | 1151 | -102743.910227889 |
|   | Loc 13 | 37 | 1249 | -86303.2334644601 |

İki dakikadan kısa bir sürede, ADX 20.000'e yakın zaman serisini analiz etti ve okuma sayısının aniden düştüğü iki anormal zaman serisi tespit etti.

ADX hızlı performans ile birlikte bu gelişmiş yetenekleri zaman serisi analizi için benzersiz ve güçlü bir çözüm sağlar.

## <a name="next-steps"></a>Sonraki adımlar

* Azure Veri Gezgini'nde [Time serisi anormallik algılama ve tahmini](/azure/data-explorer/anomaly-detection) hakkında bilgi edinin.
* Azure Veri Gezgini'nde [Makine öğrenimi özellikleri](/azure/data-explorer/machine-learning-clustering) hakkında bilgi edinin.