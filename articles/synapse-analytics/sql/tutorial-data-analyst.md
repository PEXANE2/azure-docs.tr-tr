---
title: "Veri analist öğreticisi: Azure SYNAPSE Studio 'da Azure açık veri kümelerini analiz etmek için SQL isteğe bağlı (Önizleme) kullanma (Önizleme)"
description: Bu öğreticide, SQL isteğe bağlı (Önizleme) kullanarak farklı Azure açık veri kümeleri birleştiren araştırmacı veri analizini kolayca gerçekleştirmeyi ve Azure SYNAPSE Studio 'da sonuçları görselleştirmeyi öğreneceksiniz.
services: synapse-analytics
author: azaricstefan
ms.service: synapse-analytics
ms.topic: tutorial
ms.subservice: sql
ms.date: 04/15/2020
ms.author: v-stazar
ms.reviewer: jrasnick, carlrab
ms.openlocfilehash: afe231186adec95b09a75d7fe59552ca8a07049b
ms.sourcegitcommit: 6fd28c1e5cf6872fb28691c7dd307a5e4bc71228
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85213151"
---
# <a name="use-sql-on-demand-preview-to-analyze-azure-open-datasets-and-visualize-the-results-in-azure-synapse-studio-preview"></a>Azure açık veri kümelerini analiz etmek ve sonuçları Azure SYNAPSE Studio 'da (Önizleme) görselleştirmek için SQL isteğe bağlı (Önizleme) kullanın

Bu öğreticide, Azure SYNAPSE Studio 'da SQL 'i isteğe bağlı olarak kullanarak farklı Azure açık veri kümelerini birleştirerek araştırmacı veri analizini gerçekleştirmeyi öğrenirsiniz.

Özellikle, şunu içeren [New York City (NYC) TAXI veri kümesini](https://azure.microsoft.com/services/open-datasets/catalog/nyc-taxi-limousine-commission-yellow-taxi-trip-records/) analiz edersiniz:

- Toplama ve bırakma tarihleri ve saatleri.
- Seçme ve bırakma konumları. 
- Seyahat mesafeleri.
- Fares dökümü.
- Oran türleri.
- Ödeme türleri. 
- Sürücü tarafından bildirilen yolcular sayısı.

## <a name="automatic-schema-inference"></a>Otomatik Şema çıkarımı

Veriler Parquet dosya biçiminde depolandığından, otomatik Şema çıkarımı kullanılabilir. Dosyalardaki tüm sütunların veri türlerini listelemeden verileri kolayca sorgulayabilirsiniz. Ayrıca, belirli bir dosya alt kümesini filtrelemek için sanal sütun mekanizmasını ve FilePath işlevini de kullanabilirsiniz.

İlk olarak aşağıdaki sorguyu çalıştırarak NYC TAXI verilerini öğrenelim:

```sql
SELECT TOP 100 * FROM
    OPENROWSET(
        BULK 'https://azureopendatastorage.blob.core.windows.net/nyctlc/yellow/puYear=*/puMonth=*/*.parquet',
        FORMAT='PARQUET'
    ) AS [nyc]
```

Aşağıdaki kod parçacığında, NYC TAXI verileri için sonuç gösterilmektedir:

![NYC TAXI veri sonuç parçacığı](./media/tutorial-data-analyst/1.png)

Benzer şekilde, aşağıdaki sorguyu kullanarak genel tatiller veri kümesini sorgulayabilirsiniz:

```sql
SELECT TOP 100 * FROM
    OPENROWSET(
        BULK 'https://azureopendatastorage.blob.core.windows.net/holidaydatacontainer/Processed/*.parquet',
        FORMAT='PARQUET'
    ) AS [holidays]
```

Aşağıdaki kod parçacığında, genel tatiller veri kümesinin sonucu gösterilmektedir:

![Genel tatiller veri kümesi sonuç parçacığı](./media/tutorial-data-analyst/2.png)

Son olarak, aşağıdaki sorguyu kullanarak hava durumu verileri veri kümesini de sorgulayabilirsiniz:

```sql
SELECT
    TOP 100 *
FROM  
    OPENROWSET(
        BULK 'https://azureopendatastorage.blob.core.windows.net/isdweatherdatacontainer/ISDWeather/year=*/month=*/*.parquet',
        FORMAT='PARQUET'
    ) AS [weather]
```

Aşağıdaki kod parçacığı, hava durumu veri kümesinin sonucunu gösterir:

![Hava durumu veri kümesi sonuç parçacığı](./media/tutorial-data-analyst/3.png)

[NYC TAXI](https://azure.microsoft.com/services/open-datasets/catalog/nyc-taxi-limousine-commission-yellow-taxi-trip-records/), [genel tatiller](https://azure.microsoft.com/services/open-datasets/catalog/public-holidays/)ve [Hava durumu veri](https://azure.microsoft.com/services/open-datasets/catalog/noaa-integrated-surface-data/) veri kümelerinin açıklamalarındaki tek tek sütunların anlamı hakkında daha fazla bilgi edinebilirsiniz.

## <a name="time-series-seasonality-and-outlier-analysis"></a>Zaman serisi, mevsimsellik ve aykırı değer analizi

Aşağıdaki sorguyu kullanarak yıllık sayıda TAXI bayıldığı 'i kolayca özetleyebilirsiniz:

```sql
SELECT
    YEAR(tpepPickupDateTime) AS current_year,
    COUNT(*) AS rides_per_year
FROM
    OPENROWSET(
        BULK 'https://azureopendatastorage.blob.core.windows.net/nyctlc/yellow/puYear=*/puMonth=*/*.parquet',
        FORMAT='PARQUET'
    ) AS [nyc]
WHERE nyc.filepath(1) >= '2009' AND nyc.filepath(1) <= '2019'
GROUP BY YEAR(tpepPickupDateTime)
ORDER BY 1 ASC
```

Aşağıdaki kod parçacığında, yıllık sayıda TAXI rides için sonuç gösterilmektedir:

![Yıllık TAXI bayıldığı sonuç parçacığı sayısı](./media/tutorial-data-analyst/4.png)

Veriler, **tablodan** **grafik** görünümüne geçiş yaparak SYNAPSE Studio 'da görselleştirilebilir. **Alan**, **çubuk**, **sütun**, **çizgi**, **pasta**ve **dağılım**gibi farklı grafik türleri arasından seçim yapabilirsiniz. Bu durumda, **sütun** grafiğinin **Kategori** sütununu **current_year**olarak çizimiyle:

![Yıl başına bayıldığı gösteren sütun grafiği](./media/tutorial-data-analyst/5.png)

Bu görselleştirmede, yıl boyunca azalan sayıda bayıldığı eğilimi açıkça görülebilir. Bu azalma, en iyi şekilde, Ride paylaşım şirketlerinin son derece artmasından kaynaklanır.

> [!NOTE]
> Bu öğreticinin yazıldığı sırada, 2019 için veriler eksik. Sonuç olarak, bu yıl için bayıldığı sayısı çok büyük bir düşüş vardır.

Ardından, Analizi tek bir yılda odaklanalım, örneğin, 2016. Aşağıdaki sorgu, bu yıl boyunca günlük bayıldığı sayısını döndürür:

```sql
SELECT
    CAST([tpepPickupDateTime] AS DATE) AS [current_day],
    COUNT(*) as rides_per_day
FROM
    OPENROWSET(
        BULK 'https://azureopendatastorage.blob.core.windows.net/nyctlc/yellow/puYear=*/puMonth=*/*.parquet',
        FORMAT='PARQUET'
    ) AS [nyc]
WHERE nyc.filepath(1) = '2016'
GROUP BY CAST([tpepPickupDateTime] AS DATE)
ORDER BY 1 ASC
```

Aşağıdaki kod parçacığında bu sorgunun sonucu gösterilmektedir:

![2016 sonuç parçacığı için günlük sayıda bayıldığı](./media/tutorial-data-analyst/6.png)

Daha sonra, **Kategori** sütunu **current_day** olarak ayarlanmış olan **sütun** grafiğini ve **gösterge (seri)** sütununu **rides_per_day**olarak belirterek verileri kolayca görselleştirebilirsiniz.

![2016 için günlük olarak bayıldığı gösteren sütun grafiği](./media/tutorial-data-analyst/7.png)

Çizim grafiğinde, en yoğun gün olarak Cumartesi günleri olan haftalık bir model olduğunu görebilirsiniz. Yaz ayları sırasında tatiller nedeniyle daha az TAXI bayıldığı vardır. Ayrıca, vergilenme sayısında bazı önemli düşmeler, ne zaman ve neden meydana gelen açık bir model olmadan de vardır.

Daha sonra, nzc TAXI bayıldığı veri kümesini genel tatiller veri kümesiyle birleştirerek genel tatiller ile ilişkilendirilenme olup olmadığını görelim:

```sql
WITH taxi_rides AS
(
    SELECT
        CAST([tpepPickupDateTime] AS DATE) AS [current_day],
        COUNT(*) as rides_per_day
    FROM  
        OPENROWSET(
            BULK 'https://azureopendatastorage.blob.core.windows.net/nyctlc/yellow/puYear=*/puMonth=*/*.parquet',
            FORMAT='PARQUET'
        ) AS [nyc]
    WHERE nyc.filepath(1) = '2016'
    GROUP BY CAST([tpepPickupDateTime] AS DATE)
),
public_holidays AS
(
    SELECT
        holidayname as holiday,
        date
    FROM
        OPENROWSET(
            BULK 'https://azureopendatastorage.blob.core.windows.net/holidaydatacontainer/Processed/*.parquet',
            FORMAT='PARQUET'
        ) AS [holidays]
    WHERE countryorregion = 'United States' AND YEAR(date) = 2016
)
SELECT
*
FROM taxi_rides t
LEFT OUTER JOIN public_holidays p on t.current_day = p.date
ORDER BY current_day ASC
```

![NYC TAXI bayıldığı ve genel tatiller veri kümeleri sonuç görselleştirme](./media/tutorial-data-analyst/8.png)

Bu kez, genel tatiller sırasında vergileni bayıldığı sayısını vurgulamak istiyoruz. Bu amaçla **Kategori** sütunu için **hiçbiri** ' ni, **gösterge (Seriler)** sütunları olarak **rides_per_day** ve **tatil** ' i seçeceğiz.

![Genel tatiller sırasında vergileni bayıldığı sayısı çizim grafiği](./media/tutorial-data-analyst/9.png)

Çizim grafiğinden, genel tatiller sırasında vergilenme sayısının düşük olduğunu görebilirsiniz. 23 Ocak 'ta açıklanamayan bir büyük bırakma hala var. Hava durumu verileri veri kümesini sorgulayarak, bu gündeki NYC içindeki hava durumunu kontrol edelim:

```sql
SELECT
    AVG(windspeed) AS avg_windspeed,
    MIN(windspeed) AS min_windspeed,
    MAX(windspeed) AS max_windspeed,
    AVG(temperature) AS avg_temperature,
    MIN(temperature) AS min_temperature,
    MAX(temperature) AS max_temperature,
    AVG(sealvlpressure) AS avg_sealvlpressure,
    MIN(sealvlpressure) AS min_sealvlpressure,
    MAX(sealvlpressure) AS max_sealvlpressure,
    AVG(precipdepth) AS avg_precipdepth,
    MIN(precipdepth) AS min_precipdepth,
    MAX(precipdepth) AS max_precipdepth,
    AVG(snowdepth) AS avg_snowdepth,
    MIN(snowdepth) AS min_snowdepth,
    MAX(snowdepth) AS max_snowdepth
FROM
    OPENROWSET(
        BULK 'https://azureopendatastorage.blob.core.windows.net/isdweatherdatacontainer/ISDWeather/year=*/month=*/*.parquet',
        FORMAT='PARQUET'
    ) AS [weather]
WHERE countryorregion = 'US' AND CAST([datetime] AS DATE) = '2016-01-23' AND stationname = 'JOHN F KENNEDY INTERNATIONAL AIRPORT'
```

![Hava durumu veri kümesi sonuç görselleştirme](./media/tutorial-data-analyst/10.png)

Sorgunun sonuçları, şu nedenle oluşan vergilenme sayısının meydana gelen bırakma sayısını gösterir:

- NYC 'de, ağır kar (~ 30 cm) ile bu günde bir göz at vardı.
- Bu, soğuk idi (sıcaklık 0 derecenin altında).
- Bu, WINDY (~ 10 m/s) idi.

Bu öğreticide, veri analistinin keşif veri analizini hızlı bir şekilde nasıl gerçekleştirebildiğini, isteğe bağlı SQL kullanarak farklı veri kümelerini kolayca nasıl birleştirebileceğini ve Azure SYNAPSE Studio 'Yu kullanarak sonuçları görselleştirmesini göstermiştir.

## <a name="next-steps"></a>Sonraki adımlar

SQL 'i Power BI Desktop ve rapor oluşturmak için isteğe bağlı olarak bağlama hakkında bilgi edinmek için bkz. [SQL isteğe bağlı olarak Power BI Desktop bağlama ve rapor oluşturma](tutorial-connect-power-bi-desktop.md).
 
