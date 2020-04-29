---
title: Veri analist öğreticisi-Azure SYNAPSE Studio 'da Azure açık veri kümelerini analiz etmek için SQL isteğe bağlı (Önizleme) kullanma (Önizleme)
description: Bu öğreticide, SQL isteğe bağlı (Önizleme) kullanarak farklı Azure açık veri kümeleri birleştiren araştırmacı veri analizini kolayca gerçekleştirmeyi ve Azure SYNAPSE Studio 'da sonuçları görselleştirmeyi öğreneceksiniz.
services: synapse-analytics
author: azaricstefan
ms.service: synapse-analytics
ms.topic: tutorial
ms.subservice: ''
ms.date: 04/15/2020
ms.author: v-stazar
ms.reviewer: jrasnick, carlrab
ms.openlocfilehash: 1080001cb222f91503080914d7fb253e5ee82626
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "81423232"
---
# <a name="use-sql-on-demand-preview-to-analyze-azure-open-datasets-and-visualize-the-results-in-azure-synapse-studio-preview"></a>Azure açık veri kümelerini analiz etmek ve sonuçları Azure SYNAPSE Studio 'da (Önizleme) görselleştirmek için SQL isteğe bağlı (Önizleme) kullanın

Bu öğreticide, Azure SYNAPSE Studio 'da SQL 'i isteğe bağlı olarak kullanarak farklı Azure açık veri kümelerini birleştirerek araştırmacı veri analizini gerçekleştirmeyi öğrenirsiniz.

Özellikle, seçme ve bırakma tarihleri/zamanları, çekme ve bırakma konumları, seyahat mesafeleri, liste, ücret türleri, ödeme türleri ve sürücü tarafından bildirilen yolcular sayımlarını içeren [New York City (NYC) TAXI veri kümesini](https://azure.microsoft.com/services/open-datasets/catalog/nyc-taxi-limousine-commission-yellow-taxi-trip-records/) analiz edersiniz.

Çözümlemenin odaklanmasına, zaman içinde vergilenklarca riçlü sayıdaki değişiklikler hakkında eğilimleri bulmanız. Diğer iki Azure açık veri kümesini ([genel tatiller](https://azure.microsoft.com/services/open-datasets/catalog/public-holidays/) ve [Hava durumu verileri](https://azure.microsoft.com/services/open-datasets/catalog/noaa-integrated-surface-data/)) analiz edersiniz.

## <a name="create-credentials"></a>Kimlik bilgileri oluştur

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

## <a name="automatic-schema-inference"></a>Otomatik Şema çıkarımı

Veriler Parquet dosya biçiminde depolandığından, otomatik Şema çıkarımı kullanılabilir, bu yüzden bir tane, dosyalardaki tüm sütunların veri türlerini listelemek gerekmeden verileri kolayca sorgulayabilir. Ayrıca, bir tanesi belirli bir dosya alt kümesini filtrelemek için sanal sütun mekanizması ve FilePath işlevini kullanabilir.

Aşağıdaki sorguyu çalıştırarak NYC TAXI verilerini ilk kez öğrenelim:

```sql
SELECT TOP 100 * FROM
    OPENROWSET(
        BULK 'https://azureopendatastorage.blob.core.windows.net/nyctlc/yellow/puYear=*/puMonth=*/*.parquet',
        FORMAT='PARQUET'
    ) AS [nyc]
```

Aşağıda, NYC TAXI verileri için sonuç parçacığı gösterilmektedir:

![Sonuç kod parçacığı](./media/tutorial-data-analyst/1.png)

Benzer şekilde, aşağıdaki sorguyu kullanarak genel tatiller veri kümesini sorgulayabiliriz:

```sql
SELECT TOP 100 * FROM
    OPENROWSET(
        BULK 'https://azureopendatastorage.blob.core.windows.net/holidaydatacontainer/Processed/*.parquet',
        FORMAT='PARQUET'
    ) AS [holidays]
```

Aşağıda, genel tatiller veri kümesi için sonuç parçacığı gösterilmektedir:

![Sonuç kod parçacığı 2](./media/tutorial-data-analyst/2.png)

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

Aşağıda Hava durumu veri kümesi için sonuç parçacığı gösterilmektedir:

![Sonuç kod parçacığı 3](./media/tutorial-data-analyst/3.png)

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

Aşağıda, yıllık sayıda TAXI rides için sonuç parçacığı gösterilmektedir:

![Sonuç kod parçacığı 4](./media/tutorial-data-analyst/4.png)

Veriler, tablodan grafik görünümüne geçerek SYNAPSE Studio 'da görselleştirilebilir. Farklı grafik türleri (alan, çubuk, sütun, çizgi, pasta ve dağılım) arasından seçim yapabilirsiniz. Bu durumda, Kategori sütunu "current_year" olarak ayarlanmış sütun grafiği çizelim:

![Sonuç görselleştirme 5](./media/tutorial-data-analyst/5.png)

Bu görselleştirmede, yıl boyunca azalan sayıda bayıldığı eğilimi, büyük bir olasılıkla paylaşım şirketlerinin son derece artmasından dolayı açıkça görülebilir.

> [!NOTE]
> Bu öğreticinin yazıldığı sırada, 2019 için veriler eksik olduğundan bu yıl için bir dizi bayıldığı de çok büyük bir düşüş vardır.

Daha sonra, analizimize tek bir yılda odaklanalım, örneğin, 2016. Aşağıdaki sorgu, söz konusu yıl boyunca günlük sayıda bayıldığı döndürür:

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

Bu sorgu için sonuç parçacığı aşağıda gösterilmiştir:

![Sonuç kod parçacığı 6](./media/tutorial-data-analyst/6.png)

Daha sonra, "current_day" Kategori sütunu ve "rides_per_day" gösterge (seri) sütunuyla sütun grafiği çizdirme yoluyla verileri kolayca görselleştirebiliriz.

![Sonuç görselleştirme 7](./media/tutorial-data-analyst/7.png)

Çizinden, Cumartesi 'ın tepe noktası ile haftalık bir model olduğunu gözlemlenebilir. Yaz ayları sırasında, tatil dönemi nedeniyle daha az sayıda TAXI bayıldığı vardır. Ancak, bazı önemli bırakmalarca, açık bir model olmadan ve neden gerçekleştiklerinde, açıkça bir sıra meydana gelir.

Daha sonra, bu bırakıtlar ortak tatiller veri kümesiyle NYC TAXI bayıldığı 'e katılarak genel tatiller ile bağıntılı olup olmadığını görelim:

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

![Sonuç görselleştirme 8](./media/tutorial-data-analyst/8.png)

Bu kez, genel tatiller sırasında vergileni bayıldığı sayısını vurgulamak istiyoruz. Bu amaçla, Kategori sütunu için "none", "rides_per_day" ve gösterge (seri) sütunları olarak "tatil" i seçeceğiz.

![Sonuç görselleştirme 9](./media/tutorial-data-analyst/9.png)

Çizmeden, genel tatiller sırasında çok sayıda TAXI bayıldığı 'in daha düşük olduğu açıkça görülebilir. Ancak yine de, 23 Ocak tarihinde açıklanamayan bir çok büyük bir bırakma vardır. Hava durumu veri kümesini sorgulayarak bu gündeki NYC içindeki hava durumunu kontrol edelim:

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

![Sonuç görselleştirme 10](./media/tutorial-data-analyst/10.png)

Sorgunun sonuçları, bir dizi TAXI 'nin şu nedenlerle olduğunu belirtir:

- büyük bir kar vardı (yaklaşık 30 cm), NYC 'de o güne göz at
- Bu, soğuk (santigrat derece düşük olan sıcaklık)
- ve WINDY (~ 10m/sn)

Bu öğretici, veri analistinin keşif veri analizini hızlı bir şekilde nasıl gerçekleştirebildiğini, isteğe bağlı SQL kullanarak farklı veri kümelerini kolayca nasıl birleştirebileceğini ve Azure SYNAPSE Studio 'Yu kullanarak sonuçları görselleştirmesini göstermiştir

## <a name="next-steps"></a>Sonraki adımlar

İsteğe bağlı SQL 'i Power BI Desktop 'e nasıl bağlayacağınızı ve rapor oluşturmayı öğrenmek için bkz. [SQL isteğe bağlı bağlantı Power BI Desktop & rapor oluşturma](tutorial-connect-power-bi-desktop.md) makalesi.
 
