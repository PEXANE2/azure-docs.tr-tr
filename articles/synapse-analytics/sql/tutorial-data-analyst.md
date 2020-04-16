---
title: Veri Analisti öğreticisi - Azure Synapse Studio'daki Azure Açık Veri Kümelerini analiz etmek için isteğe bağlı SQL (önizleme) kullanın (önizleme)
description: Bu eğitimde, farklı Azure Açık Veri kümelerini isteğe bağlı SQL kullanarak (önizleme) birleştirerek keşif veri analizini kolayca nasıl gerçekleştireceğinizi ve Azure Synapse Studio'daki sonuçları görselleştirmeyi öğreneceksiniz.
services: synapse-analytics
author: azaricstefan
ms.service: synapse-analytics
ms.topic: tutorial
ms.subservice: ''
ms.date: 04/15/2020
ms.author: v-stazar
ms.reviewer: jrasnick, carlrab
ms.openlocfilehash: 1080001cb222f91503080914d7fb253e5ee82626
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81423232"
---
# <a name="use-sql-on-demand-preview-to-analyze-azure-open-datasets-and-visualize-the-results-in-azure-synapse-studio-preview"></a>Azure Açık Veri Kümelerini analiz etmek ve Sonuçları Azure Synapse Studio'da görselleştirmek için isteğe bağlı SQL'i (önizleme) kullanın (önizleme)

Bu eğitimde, farklı Azure Açık Veri kümelerini isteğe bağlı SQL kullanarak birleştirerek ve ardından Sonuçları Azure Synapse Studio'da görselleştirerek araştırmacı veri çözümlemesi yapmayı öğrenirsiniz.

Özellikle, pick-up ve bırakma tarihleri/saatleri, karşılama ve bırakma yerleri, seyahat mesafeleri, belirlenen ücretler, fiyat türleri, ödeme türleri ve sürücü tarafından bildirilen yolcu sayılarını içeren [New York (NYC) Taksi veri kümesini](https://azure.microsoft.com/services/open-datasets/catalog/nyc-taxi-limousine-commission-yellow-taxi-trip-records/) analiz emzebilirsiniz.

Analizin odak noktası, zaman içinde taksi seferlerinin sayısındaki değişimlerdeki eğilimleri bulmak. Diğer iki Azure Açık Veri kümesini[(Resmi Tatiller](https://azure.microsoft.com/services/open-datasets/catalog/public-holidays/) ve [Hava Durumu Verileri)](https://azure.microsoft.com/services/open-datasets/catalog/noaa-integrated-surface-data/)analiz edin ve taksi yolculuklarının sayısındaki aykırı ları anlarsınız.

## <a name="create-credentials"></a>Kimlik bilgileri oluşturma

```sql
-- There is no secret. We are using public storage account which doesn't need a secret.
CREATE CREDENTIAL [https://azureopendatastorage.blob.core.windows.net/nyctlc]
WITH IDENTITY='SHARED ACCESS SIGNATURE',
SECRET = ''
GO

CREATE CREDENTIAL [https://azureopendatastorage.blob.core.windows.net/holidaydatacontainer]
WITH IDENTITY='SHARED ACCESS SIGNATURE',
SECRET = ''
GO

CREATE CREDENTIAL [https://azureopendatastorage.blob.core.windows.net/isdweatherdatacontainer]
WITH IDENTITY='SHARED ACCESS SIGNATURE',
SECRET = ''
GO
```

## <a name="automatic-schema-inference"></a>Otomatik şema çıkarımı

Veriler Parke dosya biçiminde depolandığından, otomatik şema çıkarımı kullanılabilir, bu nedenle dosyalardaki tüm sütunların veri türlerini listelemek zorunda kalmadan verileri kolayca sorgulayabilirsiniz. Ayrıca, bir dosyaların belirli bir alt kümesi filtrelemek için sanal sütun mekanizması ve filepath işlevi kullanabilirsiniz.

Önce aşağıdaki sorguyu çalıştırarak NYC Taksi verilerini yakından izleyelim:

```sql
SELECT TOP 100 * FROM
    OPENROWSET(
        BULK 'https://azureopendatastorage.blob.core.windows.net/nyctlc/yellow/puYear=*/puMonth=*/*.parquet',
        FORMAT='PARQUET'
    ) AS [nyc]
```

Aşağıdaki NYC Taksi verileri için sonuç snippet gösterir:

![sonuç parçacığı](./media/tutorial-data-analyst/1.png)

Benzer şekilde, resmi tatiller veri kümesini aşağıdaki sorguyu kullanarak sorgulayabiliriz:

```sql
SELECT TOP 100 * FROM
    OPENROWSET(
        BULK 'https://azureopendatastorage.blob.core.windows.net/holidaydatacontainer/Processed/*.parquet',
        FORMAT='PARQUET'
    ) AS [holidays]
```

Aşağıdaki resmi tatiller veri kümesi için sonuç snippet gösterir:

![sonuç snippet 2](./media/tutorial-data-analyst/2.png)

Son olarak, aşağıdaki sorguyu kullanarak hava durumu veri kümesini de sorgulayabiliriz:

```sql
SELECT
    TOP 100 *
FROM  
    OPENROWSET(
        BULK 'https://azureopendatastorage.blob.core.windows.net/isdweatherdatacontainer/ISDWeather/year=*/month=*/*.parquet',
        FORMAT='PARQUET'
    ) AS [weather]
```

Aşağıda, hava durumu veri kümesinin sonucu gösterilmektedir:

![sonuç snippet 3](./media/tutorial-data-analyst/3.png)

[Nyc Taksi,](https://azure.microsoft.com/services/open-datasets/catalog/nyc-taxi-limousine-commission-yellow-taxi-trip-records/) [Resmi Tatiller](https://azure.microsoft.com/services/open-datasets/catalog/public-holidays/)ve Hava [Durumu Veri](https://azure.microsoft.com/services/open-datasets/catalog/noaa-integrated-surface-data/) veri kümelerinin açıklamalarında tek tek sütunların anlamı hakkında daha fazla bilgi edinebilirsiniz.

## <a name="time-series-seasonality-and-outlier-analysis"></a>Zaman serileri, mevsimsellik ve aykırı analiz

Aşağıdaki sorguyu kullanarak taksi yolculuklarının yıllık sayısını kolayca özetleyebilirsiniz:

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

Aşağıdaki taksi sürmek yıllık sayısı için sonuç snippet gösterir:

![sonuç snippet 4](./media/tutorial-data-analyst/4.png)

Veriler Tablo'dan Grafik görünümüne geçerek Synapse Studio'da görüntülenebilir. Farklı grafik türleri (Alan, Çubuk, Sütun, Satır, Pasta ve Dağılım) arasından seçim yapabilirsiniz. Bu durumda, Sütun grafiğini Kategori sütunu "current_year" olarak ayarlanmış olarak çizelim:

![sonuç görselleştirme 5](./media/tutorial-data-analyst/5.png)

Bu görselleştirme itibaren, yıllar içinde sürmek azalan sayıda bir eğilim açıkça görülebilir, muhtemelen sürüş paylaşım şirketlerinin son zamanlarda artan popülaritesi nedeniyle.

> [!NOTE]
> Bu öğretici nin yazıldığı sırada, 2019 yılı verileri eksiktir, bu nedenle o yıl için bir dizi sürüşte büyük bir düşüş söz vardır.

Sonra, analizlerimizi tek bir yıla odaklayalım, örneğin, 2016. Aşağıdaki sorgu, o yıl içinde günlük sürüş sayısını döndürür:

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

Aşağıdaki, bu sorgu için sonuç parçacığı gösterir:

![sonuç snippet 6](./media/tutorial-data-analyst/6.png)

Yine, Kategori sütun "current_day" ve Legend (dizi) sütun "rides_per_day" ile Sütun grafik çizerek verileri kolayca görselleştirebilirsiniz.

![sonuç görselleştirme 7](./media/tutorial-data-analyst/7.png)

Arsa itibaren, bu Cumartesi zirve ile, bir haftalık desen olduğu görülebilir. Yaz aylarında, tatil dönemi nedeniyle daha az taksi yolculukları vardır. Ancak, ne zaman ve neden meydana net bir desen olmadan taksi sürmek sayısında bazı önemli düşüşler de vardır.

Sonra, bu damla lar potansiyel resmi tatil dataset ile NYC taksi sürmek katılarak resmi tatil ile ilişkili olup olmadığını görelim:

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

![sonuç görselleştirme 8](./media/tutorial-data-analyst/8.png)

Bu kez, resmi tatillerde taksi yolculuklarının sayısını vurgulamak istiyoruz. Bu amaçla, Kategori sütunu için "yok"u ve "rides_per_day"ı ve "tatil"i Legend (dizi) sütunları olarak seçeceğiz.

![sonuç görselleştirme 9](./media/tutorial-data-analyst/9.png)

Arsa itibaren, açıkça resmi tatillerde taksi sürmek bir dizi daha düşük olduğu görülebilir. Ancak, 23 Ocak'ta hala açıklanamayan bir büyük düşüş var. O gün NYC'deki hava durumunu hava durumu veri kümesini sorgulayarak kontrol edelim:

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

![sonuç görselleştirme 10](./media/tutorial-data-analyst/10.png)

Sorgu sonuçları taksi sürmek bir dizi düşüş nedeniyle olduğunu göstermektedir:

- yoğun kar (~ 30 cm) olduğu gibi, NYC o gün kar fırtınası
- soğuktu (sıcaklık sıfır derecenin altında)
- ve rüzgarlı (~10m/s)

Bu öğretici, veri analistinin araştırmacı veri çözümlemesi nasıl hızlı bir şekilde gerçekleştirebileceğini, isteğe bağlı SQL kullanarak farklı veri kümelerini kolayca birleştirebileceğini ve Azure Synapse Studio'yu kullanarak sonuçları nasıl görselleştirebileceğini göstermiştir.

## <a name="next-steps"></a>Sonraki adımlar

İsteğe bağlı Olarak [Connect SQL'i Power BI Desktop'a](tutorial-connect-power-bi-desktop.md) gözden geçirin & SQL isteğe bağlı olarak Power BI Desktop'a nasıl bağlanıp raporlar oluşturup rapor oluşturup rapor oluşturup rapor oluşturabilirsiniz.
 
