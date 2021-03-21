---
title: Azure Stream Analytics işleri iyileştirmek için yeniden bölümleme kullanma
description: Bu makalede, paralelleştirilmedi Azure Stream Analytics işleri iyileştirmek için yeniden bölümlemeden nasıl kullanılacağı açıklanır.
ms.service: stream-analytics
author: sidramadoss
ms.author: sidram
ms.date: 03/04/2021
ms.topic: conceptual
ms.custom: mvc
ms.openlocfilehash: 95749f2acea6b605cfdba5a4f3d4f5526e751c5a
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102182545"
---
# <a name="use-repartitioning-to-optimize-processing-with-azure-stream-analytics"></a>Azure Stream Analytics ile işlemeyi iyileştirmek için yeniden bölümleme kullanın

Bu makalede, tam olarak [Paralelleştirilmesi](stream-analytics-scale-jobs.md)gereken senaryolara yönelik Azure Stream Analytics sorgunuzu ölçeklendirmek için yeniden bölümlemeden nasıl kullanılacağı gösterilmektedir.

Şunu yaparsanız [paralelleştirme](stream-analytics-parallelization.md) kullanmeyebilirsiniz:

* Giriş akışınız için bölüm anahtarını denetlememeniz gerekir.
* Kaynak "sprtikleriniz", daha sonra birleştirilmek zorunda olan birden çok bölüm genelinde giriş.

Event Hubs için **PartitionID** gibi bir doğal giriş şemasına göre parçalı olmayan bir akışta verileri işleçalıştığınızda yeniden bölümlendirip veya reshuffling gereklidir. Yeniden bölümlerseniz, her parça bağımsız olarak işlenebilir ve bu da akış işlem hattınızı daha erken ölçeklendirmenize olanak tanır. 

## <a name="how-to-repartition"></a>Yeniden bölümleme
Girişinizi 2 şekilde yeniden bölümlendirebilirsiniz:
1. Yeniden bölümleme işlemini yapan ayrı bir Stream Analytics iş kullanın
2. Tek bir iş kullanın ancak özel analiz mantığınızı önce yeniden bölümlendirip yapın

### <a name="creating-a-separate-stream-analytics-job-to-repartition-input"></a>Girişi yeniden bölümlemek için ayrı bir Stream Analytics işi oluşturma
Bir bölüm anahtarı kullanarak, bir olay hub 'ına giriş ve yazma okuyan bir iş oluşturabilirsiniz. Bu olay hub 'ı daha sonra analiz mantığınızı uyguladığınız başka bir Stream Analytics iş için giriş olarak görev yapabilir. Bu olay hub 'ı İşinizdeki Bu çıktıyı yapılandırırken, Stream Analytics verilerinizi yeniden bölümleyeceğinize olan bölüm anahtarını belirtmeniz gerekir. 
```sql
-- For compat level 1.2 or higher
SELECT * 
INTO output
FROM input

--For compat level 1.1 or lower
SELECT *
INTO output
FROM input PARTITION BY PartitionId
```

### <a name="repartition-input-within-a-single-stream-analytics-job"></a>Girişi tek bir Stream Analytics işi içinde yeniden bölümleme
Sorgumanızda, önce girişi yeniden bölümleyerek bir adım da oluşturabilirsiniz ve bu, daha sonra Sorgunuzdaki diğer adımlar tarafından kullanılabilir. Örneğin, bir girişi **DeviceID**'ye göre yeniden bölümlemek istiyorsanız sorgunuz şöyle olacaktır:
```sql
WITH RepartitionedInput AS 
( 
SELECT * 
FROM input PARTITION BY DeviceID
)

SELECT DeviceID, AVG(Reading) as AvgNormalReading  
INTO output
FROM RepartitionedInput  
GROUP BY DeviceId, TumblingWindow(minute, 1)  
```

Aşağıdaki örnek sorgu, yeniden bölümlenmiş verilerin iki akışını birleştirir. Yeniden bölümlenmiş verilerin iki akışı birleştirilirken akışlar aynı bölüm anahtarına ve sayıya sahip olmalıdır. Sonuç, aynı bölüm düzenine sahip bir akıştır.

```sql
WITH step1 AS (SELECT * FROM input1 PARTITION BY DeviceID),
step2 AS (SELECT * FROM input2 PARTITION BY DeviceID)

SELECT * INTO output FROM step1 PARTITION BY DeviceID UNION step2 PARTITION BY DeviceID
```

Çıkış şeması, her alt akışın bağımsız olarak temizleneceği şekilde akış şeması anahtarıyla ve sayısıyla eşleşmelidir. Akış, temizlenmeden önce farklı bir düzen tarafından birleştirilebilir ve yeniden bölümlenebilir, ancak işlemin genel gecikme süresine eklediğinden ve kaynak kullanımını arttığı için bu yöntemden kaçının.

## <a name="streaming-units-for-repartitions"></a>Yeniden bölümler için akış birimleri

İhtiyacınız olan bölümlerin tam sayısını öğrenmek için işinizin kaynak kullanımını deneyin ve gözlemleyin. [Akış birimlerinin sayısı (su)](stream-analytics-streaming-unit-consumption.md) , her bölüm için gereken fiziksel kaynaklara göre ayarlanmalıdır. Genel olarak, her bölüm için altı SUs gerekir. İşe atanan yeterli kaynak yoksa, sistem yalnızca işi yararlılığı durumunda yeniden bölümlendirmesi uygular.

## <a name="repartitions-for-sql-output"></a>SQL çıkışı için yeniden bölümler

İşiniz çıktı için SQL veritabanı kullandığında, üretilen iş miktarını en üst düzeye çıkarmak için en iyi bölüm sayısını eşleştirmek üzere açık yeniden bölümleme kullanın. SQL sekiz yazıcı ile en iyi şekilde çalıştığından, temizlemeye başlamadan önce akışı sekiz olarak yeniden bölümlendirip veya daha fazla yukarı akış, iş performansına yarar sağlayabilir. 

8 ' den fazla giriş bölümü olduğunda, giriş bölümleme düzenini devralma uygun bir seçenek olmayabilir. Çıktı yazıcılarının sayısını açıkça belirtmek için [sorgunuzda ' i kullanmayı göz](/stream-analytics-query/into-azure-stream-analytics#into-shard-count) önünde bulundurun. 

Aşağıdaki örnek, doğal olarak bölümlenmeden bağımsız olarak girdiden ve veri kümesini DeviceID boyutuna göre yeniden bölümleyip akışa alarak çıktıyı temizler. 

```sql
SELECT * INTO [output] FROM [input] PARTITION BY DeviceID INTO 10
```

Daha fazla bilgi için bkz. [Azure SQL veritabanı 'na Azure Stream Analytics çıktısı](stream-analytics-sql-output-perf.md).


## <a name="next-steps"></a>Sonraki adımlar

* [Azure Stream Analytics kullanmaya başlayın](stream-analytics-introduction.md)
* [Azure Stream Analytics sorgu paralelleştirme özelliğinden yararlanın](stream-analytics-parallelization.md)
