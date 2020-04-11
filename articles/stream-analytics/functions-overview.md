---
title: Azure Akış Analizi'nde kullanıcı tanımlı işlevler
description: Bu makale, Azure Akış Analizi'nde kullanıcı tarafından tanımlanan işlevlere genel bir bakıştır.
author: mamccrea
ms.author: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 04/07/2020
ms.openlocfilehash: b29d66e8bb213fbbb162c3249f022e0783f9f62f
ms.sourcegitcommit: fb23286d4769442631079c7ed5da1ed14afdd5fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/10/2020
ms.locfileid: "81115592"
---
# <a name="user-defined-functions-in-azure-stream-analytics"></a>Azure Akış Analizi'nde kullanıcı tanımlı işlevler

Azure Akış Analizi'ndeki SQL benzeri sorgu dili, veri akışında gerçek zamanlı analiz mantığının uygulanmasını kolaylaştırır. Akış Analizi, sorgunuzda çağrılan özel işlevler aracılığıyla ek esneklik sağlar. Aşağıdaki kod örneği, bir `sampleFunction` parametre kabul eden, işin aldığı her giriş kaydını kabul eden ve `sampleResult`sonuç çıktıya ' olarak yazılan udf' dur.

```sql
SELECT 
    UDF.sampleFunction(InputStream) AS sampleResult 
INTO 
    output 
FROM 
    InputStream 
```

## <a name="types-of-functions"></a>İşlev türleri

Azure Akış Analizi aşağıdaki dört işlev türünü destekler: 

* JavaScript kullanıcı tanımlı işlevleri 
* JavaScript kullanıcı tanımlı agregalar 
* C# kullanıcı tanımlı fonksiyonlar (Visual Studio kullanarak) 
* Azure Machine Learning 

Bu işlevleri, makine öğrenme modellerini kullanarak gerçek zamanlı puanlama, dize manipülasyonları, karmaşık matematiksel hesaplamalar, kodlama ve verileri çözme gibi senaryolar için kullanabilirsiniz. 

## <a name="limitations"></a>Sınırlamalar

Kullanıcı tanımlı işlevler durum suz ve döndürme değeri yalnızca skaler bir değer olabilir. İşinizin performansını etkileyeceği nden, bu kullanıcı tanımlı işlevlerden harici REST uç noktalarına çağrıda bulunduramazsınız. 

Azure Akış Analizi, tüm işlev çağrılarının ve döndürülen sonuçların kaydını tutmaz. Tekrarlanabilirliği garanti etmek için - örneğin, eski zaman damgasından işinizi yeniden çalıştırmak aynı `Date.GetData()` `Math.random()`sonuçları tekrar üretir - bu işlevler her çağırma için aynı sonucu döndürmediği için aynı sonuçları kullanmayın.  

## <a name="diagnostic-logs"></a>Tanılama günlükleri

Çalışma zamanı hataları ölümcül olarak kabul edilir ve etkinlik ve tanılama günlükleri ile su yüzüne çıkar. İşlevinizin tüm özel durumları ve hataları işlemesi ve geçerli bir sonucu sorgunuza döndürmesi önerilir. Bu, işinizin başarısız bir [duruma](job-states.md)girmesini önleyecektir.  


## <a name="next-steps"></a>Sonraki adımlar

* [Azure Akış Analizi'nde JavaScript kullanıcı tanımlı işlevler](stream-analytics-javascript-user-defined-functions.md)
* [Azure Akış Analizi JavaScript kullanıcı tanımlı toplamları](stream-analytics-javascript-user-defined-aggregates.md)
* [Azure Akış Analizi işleri için .NET Standart kullanıcı tanımlı işlevleri geliştirme](stream-analytics-edge-csharp-udf-methods.md)
* [Azure Akış Analitiğini Azure Makine Öğrenimi ile tümleştirin](machine-learning-udf.md)

