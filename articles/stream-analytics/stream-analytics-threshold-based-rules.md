---
title: Azure Akış Analizi'nde yapılandırılabilir eşik tabanlı kurallar
description: Bu makalede, Azure Akış Analizi'nde yapılandırılabilir eşik tabanlı kuralları olan bir uyarı çözümü elde etmek için başvuru verilerinin nasıl kullanılacağı açıklanmaktadır.
author: mamccrea
ms.author: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 04/30/2018
ms.openlocfilehash: 94fdddf11acb6763ed98a4b7e17304fbde0e25dd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75369720"
---
# <a name="process-configurable-threshold-based-rules-in-azure-stream-analytics"></a>Azure Akış Analizi'nde işlenebilir eşik tabanlı kuralları işleme
Bu makalede, Azure Akış Analizi'nde yapılandırılabilir eşik tabanlı kuralları kullanan bir uyarı çözümü elde etmek için başvuru verilerinin nasıl kullanılacağı açıklanmaktadır.

## <a name="scenario-alerting-based-on-adjustable-rule-thresholds"></a>Senaryo: Ayarlanabilir kural eşiklerine göre uyarı
Gelen akışlı olaylar belirli bir değere ulaştığında veya gelen akışlı olaylara dayalı toplu bir değer belirli bir eşiği aştığında çıktı olarak bir uyarı oluşturmanız gerekebilir. Değeri sabit ve önceden belirlenmiş statik bir eşikle karşılaştıran bir Akış Analizi sorgusu ayarlamak kolaydır. Sabit bir eşik, basit sayısal karşılaştırmalar (daha büyük, daha az ve eşitlik) kullanılarak akış sorgusu sözdizimine sabit kodlanabilir.

Bazı durumlarda, eşik değeri her değiştiğinde sorgu sözdizimini düzenlemeden eşik değerlerinin daha kolay yapılandırılabilir olması gerekir. Diğer durumlarda, her biri aygıt türünde farklı eşik değerlerine sahip olan aynı sorgu tarafından işlenen çok sayıda aygıt veya kullanıcıgerekebilir. 

Bu desen, eşikleri dinamik olarak yapılandırmak, giriş verilerini filtreleyerek eşiğin hangi tür bir aygıt la uygulanabileceğini seçerek seçmek ve çıktıya hangi alanların dahil edileceğini seçerek seçebilir.

## <a name="recommended-design-pattern"></a>Önerilen tasarım deseni
Bir Akış Analizi işine başvuru veri girişini uyarı eşiklerinin bir araması olarak kullanın:
- Eşik değerlerini başvuru verilerinde, anahtar başına bir değer olarak depolayın.
- Akış veri giriş olaylarını anahtar sütundaki başvuru verilerine katılın.
- Başvuru verilerinden gelen anahtarlı değeri eşik değeri olarak kullanın.

## <a name="example-data-and-query"></a>Örnek veri ve sorgu
Örnekte, bir dakika süren bir pencerede aygıtlardan gelen verilerin toplamı, referans verisi olarak sağlanan kuralda öngörülen değerlerle eşleştiğinde uyarılar oluşturulur.

Sorguda, her deviceId ve deviceId altındaki her metricName için 0'dan 5'e kadar boyutları GROUP BY'ye yapılandırabilirsiniz. Yalnızca karşılık gelen filtre değerlerine sahip olaylar gruplandırılır. Gruplandıktan sonra, Min, Max, Avg'nin pencereli toplamları 60 saniyelik bir yuvarlanma penceresi üzerinden hesaplanır. Daha sonra, uyarı çıktısı olayını oluşturmak için, toplanan değerlerüzerindeki filtreler başvuruda yapılandırılan eşiğe göre hesaplanır.

Örnek olarak, **adlandırılmış kurallar**adlandırılmış bir referans veri girişi ve akış veri girişi adı verilen **ölçümlere**sahip bir Akış Analizi işi olduğunu varsayalım. 

## <a name="reference-data"></a>Referans verileri
Bu örnek başvuru verileri, eşik tabanlı bir kuralın nasıl gösterilebildiğini gösterir. Bir JSON dosyası başvuru verilerini tutar ve Azure blob depolamasına kaydedilir ve bu blob depolama **kapsayıcısı, kural**adlı bir başvuru veri girişi olarak kullanılır. Bu JSON dosyasının üzerine yazabilir ve akış işini durdurmadan veya başlatmadan zaman geçtikçe kural yapılandırmasını değiştirebilirsiniz.

- Örnek kural, CPU değer `90` yüzdesini aştığında (ortalama daha büyük veya eşit) ayarlanabilir bir uyarıyı temsil etmek için kullanılır. Alan `value` gerektiği gibi yapılandırılabilir.
- Kuralın, daha sonra sorgu sözdiziminde dinamik olarak yorumlanan `AVGGREATEROREQUAL`bir **işleç** alanı olduğuna dikkat edin. 
- Kural, belirli bir boyut anahtarındaki `2` verileri `C1`değerle filtreler. Diğer alanlar, giriş akışını bu olay alanları tarafından filtrelemememi belirten boş dizedir. Gerektiğinde diğer eşleşen alanları filtrelemek için ek CPU kuralları ayarlayabilirsiniz.
- Tüm sütunlar çıktı uyarısı olayına dahil edilmez. Bu durumda, `includedDim` akıştaki olay verilerinin 2 numaralı alan numarasını temsil edecek şekilde anahtar numarası `2` açık `TRUE` olur. Diğer alanlar uyarı çıktısı dahil edilmez, ancak alan listesi ayarlanabilir.


```json
{
    "ruleId": 1234, 
    "deviceId" : "978648", 
    "metricName": "CPU", 
    "alertName": "hot node AVG CPU over 90",
    "operator" : "AVGGREATEROREQUAL",
    "value": 90, 
    "includeDim": {
        "0": "FALSE", 
        "1": "FALSE", 
        "2": "TRUE", 
        "3": "FALSE", 
        "4": "FALSE"
    },
    "filter": {
        "0": "", 
        "1": "",
        "2": "C1", 
        "3": "", 
        "4": ""
    }    
}
```

## <a name="example-streaming-query"></a>Örnek akış sorgusu
Bu örnek Akış Analizi sorgusu, yukarıdaki örnekten, **ölçümler**adlı bir veri girişi akışına, **kurallar** referans verilerini birleştirir.

```sql
WITH transformedInput AS
(
    SELECT
        dim0 = CASE rules.includeDim.[0] WHEN 'TRUE' THEN metrics.custom.dimensions.[0].value ELSE NULL END,
        dim1 = CASE rules.includeDim.[1] WHEN 'TRUE' THEN metrics.custom.dimensions.[1].value ELSE NULL END,
        dim2 = CASE rules.includeDim.[2] WHEN 'TRUE' THEN metrics.custom.dimensions.[2].value ELSE NULL END,
        dim3 = CASE rules.includeDim.[3] WHEN 'TRUE' THEN metrics.custom.dimensions.[3].value ELSE NULL END,
        dim4 = CASE rules.includeDim.[4] WHEN 'TRUE' THEN metrics.custom.dimensions.[4].value ELSE NULL END,
        metric = metrics.metric.value,
        metricName = metrics.metric.name,
        deviceId = rules.deviceId, 
        ruleId = rules.ruleId, 
        alertName = rules.alertName,
        ruleOperator = rules.operator, 
        ruleValue = rules.value
    FROM 
        metrics
        timestamp by eventTime
    JOIN 
        rules
        ON metrics.deviceId = rules.deviceId AND metrics.metric.name = rules.metricName
    WHERE
        (rules.filter.[0] = '' OR metrics.custom.filters.[0].value = rules.filter.[0]) AND 
        (rules.filter.[1] = '' OR metrics.custom.filters.[1].value = rules.filter.[1]) AND
        (rules.filter.[2] = '' OR metrics.custom.filters.[2].value = rules.filter.[2]) AND
        (rules.filter.[3] = '' OR metrics.custom.filters.[3].value = rules.filter.[3]) AND
        (rules.filter.[4] = '' OR metrics.custom.filters.[4].value = rules.filter.[4])
)

SELECT
    System.Timestamp as time, 
    transformedInput.deviceId as deviceId,
    transformedInput.ruleId as ruleId,
    transformedInput.metricName as metric,
    transformedInput.alertName as alert,
    AVG(metric) as avg,
    MIN(metric) as min, 
    MAX(metric) as max, 
    dim0, dim1, dim2, dim3, dim4
FROM
    transformedInput
GROUP BY
    transformedInput.deviceId,
    transformedInput.ruleId,
    transformedInput.metricName,
    transformedInput.alertName,
    dim0, dim1, dim2, dim3, dim4,
    ruleOperator, 
    ruleValue, 
    TumblingWindow(second, 60)
HAVING
    (
        (ruleOperator = 'AVGGREATEROREQUAL' AND avg(metric) >= ruleValue) OR
        (ruleOperator = 'AVGEQUALORLESS' AND avg(metric) <= ruleValue) 
    )
```

## <a name="example-streaming-input-event-data"></a>Örnek akış giriş olay verileri
Bu örnek, JSON verileri, yukarıdaki akış sorgusunda kullanılan **ölçümler** giriş verilerini temsil eder. 

- Üç örnek olaylar 1 dakikalık zaman açıklığı `T14:50`içinde listelenir, değer. 
- Üçünün de `deviceId` değeri `978648`aynı.
- CPU metrik değerleri her olay, `95` `80` `98`, , sırasıyla değişir. Yalnızca ilk iki örnek olay, kuralda belirlenen CPU uyarı kuralını aşar.
- Uyarı kuralındaki includeDim alanı 2 numaralı anahtardı. Örnek olaylardaki karşılık gelen anahtar `NodeName`2 alanı nın adı . Üç örnek olayın `N024`değerleri `N024`, `N014` , ve sırasıyla var. Çıktıda, yalnızca düğümü `N024` yüksek CPU uyarı ölçütleri ile eşleşen tek veri olarak görürsünüz. `N014`yüksek CPU eşiğini karşılamaz.
- Uyarı kuralı, örnek olaylardaki `filter` `cluster` alana karşılık gelen yalnızca 2 numaralı anahtarla yapılandırılır. Üç örnek olayın tümü `C1` değere sahiptir ve filtre ölçütleri ile eşleşir.

```json
{
    "eventTime": "2018-04-30T14:50:23.1324132Z",
    "deviceId": "978648",
    "custom": {
        "dimensions": {
            "0": {
                "name": "NodeType",
                "value": "N1"
            },
            "1": {
                "name": "Cluster",
                "value": "C1"
            },
            "2": {
                "name": "NodeName",
                "value": "N024"
            }
        },
        "filters": {
            "0": {
                "name": "application",
                "value": "A1"
            },
            "1": {
                "name": "deviceType",
                "value": "T1"
            },
            "2": {
                "name": "cluster",
                "value": "C1"
            },
            "3": {
                "name": "nodeType",
                "value": "N1"
            }
        }
    },
    "metric": {
        "name": "CPU",
        "value": 98,
        "count": 1.0,
        "min": 98,
        "max": 98,
        "stdDev": 0.0
    }
}
{
    "eventTime": "2018-04-30T14:50:24.1324138Z",
    "deviceId": "978648",
    "custom": {
        "dimensions": {
            "0": {
                "name": "NodeType",
                "value": "N2"
            },
            "1": {
                "name": "Cluster",
                "value": "C1"
            },
            "2": {
                "name": "NodeName",
                "value": "N024"
            }
        },
        "filters": {
            "0": {
                "name": "application",
                "value": "A1"
            },
            "1": {
                "name": "deviceType",
                "value": "T1"
            },
            "2": {
                "name": "cluster",
                "value": "C1"
            },
            "3": {
                "name": "nodeType",
                "value": "N2"
            }
        }
    },
    "metric": {
        "name": "CPU",
        "value": 95,
        "count": 1,
        "min": 95,
        "max": 95,
        "stdDev": 0
    }
}
{
    "eventTime": "2018-04-30T14:50:37.1324130Z",
    "deviceId": "978648",
    "custom": {
        "dimensions": {
            "0": {
                "name": "NodeType",
                "value": "N3"
            },
            "1": {
                "name": "Cluster",
                "value": "C1 "
            },
            "2": {
                "name": "NodeName",
                "value": "N014"
            }
        },
        "filters": {
            "0": {
                "name": "application",
                "value": "A1"
            },
            "1": {
                "name": "deviceType",
                "value": "T1"
            },
            "2": {
                "name": "cluster",
                "value": "C1"
            },
            "3": {
                "name": "nodeType",
                "value": "N3"
            }
        }
    },
    "metric": {
        "name": "CPU",
        "value": 80,
        "count": 1,
        "min": 80,
        "max": 80,
        "stdDev": 0
    }
}
```

## <a name="example-output"></a>Örnek çıktı
Bu örnek çıktı JSON verileri, başvuru verilerinde tanımlanan CPU eşik kuralına dayalı olarak tek bir uyarı olayının üretildiğini gösterir. Çıktı olayı, uyarının adının yanı sıra düşünülen alanların toplamının (ortalama, min, max) yanı sıra içerir. Çıktı olay verileri kural yapılandırması nedeniyle alan anahtar sayısı 2 `NodeName` değeri `N024` içerir. (JSON okunabilirlik için satır sonları göstermek için değiştirildi.)

```JSON
{"time":"2018-05-01T02:03:00.0000000Z","deviceid":"978648","ruleid":1234,"metric":"CPU",
"alert":"hot node AVG CPU over 90","avg":96.5,"min":95.0,"max":98.0,
"dim0":null,"dim1":null,"dim2":"N024","dim3":null,"dim4":null}
```
