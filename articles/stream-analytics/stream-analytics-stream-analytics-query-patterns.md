---
title: Azure Akış Analizi'nde yaygın sorgu desenleri
description: Bu makalede, Azure Akış Analizi işlerinde yararlı olan birkaç yaygın sorgu deletiği ve tasarımı açıklanmaktadır.
services: stream-analytics
author: rodrigoaatmicrosoft
ms.author: rodrigoa
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 12/18/2019
ms.openlocfilehash: aa8bd6e89dd47c4e972a860691d1bc3779ba5bc7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75982307"
---
# <a name="common-query-patterns-in-azure-stream-analytics"></a>Azure Akış Analizi'nde yaygın sorgu desenleri

Azure Akış Analizi'ndeki sorgular SQL benzeri bir sorgu dilinde ifade edilir. Dil yapıları [Stream Analytics sorgu dili başvuru](/stream-analytics-query/stream-analytics-query-language-reference) kılavuzunda belgelenmiştir. 

Sorgu tasarımı, olay verilerini bir giriş akışından bir çıktı veri deposuna taşımak için basit geçiş mantığını ifade edebilir veya Akış Analizi [kılavuzunu kullanarak](stream-analytics-build-an-iot-solution-using-stream-analytics.md) çeşitli zaman pencerelerinde olduğu gibi çeşitli zaman pencerelerinde toplamları hesaplamak için zengin desen eşleştirme ve zamansal çözümleme yapabilir. Akış olaylarını birleştirmek için birden çok girişten gelen verileri birleştirebilir ve olay değerlerini zenginleştirmek için statik başvuru verilerine karşı arama yapabilirsiniz. Birden çok çıktıya veri de yazabilirsiniz.

Bu makalede, gerçek dünya senaryolarına dayalı birkaç yaygın sorgu desenleri çözümleri özetler.

## <a name="supported-data-formats"></a>Desteklenen Veri Biçimleri

Azure Akış Analizi, CSV, JSON ve Avro veri biçimlerindeki işleme olaylarını destekler.

Hem JSON hem de Avro iç içe nesneler (kayıtlar) veya diziler gibi karmaşık türler içerebilir. Bu karmaşık veri türleri ile çalışma hakkında daha fazla bilgi için [Ayrıştırma JSON ve AVRO veri](stream-analytics-parsing-json.md) makalesine bakın.

## <a name="simple-pass-through-query"></a>Basit geçiş sorgusu

Giriş akışı verilerini çıktıya kopyalamak için basit bir geçiş sorgusu kullanılabilir. Örneğin, harf çözümlemesi için sql veritabanına gerçek zamanlı araç bilgileri içeren bir veri akışı kaydedilmesi gerekiyorsa, basit bir geçiş sorgusu işi yapar.

**Giriş**:

| Marka | Zaman | Ağırlık |
| --- | --- | --- |
| Make1 |2015-01-01T00:00:01.000000Z |"1000" |
| Make1 |2015-01-01T00:00:02.000000Z |"2000" |

**Çıktı**:

| Marka | Zaman | Ağırlık |
| --- | --- | --- |
| Make1 |2015-01-01T00:00:01.000000Z |"1000" |
| Make1 |2015-01-01T00:00:02.000000Z |"2000" |

**Sorgu :**

```SQL
SELECT
    *
INTO Output
FROM Input
```

**SELECT** * sorgusu, gelen bir olayın tüm alanlarını projeleri ve bunları çıktıya gönderir. Aynı şekilde, **SELECT** yalnızca girişten gerekli alanları yansıtmak için de kullanılabilir. Bu örnekte, araç *Yap* ve *Saat* kaydedilmesi gereken tek alanlarsa, bu alanlar **SELECT** deyiminde belirtilebilir.

**Giriş**:

| Marka | Zaman | Ağırlık |
| --- | --- | --- |
| Make1 |2015-01-01T00:00:01.000000Z |1000 |
| Make1 |2015-01-01T00:00:02.000000Z |2000 |
| Make2 |2015-01-01T00:00:04.000000Z |1500 |

**Çıktı**:

| Marka | Zaman |
| --- | --- |
| Make1 |2015-01-01T00:00:01.000000Z |
| Make1 |2015-01-01T00:00:02.000000Z |
| Make2 |2015-01-01T00:00:04.000000Z |

**Sorgu :**

```SQL
SELECT
    Make, Time
INTO Output
FROM Input
```
## <a name="data-aggregation-over-time"></a>Zaman içinde veri toplama

Bir zaman penceresi üzerinden bilgi hesaplamak için veriler bir araya toplanabilir. Bu örnekte, her belirli araç yapmak için zaman son 10 dakika içinde bir sayı hesaplanır.

**Giriş**:

| Marka | Zaman | Ağırlık |
| --- | --- | --- |
| Make1 |2015-01-01T00:00:01.000000Z |1000 |
| Make1 |2015-01-01T00:00:02.000000Z |2000 |
| Make2 |2015-01-01T00:00:04.000000Z |1500 |

**Çıktı**:

| Marka | Sayı |
| --- | --- |
| Make1 | 2 |
| Make2 | 1 |

**Sorgu :**

```SQL
SELECT
    Make,
    COUNT(*) AS Count
FROM
    Input TIMESTAMP BY Time
GROUP BY
    Make,
    TumblingWindow(second, 10)
```

Bu *toplama,* arabaları Make ile gruplandırmave her 10 saniyede bir sayar. Çıkışta geçiş ücretinden geçen arabaların *Yap* ve Sayısını nitret-i sayı yada *mış.*

TumblingWindow, olayları gruplandırmak için kullanılan bir pencere işlevidir. Bir toplama tüm gruplanmış olaylar üzerinde uygulanabilir. Daha fazla bilgi için [pencereleme işlevlerine](stream-analytics-window-functions.md)bakın.

Toplama hakkında daha fazla bilgi [için, toplu işlevlere](/stream-analytics-query/aggregate-functions-azure-stream-analytics)bakın.

## <a name="data-conversion"></a>Veri dönüştürme

Veriler **CAST** yöntemi kullanılarak gerçek zamanlı olarak kullanılabilir. Örneğin, araç ağırlığı tip **nvarchar (max)** **türünden bigint** türüne dönüştürülebilir ve sayısal bir hesaplamada kullanılabilir.

**Giriş**:

| Marka | Zaman | Ağırlık |
| --- | --- | --- |
| Make1 |2015-01-01T00:00:01.000000Z |"1000" |
| Make1 |2015-01-01T00:00:02.000000Z |"2000" |

**Çıktı**:

| Marka | Ağırlık |
| --- | --- |
| Make1 |3000 |

**Sorgu :**

```SQL
SELECT
    Make,
    SUM(CAST(Weight AS BIGINT)) AS Weight
FROM
    Input TIMESTAMP BY Time
GROUP BY
    Make,
    TumblingWindow(second, 10)
```

Veri türünü belirtmek için **CAST** deyimi kullanın. Veri türlerinde desteklenen veri türlerinin listesine bakın [(Azure Akışı Analizi)](/stream-analytics-query/data-types-azure-stream-analytics).

Veri dönüştürme [işlevleri](/stream-analytics-query/conversion-functions-azure-stream-analytics)hakkında daha fazla bilgi için.

## <a name="string-matching-with-like-and-not-like"></a>LIKE ve LIKE ile eşleşen dize

**BEĞEN Ve** **BEĞENNMİş Değİl,** bir alanın belirli bir desenle eşleşip eşleşmediğini doğrulamak için kullanılabilir. Örneğin, yalnızca 'A' harfiyle başlayan ve 9 sayısıyla biten plakaları döndürmek için bir filtre oluşturulabilir.

**Giriş**:

| Marka | License_plate | Zaman |
| --- | --- | --- |
| Make1 |ABC-123 |2015-01-01T00:00:01.000000Z |
| Make2 |AAA-999 |2015-01-01T00:00:02.000000Z |
| Make3 |ABC-369 |2015-01-01T00:00:03.000000Z |

**Çıktı**:

| Marka | License_plate | Zaman |
| --- | --- | --- |
| Make2 |AAA-999 |2015-01-01T00:00:02.000000Z |
| Make3 |ABC-369 |2015-01-01T00:00:03.000000Z |

**Sorgu :**

```SQL
SELECT
    *
FROM
    Input TIMESTAMP BY Time
WHERE
    License_plate LIKE 'A%9'
```

**License_plate** alan değerini denetlemek için **LIKE** deyimini kullanın. 'A' harfi ile başlamalı, sonra 9 sayısıyla biten sıfır veya daha fazla karakter dizesi olmalıdır.

## <a name="specify-logic-for-different-casesvalues-case-statements"></a>Farklı durumlar/değerler için mantık belirtin (CASE deyimleri)

**CASE** deyimleri, belirli ölçütlere göre farklı alanlar için farklı hesaplamalar sağlayabilir. Örneğin, *Make1'in* araçlarına 'A' ve başka herhangi bir make'e 'B' şeridi atayın.

**Giriş**:

| Marka | Zaman |
| --- | --- |
| Make1 |2015-01-01T00:00:01.000000Z |
| Make2 |2015-01-01T00:00:02.000000Z |
| Make2 |2015-01-01T00:00:03.000000Z |

**Çıktı**:

| Marka |Dispatch_to_lane | Zaman |
| --- | --- | --- |
| Make1 |"A" |2015-01-01T00:00:01.000000Z |
| Make2 |"B" |2015-01-01T00:00:02.000000Z |

**Çözüm**:

```SQL
SELECT
    Make
    CASE
        WHEN Make = "Make1" THEN "A"
        ELSE "B"
    END AS Dispatch_to_lane,
    System.TimeStamp() AS Time
FROM
    Input TIMESTAMP BY Time
```

**CASE** ifadesi, sonucubelirlemek için bir ifadeyi basit ifadeler kümesiyle karşılaştırır. Bu örnekte, *Make1* araçları 'A' şeridine gönderilirken, başka herhangi bir araç 'B' şeridi atanır.

Daha fazla bilgi için [büyük/küçük harf ifadesine](/stream-analytics-query/case-azure-stream-analytics)bakın.

## <a name="send-data-to-multiple-outputs"></a>Birden çok çıktıya veri gönderme

Farklı çıktı lavabolarına veri çıktısı için birden çok **SELECT** deyimi kullanılabilir. Örneğin, bir **SELECT** eşik tabanlı bir uyarı çıktırırken, diğeri de blob depolamasına olay çıktısı verebilir.

**Giriş**:

| Marka | Zaman |
| --- | --- |
| Make1 |2015-01-01T00:00:01.000000Z |
| Make1 |2015-01-01T00:00:02.000000Z |
| Make2 |2015-01-01T00:00:01.000000Z |
| Make2 |2015-01-01T00:00:02.000000Z |
| Make2 |2015-01-01T00:00:03.000000Z |

**Çıktı Arşivi Çıktısı**:

| Marka | Zaman |
| --- | --- |
| Make1 |2015-01-01T00:00:01.000000Z |
| Make1 |2015-01-01T00:00:02.000000Z |
| Make2 |2015-01-01T00:00:01.000000Z |
| Make2 |2015-01-01T00:00:02.000000Z |
| Make2 |2015-01-01T00:00:03.000000Z |

**Çıkış Uyarısı Çıktısı**:

| Marka | Zaman | Sayı |
| --- | --- | --- |
| Make2 |2015-01-01T00:00:10.000000Z |3 |

**Sorgu :**

```SQL
SELECT
    *
INTO
    ArchiveOutput
FROM
    Input TIMESTAMP BY Time

SELECT
    Make,
    System.TimeStamp() AS Time,
    COUNT(*) AS [Count]
INTO
    AlertOutput
FROM
    Input TIMESTAMP BY Time
GROUP BY
    Make,
    TumblingWindow(second, 10)
HAVING
    [Count] >= 3
```

**INTO** yan tümcesi, Stream Analytics'e verileri hangi çıktılara yazacaklarını söyler. İlk **SELECT,** girişten veri alan ve **ArchiveOutput**adlı çıktıya gönderen bir geçiş sorgusu tanımlar. İkinci sorgu, sonuçları **AlertOutput**adlı bir akış aşağı uyarı sistemi çıkışına göndermeden önce bazı basit toplama ve filtreleme yapar.

**WITH** yan tümcesinin birden çok alt sorgu engelini tanımlamak için kullanılabileceğini unutmayın. Bu seçenek, giriş kaynağına daha az okuyucu açma nın avantajına sahiptir.

**Sorgu :**

```SQL
WITH ReaderQuery AS (
    SELECT
        *
    FROM
        Input TIMESTAMP BY Time
)

SELECT * INTO ArchiveOutput FROM ReaderQuery

SELECT 
    Make,
    System.TimeStamp() AS Time,
    COUNT(*) AS [Count] 
INTO AlertOutput 
FROM ReaderQuery
GROUP BY
    Make,
    TumblingWindow(second, 10)
HAVING [Count] >= 3
```

Daha fazla bilgi için [ **WITH** yan tümcesine](/stream-analytics-query/with-azure-stream-analytics)bakın.

## <a name="count-unique-values"></a>Benzersiz değerleri sayma

**COUNT** ve **DISTINCT,** bir zaman penceresinde akışta görünen benzersiz alan değerlerinin sayısını saymak için kullanılabilir. 2 saniyelik bir pencerede *gişeden* kaç tane benzersiz araç geçtiğini hesaplamak için bir sorgu oluşturulabilir.

**Giriş**:

| Marka | Zaman |
| --- | --- |
| Make1 |2015-01-01T00:00:01.000000Z |
| Make1 |2015-01-01T00:00:02.000000Z |
| Make2 |2015-01-01T00:00:01.000000Z |
| Make2 |2015-01-01T00:00:02.000000Z |
| Make2 |2015-01-01T00:00:03.000000Z |

**Çıkış:**

| Count_make | Zaman |
| --- | --- |
| 2 |2015-01-01T00:00:02.000Z |
| 1 |2015-01-01T00:00:04.000Z |

**Sorgu:**

```SQL
SELECT
     COUNT(DISTINCT Make) AS Count_make,
     System.TIMESTAMP() AS Time
FROM Input TIMESTAMP BY TIME
GROUP BY 
     TumblingWindow(second, 2)
```

**COUNT(DISTINCT Make)** bir zaman penceresi içinde **Yap** sütunundaki farklı değerlerin sayısını döndürür.
Daha fazla bilgi için [ **COUNT** toplam işlevine](/stream-analytics-query/count-azure-stream-analytics)bakın.

## <a name="calculation-over-past-events"></a>Geçmiş olaylara göre hesaplama

**LAG** işlevi, bir zaman penceresi içindeki geçmiş olaylara bakmak ve bunları geçerli olayla karşılaştırmak için kullanılabilir. Örneğin, geçiş ücreti geçti son araba farklı ise geçerli araba yapmak çıktı olabilir.

**Giriş**:

| Marka | Zaman |
| --- | --- |
| Make1 |2015-01-01T00:00:01.000000Z |
| Make2 |2015-01-01T00:00:02.000000Z |

**Çıktı**:

| Marka | Zaman |
| --- | --- |
| Make2 |2015-01-01T00:00:02.000000Z |

**Sorgu :**

```SQL
SELECT
    Make,
    Time
FROM
    Input TIMESTAMP BY Time
WHERE
    LAG(Make, 1) OVER (LIMIT DURATION(minute, 1)) <> Make
```

Giriş akışına bir olayı geri gözetlemek, *Make* değerini almak ve geçerli olayın *yapma* değeri yle karşılaştırmak ve olayı çıktılamak için **LAG'yi** kullanın.

Daha fazla bilgi için [**LAG'ye**](/stream-analytics-query/lag-azure-stream-analytics)bakın.

## <a name="retrieve-the-first-event-in-a-window"></a>Penceredeki ilk olayı alma

**IsFirst,** bir zaman penceresindeki ilk olayı almak için kullanılabilir. Örneğin, her 10 dakikalık aralıklarla ilk araç bilgilerini ortaya çıkarır.

**Giriş**:

| License_plate | Marka | Zaman |
| --- | --- | --- |
| DXE 5291 |Make1 |2015-07-27T00:00:05.000000Z |
| YZK 5704 |Make3 |2015-07-27T00:02:17.000000Z |
| RMV 8282 |Make1 |2015-07-27T00:05:01.000000Z |
| YHN 6970 |Make2 |2015-07-27T00:06:00.000000Z |
| VFE 1616 |Make2 |2015-07-27T00:09:31.000000Z |
| QYF 9358 |Make1 |2015-07-27T00:12:02.000000Z |
| MDR 6128 |Make4 |2015-07-27T00:13:45.000000Z |

**Çıktı**:

| License_plate | Marka | Zaman |
| --- | --- | --- |
| DXE 5291 |Make1 |2015-07-27T00:00:05.000000Z |
| QYF 9358 |Make1 |2015-07-27T00:12:02.000000Z |

**Sorgu :**

```SQL
SELECT 
    License_plate,
    Make,
    Time
FROM 
    Input TIMESTAMP BY Time
WHERE 
    IsFirst(minute, 10) = 1
```

**IsFirst** ayrıca verileri bölümleyebilir ve her 10 dakikalık aralıkta bulunan olun her belirli araç *için* ilk olay hesaplayabilirsiniz.

**Çıktı**:

| License_plate | Marka | Zaman |
| --- | --- | --- |
| DXE 5291 |Make1 |2015-07-27T00:00:05.000000Z |
| YZK 5704 |Make3 |2015-07-27T00:02:17.000000Z |
| YHN 6970 |Make2 |2015-07-27T00:06:00.000000Z |
| QYF 9358 |Make1 |2015-07-27T00:12:02.000000Z |
| MDR 6128 |Make4 |2015-07-27T00:13:45.000000Z |

**Sorgu :**

```SQL
SELECT 
    License_plate,
    Make,
    Time
FROM 
    Input TIMESTAMP BY Time
WHERE 
    IsFirst(minute, 10) OVER (PARTITION BY Make) = 1
```

Daha fazla bilgi için [**IsFirst'e**](/stream-analytics-query/isfirst-azure-stream-analytics)bakın.

## <a name="return-the-last-event-in-a-window"></a>Penceredeki son olayı döndürme

Olaylar sistem tarafından gerçek zamanlı olarak tüketildiklerinden, bir olayın bu zaman penceresine en son gelip gelmeyeceğini belirleyebilecek bir işlev yoktur. Bunu başarmak için, giriş akışının, bir olayın zamanının o penceredeki tüm olaylar için en yüksek süre olduğu başka bir akışla birleştirilmesi gerekir.

**Giriş**:

| License_plate | Marka | Zaman |
| --- | --- | --- |
| DXE 5291 |Make1 |2015-07-27T00:00:05.000000Z |
| YZK 5704 |Make3 |2015-07-27T00:02:17.000000Z |
| RMV 8282 |Make1 |2015-07-27T00:05:01.000000Z |
| YHN 6970 |Make2 |2015-07-27T00:06:00.000000Z |
| VFE 1616 |Make2 |2015-07-27T00:09:31.000000Z |
| QYF 9358 |Make1 |2015-07-27T00:12:02.000000Z |
| MDR 6128 |Make4 |2015-07-27T00:13:45.000000Z |

**Çıktı**:

| License_plate | Marka | Zaman |
| --- | --- | --- |
| VFE 1616 |Make2 |2015-07-27T00:09:31.000000Z |
| MDR 6128 |Make4 |2015-07-27T00:13:45.000000Z |

**Sorgu :**

```SQL
WITH LastInWindow AS
(
    SELECT 
        MAX(Time) AS LastEventTime
    FROM 
        Input TIMESTAMP BY Time
    GROUP BY 
        TumblingWindow(minute, 10)
)

SELECT 
    Input.License_plate,
    Input.Make,
    Input.Time
FROM
    Input TIMESTAMP BY Time 
    INNER JOIN LastInWindow
    ON DATEDIFF(minute, Input, LastInWindow) BETWEEN 0 AND 10
    AND Input.Time = LastInWindow.LastEventTime
```

Sorgudaki ilk adım, 10 dakikalık pencerelerdeki en yüksek zaman damgasını, yani bu pencereiçin son olayın zaman damgasını bulur. İkinci adım, her penceredeki son kez damgalarla eşleşen olayı bulmak için ilk sorgunun sonuçlarını özgün akışla birleştirir. 

**DATEDIFF,** daha fazla bilgi için [tarih işlevlerine](https://docs.microsoft.com/stream-analytics-query/date-and-time-functions-azure-stream-analytics)bakın, iki DateTime alanı arasındaki saat farkını karşılaştıran ve döndüren tarihe özgü bir işlevdir.

Akışları birleştirme hakkında daha fazla bilgi için [**JOIN'e**](/stream-analytics-query/join-azure-stream-analytics)bakın.


## <a name="correlate-events-in-a-stream"></a>Akıştaki olayları ilişkilendirme

Aynı akıştaki olayları **ilişkilendirme, LAG** işlevini kullanarak geçmiş olaylara bakarak yapılabilir. Örneğin, aynı *Make'ten* art arda iki araba son 90 saniye boyunca geçiş ücretinden her seferinde bir çıktı oluşturulabilir.

**Giriş**:

| Marka | License_plate | Zaman |
| --- | --- | --- |
| Make1 |ABC-123 |2015-01-01T00:00:01.000000Z |
| Make1 |AAA-999 |2015-01-01T00:00:02.000000Z |
| Make2 |DEF-987 |2015-01-01T00:00:03.000000Z |
| Make1 |GHI-345 |2015-01-01T00:00:04.000000Z |

**Çıktı**:

| Marka | Zaman | Current_car_license_plate | First_car_license_plate | First_car_time |
| --- | --- | --- | --- | --- |
| Make1 |2015-01-01T00:00:02.000000Z |AAA-999 |ABC-123 |2015-01-01T00:00:01.000000Z |

**Sorgu :**

```SQL
SELECT
    Make,
    Time,
    License_plate AS Current_car_license_plate,
    LAG(License_plate, 1) OVER (LIMIT DURATION(second, 90)) AS First_car_license_plate,
    LAG(Time, 1) OVER (LIMIT DURATION(second, 90)) AS First_car_time
FROM
    Input TIMESTAMP BY Time
WHERE
    LAG(Make, 1) OVER (LIMIT DURATION(second, 90)) = Make
```

**LAG** işlevi, giriş akışına bir olay akışını geri inceleyebilir ve geçerli olayın *Yapma* değeriyle karşılaştırarak *Make* değerini alabilir.  Koşul karşılandıktan sonra, önceki olaydaki veriler **SELECT** deyimindeki **LAG** kullanılarak yansıtılabilir.

Daha fazla bilgi için [LAG'ye](/stream-analytics-query/lag-azure-stream-analytics)bakın.

## <a name="detect-the-duration-between-events"></a>Olaylar arasındaki süreyi algılama

Bir Bitiş olayı alındıktan sonra bir etkinliğin süresi son Başlangıç etkinliğine bakarak hesaplanabilir. Bu sorgu, bir kullanıcının bir sayfada veya özellikte harcadığı zamanı belirlemek için yararlı olabilir.

**Giriş**:  

| Kullanıcı | Özellik | Olay | Zaman |
| --- | --- | --- | --- |
| user@location.com |Sağ Menü |Başlangıç |2015-01-01T00:00:01.000000Z |
| user@location.com |Sağ Menü |End |2015-01-01T00:00:08.000000Z |

**Çıktı**:  

| Kullanıcı | Özellik | Süre |
| --- | --- | --- |
| user@location.com |Sağ Menü |7 |

**Sorgu :**

```SQL
SELECT
    [user],
    feature,
    DATEDIFF(
        second,
        LAST(Time) OVER (PARTITION BY [user], feature LIMIT DURATION(hour, 1) WHEN Event = 'start'),
        Time) as duration
FROM input TIMESTAMP BY Time
WHERE
    Event = 'end'
```

**LAST** işlevi belirli bir koşulda son olayı almak için kullanılabilir. Bu örnekte, koşul, aramayı kullanıcı ve özelliğe **göre BÖLÜMleme** türü başlangıç olayıdır. Bu şekilde, Başlat olayı aranırken her kullanıcı ve özellik bağımsız olarak ele alının. **LIMIT SÜRESİ,** aramayı bitiş ve Başlangıç olayları arasında 1 saate kadar zaman içinde sınırlandırın.

## <a name="detect-the-duration-of-a-condition"></a>Bir koşulun süresini algılama

Birden çok olaya yayılan koşullar için **LAG** işlevi, bu koşulun süresini belirlemek için kullanılabilir. Örneğin, bir hatanın tüm araçların yanlış ağırla (20.000 pound'un üzerinde) olmasına neden olduğunu ve bu hatanın süresinin hesaplanması gerektiğini varsayalım.

**Giriş**:

| Marka | Zaman | Ağırlık |
| --- | --- | --- |
| Make1 |2015-01-01T00:00:01.000000Z |2000 |
| Make2 |2015-01-01T00:00:02.000000Z |25000 |
| Make1 |2015-01-01T00:00:03.000000Z |26000 |
| Make2 |2015-01-01T00:00:04.000000Z |25000 |
| Make1 |2015-01-01T00:00:05.000000Z |26000 |
| Make2 |2015-01-01T00:00:06.000000Z |25000 |
| Make1 |2015-01-01T00:00:07.000000Z |26000 |
| Make2 |2015-01-01T00:00:08.000000Z |2000 |

**Çıktı**:

| Start_fault | End_fault |
| --- | --- |
| 2015-01-01T00:00:02.000Z |2015-01-01T00:00:07.000Z |

**Sorgu :**

```SQL
WITH SelectPreviousEvent AS
(
SELECT
    *,
    LAG([time]) OVER (LIMIT DURATION(hour, 24)) as previous_time,
    LAG([weight]) OVER (LIMIT DURATION(hour, 24)) as previous_weight
FROM input TIMESTAMP BY [time]
)

SELECT 
    LAG(time) OVER (LIMIT DURATION(hour, 24) WHEN previous_weight < 20000 ) [Start_fault],
    previous_time [End_fault]
FROM SelectPreviousEvent
WHERE
    [weight] < 20000
    AND previous_weight > 20000
```
İlk **SELECT** deyimi, geçerli ağırlık ölçüsünü önceki ölçümle ilişkilendirerek, geçerli ölçümle birlikte yansıtılır. İkinci **SELECT,** *previous_weight* 20000'den az olduğu, geçerli ağırlığın 20000'den küçük olduğu ve geçerli olayın *previous_weight* 20000'den daha büyük olduğu son olaya bakar.

End_fault, önceki olayın hatalı olduğu geçerli hatalı olmayan olaydır ve Start_fault bundan önceki son hatalı olmayan olaydır.

## <a name="periodically-output-values"></a>Periyodik çıkış değerleri

Düzensiz veya eksik olaylar durumunda, daha seyrek bir veri girişinden düzenli aralıklı çıktı oluşturulabilir. Örneğin, her 5 saniyede bir en son görülen veri noktasını bildiren bir olay oluşturun.

**Giriş**:

| Zaman | Değer |
| --- | --- |
| "2014-01-01T06:01:00" |1 |
| "2014-01-01T06:01:05" |2 |
| "2014-01-01T06:01:10" |3 |
| "2014-01-01T06:01:15" |4 |
| "2014-01-01T06:01:30" |5 |
| "2014-01-01T06:01:35" |6 |

**Çıktı (ilk 10 satır)**:

| Window_end | Last_event. Zaman | Last_event. Değer |
| --- | --- | --- |
| 2014-01-01T14:01:00.000Z |2014-01-01T14:01:00.000Z |1 |
| 2014-01-01T14:01:05.000Z |2014-01-01T14:01:05.000Z |2 |
| 2014-01-01T14:01:10.000Z |2014-01-01T14:01:10.000Z |3 |
| 2014-01-01T14:01:15.000Z |2014-01-01T14:01:15.000Z |4 |
| 2014-01-01T14:01:20.000Z |2014-01-01T14:01:15.000Z |4 |
| 2014-01-01T14:01:25.000Z |2014-01-01T14:01:15.000Z |4 |
| 2014-01-01T14:01:30.000Z |2014-01-01T14:01:30.000Z |5 |
| 2014-01-01T14:01:35.000Z |2014-01-01T14:01:35.000Z |6 |
| 2014-01-01T14:01:40.000Z |2014-01-01T14:01:35.000Z |6 |
| 2014-01-01T14:01:45.000Z |2014-01-01T14:01:35.000Z |6 |

**Sorgu :**

```SQL
SELECT
    System.Timestamp() AS Window_end,
    TopOne() OVER (ORDER BY Time DESC) AS Last_event
FROM
    Input TIMESTAMP BY Time
GROUP BY
    HOPPINGWINDOW(second, 300, 5)
```

Bu sorgu her 5 saniyede bir olaylar oluşturur ve daha önce alınan son olayı çıkar. **ATLAMA PENCERESI** süresi, sorgunun en son olayı bulmak için ne kadar geriye baktığını belirler.

Daha fazla bilgi için [Atlamalı pencereye](/stream-analytics-query/hopping-window-azure-stream-analytics)bakın.

## <a name="process-events-with-independent-time-substreams"></a>Olayları bağımsız zaman (Alt akışlar) ile işleme

Olay üreticileri arasındaki saat eğriliği, bölümler arasındaki saat eğriliği veya ağ gecikmesi nedeniyle olaylar geç veya bozuk gelebilir.
Örneğin, *TollID* 2'nin aygıt saati *TollID* 1'in beş saniye gerisinde, *TollID* 3'ün aygıt saati *ise TollID* 1'in on saniye gerisindedir. Bir hesaplama, yalnızca kendi saat verilerini bir zaman damgası olarak göz önünde bulundurarak, her geçiş ücreti için bağımsız olarak gerçekleşebilir.

**Giriş**:

| Ruhsat Plakası | Marka | Zaman | TollID |
| --- | --- | --- | --- |
| DXE 5291 |Make1 |2015-07-27T00:00:01.000000Z | 1 |
| YHN 6970 |Make2 |2015-07-27T00:00:05.000000Z | 1 |
| QYF 9358 |Make1 |2015-07-27T00:00:01.000000Z | 2 |
| GXF 9462 |Make3 |2015-07-27T00:00:04.000000Z | 2 |
| VFE 1616 |Make2 |2015-07-27T00:00:10.000000Z | 1 |
| RMV 8282 |Make1 |2015-07-27T00:00:03.000000Z | 3 |
| MDR 6128 |Make3 |2015-07-27T00:00:11.000000Z | 2 |
| YZK 5704 |Make4 |2015-07-27T00:00:07.000000Z | 3 |

**Çıktı**:

| TollID | Sayı |
| --- | --- |
| 1 | 2 |
| 2 | 2 |
| 1 | 1 |
| 3 | 1 |
| 2 | 1 |
| 3 | 1 |

**Sorgu :**

```SQL
SELECT
      TollId,
      COUNT(*) AS Count
FROM input
      TIMESTAMP BY Time OVER TollId
GROUP BY TUMBLINGWINDOW(second, 5), TollId
```

**TIMESTAMP OVER BY** yan tümcesi, alt akışları kullanarak her aygıt zaman çizelgesine bağımsız olarak bakar. Her *TollID* için çıkış olayı, tüm aygıtlar aynı saatteymiş gibi yeniden sıralanmış olmak yerine olayların her *TollID'ye* göre sıralandığı anlamına gelir, hesaplandıkça oluşturulur.

Daha fazla bilgi için [TIMESTAMP BY OVER'a](/stream-analytics-query/timestamp-by-azure-stream-analytics#over-clause-interacts-with-event-ordering)bakın.

## <a name="remove-duplicate-events-in-a-window"></a>Penceredeki yinelenen olayları kaldırma

Belirli bir zaman penceresindeki olaylar üzerinden ortalamahesaplama gibi bir işlem gerçekleştirirken, yinelenen olaylar filtrelenmelidir. Aşağıdaki örnekte, ikinci olay ilkin bir kopyasıdır.

**Giriş**:  

| DeviceId | Zaman | Öznitelik | Değer |
| --- | --- | --- | --- |
| 1 |2018-07-27T00:00:01.000000Z |Sıcaklık |50 |
| 1 |2018-07-27T00:00:01.000000Z |Sıcaklık |50 |
| 2 |2018-07-27T00:00:01.000000Z |Sıcaklık |40 |
| 1 |2018-07-27T00:00:05.000000Z |Sıcaklık |60 |
| 2 |2018-07-27T00:00:05.000000Z |Sıcaklık |50 |
| 1 |2018-07-27T00:00:10.000000Z |Sıcaklık |100 |

**Çıktı**:  

| Ortalama Değer | DeviceId |
| --- | --- |
| 70 | 1 |
|45 | 2 |

**Sorgu :**

```SQL
With Temp AS (
SELECT
    COUNT(DISTINCT Time) AS CountTime,
    Value,
    DeviceId
FROM
    Input TIMESTAMP BY Time
GROUP BY
    Value,
    DeviceId,
    SYSTEM.TIMESTAMP()
)

SELECT
    AVG(Value) AS AverageValue, DeviceId
INTO Output
FROM Temp
GROUP BY DeviceId,TumblingWindow(minute, 5)
```

**COUNT(DISTINCT Time)** zaman penceresinde Zaman sütunundaki farklı değerlerin sayısını döndürür. İlk adımın çıktısı, yinelenenleri atarak aygıt başına ortalamayı hesaplamak için kullanılabilir.

Daha fazla bilgi için [COUNT(DISTINCT Time)](/stream-analytics-query/count-azure-stream-analytics)bakın.

## <a name="session-windows"></a>Oturum Windows

Oturum Penceresi, olaylar oluştukça genişlemeye devam eden ve belirli bir süre den sonra olay alınmazsa veya pencere maksimum süresine ulaştığında hesaplama için kapanan bir penceredir.
Bu pencere, özellikle kullanıcı etkileşimi verilerini bilgisayara aktarırken kullanışlıdır. Bir pencere, kullanıcı sistemle etkileşime başladığında başlar ve başka olay gözlemlenmediğinde kapanır, yani kullanıcı etkileşimi durdurur.
Örneğin, bir kullanıcı tıklama sayısının günlüğe kaydedildiği bir web sayfasıyla etkileşimde bulunmaktadır, oturum penceresi kullanıcının siteyle ne kadar süreyle etkileşimde olduğunu öğrenmek için kullanılabilir.

**Giriş**:

| User_id | Zaman | URL'si |
| --- | --- | --- |
| 0 | 2017-01-26T00:00:00.000000Z | "www.example.com/a.html" |
| 0 | 2017-01-26T00:00:20.000000Z | "www.example.com/b.html" |
| 1 | 2017-01-26T00:00:55.000000Z | "www.example.com/c.html" |
| 0 | 2017-01-26T00:01:10.000000Z | "www.example.com/d.html" |
| 1 | 2017-01-26T00:01:15.000000Z | "www.example.com/e.html" |

**Çıktı**:

| User_id | StartTime | EndTime | Duration_in_seconds |
| --- | --- | --- | --- |
| 0 | 2017-01-26T00:00:00.000000Z | 2017-01-26T00:01:10.000000Z | 70 |
| 1 | 2017-01-26T00:00:55.000000Z | 2017-01-26T00:01:15.000000Z | 20 |

**Sorgu :**

``` SQL
SELECT
    user_id,
    MIN(time) as StartTime,
    MAX(time) as EndTime,
    DATEDIFF(second, MIN(time), MAX(time)) AS duration_in_seconds
FROM input TIMESTAMP BY time
GROUP BY
    user_id,
    SessionWindow(minute, 1, 60) OVER (PARTITION BY user_id)
```

**SELECT,** etkileşim süresiyle birlikte kullanıcı etkileşimiyle ilgili verileri de yarayar. En fazla 60 dakikalık pencere boyutuyla, 1 dakika içinde etkileşim olmazsa verileri ve kapatan **SessionWindow'a** göre verileri gruplandırma.

**SessionWindow**hakkında daha fazla bilgi için [Oturum Penceresi'ne](/stream-analytics-query/session-window-azure-stream-analytics) bakın.

## <a name="language-extensibility-with-user-defined-function-in-javascript-and-c"></a>JavaScript ve C'de Kullanıcı Tanımlı İşlev ile dil genişletilebilirliği #

Azure Akış Analizi sorgu dili, JavaScript veya C# dilinde yazılmış özel işlevlerle genişletilebilir. Kullanıcı Tanımlı İşlevler (UDF), **SQL** dili kullanılarak kolayca ifade edilemeyen özel/karmaşık hesaplamalardır. Bu UDF'ler bir kez tanımlanabilir ve sorgu içinde birden çok kez kullanılabilir. Örneğin, bir UDF hexadecimal *nvarchar(max)* değerini *büyük* bir değere dönüştürmek için kullanılabilir.

**Giriş**:

| Device_id | HexValue |
| --- | --- |
| 1 | "B4" |
| 2 | "11B" |
| 3 | "121" |

**Çıktı**:

| Device_id | Ondalık |
| --- | --- |
| 1 | 180 |
| 2 | 283 |
| 3 | 289 |

```JavaScript
function hex2Int(hexValue){
    return parseInt(hexValue, 16);
}
```

```C#
public static class MyUdfClass {
    public static long Hex2Int(string hexValue){
        return int.Parse(hexValue, System.Globalization.NumberStyles.HexNumber);
    }
}
```

```SQL
SELECT
    Device_id,
    udf.Hex2Int(HexValue) AS Decimal
From
    Input
```

Kullanıcı Tanımlı İşlev, tüketilen her olayda HexValue'ın *en büyük* değerini hesaplayacaktır.

Daha fazla bilgi için [JavaScript](/azure/stream-analytics/stream-analytics-javascript-user-defined-functions) ve [C#](/azure/stream-analytics/stream-analytics-edge-csharp-udf)bakın.

## <a name="advanced-pattern-matching-with-match_recognize"></a>MATCH_RECOGNIZE ile eşleşen gelişmiş desen

**MATCH_RECOGNIZE,** bir dizi olayı iyi tanımlanmış bir normal ifade deseniyle eşleştirmek için kullanılabilecek gelişmiş bir desen eşleştirme mekanizmasıdır.
Örneğin, yöneticinin bildirilmesi gereken art arda iki uyarı iletisi varsa, ATM'nin çalışması sırasında, hatalar için gerçek zamanlı olarak bir ATM izleniyor.

**Giriş**:

| ATM_id | Operation_id | Return_Code | Zaman |
| --- | --- | --- | --- |
| 1 | "Pin Girme" | "Başarı" | 2017-01-26T00:10:00.000000Z |
| 2 | "Açılış Para Yuvası" | "Başarı" | 2017-01-26T00:10:07.000000Z |
| 2 | "Kapanış Para Yuvası" | "Başarı" | 2017-01-26T00:10:11.000000Z |
| 1 | "Çekme Miktarını Girme" | "Başarı" | 2017-01-26T00:10:08.000000Z |
| 1 | "Açılış Para Yuvası" | "Uyarı" | 2017-01-26T00:10:14.000000Z |
| 1 | "Baskı Bankası Bakiyesi" | "Uyarı" | 2017-01-26T00:10:19.000000Z |

**Çıktı**:

| ATM_id | First_Warning_Operation_id | Warning_Time |
| --- | --- | --- |
| 1 | "Açılış Para Yuvası" | 2017-01-26T00:10:14.000000Z |

```SQL
SELECT *
FROM intput TIMESTAMP BY time OVER ATM_id
MATCH_RECOGNIZE (
    PARTITON BY ATM_id
    LIMIT DURATION(minute, 1)
    MEASURES
        First(Warning.ATM_id) AS ATM_id,
        First(Warning.Operation_Id) AS First_Warning_Operation_id,
        First(Warning.Time) AS Warning_Time
    AFTER MATCH SKIP TO NEXT ROW
    PATTERN (Success* Warning{2,})
    DEFINE
        Success AS Succes.Return_Code = 'Success',
        Failure AS Warning.Return_Code <> 'Success'
) AS patternMatch
```

Bu sorgu, en az iki ardışık hata olayıyla eşleşir ve koşullar karşılandığında bir alarm oluşturur.
**DESEN** eşleştirme de kullanılacak normal ifade tanımlar, bu durumda, başarılı işlemleri herhangi bir sayı en az iki ardışık hataları takip.
Başarı ve Başarısızlık Return_Code değer kullanılarak tanımlanır ve koşul yerine getirildiğinde, **ÖLÇÜLER** *ATM_id,* ilk uyarı işlemi ve ilk uyarı süresi ile yansıtılır.

Daha fazla bilgi için [MATCH_RECOGNIZE](/stream-analytics-query/match-recognize-stream-analytics)bakın.

## <a name="geofencing-and-geospatial-queries"></a>Geofencing ve jeouzamsal sorgular
Azure Akış Analizi, filo yönetimi, sürüş paylaşımı, bağlı arabalar ve varlık izleme gibi senaryoları uygulamak için kullanılabilecek yerleşik coğrafi işlevler sağlar.
Jeouzamsal veriler olay akışı veya referans verilerinin bir parçası olarak GeoJSON veya WKT biçimlerinde yutulabilir.
Örneğin, pasaport basım makineleri imalatında uzmanlaşmış bir şirket, makinelerini hükümetlere ve konsolosluklara kiralar. Bu makinelerin konumu, pasaportların yanlış yere yerleştirilmesi ve olası kullanımının önlenmesi için yoğun bir şekilde kontrol ediliyor. Her makineye bir GPS izleyicisi takılır ve bu bilgiler azure akışı analizi işine geri aktarılır.
Üretim bu makinelerin yerini takip etmek ve bunlardan biri yetkili bir alan ayrılırsa uyarı letmek istiyorum, bu şekilde uzaktan devre dışı bırakın, yetkilileri uyarmak ve ekipman almak.

**Giriş**:

| Equipment_id | Equipment_current_location | Zaman |
| --- | --- | --- |
| 1 | "NOKTA(-122.13288797982818 47,64082002051315)" | 2017-01-26T00:10:00.000000Z |
| 1 | "POINT(-122.13307252987875 47,64081350934929)" | 2017-01-26T00:11:00.000000Z |
| 1 | "POINT(-122.13308862313283 47,6406508603241)" | 2017-01-26T00:12:00.000000Z |
| 1 | "POINT(-122.13341048821462 47,64043760861279)" | 2017-01-26T00:13:00.000000Z |

**Referans Veri Girişi**:

| Equipment_id | Equipment_lease_location |
| --- | --- |
| 1 | "POLYGON((-122.13326028450979 47,6409833866794,-122.13261655444621 47,6409833866794,-122.132.132.1326555434662 1 47.64061471602751,-122.13326028450979 47,64061471602751,-122.13326028450979 47,6409833866794)" |

**Çıktı**:

| Equipment_id | Equipment_alert_location | Zaman |
| --- | --- | --- |
| 1 | "POINT(-122.13341048821462 47,64043760861279)" | 2017-01-26T00:13:00.000000Z |

```SQL
SELECT
    input.Equipment_id AS Equipment_id,
    input.Equipment_current_location AS Equipment_current_location,
    input.Time AS Time
FROM input TIMESTAMP BY time
JOIN
    referenceInput 
    ON input.Equipment_id = referenceInput.Equipment_id
    WHERE 
        ST_WITHIN(input.Equipment_currenct_location, referenceInput.Equipment_lease_location) = 1
```

Sorgu, üreticinin makinelerin konumunu otomatik olarak izlemesine ve bir makine izin verilen geofence'den ayrıldığında uyarılar almasına olanak tanır. Yerleşik coğrafi işlev, kullanıcıların üçüncü taraf kitaplıkları olmadan sorgu daki GPS verilerini kullanmasına olanak tanır.

Daha fazla bilgi için [Azure Akış Analizi makalesiyle Geofencing ve jeouzamsal toplama senaryolarına](geospatial-scenarios.md) bakın.

## <a name="get-help"></a>Yardım alın

Daha fazla yardım için [Azure Akışı Analizi forumumuzu](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics)deneyin.

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Akış Analizine Giriş](stream-analytics-introduction.md)
* [Azure Akış Analizi'ni kullanmaya başlama](stream-analytics-real-time-fraud-detection.md)
* [Azure Akış Analizi işlerini ölçeklendirme](stream-analytics-scale-jobs.md)
* [Azure Akış Analizi Sorgu Dili Başvurusu](https://docs.microsoft.com/stream-analytics-query/stream-analytics-query-language-reference)
* [Azure Akış Analizi Yönetimi REST API'si Başvurusu](https://msdn.microsoft.com/library/azure/dn835031.aspx)
