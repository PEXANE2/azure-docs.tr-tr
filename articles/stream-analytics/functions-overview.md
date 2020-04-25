---
title: Azure Stream Analytics 'de Kullanıcı tanımlı işlevler
description: Bu makale, Azure Stream Analytics içindeki kullanıcı tanımlı işlevlere genel bir bakış.
author: mamccrea
ms.author: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 04/07/2020
ms.openlocfilehash: 45e766c624ee96f7faa06fb07d00349e620a4c0a
ms.sourcegitcommit: edccc241bc40b8b08f009baf29a5580bf53e220c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/24/2020
ms.locfileid: "82133477"
---
# <a name="user-defined-functions-in-azure-stream-analytics"></a>Azure Stream Analytics 'de Kullanıcı tanımlı işlevler

Azure Stream Analytics SQL benzeri sorgu dili, akış verilerinde gerçek zamanlı analiz mantığını uygulamayı kolaylaştırır. Stream Analytics, sorgunuzda çağrılan özel işlevlerle daha fazla esneklik sağlar. Aşağıdaki kod örneği, bir parametre kabul eden `sampleFunction` adlı UDF 'dir, işin aldığı her giriş kaydı ve sonuç çıkışa yazılır `sampleResult`.

```sql
SELECT 
    UDF.sampleFunction(InputStream) AS sampleResult 
INTO 
    output 
FROM 
    InputStream 
```

## <a name="types-of-functions"></a>İşlev türleri

Azure Stream Analytics aşağıdaki dört işlev türünü destekler: 

* JavaScript kullanıcı tanımlı işlevleri 
* JavaScript Kullanıcı tanımlı toplamalar 
* C# Kullanıcı tanımlı işlevler (Visual Studio kullanarak) 
* Azure Machine Learning 

Makine öğrenimi modelleri, dize düzenlemeleri, karmaşık matematik hesaplamaları, kodlama ve kod çözme verileri ile gerçek zamanlı Puanlama gibi senaryolar için bu işlevleri kullanabilirsiniz. 

## <a name="limitations"></a>Sınırlamalar

Kullanıcı tanımlı işlevler durumsuz ve dönüş değeri yalnızca skaler bir değer olabilir. Bu Kullanıcı tanımlı işlevlerden dış REST uç noktalarına çağrı yapılamaz, çünkü işinizin performansı büyük olasılıkla etkiler. 

Azure Stream Analytics, tüm işlev etkinleştirmeleri ve döndürülen sonuçlar için bir kayıt yapmaz. Örneğin, yinelenebilirlik sağlamak için, işinizi eski zaman damgasından yeniden çalıştırmak aynı sonuçları yeniden üretir. bu işlevler her bir çağrı için aynı sonucu döndürmediğinden `Date.GetData()` , `Math.random()`veya gibi işlevleri kullanmayın.  

## <a name="resource-logs"></a>Kaynak günlükleri

Tüm çalışma zamanı hataları önemli olarak değerlendirilir ve etkinlik ve kaynak günlükleri aracılığıyla ortaya çıkmış demektir. İşlevinizin tüm özel durumları ve hataları işlemesi ve sorgunuza geçerli bir sonuç döndürmesi önerilir. Bu, işinizin [başarısız durumuna](job-states.md)gitmesini engeller.  


## <a name="next-steps"></a>Sonraki adımlar

* [Azure Stream Analytics 'de Kullanıcı tanımlı JavaScript işlevleri](stream-analytics-javascript-user-defined-functions.md)
* [JavaScript Kullanıcı tanımlı toplamaları Azure Stream Analytics](stream-analytics-javascript-user-defined-aggregates.md)
* [Azure Stream Analytics işleri için .NET Standard Kullanıcı tanımlı işlevler geliştirme](stream-analytics-edge-csharp-udf-methods.md)
* [Azure Stream Analytics Azure Machine Learning ile tümleştirin](machine-learning-udf.md)

