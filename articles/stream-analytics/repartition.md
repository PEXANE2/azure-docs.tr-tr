---
title: Azure Akış Analizi işlerini optimize etmek için yeniden bölümleme'yi kullanma
description: Bu makalede, paralelleştirilemeyen Azure Akış Analizi işlerini optimize etmek için yeniden bölümlemenin nasıl kullanılacağı açıklanmaktadır.
ms.service: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.date: 09/19/2019
ms.topic: conceptual
ms.custom: mvc
ms.openlocfilehash: c70cfb6c1626908a2ba4e707a890f6dc7481c51a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75732391"
---
# <a name="use-repartitioning-to-optimize-processing-with-azure-stream-analytics"></a>Azure Akış Analitiği ile işlemeen iyi duruma getirmek için yeniden bölümleme'yi kullanma

Bu makalede, tam olarak paralelleştirilmeyen senaryolar için Azure Akış Analizi sorgunuzu ölçeklendirmek için yeniden bölümleme nin nasıl kullanılacağı [gösterilmektedir.](stream-analytics-scale-jobs.md)

Şu anda [paralelleştirme](stream-analytics-parallelization.md) kullanamayabilirsiniz:

* Giriş akışınızın bölüm anahtarını denetlememezsiniz.
* Kaynak "spreyler" girişi daha sonra birleştirilmesi gereken birden çok bölüm arasında.

Olay Hub'ları için **PartitionId** gibi doğal bir giriş düzenine göre bölünmeyen bir akıştaki verileri işlediğizde yeniden bölümleme veya yeniden karıştırma gerekir. Yeniden bölümlediğinizde, her parça bağımsız olarak işlenebilir ve bu da akış ardınızı doğrusal olarak ölçeklendirmenize olanak tanır.

## <a name="how-to-repartition"></a>Yeniden bölünme nasıl

Yeniden bölümleme için, sorgunuzdaki **PARTITION BY** deyiminden sonra **INTO** anahtar sözcüklerini kullanın. Aşağıdaki örnek, **DeviceID'ye** göre verileri 10'luk bir bölüm sayısına bölümler. **DeviceID'nin** karması, hangi bölümün hangi alt akışı kabul edecektir belirlemek için kullanılır. Veri, her bölümlenmiş akış için bağımsız olarak temizlenir, çıktının bölümlenmiş yazmaları desteklediğini varsayarak ve 10 bölüme sahiptir.

```sql
SELECT * 
INTO output
FROM input
PARTITION BY DeviceID 
INTO 10
```

Aşağıdaki örnek sorgu, yeniden bölümlenen iki veri akışını birleştirir. Yeniden bölümlenen iki veri akışını birleştiriyorken, akışlar aynı bölüm anahtarına ve sayısına sahip olmalıdır. Sonuç, aynı bölüm düzenine sahip bir akıştır.

```sql
WITH step1 AS (SELECT * FROM input1 PARTITION BY DeviceID INTO 10),
step2 AS (SELECT * FROM input2 PARTITION BY DeviceID INTO 10)

SELECT * INTO output FROM step1 PARTITION BY DeviceID UNION step2 PARTITION BY DeviceID
```

Çıktı düzeni akış düzeni anahtarıyla eşleşmeli ve her alt akış bağımsız olarak temizlenebilsin diye saymalıdır. Akış, yıkamadan önce farklı bir şema ile birleştirilebilir ve yeniden bölümlenebilir, ancak işlemenin genel gecikmesine katkıda bulunduğundan ve kaynak kullanımını artırdığından bu yöntemden kaçınmalısınız.

## <a name="streaming-units-for-repartitions"></a>Yeniden bölümler için Akış Birimleri

İhtiyacınız olan bölümlerin tam sayısını belirlemek için işinizin kaynak kullanımını deneyin ve gözlemleyin. Akış birimi sayısı [(SU)](stream-analytics-streaming-unit-consumption.md) her bölüm için gereken fiziksel kaynaklara göre ayarlanmalıdır. Genel olarak, her bölüm için altı SUs gereklidir. İşe atanan yeterli kaynak yoksa, sistem yalnızca işe yararsa yeniden bölümü uygular.

## <a name="repartitions-for-sql-output"></a>SQL çıktısı için yeniden bölümler

İşiniz çıktı için SQL veritabanını kullandığında, iş verisini en üst düzeye çıkarmak için en iyi bölüm sayısını eşleştirmek için açık yeniden bölümleme kullanın. SQL en iyi sekiz yazarla çalıştığından, akışı floş yapmadan önce sekize veya daha yukarı bir yere yeniden bölmek iş performansından yararlanabilir. 

8'den fazla giriş bölümü olduğunda, giriş bölümleme düzenini devralmak uygun bir seçim olmayabilir. Çıktı yazarlarının sayısını açıkça belirtmek için sorgunuzda [INTO'yu](/stream-analytics-query/into-azure-stream-analytics#into-shard-count) kullanmayı düşünün. 

Aşağıdaki örnek, doğal olarak bölümlenmiş olmasına bakılmaksızın girişten okur ve deviceID boyutuna göre akışı on kat yeniden bölümlere ayırır ve verileri çıktıya boşaltır. 

```sql
SELECT * INTO [output] FROM [input] PARTITION BY DeviceID INTO 10
```

Daha fazla bilgi için Azure [Akış Analizi çıktısı için Azure SQL Veritabanı'na](stream-analytics-sql-output-perf.md)bakın.


## <a name="next-steps"></a>Sonraki adımlar

* [Azure Akış Analizi'ni başlatma](stream-analytics-introduction.md)
* [Azure Akış Analitiği'nde sorgu paralellemi'ni kinle](stream-analytics-parallelization.md)
